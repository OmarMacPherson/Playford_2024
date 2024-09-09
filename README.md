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

![Data Analyst Professional](https://github.com/OmarMacPherson/Playford_2024/blob/main/Python%20web.jpeg)

The first step was to automate the collection of job listings from various online job portals. This was achieved using a Python script that performed web scraping on Seek, a popular job portal. The goal was to collect job details like job title, company name, location, industry, employment type, and job responsibilities.

### 1) Importing Libraries:

* **Requests:** To send HTTP requests to the website and fetch the HTML content.
* **BeautifulSoup (from bs4):** To parse the HTML and extract specific information from the job postings.
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

* **Web Scraping:** The HTML content is parsed using BeautifulSoup to extract specific data points from each job post, such as the job title, company name, and responsibilities.

> [!IMPORTANT]
> Before writing the scraping code, it's essential to inspect the HTML structure of the web page you are targeting. By identifying the tags and attributes associated with key data (e.g., job title, company name), you can write more efficient and accurate selectors in your code. This helps improve the performance and reliability of your web scraping process.

```ruby
# Inspect Job Title in Seek HTML
<h1 class="xvu5580 _159rinv4y _7vq8im0 _7vq8iml _1708b944 _7vq8ims _7vq8im21" data-automation="job-detail-title">Operations Manager</h1>
```

```ruby
# scraping code optimised.
def scrape_job_details(job_url):
    job_html = fetch_html(job_url)
    if job_html:
        soup = BeautifulSoup(job_html, 'html.parser')
# Inspect the HTML code for better performance
        title = soup.find('h1', {'data-automation': 'job-detail-title'}).get_text(strip=True)
        responsibilities = ' '.join([li.get_text(strip=True) for li in soup.find_all('li')])
        return {
# Customize your instructions
            'Job Title': title,
            'Responsibilities': responsibilities,
        }

```

* **Saving Data:** Once the job data is collected, it is stored in a pandas DataFrame and exported to an Excel file.

```ruby
df = pd.DataFrame(jobs_data)
df.to_excel('Path_Excel.xlsx', index=False)  # Saving the extracted data
```

* **Rate Limiting:** To avoid triggering anti-bot measures or being blocked by the website, the script includes a delay using the time.sleep() function. This simulates human browsing behavior by introducing pauses between consecutive requests.

```ruby
for job_link in tqdm(job_links[:1000], desc='Scraping job details'):
    job_details = scrape_job_details(job_link)
    if job_details:
        jobs_data.append(job_details)
    time.sleep(x value)  # Delay to mimic human behavior and avoid being blocked
```

This automated data collection method ensured that we had up-to-date job postings to display at the North Adelaide 2024 Career Expo, streamlining the data acquisition process significantly.

* **Output:** Testing Code. 

![Data Analyst Professional](https://github.com/OmarMacPherson/Playford_2024/blob/main/Scrapping%20Success.png)
![Data Analyst Professional](https://github.com/OmarMacPherson/Playford_2024/blob/main/excel%20success.png)

# <p align="center"> Data Cleaning and Transformation with Power Query </p>

![Data Analyst Professional](https://github.com/OmarMacPherson/Playford_2024/blob/main/Power%20Query2.jpeg)

Once the job data was collected, the next crucial step was to clean and transform the dataset to ensure consistency, accuracy, and readiness for further analysis. Power Query was primarily used to handle this process, with Google Sheets as the workspace for quick manual checks and adjustments.

### Key Steps in Data Cleaning:

* **Standardizing Location Names:** Many job listings used different formats for the same location. This inconsistency was addressed by standardizing location names using Power Query.
* **Handling Missing Data:** Several job listings had missing fields (e.g., phone numbers, emails). These were either filled in manually or labeled as "Not Provided" to ensure data integrity.
* **Adding Unique Identifiers:** Unique IDs were assigned to each job listing to make it easier to track and reference specific listings across different stages of the project.
* **Removing Duplicates:** Power Query’s built-in functionality was used to identify and remove any duplicate job entries, ensuring that the dataset was free from redundancy.

![Data Analyst Professional](https://github.com/OmarMacPherson/Playford_2024/blob/main/POWER%20QUERY.png)
![Data Analyst Professional](https://github.com/OmarMacPherson/Playford_2024/blob/main/excel%20cleaned.png)

# <p align="center"> Data Analysis Using SQL </p>

![Data Analyst Professional](https://github.com/OmarMacPherson/Playford_2024/blob/main/SQL_ANALYTICS.jpg)

After the dataset was cleaned and standardized, SQL queries were used to perform analysis and extract valuable insights. These queries were primarily focused on identifying duplicate entries, calculating totals for full-time and part-time roles, and analyzing the distribution of job listings by location. The following SQL techniques were used:

* **Finding Duplicates:** A SQL query was used to detect duplicate job listings based on the combination of job title and company name, ensuring no redundant entries were presented.

```ruby
SELECT `Job Title`, `Company Name`, COUNT(*) as DuplicateCount 
FROM Practice_Playford 
GROUP BY `Job Title`, `Company Name` 
HAVING COUNT(*) > 1;
```

Output:

![Data Analyst Professional](https://github.com/OmarMacPherson/Playford_2024/blob/main/Output%201.png)

* **Counting Total Records:** To get an overview of the dataset, a query was run to count the total number of records (job listings).

```ruby
SELECT COUNT(*) as TotalRecords 
FROM Practice_Playford;
```

Output:

![Data Analyst Professional](https://github.com/OmarMacPherson/Playford_2024/blob/main/Output%202.png)

* **Counting Job Types (Full-Time and Part-Time):** Specific queries were written to count the number of full-time and part-time roles, which helped the team see what kind of employment opportunities were most common.

```ruby
SELECT COUNT(*) as FullTimeRoles 
FROM Practice_Playford 
WHERE `Employment Type` = 'Full-time';
```

```ruby
SELECT COUNT(*) as PartTimeRoles 
FROM Practice_Playford 
WHERE `Employment Type` = 'Part-time';
```

Output:

![Data Analyst Professional](https://github.com/OmarMacPherson/Playford_2024/blob/main/Output%203.png)

* **Location Analysis:** An analysis of the top 5 locations with the highest number of job listings was conducted to guide decision-making for resource allocation at the event.

```ruby
SELECT Location, COUNT(*) as JobCount 
FROM Practice_Playford 
GROUP BY Location 
ORDER BY JobCount DESC 
LIMIT 5;
```

Output:

![Data Analyst Professional](https://github.com/OmarMacPherson/Playford_2024/blob/main/Output%204.png)

# <p align="center"> Automation Using Python </p>

Automation played a key role in ensuring the accuracy and consistency of job data during the Career Expo. Several Python scripts were created to automate different tasks, including verifying job links, checking QR codes on PowerPoint slides, and validating the information in Google Sheets. Below are the key automation techniques used:

### 1) Job Availability Check (Link Verification): 

* **Objective:** Automatically verify whether job links in the dataset were still active or if the job postings had been removed.

* **Libraries Used:**

  * **Pandas:** For handling data in Excel files.
  * **Requests:** For making HTTP requests to job listing websites.
  * **BeautifulSoup:** For parsing the HTML of the job pages.
  * **tqdm:** For adding progress bars to loops.

* **How it Works:** This script takes URLs from the dataset and checks each one to see if the job listing is still active. If a job posting is inactive, the script marks it as "Not Active."

```ruby
df = pd.read_excel('Path_To_Excel.xlsx')

def check_availability(url):
    if not url.startswith(('http://', 'https://')):
        url = 'https://' + url
    try:
        response = requests.get(url)
        if response.status_code == 200:
            soup = BeautifulSoup(response.content, 'html.parser')
            if soup.find(string="This job is no longer advertised"):
                return "Not Active"
            else:
                return "Active"
        else:
            return "Not Active"
    except Exception as e:
        print(f"Error checking URL {url}: {e}")
        return "Not Active"

df['Job Availability'] = df['Original Link'].progress_apply(check_availability)
df.to_excel('Verified_Links.xlsx', index=False)
```

> [!IMPORTANT]
> Including a delay in the requests prevents the IP from being blocked due to excessive requests. This can be done using time.sleep() to mimic human behavior.

Output:

![Data Analyst Professional](https://github.com/OmarMacPherson/Playford_2024/blob/main/Check_Links.png)

### 2) QR Code Validation in PowerPoint: 

* **Objective:** This script checks if the QR codes in the PowerPoint presentation match the correct job listings. It scans each QR code and extracts the corresponding job link, ensuring that the links are accurate.

* **Libraries Used:**

  * **pptx:** For reading and manipulating PowerPoint presentations.
  * **cv2 and pyzbar:** For decoding QR codes in the slides.
  * **pandas:** For organizing the extracted data.
  * **tqdm:** For progress bars during the processing of multiple slides.
 
* **How it Works:** The script loops through each slide in the PowerPoint presentation, extracts the QR code image, decodes it, and checks the link against the expected job listing.

```ruby
ppt_path = 'Path_To_Presentation.pptx'
presentation = Presentation(ppt_path)

def extract_qr_code_link(image):
    decoded_objects = decode(image)
    for obj in decoded_objects:
        return obj.data.decode('utf-8')
    return None

jobs_data = []
for slide in presentation.slides:
    for shape in slide.shapes:
        if shape.shape_type == 13:  
            image_stream = io.BytesIO(shape.image.blob)
            image = Image.open(image_stream)
            qr_code_image = cv2.cvtColor(np.array(image), cv2.COLOR_RGB2BGR)
            qr_link = extract_qr_code_link(qr_code_image)
            jobs_data.append({'QR Link': qr_link})

df = pd.DataFrame(jobs_data)
df.to_excel('QR_Validation.xlsx', index=False)
```

Layout Power Point:

![Data Analyst Professional](https://github.com/OmarMacPherson/Playford_2024/blob/main/LAYOUT.png)

Output:

![Data Analyst Professional](https://github.com/OmarMacPherson/Playford_2024/blob/main/QR%20Matching.png)

### 3) Content Matching Between Links: 

* **Objective:** Verify that the content of the original job link matches the content of the shortened link or QR code to prevent discrepancies.

* **Libraries Used:**

  * **requests and BeautifulSoup:** For fetching and parsing the content of each job listing.
  * **pandas:** For organizing the content and comparing the original and shortened job descriptions
  * **tqdm:** For progress tracking.
 
* **How it Works:** This script fetches the content of both the original and shortened job links and compares the text. If the text matches, it marks it as "Yes," and if there is a discrepancy, it is marked as "No."

```ruby
def fetch_content(url):
    try:
        response = requests.get(url, timeout=20)
        response.encoding = 'utf-8'
        soup = BeautifulSoup(response.content, 'html.parser')
        return ' '.join(soup.stripped_strings)
    except Exception as e:
        print(f"Error fetching content from {url}: {e}")
        return None

df['Original Content'] = df['Original Link'].progress_apply(fetch_content)
df['Short Content'] = df['Short Link'].progress_apply(fetch_content)

df['Content Match'] = df.apply(lambda row: 'Yes' if row['Original Content'] == row['Short Content'] else 'No', axis=1)

df.to_excel('Content_Comparison_Results.xlsx', index=False)
```

Output:

![Data Analyst Professional](https://github.com/OmarMacPherson/Playford_2024/blob/main/Links_Matching.png)

By employing Python for automation, we significantly reduced the manual effort required for checking job availability, ensuring QR code accuracy, and validating the content across different job posting sources. These automations not only improved data reliability but also allowed for real-time updates, ensuring that expo attendees had access to the most accurate job postings.

