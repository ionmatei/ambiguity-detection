# Policy Twin

A policy formalization and repair tool that converts ambiguous natural-language health policies into formally verified, database-executable BPMN workflows. The tool is demonstrated on a Japanese municipal diabetic nephropathy health guidance program.

---

## Overview

Municipal health policies are often written in natural language and contain ambiguities that make consistent, machine-interpretable implementation difficult. **Policy Twin** addresses this by:

1. Extracting and documenting ambiguities in raw policy documents
2. Formalizing and repairing policy language into logical expressions
3. Generating executable BPMN process workflows from the formalized policies
4. Validating the workflows against patient data

The case studies in this repository apply the methodology to a **diabetic nephropathy prevention program**, which identifies patients with both diabetes and reduced kidney function and provides targeted health guidance to slow progression toward dialysis.

---

## Repository Structure

```
Policy Twin/
├── city-1/                              # Case study: City 1
│   ├── city_1_raw_policy.pdf            # Original natural-language policy
│   ├── city_1_repaired_policy.pdf       # Policy with ambiguities resolved
│   ├── city_1_ambiguity_report.pdf      # Documentation of all identified ambiguities
│   ├── city_1_repair_report.pdf         # Summary of changes made during repair
│   ├── city_1_narrative.pdf             # Formal logical analysis and database-executable expressions
│   ├── city_1_reference.bpmn            # BPMN workflow (reference/pre-repair)
│   ├── city_1_target.bpmn               # BPMN workflow (target/post-repair)
│   ├── city_1_reference.png             # Diagram of reference process
│   └── city_1_target.png               # Diagram of target process
├── city-2/                              # Case study: City 2 (same structure as city-1)
├── input-data/
│   └── test_data.csv                    # Synthetic patient dataset (100 records)
├── supplemental_material.pdf            # Ministry-level guidelines and specifications
├── supplemental_material_narrative.pdf  # Formal database-executable narratives (ministry level)
└── tool-demo-movie.mp4                  # Video demonstration of the tool
```

---

## Technology Stack

| Component | Technology |
|---|---|
| Process modeling format | BPMN 2.0 XML |
| BPMN authoring tool | Camunda Modeler v5.34.0 |
| BPMN execution engine | SpiffWorkflow |
| Patient data format | CSV |
| Documentation | PDF |

---

## Policy Formalization Pipeline

The tool processes raw policies through five phases:

```
Raw Policy Document
    ↓
Phase 1: Structural Analysis
    Identify logical groupings, extract decision criteria,
    map natural language to formal structures
    ↓
Phase 2: Tokenization & OR Disambiguation
    Classify each OR as DISJUNCTIVE (true alternative)
    or CLARIFICATION (synonym/elaboration)
    ↓
Phase 3: Expression Tree Construction
    Build logical expression tree with explicit operator
    precedence: NOT > AND > OR
    ↓
Phase 4: Database Mapping
    Map conceptual conditions to patient data columns using
    a specificity hierarchy: EXACT > HIGH > MEDIUM > LOW
    ↓
Phase 5: Expression Generation
    Output database-executable logical expressions
    ↓
BPMN Workflow Generation
    Produce executable process diagram from formal expressions
    ↓
Outputs: Repaired Policy + BPMN + Ambiguity Report + Repair Report
```

---

## Business Process (BPMN)

The executable workflow implements 7 sequential tasks with decision gates:

| Step | Task | Condition to Continue |
|---|---|---|
| 1 | Verify Health Check Data | `Health_Check == 1` |
| 2 | Check Cancer Exclusion | `Cancer != 1` |
| 3 | Assess Clinical Eligibility | Diabetes + kidney function criteria met |
| 4 | Notify Health Guidance Eligibility | Always (send recommendation) |
| 5 | Check Health Guidance Acceptance | `Health_Guidance == 1` |
| 6 | Provide Health Guidance Sessions | 6 monthly sessions over 6 months |
| 7 | Conduct Post-Guidance Evaluation & Follow-Up | Assess HbA1c, eGFR, urinary protein changes |

### Eligibility Logic (Formal Expression)

```
Health_Check == 1
AND Type_2_Diabetes_Prior_Year_Jan_to_Dec == 1
AND Diabetes_Under_Treatment == 1
AND (Fasting_Blood_Glucose >= 126 OR HbA1c >= 6.5)
AND ((eGFR >= 30 AND eGFR <= 60) OR Urinary_Protein >= 1)
AND NOT (Cancer == 1)
```

---

## Patient Data Schema

The `input-data/test_data.csv` file contains 100 synthetic patient records with the following fields:

| Field | Description |
|---|---|
| ID, Sex, Age | Patient demographics |
| Fasting_Blood_Glucose, HbA1c | Diabetes indicators (lab values) |
| Type_2_Diabetes_Prior_Year_Jan_to_Dec | Diabetes classification flag |
| Diabetes, Diabetes_History, Diabetes_Under_Treatment | Diabetes status flags |
| Type_1_Diabetes | Exclusion criterion flag |
| eGFR | Estimated glomerular filtration rate (kidney function) |
| Urinary_Protein, Urine_lbumin | Kidney function markers |
| Cancer | Exclusion criterion flag |
| Health_Check | Health checkup participation flag |
| Health_Guidance, Specific_Health_Guidance_Target | Guidance program flags |

---

## KPI Tracking

The system tracks the following key performance indicators:

| KPI | Description |
|---|---|
| NOTIFICATION_COUNT | Number of guidance recommendations sent |
| HEALTH_GUIDANCE_COUNT | Number of counseling sessions delivered |
| GUIDANCE_RESOURCE | Resource units consumed |
| HEALTH_IMPROVEMENT_EFFECT | HbA1c and eGFR improvement rates |
| MEDICAL_COST_SAVINGS | Estimated costs prevented by avoiding dialysis progression |

---

## Case Studies

Two municipal implementations are included, demonstrating how the same formalization methodology applies across different local health policies with regional variations in eligibility thresholds and available resources.

- **City 1** — See [city-1/](city-1/)
- **City 2** — See [city-2/](city-2/)

A ministry-level baseline policy and its formal analysis are provided in `supplemental_material.pdf` and `supplemental_material_narrative.pdf`.

---

## Demo

A recorded demonstration of the tool is available at `tool-demo-movie.mp4`, showing the end-to-end workflow from raw policy document to formal expressions and BPMN diagram.
