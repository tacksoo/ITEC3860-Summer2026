# ITEC3860 Summer 2026 — Group Project

## Sell-Tracker API

A REST API for keeping track of items you want to sell online across sites like Ebay, Craigslist, and Facebook Marketplace.

### Goal

Give a seller one central place to record what they have for sale, where they have it listed, and how much money they actually made when it sold — without depending on any one selling platform.

### What it does

- Lets each user sign up and get a personal access token so their data stays private.
- Stores the items a user wants to sell (title, description, notes, cost, sold price, fees, status).
- Tracks every place an item is listed (platform name, URL, date listed, listing status), since the same item can be posted on more than one site at a time.
- Marks items as `AVAILABLE` or `SOLD` and records the sold price and fees when they sell.
- Calculates profit per item and total profit across all sold items (sold price − cost − fees).

### What it does NOT do

- It does **not** post listings to Ebay, Craigslist, or any other site. The user still creates those listings on each site themselves; this API is just for tracking them.
- No web or mobile front-end is part of this project — it is API-only.

### Tech stack

- Java + Spring Boot
- Spring Data JPA
- PostgreSQL (hosted on Supabase)
- Bearer-token authentication
- JSON over HTTP

### Project docs

Full requirements are in [`ProjectRequirements.md`](./ProjectRequirements.md).
