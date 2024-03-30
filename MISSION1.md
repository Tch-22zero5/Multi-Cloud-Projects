# Mission 1: Provisioning resources on AWS AND GCP with TERRAFORM 
You'll learn how to enable a MultiCloud architecture deployment through Terraform, with resources running in AWS and Google ﻿Cloud Platform.
# What you need :
An AWS account

https://aws.amazon.com/free/

A Google Cloud Account GCP (For GCP) Open your browser in an **Anonymous | Private | Cognito way** and go to:

[**https://console.cloud.google.com/freetrial**](https://console.cloud.google.com/freetrial)

**Advice:** We strongly recommend creating a new Gmail account, instead of using your personal!
# Steps in Amazon Web Services (AWS)

## Creating the terraform-en-1 user using the IAM service

- Access the AWS console ([https://aws.amazon.com](https://aws.amazon.com/)) **and log in with your newly created account**. In the search bar, type IAM. In the Services section, click on IAM.
- Click on Users and then Add users, enter the name **terraform-en-1**, and click Next to create a programmatic type user.
![Untitled](https://github.com/Tch-22zero5/Multi-Cloud-Projects/assets/140101993/b66b2fa9-c408-4c71-a0bd-d259df27c4a5)

- After advancing, in Set permissions, click on the Attach existing policies directly button.
 <img width="1000" alt="Screenshot_2023-02-03_at_09 27 44" src="https://github.com/Tch-22zero5/Multi-Cloud-Projects/assets/140101993/e4b7086c-ab65-451b-8a5b-cd8345d5665b">

- Type **AmazonS3FullAccess** in **Search.**
- Select **AmazonS3FullAccess**
![Untitled (1)](https://github.com/Tch-22zero5/Multi-Cloud-Projects/assets/140101993/3910e4db-18db-4389-a234-78139aabd8cc)

- Click on **Next**
- Review all the details
- Click on **Create user**
## Creating the Access Key for the terraform-en-1 user using the IAM service

- Access the **terraform-en-1** user
![Untitled (2)](https://github.com/Tch-22zero5/Multi-Cloud-Projects/assets/140101993/fa30b45b-ae4f-458b-825e-cbe90e83ed2f)
- Click on the Security credentials tab
![Untitled (3)](https://github.com/Tch-22zero5/Multi-Cloud-Projects/assets/140101993/20561d8f-f7d8-4af9-bbb2-cfcf03d32c8c)
- Navigate to the **Access keys** section
- Click on **Create access key**
![Untitled (4)](https://github.com/Tch-22zero5/Multi-Cloud-Projects/assets/140101993/ab001686-bdfd-406b-8b26-0e7e04e2f487)
- Select Command Line Interface (CLI) and I understand the above recommendation and want to proceed to create an access key.
![Untitled (5)](https://github.com/Tch-22zero5/Multi-Cloud-Projects/assets/140101993/2986e771-74b6-4637-aca1-21fa41cd53fd)
- Click on **Next**.
- Click on **Create access key**
![Untitled (6)](https://github.com/Tch-22zero5/Multi-Cloud-Projects/assets/140101993/62587550-d47f-46a3-8717-02b191586b06)
Click on Download .csv file
![Untitled (7)](https://github.com/Tch-22zero5/Multi-Cloud-Projects/assets/140101993/190e1a69-a665-448b-9bc5-1ffd7d6e5630)
- After the download finishes, click on Done.
- Once the download is complete, rename the **.csv** file to **key.csv**
# Steps in Google Cloud Platform (GCP)

## Preparing the environment to run Terraform

- Access the Google Cloud Console ([console.cloud.google.com](http://console.cloud.google.com/)) **and log in with your newly created account**
- Open the Cloud Shell
![Untitled (8)](https://github.com/Tch-22zero5/Multi-Cloud-Projects/assets/140101993/883a599e-bb46-4088-b0a4-47d1307ad2ce)
- Download the mission1.zip file in the Google Cloud shell using the wget command

  wget https://tcb-public-events.s3.amazonaws.com/icp/mission1.zip

**Result**
  ![Untitled (9)](https://github.com/Tch-22zero5/Multi-Cloud-Projects/assets/140101993/eecf8246-4a4b-4f79-8604-2a7cee380b70)
- Upload the key.csv  and mission1.zip files to the Cloud Shell using the browser
- Step 1
![Untitled (10)](https://github.com/Tch-22zero5/Multi-Cloud-Projects/assets/140101993/728dde52-4498-4366-9a19-3ce3c350fff2)
- Step 2
![Untitled (11)](https://github.com/Tch-22zero5/Multi-Cloud-Projects/assets/140101993/ddf9c98d-e1e4-444b-9432-342ba07833db)
- Step 3
![Untitled (12)](https://github.com/Tch-22zero5/Multi-Cloud-Projects/assets/140101993/9d66b844-04c5-4839-a1c1-984b512c64d7)
- Verify if the mission1.zip and key.csv files are in the folder in the Cloud Shell using the command below

  ls

**Result**
![Untitled (13)](https://github.com/Tch-22zero5/Multi-Cloud-Projects/assets/140101993/eff2c4f8-2116-44c9-ad96-f3474dc85c5d)
- Execute the file preparation commands:

mkdir mission1_en

mv mission1.zip mission1_en

cd mission1_en

unzip mission1.zip

mv key.csv mission1/en

cd mission1/en

chmod +x *.sh

**Result**
![Untitled(14)](https://github.com/Tch-22zero5/Multi-Cloud-Projects/assets/140101993/a5f657ae-1351-44c1-a761-9342420dde87)
- Execute the commands below to prepare the AWS and GCP environment
    
    mkdir -p ~/.aws/
    
    touch ~/.aws/credentials_multiclouddeploy
    
    ./aws_set_credentials.sh key.csv
    
    gcloud config set project <GOOGLE_CLOUD_PROJECT_ID>

 **NOTE**: To get Project ID and **EDIT** the  <GOOGLE_CLOUD_PROJECT_ID>
- Drag down the Cloud shell
- Click on the *MY FIRST PROJECT* dropdown 
  ![Screenshot (24)](https://github.com/Tch-22zero5/Multi-Cloud-Projects/assets/140101993/6fdc4d48-e28a-4c44-a0e8-6d02765ff017)
- **COPY** your project ID and past on the terminal of the Cloud shell
![Screenshot (25)](https://github.com/Tch-22zero5/Multi-Cloud-Projects/assets/140101993/a1e22611-3d35-4f1e-84a5-b35cf73541de)
   
- Click on Authorize
![Untitled(14)](https://github.com/Tch-22zero5/Multi-Cloud-Projects/assets/140101993/9b750fa8-82dc-4697-83b4-a88b2d57b294)
- Execute the command below to set the project in the Google Cloud Shell

  ./gcp_set_project.sh

- Execute the commands to enable the Kubernetes, Container Registry, and Cloud SQL APIs

  gcloud services enable containerregistry.googleapis.com


  gcloud services enable container.googleapis.com
​

  gcloud services enable sqladmin.googleapis.com
​
## IMPORTANT( DO NOT SKIP)
![Screenshot (26)](https://github.com/Tch-22zero5/Multi-Cloud-Projects/assets/140101993/b7276dc8-c4a4-4548-b366-796fd2a39af2)
**To open the text editor**
- Go to the right-hand-side of your Google Cloud shell terminal
- Click on the Text editor
![Screenshot (27)](https://github.com/Tch-22zero5/Multi-Cloud-Projects/assets/140101993/984ced43-7dba-43f8-90ba-635fcae84df7)

- On the Text editor, Click on the mission1_en folder
![Screenshot (28)](https://github.com/Tch-22zero5/Multi-Cloud-Projects/assets/140101993/6a9f6379-c7c8-4f11-b136-1ef50ace66f1)

- Click on **Mission1** folder
- Click on **en** folder
![Screenshot (29)](https://github.com/Tch-22zero5/Multi-Cloud-Projects/assets/140101993/8c9e69fd-e3e5-4d27-bad6-2ae105b934b3)

- Click on **Terraform** folder
- locate the file named **tcb_aws_storage.tf**
![Screenshot (31)](https://github.com/Tch-22zero5/Multi-Cloud-Projects/assets/140101993/18630d62-d834-4197-8db3-6d6f228a3eea)

- Click on **tcb_aws_storage.tf** and edit the **LINE 4 XXXX** with unique number of your choice(This is because AWS requires you to create a unique name for your Aws S3 Buckets)
![Screenshot (32)](https://github.com/Tch-22zero5/Multi-Cloud-Projects/assets/140101993/733326ea-8230-4ab1-a61d-f20811e07c66)
**AFTER EDITING RETURN TO THE CLOUD SHELL TERMINAL**
## Running Terraform to provision MultiCloud infrastructure in AWS and Google Cloud

- Execute the following commands to provision infrastructure resources

cd ~/mission1/en/terraform/

terraform init

terraform plan

terraform apply

**Attention:** The provisioning process can take between 15 to 25 minutes to finish. Keep the CloudShell open during the process. If disconnected, click on Reconnect when the session expires (the session expires after 5 minutes of inactivity by default)
## IMPORTANT (DO NOT SKIP)
![Screenshot (36)](https://github.com/Tch-22zero5/Multi-Cloud-Projects/assets/140101993/f3e79bbf-f463-44f0-b669-59550e697940)

# Appendix I - Destroying the environment and starting over

In case you have encountered any problem/error and want to reset the environment to start over, follow the step-by-step instructions below to remove the entire MultiCloud environment.

## [Google Cloud] Delete VPC Peering
![Untitled (14)](https://github.com/Tch-22zero5/Multi-Cloud-Projects/assets/140101993/e7ca02f7-66f1-410c-966f-08683861bda4)
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

### Google Cloud

cd ~


rm -rf mission*


rm -rf .ssh
# Security Tips

- For production environments, it's recommended to use only the Private Network for database access.
- Never provide public network access (0.0.0.0/0) to production databases. ⚠️

**By reaching this point, you have completed the implementation of the first part of the Hands-on Project and have implemented resources in a MultiCloud (AWS and Google Cloud) environment using Terraform!**

**Congratulations! 🚀🎉**
