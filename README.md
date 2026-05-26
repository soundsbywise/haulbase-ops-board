# Haulbase TMS App — Master Task List

> **Last updated:** 2026-05-26
> **Complexity:** 1 = small, 2 = medium, 3 = large

---

## 🔄 In Progress

| Name | Type | Complexity | Description                              |
| ---- | ---- | ---------- | ---------------------------------------- |
| —    | —    | —          | Nothing currently in active development. |

---

## 📋 Pending

| Name | Type | Complexity | Description |
|------|------|------------|-------------|
| Loads page: total count + count by status | Feature | 1 | Show total load count and a breakdown by status (e.g. In Transit: 12 / Delivered: 45 / Cancelled: 3) on the loads list page. |
| Client-side warning: duplicate load or ref number | Enhancement | 1 | When creating a load, warn inline as the dispatcher types if the load number or ref number already exists. Non-blocking — flags the conflict only, does not prevent saving. |
| Client-side warning: duplicate MC or DOT number | Enhancement | 1 | When creating a new company, warn inline if the MC # or DOT # already exists. Same non-blocking behavior as the load number warning. |
| Loads table: show time window next to pickup/delivery date | Enhancement | 1 | Table rows show date only (e.g. "May 22"). Should append the time window when set (e.g. "May 22 \| 08:00–12:00"). Applies to both the Loads page and Company → Loads sub-tab.<br>**Eng:** `formatPickupDate()` in `loads-view.tsx`. `LoadRow` type needs `scheduledTimeWindow` added and fetched in `loads/page.tsx` and `companies/[id]/loads/page.tsx`. |
| SMS clipboard: include time window for pickup/delivery | Enhancement | 1 | Both SMS builders should include the time window next to the date for pickup and delivery stops.<br>**Eng:** `buildRowSms()` in `loads-view.tsx` and `buildSmsMessage()` in `load-detail-view.tsx`. |
| Loads table: fix column value alignment | Bug | 1 | Column values across rows are not visually aligned — likely a truncation, padding, or column-width mismatch.<br>**Eng:** Row `div` with `gridTemplateColumns` in `loads-view.tsx`. Needs visual inspection and CSS fix. |
| Load form: rename "Notes" → "Internal Haulbase Notes" | Enhancement | 1 | Clarifies the field is internal only and not sent to drivers via SMS.<br>**Eng:** `load-form.tsx` ~line 602 and `load-detail-view.tsx` ~line 434. |
| Load detail: always show Internal Notes + Driver Instructions when empty | Enhancement | 1 | Both sections currently only render when non-empty. Should always display with a greyed-out placeholder ("—") when empty so dispatchers can see the fields exist.<br>**Eng:** `load-detail-view.tsx` — remove `&&` guards around Notes and Driver Instructions blocks; add empty-state fallback. |
| Dispatch fee calculation: use Revised Rate basis | Enhancement | 2 | Dispatch fee must be calculated using a Revised Rate, not just the Original Rate. Proposed formula: Revised Rate = Original Rate + Layovers + Detention + TONU. The Haulbase dispatch % is then applied to the Revised Rate. Lumper is a separate field on the load but is explicitly excluded from the dispatch fee calculation basis. Must apply retroactively to existing loads so all records use the correct formula.<br>**Eng:** ⚠️ Finalize the exact equation with Simarpreet before implementing. The general intent is confirmed (revised rate basis, retroactive recalc) but the precise formula needs sign-off. |
| Load sorting | Feature | 2 | Allow dispatchers to sort the loads list by: Delivery Date, Pickup Date, City, State, Company. |
| Load filtering: full filter set | Feature | 2 | Comprehensive load filtering across the loads page. Filter by: Broker, Company, Pickup City, Delivery City, Pickup Date, Delivery Date. Also includes broker name in fuzzy search index so typing a broker name returns matching loads. Replaces the earlier broker-only filter scope.<br>**Eng:** Needs dedicated filter chips/dropdowns and date range pickers alongside the existing status filter. |
| "Claim" load status | Feature | 2 | Add a new "Claim" load status. In a claim situation the driver still gets paid, which means the Haulbase dispatch fee is still charged. Full workflow TBD — ops team will provide more detail later. |
| Broker: add new broker confirmation during load entry | Enhancement | 2 | When a dispatcher types a new broker name during load entry, the system should prompt "Do you want to add a new broker?" before creating the record. If yes, they create the broker and confirm details are correct before saving. Prevents accidental/bogus broker entries. System should also block duplicate broker names from being created at any point. |
| Broker: additional business fields | Feature | 2 | Add core business fields to the broker record: Broker # (internal ref), RC Found (boolean), Data Entered By, MC #, DOT #, Address/City/State/ZIP, Website, Broker Phone, Dispatch Phone, After Hours Phone, Broker Email, Invoice & POD Email, Quick Pay Email, Billing/Payment Inquiries Contact # + Mailing Address. Rep fields are handled separately under "Broker: representatives and per-load selection."<br>**Eng:** ⚠️ Each field needs a scope review with Simarpreet before building — confirm required vs. optional, data types, UI grouping. Also cross-reference ops team's current Broker List spreadsheet for the authoritative required field list. |
| Broker + Company: delete only when no loads attached | Feature | 2 | Allow deleting a broker or company only if no loads are associated. If loads exist, surface a clear error: "This broker has X loads attached and cannot be deleted." |
| POD table: show pickup + delivery info | Enhancement | 2 | POD table rows should show pickup location (city/state + date/time window) and delivery location (city/state + date/time window), styled like the loads table.<br>**Eng:** `pods/page.tsx` — expand stop query. `pods-view.tsx` — update `PodLoad` interface, grid columns, render location + date/time cells. |
| Company: Documents + Pictures tab | Feature | 2 | Add a dedicated "Documents" tab on the company page. Users can upload, name, and store company-level files. Pictures are treated the same as documents — use labels or categories to distinguish them. Supports any file type with user-defined labels. |
| Credential Management System (per company) | Feature | 2 | Per-company storage for all credentials: label (what it's for), username, password. Examples: MCS-150, factoring company, Highway, RMIS, broker logins. Generic and flexible — any number of credential entries per company. Replaces the earlier factoring-only credential storage concept. Should live under a dedicated "Credentials" tab on the company page. |
| Load document management: RC + BOL + POD per load | Feature | 3 | Each load requires three documents: Rate Confirmation Sheet, Bill of Lading (BOL), and Proof of Delivery (POD). Staff upload them as received. A load cannot progress to the Invoicing section until all three are uploaded. System should support a report showing which documents are still pending per load. Upload workflow must be designed for how docs actually arrive in the field: email and text message photos — needs to be fast and friction-free (e.g. direct upload from mobile, drag-and-drop). Expands and clarifies the existing RC Scanner + Document Center concept.<br>**Eng:** RC Scanner auto-population (AI/OCR pre-fill load form from RC PDF) is still the goal for RC upload. Advanced UX: highlight parsed fields on PDF, let user redraw selection box to re-parse. Build RC Scanner, BOL, and POD document management together as one Document Center. |
| Address auto-population | Feature | 2 | When entering an address anywhere in the tool, suggest and auto-populate the full address (Google Maps typeahead style). |
| Status transition animations | Enhancement | 2 | When a load's status change moves it between tabs (Not Started → In Progress → Delivered/Cancelled), show a brief highlight flash on the moved load when the user switches to the destination tab. |
| Compliance: CDL expiration reminders | Feature | 2 | Flag drivers whose CDL is approaching expiration. Reminder cadence: 60 days, 30 days, 7 days, day of. In-app and email notifications. Part of the broader compliance/Document Center surface area. |
| Compliance: Medical Certificate tracking | Feature | 2 | Track driver Medical Certificate expiration with the same reminder cadence (60/30/7/0 days). Part of the compliance/Document Center surface area. |
| Compliance: Truck registration expiration tracking | Feature | 2 | Track Truck Registration Expiration Date (from CAB Card) with the same reminder cadence. Part of the compliance/Document Center surface area. |
| Insurance system | Feature | 3 | Full insurance tracking per company. Policies apply at company, truck, or trailer level. Track per policy: insurance type, effective date, expiration date, and uploaded proof-of-insurance document. Expiration notifications at set intervals (cadence TBD). Insurance types to support: Auto Liability, Physical Damage, Motor Truck Cargo, General Liability, Bobtail (Endorsement), OCC/ACC, Trailer Interchange (Endorsement), Non-Owned Trailer (Endorsement). Lives under the "Insurance" tab on the company page (currently a Coming Soon placeholder). Secondary goal: expiration notifications can generate leads for Nihalife Insurance — allow Nihalife team to proactively contact carriers before policies expire. |
| Broker: representatives and per-load selection | Feature | 2 | Each broker can have multiple representatives (name + direct phone number). When creating or editing a load, dispatcher selects the correct broker rep from the database. If the rep doesn't exist yet, they can add one inline. On the load view, show: Broker Name, Rep Name, Rep Direct Contact number.<br>**Eng:** Broker reps should be a separate related table (broker_reps) linked to brokers. The load form needs a rep selector that filters by the selected broker. |
| Broker reporting | Feature | 3 | Reporting section: run reports showing — number of loads per broker, total revenue by broker, total miles by broker, RPM by broker, lanes served by broker. Lane concept to be discussed separately. |
| Management Dashboard (home screen) | Feature | 3 | Full management dashboard on the home screen. Filters: select a specific company, a specific truck within a company, or all companies. Date filters: Week (current), Month (current), Quarter (current), plus custom From/To date range. Metrics displayed: Total Revenue, Total Loads (excluding cancelled), Total # of Trucks, Loaded Miles, Empty Miles, Rate Per Mile, Total HB Dispatch Fee, Average Revenue Per Truck. Trend visualization: visually show whether key metrics (Revenue, Loads, Miles, RPM, Dispatch Revenue) are increasing, decreasing, or flat over time. Design: clean, modern, not crowded — leadership can get full operational health snapshot without opening multiple screens. |
| Report: First Month Performance + Dispatch Fee Notification | Feature | 3 | Auto-generated report when a carrier completes their first month. Dispatch fee effective date = 31 days from the carrier's first load pickup date. Report metrics: Total Loads, Total Revenue, Loaded Miles, Empty/Deadhead Miles, Total Miles, Average RPM (miles via Google Maps). Delivered as a formatted email to the carrier notifying them their free period is complete and the 5% dispatch fee begins. Sample email template provided by ops — use that format. Ties into the existing Dispatch Rate Tracking (30-day free-look) logic already built. |
| Report: Monthly Revenue Report per carrier | Feature | 3 | Generate a monthly revenue report for each carrier covering a selected date range. Operational summary: Total Loads, Total Miles, Loaded Miles, Empty Miles, RPM. Financial summary: Total Invoice, Dispatch Paid, Factoring Charges, Net Revenue. Delivered as a formatted email to the carrier. Sample email template provided by ops — use that format. |
| Report: Weekly Revenue Report | Feature | 3 | Weekly report summarizing carrier billing activity and operational performance. Per-carrier data: Carrier Name, Owner Name, Number of Loads, Total Invoice Amount, Invoice Status, Free Trial Status, Operational Status. Summary metrics: Gross Sales, Number of Active Trucks, Weekly Totals, Monthly Comparison Data. Carrier groupings: Payable Invoices, Under 30 Days Free Period, OFF, INACTIVE, Under Free Look. Design should be clean and easy to read for management review of billing status and revenue trends. |
| Stage 4: Haulbase Billing + QuickBooks integration | Feature | 3 | Once stages 1–3 are complete, loads are eligible for the Friday billing run. Long-term: integrate QuickBooks API to eliminate manual data entry. "Ready to Bill" status already exists.<br>**Eng:** QuickBooks credentials likely available. Triumph Capital partnership carriers receive a better rate and direct deposit of dispatch fee — handle at this stage. |
| Load Planner | Feature | 3 | A new planning surface for managing loads. Full scope TBD.<br>**Eng:** ⚠️ Amandeep has a sheet with the full spec. Prompt Simarpreet for Amandeep's sheet before any scoping or build work. |
| Profit/Loss tab (company page) | Feature | 3 | P&L tab on the company page. Specific content TBD — part of a planned set of standard reports. Ops strongly wants the tabbed company page UI extended with more report views over time. |

---

## ✅ Done

| Name | Type | Complexity | Description |
|------|------|------------|-------------|
| Stage 1: Load lifecycle / status | Feature | 3 | Core load creation, dispatch, and status management through delivered/cancelled. Pre-existing. |
| CDL # field label (was: LICENSE #) | Enhancement | 1 | Changed driver form field label from "LICENSE #" to "CDL #". Shipped 2026-05-20. |
| "Assigned To" — Add "Other" option | Enhancement | 1 | Covers loads booked externally by owner or driver where a specific HB employee doesn't need to be tracked. Shipped 2026-05-20. |
| SMS copy: include Driver Instructions | Enhancement | 1 | Driver Instructions field now included in the SMS copy output. Shipped 2026-05-20. |
| SMS copy: include pickup & delivery details | Enhancement | 1 | SMS copy now includes pickup facility name + full address and delivery facility name + full address. Shipped 2026-05-20. |
| Home button | Enhancement | 1 | Added home/dashboard button to navigation. Shipped 2026-05-20. |
| Coming Soon tabs: Insurance + Inspections (company page) | Enhancement | 1 | Disabled placeholder tabs added to company sub-tab UI. Full Insurance feature is now a separate pending item. Shipped 2026-05-20. |
| Truck: additional fields | Enhancement | 1 | Added License Plate #, Vehicle Make, Model Year, Registration State (all optional). Shipped 2026-05-20. |
| Trailer: additional fields + ownership type | Enhancement | 1 | Added Make, Model Year, License Plate # (all optional). Ownership Type: Own / Rent / Leased. Shipped 2026-05-20. |
| Factoring Rate Tracking (date-effective history) | Feature | 2 | Each carrier supports multiple date-effective factoring rates. Loads invoiced at the rate in effect on pickup date. Shipped 2026-05-22. |
| Dispatch Rate Tracking (date-effective + free-look period) | Feature | 2 | Date-effective dispatch rates with 30-day free-look window from first pickup. System notifies staff when 5% rate begins. Shipped 2026-05-22. |
| Stage 2: Proof of Delivery (POD) | Feature | 2 | New `/pods` page + panel on load detail. Dispatcher confirms POD receipt and upload before load progresses to invoicing. Shipped 2026-05-22. |
| Stage 3: Invoicing | Feature | 3 | Full invoicing workflow: Factoring (Invoice Created → Invoice Funded → Ready for HB Billing), Quick Pay (same lifecycle, per-load charges %), Direct Billing (Awaiting Payment → Invoice Funded → Ready for HB Billing + weekly email reminder). `/invoicing` page, load detail panel, company lead, weekly email cron. Shipped 2026-05-22. |

---

## ⏭ Deferred

| Name | Type | Complexity | Description |
|------|------|------------|-------------|
| Haulbase logo / branding | Enhancement | 1 | Deferred to SaaS productization — branding will be handled holistically at that stage, not as a one-off patch. Deferred 2026-05-20. |
