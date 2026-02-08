# ReceiptLog Implementation Plan

## Problem Statement
Build a Progressive Web App (PWA) for Greek consumers to scan QR codes from receipts, fetch detailed invoice data from myDATA-compliant sources, and store/categorize purchases for personal finance tracking.

## Proposed Approach
Parallel development across three major workstreams:
1. **Schema Research & Data Model**: Study myDATA InvoicesDoc XML schema, design data extraction and storage models
2. **PWA Core**: Build React-based PWA with QR scanning, Firebase auth, and Firestore database
3. **Integration Layer**: Create adapter system for AADE standard, epsilondigital, and entersoft APIs

## Progress Summary

**Completed:**
- ✅ Created comprehensive implementation plan (46 beads total)
- ✅ Established proper inter-dependencies between beads
- ✅ Committed plan.md to ReceiptLog project repository
- ✅ Tested epsilondigital API and confirmed myDATA XML format
- ✅ Organized beads into 6 phases with P1/P2 priorities

**Current Status:** Ready to begin implementation. All work is tracked in beads and can be resumed at any time.

## Workplan

### Phase 1: Foundation & Research
- [ ] Study myDATA InvoicesDoc XML schema from public documentation (try-beads-43e)
  - [ ] Document schema structure (InvoicesDoc → Invoice → InvoiceDetails) (try-beads-43e)
  - [ ] Identify key fields for receipt itemization (products, quantities, prices, taxes) (try-beads-8ur)
  - [ ] Map merchant categorization fields (ΑΔΑΜ, ΑΦΜ, activity codes) (try-beads-3r0)
  - [ ] Research EN16831 European standard integration (try-beads-5h0)
- [ ] Analyze existing merchant APIs
  - [x] Test epsilondigital Sklavenitis API (GetInvoiceDocDetailed endpoint) - **Confirmed working**
  - [ ] Document epsilondigital format thoroughly (try-beads-asm)
  - [ ] Research entersoft e-invoicing.gr format (try-beads-rn4)
  - [ ] Document AADE QR code URL structure with /EN16831/ parameter (existing: try-beads-mio)
  - [ ] Identify commonalities and differences between formats
- [ ] Design unified data model
  - [ ] Create Receipt entity schema for Firestore (try-beads-4lh)
  - [ ] Design item-level detail structure (products, prices, taxes)
  - [ ] Plan merchant metadata storage (name, VAT, activity type)
  - [ ] Define categorization taxonomy (groceries, dining, transport, etc.)

### Phase 2: PWA Setup & Core Features  
- [ ] Initialize React PWA project (existing bead: try-beads-lsn)
  - [ ] Set up Create React App with PWA template
  - [ ] Configure build tools (Webpack/Vite)
  - [ ] Set up linting and code formatting
- [ ] Configure Firebase infrastructure (existing bead: try-beads-is8)
  - [ ] Create Firebase project
  - [ ] Enable Google Authentication
  - [ ] Set up OAuth credentials
  - [ ] Configure security rules for Firestore
- [ ] Design Firestore database schema (existing bead: try-beads-6ll)
  - [ ] Define collections structure (users, receipts, merchants, categories)
  - [ ] Plan indexing strategy for queries
  - [ ] Estimate storage costs (medium usage: ~200 receipts/user/month)
  - [ ] Design data retention and archival strategy
- [ ] Implement QR code scanner (existing bead: try-beads-bax)
  - [ ] Integrate html5-qrcode library
  - [ ] Add camera controls (zoom, flash, focus)
  - [ ] Handle QR code parsing and URL extraction
  - [ ] Add error handling for invalid QR codes
- [ ] Create PWA manifest and service worker (existing bead: try-beads-jlo)
  - [ ] Configure manifest.json (icons, theme, display mode)
  - [ ] Implement service worker for offline capability
  - [ ] Add install prompts for mobile devices
  - [ ] Plan cache strategy for static assets

### Phase 3: Data Integration & Processing
- [ ] Build merchant adapter system
  - [ ] Define adapter interface (fetch, parse, transform)
  - [ ] Implement AADE standard adapter
    - [ ] Parse QR code URLs with /EN16831/ parameter (bead: try-beads-mio)
    - [ ] Fetch XML data from AADE endpoints
    - [ ] Handle both direct display and download responses (bead: try-beads-xsu)
  - [ ] Implement epsilondigital adapter
    - [ ] Support GetInvoiceDocDetailed API format
    - [ ] Extract structured data from Sklavenitis receipts
    - [ ] Map to unified Receipt model
  - [ ] Implement entersoft adapter
    - [ ] Research e-invoicing.gr API endpoints
    - [ ] Parse entersoft XML format
    - [ ] Map to unified Receipt model
- [ ] Build XML parser and validator
  - [ ] Parse myDATA InvoicesDoc XML schema
  - [ ] Extract invoice header (date, merchant, total, taxes)
  - [ ] Extract line items (products, quantities, unit prices, VAT)
  - [ ] Validate against schema rules
  - [ ] Handle parsing errors gracefully
- [ ] Implement data transformation pipeline
  - [ ] Transform XML to JavaScript objects
  - [ ] Normalize merchant data (clean names, extract VAT numbers)
  - [ ] Calculate derived fields (subtotals, tax breakdowns)
  - [ ] Prepare for Firestore storage

