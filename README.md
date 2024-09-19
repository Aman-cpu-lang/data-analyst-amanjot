# data-analyst-amanjot
Introduction
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
https://github.com/Aman-cpu-lang/data-analyst-amanjot/blob/main/Picture9.png
S3 data is firstly sourced, then schema is changed by dropping the unnecessary column.
https://github.com/Aman-cpu-lang/data-analyst-amanjot/blob/main/Picture10.png
Aggregate for particular year is calculated.
https://github.com/Aman-cpu-lang/data-analyst-amanjot/blob/main/Picture11.png
After doing the same for total animals also, the data is joined by join feature
https://github.com/Aman-cpu-lang/data-analyst-amanjot/blob/main/Picture12.png
After this percent redeemed is calculated and results are stored in curated folders.
https://github.com/Aman-cpu-lang/data-analyst-amanjot/blob/main/Picture13.png
For data analysis Athena is used where table is structured and its content is viewed using SQL Queries.
https://github.com/Aman-cpu-lang/data-analyst-amanjot/blob/main/Picture14.png
Graph is created from the data obtained.
https://github.com/Aman-cpu-lang/data-analyst-amanjot/blob/main/Picture15.png
I created instances in EC2 and got the password for computer but could not connect to second computer as it showed error. I am attaching screenshot of instance and error.
https://github.com/Aman-cpu-lang/data-analyst-amanjot/blob/main/Picture16.png
