# README: Web Scraping and Data Analysis with Scrapy

This guide walks you through setting up a virtual environment, creating a Scrapy project, running the web crawler to collect fire incident reports, and analyzing the data. Follow the steps below to get started.

## Configurations and Installations

### 1. Create a Virtual Environment
Create a virtual environment to manage your project's dependencies.
```
python -m venv venv
```

### 2. Activate the Virtual Environment
Activate the virtual environment to use the installed packages.

- On Windows:
```
.\venv\Scripts\activate
```

- On Unix or MacOS:
```
source venv/bin/activate
```

### 3. Upgrade pip to the latest version.
```
python -m pip install --upgrade pip
```

### 4. Install Required Libraries
Install the necessary libraries specified in the requirements.txt file.

```
pip install -r requirements.txt
```
If you encounter PowerShell permission issues on Windows, use:
```
.\venv\Scripts\python.exe -m pip install -r requirements.txt
```


## Creating a Scrapy Project
Scrapy helps you create a structured web scraping project.

### 5. Start Scrapy Project
Start a new Scrapy project with the name fire_incidents.
```
scrapy startproject fire_incidents
```
If you encounter PowerShell permission issues on Windows, use:
```
.\venv\Scripts\python.exe -m scrapy startproject fire_incidents
```



## Creating the Crawler Class
The crawler retrieves information from the website using XPath expressions.
- XPath is an XML Path Language used to navigate and select nodes from XML documents.
- Testing XPath on the fly, run the following command to open the scrapy shell:

### Explanation of the Logic
scrapy shell
- Fetch the page
>>> r = scrapy.Request(url="https://fireandemergency.nz/incidents-and-news/incident-reports/")
>>> fetch(r)
- Get the h3 Central
>>> on_central_div = response.xpath("//div[@class='incidentreport__region'][h3[text()='Central']]")
>>> on_central_div
- Get one of the 1st list element
>>> li = region_central_div.xpath(".//ul[@class='incidentreport__region__list']/li/a")[0]
- extract the href from the 1st list element
>>> li.xpath("@href").get() 
- extract the text from the 1st list element
>>> li.xpath("text()").get().strip()

### 6. Create a Spider
Create a spider to crawl the web page.

```
cd fire_incidents
scrapy genspider incident_reports fireandemergency.nz/incidents-and-news/incident-reports
```

#### Check the robots.txt of the website to ensure you are allowed to crawl it:
- https://fireandemergency.nz/robots.txt




## Running the Crawler
The spider class will crawl the web, extract fire incident report data, and save it to a CSV file.
The command syntax used to execute the spider to crawl the webpage is
- scrapy crawl <spider_name> <url_without_the_https_and_forward_slash> [-o <file_name>]

### 7. Run Combine
The run combine first executes the Main_1_ScrapyRunner.py, followed by Main_2_AnalyzeData.py

```
python Main_3_Combine.py
```
### 7b. Crawl the Webpage
Run the spider to extract data and save it into a CSV file in the data folder.

```
cd path/to/your/root/directory
python Main_1_ScrapyRunner.py
```

### 7c. Perform the Analysis
This script retrieves and analysis the latest day_of_week_incident_reports CSV file to answer the following questions:

- How many incidents has the Stratford Brigade responded to in the last 7 days?
- How many medical incidents have been reported in the Central Region in the last 7 days?
- Where were the medical incidents reported in the last 7 days?
Run the analysis script:

```
python Main_2_AnalyzeData.py
```

## 8. Conclusion
By following the steps outlined in this README, you will be able to set up your environment, create and run a Scrapy project, and analyze fire incident report data effectively. This guide ensures that new users can easily make use of the provided Scrapy code.






