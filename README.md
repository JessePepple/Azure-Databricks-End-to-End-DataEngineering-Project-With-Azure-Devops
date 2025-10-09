This Data Engineering project showcases the extensive capabilities of Azure Databricks, Azure Data Factory (ADF), and Azure Data Lake, while highlighting key data engineering practices such as Slowly Changing Dimensions SCD Type 1(Manually) and Type 2(Automatic via DLT), Star Schema design, and incremental loading. My approach not only involved ingesting source data efficiently using Spark Structured Streaming but also designing robust SCD Type 1 pipelines. After transforming and curating the data from the staging layer to the core layer, the final datasets were delivered to both Azure Synapse Analytics and Databricks SQL Warehouse, making them fully prepared for analytics and reporting.

<img width="1444" height="902" alt="Group 5" src="https://github.com/user-attachments/assets/028dac7e-824f-4079-a22e-3e45eea82fbc" />

## Creating And Ingesting Orders Source Data with ADF and Azure Devops

<img width="1434" height="726" alt="Screenshot 2025-10-06 at 16 51 30" src="https://github.com/user-attachments/assets/6e23a8d2-4ba1-4b3b-a4b8-5bd2451a8b8e" />

To kickstart the project, I first logged into the Azure DevOps organization page, where I set up the project details and created a dedicated branch to begin ingesting the source raw data with Azure Data Factory.

## Configuring The Repository In DataFactory
After creating the branch, I switched back to DataFactory to configure the repository setup, preparing the environment for the data ingestion process.
<img width="1434" height="726" alt="Screenshot 2025-10-06 at 16 53 38" src="https://github.com/user-attachments/assets/65057348-8d10-44e2-8296-4805a105055f" />

<img width="1434" height="726" alt="Screenshot 2025-10-06 at 16 56 09" src="https://github.com/user-attachments/assets/376c91bf-a09f-4e5b-b3f2-5edd25616ccb" />
I ingested the data using the Lookup and ForEach activities in Azure Data Factory, transforming the source files from CSV to Parquet format during the process. Once the ingestion workflow was successfully executed, I planned to merge this branch back into the main repository.
<img width="1434" height="726" alt="Screenshot 2025-10-06 at 17 39 51" src="https://github.com/user-attachments/assets/d28501db-4512-4eec-a357-559572e24ff5" />
<img width="1434" height="717" alt="Screenshot 2025-10-07 at 20 34 53" src="https://github.com/user-attachments/assets/73610997-6fa2-4a17-99b8-e7eb176d1824" />
## Pipeline Template
<img width="1434" height="652" alt="Screenshot 2025-10-06 at 17 49 52" src="https://github.com/user-attachments/assets/dcd9bdac-cc7a-43c9-b363-e695b253517f" />

## Commits
<img width="1434" height="652" alt="Screenshot 2025-10-06 at 17 42 58" src="https://github.com/user-attachments/assets/3477eb89-a6ed-4f45-89b1-e029b7f7f962" />

## MergeIllustration
<img width="1434" height="717" alt="Screenshot 2025-10-06 at 18 07 46" src="https://github.com/user-attachments/assets/979ffedc-6248-4ee8-8378-d909e82b0ad9" />

<img width="1434" height="726" alt="Screenshot 2025-10-06 at 16 51 17" src="https://github.com/user-attachments/assets/fa282f95-be70-4e0c-9769-32e665cf5a8c" />

Now that our source data is stored in the Data Lake, it’s time to kickstart the main phase of the project by loading the data into the staging layer — our Bronze container — where raw datasets are initially processed and prepared for further transformation.

### Phase 1 Ingesting Data to the Staging Layer Using Spark Structured Streaming
Before kickstarting the ingestion process in Databricks, I first created a Catalog to organize and manage our project’s data assets. Additionally, I configured External Locations to establish secure connections between Databricks and our containers within the Azure Data Lake(Unity Catalog is already enabled in my databricks as shown in the image).
<img width="1434" height="717" alt="Screenshot 2025-10-07 at 19 43 05" src="https://github.com/user-attachments/assets/de5ec94a-89ab-4a52-a09d-8cbe05dcbd6b" />

<img width="1434" height="717" alt="Screenshot 2025-10-07 at 19 43 14" src="https://github.com/user-attachments/assets/9900f7c3-43a4-4cc5-b33b-0f46bb5e7d65" />



