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
                    sh 'npm install'
                    sh '''
                cd backend
                ls
                npm install
                ls
                cp .env.example .env
                npm run build
                ls
            '''
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

        stage('Deploy to Nginx') {
           steps {
               script {
                   sh 'sudo chmod 755 /var/www/html/vue-laravel/public'
                  sh 'sudo chown -R www-data:www-data /var/www/html/vue-laravel/public'
                 sh 'sudo chown www-data:www-data /var/www/html/vue-laravel/storage'   
                  sh 'sudo chown www-data:www-data /var/www/html/vue-laravel/bootstrap/cache'   
                  sh 'sudo cp -r backend/dist/ /var/www/html/vue-laravel/backend/dist/' // Replace with your Nginx web root
                   sh 'sudo chmod -R 755 /var/www/html/vue-laravel/backend/dist'
sh 'sudo chown -R www-data:www-data /var/www/html/vue-laravel/backend/dist'

sh 'sudo cp -r * /var/www/html/vue-laravel/'
                    // Configure Nginx for the Vue.js app (adjust server block as needed)
                   sh 'sudo cp app.conf /etc/nginx/sites-available/' // Replace with your Nginx config path
                   sh 'sudo rm /etc/nginx/sites-enabled/app.conf'
                  sh 'sudo ln -s /etc/nginx/sites-available/app.conf /etc/nginx/sites-enabled'
                   sh 'sudo nginx -t'
                  sh 'sudo systemctl restart nginx'
             }
          }
       }
    }
}
