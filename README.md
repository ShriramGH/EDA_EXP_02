# Exp - 2 Netflix Shows & Movies

### Name : SHRIRAM S
### Reg No : 212222240098

## Aim

To analyze Netflix dataset and compare movies vs TV shows, top producing countries, and release year trends.

## Procedure / Algorithm

```
1) Load dataset (netflix_titles.csv).
2) Count movies vs TV shows. 
3) Group by country → top contributors.
4) Create pivot table (release year vs type).
5) Visualize with bar & line charts.

```
## Program

```py
import pandas as pd
```

### 1. Load dataset directly from GitHub
```PY
url = "https://raw.githubusercontent.com/allenkong221/netflix-titles-dataset/main/netflix_titles.csv"
df = pd.read_csv(url)

# Basic inspection
print("Shape:", df.shape)
print("Columns:", df.columns, "\n")
print("Type counts:\n", df['type'].value_counts(), "\n")
```

### 2. Clean 'date_added' and extract year/month
```py
df['date_added'] = df['date_added'].str.strip() # remove extra spaces
df['date_added'] = pd.to_datetime(df['date_added'], errors='coerce') # invalid -> NaT
df['year_added'] = df['date_added'].dt.year
df['month_added'] = df['date_added'].dt.month_name()
```

### 3. Movies vs TV Shows
```py
count_by_type = df.groupby('type')['title'].count()
print("Count by Type:\n", count_by_type, "\n")

```

### 4. Country vs Type Pivot Table
```py
pivot_country_type = df.pivot_table(
index='country',
columns='type',
values='title',
aggfunc='count',
fill_value=0
)

# Add "Total" column and find max country
pivot_country_type['Total'] = pivot_country_type.sum(axis=1)
max_country = pivot_country_type['Total'].idxmax() # country with most titles
max_count = pivot_country_type['Total'].max() # number of titles
print("Pivot Table (Country vs Type):\n", pivot_country_type.head(), "\n")
print(f"Largest Overall: {max_country} with {max_count} titles\n")
```

### 5. Top 5 Directors
```py
top_directors = df['director'].value_counts().head(5)
print("Top 5 Directors:\n", top_directors, "\n")
```

### 6. Yearly Trend of Additions (Movies vs TV Shows)
```py
trend = df.groupby(['year_added', 'type']).size().unstack(fill_value=0)
print("Yearly Trend by Type:\n", trend.head(), "\n")
```

### 7. Expand Genres
```py
df_genre = (
df[['show_id','listed_in']]
.dropna()
.assign(listed_in=df['listed_in'].str.split(', '))
.explode('listed_in')
)

# Merge expanded genres back
df_expanded = df.merge(df_genre, on='show_id', how='left')

# Check merged columns
print("Columns after merge:\n", df_expanded.columns)

# Show sample of expanded genres
print("\nExpanded Genre Sample:\n",
df_expanded[['title','listed_in_y']].head())

# Top 5 Genres (extra useful for insights)
top_genres = df_expanded['listed_in_y'].value_counts().head(5)
print("\nTop 5 Genres:\n", top_genres, "\n")

```

## Ouptut

### Basic inspection 

<img width="1342" height="85" alt="image" src="https://github.com/user-attachments/assets/a1b0d055-9cdb-4c21-873e-83dfe95ad9b8" />



<img width="982" height="138" alt="image" src="https://github.com/user-attachments/assets/50727793-4d59-4c83-9d41-c85dbcad3f43" />



<img width="1068" height="153" alt="image" src="https://github.com/user-attachments/assets/ac03c654-6bd9-467b-962f-ddb249f34698" />

### Movies vs TV Shows


<img width="1106" height="154" alt="image" src="https://github.com/user-attachments/assets/48cf8c87-6641-4101-a4dc-ccb3f5412e23" />


### Add "Total" column and find max country

<img width="1275" height="260" alt="image" src="https://github.com/user-attachments/assets/f9801f82-0c3b-4959-982b-c89373120523" />

### Yearly Trend of Additions (Movies vs TV Shows)

<img width="1180" height="219" alt="image" src="https://github.com/user-attachments/assets/01fe0254-90c3-4f3b-b73e-8fb9975ed6d8" />

### Top 5 Directors

<img width="1104" height="213" alt="image" src="https://github.com/user-attachments/assets/57c31d32-243b-465d-b595-fb6e6f478e05" />

### Check merged columns

<img width="1268" height="134" alt="image" src="https://github.com/user-attachments/assets/8040ff98-ba01-48c8-8626-1c64fbad3beb" />

### Show sample of expanded genres

<img width="1247" height="195" alt="image" src="https://github.com/user-attachments/assets/617a840f-f9ff-477f-9897-43d3dd206678" />

### Top 5 Genres (extra useful for insights)

<img width="1236" height="226" alt="image" src="https://github.com/user-attachments/assets/d8cabe30-84bd-4029-9b30-3aa3a948d5c5" />






## Result
Helps Netflix in content planning & investments.
