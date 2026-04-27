# Data Dictionary
## Geospatial Road Risk Intelligence Analytics
### Ryan Watson Consulting Pty Ltd · Queensland Local Government · Nov 2024

---

## Table 1 — Pci_Segment_Data

One row per road segment. Derived from LiDAR point cloud survey processed through ArcGIS/PostGIS.

| Field Name | Data Type | Description | Example Value |
|------------|-----------|-------------|---------------|
| Inspect_ID | Text | Unique inspection identifier — primary key. Links to Pavement_Distress_Data | INS_001 |
| Road_Name | Text | Full road name as registered with council | Bayview St |
| PCI_Score | Decimal | Pavement Condition Index score 0–100. Higher = better condition | 83.6 |
| PCI_Lable | Text | Condition classification derived from PCI_Score | Good / Satisfactory / Fair / Poor / Very Poor / Serious |
| Latitude | Decimal | GPS latitude coordinate of segment midpoint | -27.9876 |
| Longitude | Decimal | GPS longitude coordinate of segment midpoint | 153.4012 |
| Length_M | Decimal | Length of road segment in metres | 9.2 |
| Area_SqM | Decimal | Surface area of road segment in square metres | 92.0 |
| Locality | Text | Suburb or locality name | Runaway Bay |

### PCI Score Classification

| PCI Range | Label | Condition Description |
|-----------|-------|-----------------------|
| 85 – 100 | Good | Minimal distress — routine monitoring only |
| 71 – 84 | Satisfactory | Minor distress — preventive treatment recommended |
| 56 – 70 | Fair | Moderate distress — maintenance required |
| 41 – 55 | Poor | Significant distress — rehabilitation required |
| 26 – 40 | Very Poor | Severe distress — urgent intervention required |
| 0 – 25 | Serious | Critical structural failure — immediate reconstruction |

---

## Table 2 — Pavement_Distress_Data

One row per recorded defect. Each road segment may have multiple defect records.

| Field Name | Data Type | Description | Example Value |
|------------|-----------|-------------|---------------|
| Inspect_ID | Text | Foreign key — links to Pci_Segment_Data | INS_001 |
| Road_Name | Text | Full road name — matches Pci_Segment_Data | Bayview St |
| Distress_Type | Text | Type of pavement defect recorded | crocodile_cracking |
| Severity | Text | Severity classification of the defect | High / Medium / Low |
| Defect_Area_SqM | Decimal | Surface area affected by this defect in square metres | 4.2 |
| Defect_Length_M | Decimal | Linear length of defect in metres | 2.1 |
| Locality | Text | Suburb or locality name | Runaway Bay |
| Image_URL | Text | Reference URL for inspection photo (internal use only) | — |

### Distress Type Definitions

| Distress Type | Description | Structural Risk |
|---------------|-------------|-----------------|
| crocodile_cracking | Interconnected crack pattern resembling crocodile skin — indicates sub-base failure | HIGH |
| lt_cracking | Longitudinal and transverse cracking — surface level | LOW–MEDIUM |
| block_cracking | Large rectangular crack pattern — thermal or shrinkage related | MEDIUM |
| weathering | Surface texture loss through oxidation and aggregate exposure | LOW |
| raveling | Progressive loss of aggregate from surface | LOW–MEDIUM |
| depressions | Localised surface deformation below surrounding pavement level | MEDIUM |
| patching | Previously repaired area — monitored for patch deterioration | LOW |
| sealant | Sealant joint or crack fill — monitored for failure | LOW |
| rutting | Permanent surface deformation in wheel paths | MEDIUM–HIGH |

### Severity Definitions

| Severity | Definition |
|----------|------------|
| High | Immediate intervention required — structural risk confirmed |
| Medium | Monitoring required — intervention within 12 months |
| Low | Routine maintenance — no immediate action required |

---

## Relationship

```
Pci_Segment_Data          Pavement_Distress_Data
─────────────────         ──────────────────────
Inspect_ID (PK) ─────────► Inspect_ID (FK)

Cardinality: One-to-Many
Cross-filter: Both directions
```

One segment can have multiple defect records. The relationship enables slicers on either table to filter across the entire data model.

---

*Data Dictionary prepared by Ryan Watson Consulting Pty Ltd · © 2024 All rights reserved*
