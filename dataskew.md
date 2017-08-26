## Data Skew 

### Account Skew
[Ref](https://developer.salesforce.com/blogs/engineering/2012/04/avoid-account-data-skew-for-peak-performance.html)

What is Account Data Skew?

When you have a **very large number of child records** (say 10k) associated to the **same account** in Salesforce, we call that *data skew*

Example:

When you have a whole bunch of **unassigned contacts** and need some place to **park** them, it’s tempting to put them all under one account name **Unassigned**. 

This can cause record locking and sharing performance.

When we are updating a large number of **contacts** under the same **account** in **multiple threads**:

- For each update the **system locks** both the **contact** being changed and its **parent account**, to maintain integrity in the database (RDBMS).
- Each lock is held for a **very short time**, because *all the updates are trying to lock the same account*, there is a high risk that an update will fail because a previous one is still *holding the lock on the account*.
- Sharing: when you are changing the owner of that **account**, we may need to examine every **one of those child records** and adjust their sharing as well. That may include **recalculating the role hierarchy and sharing rules**. This can take a while if we are talking about hundreds of thousands of child records.

Solution to **Parking lot** usecase:

You can create a **larger number of accounts** and modify your integration code or use a trigger on the child object to distribute the child records across this **collection of these parking accounts**. This will help to avoid the **locking and performance issues** you might experience with a highly skewed account.


-----
### Ownership Skew

[Ref](https://developer.salesforce.com/blogs/engineering/2013/04/managing-lookup-skew-to-avoid-record-lock-exceptions.html)

When a large number of records with the **same object type are owned by a single user**, this imbalance causes something called **ownership skew**.

Ownership skew causes **performance problems**, which can surface when you’re managing your **role hierarchy and sharing rules**.

-----


### Lookup Skew

Lookup skew happens **when a very large number of records are associated to a single record in the lookup object**.  Because you can place lookup fields on any object in Salesforce, lookup skew can create problems for any object within your organization.

Example:

Company ACME has 500K accounts in its production instance of Salesforce. The company’s sales organization wants to track an **educational achievement level**, so it configures a custom object (Achievement_Level__c) to store an achievement level and a description.

Next, it adds the Achievement_Level__c to the Account object as a **lookup field**.
![lookup to account](http://res.cloudinary.com/hzxejch6p/image/upload/v1371103040/lm2ifembrc3wcn1ci2bq.png)

Company ACME then creates three achievement levels, into which all of its accounts must be categorized.

![picklist](http://res.cloudinary.com/hzxejch6p/image/upload/v1371103041/vj657ofr1fx3hehbemgz.png)

Finally, Company ACME loads the update to its account records so that it can associate each account with an achievement level. 

We just created Lookup Skew!

Lookup skew occurs when a very large number of records **look up to the same record on a lookup object** regardless of whether that lookup exists on a single object or across many objects.

Even if the distribution between the lookup records is evenly split (500/3), you will have 166,000 accounts looking up to each of the three achievement levels.


 

**Under the hood:**

Lookup fields in Salesforce are essentially **foreign key** (FK) relationships between objects



What is FK?

In the context of relational databases, a foreign key is a field (or collection of fields) in one table that uniquely identifies a row of another table or the same table. 

Every time a record is inserted or updated, Salesforce must lock the target records that are **selected for each lookup field**; this practice ensures that, when when the data is committed to the database, its integrity is maintained.
 
 
If Salesforce did not lock each of the lookup records while saving, and one record was deleted in the middle of the save operation, you would face a **referential integrity issue**. Under normal circumstances, save operations execute so quickly that you don’t encounter locks. 

 However, when you add custom code and a large volume of data simultaneously in an automated process, you might encounter **lock exceptions** that cause failures when you try to insert or update records.
 
 
How to locate the Lookup Skew?

Evaluate objects with:
- A large number of records
- Heavy concurrent insert and update activity

Once you have identified the objects which are at risk, you need to look at each of the **lookup fields** on those objects.
 

#### Solutions:

**Reducing Record Save Time:**

Increasing record save performance reduces the duration of locks, which reduces or eliminates lock failures, and has the added benefit of providing an enhanced end user experience!

Increase the performance of synchronous apex code:

 - tune your triggers for peak performance by consolidating code into a **single trigger per object** and following Apex best practices.

 
Remove unnecessary workflow. The workflow lengthens the lock duration, which in turn increases the lock failure rate. 

Only process what’s required–move any code that isn’t required immediately upon save to asynchronous processing.  Locks are held the entire time your custom code is executing, so processing logic that is not critical during the save will increase the lock failure rate.


**Use Picklist Field instead of Lookup Field:**

When you have a **relatively low number of lookup values**, it’s generally a good idea to use a **picklist field** rather than a **lookup field** to define those values. 

In many cases, you cannot substitute a picklist for a lookup field if you have **additional fields and other data on the lookup records**. The main point here is that you should generally **avoid using a lookup field to represent data that can be accommodated by a picklist**.
 
 


 







 





