Loading Landed Costs in Stock Purchases
=======================================

To derive proper cost of stock purchases, it is very important to load landed costs as part of the value of inventory. Purchase of inventory is recorded using the document Stock Purchase available in Stocks and Inventory.

In coreERP, there are two alternate methods by which Landed Costs can be setup and recorded. In both the methods, a Landed Cost Type must first be defined and then it can be used in the Stock Purchase. You may also use a combination of both the methods for different types of Landed Costs.

In both methods, the amount entered as Landed Costs would be added to the cost of materials posted into the Stock Ledger and the Unit Rate would be increased based on pro-rata value (rate * qty) of each line item in Stock Purchase.

Method 1 - Expense Reference
----------------------------

In this method, we book Direct Expenses using the GST Bill and associate this transaction with a Stock Purchase. To start with, lets create a Landed Cost Type

    #. Click on *Stocks and Inventory -> Masters -> Landed Cost Type -> New*
    #. Provide a name e.g: Freight Inwards
    #. Associate the Expense Account. You should have created this Account Head in Chart Of Accounts and should be of type: Landed Costs
    #. Do Not check on Accrue Liability or associate the Liability Account. Leave this empty.
    
Now try booking a Stock Purchase 

    #. Select "Freight" in the Landed Costs Tab at the bottom of the document. 
    #. Create the document and escalate it to stage "Purchase Booking". Upon reaching this stage, you would be required to click on the button in the Landed Costs line and associate it with an already existing Bill/expense booked via **GST Bill**. 
    #. This bill should have debited the same account head as associated with the Landed Cost Type, defined in the previous step.

If the expense is not already booked at the time of creating Stock Purchase, you would be allowed to create the Stock Purchase with Landed Costs. However, to escalate to stage "Purchase Booked" and post, association of expense is required.

On booking the GST Bill, an automatic reference is created for the document and is available in Stock Purchase. Posting the Stock Purchase would not post debits into the Expense Head in this method. View GL Distribution in Stock Purchase.

Method 2 - External Reference via Liability booking
---------------------------------------------------

In this method, we proceed to associate the corresponding Liability account to the Landed Cost Type. Hence, system would create a posting into the respective accounts upon posting the Stock Purchase. No association with an expense is required. You can click on View GL Distribution to understand how the system would post the Stock Purchase in Financial.

The Stock Purchase should be completed and posted before booking the corresponding expense. Here, while using **GST Bill** to book the expense, the user should select the Liability Account and Debit the liability. He also needs to reference the Stock Purchase in **GST Bill**.



