.. |saveImage| image:: images/button-save.png
.. |newImage| image:: images/button-new.png

Customer Receipt
-----------------

Click on the menu *Accounts Receivable -> Documents -> Customer Receipt*.

This will show the Customer Receipt Collection.

.. image:: images/customer-receipt-collection.png

You can create a new Customer Receipt by clicking on |newImage|.

Step 1: Select Customer

.. image:: images/customer-receipt-wizard-step1.png

Step 2: Select invoices for payment

.. image:: images/customer-receipt-wizard-step2.png

Final Step:

.. image:: images/customer-receipt.png

The fields are explained in the following table:

==========================		=============   ===============================================
Field Name          			Required        Description
==========================		=============   ===============================================
Voucher No	    			No              This is a system generated field. 
               	         	      	 		(*Format - VoucherAlias/Branch Alias/FinYear Alias/Sequence Number*)
Date                			Yes             The Voucher Date. By default, the system date is taken as Voucher Date.
							Note : The date should be within the constraints of the Financial Year.
Settlemet Type				Yes		Settlement Type. Options - 1) Cash Bank 2) Journal. Default value is Cash Bank.
Settlement Account			Yes		Select Cash Bank or Jouranl account depend on settlement type.
Receivable Account    			Yes             Customer (from step 1)
Txn Ccy		    			Yes		Trasaction Currency. Default is *Local*. If Txn Ccy is not Local, enter the exchange rate for the selected currency.
Voucher No				Yes		Voucher Ref No
Bill No		    			Yes             Bill No
Bill Date	    			Yes		Bill Date
Balance/Balance FC			Yes		Payment balanced for voucher ref no.	
Settled Amt/Settled Amt FC		Yes		Settled Amount/Settled Amount FC
Discount/Discount FC 			No		Discount/Discount FC
Exch Diff				No		Exch Diff
Net Settled/Net Settled FC		Yes		Net Settled/Net Settled FC
Total Amt/Total Amt FC			Yes		Total Amount/Total Amount FC
Narration				No		Narration
Cheque#					No		Cheque#
cheque Date 				No		cheque Date
Bank					No		Bank
Branch					No		Branch
==========================		=============   ===============================================


Click on |saveImage| to save your changes and close. The Customer Receipt Collection will now display the newly created Customer Receipt.


