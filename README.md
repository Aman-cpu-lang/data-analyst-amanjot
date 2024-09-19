# data-analyst-amanjot

 	The rapid development of cloud computing technologies revolutionized how organizations managed and processed data for analysis. Cities' adoption of digital transformation to improve their operations and services calls for the implementation of robust data analytics platforms. This project is concerned with designing and developing a comprehensive data analytics platform, DAP, on Amazon Web Services, one of the leading cloud service providers in the City of Vancouver.
 	Like any modern metropolis, the City of Vancouver creates massive volumes of data from its many departments and services. If used properly, data can lead to better decisions, better allocation of resources, and improvements in citizen services. However, data volume, variety, and speed pose great challenges regarding storage, processing, and analysis.
 	The design and implementation of DAP from a cloud perspective have been discussed in the report, addressing the needs and requirements of the City of Vancouver. This will ensure that the data analytics ecosystem on the platform is scalable, secure, and efficient by leveraging the breadth of AWS's services. The City of Vancouver aims to transcend traditional infrastructure limitations by migrating to AWS for increased flexibility, cost efficiency, and advanced analytics achieved through cloud computing.
 	The approach taken with its implementation is structured, with thirteen major steps for the data analytics journey, from formulating a question to publication of the data. Each step utilizes specific AWS services that can handle different parts of the data analytics life cycle. This wide-reaching approach ensures that the platform can input, store, process, analyze, and visualize data from different sources of the city's operations.
 	With the adoption of this cloud-based DAP, the City of Vancouver will unlock the insights of their data for better evidence-based policymaking, enhanced urban planning, and improved public services, feeding into more effective and responsive city government. The subsequent sections discuss the design and implementation of each component of the DAP, showing how AWS services are used to build a robust, integrated analytics environment for the City of Vancouver.
