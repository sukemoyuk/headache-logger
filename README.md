<div align="center">

<img src="img/header-light@2x.png#gh-light-mode-only" alt="Headache Log" width="139" height="48">
<img src="img/header-dark@2x.png#gh-dark-mode-only" alt="Headache Log" width="138" height="48">

**A privacy-focused, serverless headache tracker for multiple devices**

[![Version](https://img.shields.io/badge/version-v0.1.0-22272e.svg?style=flat-square)](CHANGELOG.md)
[![License](https://img.shields.io/badge/license-MIT-22272e.svg?style=flat-square)](LICENSE)
[![Hosting](https://img.shields.io/badge/hosting-GitHub_Pages-22272e.svg?style=flat-square)](https://pages.github.com/)
[![Backend](https://img.shields.io/badge/backend-Supabase-22272e.svg?style=flat-square)](https://supabase.com/)

</div>

---

## Overview

**Headache Log** is a web app for quickly recording headache episodes, medication, and pain severity, then reviewing that history or sharing it with a healthcare professional.

Hosted as a static site on GitHub Pages and backed by Supabase, it provides secure syncing across computers and phones without server operating costs. Local caching also lets you keep recording while offline.

---

## Features

| Feature | Description |
| :--- | :--- |
| **Quick logging** | Record the time, pain severity, symptom tags, medication, and notes with a few taps. |
| **Calendar and history** | See episode frequency by month, and edit or delete past entries. |
| **Statistics and charts** | Summarize today, the last 7 or 30 days, all time, or a custom date range. Switch between status and count, and daily and cumulative views. Explore symptom and medication heatmaps and cumulative line charts. |
| **Sort history** | Sort entries in the statistics view by date and time, in ascending or descending order. |
| **Tags and themes** | Show and reorder symptom, medication, and note tags; switch between light and dark themes. |
| **Multi-device sync** | Authenticate with Supabase Auth and sync data both ways with PostgreSQL. |
| **Offline first** | `localStorage` caching keeps entries available when you are signed out or offline. |
| **Privacy protection** | PostgreSQL Row Level Security (RLS) prevents access by other users at the database level. |

---

## Tech Stack

* **Frontend**: HTML5, CSS3, Vanilla JavaScript (ES6+), Chart.js
* **Hosting**: GitHub Pages
* **Backend as a Service**: Supabase (PostgreSQL, Supabase Auth, PostgREST)
* **Architecture**: Serverless SPA (Single Page Application)

---

## Quick Start

### 1. Open the app in a web browser

No local build is required. Open `index.html` in a browser to use the tracker offline.

```bash
# Clone the repository
git clone https://github.com/sukemoyuk/headache-logger.git

# Open the app
open index.html  # macOS
start index.html # Windows
```

### 2. Enable cloud sync

To share data across devices, create a Supabase project and save its Project URL and API key under **Settings → Cloud Sync** in the app. You can then register an account for the project; each account stores its own entries.

After saving the connection settings, register or sign in with an email address and password in the account section on the same screen. You can use the app without signing in, but cloud sync and sharing across devices require an account. Use a Publishable key or Legacy anon key. Do not enter a `service_role` or Secret key.

When you are signed out, entries are stored in the browser's `localStorage`. Storage is managed separately for each site (origin), browser, and device; there is no fixed limit on the number of entries under normal use. Clearing browsing history alone usually does not remove them, but clearing cookies and site data, deleting the browser profile, or similar actions may erase your entries. Sync important entries to Supabase.

For detailed instructions, see the [Setup Guide](docs/SETUP_GUIDE.md).

---

## Documentation

Project design and operations documentation is available in the `docs/` directory.

* [System Architecture and Database Design](docs/ARCHITECTURE.md)  
  System diagrams (Mermaid), database schema (JSONB), and RLS security design.
* [Environment Setup Guide](docs/SETUP_GUIDE.md)  
  Supabase setup (including SQL scripts), connection instructions, and deployment to GitHub Pages.
* [Changelog and Versioning](CHANGELOG.md)  
  Release notes and semantic versioning policy.

---

## License

This project is released under the [MIT License](LICENSE).
