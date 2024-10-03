```python
pip install craigslistscraper
```


```python
# import necessary libraries
import craigslistscraper as cs
import json
import pandas as pd
import time

```


```python
# Define the search.
search = cs.Search(
    query = "nvidia | amd",
    city = "atlanta",
    category = "syp"
)
```


```python
#Setting up some variables to name our final Excel file. Feel free to adjust these.

base_path = r'C:\Users\TCF GIS\Documents\craigslistads'
TodaysDate = time.strftime("%b-%d-%Y-%H-%M-%S")

df.to_excel(base_path + "\\" + TodaysDate +".xlsx", index=False)
```


```python
# Create an empty pandas dataframe. We'll dump our data in here later.
df = pd.DataFrame()
```


```python
# Fetch the html from the server. The status will let you know if the website is down, or other problems. 
status = search.fetch()
if status != 200:
    raise Exception(f"Unable to fetch search with status <{status}>.")

```


```python
# We'll loop through the ads and add them to the pandas dataframe one by one.
for ad in search.ads:
    # Fetch additional information about each ad. Check the status again.
    status = ad.fetch()
    if status != 200:
        print(f"Unable to fetch ad '{ad.title}' with status <{status}>.")
        continue

    # There is a to_dict() method for convenience. 
    data = ad.to_dict()
    
    # json.dumps is merely for pretty printing. 
    print(json.dumps(data, indent = 4))

    # This is where the ads finally get added to the dataframe.
    df = pd.concat([df, pd.DataFrame([data])], ignore_index=True)
```


```python
# Converting the dataframe to an Excel file and saving it to your specified folder.
# Warning: this will overwrite your file each time you run this.
# 
df.to_excel(base_path + "\\" + TodaysDate +".xlsx", index=False)
```


```python
print(df)
```
