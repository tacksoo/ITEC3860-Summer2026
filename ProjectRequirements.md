# Sell-Tracker API — Project Requirements

## 1. Overview

A REST API, built with Spring Boot, that helps people keep track of items they want to sell online (Ebay, Craigslist, Facebook Marketplace, etc.). Each user has their own private list of items. For each item, the user can record where they've listed it and how much money they made when it sold.

The API only stores the data and does the math. It does **not** post listings to Ebay or Craigslist — the user still creates those listings manually on each site and then records them here.

## 2. Users and Access

- Anyone can create an account.
- Each user gets their own personal **access token** when the account is created.
- Every request (except sign-up) must include that token so the API knows who is asking.
- A user can only see and change their own items and listings. They cannot see anyone else's data.

## 3. What the API Tracks

### 3.1 User

- Username (or email) — used to identify the account
- Password — stored securely (hashed, never in plain text)
- Access token — given to the user when they sign up and used for all future requests

### 3.2 Item (the thing being sold)

Each item belongs to exactly one user and has:

- **Title** — short name of the item (required)
- **Description** — longer explanation of what it is (optional)
- **Notes** — free-form personal notes, e.g. "keys are in the top drawer" (optional)
- **Status** — either `AVAILABLE` or `SOLD` (defaults to `AVAILABLE`)
- **Cost** — what the user paid for the item, in dollars (needed for profit)
- **Sold price** — what the item sold for (only filled in once it's sold)
- **Fees** — total fees paid to the platform when it sold, e.g. Ebay's cut (only filled in once it's sold)
- **Created date** — set automatically when the item is added
- **Updated date** — set automatically whenever the item is changed

### 3.3 Listing (where the item is posted)

The same item can be listed on more than one site at the same time. Each listing belongs to one item and has:

- **Platform** — name of the site, e.g. "Ebay", "Craigslist", "Facebook Marketplace"
- **URL** — link to the actual listing on that site
- **Date listed** — when the user posted it
- **Status** — free-text status the user picks, e.g. "Active", "Removed", "Expired"

The user adds and removes listings manually — the API does not sync with Ebay, Craigslist, or any other outside service.

## 4. What the API Can Do

Below is a plain-English list of what the API must support. HTTP paths are shown to make the intent clear, but the exact URLs can be adjusted during development.

### 4.1 Accounts

- **Sign up** — create a new user account and receive an access token in the response.
- **Get my access token** — sign in with username + password and get the token back (in case the user loses it).

### 4.2 Items

- **Add an item** — with at least a title.
- **List my items** — show all items belonging to the logged-in user.
- **Search / filter my items** — at minimum, filter by status (`AVAILABLE` or `SOLD`).
- **Get one item** — including all of its listings.
- **Update an item** — change any of its fields.
- **Mark an item as sold** — set status to `SOLD` and record sold price and fees.
- **Delete an item** — also deletes all of its listings.

### 4.3 Listings (under an item)

- **Add a listing** to a specific item.
- **List all listings** for a specific item.
- **Update a listing** — change platform, URL, date, or status.
- **Delete a listing.**

### 4.4 Profit

- **Profit for one item** — for a sold item, return: cost, sold price, fees, and profit (sold price − cost − fees).
- **Total profit summary** — for the logged-in user, return the totals across all sold items: total cost, total sold, total fees, and total profit.

## 5. Rules the API Must Enforce

- A user can only see/change/delete their own items and listings. Trying to access someone else's data returns an error.
- Title is required when creating an item.
- Status must be either `AVAILABLE` or `SOLD` — nothing else.
- Sold price and fees can only be set when status is `SOLD`.
- Cost, sold price, and fees cannot be negative.
- A listing must be attached to an item that belongs to the same user.
- Passwords are never returned by the API.

## 6. Technology

- **Language / framework:** Java with Spring Boot
- **Database:** PostgreSQL, hosted on Supabase
- **Data access:** Spring Data JPA
- **Auth:** simple bearer-token authentication (token stored on the user record)
- **Format:** all requests and responses use JSON

## 7. Out of Scope (for now)

To keep the first version simple, the following are **not** part of this project:

- Photos or file uploads for items
- Categories, tags, condition, dimensions, weight, or physical location fields
- Buyer information / customer records
- Price-change history
- Automatic posting or syncing with Ebay, Craigslist, or any other site
- Detailed statuses beyond `AVAILABLE` / `SOLD`
- Admin users or role-based permissions
- Reports beyond the basic profit summary
- A web or mobile front-end (this project is API-only)