### Phase 4: Categorization & Intelligence
- [ ] Investigate myDATA REST API (existing bead: try-beads-t9q)
  - [ ] Explore mydatapi.aade.gr REST endpoints
  - [ ] Retrieve merchant activity type data
  - [ ] Map ΚΒΕΔ (activity codes) to categories
- [ ] Research RequestDocs method (existing bead: try-beads-21j)
  - [ ] Document RequestDocs API capabilities
  - [ ] Plan integration for bulk receipt fetching
  - [ ] Evaluate use cases for ReceiptLog
- [ ] Implement automatic categorization
  - [ ] Map merchant activity types to spending categories
  - [ ] Use merchant name patterns for initial categorization
  - [ ] Allow user overrides and manual categorization
  - [ ] Learn from user corrections (future: ML model)

### Phase 5: User Interface & Experience
- [ ] Design receipt list view
  - [ ] Display receipts chronologically
  - [ ] Show merchant, date, total amount
  - [ ] Add filtering by date range, category, merchant
  - [ ] Implement search functionality
- [ ] Create receipt detail view
  - [ ] Show full invoice header information
  - [ ] Display itemized line items in table
  - [ ] Show tax breakdown
  - [ ] Include merchant details and VAT number
  - [ ] Allow category editing
- [ ] Build scanning flow
  - [ ] Camera view with QR scanner overlay
  - [ ] Show scanning feedback (success/error)
  - [ ] Display loading state while fetching data
  - [ ] Show preview before saving
- [ ] Add analytics and insights
  - [ ] Monthly spending totals
  - [ ] Category breakdowns (pie/bar charts)
  - [ ] Merchant frequency
  - [ ] Tax summary for deductions

### Phase 6: Infrastructure & Operations
- [ ] Deploy to Firebase Hosting (existing bead: try-beads-286)
  - [ ] Configure Firebase Hosting
  - [ ] Set up deployment pipeline
  - [ ] Configure custom domain (optional)
  - [ ] Enable HTTPS
- [ ] Cost analysis and optimization
  - [ ] Calculate Firestore read/write costs (medium usage)
  - [ ] Estimate Cloud Storage costs (if storing XML)
  - [ ] Plan Firebase Authentication costs
  - [ ] Evaluate Cloud Functions needs (future: background processing)
  - [ ] Set up budget alerts
- [ ] Monitoring and error tracking
  - [ ] Integrate Firebase Analytics
  - [ ] Set up error logging (Sentry or Firebase Crashlytics)
  - [ ] Monitor QR scan success rates
  - [ ] Track API integration health
- [ ] Security and privacy
  - [ ] Review Firestore security rules
  - [ ] Implement user data isolation
  - [ ] Add data export capability (GDPR compliance)
  - [ ] Handle sensitive VAT/tax data appropriately

## Notes and Considerations

### myDATA Schema Specifics
- **InvoicesDoc** is the root element containing one or more Invoice elements
- Each **Invoice** has header fields (issuer, counterpart, dates, totals) and **InvoiceDetails** with line items
- **EN16831** is the European e-invoicing standard that myDATA aligns with
- Tax calculation involves multiple VAT rates (24%, 13%, 6%, 0%) and special categories

### Technical Decisions
- **React PWA**: Chosen for cross-platform support (iOS/Android) without app store requirements
- **Firebase**: Provides authentication, database, and hosting in one ecosystem
- **Firestore**: Document database suitable for receipt storage with flexible schema
- **Offline-first**: Service worker will cache receipts for offline viewing (nice-to-have)

### Cost Estimates (Medium Usage: 200 receipts/user/month, 100 users)
- **Firestore**: ~20K writes/month + 60K reads/month ≈ $1-2/month
- **Firebase Authentication**: Free tier covers most personal use cases
- **Firebase Hosting**: Free tier (10GB transfer/month) likely sufficient initially
- **Total**: $1-5/month for medium usage, scales with user base

### Merchant Integration Strategy
1. **Start with standards**: AADE myDATA is the foundation
2. **Add merchant APIs**: epsilondigital and entersoft provide richer data (product details)
3. **Adapter pattern**: Each merchant has its own adapter implementing common interface
4. **Fallback**: If merchant API fails, fall back to AADE standard endpoint
5. **Future**: Add more merchants as needed (AB Vasilopoulos, Masoutis, etc.)

### Security Considerations
- Receipts contain sensitive financial data (VAT numbers, purchase history)
- Firestore rules must ensure users only access their own data
- QR codes may expose temporary URLs - consider rate limiting
- epsilondigital URLs contain document IDs - ensure we're not storing credentials

### Future Enhancements (Out of Scope for Initial Plan)
- Machine learning for smarter categorization
- Receipt OCR for non-QR receipts
- Export to accounting software (QuickBooks, Xero)
- Shared receipts for household tracking
- Budget alerts and notifications
- Integration with bank transactions for reconciliation

## Dependencies
- React 18+
- Firebase SDK (Authentication, Firestore, Hosting)
- html5-qrcode library
- XML parsing library (fast-xml-parser or xmldom)
- Chart library for analytics (recharts or Chart.js)

## Success Criteria
1. User can scan a receipt QR code and view itemized data
2. Data is correctly parsed from AADE, epsilondigital, and entersoft formats
3. Receipts are stored securely in Firestore
4. PWA works offline with cached receipts
5. Automatic categorization achieves >70% accuracy
6. Infrastructure costs stay under $10/month for 100 users
