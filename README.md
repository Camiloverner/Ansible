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
