Spark Homes Field Estimator

A mobile-first, offline-first repair cost estimator built for Spark Homes' acquisition team, submitted for the Spark Homes Developer Contest (July 2026).

Live app: open index.html directly, or host the folder as static files (GitHub Pages, Netlify, etc.) and visit it on a phone.

What it does

Agents walk a distressed property room by room, check off needed repairs from Spark Homes' official 108-item price list, enter quantities, and get a running total in real time — no signal required. When the walkthrough is done, they export a ZIP with an Excel cost breakdown and every photo taken on site.

Requirements met


Single self-contained file. index.html is the entire app — vanilla HTML/CSS/JS, no build step, no framework, no Node server. manifest.json, sw.js, and two PNG icons are the only "additional static files," used purely for PWA install/offline support.
Installable PWA, works offline. sw.js precaches the app shell and runtime-caches CDN libraries after first load, so the app (including export) keeps working with no connection. Standalone display mode, custom icons, theme color.
All data in localStorage. Projects, price overrides, and photos (as base64) persist on-device. No backend is required; nothing about the app depends on a server being reachable.
108 real line items from Spark Homes' official Pricing_List.csv, organized into the 19 required groups plus 5 additional groups (General & Punch List, Plumbing, Grounds & Hardscape, Closet, Lighting) needed to give every single item in the real price list a home — the reference implementation left 21 of its own 108 items ungrouped (not shown anywhere in its checklist); this version places all 108. Item IDs, names, costs, and units are used exactly as provided; nothing was invented or altered.
Adjustable rooms. Kitchen, Interior/General, Systems & Structure, and Exterior are fixed single instances; Bathroom, Bedroom, and Living/Common Area are add/remove-able instances, each labeled by number (Bathroom 1, Bathroom 2, …).
"No Action Needed" toggle on every group, tracked separately from item selections.
Per-project price overrides on any line item, plus a global Settings screen ("the gear icon" on the Projects screen) that edits standard pricing and rolls out to every project.
Add / delete items on a per-item basis — both per-project (ad hoc items added during a walkthrough, or hiding a standard item from just that project) and globally (Settings screen), matching the brief's "add new line items or delete existing ones" requirement at both levels.
Progress tracking per group, rolled up across every room instance, shown as a top progress bar.
Photo capture via <input type="file" accept="image/*" capture="environment">, thumbnails, per-photo removal, and notes.
Serial number scanning: on-demand OCR using Tesseract.js (loaded from CDN only when the "Scan photo" button is pressed, so it never blocks offline use). It looks for a labeled S/N / SERIAL / MODEL pattern first, then falls back to the longest alphanumeric token — a heuristic, always presented as a "candidate to verify," not an authoritative read.
Export. One tap zips an Excel workbook (cost breakdown + a Deal Summary sheet) with every project photo, using SheetJS and JSZip, and downloads automatically.
Creative addition — Deal Analyzer (see writeup for rationale): a live profit-margin calculator that pulls the running repair total straight from the Estimate tab and projects total cost basis, profit, and margin % against ARV, with a color-coded gauge (green/amber/red) so an agent can sanity-check the deal before leaving the driveway.


Stack

Vanilla JS/HTML/CSS. CDN libraries: JSZip (ZIP export), SheetJS/xlsx (Excel export), Tesseract.js (lazy-loaded, serial-number OCR only). Google Fonts: Barlow Condensed, Inter, IBM Plex Mono.

Running locally

No install needed. Two options:


Double-click index.html — most features work directly from the filesystem, though the service worker (offline caching) requires a server context in some browsers.
Serve the folder so the service worker registers cleanly:


bash   cd spark-homes-estimator
   python3 -m http.server 8080
   # visit http://localhost:8080 on desktop, or your machine's LAN IP on a phone

File structure

index.html          the entire app: markup, styles, and logic
manifest.json        PWA manifest
sw.js                 service worker (offline caching)
repair-items.csv     the official Spark Homes price list (108 items / 24 groups) — mirrored verbatim into index.html, with a `group` column added to place every item
icons/                app icons (192px, 512px) built from the real Spark Group mark, plus the original logo file

AI tool use

Built with Claude. See the one-page PDF writeup for specifics on where AI helped and what I did by hand.
