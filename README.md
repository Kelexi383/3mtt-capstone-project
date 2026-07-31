# Project CC-02: High-Performance, Cost-Optimized NGO Static Web Application

## Executive Summary
This project demonstrates the design and deployment of a serverless, highly available, and cost-optimized web application platform tailored for Non-Governmental Organizations (NGOs). By leveraging **Azure Blob Storage (Static Website Hosting)** coupled with **Azure Content Delivery Network (CDN)** and automated cost management controls, this architecture delivers sub-second global page loads and enterprise security at virtually zero operational cost (< $0.20/month).

---

## 📐 Architecture & Traffic Flow

[ User Browser ]
│
(HTTPS / TLS 1.2+)
▼
[ Azure CDN Edge Nodes ]  <─── (Global Caching & DDoS Scrubbing)
│
(Origin Fetch / HTTP)
▼
[ Azure Blob Storage ($web) ] <─── (Serverless Host / LRS Redundancy)

1. **Edge Caching & CDN:** Global end-user requests are routed to the nearest Azure CDN POP (Point of Presence), minimizing latency and absorbing traffic surges.
2. **Serverless Static Hosting:** The backend static assets (`index.html`, `404.html`) reside in a cost-optimized, serverless `$web` storage container.
3. **Automated Security:** HTTPS encryption is provisioned natively via Azure-managed certificates across the CDN edge.

---

## 🛠️ Security, Defense & Monitoring Setup

### 1. DDoS & Edge Defense
* **Infrastructure Layer Protection:** Native Azure DDoS Basic protection scrubs Layer 3/4 volumetric floods at the cloud edge without additional cost.
* **Absorptive Caching:** The CDN acts as a buffer layer, preventing direct HTTP traffic spikes from hitting origin storage.

### 2. Cost Governance & Anomaly Alerts
* **Budget Limit:** Configured an Azure Cost Management budget capped at **$5.00/month**.
* **Automated Alerts:** Email notification triggers set at **80%** ($4.00) and **100%** ($5.00) of actual and forecasted spend to prevent unexpected usage costs.

### 3. Operational Monitoring
* **Azure Monitor Metrics:** Real-time tracking configured for CDN **Total Requests**, **Egress Bandwidth**, and **HTTP 4xx/5xx Error Codes**.

---

## 💰 Financial Breakdown & Cost Optimization Note

| Component | Tier / Specification | Monthly Cost (USD) |
| :--- | :--- | :--- |
| **Azure Blob Storage** | Standard Hot LRS (`$web` container, ~5 MB assets) | ~$0.0001 / mo |
| **Azure CDN** | Standard Microsoft (< 5 GB outbound transfer) | ~$0.00 - $0.05 / mo |
| **SSL/TLS Certificates** | Azure Managed HTTPS Certificate | $0.00 (Free) |
| **Total Estimated Spend** | **Serverless Architecture** | **< $0.20 / month** |

*Why this architecture fits an NGO budget:* Traditional virtual machines or app servers run continuously, charging $15–$50+ monthly regardless of traffic. This serverless static pattern charges purely on demand, allowing 99.9% of NGO funding to go directly toward field initiatives rather than hosting overhead.

---

## 📄 Infrastructure as Code (IaC)

This architecture is declaratively defined using Terraform in `main.tf` to support automated CI/CD deployment pipelines and cloud infrastructure auditing.


