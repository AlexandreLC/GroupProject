---
title: "Analyzing eCommerce data"
author: "Tommy Wang, Sadaf Sultan, Jean-Francois Lafon, Alexandre Le Cann"
output:
  html_document:
    css: AnalyticsStyles/default.css
    theme: paper
    toc: yes
    toc_float:
      collapsed: no
      smooth_scroll: yes
  pdf_document:
    includes:
      in_header: AnalyticsStyles/default.sty
always_allow_html: yes
---



We have gathered data from Lazada sales for 3 days. The goal of our analysis is to segment our customers 

#Importing the data

First step is of course to import the data.

Here swe have two data files: 

* "Anonymized_transactions_all.csv" which compiles all transactions that happened between January 1st and February 7th. Each row of the file corresponds to the purchase of one item (when various items are bought during the same transaction, several lines are created). The different fields listed in the file are:
  + Order numer: unique identification of the order
  + SKU number: identifier of an item in the ecommerce database
  + Unit price of the item purchased
  + Price paid for the item (can be different from the item price if there are promotions for example)
  + Date of the order
  + Time of the order
  + Payment method used
  + Price paid for the whole order
  + Anonymized name of the customer ("Anonym"+several digits)
  
* "Categories_all.csv" contains a breakdown of each item into sub categories. Each line corresponds to an item that can be purchased through the ecommerce website. The different fields are:
  + SKU number: the unique identifier for each item
  + Item description: a sentence describing the item
  + Category of the item
  + Sub-category of the item
  + Sub-sub-category of the item





After having imported the data, the idea is to create a new variable, that sums up the items bought by each customer.


```r
# We have to figure out at first the list of customers and of items sold
Customer_list = as.character(Transaction_data[1, "Anonym"])
ItemsSold_list = as.character(Transaction_data[1, "SKU"])
for (i in 2:nbTransactiona) {
    if (!(as.character(Transaction_data[i, "Anonym"]) %in% Customer_list)) {
        Customer_list <- c(Customer_list, as.character(Transaction_data[i, "Anonym"]))
    }
    if (!(as.character(Transaction_data[i, "SKU"]) %in% ItemsSold_list)) {
        ItemsSold_list <- c(ItemsSold_list, as.character(Transaction_data[i, 
            "SKU"]))
    }
}

# The number of unique customers and different items sold is then easy to
# compute
nbCustomers = length(Customer_list)
nbItemsSold = length(ItemsSold_list)

# Then we create a matrix that we will populate with the actual transactions
Sales_Cust_Items = matrix(0 * 1:(nbCustomers * nbItemsSold), nrow = nbItemsSold, 
    ncol = nbCustomers)
colnames(Sales_Cust_Items) <- Customer_list
rownames(Sales_Cust_Items) <- ItemsSold_list

for (i in 1:nbTransactiona) {
    itemsold = as.character(Transaction_data[i, "SKU"])
    customer = as.character(Transaction_data[i, "Anonym"])
    Sales_Cust_Items[itemsold, customer] <- Sales_Cust_Items[itemsold, customer] + 
        1
}
```

We have created a variable Sales_Cust_Items

#Basic data visualizations
##Distribution of the number of items ordered per customer
**To be completed**

##Distribution of number of orders per customer
**To be completed**

#Segmentation of the customers
Our data goes from January 1st to February 7th. The goal of this section is toi segment the customers to be able to predict relatively reliably the behavior of customers who have bought an item in January and who buy another one in February.
Here we want to segment the customers that ordered more than one item in January. The idea is to be able to 
