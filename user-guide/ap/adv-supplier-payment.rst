.. |saveImage| image:: images/button-save.png
.. |newImage| image:: images/button-new.png

Advance Supplier Payment
-------------------------

Click on the menu *Accounts Payable -> Documents -> Advance Supplier Payment*.

This will show the Advance Supplier Payment Collection.

.. image:: images/advance-supplier-payment-collection.png

You can create a new supplier by clicking on |newImage|.

Step 1: Select Supplier

.. image:: images/advance-supplier-payment-wizard-step1.png

Final Step:

.. image:: images/advance-supplier-payment.png

The fields are explained in the following table:

==================  			=============   ===============================================
Field Name         			Required        Description
==================  			=============   ===============================================
Bill No		    			No              This is a system generated field. 
               	         	      	 		(*Format - VoucherAlias/Branch Alias/FinYear Alias/Sequence Number*)
Date                			Yes             The Voucher Date. By default, the system date is taken as Voucher Date.
							Note : The date should be within the constraints of the Financial Year.
Settlemet Type				Yes		Settlement Type. Options - 1) Cash Bank 2) Journal. Default value is Cash Bank.
Account					Yes		Select Cash Bank or Jouranl account depend on settlement type.
Payable Account        			Yes             Supplier Name (from step 1)
Txn Ccy		    			Yes		Trasaction Currency. Default is *Local*. If Txn Ccy is not Local, enter the exchange rate for the selected currency.
Person Type				No
Section 				No
Base rate %
Base Rate Amount
Ecess %
Ecess Amount
Surcharge %
Surcharge Amount
Narration				No		Narration
Net Amount
Cheque#					No		Cheque#
cheque Date 				No		cheque Date
Bank					No		Bank
Branch					No		Branch
==================  			=============   ===============================================

Click on |saveImage| to save your changes and close. The Advance Supplier Payment Collection will now display the newly created Advance Supplier Payment.


