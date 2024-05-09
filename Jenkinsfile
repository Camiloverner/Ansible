pipeline {
    agent any

    environment {
        def myString  = "Hello World"
        def myNumber = 7
        def myBool = true
    }

    stages{
        stage("Demo"){
            steps{
                echo "my string: ${myString}"
                echo "my number: ${myNumber}"
                echo "my bool: ${myBool}"
            }
        }
    }
}
