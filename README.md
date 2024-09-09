# Playford Career Expo Automation System: Enhancing Job Discovery for North Adelaide 2024

![Data Analyst Professional](https://github.com/OmarMacPherson/Playford_2024/blob/main/Landscape_Playford.png)

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

![Data Analyst Professional](https://github.com/OmarMacPherson/Playford_2024/blob/main/Python_Automation.jpg)

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

The use of Python automation streamlined key tasks, including verifying job availability, validating QR codes, and matching content across job links. This ensured data accuracy, reduced manual effort, and provided real-time updates, maintaining the reliability of job listings throughout the event preparation process.


# <p align="center"> Real-Time Dashboard Creation </p>

![Data Analyst Professional](https://github.com/OmarMacPherson/Playford_2024/blob/main/TABLEAU.jpg)

The dashboard was built in Tableau to provide real-time insights to the managers at the City of Playford regarding the jobs collected for the 2024 Northern Adelaide Jobs and Careers Expo. The main objective of the dashboard was to allow the managers to monitor the number of job listings available by location, employment type, and industry, facilitating quick decision-making during the expo preparation.

* **Features of the Dashboard:**

  * **Total Jobs Collected:** A summary metric that shows the total number of job postings collected.
  * **Top 5 Job Locations:** A bar chart highlighting the five areas with the most job opportunities, allowing managers to see regional demand.
  * **Jobs by Employment Type:** A bar chart summarizing the job types, such as full-time, part-time, contract, and apprenticeships, to help identify the balance of roles available.
  * **Filter by Industry and Location:** A real-time interactive filter allowing the managers to narrow down the job listings by specific industries or locations.

* **Real Time Data Sync:**

The dashboard was linked to the Google Sheets that stored all job data. Any updates in the Google Sheet were reflected in real-time on the Tableau dashboard, ensuring the most current information was always available.

# Interactive Dashboard Access

Explore the interactive dashboard and you can get your own insights and answers for your own investigative questions. To access to the Dashboard [CLICK HERE](https://public.tableau.com/app/profile/omar.alan.mac.pherson/viz/Playford/Dashboard1) or in the Dashboard Picture.

[![FLipkart](https://github.com/OmarMacPherson/Playford_2024/blob/main/Dashboard%20Image.png)](https://public.tableau.com/app/profile/omar.alan.mac.pherson/viz/Playford/Dashboard1)

# <p align="center"> Challenges Encountered </p>

During the development of this project, several challenges were encountered:

* **Real-time Data Updates:** Ensuring that the data collected from job portals remained accurate and up-to-date required creating scripts that frequently checked the status of each job listing. Automation helped solve this, but it required continuous testing to avoid breaking changes in the source websites.
* **Web Scraping Limitations:** The need to avoid detection and ensure reliable performance of web scraping led to the implementation of rate limiting (delays between requests), which slowed down the process but prevented IP blocking.
* **Data Consistency:** Ensuring the job titles, descriptions, and other details were consistent and correctly formatted for display across platforms (Google Sheets, Tableau, and PowerPoint) involved significant data cleaning and transformation efforts.
* **QR Code & link Matching:** Validating that the QR codes and shortened links correctly matched the job listings was a delicate task, and we built scripts to automate this process and minimize manual intervention.

# <p align="center"> Results </p>

The City of Playford’s 2024 Career Expo Job Board Automation System successfully achieved the following results:

* **Jobs Collected:** The system gathered and displayed job listings from a wide range of industries in Northern Adelaide.
* **Real-Time Insights:** The Tableau Dashboard provided managers at the City of Playford with live updates, allowing for efficient decision-making regarding job availability and sectors to focus on.
* **Automation Benefits:** By automating job link checks, QR code verification, and data synchronization, the system reduced manual workload, ensuring up-to-date information was always available for both the expo attendees and managers.

# <p align="center"> Invitation for Use </p>

> [!IMPORTANT]
> This project is open for use, and all the code shared here can be applied to similar job board or data management tasks in other contexts. If you are working on a similar project or require further assistance with implementing this system or any of its components, feel free to reach out.
>
> You are welcome to explore and use the code in this repository for your projects, whether it's data collection, automation, or dashboard creation. If you need any clarification, guidance, or help with customization, I am available to assist. Feel free to contact me for any questions or further details.

> [!IMPORTANT]
> For any inquiries or further discussions on this project, please feel free to connect via [LinkedIn](www.linkedin.com/in/omaralan). or send an [E-mail](omar.macpherson@outlook.com).

# <p align="center"> Conclusion and Future Directions </p>

This project showcased the value of automating data collection, validation, and analysis to optimize operations at a large-scale event like the **2024 Career Expo.** The system not only provided real-time insights for decision-making but also streamlined repetitive tasks, enhancing overall efficiency.

For future developments, several data-driven enhancements could be implemented:

* **Advanced Data Analytics:** By integrating predictive analytics, we could forecast job trends and demand across industries, offering deeper insights to event organizers for better planning and resource allocation.
* **Expanded Data Sources:** Incorporating additional job portals and automating the integration of these sources would increase the depth and variety of job listings available to attendees.
* **Enhanced Reporting:** Introducing automated reporting with more dynamic visualizations and real-time updates for different stakeholders would improve tracking of event success metrics, such as job placement rates or sector performance.

These improvements would not only continue to drive operational efficiency but also offer actionable insights, making data-driven decisions more accessible to key stakeholders.

# Author

This Project has been designed and done by **Omar Mac Pherson**.

[<img src='https://cdn.jsdelivr.net/npm/simple-icons@3.0.1/icons/github.svg' alt='github' height='40'>](https://github.com/OmarMacPherson)  [<img src='https://cdn.jsdelivr.net/npm/simple-icons@3.0.1/icons/linkedin.svg' alt='linkedin' height='40'>](https://www.linkedin.com/in/omaralan/)  [<img src='https://cdn.jsdelivr.net/npm/simple-icons@3.0.1/icons/tableau.svg' alt='tableau' height='40'>](https://public.tableau.com/app/profile/omar.alan.mac.pherson/vizzes)  

# Credits

### Format & Style

    * Github Docs (https://docs.github.com/)
    * Dev (https://dev.to/github)
    * Free Code Camp (https://www.freecodecamp.org/)

### Other

    * Icons (https://github.com/anuraghazra/github-readme-stats)
    * Icons (https://www.flaticon.com/free-icons/github)
    * Design Responsive (https://arturssmirnovs.github.io/)
    * HTML (https://talk.jekyllrb.com/t/how-to-add-a-image-with-links-in-markdown/5915)
    * Markdown (https://codinhood.com/nano/git/center-images-text-github-readme/)
    * Markdown 2 (https://github.com/orgs/community/discussions/16925)
    * Badges (https://github.com/Ileriayo/markdown-badges)
    * Icons (https://cdn.jsdelivr.net/npm/simple-icons@3.0.1/icons/)
