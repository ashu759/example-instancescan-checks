# example-instancescan-checks

Open-Sourced community contributed and owned repository for Instance Scan Definitions. [ServiceNow Instance Scan](https://docs.servicenow.com/bundle/rome-servicenow-platform/page/administer/health-scan/reference/hs-landing-page.html) The checks contained in this repository are therefore considered "use at your own risk" and will rely on the open-source community to help drive fixes and feature enhancements via Issues and community members issuing and reviewing PRs. ServiceNow is not providing or authenticating these definitions. Occasionally, ServiceNow employees may choose to contribute to the open-source project as members of the community as they see fit, this does not constitute a service or product from ServiceNow.

# Checks in this repository

## Category: Manageability

### Duplicate Script Include Name
This uses a table check to find other Script Includes having the same API name. Technically this is possible, but causes issues as there is no way to control which Script Include will be instantiated when being called.

### Product Catalog without Product Models
Catalog Items in the Product Catalog should be created from the underlying Product Model and this association should be kept intact.

### Do not use hard-coded sys_ids
Hard-coded sys_ids can lead to unpredictable results (sys_ids may differ between instances) and can be difficult to track down.

### Hard coded Instance URL
Hard coding instance URL is not a best practice as they reduce the usability of your code. The URL remains static and it does not change when you move the code to a different instance or it can get overwritten during clone backs. This can break the related functionalities or create unexpected results.

## Category: Upgradability

### Incident table should not be extended
Check if the baseline restriction to extend the Incident table has been removed and at least one child table extending Incident has been created.

### Choice table should not be extended
Check if the Choice [sys_choice] table has been extended. This is not supported by ServiceNow.

### User table should not be extended
Check if the User [sys_user] table has been extended. This is not recommended and can cause problems when a user needs to be in more than one user table.

## Category: Performance

### getMessage() called in Client Script 
This is a simple table check to find client scripts with use the getMessage function but do not preload messages using the Messages field. As the check does a simple contains query it could produce false-positives if the getMessage is either commented or from another library.

### Glide-API in ACL
This will check ACL rules with operation READ for usage of Glide API calls, i.e. GlideRecord and GlideAggregate as these can cause significant performance impact. s the check does a simple contains query it could produce false-positives if the getMessage is either commented or from another library.

### Cache flushed as part of scripts
This is using some advanced linter check to find usages of gs.setProperty or gs.cacheFlush. Both functions will trigger a cache flush and thus cause performance impacts. 

### Global Business Rules
This is a simple table check to find Business Rules that are global. Global Business Rules have no condition or table restrictions and load on every page in the system. Most functions defined in global Business Rules are fairly specific, such as an advanced reference qualifier on one field of one form. There is no benefit to loading this kind of script on every page.

### Global Client Scripts 
This is a simple table check to find Client Scripts that are global. Global client scripts have no table restrictions; therefore they will load on every page in the system introducing browser load delay in the process.

### Using Synchronous AJAX calls in client script
Synchronous usage of AJAX calls (getXMLWait) pauses the browser interaction until data is retrieved from the server side and thereby reducing user experience. Using them in an onLoad or onChange client side scripts is not a best practice recommendation and can create performance issues for your users.

### Business Rule without any conditions
A business rule is triggered whenever a user opens a list or form view or when a user inserts/updates or deletes a record. Without any conditions added, it will always evaluate to true and triggerred at all times. This can add to the performance increasing server processing time and hence it is recommended to restrict business rule execution by either adding a condition or filter condition or a role condition and preventing its execution unless it is necessary.

## Category: Security

### Tables without ACLs
This check searches for any custom table if there exists at least one ACL record. If not a finding is raised.

### Scripted REST API without Authentication
Scripted REST APIs should be not be public but enforce  access controls and require additional access to data. 

### Avoid the eval function
Improper use of eval() opens up your code for injection attacks and difficulties with debugging.

### Do not use gr as a variable name
The platform is Javascript and a lot of code is run in a global variable scope. A "gr" defined in one business rule can clobber another "gr" defined in some other script.

## Category: User Experience



# Additional resources

Please check these additional links for more information and details:

- [Platform Academy Foundation #5: Instance Scan Overview](https://community.servicenow.com/community?id=community_event&sys_id=f44eb0c0db82f410019ac22305961950)
- [Mark Roethof’s Blogs](https://community.servicenow.com/community?id=community_blog&sys_id=14e51965db2200d013b5fb24399619fb#is)
- [Live Coding Happy Hour – Instance Scan in Quebec (2021-03-12)](https://youtu.be/_cPlWnh1Z68)
- [Introduction to ServiceNow HealthScan and Instance Scan](https://nowlearning.service-now.com/lxp?id=overview&sys_id=e4c538231b0d6c505b2699f4bd4bcb6f&type=course)
- [K21 CCL1062 – Writing custom instance scan checks](https://nowlearning.service-now.com/lxp?id=overview&sys_id=8d9db4921b7fe010a5e699b1b24bcbdd&type=course)
- [Quebec Instance Scan](https://developer.servicenow.com/blog.do?p=/post/quebec-instancescan/)





