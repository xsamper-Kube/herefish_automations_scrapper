# Bullhorn (Herefish) Automation Scraper

This repository contains a Selenium-based Jupyter Notebook that automates the extraction of automation metadata from **Bullhorn / Herefish**.

The scraper is designed to be resilient against session drops, page load failures, and long-running extractions across thousands of automation IDs. It produces a structured Excel report with hibernation status and full automation content.

---

## Features

### Automation Coverage
- Scrapes automation details by iterating through a configurable ID range
- Detects deleted or non-existent automations via UI popups
- Extracts:
  - Automation name
  - Status label
  - Full automation content
  - Hibernation status (TRUE/FALSE)

### Hibernation Detection
- Scrapes the full **Hibernated Automations** table first
- Uses a set-based lookup for fast comparison during scraping
- Outputs a dedicated Excel sheet with all hibernated automations

### Fault Tolerance & Recovery
- Automatic re-login if the session expires or redirects to login
- Explicit handling of `InvalidSessionIdException`
- Driver auto-restart on browser crashes
- Anti-blank loading logic:
  - Retries pages that load without content for up to 15 seconds
  - Reloads until valid content or a definitive popup appears

### Progress Persistence
- Periodic Excel backups during execution to prevent data loss
- Final Excel export consolidates all results

---

## Tech Stack

- Python 3.x
- Selenium (Chrome WebDriver)
- Pandas
- Jupyter Notebook

---

## Setup Requirements

### System Requirements
- Google Chrome installed
- Compatible ChromeDriver available on PATH
- Stable internet connection (long-running session)

### Python Dependencies
```bash
pip install selenium pandas
```

---

## Configuration

### Credentials
The notebook currently expects Herefish credentials to be defined as variables:

```python
USERNAME = "your_email"
PASSWORD = "your_password"
```

**Important:**  
Hard-coding credentials is not recommended. For production or shared use, credentials should be supplied via:
- Environment variables
- A secrets manager
- A `.env` file excluded via `.gitignore`

---

## How It Works

1. Logs into Herefish
2. Navigates to the Automations page
3. Scrapes the full **Hibernated Automations** table
4. Iterates through a defined automation ID range
5. For each ID:
   - Detects login redirects
   - Handles missing automations
   - Retries blank or stalled pages
   - Extracts metadata and content
6. Periodically saves progress to Excel
7. Produces a final report on completion

---

## Output

The scraper generates an Excel file with two sheets:

### `Automations`
Contains scraped automation data for the requested ID range:
- `Automation_ID`
- `Name`
- `Status`
- `Is_Hibernated` (TRUE/FALSE)
- `Content`
- `Extraction_Notes`

### `Hibernated Automations`
A complete snapshot of all currently hibernated automations at runtime.

---

## Performance Notes

- Approximate runtime: **~25 minutes per 1,000 automations**
- Performance depends on network stability and Herefish UI responsiveness

---

## Limitations

- Relies on static XPATH selectors (UI changes may break scraping)
- Designed for authenticated internal use only
- Not intended for parallel execution

---

## Intended Use

- Internal automation auditing
- Migration analysis
- Documentation and compliance reviews
- Operational visibility into Bullhorn automations

---

## Disclaimer

This tool interacts with a third-party web application through browser automation.  
Ensure usage complies with Bullhorn / Herefish terms of service and internal company policies.