<img width="1434" height="717" alt="Screenshot 2025-10-07 at 20 34 42" src="https://github.com/user-attachments/assets/1c8ffe74-407a-4acf-aec0-1633e11ae19c" />

## Ingestion With AutoLoaders
The ingestion of data using Spark Structured Streaming was successful on the first attempt, as I was able to load data from the source container into the bronze container. One of the key advantages of Spark Structured Streaming is its support for incremental data loading, which I implemented in this project. By setting the trigger to once=True, the stream processes new data automatically as soon as it lands in the Data Lake, while previously processed data is tracked and managed through the defined Checkpoint Location.

<img width="1434" height="717" alt="Screenshot 2025-10-07 at 21 43 48" src="https://github.com/user-attachments/assets/529acc87-ce70-4bfd-b7f5-860b5b1ba1f7" />
<img width="1434" height="717" alt="Screenshot 2025-10-07 at 21 47 58" src="https://github.com/user-attachments/assets/d7a1ef84-aaf5-41a8-8088-ffb388710f28" />



## First Ingestion(Our Fact Table)
The first data ingestion focused on our fact table—though not fully defined at this stage. As shown, the initial dataset contained 9,990 rows. To demonstrate incremental loading, I added a new file with 10 additional rows to the bronze container in the Data Lake. Once the new file landed, Spark Structured Streaming automatically detected and processed it, seamlessly updating the dataset incrementally without reprocessing existing data.
<img width="1434" height="717" alt="Screenshot 2025-10-07 at 22 00 30" src="https://github.com/user-attachments/assets/bc5e6da5-a8ae-416c-97a2-f924ec1c1ca8" />

<img width="1434" height="120" alt="Screenshot 2025-10-07 at 22 00 55" src="https://github.com/user-attachments/assets/d02cee9d-5c57-483e-ad13-0438dcdcb9aa" />

<img width="1434" height="717" alt="Screenshot 2025-10-07 at 21 47 21" src="https://github.com/user-attachments/assets/b0079af9-a35f-41f9-8e63-4c333894ce64" />

## After adding Our Data

After the addition of our new data Spark Streaming Ingested the Incremental load successfully also ofcourse creating a new column for rescued data(For data that are not aligned to the schema of our table) 



<img width="1434" height="203" alt="Screenshot 2025-10-07 at 22 03 47" src="https://github.com/user-attachments/assets/01a6c52b-c3c7-425a-9976-4daa52a7368a" />

<img width="1434" height="203" alt="Screenshot 2025-10-07 at 22 04 25" src="https://github.com/user-attachments/assets/e3d18a4d-0c94-4a67-aaad-c37c4f91fd8d" />

## Preparing Incremental Data

<img width="1434" height="702" alt="Screenshot 2025-10-07 at 23 31 56" src="https://github.com/user-attachments/assets/538a45ea-68ac-4aaf-a06e-affee94ccdc6" />

<img width="1434" height="702" alt="Screenshot 2025-10-07 at 23 32 07" src="https://github.com/user-attachments/assets/8e248b03-e281-481c-b06e-da91928b487a" />

## Ingesting our other Data With Parameter Notebooks

Using dbutils.widgets.text(), I parameterized the notebook to enable flexible and efficient ingestion of all datasets in a single run. By leveraging a ForEach loop, I automated the loading of data into the Data Lake, ensuring each table—including newly added datasets—was ingested into its respective location. The Regions table, however, was excluded from this process, as it lacked meaningful context and relevance for our curated tables and core layer.

<img width="1434" height="702" alt="Screenshot 2025-10-07 at 22 30 23" src="https://github.com/user-attachments/assets/ccc2d318-719f-4f9a-8be6-b63d7c0f5bb8" />

<img width="1434" height="702" alt="Screenshot 2025-10-07 at 22 53 58" src="https://github.com/user-attachments/assets/34999c0b-3e5c-41bd-b8e5-3b7844cc82c3" />



<img width="1434" height="702" alt="Screenshot 2025-10-07 at 22 30 14" src="https://github.com/user-attachments/assets/8fbd71d7-bf83-4060-a228-516ef93b43e3" />
<img width="1434" height="702" alt="Screenshot 2025-10-07 at 23 00 23" src="https://github.com/user-attachments/assets/b26d8f6c-ec3f-4da8-8e47-a0c5f7bd71fa" />



