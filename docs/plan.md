# Mentor Handbook

:::warning
This is the mentor's personal handbook. Nice to read, not required.
:::

- **Philosophy**: This is not a SQL course. This is a Role-Playing Game.
- **Goal**: Simulate a lifecycle of a real data project - from the ambiguity of the initial ask to the technical dept of the final deliverable.
- **Method**:
  - We present the student with "The Happy Path" (simple requests), expecting them to fall into the "Pitfalls" (common analytical errors), so we can teach them how to climp out.
  - Along the way, we introduce tools and techniques to help them solve the problems.

## Big Picture

| Phase | The Narrative Arc | The Process Stage               | The Core Skill                |
| ----: | ----------------- | ------------------------------- | ----------------------------- |
|     1 | The Translator    | Requirement Gathering           | Clarifying Ambiguity          |
|     2 | The Detective     | Exploratory Data Analysis (EDA) | Data Hygiene & Granularity    |
|     3 | The Architect     | Feature Engineering             | Business Logic Definition     |
|     4 | The Skeptic       | Statistical Analysis            | Bias Detection & Context      |
|     5 | The Engineer      | Deployment/Reporting            | Reproducibility & Refactoring |

## Phase 1: The Translator

### Narrative

The eager new hire receives a "simple" request from a VIP stakeholder. The pressure is on to answer quickly.

In the real world, stakeholders speak in business jargon ("Sales", "Churn", "Growth"), while databases speak in technical schema (`order_status`, `timestamp_utc`).

The Analyst's first job is not coding. It is **Translation**.

### General Assignment

> "The VP wants a single headline number/KPI for a specific month. Just get the number".

### Pitfalls

**Conceptual/Logical/Physical Gap.**

The database likely contains multiple candidates for the requested metric. For example:

- Date Ambiguity: Purchase Date vs. Approved Date vs. Delivered Date
- Status Ambiguity: Do we count cancelled orders? Returns? Test orders?

### Goals

- Clarify the ambiguous business terms (conceptual layer) by mapping them to specific data (logical/physical layer).

### Checklist

- Trigger: Did they ask clarifying questions before writing code?
- The Lesson: "Never accept a business term at face value. Always map 'Business Words' to 'Data Columns' and confirm with the stakeholder."

### Toolkit

- Database Documentation: dbdocs
- SQL Playgrounds: RunSQL, DBeaver
- Version Control: basic git commands
- Visualization: Metabase

<!--
This is the correct strategic approach. By abstracting the specific data values (which might change or differ based on how the data was loaded) and focusing on **Universal Data Concepts**, this guide becomes a robust framework for teaching *any* junior analyst, using the Olist dataset merely as the sandbox.

Here is the **High-Level Mentor’s Handbook**, restructured to focus on the **Data Analysis Process** and the **Narrative Arc**.

***

# 📘 The Analyst’s Journey: A Simulation Framework

**The Philosophy:** This is not a SQL course. It is a **Role-Playing Game**.
**The Goal:** Simulate the lifecycle of a real data project—from the ambiguity of the initial ask to the technical debt of the final delivery.
**The Method:** We present the student with "The Happy Path" (simple requests), expecting them to fall into "The Traps" (common analytical errors), so we can teach them how to climb out.

---

## 🗺️ The Big Picture: The Data Analysis Process

Each phase corresponds to a critical stage in the standard Analytics Lifecycle.



| Phase | The Narrative Arc | The Process Stage | The Core Skill |
| :--- | :--- | :--- | :--- |
| **1** | **The Translator** | **Requirement Gathering** | Clarifying Ambiguity |
| **2** | **The Detective** | **Exploratory Data Analysis (EDA)** | Data Hygiene & Granularity |
| **3** | **The Architect** | **Feature Engineering** | Business Logic Definition |
| **4** | **The Skeptic** | **Statistical Analysis** | Bias Detection & Context |
| **5** | **The Engineer** | **Deployment/Reporting** | Reproducibility & Refactoring |

---

## 🚦 Phase 1: The Translator (Requirement Gathering)

**The Narrative:** The eager new hire receives a "simple" request from a VIP stakeholder. The pressure is on to answer quickly.
**The Big Picture:** In the real world, stakeholders speak in business jargon ("Sales," "Churn," "Growth"), while databases speak in technical schema (`order_status`, `timestamp_utc`). The Analyst's first job is not coding; it is **Translation**.

### The Assignment (Generic)
> "The VP wants a single 'Headline Number' (e.g., Total Sales or Total Orders) for a specific month. Just get the number."

### The Trap: **The Definition Gap**
The database likely contains multiple candidates for the requested metric.
* *Date Ambiguity:* Purchase Date vs. Approved Date vs. Delivered Date.
* *Status Ambiguity:* Do we count cancelled orders? Returns? Test orders?

### The Mentor's Check
* **Trigger:** Did they ask clarifying questions *before* writing code?
* **The Lesson:** "Never accept a business term at face value. Always map 'Business Words' to 'Data Columns' and confirm with the stakeholder."

---

