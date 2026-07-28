# Module 1 — Data Pipeline (/data_pipeline)

This module is part of the Zepto Data & AI Platform Capstone Project. It demonstrates a raw-to-relational data engineering pipeline that scrapes live catalog data, cleans and enriches it, and stores it in a normalized SQLite database.

## Project Setup & Installation

**Requirements:**
* Python 3.7+
* `requests`
* `beautifulsoup4`
* `pandas`
* `sqlite3` (Standard Library)

**Installation:**
1. Clone this repository.
2. Install the required packages (if running locally): `pip install requests beautifulsoup4 pandas`
3. Open the provided Jupyter Notebook in Google Colab or your local Jupyter environment.

## Execution Steps
Run the cells in the provided notebook sequentially to:
1. Scrape 300 books across the first 15 pages of the catalog.
2. Clean and format the raw data fields.
3. Generate the normalized `books_project.db` SQLite database.
4. Execute the SQL queries and verify the JOIN logic against pandas.

## Design & Cleaning Decisions
* **Data Source:** Data was scraped freely from `books.toscrape.com`.
* **Price Extraction:** Stripped the `£` currency symbol and converted the text to a float (`price_gbp`).
* **Currency Conversion:** Applied the project's baseline fixed-rate conversion: **1 GBP = 105.50 INR**.
* **Rating Parsing:** Mapped text-based star ratings ('One' through 'Five') to integers (1-5). Missing ratings were handled using median imputation.
* **Availability:** Parsed the raw availability string to create a boolean `in_stock` column (True if "In stock" was present).
* **Handling Imperfect Rows:** Any rows missing critical base fields (`price_gbp`, `title`, `category`) after initial parsing were explicitly dropped to prevent pipeline crashes.

## Database Schema
The data is stored in a normalized relational SQLite database with a primary/foreign key relationship:
* **`categories` table:** `category_id` (INTEGER PRIMARY KEY AUTOINCREMENT), `category_name` (TEXT UNIQUE)
* **`books` table:** `book_id` (INTEGER PRIMARY KEY AUTOINCREMENT), `title` (TEXT), `price_gbp` (REAL), `price_inr` (REAL), `rating` (INTEGER), `in_stock` (INTEGER), `category_id` (INTEGER FOREIGN KEY referencing `categories(category_id)`)
