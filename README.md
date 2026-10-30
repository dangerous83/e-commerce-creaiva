# Creaiva Marketplace

A responsive UAE classifieds marketplace **interface preview**, hosted on GitHub Pages.

**Website:** https://dangerous83.github.io/e-commerce-creaiva/

## What works

- Responsive storefront with eight categories and twelve clearly marked sample listings.
- Keyword search, UAE location filters, condition and price filters, and sorting.
- Listing detail dialogs with original inline SVG product illustrations.
- Saved favourites persisted in this browser with local storage.
- Create and delete local listing drafts; drafts stay in the current browser and are never published.
- Keyboard-accessible dialogs, labelled controls, and reduced-motion support.

## Preview scope

All listings, prices, property advertisements and vacancies are illustrative. There are no real sellers to contact. Sign-in, seller accounts, image uploads, shared listings, private messaging and payments are **not connected**. The interface clearly explains these limits instead of accepting credentials or pretending to send inquiries.

Favourites and drafts use browser local storage. If it is unavailable, changes remain in memory for the current visit. Clear browser data to remove stored preview data. The page requests Inter from Google Fonts and falls back to system fonts. Product artwork is included in the page and has no external image dependencies.

## Run or edit

The complete website is in `index.html`; no install or build step is required. Open it in a browser or serve it with any static web server. GitHub Pages serves the root of the `main` branch.

## Next stage

Real multi-user accounts, persistent public listings, image uploads, messaging and billing require a backend. The previous README described a React/Vinext/D1/R2 application, but that application source was absent from this repository; the repository contained only that README and an empty `index.html`. This commit supplies the working static interface without claiming those server features exist.
