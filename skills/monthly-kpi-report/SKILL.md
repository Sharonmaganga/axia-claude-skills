---
name: monthly-kpi-report
description: Build a one-page monthly management report from sales, finance or operations data (Excel, CSV, Google Sheets export or accounting system export). Use when the user asks for a monthly report, board pack summary, KPI dashboard, management report, month-end review, or "how did we do this month".
---

# Monthly KPI Report

Give a busy manager a one-page answer to the question "how did we do this month, and what needs attention?"

## 1. Understand the data
- Load the files and list the columns you found, the date range and the row counts.
- Ask the user which 4–6 KPIs matter most. If they don't know, suggest some based on their sector:
  - **Retail / SME:** revenue, gross margin %, average basket, top products, stock days.
  - **SACCO / microfinance:** loan disbursements, portfolio at risk (PAR 30), collections rate, member growth, deposits.
  - **Services:** billable hours, utilisation %, invoices outstanding, days sales outstanding (DSO).
- Confirm how each KPI is calculated before you compute it.

## 2. Clean
- Standardise the dates, currencies and category names, and remove duplicate rows.
- Report anything you excluded and why. Don't drop rows without saying so.

## 3. Analyse
For each KPI, show:
- This month's figure, the change on last month, the change on the same month last year (if the data covers it), and the change against target (if the user gives one).
- A status: 🟢 on track, 🟠 watch, 🔴 action needed. Use the user's thresholds, or ±5% from target by default.
- One sentence on the main driver, backed by the data (e.g. "Down 12% because Kisumu branch sales fell after the stock-out in week 2").

## 4. Deliver
- **One-page summary** (Word, PDF or a doc): a KPI table, 3 wins, 3 concerns and 3 recommended actions.
- **Workbook**: the cleaned data, KPI calculations as live formulas, and one chart per KPI.
- Keep the language plain. Explain any jargon the first time you use it.

## Rules
- Never invent targets or benchmarks. Ask for them, or leave them blank.
- If there's too little data to see a trend, say so rather than over-interpreting.
