# Final-Project-Assessment-for-Scalefocus-Academy

Requirement for the Project Assessment

Deploy a WordPress on Kubernetes (using Minicube) with Helm and automation with Jenkins.

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
             
      
Step 4. Deploying the helm chart using the jenkins pipeline
![image](https://github.com/DamjanGj77/Final-Project-Assessment-for-Scalefocus-Academy/assets/125911118/46dabaf5-6664-4102-bf86-1bcaddb63547)

![image](https://github.com/DamjanGj77/Final-Project-Assessment-for-Scalefocus-Academy/assets/125911118/91db75d1-1057-4a22-99c9-d703d9d252fd)

![image](https://github.com/DamjanGj77/Final-Project-Assessment-for-Scalefocus-Academy/assets/125911118/d1757eec-19fd-406e-8f1b-0fbebb28a1d3)

Step 5. For loading the home page we need to execute the following command **kubectl port-forward --namespace wp svc/final-project-wp-scalefocus-wordpress 80:80**
![image](https://github.com/DamjanGj77/Final-Project-Assessment-for-Scalefocus-Academy/assets/125911118/d5e5fc03-7a52-4c56-9f20-925e6b9e1bc2)

![image](https://github.com/DamjanGj77/Final-Project-Assessment-for-Scalefocus-Academy/assets/125911118/62bae536-de2c-45fd-93ed-a7b65a6b863c)

-wp-login
![image](https://github.com/DamjanGj77/Final-Project-Assessment-for-Scalefocus-Academy/assets/125911118/665df7e5-6f56-4b63-86f0-a45bf492dee6)

-wp admin
![image](https://github.com/DamjanGj77/Final-Project-Assessment-for-Scalefocus-Academy/assets/125911118/700c760b-39f4-4740-b53c-8c4a52f1ce4b)



HOPE YOU WILL LIKE MY PROJECT :))))

            
