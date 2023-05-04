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
