# AML Alert Prioritization and Risk Scoring System

## Overview
This project implements a two stage, risk based Anti Money Laundering monitoring framework that mirrors how financial institutions design and operate transaction monitoring programs. The system combines scenario based detection with model driven alert prioritization to improve investigator efficiency while maintaining interpretability and regulatory alignment.

The objective is not to automate suspicious activity reporting, but to prioritize alerts for human review in accordance with common AML regulatory expectations.

---

## System Architecture

The framework is structured into two primary stages.

### Stage 1: Scenario Based Transaction Monitoring
Stage 1 focuses on coverage and detection. Transaction level rules are applied to identify behaviors associated with known money laundering typologies.

Key characteristics:
- Rolling time window feature engineering
- Scenario rules aligned to CAMS typologies
- Alerts generated at the account day level
- Emphasis on interpretability and explainability

Example scenarios include:
- High transaction velocity
- Cross border activity bursts
- Cash intensive behavior
- Rapid transaction patterns

An alert is generated when one or more scenario rules are triggered for an account on a given day.

---

### Stage 2: Alert Risk Scoring and Prioritization
Stage 2 focuses on efficiency and prioritization. Alerts generated in Stage 1 are enriched and ranked to determine which alerts should be reviewed first by investigators.

Stage 2 is divided into three sub steps.

#### Stage 2A: Alert Aggregation and Labeling
- Transactions are grouped into alerts
- An alert is labeled positive if any underlying transaction is labeled as laundering

#### Stage 2B: Alert Enrichment and Typology Attribution
- Alerts are enriched with a dominant laundering typology for analysis
- Typology labels are used only for validation and monitoring
- Typology information is intentionally excluded from model features to avoid leakage

#### Stage 2C: Alert Scoring Model
- Supervised models are trained to estimate alert risk
- Logistic regression is used as an interpretable baseline
- Gradient boosted trees are used for improved ranking performance
- Alerts are ranked by risk score for investigation

---

## Dataset
The project uses a synthetic AML transaction dataset with embedded laundering behaviors and typology labels. The data is used strictly for research and demonstration purposes.

Key fields include:
- Sender and receiver account identifiers
- Transaction amount, currency, and payment type
- Sender and receiver bank locations
- Ground truth laundering indicator
- Laundering typology labels

---

## Evaluation Approach
Model performance is evaluated using AML relevant metrics, rather than accuracy alone.

Primary evaluation criteria:
- Precision at the top 1 percent, 5 percent, and 10 percent of alerts
- Concentration of true positives within investigator capacity
- Stability across time based train test splits

This evaluation reflects how alert scoring models are assessed in production AML environments.

---

## Explainability and Reason Codes
To support investigator decision making, each alert is accompanied by reason codes derived from the most extreme contributing risk factors. These reason codes highlight behaviors such as elevated transaction volume, cross border activity, or cash intensity.

This approach supports transparency and aligns with expectations for explainable AML systems.

---

## Governance Considerations and Limitations
- The system is designed for alert prioritization, not automated decisioning
- Scenario thresholds require periodic tuning as behavior evolves
- Model performance should be monitored for drift and stability
- All alerts require human review and investigator judgment

---

## Project Structure
```
risk_based_aml_monitoring/
├── aml_alert_scoring.ipynb
├── README.md
└── data/
    └── saml_d.csv
```

---

## Key Takeaway
This project demonstrates how rule based monitoring and machine learning can be combined to create a practical, regulator friendly AML alert prioritization system that improves efficiency without sacrificing interpretability.
