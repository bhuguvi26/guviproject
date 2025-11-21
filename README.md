# guviproject
User History Extraction & Analysis
Overview
This project is designed to make sense of email activity recorded in server log files. Logs are usually unstructured and hard to analyze, so the goal here is to extract email addresses and timestamps, store them in databases, and run queries to get actionable insights. In short, it’s a small but complete data pipeline for tracking historical email activity.

Problem
Server logs contain valuable information about user activity, but the raw data is messy. This project solves that by:

Extracting every email address along with its associated date and time.
Cleaning and standardizing the data.
Storing it in MongoDB and SQLite.
Running SQL queries to answer practical questions like “Who sent the most emails?” or “Which email domains are most active?”
Project Workflow
Extract Emails and Dates – Reads the log file and captures all email addresses with their timestamps.
Transform Data – Formats dates in a standard YYYY-MM-DD HH:MM:SS format and structures the data for database insertion.
Save to MongoDB – Inserts cleaned data into a MongoDB collection (user_history).
Move to SQLite – Fetches data from MongoDB (or directly uses local data if MongoDB isn’t available) and inserts it into a relational table.
Analyze – Executes SQL queries to generate insights like unique users, email counts per day, first/last email dates, and top domains.
Tools & Technologies
Python 3 – Main scripting and data processing.
MongoDB Atlas – NoSQL storage for document-based data.
SQLite – Relational database for SQL analysis.
Regular Expressions (Regex) – For extracting emails and dates.
Google Colab – Supports file upload, processing, and SQLite download.
Project Structure
project/ │ ├── mbox.txt # Input server log file ├── user_history.db # Generated SQLite database ├── pipeline.py # Main Python script └── README.md # This document 2. Upload Your Log File

The script will prompt you to upload mbox.txt.

Ensure log entries look like:

From user@example.com Sat Jan 5 09:14:16 2025

Run the Pipeline python pipeline.py
The script will:

Extract email-date pairs.

Upload to MongoDB (if available).

Save the data into SQLite.

Run SQL queries to generate insights.

Example SQL Queries

Here are some sample queries to analyze the data:

Count unique email addresses

SELECT COUNT(DISTINCT email) FROM user_history;

Emails per day

SELECT DATE(date), COUNT(*) FROM user_history GROUP BY DATE(date);

First and last email per address

SELECT email, MIN(date) AS first_email, MAX(date) AS last_email FROM user_history GROUP BY email;

Top email domains

SELECT SUBSTR(email, INSTR(email,'@')+1) AS domain, COUNT(*) AS count FROM user_history GROUP BY domain ORDER BY count DESC;

Total emails

SELECT COUNT(*) FROM user_history;

Top 5 users by email count

SELECT email, COUNT(*) AS count FROM user_history GROUP BY email ORDER BY count DESC LIMIT 5;

Emails from Gmail

SELECT COUNT(*) FROM user_history WHERE email LIKE '%@gmail.com';

Emails after a specific date

SELECT * FROM user_history WHERE date > '2025-01-01 00:00:00';

Emails per month

SELECT STRFTIME('%Y-%m', date) AS month, COUNT(*) FROM user_history GROUP BY month;

Custom queries

Any other analysis you need can be executed using standard SQL.
