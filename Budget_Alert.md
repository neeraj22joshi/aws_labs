# AWS Billing Alarm (Budget Alert) Setup Guide

## Objective
In this lab, you will create an AWS Budget Alert to monitor your AWS spending and receive email notifications when your usage reaches a defined threshold.

---

## Prerequisites
- An active AWS account
- Access to the AWS Management Console
- A valid email address for receiving alerts

---

# Step 1: Open Billing and Cost Management
Go to the AWS Console and open **Billing and Cost Management**.

---

# Step 2: Open Budgets
From the left navigation pane, select **Budgets**.

---

# Step 3: Create a Budget
Click **Create budget**.

> Note: AWS provides a multi-step wizard for creating the budget alarm.

---

# Step 4: Choose Budget Setup Type
Inside the budget setup page:

- Select the **Customize (advanced)** radio button.

---

# Step 5: Configure Basic Budget Details
Enter the following information:

| Setting | Value |
|---|---|
| Budget name | Enter a suitable budget name |
| Budget period | Monthly |
| Budget renewal type | Recurring budget |

---

# Step 6: Configure Budget Amount and Scope
Set the following options:

| Setting | Value |
|---|---|
| Budget amount | Example: `$10` |
| Budget scope | All AWS Services |
| Aggregate costs by | Unblended costs |

---

# Step 7: Continue
Click **Next**.

---

# Step 8: Create an Alert
Configure the alert settings:

| Setting | Value |
|---|---|
| Alert threshold | 50% of budget |
| Notification type | Email |
| Email recipients | Enter your email address |

### Example
If your monthly budget is **$10**, the alert will trigger when estimated charges reach **$5**.

---

# Step 9: Proceed
Click **Next**.

---

# Step 10: Review Configuration
Review all configured settings carefully, including:

- Budget name
- Budget amount
- Alert threshold
- Email recipient

---

# Step 11: Create Budget
Click **Create budget**.

---

# Step 12: Verify Budget Creation
After creation:

- The new budget appears in the **Budgets** dashboard.
- AWS sends a confirmation email notification setup.
- Your billing alarm is now active.

---

# Expected Outcome
You have successfully configured an AWS Billing Alarm using AWS Budgets. AWS will now notify you via email when your spending reaches the defined threshold.

---

# Best Practices
- Start with a low monthly budget while learning AWS.
- Configure multiple alerts (50%, 80%, 100%).
- Regularly review AWS usage to avoid unexpected charges.
