# ReCommerce Automation Pipeline

**AI-Powered Reverse Logistics Automation**

Automate your reCommerce warehouse intake, listing generation, and inventory tracking. Transform manual processes into a scalable, AI-driven pipeline that scales from 10 → 50-100 listings per day.

---

## 📊 Quick Wins

| Metric | Current | Level 1 | Level 2 |
|--------|---------|---------|---------|
| **Time per batch (20 items)** | 4+ hours | 45 min | 30 min |
| **Listings per 4-hour shift** | ~10 | 26-30 | 50-100 |
| **Staff role** | Creation | Validation | Automated + validation |
| **Warehouse tracking** | Manual spreadsheet | Spreadsheet + barcode ready | Real-time QR scanning |
| **Cost/month** | N/A | $10-30 | $25-60 |

---

## 🎯 Current Problem

Client’s warehouse currently:
- ✗ Takes **4+ hours** to process 20 items (1 batch)
- ✗ Requires staff to **manually write titles, descriptions, and pricing** (20 min per item)
- ✗ Produces **~10 listings/day** at capacity
- ✗ Has **zero warehouse visibility** into item locations and hold times
- ✗ Struggles with **90-day liquidation rule** (manual tracking only)

### Current Workflow (Cloud Folder Method)

```
┌─────────────────────────────────────────────────────────────────┐
│                     CURRENT METHOD                               │
├─────────────────────────────────────────────────────────────────┤
│                                                                   │
│  1. Take photos               → 5 min/batch                      │
│  2. Upload to Google Drive    → 5 min/batch                      │
│  3. Manual review & write     → 20 min/item ⚠️ BOTTLENECK       │
│  4. Price lookup (manual)     → 5 min/item                       │
│  5. Create drafts (manual)    → 5 min/item                       │
│  6. Print labels              → 5 min/batch                      │
│                                                                   │
│  ⏱️  TOTAL TIME: 4+ hours for 20 items                           │
│  📦 OUTPUT: ~10 listings/day                                     │
│  🧑‍💼 EFFORT: Labor-intensive (staff writing descriptions)        │
│                                                                   │
└─────────────────────────────────────────────────────────────────┘
```

**The Problem:** Staff are spending 80% of their time *creating* content (writing titles, descriptions, searching prices). Only 20% is actual logistics work.

---

## ✨ Solution: Level 1 (Improved Workflow)

**Shift staff from creating to validating.**

Instead of writing descriptions from scratch (20 min), staff review AI-generated ones (1 min). AI does the heavy lifting in parallel while they process the next batch.

### Improved Workflow (Custom Intake Form + AI)

```
┌──────────────────────────────────────────────────────────────────┐
│                   IMPROVED METHOD                                 │
├──────────────────────────────────────────────────────────────────┤
│                                                                    │
│  1. Mobile Intake Form           → 3 min/item                    │
│     (Photos + category + condition)                              │
│                                                                    │
│  2. Auto Cloud Upload            → Instant                       │
│     (Triggers AI pipeline in background)                         │
│                                                                    │
│  3. AI Analysis (PARALLEL) ⚡    → 2 min/batch total             │
│     ✓ Generate SEO title (60 chars)                              │
│     ✓ Create description from photos                             │
│     ✓ Lookup pricing (eBay market research)                      │
│     ✓ Assign category + confidence threshold                     │
│                                                                    │
│  4. Staff Quick Review            → 1 min/item                   │
│     (Accept/edit AI output, not creating)                        │
│                                                                    │
│  5. Auto Draft Creation           → Instant                      │
│     (eBay Listing API + Etsy Open API v3)                        │
│                                                                    │
│  6. Thermal Printer Auto          → Instant                      │
│     (One-click print, no manual label creation)                  │
│                                                                    │
│  ⏱️  TOTAL TIME: ~45 min for 20 items (89% reduction)           │
│  📦 OUTPUT: 26-30 listings/day (3x faster)                       │
│  🎯 EFFORT: Review-based (fast, human eyes only on edge cases)   │
│                                                                    │
└──────────────────────────────────────────────────────────────────┘
```

### Architecture: Level 1 Stack

```
┌─────────────┐
│   Intake    │
│   Form      │ (Airtable or Flask web app)
│ (iPad/Web)  │
└──────┬──────┘
       │
       ├─→ Google Drive (photo auto-upload)
       │
       ├─→ Google Sheets (source of truth)
       │
       ├─→ Python Polling Script (local machine)
       │   ├─→ Claude API (Sonnet)
       │   │   ├─ Vision: Item ID, condition, damage flags
       │   │   ├─ LLM: SEO title (60-70 chars), description
       │   │   └─ LLM: Pricing lookup (eBay market research)
       │   │
       │   └─→ eBay Listing API (createItemDraft)
       │   └─→ Etsy Open API v3 (createDraft)
       │
       └─→ Thermal Printer (via Python script)
           └─ Label: SKU + barcode + title + price + date
```

