# Pandas-Assignment

# Heroes of Pymoli - Homework Assignment

Eugene Miao

ou must use the Pandas Library and the Jupyter Notebook.
You must submit a link to your Jupyter Notebook with the viewable Data Frames.
You must include an exported markdown version of your Notebook called  README.md in your GitHub repository.
You must include a written description of three observable trends based on the data.
See Example Solution for a reference on expected format.

#import pandas information

import os
import pandas as pd
import numpy as np
import json

jsonpath = os.path.join('purchase_data.json')

with open('purchase_data.json') as json_data:
    heroes_data = json.load(json_data)
    #print(heroes_data)
    
heroes_data_pd = pd.read_json(jsonpath)
heroes_data_pd.head()

player_count = heroes_data_pd["SN"].count()
player_count

player_count_pd = pd.DataFrame([
    {"Total Player Count" : player_count}
    ])
player_count_pd

#Number of Unique Items
unique = heroes_data_pd["Item ID"].unique()

#Average Purchase Price
average_purchase = heroes_data_pd["Price"].mean()

#Total Number of Purchases
total_purchase = heroes_data_pd["Price"].count()

#Total Revenue
total_revenue = heroes_data_pd["Price"].sum()

purchasing_analysis_pd = pd.DataFrame([
    {"Number of Unique Items" : len(unique)},
    {"Average Purchase Price" : average_purchase},
    {"Total Number of Purchases" : total_purchase},
    {"Total Revenue" : total_revenue}
])

purchasing_analysis_pd = pd.DataFrame({
    "Number of Unique Items" : [len(unique)],
    "Average Purchase Price" : average_purchase,
    "Total Number of Purchases" : total_purchase,
    "Total Revenue" : total_revenue
})

purchasing_analysis_pd ["Average Purchase Price"] = purchasing_analysis_pd["Average Purchase Price"].map("${:.2f}".format)
purchasing_analysis_pd ["Total Revenue"] = purchasing_analysis_pd["Total Revenue"].map("${:.2f}".format)

purchasing_analysis_pd

#Percentage and Count of Male Players

gender_count = heroes_data_pd["Gender"].value_counts()
gender_count

#Count of gendered players
male_players = len(heroes_data_pd.loc[heroes_data_pd["Gender"] == "Male", :])
female_players = len(heroes_data_pd.loc[heroes_data_pd["Gender"] == "Female", :])
other_players = len(heroes_data_pd.loc[heroes_data_pd["Gender"] == "Other / Non-Disclosed", :])

#Percentage of gendered players
male_percentage = (male_players / (male_players + female_players + other_players)) * 100
female_percentage = (female_players / (male_players + female_players + other_players)) * 100
other_percentage = (other_players / (male_players + female_players + other_players)) * 100

gender_demographics = pd.DataFrame({
    "Gender" : ["Male", "Female", "Other / Non-Disclosed"],
    "Total Count" : [male_players, female_players, other_players],
    "Percentage of Players" : [male_percentage, female_percentage, other_percentage]
})
gender_demographics = gender_demographics.set_index("Gender")

#format the percentage of players to 2 decimals
gender_demographics["Percentage of Players"] = gender_demographics["Percentage of Players"].map("{:.2f}".format)
gender_demographics

#The below each broken by gender
#Purchase Count
#Average Purchase Price
#Total Purchase Value

male_purchase = heroes_data_pd.loc[heroes_data_pd["Gender"] == "Male", :]
male_purchase_count = len(male_purchase)
male_purchase_total = male_purchase["Price"].sum()
male_purchase_average = male_purchase["Price"].mean()

female_purchase = heroes_data_pd.loc[heroes_data_pd["Gender"] == "Female", :]
female_purchase_count = len(female_purchase)
female_purchase_total = female_purchase["Price"].sum()
female_purchase_average = female_purchase["Price"].mean()

other_purchase = heroes_data_pd.loc[heroes_data_pd["Gender"] == "Other / Non-Disclosed", :]
other_purchase_count = len(other_purchase)
other_purchase_total = other_purchase["Price"].sum()
other_purchase_average = other_purchase["Price"].mean()

