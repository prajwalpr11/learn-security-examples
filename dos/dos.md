# Denial-of-Service (DoS)

This example demonstrates DoS vulnerabilities and how they can be exploited.

## Steps to reproduce

1. Install all dependencies

    `$ npm install`

2. Ignore if you have already done this once. Insert test data in the MongoDB database. Make sure the mongod is up and running by typing the `mongosh` command in the termainal. If mongod process is up then you will see that the connection was successful. Command to insert test data:

    `$ npx ts-node insert-test-users.ts`

This will create a database in MongoDB called __infodisclosure__. Verify its presence by connecting with mongosh and running the command `show dbs;`.

2. Start the **insecure.ts** server

    `$ npx ts-node insecure.ts`

3. In the browser, pretend to be a hacker and type a malicious request

    ```
        http://localhost:3000/userinfo?id[$ne]=
    ```

4. Do you see the server crashing?

## For you to do

Answer the following:

1. Briefly explain the potential vulnerabilities in **insecure.ts** that can lead to a DoS attack.

The id parameter from the query string is directly used in the User.findOne query without validation or sanitization, allowing attackers to craft malicious queries.

The server does not validate or sanitize the incoming id parameter, enabling malicious inputs like $ne to exploit the database query.

The endpoint does not implement rate limiting, leaving the server vulnerable to being overwhelmed by a high volume of requests.
Errors resulting from invalid queries might be logged or exposed, giving attackers insights into the system's behavior.

2. Briefly explain how a malicious attacker can exploit them.

An attacker can craft a malicious query, such as id[$ne]=, to bypass authentication and gain unauthorized access to data.

Submitting malformed or overly complex queries can cause the database or server to crash, leading to a denial of service.

Sending numerous malicious requests without restriction can overwhelm server resources, rendering the application unresponsive to legitimate users.

3. Briefly explain the defensive techniques used in **secure.ts** to prevent the DoS vulnerability?

The express-rate-limit middleware is applied to the /userinfo endpoint, limiting the number of requests a client can make in a specific time window. This helps mitigate brute force and flooding attacks.

The try-catch block ensures that database errors are gracefully handled without crashing the server or leaking sensitive error messages.

Sanitization of inputs should be implemented to prevent NoSQL injection attacks (e.g., using libraries like mongoose-sanitizer or validating input types).

The query parameter id is validated to ensure it is a valid ObjectId, reducing the risk of injection attacks.
Any database errors are logged internally and a generic error message is returned to the client, avoiding information leakage.