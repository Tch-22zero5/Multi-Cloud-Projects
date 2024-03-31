# Mission3: Files and Database Migration from On-Premises Application to the Multi-Cloud Environment
To do this, you have to extract data from a source DB, create a database dump file of the on-premises Database which is the source DB, import this DB dump into our Google Cloud SQL which is the destination Database (GCP), and extract the existing PDF files (covid 19 testing results) from the Application server, Zip the pdf files, copy and move the zip files to our Amazon S3 bucket (AWS).

**NOTE** The extraction and creation of both the PDF files and the DB Dump file have already been done, so our focus on the last part of this project is moving the database migration and file migration into the multi-cloud environment.
# Steps in Google Cloud Platform (Database Migration)
- Connect to Google Cloud Shell
- Download the dump using wget

  cd


  mkdir mission3_en


  cd mission3_en
​

  wget https://tcb-public-events.s3.amazonaws.com/icp/mission3.zip
​

  unzip mission3.zip
​
- Connect to MySQL DB running on Cloud SQL (once it prompts for the password, provide welcome123456). Don’t forget to replace the placeholder with your Cloud SQL Public IP

  mysql --host=<replace_with_public_ip_cloudsql> --port=3306 -u app -p
- If it prompts a password use: welcome123456
- Import the dump on Cloud SQL

  use dbcovidtesting;

  source ~/mission3/en/db/db_dump.sql
- Check if the data got imported correctly
  
  select * from records;

  exit;
# Steps in Amazon Web Services (PDF Files Migration)
- Connect to the **AWS Cloud Shell**
- Download the PDF files

  cd

  mkdir mission_en


  cd mission3_en


  wget https://tcb-public-events.s3.amazonaws.com/icp/mission3.zip


  unzip mission3.zip
- Sync PDF Files with your AWS S3 used for COVID-19 Testing Status System. Replace the bucket xxxx name with yours.
  
  cd mission3/en/pdf_files

  aws s3 sync . s3://**luxxy-covid-testing-system-pdf-en-xxxx**
- Test the application. Upon migrating the data and files, you should be able to see the entries  under the “View Guest Results” page.
![Screen_Shot_2022-02-07_at_4 41 37_PM](https://github.com/Tch-22zero5/Multi-Cloud-Projects/assets/140101993/8ea172f5-713e-4b58-9e1d-31f7d303bc12)

- Click on **Link** in any of the Customers' Data to view each of their PDF files (COVID testing results)
![Screenshot (40)](https://github.com/Tch-22zero5/Multi-Cloud-Projects/assets/140101993/e5543cc3-b2b1-47dc-983d-637be11b49fb)

Congratulations! You have migrated an "on-premises" application & database to a MultiCloud Architecture!
# SOLUTION ARCHITECTURE [ACHIEVED MULTI-CLOUD ENVIRONMENT]
![Screenshot (37)](https://github.com/Tch-22zero5/Multi-Cloud-Projects/assets/140101993/0e6fe32c-312a-49c4-a5ee-e930318a0b3e)


# Appendix I - Destroying the environment permanently

After completing the hands-on project and gathering the implementation evidence, follow the step-by-step instructions below to remove the entire MultiCloud environment.

## [Google Cloud] Delete Kubernetes resources

**Step 1**
![Untitled (25)](https://github.com/Tch-22zero5/Multi-Cloud-Projects/assets/140101993/5cc571c8-3931-416b-84bf-44105f5fa725)
**Step 2**
![Untitled (24)](https://github.com/Tch-22zero5/Multi-Cloud-Projects/assets/140101993/a84e7764-8ecc-40b9-8578-90aba87dc0ee)

  kubectl delete deployment luxxy-covid-testing-system


  kubectl delete service luxxy-covid-testing-system
# [Google Cloud] Delete VPC Peering
![Untitled (26)](https://github.com/Tch-22zero5/Multi-Cloud-Projects/assets/140101993/270e7f37-312a-47e3-8f35-29e85930d2a3)
# AWS] Delete files inside of S3
![Untitled (27)](https://github.com/Tch-22zero5/Multi-Cloud-Projects/assets/140101993/73cc8b63-43dc-4a06-a2e2-a9b353126b18)
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
