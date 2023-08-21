pipeline {
    agent any

    

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Vue App') {
            steps {
                script {
                    sh 'cd backend' // Change to your Vue.js app directory
                    sh 'npm install'
                    sh 'npm run build'
                }
            }
        }

        stage('Build Laravel API') {
            steps {
                script {
                    sh 'composer install'
                    sh 'cp .env.example .env'
                    sh 'php artisan key:generate'
                    sh 'php artisan config:cache'
                }
                script {
            // Set your database credentials here
            env.DB_HOST = 'localhost'
            env.DB_PORT = '3606'
            env.DB_DATABASE = 'laravel'
            env.DB_USERNAME = 'laraveluser'
            env.DB_PASSWORD = '123'
        }
            }
        }

        //stage('Deploy to Nginx') {
           // steps {
           //     script {
                    // Copy built Vue.js files to Nginx web root
                  //  sh 'cp -r vue-app-directory/dist/* /var/www/html' // Replace with your Nginx web root

                    // Configure Nginx for the Vue.js app (adjust server block as needed)
                   // sh 'cp nginx-config/vue-app.conf /etc/nginx/sites-available' // Replace with your Nginx config path
                  //  sh 'ln -s /etc/nginx/sites-available/vue-app.conf /etc/nginx/sites-enabled'
                  //  sh 'service nginx restart'
             //   }
          //  }
       // }
    }
}
