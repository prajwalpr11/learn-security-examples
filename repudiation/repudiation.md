# Repudiation

The example demonstrates a vulnerability that can lead to repudiation by malicious users attempting to access the services provided by a server.

## Steps to reproduce

1. Install all dependencies

    `$ npm install`

2. Run the server __insecure.ts__.

3. Pretend to be a malicous user and interact with the services by sending requests from the browser.

4. Do you think your actions can be repudiated?

## For you to do

1. Briefly explain the vulnerability.
<ul>
<li>The server does not log or track actions performed by users, making it impossible to trace their activities or hold them accountable.</li>
<li>There is no authentication or authorization mechanism, allowing any user to perform actions without verifying their identity.</li>
<li>Messages are sent and retrieved without recording which user initiated the action, enabling users to deny their actions (repudiation).</li>
</ul>

2. Briefly explain why the vulnerability is addressed in __secure.ts__.

<ul>
<li>Logging middleware records all user actions along with the timestamp, method, and IP address, ensuring traceability of activities.</li>
<li>User authentication is implemented to validate and track the identity of the user before allowing access to resources or actions.</li>
<li>Logs for sending and retrieving messages include the user's identity, providing evidence of their actions.</li>
</ul>

3. Which design pattern is used in the secure version to address the vulnerability? Briefly explain how it works?

<ul>
<li>The **Intercepting Filter** design pattern is used through middleware that intercepts requests and performs logging, authentication, and error handling.</li>
<li>The logging middleware ensures every request is recorded with relevant details before being processed by other handlers.</li>
<li>The authentication middleware (placeholder in the example) validates the user identity before granting access to sensitive routes, ensuring only authorized actions are performed.</li>
</ul>