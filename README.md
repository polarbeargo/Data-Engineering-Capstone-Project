# Data-Engineering-Capstone-Project  

### Data dictionary  
[Kaggle Berkeley Earth Climate Change: Earth Surface Temperature Data
Exploring global temperatures since 1750](https://www.kaggle.com/berkeleyearth/climate-change-earth-surface-temperature-data)  
Global Land and Ocean-and-Land Temperatures (GlobalTemperatures.csv):  

- Date: starts in 1750 for average land temperature and 1850 for max and min land temperatures and global ocean and land temperatures  
- LandAverageTemperature: global average land temperature in celsius  
- LandAverageTemperatureUncertainty: the 95% confidence interval around the average  
- LandMaxTemperature: global average maximum land temperature in celsius   
- LandMaxTemperatureUncertainty: the 95% confidence interval around the maximum land temperature   
- LandMinTemperature: global average minimum land temperature in celsius  
- LandMinTemperatureUncertainty: the 95% confidence interval around the minimum land temperature  
- LandAndOceanAverageTemperature: global average land and ocean temperature in celsius  
- LandAndOceanAverageTemperatureUncertainty: the 95% confidence interval around the global average land and ocean temperature  

Global Land Temperatures By City (GlobalLandTemperaturesByCity.csv):  
- Date: starts in 1750 for average land temperature and 1850 for max and min land temperatures and global ocean and land temperatures  
- AverageTemperature: global average land temperature in celsius  
- AverageTemperatureUncertainty: the 95% confidence interval around the average 
- City: global cities list 
- Country: global countries list
- Latitude: Latitude 
- Longitude: Longitude

#### Raw data:
[Kaggle Berkeley Earth Climate Change: Earth Surface Temperature Data
Exploring global temperatures since 1750](https://www.kaggle.com/berkeleyearth/climate-change-earth-surface-temperature-data)   
Fact Table Source: [GlobalLandTemperaturesByCity.csv](https://www.kaggle.com/berkeleyearth/climate-change-earth-surface-temperature-data?select=GlobalLandTemperaturesByCity.csv)
- dt (object)  
- AverageTemperature (float64)  
- AverageTemperatureUncertainty (float64)  
- City (object)  
- Country (object)  
- Latitude (object)  
- Longitude (object)  

Source: https://www.kaggle.com/berkeleyearth/climate-change-earth-surface-temperature-data?select=GlobalTemperatures.csv
- dt (object)
- LandAverageTemperature (float64)
- LandAverageTemperatureUncertainty (float64)
- LandMaxTemperature (float64)
- LandMaxTemperatureUncertainty (float64)
- LandMinTemperature (float64)
- LandMinTemperatureUncertainty (float64)
- LandAndOceanAverageTemperature (float64)
- LandAndOceanAverageTemperatureUncertainty (float64)  

### Conceptual Data Model  

Climate change is close relate to time series analysis so I chosen that model to make time series analysis based on country, city and their AverageTemperature, AverageTemperatureUncertainty with LandAndOceanAverageTemperature base on dt, year, month levels as simple as possible. So all this information is stored in the fact tables. The purpose of this structure is to perform better time series analysis at the city and country level by year or by month. So i create two two dimension tables that come into question: city and country 

### Dimension Table  
Dimension_table_GlobalLandTemperaturesByCountry.csv  
- dt (object)  
- year (int) create from dt  
- month (int) create from dt
- AverageTemperature (float64)  
- AverageTemperatureUncertainty (float64)    
- Country (object)  
    
Dimension_table_GlobalLandTemperaturesByCity.csv
- dt (object)  
- AverageTemperature (float64)  
- AverageTemperatureUncertainty (float64)  
- City (object)
- LandAndOceanAverageTemperature (float64) join from GlobalTemperatures.csv
- year (int) create from dt  
- month (int) create from dt

### Mapping Out Data Pipelines  
![](starSchema.png)  

### Instructions

Step 1: Scope the Project and Gather Data  
Step 2: Explore and Assess the Data  
Step 3: Define the Data Model  
Step 4: Run ETL to Model the Data  
Step 5: Complete Project Write Up  


### Tools and technologies for the project.   
 - The raw data is accessible in .csv format. Python's pandas is a great choice here for information cleaning and crunching. The amount of information is little and can be prepared very efficiently in dataframes. I set up the ETL pipeline and quality checks with pyspark. The Airflow can be an important addition to allow schedule of tasks according to a scheduler and make the process more agile and automatic without losing control of the steps.    
 
### The schedule for Data Updates.      
 - The information ought to be upgraded on a every day premise, as the raw data is updated on a every day premise as well.  
  
### Scenarios:  
  - If the data was increased by 100x. 
   Let us imagine we are deploying a AWS EMR Start Cluster instance .xlarge with its linearly scalable, we may simply add the number of clusters to increase the performance thus we will be able to adjust the size and number of clusters as we see fit. if the data increased by 100x, we can save data to S3 and switch from a .xlarg to .10xlarge. This one has 640 GiB should be be able to handle it. Another way is to split the data into smaller chunks that can run in parallel thus make the process faster and more efficient.

  - If the pipelines were run on a daily basis by 7am.  
    - The Airflow helps us monitored and scheduler the runtime to avoid running past the 7am. The output data must be stored in an accessible database or accessible in S3 allow the data to search every day if there is new data coming. Assume the pipeline takes approximate 1h to run. Then a schedule should be set for running the pipeline every night at 5am leave a buffer of 1h or combination of Airflow + Spark + Apache Livy in EMR cluster so that Spark commands can be passed through an API interface.     
  - If the database needed to be accessed by 100+ people.  
    - Amazon Redshift Clusters are scaleable with elastic resize such that when ever the database or data warehouse runs the risk of not response the requests anymore, its performance could be increased to handle requests of the authorized 100+ people. Another way is with Amazon RDS, we can deploy scalable PostgreSQL DBs. Through AWS IAM features we can manage user access to our clusters. We could even use something like Amazon Cognito to manage user accounts. It allows us to create a custom sign-up + login page as well as a user management system.    

### ETL Result Query
![](ETLResultQuery.png)
