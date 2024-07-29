# README: Web Scraping and Data Analysis with Scrapy

This guide walks you through setting up a virtual environment, creating a Scrapy project, running the web crawler to collect fire incident reports, and analyzing the data. Follow the steps below to get started.

## Table of Contents
1. [Configurations and Installations](#configurations-and-installations)
    11. [Select Interpreter](#Select-interpreter)
    12. [Create a Virtual Environment](#create-a-virtual-environment)
    13. [Activate the Virtual Environment](#activate-the-virtual-environment)
    14. [Upgrade pip](#upgrade-pip)
    15. [Install Required Libraries](#install-required-libraries)
2. [Creating a Scrapy Project](#creating-a-scrapy-project)
    21. [Start Scrapy Project](#5-start-scrapy-project)
3. [Creating the Crawler Class](#creating-the-crawler-class)
    31. [XPath Explanation](#explanation-of-the-logic)
    32. [Create a Spider](#6-create-a-spider)
4. [Running the Crawler](#running-the-crawler)
    41. [Run Combine](#7-run-combine)
    42. [Crawl the Webpage](#7b-crawl-the-webpage)
    43. [Perform the Analysis](#7c-perform-the-analysis)
5. [Conclusion](#8-conclusion)

## 1. Configurations and Installations
### 11. Select Interpreter

- Go to VS Code
    - Windows:
        - Ctrl + Shift + P
    - Mac:
        - Cmd + Shift + P
- Type:
    - Python: Select Interpreter
    - Click on your preferred Interpreter 
        - Python 3.11.9 64-bit (Microsoft Store)

### 12. Create a Virtual Environment
Create a virtual environment to manage your project's dependencies.
```sh
python -m venv venv
```

### 13. Activate the Virtual Environment
Activate the virtual environment to use the installed packages.

- On Windows:
    ```sh
    .\venv\Scripts\activate
    ```

- On Unix or MacOS:
    ```sh
    source venv/bin/activate
    ```

### 14. Upgrade pip
Upgrade pip to the latest version.
```sh
python -m pip install --upgrade pip
```

### 15. Install Required Libraries
Install the necessary libraries specified in the `requirements.txt` file.

```sh
pip install -r requirements.txt
```
If you encounter PowerShell permission issues on Windows, use:
```sh
.\venv\Scripts\python.exe -m pip install -r requirements.txt
```

## 2. Creating a Scrapy Project
Scrapy helps you create a structured web scraping project.

### 21. Start Scrapy Project
Start a new Scrapy project with the name `fire_incidents`.
```sh
scrapy startproject fire_incidents
```
If you encounter PowerShell permission issues on Windows, use:
```sh
.\venv\Scripts\python.exe -m scrapy startproject fire_incidents
```

## 3. Creating the Crawler Class

### 31. Explanation of the Logic
The crawler retrieves information from the website using XPath expressions.

311. Open the Scrapy shell to test XPath on the fly:
    ```sh
    scrapy shell
    ```

312. Fetch the page:
    ```python
    r = scrapy.Request(url="https://fireandemergency.nz/incidents-and-news/incident-reports/")
    fetch(r)
    ```

313. Get the Central region div:
    ```python
    on_central_div = response.xpath("//div[@class='incidentreport__region'][h3[text()='Central']]")
    on_central_div
    ```

314. Get the first list element:
    ```python
    li = on_central_div.xpath(".//ul[@class='incidentreport__region__list']/li/a")[0]
    ```

315. Extract the href from the first list element:
    ```python
    li.xpath("@href").get()
    ```

316. Extract the text from the first list element:
    ```python
    li.xpath("text()").get().strip()
    ```

### 32. Create a Spider
Create a spider to crawl the web page.
```sh
cd fire_incidents
scrapy genspider incident_reports fireandemergency.nz/incidents-and-news/incident-reports
```
Check the `robots.txt` of the website to ensure you are allowed to crawl it:
- [https://fireandemergency.nz/robots.txt](https://fireandemergency.nz/robots.txt)

## 4. Running the Crawler
The spider class will crawl the web, extract fire incident report data, and save it to a CSV file.

### 41. Run Combine
First, execute the `Main_1_ScrapyRunner.py`, followed by `Main_2_AnalyzeData.py`.
```sh
python Main_3_Combine.py
```

### 42. Crawl the Webpage
Run the spider to extract data and save it into a CSV file in the `data` folder.
```sh
cd path/to/your/root/directory
python Main_1_ScrapyRunner.py
```

### 43. Perform the Analysis
This script retrieves and analyzes the latest `day_of_week_incident_reports` CSV file to answer the following questions:

- How many incidents has the Stratford Brigade responded to in the last 7 days?
- How many medical incidents have been reported in the Central Region in the last 7 days?
- Where were the medical incidents reported in the last 7 days?

Run the analysis script:
```sh
python Main_2_AnalyzeData.py
```

## 5. Conclusion
By following the steps outlined in this README, you will be able to set up your environment, create and run a Scrapy project, and analyze fire incident report data effectively. This guide ensures that new users can easily make use of the provided Scrapy code.