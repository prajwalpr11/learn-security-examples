# information disclosure

This example demonstrates information disclosure by injecting malicious query objects to a NoSQL database.

## Steps to reproduce

1. Install all dependencies

    `$ npm install`

2. Insert test data in the MongoDB database. Make sure the mongod is up and running by typing the `mongosh` command in the termainal. If mongod process is up then you will see that the connection was successful. Command to insert test data:

    `$ npx ts-node insert-test-users.ts`

This will create a database in MongoDB called __infodisclosure__. Verify its presence by connecting with mongosh and running the command `show dbs;`.

2. Start the **insecure.ts** server

    `$ npx ts-node insecure.ts`

3. In the browser, pretend to be a hacker and type a malicious request

    ```
        http://localhost:3000/userinfo?username[$ne]=
    ```

4. Do you see user information being displayed despite the malicious request not having a valid username in the request?

## For you to do

Answer the following:

1. Briefly explain the potential vulnerabilities in **insecure.ts**
<ul>
<li>The `username` parameter from the query string is directly used in the `User.findOne` query without validation or sanitization, allowing malicious inputs to bypass intended logic.</li>
<li>No input validation is implemented to ensure that the `username` is in the expected format, making the application vulnerable to NoSQL injection attacks.</li>
<li>Using user-supplied data directly in the query opens up the database to malicious injection, which can manipulate the query to retrieve unintended data.</li></ul>

2. Briefly explain how a malicious attacker can exploit them.
<ul>
<li>By crafting a query like `username[$ne]=`, an attacker can bypass the intended filtering logic, retrieve unintended records, and disclose sensitive user information.</li>
<li>Injecting malicious query parameters enables attackers to access data without providing a valid username or authentication credentials.</li>
<li>Repeatedly exploiting the lack of validation could allow an attacker to extract a significant amount of data from the database.</li></ul>

3. Briefly explain the defensive techniques used in **secure.ts** to prevent the information disclosure vulnerability?
<ul>
<li>Input validation is implemented to ensure that the `username` parameter is of type string before it is used in the database query.</li>
<li>The `username` is sanitized by removing non-alphanumeric characters, preventing malicious query objects like `$ne` from altering the query logic.</li>
<li>Any invalid or malformed inputs return a `400 Bad Request` response, ensuring invalid requests are rejected early in the process.</li>
<li>Error handling with a `try-catch` block ensures that database errors are logged and a generic error message is returned to the client, preventing sensitive information leakage.</li></ul>