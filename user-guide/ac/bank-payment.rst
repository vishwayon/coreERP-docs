.. |newImage| image:: images/button-new.png
.. |saveImage| image:: images/button-save.png
.. |postImage| image:: images/button-post.png
.. |deleteImage| image:: images/button-delete.png
.. |unpostImage| image:: images/button-unpost.png

Bank Payment
------------

Bank Payment enables payment to be made from a specified Bank Account. Entries can be made affecting one bank to any other account(s) except Bank and Cash.

Click on the menu *Financial Accounting -> Documents -> Bank Payment*.

The following screen should appear. This is the Bank Payment Collection.

.. image:: images/BankPayment-Collection.png

You can create a new Bank Payment by clicking on |newImage|

.. image:: images/BankPayment.png

The fields are explained in the following table:

=======================		 =============   ===============================================
Field Name          		 Required        Description
=======================		 =============   ===============================================
Voucher No       		 No              This is a system generated field. 
                       	               	 	 (*Format - VoucherAlias/Branch Alias/FinYear Alias/Sequence Number*)
Date                	  	 Yes             The Voucher Date. By default, the system date is taken as Voucher Date.
						 Note : The date should be within the constraints of the Financial Year.
Paid To             	 	 No              The Party name or name of person to whom the amount is paid.
Inter Branch        	 	 No              Select this option to make interbranch payment.
Bank Account        		 Yes             Account to which amount is to be credited.
Amount Paid         	 	 Yes             Amount to be credited to the bank. This is the net amount for which the payment is dispersed.
Account Info - D or C	  	 Yes             D - Debit, C- Credit
Account Info - Account    	 Yes             Ledger Account - Account to which amount is to be credited/Debited based on *D or C*.  
Account Info - Debits     	 Yes             Amount to be debited to the ledger account. Required if *D or C * is *D*.
Account Info - Credits    	 Yes             Amount to be credited to the ledger account. Required if *D or C * is *C*.
Narration                 	 No              This is an optional field. Press Ctrl **+** A to bring the default narration - *Being amount paid to (Paid To)*
Debit/Credit Diff        	 No              This will show the Credit and Debit total Difference.
Cheque#                   	 No              The cheque # through which the payment is dispersed.
Cheque Date               	 No              The date that is mentioned on the cheque leaf issued to the party.
=======================		 =============   ===============================================

Click on |saveImage| to save your changes.

Click on |postImage| to change status of the voucher and close. The Bank Payment Collection will now display the newly created Bank Payment.

Click on |deleteImage| to delete the saved voucher. This will permanently delete the voucher from the system. Posted voucher cannot be deleted.

To delete posted voucher, unpost them by clicking on |unpostImage| Then delete the voucher.
