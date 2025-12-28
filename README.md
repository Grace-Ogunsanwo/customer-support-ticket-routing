**Customer Support Ticket Routing with NLP
Overview**

This project explores how natural language processing can be used to route customer support tickets to the right team. The goal is not just to maximise a single accuracy score, but to design a routing system that reflects how real support organisations actually work.

The project starts with simple text classification models and gradually evolves into a more realistic, multi-stage routing approach. Along the way, different modelling and problem-framing decisions are tested and evaluated.

**Problem statement**

Given the text of a customer support ticket, automatically route it to the appropriate queue.

Original ticket types:

Billing inquiry

Refund request

Cancellation request

Product inquiry

Technical issue

The challenge is that many of these categories share similar language, which makes fine-grained classification difficult using text alone.

**Data**

Each ticket contains:

Subject

Description

Ticket type (label)

The subject and description are combined into a single text field for most experiments. Minimal text cleaning is applied to preserve natural language signals.

**Baseline approach**

The initial models use:

TF-IDF vectorisation

Linear classifiers (Logistic Regression, Linear SVM)

These models establish a baseline and help identify the limits of flat, single-stage classification. Performance plateaus around 20–21 percent accuracy, even after experimenting with different n-gram ranges and models.

**Feature experiments**

Several feature-level improvements are tested:

Subject-weighted TF-IDF

Character n-grams

Alternative linear classifiers

These changes produce small gains but do not significantly improve top-1 accuracy. However, top-2 accuracy reveals that the model often includes the correct label among its top predictions, suggesting the presence of useful signal.

**Label restructuring**

To better reflect real-world support workflows, ticket types are regrouped into broader routing queues:

Finance (Billing, Refund)

Account (Cancellation)

Support (Product, Technical)

This restructuring leads to a clear improvement, with overall accuracy rising to approximately 33 percent. This demonstrates that problem definition and label design can matter as much as model choice.

**Two-stage routing architecture**

The final approach uses a hierarchical routing system:

**Stage 1:**
Classify tickets as Account vs Non-Account

**Stage 2:**
For Non-Account tickets, classify between Finance and Support

Each stage uses a character n-gram TF-IDF model with Logistic Regression. This mirrors how many real support teams triage tickets in practice.

Results:

Stage 1 accuracy: ~61 percent

Stage 2 accuracy: ~48 percent

End-to-end routing accuracy significantly higher than any flat model

This shows that breaking a complex problem into simpler decisions can meaningfully improve routing performance.

**Key takeaways**

Flat classification struggles when labels overlap linguistically

Feature engineering alone has limited impact

Label design and system architecture drive the biggest gains

Hierarchical routing is more realistic and more effective than a single classifier

**Repository structure**
customer-support-ticket-routing/
├── notebooks/
│   └── 01_eda_and_text_preprocessing.ipynb
├── data/
│   └── customer_support_tickets.csv
├── README.md
└── .gitignore

**Future work**

Incorporating metadata such as ticket priority or channel

Adding confidence thresholds and human-in-the-loop review

Exploring transformer-based models for comparison

Evaluating cost-sensitive routing errors
