# stock-price-tracking-bot

This project is a Google Sheets automation bot that retrieves real-time stock prices and financial news using Google Apps Script. It updates data automatically on a schedule and helps monitor market movements directly inside a spreadsheet.

# Features

-- Fetches real-time stock prices using the Alpha Vantage API

-- Retrieves financial news headlines using NewsAPI (currently under construction)

-- Shows price movement direction by comparing current and previously stored prices

-- Displays percentage changes and price updates

-- Automatically refreshes data on an hourly schedule

-- Simple setup using Google Sheets and Apps Script

# How It Works

-- The user enters stock symbols into the Google Sheet (e.g., AAPL, TSLA, MSFT).

-- The Apps Script calls the Alpha Vantage API to retrieve the latest price and change percentage.

-- The script compares the new price with the previously stored price and determines whether it increased, decreased, or stayed the same.

-- A direction indicator (up, down, or no change) is added next to the percent change.

-- Another script fetches the latest financial news headlines and populates them into a separate sheet.

-- Triggers run the updates automatically every hour.

# File Structure

-- updateStockPrices() — Fetches stock prices and identifies price movement

-- updateStockNews() — Loads financial news headlines

-- createTriggers() — Schedules hourly automatic updates

-- setup() — One-time initializer

**Notes**

Alpha Vantage free tier is rate limited to 5 requests per minute.

Ensure stock symbols are valid and placed correctly in the sheet.

Sheets update automatically but can also be refreshed manually by running individual functions.
