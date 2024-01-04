# Customer-Bank-Statement
ETL Project using Tableau prep


# Requirements
we have three tables: Transactions Details, Transactions Path And Account Information:
![10Input](https://github.com/MohamedAtef3155/Customer-Bank-Statement/assets/126327548/9f803dd7-58bf-4e72-9eb1-cd494be98fb5)

We want to create table that track customer transactions and balance after (Customer Bank Statment).

Output should look like this:

![11Output](https://github.com/MohamedAtef3155/Customer-Bank-Statement/assets/126327548/8c26b50a-eeca-45d6-a9eb-931a3d8379a1)


# Project Overview

![3Full Flow](https://github.com/MohamedAtef3155/Customer-Bank-Statement/assets/126327548/ed98818a-de46-4645-982c-e5372b194ff4)


# Project Steps

# Step 1 -Combine Transaction Details & Path
First I want to input the Transaction Path and Transaction Details tables into the workflow. For the Transaction Path we want to replace any '_' with a space. We can utilise the Rename Fields functionality for this.

For the Transaction Details we want to filter to exclude 'Y' from the 'Cancelled?' field and then remove the 'Cancelled?' field. 
We are then ready to join our tables together using an inner join on Transaction ID. 

Our table should now look like this:

![4Join Transactions](https://github.com/MohamedAtef3155/Customer-Bank-Statement/assets/126327548/8afa56c1-a9ad-43b5-add3-8a34e6bfdff9)

# Step 2 - Transactions In & Out
We now need to split the transactions into incomings and outgoings.
For this we want to create two separate branches Incoming Transactions where we remove the Account From field And Outgoing Transactions where we remove the Account To field and also make the values negative by multiplying by -1
Then we aggregate both at the level of (Account And Date).

![5Aggregated From Value](https://github.com/MohamedAtef3155/Customer-Bank-Statement/assets/126327548/1c0a8788-2cd0-4e53-8b40-bbdc00e8a36c)

Then we union both Tables so we have 3 columns (Account - Transaction Value - Transaction Date )




# Step 3 - Prepare the Account_information Table

I Just did some cleaning and removed unwanted columns.
Table now looks like this:

![6Account Information Table](https://github.com/MohamedAtef3155/Customer-Bank-Statement/assets/126327548/939db311-af0b-4a67-b00c-dcafff26912f)

# Step 4 - Union (Transactions & Account )

![7Account and Transactions](https://github.com/MohamedAtef3155/Customer-Bank-Statement/assets/126327548/6fea6046-a980-452d-b07e-567feb25c9b1)


# Step 5 -  Creating Calculated Field and Cleaning Data for Output Extraction

Since we have Income transaction as "+", outgoing transactions as "-" and we already have customers starting balance
All I need is to create running sum column for each customer (partition):

![8Calculated Field balance after Transactions](https://github.com/MohamedAtef3155/Customer-Bank-Statement/assets/126327548/2a10759a-d339-448f-a7c1-69ca7d6af22d)

Final table looks like this (image filtered on account number 10011977):

![9Final Table](https://github.com/MohamedAtef3155/Customer-Bank-Statement/assets/126327548/83e81927-3835-44ee-98c2-afc7a886b8ce)

Finally we can easily track our customer balance and transactions!








