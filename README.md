# web-project
simple html project, this will act as the root folder for nginx

This project will be used as the root folder for nginx. when ever a commit happen it will trigger the CICD pipe line and the corresponding images will be created and pushed to ECR. 

Once the image is pushed to ECR, it will be deployed in the ec2 instance using jenkins
