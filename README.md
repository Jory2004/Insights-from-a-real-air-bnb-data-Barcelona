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




