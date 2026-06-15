# Salmon-Recall
Analyzed Hill’s Science Diet recall by identifying salmon-based SKUs with a dynamic FILTER query in Excel. Built a synthetic inventory dataset to match affected items to store stock, then identified impacted locations and contacts, enabling efficient recall communication and reverse logistics planning. 


# Business Question
- Hill’s Science Diet just announced a recall of all salmon products due to some cross contamination. We need an item number list of their salmon products so we can contact the impacted stores. 
- **BONUS:** We want to find out which stores have the product recalled so we can contact them. This info will also be useful to determine the transportation costs and quantity expected to need to ship back to Hill’s.

# Finished Product
<img width="915" height="565" alt="image" src="https://github.com/user-attachments/assets/43c4fc21-4670-469e-8c00-9c188e974366" />

# Goals
To return a list from the master items list of products we carry from Hill’s Science Diet that have Salmon ingredients. Then calculate how many of each item is in our stores and pull the contact info from impacted stores.  

# Program
Excel

# Data Source
I got this dataset from Kaggle! Here is the link [Premium Dog Outlets](https://www.kaggle.com/datasets/scoeyd/premium-dog-outlets). The data is synthetic but from my personal experience in a warehouse environment, I found its vastness and general layout to be very familiar! 

# Process
For the primary question: I addressed pulling a list from our master items database that includes all of the salmon food items affected by the recall using the following formula:  <br>
=FILTER(CHOOSECOLS(Product3,1, 2, 8),ISNUMBER(SEARCH("Salmon",Product3[Flavor]))*(Product3[Brand]="Hill's Science Diet")) <br>

<img width="932" height="132" alt="image" src="https://github.com/user-attachments/assets/9ce463cc-5b41-40cc-8464-3f23a6cfaebc" /> <br>

# Example Use Case: Bonus Execution
I completed the dynamic array filter quickly but decided I wanted to expand the scope. I thought about what the next steps would be if I were a product flow specialist getting informed of this information. So, I needed to generate more data. I created synthetic store numbers and contact info. Then, for the store stock quantities, I used the following formula from [this tutorial](https://www.exceldemy.com/randomly-select-from-a-list-in-excel/). <br>

=INDEX(B5:B12,RANDBETWEEN(1,8)) <br>

This was perfect because I needed random data that was pulled from our list of pre-existing stores from the contact dataset and a seemingly random list of pre-existing items from the master items database.  <br>

Then I used RANDBETWEEN to generate the quantities that each store would have in stock. I followed this up by removing duplicates to ensure that every store and item randomly assigned only had one pairing. For instance, it wouldn’t make sense for store 664 to have records with 2 differing quantities of item 51. It was also important to simulate a real store environment that would have more than just the items affected by the recall but still ensure that SOME of the recall items were included. The result was a simple but realistic data sheet. <br>

<img width="369" height="567" alt="image" src="https://github.com/user-attachments/assets/39e58e89-13a0-4154-9397-29a3c78f7b87" /> <br>

So, what are we doing with that data? We want to find out which stores have the product recalled so we can contact them. This info will also be useful to determine the transportation costs and quantity expected to need to ship back to Hill’s.  <br>

=SORT(FILTER(CHOOSECOLS(Table4,2,1,3),ISNUMBER(MATCH(Table4[item],'Salmon Slap Query'!B7:B11,0))),1) <br>

This bad boy is an advanced filter used to match the item numbers found in the first query to the larger store inventory dataset we just generated. It sorts the data naturally by store and displays each of the recalled item numbers and current on-hand store quantities. <br>
<img width="277" height="664" alt="image" src="https://github.com/user-attachments/assets/52eb4f14-1538-4d53-9f10-466168add029" /> <br>

Using this, we performed 2 final queries. A simple unique query to get the list of affected stores.  Then an Xlookup on our contact table to retrieve the emails associated.  <br>

<img width="497" height="415" alt="image" src="https://github.com/user-attachments/assets/db877dec-da4e-48aa-8d03-5647acc411e2" /> <br>

This information makes it easy to contact the managers of the stores who can quickly refer to our other query (ideally filtered down to the store the email is being sent to) to remove the product off the shelves, destroy the product or have it shipped back to Hill’s.  
