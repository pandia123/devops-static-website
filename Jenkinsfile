pipeline {

    agent any

    environment {

        IMAGE_NAME = "devops-static-website"
        CONTAINER_NAME = "website"

        GITHUB_REPO = "https://github.com/pandia123/devops-static-website.git"

        WEBSITE_URL = "http://13.233.93.127:8081"

        EMAIL = "gudlu.pand001@gmail.com"

        AWS_REGION = "ap-south-1"
        AWS_ACCOUNT_ID = "314146340349"

        ECR_REPOSITORY = "devops-static-website"

        ECR_URI = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${ECR_REPOSITORY}"
    }

    stages {

        stage('Checkout Source Code') {

            steps {

                echo "Checking out source code..."

                git branch: 'main',
                    url: "${GITHUB_REPO}"
            }
        }

        stage('Build Docker Image') {

            steps {

                echo "Building Docker Image..."

                sh '''
                    docker build -t ${IMAGE_NAME}:${BUILD_NUMBER} .
                    docker tag ${IMAGE_NAME}:${BUILD_NUMBER} ${IMAGE_NAME}:latest
                '''
            }
        }

        stage('Login to Amazon ECR') {

            steps {

                echo "Logging into Amazon ECR..."

                sh '''
                    aws --version

                    aws ecr get-login-password --region ${AWS_REGION} | \
                    docker login \
                    --username AWS \
                    --password-stdin \
                    ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com
                '''
            }
        }

        stage('Create ECR Repository (If Not Exists)') {

            steps {

                sh '''
                    aws ecr describe-repositories \
                    --repository-names ${ECR_REPOSITORY} \
                    --region ${AWS_REGION} >/dev/null 2>&1 || \

                    aws ecr create-repository \
                    --repository-name ${ECR_REPOSITORY} \
                    --region ${AWS_REGION}
                '''
            }
        }

        stage('Tag Docker Image') {

            steps {

                sh '''

                    docker tag ${IMAGE_NAME}:${BUILD_NUMBER} ${ECR_URI}:${BUILD_NUMBER}

                    docker tag ${IMAGE_NAME}:latest ${ECR_URI}:latest

                '''
            }
        }

        stage('Push Image to Amazon ECR') {

            steps {

                echo "Pushing Image to ECR..."

                sh '''

                    docker push ${ECR_URI}:${BUILD_NUMBER}

                    docker push ${ECR_URI}:latest

                '''
            }
        }

        stage('Pull Latest Image From ECR') {

            steps {

                echo "Pulling Image From ECR..."

                sh '''

                    docker pull ${ECR_URI}:latest

                '''
            }
        }

        stage('Stop Old Container') {

            steps {

                echo "Stopping Existing Container..."

                sh '''

                    docker stop ${CONTAINER_NAME} || true

                    docker rm ${CONTAINER_NAME} || true

                '''
            }
        }

        stage('Deploy Docker Container') {

            steps {

                echo "Deploying Container..."

                sh '''

                    docker run -d \
                    --name ${CONTAINER_NAME} \
                    -p 8081:80 \
                    --restart unless-stopped \
                    ${ECR_URI}:latest

                '''
            }
        }

        stage('Verify Deployment') {

            steps {

                echo "Verifying Deployment..."

                sh '''

                    sleep 15

                    STATUS=$(curl -o /dev/null -s -w "%{http_code}" http://localhost:8081)

                    if [ "$STATUS" != "200" ]; then
                        echo "Website Deployment Failed"
                        exit 1
                    fi

                    echo "Website is Running Successfully."

                '''
            }
        }

        stage('Cleanup Docker Images') {

            steps {

                echo "Cleaning Docker Images..."

                sh '''

                    docker image prune -af

                '''
            }
        }
    }

    post {

        always {

            echo "Pipeline Finished."
        }

        success {

            emailext(

                subject: "SUCCESS : ${JOB_NAME} #${BUILD_NUMBER}",

                mimeType: 'text/html',

                to: "${EMAIL}",

                body: """

<html>

<body>

<h2 style="color:green;">Docker Deployment Successful</h2>

<hr>

<b>Job Name :</b> ${JOB_NAME}<br><br>

<b>Build Number :</b> ${BUILD_NUMBER}<br><br>

<b>Status :</b> SUCCESS<br><br>

<b>Docker Image :</b><br>

${ECR_URI}:${BUILD_NUMBER}

<br><br>

<b>Website :</b><br>

<a href="${WEBSITE_URL}">
${WEBSITE_URL}
</a>

<br><br>

<b>Build Logs :</b><br>

<a href="${BUILD_URL}">
${BUILD_URL}
</a>

</body>

</html>

"""
            )
        }

        failure {

            emailext(

                subject: "FAILED : ${JOB_NAME} #${BUILD_NUMBER}",

                mimeType: 'text/html',

                to: "${EMAIL}",

                body: """

<html>

<body>

<h2 style="color:red;">Pipeline Failed</h2>

<hr>

<b>Job Name :</b> ${JOB_NAME}<br><br>

<b>Build Number :</b> ${BUILD_NUMBER}<br><br>

<b>Status :</b> FAILED<br><br>

<b>Check Console Output:</b><br>

<a href="${BUILD_URL}">
${BUILD_URL}
</a>

</body>

</html>

"""
            )
        }
    }
}
