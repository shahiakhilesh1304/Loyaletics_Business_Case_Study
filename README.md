# Loyaletics_Business_Case_Study
Loyalytics is an AI and analytics company helping some of the world's leading brands solve their complex data challenges. Their end-to-end platform enables companies to leverage the power of data to craft strategies, create engaging customer experiences, and drive measurable business impact. They were voted one of the best CRM solutions providers in India by the CIO Review magazine in the year 2019.

Dataset [Link](https://drive.google.com/file/d/1D5aS3Ynq8RJBtnNZmQWgreqILT6P2dOF/view?usp=sharing)

This dataset has information on more than 500k transactions from 2020 to 2022 at brand A across different stores in UAE and Qatar. 

customer_Id : unique identifier for a customer for a given transaction. Where this is blank, it means that the transaction was a non-loyal transaction (i.e. we do not know who has purchased this product)
current Tier : is a way of categorizing customers based on purchase history

customer_nationaity :	nationality of the customer
date_transacted	: date of transaction
storeId	: store from where the transaction has happened
store_city	: city of where the store is located
transactionId :	similar to a receipt ID in a store, a way of identifying a particular transaction. You will see that this data can be repeated because customers can buy multiple products in a given transaction
itemid :	Id of item
Brand :	A (name of the brand)
itemName : name of the item.
product_category :	category under which the product falls.
high_level_product_category :	broader categories of product_category.
quantity :	total quantity of the item the customer purchased.
total_spend	: total amount the customer spent on that item within that transaction.
signed_up_loyalty_program_date : date that the customer signed up for a loyalty program. If a specific row has customer_id populated, you will see that this column is also populated. where this is blank, it means that the customer has not registered for the loyalty program.
signed_up_app_date : date that the customer signed up on the mobile application. Some customers will not have the app, in which case this will remain blank.



The dataset is available in a CSV file of size 65.7 MB 

Assumptions:
1.	There are negative values in the total spend column, which we assume represents a refund given by the brand.
2.	There are negative values in the quantity column, which we assume that the customer returned the item.
3.	The Total spend column contains values of zero, which we assume represent a free item(freebie) given to the customer by the brand.




BASIC TASK APPLIED ON ABOVE:

Basic Data cleaning:
1.	Add data source filter to remove null transaction ids.
2.	Change the datatype of Itemid field from number to string.  
3.	Assign geographic role country/region to customer nationality field.
4.	Convert Itemid to dimension.  

Create additional fields that will make analyzing the data easier in tableau
1.	Create a field called Registered or not to check whether a customer is registered or not.
○	A customer is considered to be registered only when the customer id field is not null and signed up loyalty program date field is also not null
○	Create a calculated field->name it Registered or not 
○	Use formula 
IF (NOT ISNULL([Customer Id])) AND (NOT ISNULL([Signed Up Loyalty Program Date]))
THEN 
"Registered"
ELSE
"Not Registered"
END

○	We are checking signed up loyalty program date field also because in the dataset we have some customers where customer id is present but we do not have their signed up date.

2.	Create a field called Refund transaction or not to determine whether or not the transaction was a refund
○	Based on the assumption that if the total spend field is less than 0 then it was a refund given to the customer and if the total spend was greater than 0 it was a normal sales transaction and if the total spend value was 0 and quantity was greater than 0 then it was a freebie given to the customer.
○	Create a calculated field and name it Refund transaction or not
○	Use formula- 
IF [Total Spend]<0
THEN
"Refund transaction"
ELSEIF [Total Spend]=0
THEN
"Freebies"
ELSE
"Sales"  
END

3.	Create a field called Item refunded or not to determine whether or not the item was refunded.
○	We are checking whether the quantity field was less than 0 or not, if the quantity is less than 0 then refund else not a refund
○	Create a calculated field and name it  Item refunded or not
○	Use formula  
IIF([Quantity]<0,"Item refunded","Item not refunded")

Note: You would have the following Distinct values after creating the above three calculated fields or when you drag any of the fields either on rows or column shelf you would see following Distinct values:
○	Registered or not 
■	Registered
■	Not Registered
○	 Refund transaction or not
■	Refund
■	Freebies
■	Sale
○	Item refunded or not
■	Item refund
■	Item not refund

4.	For the current tier field edit the Null entry with an alias No current tier. 
5.	For the customer nationality field edit the Null entry with an alias No nationality.



