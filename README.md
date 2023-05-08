# Expert Data Modeling with Power BI - Second Edition 
https://subscription.packtpub.com/book/data/9781803246246/3/ch03lvl1sec11/summary


## Introduction to Data Modeling in Power BI

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

Snowflaking

Snowflaking is when we do not have a perfect star schema when dimension tables surround the fact tables. In some cases, we have some levels of descriptions stored in different tables. Therefore, some dimensions in the model are linked to other tables describing the dimensions in more detail. Snowflaking is normalizing the dimension tables. In some cases, snowflaking is inevitable; nevertheless, the general rule of thumb in data modeling in Power BI (when following a star schema) is to avoid snowflaking as much as possible to have a simpler and more performant model.

Understanding denormalization

In real-world scenarios, not everyone has the luxury of having a pre-built data warehouse designed in a star schema. In reality, snowflakes in data warehouse designs are inevitable. The data models are usually connected to various data sources, including transactional database systems and non-transactional data sources such as Excel files and CSV files. So, we almost always need to denormalize the model to a certain degree. Depending on the business requirements, we may have some normalization along with some denormalization. The reality is that there is no specific rule for the level of normalization and denormalization. The general rule of thumb is denormalizing the model, so each dimension describes all the related details as much as possible.

#### The iterative data modeling approach 