## 🕵️ Phase 2: The Detective (Exploratory Data Analysis)

**The Narrative:** The initial number was "technically" correct, but now Finance or Operations disputes it. The analyst is forced to defend their data, only to realize the data itself is messy.
**The Big Picture:** Raw data is rarely ready for analysis. EDA is the process of understanding the **shape** and **grain** of the data before aggregating it.

### The Assignment (Generic)
> "Combine two different datasets (e.g., Revenue and Payments, or Orders and Items) to reconcile a financial number."

### The Trap: **The Granularity Mismatch (Fan-Out)**
The student will likely attempt to join a "One-to-One" table with a "One-to-Many" table (or worse, two "One-to-Many" tables) without aggregating first.
* *The Risk:* Row explosion (Cartesian product) leading to inflated sums.
* *The Risk:* Inner Joins unintentionally filtering out valid "zero-value" records (e.g., customers with no orders).



### The Mentor's Check
* **Trigger:** Did they audit the row counts before and after the join?
* **The Lesson:** "Always understand the 'Primary Key' of every table. If you join tables with different grains (e.g., Order level vs. Item level), you must aggregate first."

---

## 🏗️ Phase 3: The Architect (Feature Engineering)

**The Narrative:** The data is clean, but the business question is subjective. Stakeholders are arguing over definitions (e.g., "What counts as 'Late'?"). The analyst must become the judge.
**The Big Picture:** Data doesn't always contain the answer directly. Analysts often need to create new features (columns) based on **Business Logic** to make the data useful.

### The Assignment (Generic)
> "Create a categorization metric (e.g., 'On Time' vs. 'Late', or 'VIP Customer' vs. 'Standard')."

### The Trap: **The Context Vacuum**
The student will likely apply a rigid mathematical rule (e.g., `Date A - Date B > 5`) without considering business context.
* *The Risk:* Ignoring workdays vs. weekends (Calendar Logic).
* *The Risk:* Hard-coding thresholds that become obsolete when business rules change.

### The Mentor's Check
* **Trigger:** Did they handle edge cases (Null dates, weekends, negative values)? Did they use scalable logic (e.g., `CASE WHEN`)?
* **The Lesson:** "Business logic is complex. Your code must be flexible enough to handle the 'Real World' rules, not just the mathematical ones."

---

## ⚖️ Phase 4: The Skeptic (Analysis & Insight)

**The Narrative:** The dashboard is built. The numbers show a clear trend. The analyst is ready to present. But wait—is the trend real, or is it a statistical illusion?
**The Big Picture:** This is the transition from "Reporting" (showing numbers) to "Analytics" (explaining why). It involves checking for bias and confounding variables.

### The Assignment (Generic)
> "Compare two groups (e.g., Region A vs. Region B) and tell us which one is performing better."

### The Trap: **Aggregation Bias (Simpson’s Paradox)**
The student will likely compare global averages.
* *The Risk:* Averages hide underlying structural differences (e.g., Region A sells cheap items, Region B sells expensive items).
* *The Risk:* Volume mix shifts (e.g., a bad week looks "good" because low-volume days performed well).



### The Mentor's Check
* **Trigger:** Did they segment the data to compare "Apples to Apples"?
* **The Lesson:** "Averages lie. To find the truth, you must control for variables (like product type, order size, or seasonality)."

---

## ⚙️ Phase 5: The Engineer (Production & Reporting)

**The Narrative:** The analysis was a success! But now the analyst is moving to a new project (or going on vacation), and the VP needs the report to run automatically every Monday.
**The Big Picture:** Ad-hoc analysis is ephemeral. **Data Engineering** is about creating sustainable, reproducible assets that survive the creator.

### The Assignment (Generic)
> "Take the messy code from the previous phases and turn it into a production-ready script or View."

### The Trap: **Technical Debt**
The student's current code is likely "Spaghetti Code"—full of nested subqueries, hard-coded dates, and unclear variable names.
* *The Risk:* The next person (or the future self) cannot understand or debug the code.
* *The Risk:* The query is computationally expensive/slow.

### The Mentor's Check
* **Trigger:** Is the code readable? Is it modular (using CTEs)? Is it documented?
* **The Lesson:** "Code is for humans first, computers second. Structure your logic so the next analyst can pick it up without calling you."

---

## 📝 Mentor's Note: How to Validate Assumptions

Since we are avoiding strict assumptions about the dataset, you must perform a **"Pre-Flight Check"** before assigning each phase.

**The "Golden Rule" of Mentoring this Simulation:**
*Before assigning a Trap, run the query yourself to ensure the Trap actually exists in your version of the data.*

1.  **For Phase 1:** Check if `count(*)` differs significantly from `count(distinct order_id)` or if `status='canceled'` records exist in the target month.
2.  **For Phase 2:** Find a specific Order ID that definitely has multiple items or payments. Use this ID as your "Gotcha" example.
3.  **For Phase 4:** Quickly run an average `freight_value` (or `price`) by State. If there is no significant difference, choose a different metric (e.g., `delivery_delay`) where a paradox might exist.
-->