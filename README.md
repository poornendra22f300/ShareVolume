# Overview
This single-page web app loads the SEC XBRL company concept data for EntityCommonStockSharesOutstanding, computes the maximum and minimum values since fiscal year 2021, and displays them clearly. It also prepares a standards-compliant data.json for download with the structure:
{"entityName": "3M", "max": {"val": ..., "fy": ...}, "min": {"val": ..., "fy": ...}}

Key features:
- Default target is 3M (CIK 0000066740).
- Live fetch from the SEC endpoint, with automatic fallback via a proxy for compatibility.
- Support for dynamic company switching using a CIK query parameter (?CIK=XXXXXXXXXX) or via the UI field, without reloading the page.
- A “Download data.json” link is generated on-the-fly matching the requested format.

# Setup
No build tools required.

1. Download or clone this project.
2. Open index.html in any modern browser.

Notes:
- The app attempts to call the SEC API directly for 3M using a descriptive User-Agent header. Browsers generally block setting the User-Agent header explicitly; this is included for compliance/readability and will function if run in a server environment.
- For alternate CIKs or if direct CORS is restricted, the app uses a read-through proxy (AIPipe-style via the Jina reader endpoint).

# Usage
- Default view: Open index.html to load 3M data automatically.
- Query parameter: Open index.html?CIK=0001018724 (10-digit CIK) to load a different company.
- Inline control: Enter a 10-digit CIK in the input box and click “Load” to switch companies without reloading.
- Download the computed file: Click “Download data.json” to save the output payload:
  - entityName: "3M" for the default 3M CIK; otherwise the live entity name from the SEC response.
  - max/min: The highest and lowest “val” from units.shares entries with fy > "2020", each including its fiscal year as a string.

The page updates these elements with live data:
- Title and H1 (must include the live entityName from the SEC payload).
- share-entity-name
- share-max-value, share-max-fy
- share-min-value, share-min-fy

If any fetch error occurs, a helpful message is shown and can be retried.