### Timeline: Level 1 (4 weeks)

| Week | Milestone | Deliverable |
|------|-----------|-------------|
| 1-2 | **Intake Form Setup** | Airtable/Flask form + Google Drive auto-upload + Claude API integration (vision + title/description generation) |
| 3-4 | **API Integration** | eBay & Etsy draft creation, thermal printer script, category routing logic (LLM confidence threshold) |
| 5 | **Testing & Training** | End-to-end test with 20 items, warehouse staff training on new intake form |
| 6+ | **Launch & Measure** | Deploy to production, track metrics (target: 26-30 items/day within week 1) |

**Effort:** ~40-60 developer hours (can be done in 2-3 weeks with one developer)

**Cost:** $10-30/month (Claude API only)

---

## 🚀 Solution: Level 2 (Advanced B2B Liquidation Platform)

**Full automation with warehouse tracking, barcode scanning, and liquidation rules.**

Builds on Level 1 by adding AI condition scoring, barcode generation, thermal printer automation, warehouse inventory dashboard, and automated liquidation decisions.

### Advanced 8-Stage Pipeline

```
Stage 1: Mobile Intake Form
│
├─ Photos (5-8 angles)
├─ Category dropdown
├─ Condition checkboxes
├─ Weight/size estimate
│
↓
├─→ Google Drive (auto-upload)
├─→ Google Sheets (new row created)
│
↓
Stage 2: AI Condition Assessment
│
├─ Claude Vision API analyzes photos
├─ Condition score (1-10)
├─ Damage detection (bounding boxes on UI)
├─ Missing pieces flagged
├─ Staff confirms or overrides
│
↓
Stage 3: Viability Filter (Economics Decision Tree)
│
├─ Price lookup (eBay market research tool)
├─ Calculate turnover velocity
├─ Rule engine:
│  ├─ IF price < $50 AND bulky AND slow → "Do Not List"
│  ├─ IF $35-50 range → Must sell within 30 days
│  ├─ IF ≥$50 OR small/valuable → "Worth Listing"
│  ├─ Else → "Hold for 60+ days"
│
├─→ 3 branches:
│   ├─ Do Not List → Donation/Recycling
│   ├─ Worth Listing → Continue to Stage 4
│   └─ Liquidation Candidate → Bundle/Bulk Buyer workflow
│
↓ (for "Worth Listing" path)
Stage 4: Metadata & SKU Generation
│
├─ SEO title (60-70 chars)
├─ Category assignment (with confidence threshold)
├─ Description (with condition details)
├─ Etsy tags (10 keywords + item specifics)
├─ Unique SKU (auto-incrementing)
├─ Shipping dimensions (accounting for packaging: +10cm or +1kg)
│
↓
Stage 5: Draft Creation (APIs)
│
├─ eBay Listing API → createItemDraft
│   └─ Routes to correct account by category
│
├─ Etsy Open API v3 → createDraft
│
├─→ All data in Google Sheets (source of truth)
│
↓
Stage 6: Barcode Generation & Thermal Printer Automation
│
├─ Generate QR code from SKU
├─ Python script generates label:
│   ├─ SKU + barcode/QR
│   ├─ Item title (truncated to fit)
│   ├─ Listing date
│   ├─ Price
│   └─ Shipping dimensions
│
├─→ Python script sends to thermal printer (USB/network)
│
├─→ Zebra/Dymo thermal printer outputs label
│   └─ Staff affixes label to item
│
↓
Stage 7: Warehouse Intake & Barcode Scanning
│
├─ Staff scans QR code on item (mobile app or iPad)
├─ Bin location auto-updates in Google Sheets
├─ Inventory dashboard shows:
│   ├─ Item ID + title
│   ├─ SKU + bin location
│   ├─ List date + hold duration
│   ├─ Listing platforms (eBay, Etsy, Shopify, Facebook)
│
├─ System automatically flags items for liquidation
│   └─ After 90 days: Send to bulk buyer or donation
│
↓
Stage 8: Human Review & Multi-Platform Publish
│
├─ Seller reviews eBay draft on eBay.com
├─ Seller reviews Etsy draft on Etsy.com
├─ Final edits if needed
├─ Hit "Publish" on each platform
│
├─→ Auto cross-list to:
│   ├─ Shopify (CSV export or API)
│   └─ Facebook Marketplace (manual or CSV)
│
├─→ Inventory sheet updated with "published_date" & platforms
│
└─→ Item tracked in warehouse (barcode scanning)

═════════════════════════════════════════════════════════
RESULT: 50-100 items/day | Real-time warehouse visibility
        Automated 90-day liquidation | Condition quality control
═════════════════════════════════════════════════════════
```

