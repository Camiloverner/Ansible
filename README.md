![](https://i0.wp.com/blog.knoldus.com/wp-content/uploads/2017/10/ansible_logo.png?fit=1800%2C514&ssl=1)
# Ansible Studies Repository<h1>
Repositório de estudos da ferramenta Ansible. Neste repositório vou deixar scripts prontos para poder estudar a sintaxe da ferramenta e sua própria linguagem declarativa para descrever a configuração do sistema.

### Exemplo de playbook do Ansible <h3>
Abaixo está um exemplo simples de um playbook do Ansible em formato YAML para criar um usuário em máquinas CentOS, Ubuntu e Red Hat. O script esta no arquivo criar_usuario2.yml.

~~~YML
---
- name: Criar usuário
  hosts: centos, ubuntu, redhat
  become: yes  # Para executar com privilégios de superusuário (sudo)

  tasks:
    - name: Criar usuário
      user:
        name: novo_usuario
        comment: "Nome Completo do Usuário"
        password: senha_criptografada  # Use 'mkpasswd --method=sha-512' para gerar a senha criptografada
        state: present

    - name: Adicionar usuário aos grupos
      user:
        name: novo_usuario
        groups: wheel  # Adicione outros grupos se necessário
        append: yes

    - name: Configurar chave SSH para o usuário
      authorized_key:
        user: novo_usuario
        key: "{{ lookup('file', '~/.ssh/id_rsa.pub') }}"  # Substitua pelo caminho da chave pública do usuário

    # Adicione mais tarefas conforme necessário
~~~~

Beleza. Agora vamos entender oque esse Playbook faz! 

* Cria um usuário com o nome **novo_usuario**.
* Adiciona o usuário ao grupo **wheel**.
* Configura a chave **SSH** para o usuário. Certifique-se de ter sua chave pública no arquivo **~/.ssh/id_rsa.pub**.

### Exemplo de um arquivo de inventário hosts.<h3>
Este arquivo define os hosts nos quais o playbook será executado.
~~~YML
[centos]
ip_do_centos            #Substitua ip_do_centos, ip_do_ubuntu e ip_do_redhat pelos endereços IP reais ou nomes de host das suas máquinas. 

[ubuntu]
ip_do_ubuntu

[redhat]
ip_do_redhat

~~~
### Playbook para atualizar pacotes em máquinas CentOS, Ubuntu e Red Hat  <h3>
Este arquivo atualiza os pacotes nas maquinas mencionadas acima.
~~~YML
---
- name: Atualizar pacotes
  hosts: centos, ubuntu, redhat
  become: yes  # Para executar com privilégios de superusuário (sudo)

  tasks:
    - name: Atualizar pacotes no CentOS
      yum:
        name: '*'
        state: latest
      when: "'centos' in inventory_hostname"

    - name: Atualizar pacotes no Ubuntu
      apt:
        upgrade: yes
      when: "'ubuntu' in inventory_hostname"

    - name: Atualizar pacotes no Red Hat
      yum:
        name: '*'
        state: latest
      when: "'redhat' in inventory_hostname"
~~~
Este playbook faz o seguinte:
* Atualiza todos os pacotes no CentOS usando o **módulo yum**.
* Atualiza todos os pacotes no Ubuntu usando o **módulo apt**.
*  Atualiza todos os pacotes no Red Hat usando o **módulo yum**.

💾 Lembre-se esse é apenas um pequeno exemplo de como é sintaxe e o funcionamento do Playbook! Apenas para fins de estudo, não aplicar em Produção!
