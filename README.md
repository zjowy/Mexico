# Mexico
Python code for insight on airbwb booking of Mexico I am using calendar and listing CSV files.
I have done all the calculation based on data from Mexico https://data.insideairbnb.com/mexico/df/mexico-city/2024-09-25/data/listings.csv.gz 
https://data.insideairbnb.com/mexico/df/mexico-city/2024-09-25/data/calendar.csv.gz

```python
import pandas as pd
calendar = pd.read_csv('calendar.csv')
```
# Questions that you might want to address about the given data :sparkles: :memo:

# 1-Want to know the number of available and unavailable rooms

```python
calendar.available.value_counts()
```
available
t    5900401
f    3802131

(F)alse meanes not avilable , (T)rue meanes avilable

# 2-Purpose: Calculates the percentage of available (t) and unavailable (f) dates.
Explanation: value_counts(normalize=True) gives proportions. Multiplying by 100 converts the proportions into percentages

```python
availability_percentage = calendar['available'].value_counts(normalize=True) * 100
availability_percentage
```

# 3 Let`s Count the busiest day! :pencil2: 
Hint: We will be counting the most unavailable days (given by f)

```python
busiest_dates = calendar[calendar['available'] == 'f']['date'].value_counts()
print("Busiest Dates:")
print(busiest_dates.head())
```

# 4-Plot a bar graph to show availability percentage

```python
import matplotlib.pyplot as plt
availability_percentage.plot(kind='bar', color=['purple', 'thistle'])
plt.title('Availability Percentages')
plt.ylabel('Percentage')
plt.xlabel('Available (t/f)')
plt.show()
```
![image](https://github.com/user-attachments/assets/208c7263-cb24-44f1-ac4b-5025c944632f)

# 5-Plot the busiest day
```python
busiest_dates.head(10).plot(kind='bar', color='indigo')
plt.title('Top 10 Busiest Dates')
plt.ylabel('Number of Listings Unavailable')
plt.xlabel('Date')
plt.show()
```
![image](https://github.com/user-attachments/assets/f02cc387-e93f-481a-bcb3-d443437f385f)

# Download listings dataset of Hawaii from https://data.insideairbnb.com/united-states/hi/hawaii/2024-09-13/data/listings.csv.gz :page_facing_up: :page_facing_up: :page_facing_up:

```python
import pandas as pd
listings = pd.read_csv('listings.csv')
```
```python
listings.columns
```
![image](https://github.com/user-attachments/assets/08c56045-5919-435e-b683-0c5533fcd29b)

# 1-Room type and price is given seperately
Let`s Combine and visualize them
```python
import matplotlib.pyplot as plt
price_by_room = listings.groupby('room_type')['price'].mean()
print(price_by_room)

# Plot price by room type
price_by_room.plot(kind='bar', color='purple')
plt.title('Average Price by Room Type')
plt.ylabel('Average Price')
plt.xlabel('Room Type')

plt.show()
```

![image](https://github.com/user-attachments/assets/6b8b704a-1169-4a09-84f5-32724f15ae16)

# 2-Which are the top 10 neighborhoods with the most listings? :boom:

```python
neighborhood_counts = listings['neighbourhood'].value_counts().head(10)
print("Top 10 Neighborhoods by Listings:")
print(neighborhood_counts)
Plot neighborhoods with most listings
neighborhood_counts.plot(kind='bar', color='lightcoral')
plt.title('Top 10 Neighborhoods by Listings')
plt.ylabel('Number of Listings')
plt.xlabel('Neighborhood')
plt.xticks(rotation=90)
plt.show()
```
![image](https://github.com/user-attachments/assets/749c9ee8-404c-4d8f-8d9e-4473a36fa4b8)

# 3-Geographical Distribution of Listings (Price Colored)
```python
import matplotlib.pyplot as plt
import seaborn as sns
```
```python
plt.figure(figsize=(10, 6))
sns.scatterplot(data=listings, x='longitude', y='latitude', hue='price', palette='viridis', size='price', sizes=(10, 200))
plt.title('Geographical Distribution of Listings (Price Colored)')
plt.xlabel('Longitude')
plt.ylabel('Latitude')
plt.show()
```
![image](https://github.com/user-attachments/assets/bfc924e0-a28c-4b7d-b654-fdd3e9d7e086)


# Let us see the listings on a real map :seedling:
```python
import folium
from folium.plugins import HeatMap
import pandas as pd


mexico_data = listings[['latitude', 'longitude', 'price']]  # Example, you may add more columns

# Create a base map centered around mexico 
m = folium.Map(location=[20.7967, -156.3319], zoom_start=10)

# Prepare the data for the heatmap
heat_data = [[row['latitude'], row['longitude']] for index, row in mexico_data.iterrows()]

# Add the heatmap to the map
HeatMap(heat_data).add_to(m)

# Save the map as an HTML file to view in a browser
m.save('mexico_heatmap.html')

# If you're using Jupyter Notebook, you can display the map directly in the notebook:
m
```
![image](https://github.com/user-attachments/assets/055b81df-603b-4a8d-87bf-7cac20c404b8)