### Key Features: Level 2

- **AI Condition Assessment:** Damage detection with photo annotation (bounding boxes for staff review)
- **Viability Filter:** Automated economics decision tree (keep/hold/liquidate)
- **Barcode + QR:** Full warehouse tracking with bin locations
- **Liquidation Automation:** 90-day hold rule enforced automatically
- **Inventory Dashboard:** Turnover velocity, hold duration, automated picks
- **Returns Reduction:** AI condition flags reduce surprise customer returns

### Timeline: Level 2 (8 weeks after Level 1)

| Week | Milestone | Deliverable |
|------|-----------|-------------|
| 8-9 | **Barcode & QR Setup** | Python barcode library, QR code generation, mobile scanning app integration |
| 9-10 | **Dashboard Design** | Inventory UI (Airtable Pro or custom), turnover tracking, liquidation automation rules |
| 10-11 | **Thermal Printer Driver** | Upgrade to Zebra/Dymo if needed, driver integration, label template design |
| 11+ | **Testing & Training** | End-to-end test with barcode scanning, staff training on warehouse workflow |

**Effort:** ~60-80 developer hours (3-4 weeks additional)

**Cost:** $200-450 hardware (one-time) + $25-60/month operating

---

## 💰 Tech Stack & Costs

### Level 1: Custom Intake Form + AI Validation

| Component | Solution | Cost | Notes |
|-----------|----------|------|-------|
| **Intake Form** | Airtable Free OR Flask web app | $0-10/mo | Simple, no backend needed |
| **Photo Storage** | Google Drive API OR AWS S3 free tier | $0 | Auto-upload from form |
| **AI Enrichment** | Claude API (Sonnet) | $10-20/mo | ~100 items/day = $20/mo |
| **Data Store** | Google Sheets (free) | $0 | Source of truth for inventory |
| **Drafts API** | eBay Listing API + Etsy Open API v3 | $0 | Free, within ToS for drafts |
| **Thermal Printer** | Python script (local machine) | $0 | You already have the printer |
| **Hosting** | Local machine (Python polling script) | $0 | Runs 24/7 on warehouse computer/Pi |

**Level 1 Total: $10-30/month (operating) + $0 (setup)**

### Level 2: Advanced B2B + Barcode + Dashboard

| Component | Solution | Cost | Notes |
|-----------|----------|------|-------|
| **Barcode Generation** | Python barcode library (free) | $0 | QR codes from SKU |
| **Thermal Printer Driver** | Zebra/Dymo upgrade (optional) | $200-400 | One-time if upgrading from current printer |
| **QR Scanning** | Mobile app (iOS/Android) | $0-50/mo | Or free open-source scanner + custom UI |
| **Inventory Dashboard** | Airtable Pro OR custom UI (Vercel) | $20-50/mo | Airtable Pro = easiest; custom UI = more control |
| **Light Hosting** | Linode/DigitalOcean VPS (optional) | $5-10/mo | Only if printer daemon runs in cloud |
| **API Monitoring** | Sentry/LogRocket (optional) | $0 | Free tier sufficient |

**Level 2 Total: $25-60/month (operating) + $200-450 (hardware, one-time)**

---

## 🛠️ Getting Started

### Prerequisites

- Python 3.9+
- Google Drive account (for file storage)
- Google Sheets account (source of truth)
- eBay API credentials (free)
- Etsy API credentials (free)
- Claude API key (from Anthropic)
- Thermal printer (USB or network connected)

### Quick Start: Level 1

#### 1. Set Up Intake Form (Airtable or Flask)

**Option A: Airtable (No-code, 10 min setup)**

```
1. Create new Airtable base
2. Add table: "New Items"
   ├─ Fields:
   │  ├─ Photo URL (attachment)
   │  ├─ Category (single select)
   │  ├─ Condition (single select)
   │  ├─ Status (single select: Pending, Reviewed, Listed)
   │  └─ Notes (long text)
3. Create form view → Share with warehouse staff
4. Enable Zapier automation → Triggers Google Drive upload
```

**Option B: Flask (Custom, 30 min setup)**

```bash
git clone https://github.com/your-org/recommerce-intake.git
cd recommerce-intake
pip install -r requirements.txt
python app.py
# Visit http://localhost:5000
```

