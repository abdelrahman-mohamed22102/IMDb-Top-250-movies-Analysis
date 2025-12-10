# IMDb Top 250 Movies Analysis

This project presents an interactive Power BI dashboard that visualizes and analyzes the Top 250 movies listed on IMDb based on user ratings.

<img width="1440" height="425" alt="Dashboard Preview" src="https://github.com/user-attachments/assets/e20e8b47-2c12-4b3b-90d0-fb6830d2dba1" />

![License](https://img.shields.io/badge/license-MIT-green)
![Python](https://img.shields.io/badge/python-3.14+-blue)
![Python re](https://img.shields.io/badge/Python%20re-3776AB?logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-150458?logo=pandas&logoColor=white)
![Power BI](https://img.shields.io/badge/Power%20BI-FFCB00?logo=powerbi&logoColor=black)
![Power Query](https://img.shields.io/badge/Power%20Query-217346?logo=microsoftexcel&logoColor=white)

---

## Data Source

The dataset was collected directly from the IMDb website:  
[IMDb Top 250 Movies](https://www.imdb.com/chart/top/)

---

## Data Preview

<table>
  <tr>
    <th>Raw Data Sample (Before)</th>
    <th>Preprocessed Data (After)</th>
  </tr>
  <tr>
    <td>
      <img width="273" height="246" alt="Raw Data" src="https://github.com/user-attachments/assets/2c009fbb-3e1d-4b76-8301-20a47487127b" />
    </td>
    <td>
      <img width="322" height="225" alt="Preprocessed Data" src="https://github.com/user-attachments/assets/0ea177f6-784a-493b-8b5e-dc36aadf6bd3" />
    </td>
  </tr>
</table>

---

## Data Quality Issues Identified

During initial data profiling, the following issues were found:

1. **Non-descriptive Column Names**  
   Several columns were labeled generically and did not describe their contents.

2. **Irrelevant Prefix in ID Column**  
   The "ID" column contained a "#" prefix that needed to be removed.

3. **Unsupported Number Format**  
   The “Number of Ratings” column included values such as “3.2M”, which required conversion to numeric format.

4. **Unsupported Duration Format**  
   The “Duration” column used the format “2h 22m”, requiring standardization into numeric minutes.

---

## Applied Transformations

### Python Transformations (Duration & Number of Ratings)

```python
import pandas as pd
import re 

df = dataset.copy()

def to_minute(x):
    h = re.search(r'(\d+)\s*h', x)
    m = re.search(r'(\d+)\s*m', x)
    
    hours = int(h.group(1)) if h else 0
    minutes = int(m.group(1)) if m else 0
    return str(hours) + ':' + str(minutes)

def to_numeric(x):
    m = re.search(r'(\d+(?:\.\d+)?)\s*([MK])', str(x), re.IGNORECASE)
    if not m:
        return None
    
    number = float(m.group(1))
    suffix = m.group(2).upper()

    if suffix == 'K':
        return int(number * 1000)
    elif suffix == 'M':
        return int(number * 1_000_000)

df['Duration'] = df['Duration'].apply(to_minute)
df['Number Of Ratings'] = df['Number Of Ratings'].apply(to_numeric)
```
## Dashboard Screenshot
<img width="1281" height="720" alt="image" src="https://github.com/user-attachments/assets/674bf26c-bdca-4ce2-b8a7-289c8155d141" />

### Dashboard Features
- A new feature named **Decade** was created from the movie release year.  
  This feature enhances trend detection, comparative analysis, and visual grouping across different time periods.

- dashboard supports **interactive drill-down**, allowing users to navigate from **decade-level insights** into **year-by-year trends** for deeper analysis.