purchasing_analysis_gender = pd.DataFrame({
    "Gender" : ["Male", "Female", "Other / Non-Disclosed"],
    "Purchase Count" : [male_purchase_count, female_purchase_count, other_purchase_count],
    "Total Purchase Value" : [male_purchase_total, female_purchase_total, other_purchase_total],
    "Average Purchase Price" : [male_purchase_average, female_purchase_average, other_purchase_average]
})
purchasing_analysis_gender = purchasing_analysis_gender.set_index("Gender")

#format the price and values to include $ and rounded to 2 decimals
purchasing_analysis_gender["Total Purchase Value"] = purchasing_analysis_gender["Total Purchase Value"].map("${:.2f}".format)
purchasing_analysis_gender["Average Purchase Price"] = purchasing_analysis_gender["Average Purchase Price"].map("${:.2f}".format)

purchasing_analysis_gender = purchasing_analysis_gender[["Purchase Count", "Average Purchase Price", "Total Purchase Value"]]
purchasing_analysis_gender


#Normalized Totals
#what is normalized total???

#The below each broken into bins of 4 years (i.e. <10, 10-14, 15-19, etc.)

bins = [0, 9, 14, 19, 24, 29, 34, 39, 50]
group_names = ["<10", "10-14", "15-19", "20-24", "25-29", "30-34", "35-39", ">40"]

heroes_data_pd["Age Demographics"] = pd.cut(heroes_data_pd["Age"], bins, labels=group_names)
#heroes_data_pd

#age_demographics = heroes_data_pd.set_index("Age Demographics")

#separate via age demographics
less_than_10 = heroes_data_pd.loc[heroes_data_pd["Age Demographics"] == "<10", :]
ten_to_14 = heroes_data_pd.loc[heroes_data_pd["Age Demographics"] == "10-14", :]
fifteen_to_19 = heroes_data_pd.loc[heroes_data_pd["Age Demographics"] == "15-19", :]
twenty_to_24 = heroes_data_pd.loc[heroes_data_pd["Age Demographics"] == "20-24", :]
twentyfive_to_29 = heroes_data_pd.loc[heroes_data_pd["Age Demographics"] == "25-29", :]
thirty_to_34 = heroes_data_pd.loc[heroes_data_pd["Age Demographics"] == "30-34", :]
thirtyfive_to_39 = heroes_data_pd.loc[heroes_data_pd["Age Demographics"] == "35-39", :]
greater_than_40 = heroes_data_pd.loc[heroes_data_pd["Age Demographics"] == ">40", :]

#Purchase Count
purchase_count_1 = len(less_than_10)
purchase_count_2 = len(ten_to_14)
purchase_count_3 = len(fifteen_to_19)
purchase_count_4 = len(twenty_to_24)
purchase_count_5 = len(twentyfive_to_29)
purchase_count_6 = len(thirty_to_34)
purchase_count_7 = len(thirtyfive_to_39)
purchase_count_8 = len(greater_than_40)

#Total Purchase value
total_purchase_1 = less_than_10["Price"].sum()
total_purchase_2 = ten_to_14["Price"].sum()
total_purchase_3 = fifteen_to_19["Price"].sum()
total_purchase_4 = twenty_to_24["Price"].sum()
total_purchase_5 = twentyfive_to_29["Price"].sum()
total_purchase_6 = thirty_to_34["Price"].sum()
total_purchase_7 = thirtyfive_to_39["Price"].sum()
total_purchase_8 = greater_than_40["Price"].sum()

#Average Purchase Price
average_purchase_1 = total_purchase_1 / purchase_count_1
average_purchase_2 = total_purchase_2 / purchase_count_2
average_purchase_3 = total_purchase_3 / purchase_count_3
average_purchase_4 = total_purchase_4 / purchase_count_4
average_purchase_5 = total_purchase_5 / purchase_count_5
average_purchase_6 = total_purchase_6 / purchase_count_6
average_purchase_7 = total_purchase_7 / purchase_count_7
average_purchase_8 = total_purchase_8 / purchase_count_8

#Normalized Totals
#what is a normalized total?

age_demographics_df = pd.DataFrame({
    "Age Demographic" : ["<10", "10-14", "15-19", "20-24", "25-29", "30-34", "35-39", ">40"],
    "Purchase Count" : [purchase_count_1, purchase_count_2, purchase_count_3, purchase_count_4, purchase_count_5, purchase_count_6, purchase_count_7, purchase_count_8],
    "Total Purchase Value" : [total_purchase_1, total_purchase_2, total_purchase_3, total_purchase_4, total_purchase_5, total_purchase_6, total_purchase_7, total_purchase_8],
    "Average Purchase Price" : [average_purchase_1, average_purchase_2, average_purchase_3, average_purchase_4, average_purchase_5, average_purchase_6, average_purchase_7, average_purchase_8]
})
age_demographics_df = age_demographics_df.set_index("Age Demographic")

