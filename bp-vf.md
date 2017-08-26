## Best Practices for Improving Visualforce Performance

[Ref](https://developer.salesforce.com/docs/atlas.en-us.pages.meta/pages/pages_best_practices_performance.htm)

 If your users experience:
 
 - delays
 - unexpected behavior
 - other issues specifically around Visualforce
  
there are several actions you can take to not only improve their experience, but to also make for improved coding.

Steps:

1. Make sure it is not an one-off isoloated issue: The problems aren’t confined to a single user’s computer by testing expected Visualforce functionality on other machines as well as using different browsers.
2. Make sure the slow load times aren’t the result of a network issue by checking the load time of other Salesforce pages. If they’re also slow, it could be the result of bandwidth or latency issues to Salesforce. To check on the status of the Salesforce servers, visit [trust.salesforce.com](https://trust.salesforce.com/en/). You should also check the status of your network connections and ensure they’re functioning properly.
3. You’re following general Web design best practices, such as the minification of JavaScript and CSS, optimizing images for the Web, and **avoiding iframes whenever possible**.
4. You’ve used the Developer Console to step through the request and determine which items in the request used the most system resources
5. The **view state size** of your Visualforce pages must be under 135 KB. By reducing your view state size, your pages can load quicker and stall less often.
6. Use the transient keyword in your Apex controllers for variables that aren’t essential for maintaining state and aren’t necessary during page refreshes.
7. If you notice that a large percentage of your view state comes from objects used in controllers or controller extensions, consider refining your SOQL calls to return only data that's relevant to the Visualforce page.
8. If your view state is affected by a large component tree, try reducing the number of components your page depends on.
9. Cache any data that is frequently accessed, such as icon graphics.
10. Avoid SOQL queries in your Apex controller getter methods.
11. Limiting the data coming back from SOQL calls in your Apex controllers. For example, using AND statements in your WHERE clause, or removing null results

12. Taking advantage of pagination with a list controller to present fewer records per page

13. **Lazy load** (defer initialization of an object until the point at which it is needed) Apex objects to reduce request times. 

14. Consider moving any JavaScript outside of the <apex:includeScript> tag and placing it into a <script> tag right before your closing <apex:page> tag. The <apex:includeScript> tag places JavaScript right before the closing <head> element; thus, Visualforce attempts to load the JavaScript before any other content on the page. However, you should only move JavaScript to the **bottom of the page** if you’re certain it **doesn’t have any adverse effects** to your page. For example, JavaScript code snippets requiring document.write or event handlers should remain in the <head> element.

15. In all cases, Visualforce pages must be under 15 MB.

16. Concurrent requests are long-running tasks that could block other pending tasks. To reduce these delays: Action methods used by <apex:actionPoller> should be lightweight. It’s a best practice to avoid performing DML, external service calls, and other resource-intensive operations in action methods called by an <apex:actionPoller>. Carefully consider the effect of your action method being called repeatedly by an <apex:actionPoller> at the interval you specify, especially if it’s used on a page that will be widely distributed, or open continuously.
17. Increase the time interval (less frequently) for calling Apex from your Visualforce page. For example, when using the <apex:actionPoller> component, you could adjust the interval attribute to 30 seconds instead of 15.
18. Move non-essential logic to an asynchronous code block using Ajax.
19. By using the with **sharing** keyword when creating your Apex controllers, you have the possibility of improving your SOQL queries by **only viewing a data set for a single user**.
20. If your page contains many fields, including large text area fields, and has master-detail relationships with other entities, **it may not display all data due to limits on the size of data returned to Visualforce pages** and batch limits. The page displays this warning: ```You requested too many fields to display. Consider removing some to prevent field values from being dropped from the display.```
21. To prevent field values from being dropped from the page, remove some fields to reduce the amount of data returned. Alternatively, you can write your own controller extensions to query child records to be displayed in the related lists.

	


