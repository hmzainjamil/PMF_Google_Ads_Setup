# PMF_Google_Ads_Setup
PMF Google Ads campaign setup — complete structure, bidding, audiences, and tracking for lead gen

![Google Ads](https://img.shields.io/badge/Google_Ads-Campaign-4285F4?style=flat&labelColor=555)
![Python](https://img.shields.io/badge/Python-3.11+-3776AB?style=flat&labelColor=555&logo=python)
![GA4](https://img.shields.io/badge/GA4-Tracking-orange?style=flat&labelColor=555)
![Lead Gen](https://img.shields.io/badge/Goal-Lead_Gen-green?style=flat&labelColor=555)
![Status](https://img.shields.io/badge/Status-Active-brightgreen?style=flat&labelColor=555)

[Concepts](#-concepts) · [How It Works](#️-how-it-works) · [Structure](#-campaign-structure) · [Install](#-install) · [Tips](#-tips-and-tricks-12) · [Startups](#️-startups--businesses)

---

## 🧠 CONCEPTS

| Feature | Location | Description |
|---------|----------|-------------|
| [**Campaign Blueprint**](campaign-blueprint.xlsx) | `campaign-blueprint.xlsx` | Full campaign structure: campaigns, ad groups, keywords, bids |
| [**Keyword Sets**](keywords/) | `keywords/` | Branded, competitor, service, location keyword lists |
| [**Ad Copy Templates**](ad-copy/) | `ad-copy/` | RSA headlines, descriptions, extensions for PMF vertical |
| [**Audience Setup**](audiences/) | `audiences/` | Remarketing lists, customer match, similar audiences |
| [**Conversion Tracking**](tracking/) | `tracking/` | GA4 events, Google Tag, call tracking setup |
| [**Bidding Strategy**](docs/bidding.md) | `docs/bidding.md` | tCPA ramp protocol: manual CPC → enhanced → tCPA |

### 🔥 Hot

| Feature | Location | Description |
|---------|----------|-------------|
| [**tCPA Ramp Protocol**](docs/bidding.md) | `docs/bidding.md` | Week-by-week bidding escalation — avoids learning phase reset |
| [**Competitor Keywords**](keywords/competitors.csv) | `keywords/competitors.csv` | Competitor brand terms with SKAG structure |
| [**Extension Stack**](ad-copy/extensions/) | `ad-copy/extensions/` | Sitelinks, callouts, structured snippets, call extensions |

---

## ⚙️ HOW IT WORKS

```
Campaign Structure:
├── Brand Campaign (exact match, high bid)
│   └── [Brand] Ad Group
├── Service Campaign (broad → phrase → exact funnel)
│   ├── [Service Type 1] Ad Group
│   └── [Service Type 2] Ad Group
├── Competitor Campaign (separate budget, lower CPC target)
│   └── [Competitor Name] Ad Groups
└── Remarketing Campaign (RLSA)
    └── Past visitors — higher bids

Bidding ramp:
Week 1-2: Manual CPC ($X target)
Week 3-4: Enhanced CPC
Week 5+:  tCPA ($X target)
```

---

## 🚀 INSTALL

```bash
git clone https://github.com/hmzainjamil/PMF_Google_Ads_Setup
cd PMF_Google_Ads_Setup
pip install google-ads openpyxl
# Configure: credentials.yaml (Google Ads API)
python3 setup/create_campaigns.py --dry-run
python3 setup/create_campaigns.py --execute
```

---

## 📊 CAMPAIGN STRUCTURE

| Campaign | Match Types | Budget % | Goal |
|---|---|---|---|
| Brand | Exact | 15% | Defend brand, low CPC |
| Service - Exact | Exact | 35% | High intent, convert |
| Service - Phrase | Phrase | 25% | Mid funnel, learn |
| Competitor | Exact + Phrase | 15% | Conquest |
| Remarketing | All | 10% | Re-engage |

---

## 💡 TIPS AND TRICKS (12)

[structure](#tips-structure) · [bidding](#tips-bidding) · [keywords](#tips-keywords) · [tracking](#tips-tracking)

<a id="tips-structure"></a>■ **Structure (3)**

| Tip | Source |
|-----|--------|
| One theme per ad group — single keyword ad groups (SKAG) for max QS control | [HMZ](https://github.com/hmzainjamil) |
| Separate brand campaign — never mix brand + generic, different CPC economics | [DigiMinds](https://github.com/hmzainjamil) |
| Competitor campaign at 80% of service CPC — conquest with margin protection | [HMZ](https://github.com/hmzainjamil) |

<a id="tips-bidding"></a>■ **Bidding (3)**

| Tip | Source |
|-----|--------|
| Never switch to tCPA with <30 conversions — learning phase needs data | [Google Ads Best Practices](https://ads.google.com/intl/en_us/home/resources/) |
| tCPA target = 1.5x actual CPA for first 2 weeks — give algorithm room to learn | [HMZ](https://github.com/hmzainjamil) |
| Lower tCPA by 10% max per adjustment — aggressive cuts reset learning phase | [DigiMinds](https://github.com/hmzainjamil) |

<a id="tips-keywords"></a>■ **Keywords (3)**

| Tip | Source |
|-----|--------|
| Run search terms report weekly — add negatives before waste compounds | [HMZ](https://github.com/hmzainjamil) |
| Pause broad match until tCPA is stable — broad in learning phase = wasted spend | [DigiMinds](https://github.com/hmzainjamil) |
| Location modifiers for local: "[service] near me", "[service] [city]" | [HMZ](https://github.com/hmzainjamil) |

<a id="tips-tracking"></a>■ **Tracking (3)**

| Tip | Source |
|-----|--------|
| Import GA4 goals into Google Ads — richer attribution than Google Tag alone | [HMZ](https://github.com/hmzainjamil) |
| Call tracking: set 60s as conversion threshold — under 60s = wrong number | [DigiMinds](https://github.com/hmzainjamil) |
| Enhanced conversions: wire first-party data — improves tCPA 15-25% on average | [Google](https://support.google.com/google-ads/answer/9888656) |

---

## ☠️ STARTUPS / BUSINESSES

| This Repo / Feature | Replaced |
|-|-|
| **Campaign blueprint** | [Optmyzr](https://optmyzr.com), [WordStream](https://wordstream.com) setup tools |
| **tCPA ramp protocol** | Agency retainers charging $1,500/mo for bidding management |
| **Keyword sets** | [SEMrush](https://semrush.com), [Ahrefs](https://ahrefs.com) keyword research — free here |
| **Conversion tracking setup** | [Analyzify](https://analyzify.com), [Elevar](https://getelevar.com) tracking services |

---

## Star History

[![Star History Chart](https://api.star-history.com/svg?repos=hmzainjamil/PMF_Google_Ads_Setup&type=Date)](https://star-history.com/#hmzainjamil/PMF_Google_Ads_Setup&Date)

---

<div align="center">
Built by <a href="https://github.com/hmzainjamil">HMZ</a> · <a href="https://digiminds.org">DigiMinds</a> · Google Ads lead gen framework
</div>
