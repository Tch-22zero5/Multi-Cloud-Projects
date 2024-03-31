# Mission2: Migrating the on Premises existing Application to AWS and GCP Cloud 
This is for the Kubernetes cluster to communicate with both the files of the App which will be moved to Amazon S3 and the App deployed to the Google cloud and not to expose the files publicly. 

# Steps in Amazon Web Services (AWS)

- Access AWS console and go to IAM service
- Under Access management, Click in "Users", then "Add users". Insert the User name **luxxy-covid-testing-system-en-app1** and click in **Next** to create a programmatic user.
<img width="1000" alt="Screenshot_2023-02-03_at_09 19 48" src="https://github.com/Tch-22zero5/Multi-Cloud-Projects/assets/140101993/10701298-839e-4f68-b66b-813d0ae0fa7c">

- On Set permissions, Permissions options, click in "Attach policies directly" button.
<img width="1000" alt="Screenshot_2023-02-03_at_09 27 44 (1)" src="https://github.com/Tch-22zero5/Multi-Cloud-Projects/assets/140101993/5425ac55-504d-40ed-9198-5f70d1bcebb1">

- Type **AmazonS3FullAccess** in **Search**.
- Select **AmazonS3FullAccess**
<img width="1000" alt="Screenshot_2023-02-03_at_09 31 36" src="https://github.com/Tch-22zero5/Multi-Cloud-Projects/assets/140101993/192b0595-cd00-422d-b76c-cc636365ee14">

- Click in **Next**
- Review all details and click in Create user
<img width="1000" alt="Screenshot_2023-02-03_at_09 32 38" src="https://github.com/Tch-22zero5/Multi-Cloud-Projects/assets/140101993/c1dd9f3e-f8f4-4755-8af1-245eaad1fddb">

### **Steps to create access key:**

- Click on the user you have created.
- Go to Security credentials tab.
- Scroll down and go to Access keys section.
- Click on Create access key

<img width="1000" alt="Screenshot_2023-02-03_at_09 48 38" src="https://github.com/Tch-22zero5/Multi-Cloud-Projects/assets/140101993/d263cb1d-b428-42fe-852e-f3b38b224ca0">

- Select **Command Line Interface (CLI)** and **I understand the above recommendation and want to proceed to create an access key** checkbox.
- Click Next
- Click on Create access key
- Click on Download .csv file
- After download, click Done.
- Now, rename .csv file downloaded to **luxxy-covid-testing-system-en-app1.csv**

# Steps in Google Cloud Platform (GCP)

- Navigate to Cloud SQL instance and create a new user **app** with password **welcome123456** on Cloud SQL MySQL database ( For the App to connect the database it needs to have a user)


https://github.com/Tch-22zero5/Multi-Cloud-Projects/assets/140101993/5c76c78d-d93e-4b2a-8af1-70092ff19192

- Connect to Google Cloud Shell
- **Download** the mission2 files to Google Cloud Shell using the wget command as shown below
    
  
    cd
  
    mkdir mission2_en
  
    cd mission2_en
  
    **wget https://tcb-public-events.s3.amazonaws.com/icp/mission2.zip**
  
    unzip mission2.zip
    
