# Playford Career Expo Automation System: Enhancing Job Discovery for North Adelaide 2024

![Data Analyst Professional](https://github.com/OmarMacPherson/Playford_2024/blob/main/Omar_Macpherson_Playford.jpg)

This project was developed for the inaugural North Adelaide 2024 Career Expo, organized by the City of Playford. Aimed at streamlining the presentation of job opportunities, the project prepared for an event expected to attract 10,000 attendees, facilitating direct connections between job seekers and employers in the Northern Adelaide area.

# Objective: 

The objective of this project was to enhance event operations by automating the collection, validation, and display of job listings. I developed a system that not only ensured all job information was current and properly formatted but also provided event organizers with a real-time dashboard to monitor key metrics such as job totals and sector distribution. This tool significantly improved decision-making efficiency and event management, ensuring a smooth and effective experience for all participants.

# Data Source

- 📁 The data for the North Adelaide 2024 Career Expo was exclusively collected through a series of Python scripts I developed to automate the scraping of job listings from various online portals. After collection, the data was organized and stored in Google Sheets, allowing for easy access and manipulation. The finalized dataset is included in this repository for reference and transparency.

# Tech Stack Used in this project

This project harnessed a variety of modern data collection, analysis, and visualization technologies to automate and enhance the job board system for the Career Expo. 

![MySQL](https://img.shields.io/badge/mysql-4479A1.svg?style=for-the-badge&logo=mysql&logoColor=white)
![Python](https://img.shields.io/badge/python-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54)
![Microsoft Excel](https://img.shields.io/badge/Microsoft_Excel-217346?style=for-the-badge&logo=microsoft-excel&logoColor=white)
![Tableau](https://img.shields.io/badge/Tableau-E97627?style=for-the-badge&logo=Tableau&logoColor=white)
![Jupyter Notebook](https://img.shields.io/badge/jupyter-%23FA0F00.svg?style=for-the-badge&logo=jupyter&logoColor=white)
![macOS](https://img.shields.io/badge/mac%20os-000000?style=for-the-badge&logo=macos&logoColor=F0F0F0)

# Techniques Used in this Project

This project involved a series of specialized techniques across various stages of data handling and presentation:

### **Data Collection:**

* **Web Scraping with Python:** Automated scripts to extract job listings from online portals.
* **Data Extraction:** Techniques included parsing HTML to retrieve relevant job data such as titles, descriptions, and company names.

### **Data Cleaning and Transformation:**

* **Data Standardization:** Utilized Power Query to standardize location names and other textual data for consistency.
* **Data Enrichment:** Added unique identifiers and relevant metadata to enhance data usability.
 
### **Data Analysis Using SQL:**

* **Duplicate Identification:** SQL queries to detect and remove duplicate entries.
* **Data Aggregation:** Employed SQL functions to summarize data, such as counting jobs by location and categorizing them by industry.

### **Automation Using Python:**

* **Link Validation:** Scripts to check the validity of job URLs periodically and update their status.
* **Automation of Data Updates:** Python code to refresh data in Google Sheets based on new scraping results.
 
### **Dashboard Creation:**

* **Real-Time Data Visualization:** Designed and implemented a Tableau dashboard that displays job metrics dynamically.
* **Interactive Filters:** Setup filters for industry and location to allow managers to customize views according to their needs.

# <p align="center"> Data Collection with Python </p>

The first step was to automate the collection of job listings from various online job portals. This was achieved using a Python script that performed web scraping on Seek, a popular job portal. The goal was to collect job details like job title, company name, location, industry, employment type, and job responsibilities.

### 1) Importing Libraries:

* **Requests:** To send HTTP requests to the website and fetch the HTML content.
* **BeautifulSoup** (from bs4): To parse the HTML and extract specific information from the job postings.
* **Pandas:** To store the extracted data in a structured format (dataframes) and export it as an Excel file.
* **TQDM:** To provide a progress bar, offering real-time feedback on the scraping process.
* **Time:** To introduce delays between requests to mimic human browsing behavior and avoid being blocked by the website.

```ruby
import requests
from bs4 import BeautifulSoup
import pandas as pd
from tqdm import tqdm
import time
```

### 2) Python Script Overview:

* **Fetching HTML:** Using the requests library, the script sends an HTTP request to retrieve the HTML content of job listings from Seek.

```ruby
def fetch_html(url):
    headers = {'User-Agent': 'Mozilla/5.0'}
    try:
        response = requests.get(url, headers=headers)
        response.raise_for_status()  # Raise an exception for HTTP errors
        return response.text
    except requests.RequestException as e:
        print(f"Request error for {url}: {e}")
        return None
```
