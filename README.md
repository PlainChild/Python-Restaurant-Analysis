# Python Restaurant Analysis

## Key Objectives
Using Python, we analyzes restaurant data to compare the availability of online ordering services across different types of restaurants (e.g., Buffet, Cafes, Dining) and visualizes the findings using charts. This data analysis will address the questions:
1. Do a greater number of restaurants provide online delivery as opposed to offline services?
2. Which types of restaurants are the most visited by the general public?
3. What price range is preferred by couples for their dinner at restaurants?


### Dataset Link : http://raw.githubusercontent.com/PlainChild/Python-Restaurant-Analysis/refs/heads/main/Restaurant-rating-data.csv


## Workflow 
1. Import all necessary library (panda, numpy, and Matplotlib).

        import numpy as np
        import pandas as pd
        import matplotlib.pyplot as plt
        import seaborn as sns
2. Import the dataset.

        df = pd.read_csv('http://raw.githubusercontent.com/PlainChild/Python-Restaurant-Analysis/refs/heads/main/Restaurant-rating-data.csv')
3. Data cleaning.
- Dataset information (there is no data missing)

        df.info()
- Data duplication (there is data duplication)

        df[df.duplicated()]
- Change 'rate' data type into float and remove its denominator (/)

        df["rate"] = df["rate"].str.split("/").str[0].astype(float)
4. Check how many restaurant by type

        sns.countplot(x=df['listed_in(type)'])
![Image](https://github.com/user-attachments/assets/d998a30b-54c5-4b3d-980c-23d1e790ec60)
5. Check how many votes by restaurants type.

        vote_per_type = df.groupby('listed_in(type)')['votes'].sum()
        plt.plot(vote_per_type)
![Image](https://github.com/user-attachments/assets/6cd78a2e-f377-4e9c-9640-2c51b89d2a39)
6. check the most visited restaurant by having the most votes. 

        most_votes = df['votes'].max()
        most_votes_rest = df.loc[df['votes'] == most_votes]
|index|name|online\_order|book\_table|rate|votes|approx\_cost\(for two people\)|listed\_in\(type\)|
|---|---|---|---|---|---|---|---|
|38|Empire Restaurant|Yes|No|4\.4|4884|750|other|
7. Check how many restaurant has online order service.

        sns.countplot(x=df['online_order'])
![Image](https://github.com/user-attachments/assets/27af3c3f-9127-448c-98ac-2011955aae42)
8. Check the rating distributions

        plt.hist(dataframe['rate'])
![Image](https://github.com/user-attachments/assets/2fdf64e6-88bc-47a7-9f54-612149b6837b)
9. Check the preferred cost for a couple spent in a restaurant
- it is has been decided, the popular restaurants are top 25% of most votes

        threshold = df['votes'].quantile(0.75)
        popular_restaurants = df[df['votes'] >= threshold]
- Create bar chart

        sns.countplot(
                x=pop_restaurants['approx_cost(for two people)'],
                order=pop_restaurants['approx_cost(for two people)'].value_counts().index) # Order the data from most popular restaurants
![Image](https://github.com/user-attachments/assets/b78ea7a4-4909-4bb2-8476-75c9ba935cb5)
10. compare rating of restaurants has online ordering and restaurants who has not

        sns.boxplot(x = 'rate', y = 'online_order', data = df)
![Image](https://github.com/user-attachments/assets/cb5d1d5e-e948-4425-bcff-3a670e686ed6)
11. Last, crete heatmap based on restaurant type and has online order or no.

        pivot_table = df.pivot_table(index='online_order', columns='listed_in(type)', aggfunc='size')
        sns.heatmap(pivot_table, annot=True, cmap='seismic')
![Image](https://github.com/user-attachments/assets/fdac4a11-4297-459d-84e3-211facd72c5e)
## Insights & Findings
1. The majority of the restaurants fall into the dining category.
2. Dining restaurants are likely preferred by customers or definitely the most visited, since it has the most votes activity.
3. Majority of the restaurants do not accept online orders.
4. The majority of restaurants received ratings ranging from 3.75 to 4.
5. Based on popular restaurants (top 25% most voted), The majority of couples prefer restaurants with an approximate cost of between 600-800.
6. Offline orders received lower ratings in comparison to online orders, which obtained excellent ratings.
7. Based on the boxplot for ratings, Offline orders received lower ratings in comparison to online orders, which obtained excellent ratings.
8. Based on heatmap, Dining restaurants primarily accept offline orders, whereas cafes primarily receive online orders. This suggests that cliecustomers nts prefer to place orders in person at restaurants, but prefer online ordering at cafes.