#### 2. Configure Google Drive & Sheets

```bash
# Install Google client libraries
pip install google-api-python-client google-auth-oauthlib google-auth-httplib2

# Authenticate (one-time)
python setup_google_auth.py
```

Google Sheets template: [Link to template]
Google Drive folder structure:
```
ReCommerce/
├─ New Items (auto-upload from form)
├─ Listed Items (archive)
└─ Donation/Liquidation
```

#### 3. Run the AI Pipeline (Local Python Script)

```bash
# Install dependencies
pip install anthropic google-api-python-client pillow requests

# Configure environment
export CLAUDE_API_KEY="sk-..."
export GOOGLE_SHEETS_ID="your-sheet-id"
export EBAY_API_KEY="your-key"
export ETSY_API_KEY="your-key"

# Run polling script (runs 24/7)
python pipeline.py
```

What it does:
1. Checks Google Sheets every 5 min for new rows with Status="Pending"
2. Downloads photos from Google Drive
3. Calls Claude Vision API for item analysis
4. Generates title, description, pricing recommendations
5. Updates Google Sheets with results
6. Creates eBay/Etsy drafts via APIs
7. Generates thermal printer labels

#### 4. Set Up Thermal Printer Script

```bash
# Install printer driver (for Zebra/Dymo)
pip install zebra-zpl

# Test print
python print_label.py --test-sku SKU001
# Output: Label prints from thermal printer
```

#### 5. Train Warehouse Staff

- Show staff the mobile intake form
- Walk through: photo upload → category → condition → submit
- Show how to review AI-generated titles/descriptions in Google Sheets
- Demo: Click "Print Label" → label prints
- Demo: Apply label to item → file in warehouse location

**Estimated setup time: 2-3 days (with developer)**

---

## 📖 API Reference

### Claude Vision API (Item Analysis)

```python
from anthropic import Anthropic

client = Anthropic()

response = client.messages.create(
    model="claude-opus-4",
    max_tokens=500,
    messages=[{
        "role": "user",
        "content": [
            {
                "type": "image",
                "source": {
                    "type": "base64",
                    "media_type": "image/jpeg",
                    "data": base64_image,
                },
            },
            {
                "type": "text",
                "text": """Analyze this item for resale. Provide:
1. Item ID (brand, model, category)
2. Condition (1-10)
3. Damage flags (none, minor, major)
4. Suggested SEO title (60 chars max)
5. Suggested description (100-150 words)
6. Recommended price range (eBay comps)

Format as JSON."""
            }
        ],
    }]
)

print(response.content[0].text)
```

### eBay Listing API (Draft Creation)

```python
import ebay_api

ebay_client = ebay_api.Client(credentials=ebay_key)

# Create draft
draft = ebay_client.create_item_draft({
    "title": "Vintage Sony Walkman, Excellent Condition",
    "description": "...",
    "price": 45.00,
    "category": "Electronics > Portable Audio",
    "condition": "Like New",
    "item_specifics": {...}
})

print(f"Draft created: {draft.get('draftId')}")
# Staff reviews on eBay.com, then publishes
```

### Etsy Draft API (Draft Creation)

```python
import etsy_api

etsy_client = etsy_api.EtsyClient(api_key=etsy_key)

# Create draft
draft = etsy_client.createDraft({
    "title": "Vintage Sony Walkman",
    "description": "...",
    "price": 45.00,
    "category": "Vintage Electronics",
    "tags": ["vintage", "walkman", "1980s", ...],
    "inventory": {"quantity": 1}
})

print(f"Draft created: {draft['listing_id']}")
```

### Thermal Printer (Label Generation)

```python
from zebra import Zebra
from barcode import generate

# Generate QR code
generate('qrcode', 'SKU001', output='/tmp/label')

# Send to thermal printer
zpl_commands = f"""
^XA
^FO50,50^BY3^BQN,2,10
^FDHA,SKU001^FS
^FO50,200^A0N,25,24^FDSKU001^FS
^FO50,250^A0N,20,20^FDVintage Sony Walkman^FS
^FO50,300^A0N,20,20^FD$45.00^FS
^FO50,350^A0N,15,15^FD2026-09-09^FS
^XZ
"""

printer = Zebra(queue_name="thermal_printer")
printer.output(zpl_commands)
```

---

## 📊 Metrics & Monitoring

Track these KPIs to measure success:

### Level 1 Targets (Weeks 1-6)

- ✓ Items processed per 4-hour shift: 10 → 26-30
- ✓ Time per item: 20+ min → 1-3 min
- ✓ Manual draft creation eliminated
- ✓ Staff training completed (100%)
- ✓ Uptime: 99% (polling script runs 24/7)