Step 1: Data Analytic Question Formulation
I have used the data of Animals control and inventory management. For the animals who are reported on roads are sometimes redeemed by their owners, sometimes sold, sometimes they die of medical issues. In this my goal is to find the percent redeemed animals.
![image](https://github.com/user-attachments/assets/8fe601ab-9401-4739-a698-f862b0bd7241)

Different analytical questions and metrics are:
Descriptive: Total number of redeemed animals from custody of Vancouver Animal Control this year?
Diagnostic: What factors contribute to different status of animals?
Predictive: How many animals are expected to be redeemed next year?
Prescriptive: How can we allocate resources to manage the increase in animals that come into custody?
![image](https://github.com/user-attachments/assets/3b83bee2-db40-4496-a0a8-9a56c318d211)

When we get data we need to store it somewhere. So AWS provide service S3 to store data. I created my bucket named “animals”, domain “records” in S3.
![image](https://github.com/user-attachments/assets/096889ad-c727-4ce5-b54d-e00719585ca4)

In this domain folders for 2023 and 2024 are created. In 2024 further landing, raw and curated folders are created.
After creating folders upload the data for “redeemed” and “total” animals in respective folders in excel format in the landing folder where we upload operational dataset.
![image](https://github.com/user-attachments/assets/1195470f-eb5f-4e02-ad1d-a7b36d84ecb6)

![image](https://github.com/user-attachments/assets/fd83e3bd-d48f-4e9c-8377-6dcf4faffec8)

For data cleaning I used AWS Glue data brew, where I created project first. After creating project, data is uploaded from landing folders. Now all the missing values are either relaced with null, or rows are deleted. Also year is extracted from the date.
Data cleaning Redeemed:
![image](https://github.com/user-attachments/assets/22a7cd86-b773-40c9-b696-4c84d9cc714b)

Data cleaning Total
![image](https://github.com/user-attachments/assets/23e0875d-ae1b-4608-a631-c54f55bf07fb)

I only replaced missing values for data structuring. After cleaning job is created and run, now the leaned data is stored in raw folder.
Reedemed
![image](https://github.com/user-attachments/assets/03ee6db9-d50b-4456-9275-9c03f7524623)

Total
![image](https://github.com/user-attachments/assets/20453702-3b6b-4aaa-8df0-05c290018d4b)

After this I created ETL pipeline on AWS Glue
![image](https://github.com/user-attachments/assets/315f91db-8fff-49dc-b160-9fce61b0b817)

S3 data is firstly sourced, then schema is changed by dropping the unnecessary column.
![image](https://github.com/user-attachments/assets/b4ccc1ae-60b5-4465-8d14-bcc97f3a5fc9)

Aggregate for particular year is calculated.
![image](https://github.com/user-attachments/assets/6d462d09-d352-4c8a-9007-1a6aa8a436fc)

After doing the same for total animals also, the data is joined by join feature
![image](https://github.com/user-attachments/assets/86d8be02-ff78-4fc1-b8f2-8b47d5cd7018)

After this percent redeemed is calculated and results are stored in curated folders.
![image](https://github.com/user-attachments/assets/c767dc96-0a6a-4ed0-b89f-b0e7b1a68aaf)

For data analysis Athena is used where table is structured and its content is viewed using SQL Queries.
![image](https://github.com/user-attachments/assets/ae62e376-1605-4411-bcbf-dc989c416dce)

Graph is created from the data obtained.
![image](https://github.com/user-attachments/assets/dbdf8d80-026d-437c-bb64-2bf6b3df5f43)

I created instances in EC2 and got the password for computer but could not connect to second computer as it showed error. I am attaching screenshot of instance and error.
![image](https://github.com/user-attachments/assets/33f43515-c5bd-48b0-a003-631146e27243)

We need to protect data from bad users due to confidentiality problem. If a bad user can change the data then it causes integrity problems. If bad user blocks the existing user then they can not use their data as it is not available to them. This is called availability problems. Your data is protected when you protect it against all these three problems.
User management: Here we identify which user is bad or good. There are 3 phases – identification, authentication, authorisation. If a user is able to pass all 3 phases then he/she is a good user.
Having unique username, email, phone is phase 1. Verifying ID is phase 2. Providing access is phase 3. We check two things – action, scope. I can edit the rows is action. I can edit my row in scope.
Open IAM and later roles. You will see all the permissions that are available in a role.
Open key management system and create key. We will create symmetric key by name “ animals-records-key-amanjot”. Add labrole permissions. Create key.
![image](https://github.com/user-attachments/assets/0f3c77b4-2d53-46c6-802f-d215152869fe)

Now the users in S3 bucket can view sensitive information. We need to encrypt it and decrypt it with key. Go to your S3 bucket  properties  default encryption. Select “Server-side encryption with AWS Key Management Service keys (SSE-KMS)”. For AWS key “Choose from your AWS KMS keys”. Select the suitable key.
![image](https://github.com/user-attachments/assets/a6d111a3-b76c-468c-8a37-b19fcc948355)

Enable the bucket version. By this each time you save your data a new version will be save. If one version is deleted you can still access the other version.
![image](https://github.com/user-attachments/assets/390b536b-e73a-4666-bb94-9cecc9db68c6)

Creating a backlog bucket by name ”animals-records-backup-amanjot”. We need the data to be automatically backed up in this. In the management tab of actual S3 bucket – create replication rule by name “ animals-recodrs-repru-amanjot” and apply it to all objects in bucket. Choose destination folder as ”animals-records-backup-amanjot”. For IAM role chose labrole. Choose “Replicate objects encrypted with AWS Key Management Service (AWS KMS)”. Provide the relevant KMS key. Save with “No, do not replicate existing objects.”
If you upload something in bucket now then it will be automatically backedup
![image](https://github.com/user-attachments/assets/c04e8de5-ef27-43e5-9da3-5abc47d9e50b)

In the previous phase of project we worked with operational dataset in landing zone. We did cleaning and stored it in raw zone. Later we analysed and summarized the dataset and saved it in curated zone. For data governance we need a new zone. I created “trusted” folder in my S3 bucket where data will be stored after we have checked it data quality, privacy, compliance. The data for all these checks is fetched from raw folder instead of curated zone because in curated all these checks have already been made. Now make a folder inside trusted zone for the particular dataset that you want to work on.
![image](https://github.com/user-attachments/assets/a4bdb82e-daf7-4d40-8752-3dbdca4da261)

Now we use Glue, and create a new ETL for implementing quality and privacy rules. Add name for pipeline as “animals-records-QRPR-amanjot08”. In the job details, change the IAM role to Lab role. First we need to fetch data from raw zone. To do this click + sign to add a node where we source data from Amazon S3. We update its name to “load_total_raw” and browse the location from raw folder. Change the data format to CSV. In the data preview we can see data that means it is fetched.
Now we should make a privacy check. This is done by choosing transform with “detect sensitive data”. Rename this node to “Detect Sensitive Data for total”. Select “specific patterns” from type of information to detect. Select Canada’s patterns as it is cost effective. For select global action choose “Redact” as this replace sensitive information to ******.
Now select +  Transform  Evaluate data quality. Rename it to “Evaluate Data Quality for total”. In the code on the right move the cursor to rules and add completeness. Change completeness threshold value as 95% so that we have 95% of rows with specific unique animal ID. For data quality output we need to select “original data with adding new columns to indicate data quality errors”. New node will be added by AWS and in data new columns will be added. 
We will use conditional router to divide this information into two categories : passed and failed. Now we need to drop the extra columns added by quality evaluation. So in the passed category change schema and drop columns.
Now we need to save the result in “trusted zone” . Chose +  target  Amazon S3. Browse the location of trusted dataset folder.
![image](https://github.com/user-attachments/assets/ecf02394-5b39-4235-845e-221f4a1cce4d)

Save and Run the ETL. Now we can see many files in trusted zone which are the results
![image](https://github.com/user-attachments/assets/7a5f16d3-0e98-42fa-abc3-277e9ccb8d18)

To automate these runs we can schedule it through schedules section  Set time for the same with frequency. We need workflow to run ETL on schedule. 
Create workflow with name “animals-records-workflow-amanjot”. Add trigger with the schedule.
![image](https://github.com/user-attachments/assets/a43fcf9b-2c20-46ef-8923-aa4ef09f01d3)

If monitored value is greater than the threshold value of metric then we need to take an action. Firstly open cloudwatch AWS service which is used for monitoring. Go to dashboards and create one. Name it “animals-records-dashboard-aman”. Add  widgets “Estimated charges” from billing and “number of objects” from Amazon S3 bucket. Change the time span to 1 month. To analyse these metrics we need to create alarm, if estimated charge for keeping an animal is more that $35, send me email as an alarm. Create alarm and add metrics, change frequency. Add threshold as 35$. Now move to next page which is an actian page where you provide an action that needs to be taken in case of monitored value is more than threshold. Create a topic, add email  give alarm name and create.
Update your dashboard with a new widget of alarm. This is monitoring and controlling dashboard.
![image](https://github.com/user-attachments/assets/40f2450c-3cac-4c47-8dba-956c2c132946)

Open cloudtrail, to track activities create trail. Give name as “records-animals-trail-amanjot”. We need to store this in new S3 bucket under the name “aws-cloudtrail-logs-animals-amanjot” use a KMS key as “animalslogskey”. Choose management events as “event type”. For activity choose “read” and “write”. And the trail is created.
![image](https://github.com/user-attachments/assets/77fb47b1-87b6-437f-a7ad-11454592fb2f)

To enhance the Data Analytic Platform (DAP) Architecture Analysis with examples from the screenshots, let’s break down each section with more detailed explanations tied to specific aspects visible in the screenshots.
1. Operational Excellence
In operational excellence, the key aspects include automation, monitoring, and continuous improvement.
•	Example: Data Monitoring
Screenshot: In the screenshot where the CloudWatch dashboard is shown, widgets are used to monitor the "Number of Objects" in the S3 bucket and "Estimated Charges" from AWS services (Fig.34).
Detail: The CloudWatch dashboard displays a real-time view of how many objects are stored in S3 and the total estimated charges associated with the AWS services being used. This helps the team stay informed of changes in data volume and costs, enabling quick action if thresholds are crossed. The team uses these dashboards to maintain operational excellence by continuously monitoring the platform and setting up alerts for early warning on costs and data storage.
o	Automation: AWS Glue automates ETL jobs, and CloudTrail audits every activity. The dashboard enables the team to monitor the costs and system performance without manual intervention.
o	Proactive Management: The setup in the dashboard allows the team to anticipate problems before they affect operations.
2. Security
Security is critical for protecting sensitive data, and AWS provides various tools to implement robust security features.
•	Example: Encryption and Access Control
Screenshot: A screenshot shows encryption setup in S3 using AWS KMS (Fig. 1 - Key Creation and Encryption) and IAM roles for managing access permissions (Fig. 3 - Replication).
Detail: In this example, the DAP uses AWS KMS to encrypt data stored in S3, ensuring confidentiality. The key management system (KMS) enables encryption of both data at rest and data in transit. IAM roles are used to restrict access to specific users who have the appropriate permissions to read, modify, or delete data. CloudTrail, as shown in another screenshot, logs every access request and action taken on the data (Fig.36  - CloudTrail logging), providing a robust audit trail for security purposes.
o	Data Masking: AWS Glue’s masking feature is also highlighted in screenshots. Data masking ensures that sensitive information, like license registration numbers, is protected during analysis, ensuring privacy compliance.
3. Reliability
Reliability ensures the DAP remains functional and recoverable in case of failure.
•	Example: S3 Replication and Versioning
Screenshot: In one of the screenshots, we see the backup S3 bucket and the replication rule setup (Fig. 4 - Replication Rules and Backup Creation).
Detail: The replication rule ensures that any data uploaded to the main S3 bucket is automatically copied to a backup bucket. Versioning is enabled in S3, meaning multiple versions of a file are maintained, allowing rollback to previous versions if the current one becomes corrupted or lost. This ensures data redundancy and reliability because, even in case of accidental deletion or modification, the platform can revert to older versions and restore operations quickly.
o	High Availability: AWS S3 is designed for high availability, and by using replication, the team ensures that even if one data center goes down, the data remains available in another location.
4. Performance Efficiency
This pillar ensures that the platform can scale and operate efficiently without wasting resources.
•	Example: Serverless Computing and ETL Pipelines
Screenshot: The ETL workflow creation in AWS Glue is depicted in screenshots showing the workflow setup for data quality and privacy checks (Fig. 19 - ETL Creation).
Detail: AWS Glue automates ETL processes (Extract, Transform, Load), ensuring that data is processed efficiently. This workflow fetches raw data from the S3 landing zone, applies data quality checks and privacy rules, and then stores the cleaned data in the "Trusted" zone. Since Glue is a serverless service, it only consumes resources when the job is running, thus optimizing performance. The serverless architecture of AWS Glue ensures scalability, meaning that as the volume of data grows, the system can handle increased loads without any manual intervention.
o	Athena Queries: In another screenshot, Athena is shown querying data directly from S3 without needing a dedicated infrastructure, improving efficiency by only paying for the queries executed.
5. Cost Optimization
Cost optimization focuses on minimizing expenses while maintaining system performance and reliability.
•	Example: Cost Monitoring and Alerts
Screenshot: CloudWatch’s cost monitoring dashboard shows a metric for estimated charges, with an alert configured to notify the team if costs exceed $45 (Fig. 34 - Cost Alert Setup).
Detail: The team uses a CloudWatch dashboard to track the estimated charges in real-time. By setting a threshold of $45, the team is notified if they approach their budget limit. This enables proactive cost management, allowing the team to adjust usage before incurring excessive charges. The screenshots also show the use of Athena for querying, which charges based on usage rather than requiring a dedicated instance, thus reducing overall costs.
o	Data Tiering: By categorizing data into landing, raw, and curated folders in S3, the DAP only stores critical data in higher-cost storage, further optimizing costs.
6. Sustainability
Sustainability is about reducing the platform's environmental impact while maintaining efficiency.
•	Example: Serverless Architecture
Screenshot: The serverless nature of AWS Glue and Athena is illustrated through the ETL pipelines and the lack of need for dedicated servers (Fig. 19 - ETL and Fig. 18 - Trusted Zone Creation).
Detail: AWS Glue and Athena are both serverless, meaning they only use resources when needed, reducing idle server time and energy consumption. This architecture minimizes the environmental footprint by efficiently utilizing AWS’s shared data center infrastructure rather than relying on physical servers that consume constant energy. By using AWS cloud services, the City of Vancouver also avoids the need for large on-premise data centers, contributing to energy savings and reduced carbon emissions.
o	Data Storage Efficiency: The platform stores data in different S3 tiers based on the processing stage, ensuring that the most important data uses higher-cost storage only when necessary, optimizing energy use and cost.
Conclusion
By using the AWS Well-Architected Framework, the DAP effectively incorporates the six key pillars into its architecture. The screenshots provide visual evidence of the key components at work—automation through Glue, encryption and access management via KMS, reliability through S3 versioning and replication, performance optimization via serverless services, real-time cost monitoring with CloudWatch, and sustainability with a serverless approach that reduces energy consumption. These examples show that the DAP for the City of Vancouver is not only secure and cost-efficient but also designed with future growth and sustainability in mind.

