# Herefish Automation Scraper

This script automates the extraction of automation content from
**Herefish (Bullhorn Automation)** using Selenium. It logs in, iterates
through a range of automation URLs, collects their contents, and saves
everything into an Excel file. The script includes retry logic,
throttling, popup detection, and periodic saving to prevent data loss.

## Features

-   Automated login to the Herefish web app\
-   Scrapes automation pages sequentially by ID\
-   Detects and skips popups and empty pages\
-   Retries on network failures\
-   Throttles requests to avoid rate-limits\
-   Colelcts hibernated automations and notes which one is it\
-   Saves progress at regular intervals\
-   Exports results to an Excel file (`herefish_automations.xlsx`)

## Requirements

### Python Packages

    pip install selenium pandas openpyxl

### WebDriver

Install a compatible WebDriver (ChromeDriver or GeckoDriver).\
Ensure it is in your PATH or in the same directory as the script.

### Other Requirements

-   Python 3.8+\
-   Valid Herefish login credentials\
-   Stable internet connection

## Configuration

### Credentials

Update the credentials section or replace them with environment
variables:

    USERNAME = "YOUR_USERNAME_HERE"
    PASSWORD = "YOUR_PASSWORD_HERE"

### Automation ID Range

Adjust as needed:

    automation_ids = range(10000, 21676)

### Output File

The script writes results to:

    output_file = "herefish_automations.xlsx"

## How It Works

1.  Opens the browser and logs into Herefish\
2.  Iterates over each automation ID\
3.  Loads the page and checks for a popup\
4.  Extracts automation text from the main container\
5.  Saves each successful scrape to memory\
6.  Writes progress to Excel every few scrapes\
7.  Retries when network issues occur\
8.  Saves a final Excel file when complete

Skipped automations include:\
- Pages that display a popup\
- Pages that return no content\
- Pages that fail repeatedly during scraping

## Throttling & Reliability

-   **Success delay:** 10 seconds\
-   **Failure delay:** 2 seconds\
-   **Network retry wait:** 5 minutes\
-   **Periodic save interval:** Every 2 successful scrapes

## Output Format

The resulting `herefish_automations.xlsx` file contains two columns:

  ID      Content
  ------- -------------------
  10001   Extracted text...
  10002   Extracted text...
  ...     ...

## Notes

-   The script relies on specific XPaths that may change if the Herefish
    UI updates.\
-   Scraping thousands of automations may take several hours depending
    on your throttling settings.\
-   Ensure you have permission to automate and scrape data from
    Herefish.

## Disclaimer

Use this script only if you have authorization to access and extract
data from your organization's Herefish environment. Respect all internal
policies and terms of service.
