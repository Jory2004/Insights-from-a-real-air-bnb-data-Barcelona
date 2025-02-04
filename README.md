# Insights-from-a-real-air-bnb-data-Barcelona
### https://data.insideairbnb.com/spain/catalonia/barcelona/2024-12-12/data/calendar.csv.gz
### I have done all the calculations based on data from Barcelona
![image](https://github.com/user-attachments/assets/4b691c6c-ba42-4110-b73c-b274685e0310)
### After landing on the website you can click on calendar.csv.gz and it will be downloaded!
### If you are using Google Collab then .gz will give you no issues
```python
import pandas as pd
calendar = pd.read_csv('/Users/joryalhumaydani/Downloads/calendar.csv')
```
![image](https://github.com/user-attachments/assets/0bf52e4c-e22a-41dc-a729-cb66886519b6)
### 1. Want to know the number of available and unavailable rooms
```python
calendar.available.value_counts()
```
![image](https://github.com/user-attachments/assets/0bdc7528-d598-4b99-8864-5e493c0eab4a)

### 2. Calculates the percentage of available (t) and unavailable (f) dates
###### Explanation:
###### value_counts(normalize=True) gives proportions.
###### Multiplying by 100 converts the proportions into percentages
```python
availability_percentage = calendar['available'].value_counts(normalize=True) * 100
availability_percentage
```
![image](https://github.com/user-attachments/assets/5dd1a2b9-3c85-4e74-9ced-f2589747c41b)

### 3. Let`s Count the busiest day
```python
busiest_dates = calendar[calendar['available'] == 'f']['date'].value_counts()
print("Busiest Dates:")
print(busiest_dates.head())
```
![image](https://github.com/user-attachments/assets/36211b67-4074-436d-88a6-64ebb75f2d56)
### 4. Plot a bar graph to show availability percentage
```python
import matplotlib.pyplot as plt
availability_percentage.plot(kind='bar', color=['green', 'red'])
plt.title('Availability Percentages')
plt.ylabel('Percentage')
plt.xlabel('Available (t/f)')
plt.show()
```
![image](https://github.com/user-attachments/assets/f74dd5e6-eb1d-43c6-9a98-203e3e8d2a0c)
### 5. Plot the busiest day
```python
busiest_dates.head(10).plot(kind='bar', color='orange')
plt.title('Top 10 Busiest Dates')
plt.ylabel('Number of Listings Unavailable')
plt.xlabel('Date')
plt.show()
```
![image](https://github.com/user-attachments/assets/4de94d19-1415-4b31-a4c2-ef8835c85169)

## Download listings dataset of Barcelona from https://data.insideairbnb.com/thailand/central-thailand/bangkok/2024-12-25/visualisations/listings.csv
``` python
import pandas as pd
listings = pd.read_csv('/Users/joryalhumaydani/Downloads/listings.csv')
listings
```
![image](https://github.com/user-attachments/assets/0f277a47-9abd-45eb-9916-49ed7ecace76)

```python
listings.columns
```
![image](https://github.com/user-attachments/assets/3894ca65-54c7-4889-ae64-04ae19fa933b)

### Room type and price is given seperately
```python
import matplotlib.pyplot as plt
price_by_room = listings.groupby('room_type')['price'].mean()
print(price_by_room)

# Plot price by room type
price_by_room.plot(kind='bar', color='cyan')
plt.title('Average Price by Room Type')
plt.ylabel('Average Price')
plt.xlabel('Room Type')

plt.show()
```
![image](https://github.com/user-attachments/assets/e6d4a8a3-bd00-4cb7-b42e-9347fab0d715)
### Which are the top 10 neighborhoods with the most listings?

```python
neighborhood_counts = listings['neighbourhood'].value_counts().head(10)
print("Top 10 Neighborhoods by Listings:")
print(neighborhood_counts)

# Plot neighborhoods with most listings
neighborhood_counts.plot(kind='bar', color='lightcoral')
plt.title('Top 10 Neighborhoods by Listings')
plt.ylabel('Number of Listings')
plt.xlabel('Neighborhood')
plt.xticks(rotation=90)
plt.show()
```
![image](https://github.com/user-attachments/assets/1d0e7108-9c87-4d1d-b56b-e6172b2bae75)
### Geographical Distribution of Listings (Price Colored)
```python
import matplotlib.pyplot as plt
import seaborn as sns
plt.figure(figsize=(10, 6))
sns.scatterplot(data=listings, x='longitude', y='latitude', hue='price', palette='viridis', size='price', sizes=(10, 200))
plt.title('Geographical Distribution of Listings (Price Colored)')
plt.xlabel('Longitude')
plt.ylabel('Latitude')
plt.show()
```
![image](https://github.com/user-attachments/assets/3e180f74-f068-4f38-8335-4a6d6be9db73)

### Let us see the listings on a real map

###### Hotter Areas (Red/Yellow):  High Density: The areas that appear in red or yellow (the "hot" colors) indicate higher density or concentration of listings. This means there are more listings in these areas. Popular Locations: These regions might be more popular or in high demand. It could be near tourist attractions, popular neighborhoods, or central areas in Barcelona where people tend to stay more often. Colder Areas (Green/Blue):  

###### Low Density: Areas with blue or green (the "cold" colors) indicate a lower concentration of listings. These regions have fewer listings available.Less Popular Locations: These areas might be less popular or further from key attractions. If you're looking at pricing or other factors, lower density could imply less competition in these regions, which might indicate more affordable areas or less tourist traffic. Density Patterns:  

###### Clustered Areas: If you notice clusters of heatmap intensity, they represent hotspots. These might correspond to high-traffic areas like resorts, beaches, or urban centers. Spread-Out Listings: If the heatmap shows a more uniform distribution, it could suggest that listings are more evenly spread across the region, which may reflect a more balanced demand for rentals across different areas of Barcelona.
```python
import folium
from folium.plugins import HeatMap
import pandas as pd

# Example dataset with latitude, longitude, and price for Barcelona listings
barcelona_data = listings[['latitude', 'longitude', 'price']]  # Modify as needed

# Create a base map centered around Barcelona
m = folium.Map(location=[41.3874, 2.1686], zoom_start=12)

# Prepare the data for the heatmap
heat_data = [[row['latitude'], row['longitude']] for index, row in barcelona_data.iterrows()]

# Add the heatmap to the map
HeatMap(heat_data).add_to(m)

# Save the map as an HTML file to view in a browser
m.save('barcelona_heatmap.html')

# If you're using Jupyter Notebook, you can display the map directly in the notebook:
m
```
![image](https://github.com/user-attachments/assets/8d5836fd-f9e2-4cf0-8761-0a2c4d70ff07)

### How do I find location for my city?
#### Type your city name on google maps
#### Click on What`s here
![image](https://github.com/user-attachments/assets/bec52846-3e34-49c3-8dd4-065514048449)


#### You will find latitude and longitude at the bottom of screen
![image](https://github.com/user-attachments/assets/4520e4a6-0024-4ced-b2c1-8c100159f819)

### 6. What are the most expensive neighborhoods in Barcelona?
##### This will help identify the areas with the highest average rental prices.
```python
# Calculate the average price per neighborhood
expensive_neighborhoods = listings.groupby('neighbourhood')['price'].mean().sort_values(ascending=False).head(10)

# Print the top 10 most expensive neighborhoods
print("Top 10 Most Expensive Neighborhoods:")
print(expensive_neighborhoods)

# Plot the data
expensive_neighborhoods.plot(kind='bar', color='purple')
plt.title('Top 10 Most Expensive Neighborhoods')
plt.ylabel('Average Price (€)')
plt.xlabel('Neighborhood')
plt.xticks(rotation=90)
plt.show()
```
![image](https://github.com/user-attachments/assets/c56095c7-5ce0-4cae-88bc-307e5dfeab93)

### 7. What is the relationship between price and number of reviews?
##### This will help analyze whether higher-priced listings tend to receive more or fewer reviews.
```python
import seaborn as sns

# Scatter plot of price vs. number of reviews
plt.figure(figsize=(10,6))
sns.scatterplot(data=listings, x='price', y='number_of_reviews', alpha=0.5)
plt.title('Price vs. Number of Reviews')
plt.xlabel('Price (€)')
plt.ylabel('Number of Reviews')
plt.xlim(0, listings['price'].quantile(0.99))  # Exclude extreme outliers
plt.ylim(0, listings['number_of_reviews'].quantile(0.99))  # Exclude extreme outliers
plt.show()
```
![image](https://github.com/user-attachments/assets/9898cf27-be69-4a79-a047-4512297da62d)