age_demographics_df["Average Purchase Price"] = age_demographics_df["Average Purchase Price"].map("${:.2f}".format)
age_demographics_df["Total Purchase Value"] = age_demographics_df["Total Purchase Value"].map("${:,.2f}".format)
age_demographics_df

#Identify the the top 5 spenders in the game by total purchase value, then list (in a table):

#groupby SN
sn_total = heroes_data_pd.groupby(["SN"])
sn_total.head()

#set variable for Total Purchase Value
total_purchase = sn_total.sum()

#set variable for Average Purchase Price, but rename "Price" to avoid duplicate with column from total_purchase
average_spenders = sn_total.mean()
average_spenders #know "Price"
highest_average_spenders = average_spenders.sort_values("Price", ascending=False)
rename_highest_average_spenders = highest_average_spenders.rename(columns = {"Price" : "Average Purchase Price"})
rename_highest_average_spenders

#set variable for Purchase Count
purchase_count = heroes_data_pd["SN"].value_counts()
purchase_count.head()

#combine variables to create new dataframe
top_spenders_initial = pd.concat([total_purchase, rename_highest_average_spenders, purchase_count], axis = 1)

#Create final table to include all columns of interest
top_spenders = top_spenders_initial[["Price", "Average Purchase Price", "SN"]]
top_spenders

top_spenders = top_spenders.rename(columns={
    "Price" : "Total Purchase Value",
    "SN" : "Purchase Count"
})
top_spenders

#format the column values
top_spenders = top_spenders.sort_values("Total Purchase Value", ascending=False)
top_spenders["Total Purchase Value"] = top_spenders["Total Purchase Value"].map("${:.2f}".format)
top_spenders["Average Purchase Price"] = top_spenders["Average Purchase Price"].map("${:,.2f}".format)

top_spenders.head()

#Identify the 5 most popular items by purchase count, then list (in a table):
#group by multiple columns

#group by Item Name and Item ID
item_groupby = heroes_data_pd.groupby(["Item ID", "Item Name"])
item_groupby.count()
#age, gender, price, sn, age demo

#Purchase Count
item_purchase_count = heroes_data_pd["Item ID"].value_counts()
item_purchase_count = pd.DataFrame({"Purchase Count" : item_purchase_count})
item_purchase_count.head()
# 84, 39, 31, 34, 175

#Total Purchase Value
item_sum = item_groupby.sum()
item_sum
#age, price

#Item Price
#item_price = item_sum["Price"] / item_purchase_count["Purchase Count"]
#item_price


df = pd.concat([item_purchase_count, item_sum], axis = 1, ignore_index = True)
df
#Purchase Count


#item_purchase_count

#popular_items = pd.concat([item_sum, item_price, item_purchase_count], axis = 1)
#popular_items

#Identify the 5 most profitable items by total purchase value, then list (in a table):
#group by multiple columns

#group by Item ID and Item name
item_groupby_id = heroes_data_pd.groupby(["Item ID"])
#age, gender, item name, price, sn, age demographics

#Total Purchase value
profit_total = item_groupby_id.sum()
profit_total = profit_total.sort_values("Price", ascending=False)
#age, price

#Item Price
profit_price = heroes_data_pd["Price"]
profit_price_df = pd.DataFrame(profit_price)
#profit_price_df = profit_price_df.rename(columns={"Price" : "Item Price"})
#price

#Purchase Count
profit_count = item_groupby_id["Item Name"].value_counts()
profit_count_df = pd.DataFrame(profit_count)
profit_count_df
#profit_count = profit_count.sort_values("Item Name", ascending=False)
#profit_count.head()

df_news = pd.merge(profit_total, profit_price_df, on = "Price", how = "outer")
df_news
# = item_groupby.rename(columns = {"Item Name" : "Item Title"})

#profit_count = item_groupby_renamed["Item Title"].value_counts()
#profit_count_sort = profit_count.sort_values("")

#df_new = pd.concat([profit_count, highest_profit_total], axis = 1)
#df_new

#Item Price
#Total Purchase Value