Taking advantage of an agile approach would be genuinely beneficial for Power BI development. Here is the iterative approach to follow in Power BI development:
![image](https://user-images.githubusercontent.com/118057504/236272413-74d07daa-daf6-40a2-a433-b7a3de461e11.png)

Conducting discovery workshops

The Power BI development process starts with gathering information from the business by conducting discovery workshops to understand the requirements better. A business analyst may take care of this step in the real world, and many Power BI users are indeed business analysts. Whether we are business analysts or not, we are data modelers, so we need to analyze the information we receive from the business. We have to ask relevant questions that lead us toward various design possibilities. We have to identify potential risks, limitations, and associated costs and discuss them with the customer. After we get the answers, we can confidently take the next steps to design a data model that best covers the business requirements, mitigates the risks, and aligns with the technology limitations well.

Data preparation based on the business logic

We have a lot on our plate by now. We must get the data from multiple sources and go through the data preparation steps. Now that we have learned a lot about business logic, we can take the proper data preparation steps. For instance, if the business needs to connect to an OData data source and get a list of the columns required, we can prepare the data more efficiently with all the design risks and technology limitations in mind. After consciously preparing the data, we go to the next step, data modeling.

Data modeling

If we properly go through the previous steps, we can build the model more efficiently. We need to think about the analytical side of things while considering all the business requirements, design possibilities, risks, and technology limitations. For instance, if the business cannot tolerate data latency longer than 5 minutes, we may need to consider using DirectQuery. Using DirectQuery comes with some limitations and performance risks. So, we must consider the design approach that satisfies the business requirements the most. We cover DirectQuery in Chapter 4, Getting Data from Various Sources, in the Dataset storage modes section.

Testing the logic

One of the most trivial and yet most important steps in data modeling is testing all the business logic we implement to meet the requirements. Not only do we need to test the figures to ensure the results are accurate, but we also need to test the solution from a performance and user experience perspective. Be prepared for tons of mixed feedback and sometimes strong criticism from the end users, especially when we think everything is OK.

Demonstrating the business logic in basic data visualizations

As we are modeling the data, we do not need to worry about data visualization. Confirming with the business SMEs (Subject Matter Experts) is the fastest way to ensure all the reporting logic is correct. The fastest way to get confirmation from SMEs is to demonstrate the logic in the simplest possible way, such as using table and matrix visuals and some slicers on the page. When demonstrating the solution to SMEs, do not forget to highlight that the visualization is only to confirm the reporting logic with them and not the actual product delivery. In the real world, we have many new discussions and surprises during the demonstration sessions, which usually means we are at the beginning of the second iteration, so we start gathering information about the required changes and the new requirements, if any.

We gradually become professional data modelers as we go through the preceding steps several times. This book also follows an iterative approach, so we go back and forth between different chapters to cover some scenarios.

## Data Analysis eXpressions (DAX)

### Understanding virtual tables
Whenever we use a DAX function that results in a table of values, we are creating a virtual table.
When generating or loading the data from other tables, taking some table operations and loading the results into a calculated table, we probably created a virtual table first, then populated the calculated table with the results. 

#### Creating a calculated table
We want to create a calculated table and name it Sequential Numbers with an ID column. The ID column values are sequential numbers between 1 and 20, increasing by one step in each row:

 - Open a new Power BI Desktop instance.
 - Click New table from the Modeling tab.
 - Type the following DAX expression, then press Enter:

```
Sequential Numbers = GENERATESERIES(1, 20, 1)
```

The GENERATESERIES() function generates values for us. The output is a desirable table, but we need to do one last operation to rename the Values column to ID.

- Replace the previous expression with the following expression, then press Enter:

```
Sequential Numbers =
SELECTCOLUMNS(
    GENERATESERIES(1, 20, 1)
    , "ID"
    , [Value]
    )
 ```
    
    In the preceding scenario, we used a virtual table to create a calculated table. In the following scenario, we demonstrate the usage of virtual tables in a measure.

 ####  Using virtual tables in a measure – Part 1

Create a measure in the Internet Sales table to calculate the quantities ordered for products when their list price is higher than $1,000. Name the measure Orders with List Price Bigger than or Equal to $1,000:

```
Orders with List Price Bigger than or Equal to $1,000 =
CALCULATE(
    SUM('Internet Sales'[OrderQuantity])
        , FILTER('Product' //Virtual table start
            , 'Product'[List Price]>=1000
                ) //Virtual table end
        )
```

 - We created a virtual table using the FILTER() function on top of the Product table to get only the products with a List Price value bigger than or equal to $1,000. All columns from the Product tables are available in this virtual table, which lives in memory. It is only available within the Orders with List Price Bigger than or Equal to $1,000 measure and nowhere else in the data model.
 - The SUM() function then calculates the summation of the OrderQuantity from the Internet Sales table.
 
 #### Using virtual tables in a measure – Part 2
 
Using the same sample file used in the previous scenario, create a measure in the Internet Sales table to calculate quantities that the customers ordered more than 4 products with a list price bigger than $1,000. Name the measure Order Qty for Customers Buying More than 4 Products with List Price Bigger Than $1,000.

To solve this scenario, we need to create a virtual table of customers with more than 4 orders for products that cost more than $1,000. The following code will take care of that:

```
Order Qty for Customers Buying More than 4 Product with List Price Bigger Than $1,000 =
SUMX(
    FILTER(
        VALUES(Customer[CustomerKey]) //Virtual table
        , [Orders with List Price Bigger than or Equal $1,000] > 4
        )
    , [Orders with List Price Bigger than or Equal $1,000]
    )
    
 ```

Analyzing the preceding calculation helps us to understand the power of virtual tables much better:

 - The virtual table here has only one column, which is CustomerKey. Remember, this column is only accessible within the current calculation.
 - Then, we use the FILTER() function to filter the results of VALUES() only to show the customer keys that have more than 4 orders for products with a list price of more than $1,000.
 - Last, we sum all those quantities for the results of FILTER().

As stated earlier, we can use all DAX functions that return a table value to create virtual tables. However, the functions in the following table are the most common ones:

![image](https://user-images.githubusercontent.com/118057504/236339075-966246ee-7424-43bf-b08a-db0b83f4c110.png)

### Using DAX Studio
DAX Studio is one of the most popular third-party tools, created by the amazing team at SQLBI, available to download for free. You can get it from the tool's official website (https://daxstudio.org/). You can easily use DAX Studio to see the results of your virtual tables. First of all, you need to open your Power BI file (*.pbix) before you can connect to it from DAX Studio. Open DAX Studio and follow these steps:

 - Click PBI/SSDT Model.
 - Select a desired Power BI Desktop instance.
 - Type in the EVALUATE statement, then copy and paste the Virtual Table part of the calculation on a new line:
```
EVALUATE
FILTER('Product' //Virtual table start
            , 'Product'[List Price]>=1000
                ) //Virtual table end
```   

 - Press F5 or click the Run button to run the query:
 - ![image](https://user-images.githubusercontent.com/118057504/236340475-465e7c7d-8b5e-42a8-9f9d-f36d0332be24.png)
 - In DAX Studio, you can run DAX queries, which must start with the EVALUATE statement.


#### Understanding relationships in virtual tables

When we think about tables in relational systems, we usually think about tables and their relationships. So far, we have learned about virtual tables. Now let us think about the relationships between virtual tables and other tables (either physical tables available in the data model or other virtual tables). As stated earlier, we create a virtual table by generating it, constructing it, or deriving it from an existing table within the data model. Moreover, there are some cases where we can create more than one virtual table to calculate results. 

When it comes to virtual tables, there are two types of relationships:

Suppose a virtual table has been derived from an existing physical table in the data model. There is a relationship between the virtual table and the original physical table, which is the so-called <b>lineage</b>.
In some cases, we create more than one virtual table in a calculation. Then, we create relationships between those virtual tables programmatically. In some other cases, we might need to replace an existing relationship with a new one. This type of relationship is called a<b> virtual relationship</b>.

Using virtual tables is an effective technique we can use on many occasions. A great example is when there is no relationship between two tables, and we cannot create a physical relationship between the two as the relationship is only legitimate in some business cases.

Let’s look at more complex scenarios with multiple virtual tables with inter-virtual relationships. The business needs to calculate Internet Sales in USD. At the same time, there are several values in the Internet Sales table in other currencies.
The model contains an Exchange Rates table. As per the scenario, the base currency is USD. That is why the AverageRate column is always 1 when Currency is USD. In other words, the Exchange Rates table contains all currency ratings for each day compared to USD. The following figure shows the Exchange Rates data:

![image](https://user-images.githubusercontent.com/118057504/236799627-cf7448cc-c315-4bfc-8c4d-a86ab4082f1c.png)

To calculate the internet sales in USD for a specific date, we need to find the relevant CurrencyKey for that specific date in the Exchange Rates table, then multiply the value of SalesAmount from the Internet Sales table by the value of AverageRate from the Exchange Rates table.

The following measure caters to that:
```
Internet Sales USD =
SUMX(
    NATURALINNERJOIN (
            SELECTCOLUMNS(
                'Internet Sales'
, "CurrencyKeyJoin", 'Internet Sales'[CurrencyKey] * 1
                , "DateJoin", 'Internet Sales'[OrderDate] + 0
                , "ProductKey", 'Internet Sales'[ProductKey]
                , "SalesOrderLineNumber", 'Internet Sales'[SalesOrderLineNumber]
                , "SalesOrderNumber", 'Internet Sales'[SalesOrderNumber]
                , "SalesAmount", 'Internet Sales'[SalesAmount]
                )
            , SELECTCOLUMNS (
                'Exchange Rates'
                , "CurrencyKeyJoin", 'Exchange Rates'[CurrencyKey] * 1
                , "DateJoin", 'Exchange Rates'[Date] + 0
                , "AverageRate", 'Exchange Rates'[AverageRate]
            )
        )
, [AverageRate] * [SalesAmount]
)
```
![image](https://user-images.githubusercontent.com/118057504/236800123-14c94f6b-33a4-4c59-be0c-07b158008bbe.png)

Looking at the Exchange Rates table, we see that the combination of the values of the CurrencyKey and Date columns ensures the uniqueness of the rows.

As the preceding figure shows, we first create two virtual tables. We join those two virtual tables using two columns, the CurrencyKeyJoin and DateJoin columns. If you look at the construction of the two columns, you see the following:

We added 0 days to 'Internet Sales'[OrderDate] to construct DateJoin for the virtual table derived from the Internet Sales table. We did the same to 'Exchange Rates'[Date] to construct DateJoin for the virtual table derived from the Exchange Rates table.
We multiplied CurrencyKey by 1 to construct the CurrencyKeyJoin column in both virtual tables.


At this stage, you may ask why we need to do any of this. The reason is purely to make the NATURALINNERJOIN() function work. The NATURALINNERJOIN() function, as its name implies, performs an inner join of a table with another table using the same column names with the same data type and the same lineage.

We want to perform an inner join between the two virtual tables based on the following columns:

'Internet Sales'[OrderDate] → 'Exchange Rates'[Date]
'Internet Sales'[CurrencyKey] → 'Exchange Rates'[CurrencyKey]

The first requirement for the NATURALINNERJOIN() function is to have the same column names in both joining tables. To meet that requirement, we renamed both the OrderDate column from the Internet Sales table and the Date column from the Exchange Rates table to DateJoin.

The second requirement is that the columns participating in the join must have the same data type. We already meet this requirement.
The last requirement, which is the most confusing one yet, is that the columns contributing to the join must have the same lineage if there is a physical relationship between the two tables. If there is no physical relationship between the tables, the columns participating in the join must not have lineage to any physical columns within the data model. Therefore, we need to break the lineage of the join columns. Otherwise, the NATURALINNERJOIN() function does not work. We need to use an expression rather than an actual column name to break the lineage. Therefore, we add 0 days to 'Internet Sales'[OrderDate] and multiply 'Exchange Rates'[CurrencyKey] by 1 to break the lineage of those two columns.

Finally, we have the desired result set, so we can multiply [SalesAmount] by [AverageRate] to get the sales amount in USD.

The following figure shows the Internet Sales (without considering the exchange rates) and Internet Sales USD measures side by side:
![image](https://user-images.githubusercontent.com/118057504/236800872-6c4cbed8-023e-4d66-9040-b20aa0eaf1a0.png)

<!-- #### Time intelligence and data modeling
Detecting valid dates in the date dimension
When dealing with periodic calculations in time intelligence, it is often hard to get just valid dates to show in the visuals.

Let's have a look at how to do this with a scenario.

A business needs to see the following calculations:

Internet Sales Month to Date (MTD)
Internet Sales Last Month to Date (LMTD)
Internet Sales Year to Date (YTD)
Internet Sales Last Year to Date (LYTD)
Internet Sales Last Year Month to Date (LY MTD)
Writing the preceding measures is super easy using the existing DAX functions. The calculations are as follows.

To calculate MTD, use the following DAX expressions:
```
Internet Sales MTD =
TOTALMTD(
    [Internet Sales]
    , 'Date'[Full Date]
)
```

Use the following DAX expressions to calculate LMTD:

```
Internet Sales LMTD =
TOTALMTD(
    [Internet Sales]
    , DATEADD('Date'[Full Date], -1, MONTH)
    )

```

Use the following DAX expressions to calculate YTD:

```
Internet Sales YTD =
TOTALYTD(
    [Internet Sales]
    , 'Date'[Full Date]
    )

```

Use the following DAX expressions to calculate LYTD:

```
Internet Sales LYTD =
TOTALYTD (
    [Internet Sales]
    , DATEADD('Date'[Full Date], -1, YEAR)
)

```

Finally, use the following DAX expressions to calculate LY MTD:

```
Internet Sales LY MTD =
TOTALMTD(
    [Internet Sales]
    , SAMEPERIODLASTYEAR('Date'[Full Date])
)

```

The SAMEPERIODLASTYEAR('Date'[Full Date])and DATEADD('Date'[Full Date], -1, YEAR) act the same. We used different functions to demonstrate the possibilities. -->

### Data Preparation in Query Editor

#### Introducing the Power Query M formula language in Power BI

Power Query is a data preparation technology offering from Microsoft to connect to many different data sources from various technologies, enabling businesses to integrate data, transform it, make it available for analysis, and get meaningful insights from it. Power Query can currently connect to many data sources.

It also provides a custom connectors software development kit (SDK) that third parties can use to create their data connectors.

https://learn.microsoft.com/en-us/power-query/power-query-what-is-power-query?WT.mc_id=DP-MVP-5003466#where-can-you-use-power-query

Power Query M is a formula language capable of connecting to various data sources to mix and match the data between those data sources, which then loads into a single dataset. In this section, we introduce Power Query M.

#### Power Query is CaSe-SeNsItIvE
While Power Query is a case-sensitive language, Data Analysis Expressions (DAX) is not.
Not only is Power Query case-sensitive in terms of syntax, but it is also case-sensitive when interacting with data. For instance, we get an error message if we run the following function:
```
datetime.localnow()
```

This is because the following is the correct syntax:
```
DateTime.LocalNow()
```

Ignoring Power Query’s case sensitivity in data interactions can become an issue that is hard and time-consuming to identify. A real-world example is when we get globally unique identifier (GUID) values from a data source containing lowercase characters. Then, we get some other GUID values from another data source with uppercase characters. When we match the values in Power Query to merge two queries, we do not get any matching values. But if we turn the lowercase GUID into uppercase, the values match.

Queries
In Power Query, a query contains expressions, variables, and values encapsulated by let and in statements. A let and in statement block is structured as follows:

```
let  
   Variablename = expression1,  
   #"Variable name" = expression2  
in   
   #"Variable name"
```

As the preceding structure shows, we can have spaces in the variable names. However, we need to encapsulate the variable name using a number sign (#) followed by quotation marks—for example, #”Variable Name”. By defining a variable in a query, we create a query formula step in Power Query. Query formula steps can reference any previous steps. Lastly, the query output is the variable that comes straight after the in statement. Each step must end with a comma, except the last step before the in statement.

Expressions
In Power Query, an expression is a formula that results in values. For instance, the following image shows some expressions and their resulting values:

![image](https://user-images.githubusercontent.com/118057504/236884377-941d929c-e876-466d-ad50-5864a82707d0.png)

Values

In Power Query, values fall into two general categories: primitive values and structured values.

Primitive values
A primitive value is a constant value such as a number, a text, a null, and so on. For instance, 123 is a primitive number value, while "123" (including quotation marks) is a primitive text value.

Structured values
Structured values contain either primitive values or other structured values. There are four kinds of structured values: list, record, table, and function values:
 - List value: A list is a sequence of values shown in only one column. We can define a list value using curly brackets {}. For instance, we can create a list of lowercase English letters using {"a".."z"} or a list of numbers between 1 and 10 by using {1..10}. The following image shows the list of English letters from a to z:
 
 ![image](https://user-images.githubusercontent.com/118057504/236884806-84fc332d-0968-40a1-a1d9-24d46887f353.png)

 - Record value: A record is a set of fields that make up a row of data. To create a record, we use brackets []. Inside the brackets, we mention the field name and an equal sign followed by the field’s value. We separate different fields and their values using a comma, as follows:
```

[
    First Name = "Soheil"
    , Last Name = "Bakhshi"
    , Occupation = "Consultant"
    ]

```

The following image shows the expression and the values:

![image](https://user-images.githubusercontent.com/118057504/236885593-84b77bd1-fcc6-40d0-b42f-008c3b6438d8.png)

When defining a record, we do not need to put the field names in quotation marks. As illustrated in the previous image, records are shown vertically.

 - Table value: A table is a set of values organized into columns and rows. Each column must have a name. There are several ways to create a table using various Power Query functions. Nevertheless, we can construct a table from lists or records. Figure 3.5 shows two ways to construct a table, using the <i>#table</i> keyword shown in the next code snippet:
Here is the first way to construct a table:

```
#table( {"ID", "Fruit Name"}, {{1, "Apple"}, {2, "Orange"}, {3, "Banana"}})
```

Here is the second way to construct a table:

```
#table( type table [ID = number, Fruit Name = text], {{1, "Apple"}, {2, "Orange"}, {3, "Banana"}} )
```

The following image shows the results:

![image](https://user-images.githubusercontent.com/118057504/236886533-ef8bb3c5-38bd-41e8-aaa7-372fedfdf2fb.png)

As you can see in the preceding image, we defined the column data types in the second construct, while in the first one, the column types are <b>any</b>.

 - Function value: A function is a value that accepts input parameters and produces a result. To create a function, we put the list of <i>parameters</i> (if any) in <i>parentheses</i>, followed by the output <i>data type</i>. We use <i>the goes-to symbol (=>)</i>, followed by the function’s definition. For instance, the following function calculates the end-of-month date for the current date:
 
```
()as date => Date.EndOfMonth(Date.From(DateTime.LocalNow()))
```

The preceding function does not have any input parameters but produces an output. The following image shows a function invocation without parameters that returns the end-of-month date for the current date (today’s date): 

![image](https://user-images.githubusercontent.com/118057504/236888747-8b62ed89-1396-47a7-a607-d22ca140cbb9.png)

#### Introduction to Power Query Editor

Applied Steps
The Applied Steps contain the transformation steps applied to the selected query in sequential order. Each step usually references its previous step, but it is not always the case. We might create some steps referring to one of the previous steps or not referring to any steps at all. For example, we may want to take some operations over the current date. So we use the DateTime.LocalNow() function in a step without referring to any previous steps. The following options are available when right-clicking each transformation:

 - Edit Settings: This option is enabled if there is a UI available for the selected transformation step.
 - Rename: It is good to give each transformation step a meaningful name so we can quickly recognize what each step does. Use this option to rename the selected transformation step.
 - Delete: Use this option to delete the selected transformation step.
 - Delete Until End: Deletes the selected transformation step and all its following steps.
 - Insert Step After: Inserts a new step after the selected step.
 - Move before: Moves up the selected step.
 - Move after: Moves down the selected step.
 - Extract Previous: Creates a new query by moving all previous steps to the selected one and references the new query in the current query while keeping the selected step and all its following steps in the current query.
 - View Native Query: When we connect to a relational database such as a SQL Server instance, Power Query tries to translate the expressions into the native query language supported by the source system, which is T-SQL for a SQL Server data source. This option is enabled if Power Query can translate the selected transformation step into the native query language. If the source system does not have any query languages or Power Query cannot translate the step into the native query language, then this option is disabled. We discuss the query folding concept in detail in Chapter 7, Data Preparation Common Best Practices.
 - Diagnose: We can diagnose a query for performance tuning, analyzing query folding, and more. We look at query diagnostics in Chapter 7, Data Preparation Common Best Practices, in more detail.
 - Properties: We can use this option to add some descriptions to the selected transformation step to explain what it does and how it works in more detail.

#### Understanding query parameters
One of the most valuable features is the ability to define query parameters. We can then use defined query parameters in various cases. For instance, we can create a query referencing a parameter to retrieve data from different datasets, or we can parameterize filter rows. With query parameters, we can parameterize the following:

 - Data Source
 - Filter Rows
 - Keep Rows
 - Remove Rows
 - Replace Rows
In addition, we can load the parameters’ values into the data model to reference them from measures, calculated columns, calculated tables, and report elements if necessary.

We can easily define a query parameter from Power Query Editor, as follows:

1 Click Manage Parameters.
2 Click New.
3 Enter a name.
4 Type in an informative description that helps the user understand the parameter’s purpose.
5 Checking the Required box makes the parameter mandatory.
6 Select a type from the drop-down list.
7 Select a value from the Suggested Values drop-down list.
8 Depending on the suggested values selected in the previous step, you may need to enter some values (this is shown in Figure 3.35). If you selected Query from the Suggested Values drop-down list, we need to select a query in this step.
9 Again, depending on the selected suggested values, we may/may not see the default value. If we selected List of values, we need to pick a default value.
10 Pick or enter a value for Current Value.
11 Click OK.
The preceding steps are illustrated in the following image:

![image](https://user-images.githubusercontent.com/118057504/236935405-2b220737-530f-4d86-b500-9a718bf98718.png)

<b>Note:</b> It is best practice to avoid hardcoding the data sources by parameterizing them. Some organizations consider their data source names and connection strings as sensitive data. They only allow PBIT files to be shared within the organization or with third-party tools, as PBIT files do not contain data. So, if the data sources are not parameterized, they can reveal server names, folder paths, SharePoint Uniform Resource Locators (URLs), and so on. Using query parameters with Suggested Values of Any value makes perfect sense to avoid data leakage.

The number of use cases for query parameters is quite vast. Let’s look at a real-world scenario when using query parameters comes in handy.

In this scenario, we want to parameterize the data sources. Parameterizing a data source is helpful. One of the most significant benefits of parameterizing data sources is avoiding hardcoding the server names, database names, files, folder paths, and so on.

The business has a specific BI governance framework requiring separate Development (Dev), User Acceptance Testing (UAT), and Production (Prod) environments. The business wants us to produce a sales analysis report on top of the enterprise data warehouse in SQL Server. We have three different servers, one for each environment, hosting the data warehouse. While a Power BI report is in the development phase, it must connect to the Dev server to get the data from the Dev database. When the report is ready for testing in the UAT environment, we must switch both the server and the database to the UAT environment. When the UAT people have done their testing, and the report is ready to go live, we need to switch the server and the database again to point to the Prod environment. To implement this scenario, we need to define two query parameters. One keeps the server names, and the other keeps the database names. Then, we set all relevant queries to use those query parameters. It is much easier to implement such a scenario if we use the query parameters from the beginning of the project. The process is easy even if there is currently an existing Power BI report, and we must parameterize the data sources. Once we set it, we do not need to change any code in the future to switch between different environments. Let’s create a new query parameter as follows:

1 In Power Query Editor, click Manage Parameters.
2 Click New.
3 Enter the parameter name as Server Name.
4 Type in a description.
5 Check the Required field
6 Select the Type as Text from the drop-down list.
7 Select List of values from the Suggested Values drop-down list.
8 Enter the server names in the list.
9 Select devsqlsrv01\edw as the Default Value.
10 Pick devsqlsrv01\edw again as the Current Value.
11 Click OK.

The following image highlights the preceding steps:

![image](https://user-images.githubusercontent.com/118057504/236936011-c5674350-079f-4899-acba-45f9975962d7.png)


If we already have some queries, then we need to modify the data sources as follows:

1 Click a query to parameterize.
2 Click the gear icon of the first step, Source.
3 Select Parameter from the Server dropdown.
4 Select the Server Name parameter from the Server parameters dropdown.
5 Select Parameter again from the Database dropdown.
6 Select the Database Name parameter from the database parameters dropdown.
7 Click OK.

![image](https://user-images.githubusercontent.com/118057504/236937416-db0caddf-c929-4244-9889-6f8346f49570.png)

We need to go through similar steps to parameterize other queries. After finishing the parameterization, we only need to change the parameters’ values whenever we want to switch the data sources. To do so from Power Query Editor, proceed as follows:

1 Click the Manage Parameters drop-down button.
2 Click Edit Parameters.
3 Select the UAT Server Name.
4 Select the UAT Database Name.
5 Click OK.

![image](https://user-images.githubusercontent.com/118057504/236937625-df0409d9-b0dd-4cfd-87df-ddb8ee7ac442.png)

