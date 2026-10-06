---
name: mpesa-statement-analysis
description: Analyse M-Pesa (or other mobile money) statements exported as PDF, CSV or Excel and turn them into a clean transaction table, cash-flow summary and plain-English insights. Use when the user shares an M-Pesa, Airtel Money or MTN MoMo statement, or asks to categorise mobile money transactions, find where money is going, reconcile till/paybill receipts, or summarise business cash flow from mobile money.
---

# M-Pesa Statement Analysis

Turn a raw mobile money statement into numbers a business owner can act on.

## 1. Extract
- Read the statement (PDF, CSV or XLSX). For password-protected M-Pesa PDFs, ask the user to unlock it first. Never ask for or store their PIN or ID number.
- Build one table with these columns: `date`, `time`, `receipt_no`, `details`, `direction` (in/out), `amount`, `balance`, `counterparty`, `channel` (Till, Paybill, Send Money, Withdraw, Airtime, Fuliza, Charges, Other).
- Drop duplicate receipt numbers. Check that the opening balance plus the money in, minus the money out, equals the closing balance. If it doesn't, report the gap and don't hide it.

## 2. Categorise
- Map `details` text to the categories above using the M-Pesa wording (e.g. "Customer Payment to Small Business", "Pay Bill to", "Merchant Payment", "Customer Transfer", "Withdrawal Charge", "OverDraft of Credit Party" = Fuliza).
- Group transaction charges separately so the user can see what fees cost them.
- Ask the user once to label their 5–10 largest counterparties (e.g. supplier, customer, staff, rent). Re-use those labels for the rest.

## 3. Summarise
Produce:
- Total money in, money out and the net figure for the period, plus a weekly trend.
- The top 10 counterparties by value in each direction.
- Total fees and Fuliza costs, as an amount and as a % of the money out.
- Unusual items: one-off large transfers, repeated payments of the same amount, transactions late at night.

## 4. Deliver
- An Excel workbook with the tabs `Transactions`, `Summary` and `Counterparties`, built with formulas rather than pasted values where practical.
- A 5-bullet plain-English summary that starts with the single most useful finding (e.g. "Fees took KES 14,200 this quarter, 3.1% of your outflows").

## Rules
- Use the statement's currency and show amounts with thousands separators.
- Don't guess a category when the text is ambiguous. Mark it `Unclassified` and list those transactions.
- Treat the statement as confidential. Don't send its data to outside services.
