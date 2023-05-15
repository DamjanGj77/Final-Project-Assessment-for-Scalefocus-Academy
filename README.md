# Final-Project-Assessment-for-Scalefocus-Academy

Requirement for the Project Assessment

Step 1. Helm chart was downloaded and cloned on path Users\Damjan.Gjorgievski\Documents\final_project_scalefocus\charts\bitnami\wordpress
Step 2. The values.yaml line 543 was changed from LoadBalancer to ClusterIP

![image](https://github.com/DamjanGj77/Final-Project-Assessment-for-Scalefocus-Academy/assets/125911118/ad429154-3283-4916-bfcc-6633f9d30889)

Step 3. Jenkins pipeline that checks if wp namespace exists, if it doesn’t then it creates one. Checks if WordPress exists, if it doesn’t then it installs the chart
             
             pipeline {
               agent any
               environment {
                 //Set the KUBECONFIG environment variable
                 KUBECONFIG = '/Users/Damjan.Gjorgievski/.kube/config' 
               }
               stages {
                 stage('Verify') {
                   steps {
                     //this commands verify that the authentication and configuration are working correctly
                     bat 'kubectl config get-contexts'
                   }
                 }
                 stage('Deploy : final-project-wp-scalefocus.') {
                   steps {
                     script {
                       try {
                         // Check if the namespace exists
                         def namespaceExists = bat(script: 'kubectl get namespace wp', returnStatus: true) == 0
                         if (!namespaceExists) {
                           // Create the namespace
                           bat 'kubectl create namespace wp'
                         }
                         // Deploy the application using Helm
                         bat 'helm dependency build ./bitnami/wordpress'
                         bat 'helm install final-project-wp-scalefocus C:/Users/Damjan.Gjorgievski/Documents/final_project_scalefocus/charts/bitnami/wordpress -n wp -f C:/Users/Damjan.Gjorgievski/Documents/final_project_scalefocus/charts/bitnami/wordpress/values.yaml'
                       } catch (Exception e) {
                         // Handle any deployment errors
                         error "Deployment failed: ${e.getMessage()}"
                       }
                     }
                   }
                 }
               }
             }
            
