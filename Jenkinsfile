pipeline {
    agent any

    environment {
        TF_VAR_access_key = credentials('AWS_ACCESS_KEY_ID')
        TF_VAR_secret_key = credentials('AWS_SECRET_ACCESS_KEY')
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Terraform Apply') {
            steps {
                script {
                    dir('terraform') {
                        sh 'terraform init'
                        sh 'terraform plan'
                        sh 'terraform apply -auto-approve'
                    }
                }
            }
        }

        stage('Ansible Deploy') {
            steps {
                script {
                    dir('ansible') {
                        sh 'ansible-playbook -i inventory.ini playbook.yml'
                    }
                }
            }
        }
    }

    // post {
    //     always {
    //         stage('Terraform Destroy') {
    //             steps {
    //                 script {
    //                     dir('terraform') {
    //                         sh 'terraform destroy -auto-approve'
    //                     }
    //                 }
    //             }
    //         }
    //     }
    // }
}