- Connect to MySQL DB running on Cloud SQL (once it prompts for the password, provide **welcome123456**). **Don’t forget to replace the placeholder with your Cloud SQL Public IP,  You can go to overview and copy the public IP of the instance**
![Untitled (15)](https://github.com/Tch-22zero5/Multi-Cloud-Projects/assets/140101993/4f9613dc-24a9-4dc6-9e39-c9f2552b7d3d)
mysql --host=<replace_with_public_ip_cloudsql> --port=3306 -u app -p
- If it prompts for Password use:  welcome123456 
- Once you're connected to the database instance, create the products table for testing purposes

  use dbcovidtesting;

  source ~/mission2/en/db/create_table.sql

  show tables;
  
  desc records;
  
  exit;
- Enable Cloud Build API via Cloud Shell.

  gcloud services enable cloudbuild.googleapis.com

![Untitled (16)](https://github.com/Tch-22zero5/Multi-Cloud-Projects/assets/140101993/ba7cfec7-bc6a-4052-9039-347a2ac5da40)
- Build the Docker image and push it to Google Container Registry.
- Replace <GOOGLE_CLOUD_PROJECT_ID> with your First Project ID and copy these Commands

  cd ~/mission2/en/app

  gcloud builds submit --tag gcr.io/<GOOGLE_CLOUD_PROJECT_ID>/luxxy-covid-testing-system-app-en

- You can search the Google container registry to view the Docker image

- Open the Cloud Editor
- Go to mission2_en folder
- Go to mission2 folder
- Go to the **en** folder
- Go to the Kubernetes folder and edit the Kubernetes deployment file (luxxy-covid-testing-system.yaml) and update the variables below
- on line 33 in red with your <PROJECT_ID>
- on the Google Container Registry path, on line 42 AWS Bucket name, AWS Keys (open file luxxy-covid-testing-system-en-app1.csv and use Access key ID on line 44 and Secret access key on line 46)
- and Cloud SQL Database Private IP on line 48.

  cd ~/mission2/en/kubernetes

  luxxy-covid-testing-system.yaml

  image: gcr.io/<PROJECT_ID>/luxxy-covid-testing-system-app-en:latest

  - name: AWS_BUCKET

    value: "luxxy-covid-testing-system-pdf-en-xxxx"

  - name: S3_ACCESS_KEY

    value: "xxxxxxxxxxxxxxxxxxxxx"

  - name: S3_SECRET_ACCESS_KEY

    value: "xxxxxxxxxxxxxxxxxxxx"

  - name: DB_HOST_NAME

    value: "172.21.0.3"

- Note: The S3 access key and secret access key are on the .csv file we downloaded and renamed
![Screenshot (38)](https://github.com/Tch-22zero5/Multi-Cloud-Projects/assets/140101993/f950ad88-ce34-4930-a931-95653c407914)

- NOTE: The Value of the DB hostname is the Private IP address of the DB instance which you can find and copy from the overview of the DB in the Google Cloud
![Screenshot (39)](https://github.com/Tch-22zero5/Multi-Cloud-Projects/assets/140101993/7e028c90-ac37-4750-9781-98e8b1a8cda6)
- NOTE: Save the File after editing.
- Connect to the GKE (Google Kubernetes Engine) cluster via Console
- Search for GKE and go to the Kubernetes engine
- Go inside of your Kubernetes cluster
- Above the icons, find and click on **Connect**
- It will display the command you need to run to connect your Kubernetes cluster
- Click on **run in Cloudshell**
- In the Cloudshell terminal, press Enter
    
    **Step 1**
![Untitled (17)](https://github.com/Tch-22zero5/Multi-Cloud-Projects/assets/140101993/c5fe52ff-0de6-45b1-980e-459f8ed25589)
- Deploy the application Luxxy in the Cluster
cd ~/mission2/en/kubernetes
​
kubectl apply -f luxxy-covid-testing-system.yaml
​
- Under GKE > Workloads > Exposing Services, get the application Public IP
**Step 1**
![Untitled (18)](https://github.com/Tch-22zero5/Multi-Cloud-Projects/assets/140101993/4e620e9b-b8b5-4a23-833c-1b4835138e11)
**Step 2**
![Untitled (19)](https://github.com/Tch-22zero5/Multi-Cloud-Projects/assets/140101993/2c8668e8-9126-4cb4-8817-e1e1f36fe73e)
- You should see the app up & running!
![Screen_Shot_2022-02-07_at_11 45 31_AM](https://github.com/Tch-22zero5/Multi-Cloud-Projects/assets/140101993/a2aeb694-0792-4556-a358-d6b593f8acc5)
- (Optional) Download a sample COVID testing and add an entry in the application.
    
    **Click on the icon below to download the PDF ⬇️**

    [covid-testing.pdf](https://github.com/Tch-22zero5/Multi-Cloud-Projects/files/14813798/covid-testing.pdf)

Congratulations for finishing the hands-on project part 2! 🚀

# Appendix I - Destroying the environment and starting over

In case you have encountered any problem/error and want to reset the environment to start over, follow the step-by-step instructions below to remove the entire MultiCloud environment.

## [Google Cloud] Delete Kubernetes resources

**Step 1**
![Untitled (20)](https://github.com/Tch-22zero5/Multi-Cloud-Projects/assets/140101993/694e33c5-1f7d-4c6e-8a88-b0cb0c9e9081)
**Step 2**
![Untitled (21)](https://github.com/Tch-22zero5/Multi-Cloud-Projects/assets/140101993/12e59351-903c-4021-a348-5ce14c922393)

kubectl delete deployment luxxy-covid-testing-system

kubectl delete service luxxy-covid-testing-system
# [Google Cloud] Delete VPC Peering
![Untitled (22)](https://github.com/Tch-22zero5/Multi-Cloud-Projects/assets/140101993/64882caf-1810-4814-b1a0-32ea7283c94e)
# [AWS] Delete files inside of S3
![Untitled (23)](https://github.com/Tch-22zero5/Multi-Cloud-Projects/assets/140101993/4eb9768d-092d-450b-9526-c472d10d3a1e)

# [Google Cloud] Delete remaining resources w/ Terraform - Cloud Shell
cd ~/mission1/en/terraform/
​

terraform destroy
​
# Clean the Cloud Shell in AWS and Google Cloud 
# AWS
cd ~
​

rm -rf mission*

# Google Cloud
cd ~
​

rm -rf mission*
​

rm -rf .ssh
