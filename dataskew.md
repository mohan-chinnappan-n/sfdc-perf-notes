## Data Skew 

[Ref](https://developer.salesforce.com/blogs/engineering/2012/04/avoid-account-data-skew-for-peak-performance.html)

What is Data Skew?

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
 





