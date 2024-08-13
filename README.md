# Top Indian Instagram Influencers 2024

## Objective
The primary goal of this project is to analyze and visualize the effectiveness of Indian Instagram influencers in 2024. The analysis focuses on key metrics such as follower counts, engagement rates, potential reach, and the overall impact of influencers across various sectors including acting, music, cricket, politics, etc.

## Data Source
The data for this project consists of a comprehensive list of Indian influencers, with metrics such as:
- Name
- Followers
- Engagement Rate
- Influence Area
- Potential Reach

The dataset is used from Kaggle. [Get data source](https://www.kaggle.com/datasets/bhavyadhingra00020/top-100-social-media-influencers-2024-countrywise?resource=download)

## Tools & Technologies
- **Excel:** For initial data exploration and preparation.
- **SQL:** To perform data cleaning, transformations, and advanced queries.
- **Power BI:** To transform data, create interactive dashboards and visualizations.

## Analytical Features
### Dashboard Creation
The project includes the design and development of interactive dashboards that display key metrics:
- Most influential personalities based on Engagement Rate (ER) and followers.
- Industry comparison (e.g., acting vs. sports).
- Trends and influencer growth over time.

This project transforms raw social media data into actionable insights, aiming to enhance marketing strategies and maximize the impact of influencer partnerships.

## Process Overview

### 1. Get the Data
- The dataset is obtained from the Kaggle website.

### 2. Explore the Data in Excel, in this stage, the data is being explored, and after closely examining the dataset, the following points were observed:
- **Data Exploration:** Initial exploration of the dataset to understand its structure and identify necessary columns for analysis.
- **Column Selection:** Focused on five essential columns, with irrelevant ones removed.
- **Name Extraction:** The "USERNAME" column contains both the name and user ID of the influencer. Using Excel's Flash Fill, the name was extracted and verified for accuracy.
- **Column Renaming:** The column headers were initially in capital letters and required renaming for clarity. This renaming will be done in SQL.
- **Data Type Correction:** Some columns had inappropriate data types, which will be corrected using Power Query Editor in Power BI.

### 3. Load the data in SQL Server
- Extract & Transform the data

```sql
/*
# 1. Select the required columns
# 2. Extract the user id from 'USERNAME' column
# 3. Rename the columns name 
*/

SELECT 
     Name, SUBSTRING(USERNAME, CHARINDEX('@', USERNAME) + 1, LEN(USERNAME)) AS Username, 
     FOLLOWERS AS Followers, ER AS Engagement_Rate, TOPIC_OF_INFLUENCE AS Influence_Area,
     POTENTIAL_REACH AS Potential_Reach 
FROM 
     instagram_data_india;
```
- Create the SQL view 
```sql
/*
# 1. Create a view to store the transformed data
# 2. Select the required columns from the instagram_data_india SQL table 
*/

CREATE VIEW Top_Influencers_2024 AS

SELECT 
     Name, SUBSTRING(USERNAME, CHARINDEX('@', USERNAME) + 1, LEN(USERNAME)) AS Username, 
     FOLLOWERS AS Followers, ER AS Engagement_Rate, TOPIC_OF_INFLUENCE AS Influence_Area,
     POTENTIAL_REACH AS Potential_Reach 
FROM 
     instagram_data_india;
```

### 4. Visualize the data in Power BI

Before creating visualizations, we need to transform the data in the Power Query Editor:

1. **Transform Data**:
   - Columns "Followers," "Engagement_Rate," and "Potential_Reach" contain inappropriate data types, including letters and percentage signs.
   - Replace unwanted characters (e.g., percentage signs) and choose the correct data types for these columns.

### Visualizations
Now coming on to Visualizations, we have used:
Table,Treemap, Clustered bar chart, Pie chart, Scatter Chart, Cards

#### 1. Table

The table visualization provides a clear and organized view of the raw data, displaying exact figures for each influencer. It shows:
- Name
- Followers
- Engagement Rate
- Influence Area
- Potential Reach

#### 2. Treemap

The treemap visualization displays the relative number of followers for each influencer. The top 10 influencers are selected, and the size of each box represents the number of followers, making it easy to identify the influencers with the largest following.

#### 3. Clustered Bar Chart

The clustered bar chart is used to compare multiple variables across different influencers. For example, it compares:
- Potential Reach
- Engagement Rate


#### 4. Pie Chart

The pie chart visualizes the proportion of the total followers attributed to each influencing area. It highlights which Influence Area has the largest share of followers, providing insight into the distribution of influence.

#### 5. Scatter Chart

The scatter chart is used to explore the relationship between two numerical variables:
- Engagement Rate vs. Number of Followers

This chart helps identify correlations between follower count and engagement rate, such as whether influencers with more followers tend to have higher or lower engagement rates.

#### 6. Cards

Two cards are used to display summary statistics:
- **Card 1**: Average Engagement Rate by Influencing Area
- **Card 2**: Average Potential Reach by Influencing Area

These cards provide quick insights into average engagement and potential reach across different influence areas.

https://github.com/user-attachments/assets/858fb21c-7435-473a-b074-dab912bbbd0b

## DAX Measures

### 1. Total Followers(M)
```sql
Total Followers(M) = 
VAR Totalfollowers = SUM(Top_Influencers_2024[Followers])
RETURN Totalfollowers

```

### 2. Total Engagement Rate
```sql
Total Engagement Rate(%) = 
VAR ER = SUM(Top_Influencers_2024[Engagement_Rate])
RETURN ER

```

### 3. Avg Followers By Influencing Area
```sql
AvgFollowersByTopic = 
CALCULATE(
    AVERAGE('Top_Influencers_2024'[Followers]), 
    ALLEXCEPT('Top_Influencers_2024', 'Top_Influencers_2024'[Influence_Area])
)

```

### 4. Avg Engagement By Influencing Area
```sql
AvgEngagementByArea = 
CALCULATE(
    AVERAGE('Top_Influencers_2024'[Engagement_Rate]), 
    ALLEXCEPT('Top_Influencers_2024', 'Top_Influencers_2024'[Influence_Area])
)

```

### 5. Top Influencers Reach 
```sql
TopCelebrityReach = 
CALCULATE(
    SUM('Top_Influencers_2024'[Potential_Reach]), 
    TOPN(10, 'Top_Influencers_2024', 'Top_Influencers_2024'[Potential_Reach], DESC)
)
 

```

### 6. Total Followers By Influencing Area
```sql
TotalFollowersByArea = 
SUMX(
    VALUES('Top_Influencers_2024'[Influence_Area]), 
    CALCULATE(SUM('Top_Influencers_2024'[Followers]))
)

```

### 5. Conclusions from the Analysis

Based on the data, here are some insights and findings that can be derived using the visualizations from Power BI:

#### 1. General Observations

**Top 10 Influencers by Followers:**
- **Virat Kohli** leads with 267.1 million followers, significantly higher than the rest.
- **Narendra Modi** and **Alia Bhatt** follow with 87.3 million and 83.7 million followers, respectively.

![Screenshot (237)](https://github.com/user-attachments/assets/47a09b4e-bae3-4a0e-97e8-e8b97be6a5f3)

**Top 10 Influencers by Engagement Rate:**
- **Sidhu Moosewala** has the highest engagement rate at 23.22%, indicating strong follower interaction.
- **Sushant Singh Rajput** and **MC Stan** also have very high engagement rates at 19.81% and 13.19%, respectively.

![Screenshot (238)](https://github.com/user-attachments/assets/9599ba43-72d5-4b04-ac67-87ab3cad876b)

**Top 10 Influencers by Potential Reach:**
- **Virat Kohli** has the highest potential reach of 80.1 million.
- **Narendra Modi** follows with a potential reach of 26.2 million, showing his significant influence despite the lower follower count compared to Virat Kohli.

![Screenshot (239)](https://github.com/user-attachments/assets/ef2edb95-21e8-479c-b06a-a519ac7a8e8c)

#### 2. Specific Findings

**High Engagement Influencers:** Influencers with relatively lower followers but very high engagement rates like Sidhu Moosewala, MC Stan, and Sushant Singh Rajput could be high-value targets for marketing.

![Screenshot (246)](https://github.com/user-attachments/assets/9c86f639-e889-4fc0-a8c7-d88498f6fe3f)
![Screenshot (245)](https://github.com/user-attachments/assets/2e631d44-9eb3-431e-b605-2e35f51be5fe)

**Celebrity Influence in Acting:** Acting dominates the influence categories, with many top influencers like Alia Bhatt, Katrina Kaif, and Deepika Padukone coming from this sector. This suggests that the film industry is a significant driver of influence in India.

![Screenshot (242)](https://github.com/user-attachments/assets/3a9d0913-a75e-49ad-9da4-ee87cd91771b)

**Cricket's Influence:** While not as many cricketers are in the top follower rankings, those who are (like Virat Kohli and MS Dhoni) have exceptionally high potential reach and influence, indicating that cricket remains a powerful area of influence.

![Screenshot (243)](https://github.com/user-attachments/assets/caa3b3c6-69e5-46be-a65b-6e8f7badfdf3)

**Emerging Influencers:** Influencers from areas like YouTube (e.g., Carry Minati, Bhuvan Bam) and Fashion & Modeling (e.g., Faisal Shaikh, Riyaz Aly) are also prominent, showing the rise of digital and social media platforms as new avenues of influence.
