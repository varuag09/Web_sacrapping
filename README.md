# Web_scrapping
# 🛒 Amazon Smartphone Price Tracker

This project is a **Web Scraper + Price Trend Visualizer** built using **Selenium**, **Pandas**, and **Plotly**.  
It tracks the price of a product from **Amazon India** and visualizes the price trend over time.

---

## ⚙️ Features
- Automated price scraping using Selenium WebDriver
- Saves data with timestamps in a CSV file
- Visualizes price trend using interactive Plotly charts
- Supports repeated tracking for price history analysis

---

## 🧩 Tech Stack
- Python 
- Selenium (Web Scraping)  
- Pandas (Data Handling)  
- Plotly (Visualization)  

---

## 📝 Sample Code Snippets

### 📦 Web Scraping with Selenium

```python
from selenium import webdriver
from selenium.webdriver.chrome.service import Service
from selenium.webdriver.common.by import By
import pandas as pd
from datetime import datetime
import time
import os

# Set ChromeDriver Path
chrome_driver_path = r"C:\path\to\chromedriver.exe"

# Amazon Product URL
url = 'https://www.amazon.in/your-product-url'

# Initialize Chrome in Headless Mode
service = Service(chrome_driver_path)
options = webdriver.ChromeOptions()
options.add_argument("--headless")
options.add_argument('--disable-blink-features=AutomationControlled')
driver = webdriver.Chrome(service=service, options=options)

# Visit Page and Scrape
driver.get(url)
time.sleep(5)

try:
    title = driver.find_element(By.ID, 'productTitle').text.strip()
    price = driver.find_element(By.CLASS_NAME, 'a-offscreen').get_attribute('innerText').strip()

    data = {
        "Date": [datetime.now().strftime("%Y-%m-%d %H:%M:%S")],
        "Title": [title],
        "Price": [price]
    }

    df = pd.DataFrame(data)
    file_exists = os.path.isfile('price_history.csv')
    df.to_csv("price_history.csv", mode='a', header=not file_exists, index=False)

    print("✅ Scraped Successfully:", df)

except Exception as e:
    print("❌ Error during scraping:", e)

finally:
    driver.quit()
```

---

### 📊 Price Trend Visualization with Plotly

```python
import pandas as pd
import plotly.express as px

df = pd.read_csv("price_history.csv")

# Convert Date to datetime and clean price
df["Date"] = pd.to_datetime(df["Date"])
df["Clean_Price"] = df["Price"].str.replace(r'[^\d.]', '', regex=True).astype(float)

# Sort & Aggregate Data
df = df.sort_values("Date")
df = df.groupby("Date", as_index=False)["Clean_Price"].mean()

# Plotting the Price Trend
fig = px.line(df, x="Date", y="Clean_Price", markers=True,
              title="Amazon Smartphone Price Tracker",
              labels={"Date": "Date", "Clean_Price": "Price (₹)"})

fig.update_layout(xaxis_tickangle=45, xaxis_showgrid=True, yaxis_showgrid=True)
fig.show()
```
![Price Tracker](https://github.com/varuag09/Web_sacrapping/blob/main/Price_tracker.png)
---

## 🚀 How to Use
1. Install Python libraries:
   ```bash
   pip install selenium pandas plotly
   ```
2. Download and set up **ChromeDriver**  
3. Set your product URL and ChromeDriver path in the script  
4. Run the script periodically (manual/automated) to track price  
5. Use the visualization code to see price trends  

---


## 👤 Author
- [Gaurav Kumar]
