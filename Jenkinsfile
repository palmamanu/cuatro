pipeline {
  agent any
  stages {
    stage('Build') {
      parallel {
        stage('Build') {
          steps {
            checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'palmamanu', url: 'https://github.com/palmamanu/uno/']]])
            echo 'Building...'
            sh 'npm install'
            task 'tarea1'
            pwd()
          }
        }
        stage('prueba2') {
          steps {
            echo 'hola'
            sh '/usr/bin/ansible-playbook /tmp/apache.yml'
          }
        }
        stage('tomcat') {
          agent any
          steps {
            sh '/usr/bin/ansible-playbook /tmp/tomcat.yml'
            echo 'Instalo tomcat en maquinas remotas'
          }
        }
      }
    }
    stage('Test') {
      parallel {
        stage('Test') {
          steps {
            echo 'Testing...'
          }
        }
        stage('copia estaticos') {
          steps {
            pwd(tmp: true)
            sh 'scp /tmp/lala* root@10.28.107.98:/var/www'
          }
        }
      }
    }
    stage('Testeadoya') {
      steps {
        echo 'Testing...'
      }
    }
    stage('checkout') {
      steps {
        checkout scm
      }
    }
    stage('prepare') {
      steps {
        sh 'git clean -fdx'
      }
    }
    stage('compile') {
      steps {
        echo 'nothing to compile for hello.sh...'
        sh 'mvn clean compile war:war'
      }
    }
    stage('testcompile') {
      steps {
        sh '/tmp/test_hello.sh'
        sh 'scp ./target/*.war root@10.28.107.98:/usr/share/tomcat/webapps'
      }
    }
    stage('package') {
      steps {
        sh 'tar -cvzf hello.tar.gz hello.sh'
      }
    }
    stage('publish') {
      steps {
        echo 'uploading package...'
      }
    }
  }
}