# Bugs found

Add one section per issue. Bug 1 is filled in to show the format — fix it, then write what you changed. Copy the blank template for the rest.

Keep this file in the repo and **commit it** with your fixes.

---

## Bug 1

**How to reproduce:** Open the app. The expense list says “Newest first”. The first row is Wine (7 Mar). Board game (15 Mar) is further down.

**What is wrong:** The list is showing oldest expenses first. Newest should be at the top.

**What I changed:** In src/components/ExpenseList.jsx, reversed the sort order from  - b to dateValue(b.date) - dateValue(a.date). Also updated dateValue in src/lib/format.js to return 
ew Date(date).getTime().

---

## Bug 2

**How to reproduce:** Filter expenses by searching "Uber", then click Delete on the Uber expense. Clear the search bar.

**What is wrong:** The wrong expense is deleted because deletion and editing used array index instead of unique expense IDs.

**What I changed:** Updated src/state/store.js reducer to handle DELETE_EXPENSE and UPDATE_EXPENSE by ction.id instead of array index. Updated App.jsx and ExpenseList.jsx to pass expense.id and use key={expense.id}.

---

## Bug 3

**How to reproduce:** In the Filter card, select any member from the "Paid by" dropdown.

**What is wrong:** The expense list turns empty because e.paidBy (number) was compared to paidBy (string) with strict inequality (!==).

**What I changed:** In src/App.jsx, changed e.paidBy !== paidBy to String(e.paidBy) !== String(paidBy).

---

## Bug 4

**How to reproduce:** Check the Balances card for a member who paid more than their share (e.g. Ben Okonkwo).

**What is wrong:** The label and CSS classes were inverted. Creditors were shown in red with "owes" and debtors were shown in green with "is owed".

**What I changed:** In src/components/BalancesPanel.jsx, swapped the conditions so positive balance displays "is owed" (green) and negative balance displays "owes" (red).

---

## Bug 5

**How to reproduce:** Add an expense paid by someone who is not included in the split (e.g., Diya paying for Aisha and Ben's cab).

**What is wrong:** An extra share was subtracted from the payer's balance, penalizing them and causing total group balances not to sum to zero.

**What I changed:** In src/lib/balances.js, removed the conditional block that subtracted mount / n when the payer was not in shares.

---

## Bug 6

**How to reproduce:** Add an expense where a single debtor owes the exact amount a single creditor is owed.

**What is wrong:** In the Settle Up panel, exact-match settlements were completely missing from the suggested transfers.

**What I changed:** In src/lib/settle.js, updated the else branch to push the transfer to 	ransfers before advancing both indices i and j.

---

## Bug 7

**How to reproduce:** Add an expense, choose Custom %, and split 3 ways with 33.33%, 33.33%, 33.34%. Click Save expense.

**What is wrong:** JavaScript floating-point arithmetic evaluated the sum to 100.00000000000001, failing the strict === 100 check.

**What I changed:** In src/lib/money.js, updated percentsSumTo100 to allow standard epsilon tolerance: Math.abs(sum - 100) < 0.01.

---

## Bug 8

**How to reproduce:** Split  equally among 3 members.

**What is wrong:** Each member was assigned .33, totaling .99 and losing 1 cent to rounding.

**What I changed:** In src/lib/money.js, updated splitEqual to calculate total cents, compute base cents, and distribute remainder cents among members so the sum always matches the bill amount.

---

## Bug 9

**How to reproduce:** Add a new member in the Summary card using "Add member".

**What is wrong:** The new member did not show up in the "Paid so far" list until an expense changed.

**What I changed:** In src/components/SummaryCards.jsx, added members to the useMemo dependency array [members, expenses].

---

## Bug 10

**How to reproduce:** Refresh the page after saving expenses to localStorage.

**What is wrong:** Dates were displayed as raw ISO date strings instead of formatted locale date strings.

**What I changed:** In src/lib/format.js, updated ormatDate to convert input to 
ew Date(date) before calling 	oLocaleDateString.
