# HCP License Lookup Tool

**Automated search preparation for HCP license verification against Indian medical registries.**

Parses Veeva OpenData HCP exports and maps each healthcare professional to their target State Medical Council, generating structured search queries for license verification across 22+ Indian medical registries.

---

## The Problem

Veeva OpenData HCP records frequently lack verified license numbers. Manually looking up each HCP across fragmented state medical council websites is time-consuming — India has 22+ state councils, each with a different portal, search interface, and data format. A dataset of 500 HCPs can take days to verify by hand.

This tool automates the preparation step: parsing Veeva exports, resolving each HCP to the correct state council, and generating ready-to-use search queries that cut verification time significantly.

---

## How It Works

1. **Parse** -- Reads a Veeva HCP export (CSV or Excel), extracting name, specialty, qualifications, HCO affiliation, city, and state fields using Veeva column naming conventions.
2. **Map** -- Resolves each HCP to their target State Medical Council using explicit state data or city-to-state inference (80+ Indian cities mapped).
3. **Generate** -- Produces structured search queries for NMC Indian Medical Register and state council portals.
4. **Export** -- Outputs a formatted Excel workbook with pre-filled search parameters and blank columns for verification results.

---

## Quick Start

```bash
# Install dependencies
pip install pandas openpyxl

# Run the tool
python hcp_license_lookup.py --input hcp_export.xlsx --output lookup_prepared.xlsx
```

---

## Output Columns

| Column | Description |
|--------|-------------|
| HCP_VID | Veeva identifier (18-digit, text format) |
| HCP_Name | Full name constructed from Veeva fields |
| Specialty | Primary specialty |
| Degrees | Combined qualifications from degree columns |
| Affiliation | Parent HCO name (locality and department stripped) |
| City / State | Location fields |
| Target_Council | Mapped State Medical Council name |
| Council_URL | Direct link to council portal |
| Search_Query_NMC | Ready-to-use NMC search string |
| Search_Query_Web | Web search query with name + credentials |
| License_Number | Blank -- fill during verification |
| Status | Pending / Found / Not Found |

---

## Registry Coverage

Supports all major Indian State Medical Councils:

| Region | Councils |
|--------|----------|
| North | Delhi MC, UP State MC, Punjab MC, Haryana MC, Rajasthan MC, HP MC, Uttarakhand MC |
| South | Tamil Nadu MC, Karnataka MC, Kerala (TCMC), Telangana MC, AP MC, Goa MC |
| East | West Bengal MC, Bihar MC, Odisha MC, Jharkhand MC, Assam MC |
| West | Maharashtra MC, Gujarat MC, MP MC, CG MC |
| National | NMC Indian Medical Register (all India, MBBS+) |

City-to-state inference covers 80+ Indian cities for automatic council resolution.

---

## AI-Powered Deep Verification

The `prompts/` directory includes a comprehensive system prompt (v1.4) for AI-assisted license verification. It covers:

- Multi-tier search strategy (NMC, state councils, healthcare directories)
- Disambiguation rules for common names across multiple councils
- Close-match logic when only initials are available
- Social media and digital footprint verification for inconclusive cases
- Confidence scoring with weighted matching (name, specialty, location, affiliation)

---

## Tech Stack

| Component | Technology |
|-----------|-----------|
| Data Engine | pandas |
| Excel Output | openpyxl |
| Language | Python 3.10+ |

---

## Built For

Veeva OpenData India Operations — supporting HCP license verification workflows where completeness and accuracy of medical credentials are regulatory requirements.
