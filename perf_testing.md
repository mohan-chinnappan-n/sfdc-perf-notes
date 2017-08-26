### SFDC performance Testing

[Ref](https://developer.salesforce.com/blogs/engineering/2013/09/performance-testing-force-com-application.html)


The Force.com platform includes a framework for creating and running test classes.


Question:
  
 How can we test whether our application can scale to meet the needs of our growing user base? 

Need:

When we need to take care for larger volumes of data and more demanding application conditions, we need to move a level beyond unit testing.

 

### Types of Testing:



#### Stress testing

Stress testing is used to gauge performance during **abnormal or extreme conditions**, which are often caused by **diminished resources** or **excessive requests**. The main goal of stress testing is to find the boundaries of a system.
	
#### Load testing

Load testing is used to gauge performance **under expected conditions with varying loads** (i.e., increasing numbers of users or transactions) and configurations.
	
#### Performance testing

Performance testing measures performance **under a particular workload, and includes both stress testing and load testing**.

-------

### Loading Testing 

The Force.com platform serves  billions of transactions daily with an average response time of under 300 milliseconds, and salesforce.com tests and monitors the platform to ensure that it has excess capacity.

[Here is current transaction graph](https://status.salesforce.com/performance)


Also guards against highly inefficient code by using governor limits, which help ensure both that your users have a consistent application experience and that resources are available to all salesforce.com customers.

Our goal should be to use **load testing** instead of stress testing.

So you do not need to do stress testing of the Force.com platform.
If you try to stress test, you’re likely to **reach governor limits** or have salesforce.com terminate your tests so they don’t affect shared resources on the multitenant Force.com platform. 

-----

Load Testing:

if you have highly customized code or large transactional volumes.
The goals of performance testing on the Force.com platform are to:


- Ensure that custom application or business logic meets your desired response times
- Determine whether your estimated transaction throughput of anticipated loads is accurate


**Don’t** benchmark performance in a **sandbox organization** anticipating the same performance in your production organization. Due to variables such as data distribution and user load patterns, which can be fluctuate because of database caching and the queue performance associated with asynchronous processing (@future calls), requests might perform faster or slower in production.

Sandbox use:

You can use performance testing in sandbox to **uncover** conflicts between requests. These conflicts might include locking issues that are caused by **data skew** or user requests that stall because of an integration’s DML operations. In addition, you can load test in your sandbox organization to test the performance of external Web services.


Before you can performance test in your sandbox environment, you must first create a **test plan and submit it to salesforce.com Customer Support**.

The test plan should include:

- A description of your tests
- Record counts of the objects in your tests
- The test scripts and tool (e.g., LoadRunner) that your tests will use
- Your estimated testing loads
- When you plan to conduct the tests
- Your sandbox organization’s and production organization’s IDs
- Contact information that salesforce.com can use to reach you during your tests


Once salesforce.com Customer Support approves your test plan, salesforce.com can help monitor your tests. The performance test request should be submitted at least 2 weeks in advance.

#### Test Plan

Identify your key business processes and build test scripts around them.

 You can use tools like **LoadRunner** to **simulate** concurrent users and put your application through real-life loads.  If you have inbound/outbound integrations, ensure that they are included in your test scripts. 
 
  Run a very low-volume test to validate these scripts. You can then compare the metrics from this small test with the baseline numbers from the unit tests.
  
 -------
 
  
**TPS**

To estimate your load, use TPS (transactions per second) as a metric. 

Example:

 
A service cloud customer has 3,000 support representatives handling 5 cases per hour, and each case executes 10 transactions (for Visualforce pages, Javascript Remoting, SOQL, workflow rules, and triggers).

In addition, the customer application has 1,000 inbound/outbound API calls per hour, each of which executes 10 transactions.

To calculate TPS, you would make the following calculation.

```
((15,000 cases * 10 transactions) + (1,000 API calls * 10 transactions)) / 3,600 

// this will be equal to: 44.4 TPS

// When calculating TPS, remember to factor in the potential wait time and pauses between requests.
//  Realistic load values for the rounds of tests would be 22 TPS (~0.5 x 44.4), 33 TPS (~0.75 x 44.4), and 44 TPS. 

// You might also want to test for growth, which would involve testing at 1.5 or 2 times your calculated TPS. (2 times 44 = 88 TPS)

```


#### Monitoring Each Round of Testing

Monitor each round of testing as the load increases and compare your results with your projected metrics. Salesforce.com might contact you to abort the tests if their performance approaches unhealthy threshold!


Proper unit testing, accurate test scripts, and realistic estimated loads can help you avoid this scenario, and salesforce.com Customer Support can provide metrics from the salesforce.com logs to help you with your post-test analysis.













------