### Level 2 Targets (Weeks 8-12)

- ✓ Items with barcode scanning: 0% → 100%
- ✓ Warehouse lookup time: 10+ min → instant
- ✓ 90-day liquidation rule automated (0 manual cases)
- ✓ Items per shift: 26-30 → 50-100
- ✓ Returns due to condition mismatch: baseline → 50% reduction

### Dashboard (Airtable or custom)

```
Daily Metrics:
├─ Items listed today (target: 26-30 for Level 1, 50+ for Level 2)
├─ Average time per item (target: < 3 min with AI)
├─ eBay/Etsy draft approval rate (target: >90%)
├─ QR scanning success rate (Level 2, target: >98%)
│
Weekly Metrics:
├─ Total items listed
├─ Avg hold duration (target: <30 days for "worth listing")
├─ Items liquidated (90-day rule)
├─ Returns/refunds (target: trending down)
│
Monthly Metrics:
├─ Items → revenue (top performers)
├─ Turnover velocity by category
├─ Cost per listing (target: <$2)
└─ ROI of automation vs manual
```

---

## 🐛 Troubleshooting

### Claude API Rate Limits

If you hit rate limits:

```python
import time
from anthropic import RateLimitError

for item in items:
    try:
        response = client.messages.create(...)
    except RateLimitError:
        time.sleep(60)  # Wait 1 min, retry
        response = client.messages.create(...)
```

**Solution:** Batch processing (process items in groups of 5-10 with 30-sec delays between batches)

### Thermal Printer Not Found

```bash
# Check connected printers
lpstat -p -d

# If Zebra printer not listed, install driver
sudo apt install printer-driver-zebra  # Linux
# Or use macOS Printer & Scanner settings
```

### eBay/Etsy Draft Not Creating

- **Check:** API credentials are correct
- **Check:** Rate limits (both APIs have request limits)
- **Check:** Item data matches required fields (title, price, category)
- **Solution:** Log all API responses to Google Sheets for debugging

---

## 📚 Documentation

- [Claude Vision API Docs](https://docs.anthropic.com/vision)
- [eBay Listing API Guide](https://developer.ebay.com/api-docs/sell/listing/overview.html)
- [Etsy API Reference](https://developers.etsy.com/documentation)
- [Google Sheets API Setup](https://developers.google.com/sheets/api)
- [Thermal Printer ZPL Reference](https://www.zebra.com/content/dam/zebra_new_ia/en_us/solutions-industry/product/software/link-os/zpl-command-reference.pdf)

---

## 🤝 Contributing

This is a team project. To contribute:

1. Fork the repo
2. Create a feature branch (`git checkout -b feature/your-feature`)
3. Commit changes (`git commit -am 'Add feature'`)
4. Push to branch (`git push origin feature/your-feature`)
5. Open a Pull Request

### Development Setup

```bash
git clone https://github.com/your-org/recommerce-automation.git
cd recommerce-automation
python -m venv venv
source venv/bin/activate  # or `venv\Scripts\activate` on Windows
pip install -r requirements.txt
```

Run tests:
```bash
pytest tests/
```

---

## 📝 License

This project is internal to [Your Company]. Not for public distribution.

---

## ❓ FAQ

**Q: Will AI miss items or mislabel them?**
A: No system is perfect, but Claude Vision is 90%+ accurate on common items. Staff review every AI-generated title/description before publishing. Low-confidence items (< 70% category confidence) are flagged for manual review.

**Q: Can we start with just Level 1?**
A: Yes! Level 1 is designed to run independently. You can add Level 2 (barcode + dashboard) later without rebuilding.

**Q: How long does it take to get 26-30 items/day?**
A: 4-6 weeks from start to hitting targets. First 2 weeks are setup, last 4 are testing + staff training + iteration.

**Q: What if we want to add Shopify or Facebook Marketplace?**
A: Level 1 generates drafts on eBay/Etsy. Adding Shopify/Facebook requires CSV export or light API integration (1-2 weeks of additional work). Level 2 dashboard can auto-list across all platforms.

**Q: Do we need a developer?**
A: Level 1 can be built by one developer in 2-3 weeks. If you don't have one, budget $5-10k for contract development.

**Q: Can we run this on a Raspberry Pi?**
A: Yes! The polling script is lightweight (~50MB RAM). A Pi 4 can run it 24/7 for ~$50 one-time cost.

---

## 📞 Contact

- **Project Lead:** [JEIN JOSH ]
- **Engineering:** [JEIN]


---


