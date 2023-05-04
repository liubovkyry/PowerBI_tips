# PowerBI_tips

### Understanding the Power BI layers

  The following illustration shows a straightforward process we usually go through while building a report in Power BI Desktop:
  ![image](https://user-images.githubusercontent.com/118057504/236256972-b8c3fde4-ed35-4036-9951-75e6903735fb.png)
We use different conceptual layers of Power BI to go through the preceding processes:
 - The Power Query (data preparation) layer
 - The Model layer (data model)
 - The Report layer (data visualization)
 
 The data preparation layer (Power Query)
 
In this layer, we get the data from various data sources, transform and cleanse that data, and make it available for the next layer. This is the first layer that touches the data, so it is an essential part of the data journey in Power BI. In the Power Query layer, we decide which queries load data into the data model and which ones take care of data transformation and data cleansing without loading the data into the data model

The data model layer

This layer has two views, the Data view and the Model view. In the Data view, we see the data being loaded; in the Model view, we see the data model, including the tables and their relationships.

The data visualization layer

In this layer, we bring the data to life by making meaningful and professional-looking data visualizations. The data visualization layer is accessible via the Report view, the default view in Power BI Desktop.

### How data flows in Power BI

Understanding how data flows during its journey in Power BI is vital. For instance, when we face an issue with some calculations in a report, we know how to analyze the root cause and trace the issue back to an actionable point. When we find an incorrect value in a line chart, and the line chart uses a measure dependent on a calculated column, we know that we do not find that calculated column in Power Query. The reason is that the objects we create in the data model are not accessible in Power Query. So, in that sense, we never look for a measure in the Power Query layer. We also do not expect to use custom functions created within Power Query in the data model layer.

### What data modeling means in Power BI

A well-designed data model in Power BI must be optimized for querying the data and reducing the dataset size by aggregating the data.

The best practice is to avoid importing everything from the data sources into Power BI and solving the related problems later such as performance issues, data model complexities, and having unnecessarily large data models. Instead, it is wise to get the data model right to precisely answer business-driven questions in the most performant way. When modeling data in Power BI, we must build a data model based on the business logic. So, we may need to join different tables and aggregate the data to a level that answers all business-driven questions, which can be tricky when we have data from various data sources of different grains.

### Building an efficient data model in Power BI

An efficient model must do the following:

 - Perform well (be fast)
 - Be business-driven
 - Decrease the complexity of the required DAX expressions (easy to understand)
 - Low maintenance (low cost)
 
Some related questions we might ask the business to optimize the performance of the report and avoid customer dissatisfaction:
 - Do we need to import all the columns from those tables?
 - Do we also need to import all data, or is just a portion of the data enough? For example, if the data source contains 10 years of data, does the business need to analyze all the historical data, or does bringing 1 or 2 years of data fit the purpose?
 Excel: Excel files with many formulas can be tricky data sources.
 - Does the business require you to analyze all the data contained in the all sheets? We may be able to exclude some of those sheets.
 - How often are the formulas edited? This is critical as modifying the formulas can easily break the data processing in Power Query and generate errors. It is best to replicate the Excel formulas in Power BI and load the raw data from Excel before the formulas are applied.

#### Star schema (dimensional modeling) and snowflaking
First things first, the star schema and dimensional modeling are the same things. In Power BI data modeling, the term star schema is more commonly used. The following sections are generic reminders about some star schema concepts.

#### Transactional modeling versus star schema modeling
In transactional systems, the main goal is improving the solution’s performance in creating new records and updating/deleting existing ones. So, when designing transactional systems, it is essential to go through the normalization process to decrease data redundancy and increase data entry performance by breaking the tables down into master-detail tables.

But the goal of a business analysis system is very different. In a business analysis solution, we need a data model optimized for querying in the most performant way.

Let us continue with a scenario. Suppose we have a transactional retail system for an international retail shop. We have hundreds of transactions every second from different parts of the world. The company owners want to see the total sales in the past 6 months.

This calculation sounds easy. It is just a simple SUM of sales. But wait, we have hundreds of transactions every second, right? If we have 100 transactions per second, then we have 8,640,000 transactions a day. So, for 6 months of data, we have more than 1.5 billion rows. Therefore, a simple SUM of sales takes a reasonable amount of time to process.

Now, the business raises a new request. The company owners now want to see the total sales in the past 6 months by country and city. They simply want to know what the best-selling cities are.

We need to add another condition to our simple SUM calculation, which translates into a join to the geography table. For those coming from a relational database design background, it is trivial that joins are relatively expensive operations. This scenario can go on and on. So, you can imagine how quickly a simple scenario turns into a rather tricky situation.

In the star schema, however, we already joined all those tables based on business entities. We aggregated and loaded the data into denormalized tables. In the preceding scenario, the business is not interested in seeing every transaction at the second level. So, summarizing the data at the day level decreases the row count from 1.5 billion to approximately 18,000 rows for the 6 months. Now you can imagine how fast the summation would run over 18,000 instead of 1.5 billion rows.

The idea of the star schema is to keep all numeric values in separate tables called fact tables and put all descriptive information into other tables called dimension tables. Usually, the fact tables are surrounded by dimensions explaining the facts. A data model with a fact table in the middle surrounded by dimensions looks like a star, which is why this modeling approach is called a star schema.
