pipeline{
   agent any
   tools {
       nodejs 'NodeJS-18'
    }

   stages {
       stage('Clone Repository') {
           steps {
               git 'https://github.com/BrianBravoski/gallery.git'
           }
       }
       stage('Install dependencies') {
           steps {
               
               sh 'npm install'
           }
       }
   }