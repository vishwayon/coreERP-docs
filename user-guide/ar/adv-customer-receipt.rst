.. |saveImage| image:: images/button-save.png
.. |newImage| image:: images/button-new.png

Advance Customer Receipt
-------------------------

Click on the menu *Accounts Payable -> Documents -> Advance Customer Receipt*.

This will show the Advance Customer Receipt Collection.

.. image:: images/adv-customer-receipt-collection.png

You can create a new Advance Customer Receipt by clicking on |newImage|.

.. image:: images/adv-customer-receipt.png

The fields are explained in the following table:

==================  			=============   ===============================================
Field Name         			Required        Description
==================  			=============   ===============================================
Voucher No	    			No              This is a system generated field. 
               	         	      	 		(*Format - VoucherAlias/Branch Alias/FinYear Alias/Sequence Number*)
Date                			Yes             The Voucher Date. By default, the system date is taken as Voucher Date.
							Note : The date should be within the constraints of the Financial Year.
Settlemet Type				Yes		Settlement Type. Options - 1) Cash Bank 2) Journal. Default value is Cash Bank.
Settlement Account			Yes		Select Cash Bank or Jouranl account depend on settlement type.
Txn Ccy		    			Yes		Trasaction Currency. Default is *Local*. If Txn Ccy is not Local, enter the exchange rate for the selected currency.
Customer				Yes
Narration				No		Narration
Total Amount				Yes		Advance Amount received from Customer
Cheque#					No		Cheque#
cheque Date 				No		cheque Date
Bank					No		Bank
Branch					No		Branch
==================  			=============   ===============================================

Click on |saveImage| to save your changes and close. The Advance Customer Receipt Collection will now display the newly created Advance Customer Receipt.


