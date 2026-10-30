# Topic 9 - Testing, Debugging, and Optimizing API Integrations

## Project
N8N Performance Tuning Workflow | 30 October 2026

## Objective
Demonstrate independent HTTP API requests in n8n, aggregate their outputs, and validate an API using Postman environment variables and JavaScript assertions.

## Workflow architecture
Manual Trigger -> Fetch User 1 (GET /users/1) -> Merge (Input 1)
Manual Trigger -> Fetch User 2 (GET /users/2) -> Merge (Input 2)
Merge mode: Append; inputs: 2.

## Setup and execution
1. Create a Manual Trigger.
2. Create two HTTP Request nodes using GET and no authentication:
   - https://jsonplaceholder.typicode.com/users/1
   - https://jsonplaceholder.typicode.com/users/2
3. Connect both directly to the trigger, then to separate Merge inputs.
4. Set Merge mode to Append and Number of Inputs to 2.
5. Execute the full workflow and inspect the Merge output.

## Verified n8n results
- Fetch User 1: successful response, id 1, Leanne Graham.
- Fetch User 2: successful response, id 2, Ervin Howell.
- Merge: successful, two output items.
- Canvas screenshot: green success checks on all four nodes.

## Postman validation
Environment selected: ReqRes_Sandbox_Env (name retained although endpoint was changed).
- base_url = https://jsonplaceholder.typicode.com
- user_id = 2
- Request: GET {{base_url}}/users/{{user_id}}
- Observed HTTP status: 200 OK; observed response time: 342 ms in latest screenshot; returned id: 2.
- Earlier ReqRes request returned HTTP 429 anonymous demo rate limit; the test endpoint was changed to JSONPlaceholder.

## Post-response tests
```javascript
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});
pm.test("Response time is below 2000ms", function () {
    pm.expect(pm.response.responseTime).to.be.below(2000);
});
pm.test("User ID matches environment", function () {
    const jsonData = pm.response.json();
    pm.expect(jsonData.id).to.eql(Number(pm.environment.get("user_id")));
});
```

## Latest n8n execution evidence
- Execution ID: 549746
- Timestamp: 30 October 2026, 16:39:23
- Status: Succeeded
- Total duration: 216 ms
- Execution data: 5 KB
- All four nodes display successful completion.
- This is one branched run, not a measured sequential-vs-branched improvement.

## Suggested final submission
Submit the n8n exported workflow JSON, Postman collection/environment export (with secrets removed), README, evidence PDF, and a short demonstration video. The branched execution timing screenshot is included.
