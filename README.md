# Product Automatisering Platform

> AI-powered content generatie en CMS integratie platform voor e-commerce productbeheer op schaal.

**Genereert en publiceert 5.000+ productbeschrijvingen in een halve dag met behulp van web scraping, PDF parsing en multi-model AI.**

![Python](https://img.shields.io/badge/Python-3.9+-blue.svg)
![AI](https://img.shields.io/badge/AI-ChatGPT%20%7C%20Claude-purple.svg)
![Status](https://img.shields.io/badge/Status-Productie-success.svg)

> ℹ️ **Over deze repository** — De productiecode van dit platform draait intern bij FOV en is om bedrijfsredenen niet publiek. Deze README is een technische beschrijving van de architectuur, aanpak en resultaten. Een uitgebreide case study lees je op mijn [portfolio](https://bastiaanvdh.github.io/portfolio/cases/product-automation-platform.html).

---

## 📊 Impact Metrics

| Metric | Waarde |
|--------|--------|
| **Producten Verwerkt** | 5.000+ in 4 uur |
| **Verwerkingssnelheid** | ~20 producten/minuut |
| **AI Modellen Gebruikt** | ChatGPT + Claude (dual-model) |
| **Databronnen** | Web scraping + PDF parsing + Interne docs |
| **CMS Integratie** | KING (Bjorn Lunden) |
| **Handmatige Tijd Bespaard** | ~400 uur (vs handmatig schrijven) |

---

## 🎯 Wat Dit Platform Doet

Dit platform automatiseert de gehele product content creatie workflow voor e-commerce:

1. **Data Collectie**
   - Scrapt concurrent websites voor productinformatie
   - Parseert interne PDF documentatie (productsheets, veiligheidsbladen)
   - Extraheert gestructureerde data uit meerdere bronnen

2. **AI Content Generatie**
   - Genereert Meta Titel 1 (SEO-geoptimaliseerd)
   - Genereert Meta Titel 2 (conversie-gericht)
   - Creëert gedetailleerde productbeschrijvingen
   - Gebruikt dual-AI model benadering (ChatGPT + Claude) voor kwaliteit

3. **Document Matching**
   - Koppelt automatisch producten aan juiste PDF's
   - Linkt veiligheidsbladen
   - Verbindt technische specificaties

4. **CMS Integratie**
   - Exporteert naar Excel in KING-compatibel formaat
   - Bulk import in KING CMS
   - Valideert data voor import

**Resultaat:** Van ruwe data naar gepubliceerde producten in uren, niet weken.

---

## 🏗️ Platform Architectuur

```
┌──────────────────────────────────────────────────────────────────┐
│            PRODUCT AUTOMATISERING PLATFORM                        │
├──────────────────────────────────────────────────────────────────┤
│                                                                   │
│  FASE 1: DATA COLLECTIE                                          │
│  ┌─────────────────┐  ┌──────────────────┐  ┌────────────────┐ │
│  │ Web Scraper     │  │ PDF Parser       │  │ Interne Docs   │ │
│  │                 │  │                  │  │                │ │
│  │ • BeautifulSoup │  │ • OCR extractie  │  │ • Tech specs   │ │
│  │ • Concurrent    │  │ • Veiligheids-   │  │ • Product info │ │
│  │   sites         │  │   bladen         │  │ • Beschrijv.   │ │
│  └────────┬────────┘  └────────┬─────────┘  └────────┬───────┘ │
│           │                    │                      │          │
│           └────────────────────┼──────────────────────┘          │
│                                ↓                                 │
│  FASE 2: DATA VERWERKING                                         │
│  ┌──────────────────────────────────────────────────┐           │
│  │        Data Aggregatie & Matching                │           │
│  │                                                   │           │
│  │  • Normaliseer productnamen                      │           │
│  │  • Match PDF's aan producten                     │           │
│  │  • Dedupliceer informatie                        │           │
│  │  • Structureer data voor AI input                │           │
│  └────────────────────┬─────────────────────────────┘           │
│                       ↓                                          │
│  FASE 3: AI CONTENT GENERATIE                                    │
│  ┌──────────────────┐         ┌──────────────────┐             │
│  │   ChatGPT API    │         │   Claude API     │             │
│  │                  │         │                  │             │
│  │  • Meta Titel 1  │ ◄─────► │  • Validatie     │             │
│  │  • Meta Titel 2  │         │  • Alternatieve  │             │
│  │  • Beschrijv.    │         │    versies       │             │
│  └────────┬─────────┘         └────────┬─────────┘             │
│           │                            │                        │
│           └──────────┬─────────────────┘                        │
│                      ↓                                           │
│  FASE 4: KWALITEITSCONTROLE                                      │
│  ┌─────────────────────────────────────────────────┐            │
│  │       Validatie & Formattering                  │            │
│  │                                                  │            │
│  │  • Karakter limiet checks                       │            │
│  │  • SEO compliance                               │            │
│  │  • Duplicaat detectie                           │            │
│  │  • Formatteren voor KING CMS                    │            │
│  └────────────────────┬────────────────────────────┘            │
│                       ↓                                          │
│  FASE 5: CMS INTEGRATIE                                          │
│  ┌──────────────────────────────────────────────────┐           │
│  │            Excel Export → KING                   │           │
│  │                                                   │           │
│  │  • Export naar KING-compatibel Excel formaat    │           │
│  │  • Bulk import validatie                        │           │
│  │  • Upload naar KING CMS                         │           │
│  │  • Verifieer succesvolle import                 │           │
│  └──────────────────────────────────────────────────┘           │
│                                                                   │
└──────────────────────────────────────────────────────────────────┘
```

---

## 🛠️ Kerncomponenten

### 1. **Web Scraper**

Extraheert productinformatie van concurrent websites.

**Functionaliteiten:**
- BeautifulSoup-gebaseerde crawler
- Handelt dynamische content af
- Extraheert:
  - Product titels
  - Beschrijvingen
  - Technische specificaties
  - Prijzen (indien beschikbaar)
  - Categorie informatie
- Rate limiting en respectful crawling
- Error handling en retry logica

**Tech Stack:** Python, BeautifulSoup, Requests, Pandas

**Output:** Gestructureerde CSV met concurrent product data

---

### 2. **PDF Parser (OCR)**

Extraheert tekst uit product documentatie PDF's.

**Functionaliteiten:**
- OCR-gebaseerde tekst extractie
- Handelt af:
  - Product informatiebladen
  - Veiligheidsbladen (MSDS)
  - Technische specificaties
- Intelligente document matching:
  - Linkt juiste PDF aan elk product
  - Handelt meerdere documenten per product af
  - Fuzzy matching voor productnamen
- Gestructureerde data extractie

**Tech Stack:** Python, OCR libraries, PyPDF2, Pandas

**Verwerkte Documenten:** 1000+ PDF's

---

### 3. **AI Content Generator (Dual-Model)**

Genereert SEO-geoptimaliseerde product content met meerdere AI modellen.

**Functionaliteiten:**
- **Dual-AI benadering:**
  - ChatGPT voor initiële generatie
  - Claude voor validatie en alternatieven
  - Cross-validatie zorgt voor kwaliteit
- **Gegenereerde content:**
  - Meta Titel 1 (SEO-gericht, 60 karakters)
  - Meta Titel 2 (Conversie-gericht, 60 karakters)
  - Product beschrijving (150-300 woorden)
- **Input bronnen:**
  - Concurrent data (gescraped)
  - Interne PDF's (geparsed)
  - Bestaande product data
- **Kwaliteitscontroles:**
  - Karakter limiet handhaving
  - Keyword optimalisatie
  - Duplicaat detectie

**Tech Stack:** Python, OpenAI API, Anthropic Claude API

**Schaal:** 5.000+ producten verwerkt in 4 uur

**Voorbeeld Output:**
- Product: "Festool ETS 125 FEQ Plus Systainer"
- Meta 1: "Festool ETS 125 FEQ Plus | Excentrische Schuurmachine + Systainer"
- Meta 2: "Festool ETS 125 FEQ Plus Schuurmachine | Professionele Kwaliteit"
- Beschrijving: AI-gegenereerde 200-woord beschrijving met specs en voordelen
- Live voorbeeld: https://fov.nl/festool-ets-125-feq-plus-systainer

---

### 4. **Document Matching Engine**

Koppelt intelligent producten aan hun documentatie.

**Functionaliteiten:**
- Fuzzy name matching algoritme
- Handelt variaties af:
  - "Festool ETS 125" vs "ETS125" vs "ETS-125"
  - Verschillende talen
  - Afkortingen en modelnummers
- Multi-document ondersteuning:
  - Product sheet
  - Veiligheidsblad
  - Technische specificatie
- Validatie en confidence scoring

**Tech Stack:** Python, FuzzyWuzzy, Pandas

**Nauwkeurigheid:** 95%+ correcte matches

---

### 5. **KING CMS Integratie**

Exporteert en importeert product data naar KING enterprise systeem.

**Functionaliteiten:**
- Excel export in KING-compatibel formaat
- Kolom mapping:
  - Product ID → KING ID
  - Meta titels → SEO velden
  - Beschrijvingen → Content velden
- Bulk import validatie
- Error rapportage
- Import verificatie

**Tech Stack:** Python, openpyxl, Pandas

**CMS:** KING (Bjorn Lunden) - Zweeds enterprise ERP/CMS systeem

---

## 💻 Technische Stack

### Core Technologieën
| Categorie | Technologieën |
|-----------|---------------|
| **Taal** | Python 3.9+ |
| **Web Scraping** | BeautifulSoup, Requests |
| **PDF Processing** | PyPDF2, OCR libraries |
| **AI APIs** | OpenAI (ChatGPT), Anthropic (Claude) |
| **Data Processing** | Pandas, NumPy |
| **Excel Handling** | openpyxl, xlsxwriter |
| **Text Matching** | FuzzyWuzzy, difflib |

### Belangrijkste Libraries
```python
# Web Scraping
beautifulsoup4>=4.11.0
requests>=2.28.0
selenium>=4.8.0  # Voor dynamische content

# PDF & OCR
PyPDF2>=3.0.0
pytesseract>=0.3.10
pdf2image>=1.16.0

# AI APIs
openai>=1.0.0
anthropic>=0.8.0

# Data Processing
pandas>=1.5.0
numpy>=1.23.0
openpyxl>=3.1.0

# Text Processing
fuzzywuzzy>=0.18.0
python-Levenshtein>=0.20.0
```

---

## 📈 Performance Metrics

### Processing Pipeline
```
5.000 producten verwerkt in 4 uur

Breakdown:
├── Web scraping:      30 min   (concurrent data voor alle producten)
├── PDF parsing:       45 min   (1000+ documenten verwerkt)
├── AI generatie:      150 min  (20 producten/minuut gemiddeld)
├── Validatie:         20 min   (kwaliteitscontroles, duplicaat detectie)
├── Excel export:      5 min    (KING formaat export)
└── CMS import:        10 min   (bulk upload naar KING)

Totale tijd: ~4 uur
Handmatig equivalent: 400+ uur (80x sneller)
```

### Kostenefficiëntie
- **AI API kosten:** ~€50-100 per 5.000 producten
- **Tijd bespaard:** 400 uur handmatig schrijven
- **Kosten per product:** €0,01-0,02 (vs €5-10 handmatig)
- **ROI:** 99% kostenreductie

---

## 🚀 Real-World Impact

### Voor Platform
- ❌ Handmatig product beschrijving schrijven
- ❌ 5-10 minuten per product
- ❌ Inconsistente kwaliteit
- ❌ Geen systematisch PDF gebruik
- ❌ Hoge kosten (€5-10 per product)

### Na Platform
- ✅ Geautomatiseerde content generatie
- ✅ 0,48 minuten per product (20/min)
- ✅ Consistente, hoogwaardige output
- ✅ Alle documentatie benut
- ✅ 99% kostenreductie

### Bedrijfsresultaten
- **5.000+ producten** verwerkt en gepubliceerd
- **400 uur bespaard** (vs handmatig proces)
- **€0,01-0,02 per product** (vs €5-10 handmatig)
- **Dual-AI validatie** zorgt voor kwaliteit
- **Live op productie site** (fov.nl)

---

## 🔧 Workflow Voorbeeld

> De onderstaande snippets zijn illustratief en tonen de kern van de pipeline-logica.

### Input Data
```python
product = {
    "name": "Festool ETS 125 FEQ Plus",
    "category": "Schuurmachines",
    "brand": "Festool",
    "model": "ETS 125 FEQ",
    "competitor_description": "...",  # Van web scraper
    "pdf_data": "...",                # Van PDF parser
    "safety_sheet": "..."             # Van veiligheidsbladen
}
```

### AI Processing
```python
# ChatGPT genereert initiële content
meta1 = chatgpt.generate(
    prompt=f"Maak SEO meta titel voor {product['name']}",
    max_length=60
)

# Claude valideert en maakt alternatief
meta2 = claude.generate(
    prompt=f"Maak conversie-gericht alternatief",
    context=meta1
)

description = chatgpt.generate(
    prompt=f"Schrijf productbeschrijving met: {pdf_data}",
    max_length=300
)
```

### Output
```python
result = {
    "meta_title_1": "Festool ETS 125 FEQ Plus | Excentrische Schuurmachine + Systainer",
    "meta_title_2": "Festool ETS 125 FEQ Plus Schuurmachine | Professionele Kwaliteit",
    "description": "De Festool ETS 125 FEQ Plus is een professionele...",
    "product_sheet": "link_to_pdf.pdf",
    "safety_sheet": "link_to_safety.pdf"
}
```

---

## 🔒 Security & Compliance

- ✅ Alle API keys in environment variables
- ✅ Geen hardcoded credentials
- ✅ Respectful web scraping (rate limits, robots.txt)
- ✅ PDF data privacy (alleen interne docs)
- ✅ GDPR-compliant data handling

---

## 📝 Licentie

MIT License

---

## 👤 Auteur

**Bastiaan van der Horst** — [LinkedIn](https://www.linkedin.com/in/bastiaan-v-01846112a) · [Portfolio](https://bastiaanvdh.github.io/portfolio) · [GitHub](https://github.com/bastiaanvdh)

---

## 🙏 Achtergrond

Gebouwd om real-world e-commerce uitdagingen op schaal op te lossen. Dit platform demonstreert de kracht van het combineren van web scraping, AI, en enterprise integraties om production-ready automatisering te creëren die meetbare bedrijfswaarde levert.

**Van concept tot 5.000 producten live in productie.**
