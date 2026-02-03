# 🚀 Enterprise Messaging Engine: 98% Cost Reduction
### Salesforce + Twilio SendGrid Decoupled Architecture

<p align="center">
  <img src="./assets/architecture-diagram.png" width="800" alt="RelayForce System Architecture">
</p>

![Salesforce](https://img.shields.io/badge/Platform-Salesforce-blue) ![AWS-Ready](https://img.shields.io/badge/Architecture-AWS--Ready-orange) ![License-MIT](https://img.shields.io/badge/License-MIT-green)

## 💰 The Business Impact (ROI)
**The Problem:** Salesforce limits daily emails (5k/day) and charges ~$15,000/year for Marketing Cloud to bypass it.
**The Solution:** This engine uses SendGrid to provide **unlimited volume** for **$240/year**.
**Annual Savings: $14,760+ per Org.**

---

## 🏗️ System Design: "The Post Office Model"
To avoid crashing Salesforce with high volume, I implemented a **Decoupled Messaging Pattern**.



### How it Works:
1. **The Sorter (Flow):** Logic separates "Urgent" vs "Bulk" mail. Bulk mail is queued in a **Staging Object** (The "Waiting Room").
2. **The Delivery Truck (Apex Batch):** A scheduled job runs every 5 minutes, picking up 1,000 records per "trip" to maximize API efficiency.
3. **The Handshake (Integration):** We use the **SendGrid v3 API** to deliver the mail and bring "Success/Failure" data back into Salesforce for reporting.

---

## 🛠️ Technical Highlights (For Architects)
* **Asynchronous Pattern:** Prevents "Apex CPU Limit" errors by offloading heavy work.
* **At-Least-Once Delivery:** Uses status tracking on staging records to ensure no message is lost.
* **Bulkification:** Aggregates 1,000 records into a single JSON payload to respect API rate limits.

## 🚀 Roadmap: Scaling to AWS
As volume grows, the next phase replaces the Apex Batch with **AWS EventBridge + Lambda**. This "Multi-Cloud" approach removes the processing load from Salesforce entirely, allowing for infinite scalability.

---

## 📂 Project Structure
* `/force-app/main/default/classes`: [EmailBatchProcessor.cls](LINK_TO_FILE) - The core engine.
* `/force-app/main/default/objects`: [Pending_Comm__c](LINK_TO_FILE) - The Messaging Queue.
* `/force-app/main/default/flows`: [Email_Router.flow](LINK_TO_FILE) - The Logic Engine.