# PMF_Google_Ads_Setup

Product-market fit Google Ads setup: battle-tested campaign structure, bidding strategy, and conversion tracking for early-stage products.

![Google Ads](https://img.shields.io/badge/Google_Ads-Expert-blue?style=flat&labelColor=555) ![PPC](https://img.shields.io/badge/PPC-PMF-green?style=flat&labelColor=555) ![Stage](https://img.shields.io/badge/Stage-Early_PMF-orange?style=flat&labelColor=555) ![License](https://img.shields.io/badge/License-MIT-yellow?style=flat&labelColor=555)

[Concepts](#-concepts) · [How It Works](#-how-it-works) · [Install](#-install) · [Usage](#-usage) · [Config](#-configuration) · [Tips](#-tips-and-tricks-12) · [Troubleshooting](#-troubleshooting) · [Architecture](#-architecture) · [Startups](#️-startups--businesses)

---

## 🧠 CONCEPTS

| Feature | Location | Description |
|---|---|---|
| PMF Campaign Blueprint | `campaigns/pmf_structure.json` | 3-campaign structure: Brand + Competitor + Broad Intent |
| Bidding Ladder | `bidding/ladder.md` | Manual CPC → tCPA → tROAS progression with trigger conditions |
| Conversion Stack | `tracking/conversion_stack.js` | GTM-based: form submit + phone + scroll depth + session quality |
| Search Term Mining | `scripts/search_term_miner.py` | Auto-pulls SQR data, clusters intent, flags negatives |
| Quality Score Tracker | `scripts/qs_tracker.py` | Daily QS pull per keyword — alerts on drops below 6 |
| Budget Pacing | `scripts/budget_pacing.py` | Hourly spend check, adjusts if pacing over/under |
| Ad Copy Matrix | `copy/matrix.md` | RSA headline × description combinations by intent stage |
| Negative Keyword Library | `negatives/master.csv` | 400+ PMF-stage negatives — job seekers, researchers, competitors |
| Audience Layers | `audiences/layers.json` | RLSA, customer match, similar audiences layered onto search |
| Landing Page Checklist | `landing/checklist.md` | 22-point PMF landing page audit before first dollar spent |
| Reporting Template | `reporting/weekly_template.xlsx` | Weekly PMF metrics: search impression share, CVR, CPL trend |
| Alert Rules | `alerts/rules.yaml` | Automated rules for budget, QS, CTR, and conversion rate anomalies |

### 🔥 Hot

| Feature | Location | Description |
|---|---|---|
| PMF Bidding Ladder | `bidding/ladder.md` | Never jump to Smart Bidding before 30 conversions — this doc defines exact triggers |
| Search Term Miner | `scripts/search_term_miner.py` | Weekly SQR clustering reveals real PMF signal from actual search demand |
| Conversion Stack | `tracking/conversion_stack.js` | Multi-touch conversion tracking — not just leads, but engaged leads |
| Negative Library | `negatives/master.csv` | 400+ negatives prevent PMF budget waste on non-buyers |
| Ad Copy Matrix | `copy/matrix.md` | RSA combinations tested against JTBD framework |

---

## ⚙️ HOW IT WORKS

```
PMF Stage Diagnosis
    │
    ▼
┌──────────────────────────────────────────────┐
│  PHASE 1: SIGNAL CAPTURE (Week 1-2)          │
│  Manual CPC · Broad Match · $20/day cap      │
│  Goal: find converting search terms          │
└──────────────────────────────────────────────┘
    │ (15+ conversions collected)
    ▼
┌──────────────────────────────────────────────┐
│  PHASE 2: STRUCTURE (Week 3-4)               │
│  SKAGs on winners · Pause losers             │
│  Negative list built from SQR                │
└──────────────────────────────────────────────┘
    │ (30+ conversions, tCPA stable)
    ▼
┌──────────────────────────────────────────────┐
│  PHASE 3: SCALE (Month 2+)                   │
│  tCPA → tROAS · Audience layers · PMax test  │
└──────────────────────────────────────────────┘
```

**3-Campaign PMF Structure:**
- **Campaign 1 — Brand Defense:** Branded keywords, manual CPC, protect CVR
- **Campaign 2 — Competitor Conquest:** Competitor brand terms, separate budget
- **Campaign 3 — Intent Capture:** High-intent non-brand terms, Broad → SKAG workflow

---

## 🚀 INSTALL

```bash
git clone https://github.com/hmzainjamil/PMF_Google_Ads_Setup
cd PMF_Google_Ads_Setup

# Python deps for scripts
pip install google-ads pandas openpyxl python-dotenv

# Google Ads API credentials
cp .env.example .env
# Fill: GOOGLE_ADS_CLIENT_ID, CLIENT_SECRET, REFRESH_TOKEN, DEVELOPER_TOKEN, CUSTOMER_ID

# Validate connection
python3 scripts/validate_connection.py

# GTM tracking code
# Copy tracking/conversion_stack.js → GTM Custom HTML tag
# Trigger: All Pages + Thank You page
```

---

## 📟 USAGE

```bash
# Pull search term report and cluster intent
python3 scripts/search_term_miner.py --days 30 --min-clicks 3

# Check quality scores — flag below 6
python3 scripts/qs_tracker.py --alert-threshold 6

# Budget pacing check
python3 scripts/budget_pacing.py --today

# Generate weekly report
python3 reporting/generate_weekly.py --output ~/Downloads/weekly_pmf.xlsx

# Apply negative keyword list to all campaigns
python3 scripts/apply_negatives.py --list negatives/master.csv

# Run full PMF health audit
python3 scripts/pmf_audit.py --account $CUSTOMER_ID

# Import campaign structure template
python3 scripts/import_structure.py --template campaigns/pmf_structure.json
```

---

## ⚙️ CONFIGURATION

| Variable | Default | Description |
|---|---|---|
| `GOOGLE_ADS_CUSTOMER_ID` | — | 10-digit Google Ads account ID |
| `PMF_DAILY_BUDGET_USD` | `30` | Total daily budget cap across all 3 campaigns |
| `BRAND_CAMPAIGN_SHARE` | `0.2` | Brand campaign budget share (20% of total) |
| `COMPETITOR_CAMPAIGN_SHARE` | `0.2` | Competitor conquest budget share |
| `INTENT_CAMPAIGN_SHARE` | `0.6` | High-intent campaign budget share |
| `TCPA_TRIGGER_CONVERSIONS` | `30` | Min conversions before switching to tCPA bidding |
| `QS_ALERT_THRESHOLD` | `6` | QS below this triggers Slack/email alert |
| `SQR_MIN_CLICKS` | `3` | Minimum clicks to include search term in mining |
| `NEGATIVE_AUTO_ADD_CTR` | `0.005` | CTR below 0.5% with 50+ impressions → auto-negative |
| `REPORT_LOOKBACK_DAYS` | `30` | Default lookback for reporting scripts |

---

## 💡 TIPS AND TRICKS (12)

[Bidding](#tips-bidding) · [Search Terms](#tips-sqr) · [Tracking](#tips-tracking) · [Scaling](#tips-scaling)

<a id="tips-bidding"></a>■ **Bidding Strategy (3)**

| Tip | Source |
|---|---|
| Never enable tCPA before 30 conversions in the last 30 days — Google's algorithm needs data | Google Ads Help |
| Set Manual CPC bids at 2× your target CPA to buy traffic initially — adjust down as QS rises | PMF bidding ladder |
| Use bid adjustments: +20% mobile if mobile CVR > desktop CVR in your niche | Google Ads docs |

<a id="tips-sqr"></a>■ **Search Term Mining (3)**

| Tip | Source |
|---|---|
| Run SQR weekly, not monthly — PMF-stage accounts accumulate spend fast in the wrong terms | Campaign blueprint |
| Cluster search terms by JTBD (Job-to-be-Done) intent — reveals real PMF signal | SQR miner script |
| Add exact-match negatives for competitor product names unless running conquest campaign | Negative library |

<a id="tips-tracking"></a>■ **Conversion Tracking (3)**

| Tip | Source |
|---|---|
| Track scroll depth >75% as a micro-conversion — early signal before form fills arrive | Conversion stack |
| Import CRM-verified leads as offline conversions — aligns Google optimization with actual revenue | Google Ads API |
| Use `session_quality` event (3+ pages + 2min+) as secondary conversion — prevents gaming | GTM setup |

<a id="tips-scaling"></a>■ **PMF Scaling (3)**

| Tip | Source |
|---|---|
| Don't scale budget until CAC stabilizes for 2 weeks — scaling broken economics kills runway | PMF audit script |
| Add RLSA layers at Phase 3 — past visitors convert 2-3× better, lower CPA | Audience layers |
| Test Performance Max only after tCPA stable — PMax needs conversion history to work | Google Ads docs |

---

## 🔧 TROUBLESHOOTING

| Issue | Fix |
|---|---|
| API auth error | Regenerate refresh token: `python3 scripts/refresh_token.py` |
| No conversions tracking | Check GTM preview — ensure conversion_stack.js tag firing |
| tCPA not stabilizing | Check for conversion lag — add 7-day attribution window |
| Budget under-delivery | Lower max CPC bids — over-restricted bids cause under-delivery |
| High CPC, low QS | Improve ad relevance + landing page experience for keyword |
| Smart Bidding regression | Revert to Manual CPC temporarily, rebuild conversion data |
| Search terms irrelevant | Apply negatives/master.csv — likely broad match without negatives |

---

## 📊 ARCHITECTURE

```
PMF_Google_Ads_Setup/
├── campaigns/
│   └── pmf_structure.json      # 3-campaign JSON template
├── bidding/
│   └── ladder.md               # Manual → tCPA → tROAS progression
├── tracking/
│   ├── conversion_stack.js     # GTM tag code
│   └── gtm_container.json      # Importable GTM container
├── scripts/
│   ├── search_term_miner.py    # SQR clustering
│   ├── qs_tracker.py           # Quality score monitoring
│   ├── budget_pacing.py        # Hourly budget check
│   ├── apply_negatives.py      # Bulk negative application
│   ├── pmf_audit.py            # Full account health audit
│   └── import_structure.py    # Campaign structure importer
├── copy/
│   └── matrix.md               # RSA headline × description matrix
├── negatives/
│   └── master.csv              # 400+ PMF-stage negatives
├── audiences/
│   └── layers.json             # RLSA + customer match config
├── landing/
│   └── checklist.md            # 22-point landing page audit
├── reporting/
│   ├── weekly_template.xlsx    # Weekly PMF report template
│   └── generate_weekly.py      # Report generator
└── alerts/
    └── rules.yaml              # Automated alert rules
```

---

## ☠️ STARTUPS / BUSINESSES

| This Repo / Feature | Replaced |
|---|---|
| PMF Campaign Blueprint | Hiring a PPC agency at $2k/mo before product validated |
| Bidding Ladder | Jumping to Smart Bidding before 30 conversions — burning budget |
| Negative Library | Wasting 30-40% of spend on job seekers and researchers |
| Conversion Stack | Tracking only form fills — missing 80% of PMF signal |
| Budget Pacing Script | End-of-month budget exhaustion surprise |
| QS Tracker | Paying 3× more per click due to unmonitored QS drops |
| SQR Miner | Manual weekly review in Google Ads UI — takes 2 hours |
| Landing Checklist | Launching ads to unoptimized pages — CVR below 1% |

---

## Star History

[![Star History Chart](https://api.star-history.com/svg?repos=hmzainjamil/PMF_Google_Ads_Setup&type=Date)](https://star-history.com/#hmzainjamil/PMF_Google_Ads_Setup&Date)

---
<div align="center">Built by <a href="https://github.com/hmzainjamil">HMZ</a> · Part of HMZ Claude AI System</div>

---

## 🔬 PMF METRICS TO TRACK

| Metric | Green | Yellow | Red |
|---|---|---|---|
| Search Impression Share | >30% | 15-30% | <15% |
| CTR (Search) | >5% | 3-5% | <3% |
| Quality Score (avg) | 7+ | 5-6 | <5 |
| Conversion Rate | >3% | 1-3% | <1% |
| Cost per Lead | Below target | 1-2× target | >2× target |
| Search Abs. Top IS | >20% | 10-20% | <10% |

---

## 📈 BIDDING LADDER QUICK REFERENCE

| Phase | Trigger | Strategy | Bid Constraint |
|---|---|---|---|
| 1 | Day 1 | Manual CPC | Start at $2-5 CPC |
| 2 | 15+ conversions | Enhanced CPC | Keep tCPA uncapped |
| 3 | 30+ conv / 30 days | Target CPA | Set tCPA at 1.2× current CPL |
| 4 | tCPA stable 2 weeks | Target ROAS | Set tROAS at 0.8× actual ROAS |
| 5 | tROAS stable 1 month | Performance Max | Run parallel, compare |

---

## 🏗️ CAMPAIGN NAMING CONVENTION

```
[Account] - [Type] - [Network] - [Match] - [Target]

Examples:
DigiMinds - Brand - Search - Exact - All
DigiMinds - Competitor - Search - Exact - HubSpot
DigiMinds - Intent - Search - Broad - PMF_V1
DigiMinds - Remarketing - Display - Auto - 30day
```

---

## 🔄 CONTRIBUTING

Issues and PRs welcome. Areas needing contribution:
- Additional negative keyword verticals
- International market campaign templates
- eCommerce-specific conversion tracking setup
- Automated anomaly detection scripts

---

## 🔍 PRE-LAUNCH CHECKLIST

Before spending first dollar:
- [ ] Conversion tracking verified in Tag Assistant
- [ ] Negative keywords from master.csv applied
- [ ] Landing page scores 70+ on `landing/checklist.md`
- [ ] At least 3 RSA ads per ad group
- [ ] Brand campaign running to protect direct traffic
- [ ] Daily budget set with delivery method = Standard
- [ ] Alert rules from `alerts/rules.yaml` configured
