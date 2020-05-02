Retail Price based Price Lists
==============================

Almost all products traded or manufactured and sold by a business, undergo price revisions. Such price revisions change the Minimum/Acutal Retail Price of the product. The topic for discussion here is how to manage such Retail Price changes over a period of time while holding inventory of Stock Item at multiple Retail Price. 

Complications arise when there is a change in the price and the business is carrying inventory of items before price revisions and also manufacturing stock items with new revised prices. This problem is further complicated by introduction of multiple stock locations/warehouses where the same stock item is carried at multiple prices.

In doing the following changes, we should not disturb the existing option of **Fixed Price**. This is the most simplest of Price Types and is required by most of the implementations and used for Trading activity. However, manufacturing would require complex structures to be implemented.

To enable Retail Price feature at the time of calculating the Order/Invoice rate, we will build the following structure in coreERP. 

    1. Create a new Price Type called **Retail Price** in Stock Item master. 
    2. Create a new master to store *Retail Price* for each Material (rp_id). Only materials with *Retail Price* would be listed here. In the list, display imediate Previous Price and Current Price. 
        - st.rp
            * rp_id (bigint)
            * rate_pu (numeric(18,4))
            * material_id 
            * apply_from (Date)
            * sl_ids (BigInt[])
            * Unique Key for material_id + apply_from
    
    3. In the procedure **st.fn_mat_info_sale** we suitably modify to pick-up the related sale rate based on Price Type and Stock Location. If we get multiple stock locations, where this retail price is applicable, then we need to provide feature in Order/Invoice for user to select proper Retail Price. 
 