## Our Load Was Successful

<img width="1434" height="702" alt="Screenshot 2025-10-07 at 23 26 42" src="https://github.com/user-attachments/assets/6106f143-b3bd-4929-8658-0fbf72b87346" />

<img width="1434" height="702" alt="Screenshot 2025-10-07 at 23 26 23" src="https://github.com/user-attachments/assets/647325d0-1bb3-4816-bbd5-933512389b1c" />


<img width="1434" height="702" alt="Screenshot 2025-10-07 at 23 34 14" src="https://github.com/user-attachments/assets/0fc1cccf-94c1-418a-8288-b3e4ef0872cc" />
<img width="1434" height="702" alt="Screenshot 2025-10-07 at 23 35 55" src="https://github.com/user-attachments/assets/54d01c44-1ba9-4e21-9939-0ba2eb398311" />

<img width="746" height="702" alt="Screenshot 2025-10-07 at 23 37 59" src="https://github.com/user-attachments/assets/d3605a4c-e639-4466-af05-1ecec46db48e" />


<img width="746" height="702" alt="Screenshot 2025-10-07 at 23 37 44" src="https://github.com/user-attachments/assets/d5f3cb3c-3919-4e85-b124-32290475f4ac" />



As shown with the Region table, no changes were detected since no new data was added—demonstrating that our incremental load process worked successfully.
<img width="746" height="657" alt="Screenshot 2025-10-07 at 23 38 25" src="https://github.com/user-attachments/assets/aa0b7da5-45a7-4868-97c1-a799624902c6" />


### Phase 2 Transformations
After performing the necessary transformations for our enriched (Silver) layer, particularly on the Orders (Fact) table and two of our Dimension tables, I proceeded to implement Slowly Changing Dimension (SCD) Type 1. In this case, I chose to handle it manually by splitting the DataFrames into old and new datasets to create a pseudo table. This approach allowed me to efficiently track Dimension Keys during incremental loads and easily determine the maximum DimensionKey value without complications.
## Manual SCD TYPE 1(UPSERT)
<img width="1423" height="700" alt="Screenshot 2025-10-09 at 03 48 18" src="https://github.com/user-attachments/assets/00d00dfe-99cf-4bc2-a838-23ea10d5db17" />

## Automatic SCD TYPE 2
Using Delta Live Tables (DLT), a declarative ETL framework, I was able to automate SCD Type 2 logic with ease. Unlike Type 1, which I handled manually using incremental loads and a pseudo table, DLT simplified the process for Type 2, ensuring reliable and efficient tracking of historical changes whilst also setting data quality checks to improve data standards. Despite the manual approach for Type 1, the solution was successful and met the project requirements.

As seen there were no dropped data so our data quality check is a success
<img width="1423" height="705" alt="Screenshot 2025-10-09 at 06 34 09" src="https://github.com/user-attachments/assets/fb602b6f-0262-4b1a-ae7a-c2e866b4fc95" />
<img width="1423<img width="1423" height="433" alt="Screenshot 2025-10-09 at 06 44 35" src="https://github.com/user-attachments/assets/2022535c-d5b1-454d-89a0-a16f108d0e8c" />
" height="705" alt="Screenshot 2025-10-09 at 06 39 30" src="https://github.com/user-attachments/assets/ca82b65d-0f46-4b93-b956-c32b4abc3f04" />
<img width="1423" height="700" alt="Screenshot 2025-10-09 at 04 58 43" src="https://github.com/user-attachments/assets/d8564cbf-cdfe-4451-933e-fcd5267e8106" />

<img width="1423" height="433" alt="Screenshot 2025-10-09 at 06 44 35" src="https://github.com/user-attachments/assets/82e6f9a5-182b-47bf-b891-fbeed78dc3fc" />

### Phase 3 Serving(Star Schema)
With our two Dimension tables loaded, the next step was to finalize the Star Schema by preparing the Fact Table. Once complete, the curated datasets were delivered to Databricks SQL Warehouse and Azure Synapse Analytics, making them fully ready for analysis, reporting, and business intelligence. After the finalisation of Our Star Schema I applied SCD Type 1 Logic on it for Upserts as it is a big table and updating and inserting of new data is plausible

