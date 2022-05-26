# Obtaining the Password Hashing Application
## Background:
A password hashing application in Golang was implemented in this assignment by JumpCloud. I was assigned to create test plans, and test cases, test the application, execute the test cases, and report a defect that was purposefully left in the application. Here, I am presenting my test plan, Test cases, Test report, and bugs which I found while testing. A automated JSON file is created for Postman. The scenarios are marked as "automated" under the test cases topic.


## Specification: 
The following is the requirement specification that was used in building the password hashing application. It describes what the application should do. 

- When launched, the application should wait for http connections. 
- It should answer on the PORT specified in the PORT environment variable. 
- It should support three endpoints: 
   1. A POST to /hash should accept a password. It should return a job identifier immediately. It should then wait 5 seconds and compute the password hash. The hashing algorithm should be SHA512. 
   2. A GET to /hash should accept a job identifier. It should return the base64 encoded password hash for the corresponding POST request. 
   3. A GET to /stats should accept no data. It should return a JSON data structure for the total hash requests since the server started and the average time of a hash request in milliseconds. 
- The software should be able to process multiple connections simultaneously. 
- The software should support a graceful shutdown request. Meaning, it should allow any in-flight password hashing to complete, reject any new requests, respond with a 200 and shutdown. 
- No additional password requests should be allowed when shutdown is pending


## Interacting with the Application: 
The following are examples that would/should generate similar returns - the job identifier does not need to conform to a specification. 

- Post to the /hash endpoint | curl -X POST -H "application/json" -d '{"password":"angrymonkey"}' http://127.0.0.1:8088/hash
JumpCloud Confidential, please do not distribute this document in any form.** > 42 

- Get the base64 encoded password | curl -H "application/json" http://127.0.0.1:8088/hash/1  
zHkbvZDdwYYiDnwtDdv/FIWvcy1sKCb7qi7Nu8Q8Cd/MqjQeyCI0pWKDGp74A1g== 

- Get the stats | curl http://127.0.0.1:8088/stats > {"TotalRequests":3,"AverageTime":5004625} 

- Shutdown | curl -X POST -d 'shutdown' http://127.0.0.1:8088/hash 200 Empty Response 


## Test Plan:
Following is the scenario for testing in this test plan:

- TP_01 : Verify the application supports Post to the /hash endpoint (Automated)
- TP_02 : Verify the application supports Get the base64 encoded password endpoint
- TP_03 : Verify the application supports Get the stats endpoint (Automated)
- TP_04 : Verify POST to /hash accepts password and returns job identifier (Automated)
- TP_05 : Verify invalid POST to /hash if payload password field is empty (payload: password empty) | (Automated)
- TP_06 : Verify invalid POST to /hash if the payload is empty (Automated)
- TP_07 : Verify job identifier is returned immediately after password is accepted
- TP_08 : Verify application waits 5 seconds before computing the password hash
- TP_09 : Verify application returns a BASE64 encoded password for a correct job identifier GET test
- TP_10 : Verify GET to /stats returns average time of total requests (should be around 5 sec)
- TP_11 : Verify POST to /stats should not work (Automated)
- TP_12 : Concurrent POST requests to /hash should return unique job identifiers
- TP_13 : POST to /hash with shutdown should shut the app down
- TP_14 : In process of shutdown the inflight request should complete
- TP_15 : Verify Get request for shutdown should throw the bad request error (Automated)
- TP_16 : Verify GET to /stats throws bad request error when json payload is on body

## Test Cases:
In test cases, I tried to cover Project Name, Reference Document, Created By, and Date of Creation, and it is in the below format. 

Test Case ID | Test Scenario | Test Case | Steps | Test data | Expected Result | Status (Pass/Fail)

## Test/Automation Execution:
The test is based on the above scenarios and test steps. There were no screenshots provided. For the manual testing there is a “Manual tests.json” file. Import to Postman, and follow the steps from the scenario. To automate testing, upload the "automated tests.json" file to Postman, click on the scenario, and send the request.


## Bug:
During testing, I discovered some bugs, which are as follows. 

- Defect_01 : Job Identifier is returned immediately after password is accepted
- Defect_02 : Verify GET to /stats returns average time of total requests (should be around 5 sec)
- Defect_03 : Verify POST to /stats should not work
- Defect_04 : Verify Get request for shutdown should throw the bad request error
- Defect_05 : Verify GET to /stats throws bad request error when json payload is on body
- Defect_06 : Verify invalid POST to /hash if payload password field is empty (payload: password empty)

## Conclusion:
The exercise involved creating test cases, testing the application, executing the test cases, and reporting a problem with a password hashing application. In addition, I presented a possible automation code for some scenarios. The goal was to comply with the assignment requirements. Thanks for taking the time to read it. 


***
