# EatFoodWise (Restaurant quality metrics aggregator)

## Vision
- Goal of the project is to build a data pipeline aggregating various data points pertaining to restaurants collected from a variety of data sources and providing analytical insights into restaurant quality over a timeline.
- Analytical insights generated has the following use cases:
  - While choosing a restaurant, **customers** look at various quality metrics(ambience, taste, service, hygiene) and a lot of review sites to make their decision. It is also commonly observed that restaurant quality varies for better/worse over a timespan. Our goal is to visualize these metrics in an easily observable way to help customers with their choices. Not all metrics are available from all the data sources. For example, restaurant hygiene related data points are gathered from data published by city health departments. Note: Extraction of data, not available via apis, can be done via web scraping etc. Work pertaining to this is considered out of scope of this project.
  - Restaurant insights aggregating different data points are also informative to **restaurant owners/corporate chains** to come up with action items to improve restaurant performance.
    - Visualizing correlation between various data point over a selected time period(restaurant rating vs hygiene).
    - Outliers in ratings collected from different review sites could indicate potential fraud.





## Outline
- Provide transparency into the quality of a restaurant over different time periods  to help consumer make better decisions.
- Restaurant quality metrics are aggregated from various data sources(google, yelp reviews, health department grades etc).
- Data collected is fed into data processing pipeline, built off AWS Glue(a serverless data integration service) using S3(object storage service offering scalability, data availability, security, and performance) as the storage layer. Data post extraction/transformation is exposed for interactive analytics on Aws Athena(serverless interactive query service). Athena service is integrated with Apache Superset to provide further data exploration and visualization capabilities.
- Data ingestion into the system is  done using various triggers
  - AWS cloudwatch cron: Raw data uploaded to S3 is processed as micro batches using glue jobs.
  - On-demand triggers for glue jobs  

 The data visualized using Apache Superset  is hosted on DigitalOcean droplet as a cost-effective method to create and share the visulization.



![Getting Started](./images/foodieViews.svg)

## [Dashboard](TBD)


## Model

The data lake consists of foundational fact, dimension, and aggregate tables developed using dimensional data modeling techniques that can be accessed by engineers and data scientists in a self-serve manner to power data engineering. The ETL (extract, transform, load) pipelines that compute these tables are thus mission-critical.Our aim is to have data freshness and give engineering efforts that process data as quickly as possible to keep it up to date.
In order to achieve the data freshness in our ETL pipelines, a key challenge is incrementally updating these modeled tables rather than recomputing all the data with each new ETL run. This is also necessary to operate these pipelines cost-effectively.   In this project, we aim to achieve an incremental data processing model which is efficient and cost-effective at the same time.


###Setup

The raw data for health_grade, google_reviews and yelp_reviews were generated locally and were inserted into amazon S3's input bucket. The raw data of the reviews were designed in sach a way that the review corresponding to a particular review_id could be updated in cases where reviewer has a change of mind and updated the review to a new value. The design is based on an incremental data processing model. New data is uploaded on a regular interval in the raw data bucket. When new data is added, a trigger is initiated causing Glue job run and therby process the data. The trigger could be scheduled trigger, conditional trigger or on-demand trigger. In all cases, the glue job runs and data is processed.
The glue job processes the data modeling it into form that could be further used to perform meaningful queries and to visualize the data.
AWS Athena is the serverless interactive analytics service we use to run meanigful queries on the processed data tables thereby giving insights on the restaurants.
We can visualize the results of our query and more in our visualization platform, which in our case is, Apache Superset. Being an open source visualization platform, Superset offers a cost-effective way to visualize our data. We can plot the correlation between health_grade and customer reviews, the change the customer reviews as well as health_grade  of a restaurant over period of time and much more. This could help not just the consumers while choosing a restaurant , but the business owners as well in improving the quality and customer experience.
The Apache superset dashboard is hoted on a DigitalOcean droplet as yet another step towards cost effectiveness