<img width="573" height="617" alt="Screenshot 2025-10-09 at 11 50 21" src="https://github.com/user-attachments/assets/15b47fa9-58f9-49e2-a134-b042e9260aa1" />

Now that our data has been loaded, transformed, and fully curated, it is ready for analysis and reporting.

### Databricks SQL Warehouse 

I tested out our curated datsets with the SQL Warehouse playing with Queries and creating dashboards.


<img width="1419" height="705" alt="Screenshot 2025-10-09 at 07 46 53" src="https://github.com/user-attachments/assets/fdef7201-5fb9-45e3-a5e1-fcb6e2bf8946" />
<img width="1419" height="705" alt="Screenshot 2025-10-09 at 07 47 13" src="https://github.com/user-attachments/assets/653214ec-de6f-4faf-8e8e-ce231c116768" />

<img width="1419" height="705" alt="Screenshot 2025-10-09 at 07 47 37" src="https://github.com/user-attachments/assets/37b13e53-879b-4ca4-a69d-c0f4d23b8e68" />

<img width="1419" height="705" alt="Screenshot 2025-10-09 at 07 50 13" src="https://github.com/user-attachments/assets/f2e23b2b-7c94-4452-97c6-c65608f2ffeb" />

### BI Partner Connect
Thanks to Databricks Partner Connect, I was able to provide the BI connector to the Data Analyst, enabling them to directly query and visualize the cleaned data in Power BI with ease—without needing to rely on the SQL Data Warehouse. Followed after was loading my data in Synapse Warehouse thus, the conclusion of my project.
<img width="1419" height="705" alt="Screenshot 2025-10-09 at 07 48 18" src="https://github.com/user-attachments/assets/ed44c7a0-bd23-450b-af29-b380c646c08b" />

### Loading In Synapse
To load our curated data, I used CETAS functions and OPENROWSET to create External Tables, carefully defining the data sources and file formats to ensure accurate and efficient table creation.
<img width="1419" height="705" alt="Screenshot 2025-10-09 at 08 36 3<img width="1419" height="705" alt="Screenshot 2025-10-09 at 09 00 48" src="https://github.com/user-attachments/assets/d31db5ac-0847-4b73-92b7-d7d9b3f86f50" />
3" src="https://github.com/user-attachments/assets/4e4e82![Uploading Screenshot 2025-10-09 at 09.00.48.png…]()
77-56be-40f4-9707-b1ba8e91b069" />
<img width="1419" height="705" alt="Screenshot 2025-10-09 at 08 37 14" src="https://github.com/user-attachments/assets/31131c51-75dc-42d1-80f0-76fdde958936" />
<img width="1419" height="705" alt="Screenshot 2025-10-09 a![Uploading Screenshot 2025-10-09 at 09.00.48.png…]()
t 08 57 19" src="https://github.com/user-attachments/assets/9efce4de-b90a-49bf-be26-12350cefbba1" />



<img width="1419" height="705" alt="Screenshot 2025-10-09 at 09 26 41" src="https://github.com/user-attachments/assets/42a245e6-89f3-4067-b751-ab24c3651ffc" />

<img width="1419" height="705" alt="Screenshot 2025-10-09 at 09 50 38" src="https://github.com/user-attachments/assets/eb625828-0925-4bf4-ab37-da5ddbb5d39e" />


<img width="1419" height="705" alt="Screenshot 2025-10-09 at 09 51 00" src="https://github.com/user-attachments/assets/ca040470-c047-41d7-8fea-a776a09cc209" />

<img width="1419" height="705" alt="Screenshot 2025-10-09 at 10 04 54" src="https://github.com/user-attachments/assets/db2c37b7-dc31-4fe6-bbec-a86176ea845f" />



<img width="1419" height="705" alt="Screenshot 2025-10-09 at 10 06 05" src="https://github.com/user-attachments/assets/362749af-7aa0-4c14-8ab0-7448d427e523" />
<img width="1419" height="705" alt="Screenshot 2025-10-09 at 10 06 27" src="https://github.com/user-attachments/assets/fdf27db2-9d33-47b1-a807-4a6cd53bd526" />






<img width="1419" height="705" alt="Screenshot 2025-10-09 at 09 54 27" src="https://github.com/user-attachments/assets/5bbae19f-e846-4089-a248-5841ac9f957e" />













