# Privilege Escalation

The example demonstrates a privilege escalation vulnerability and how to exploit it.

## Steps to reproduce

1. Install all dependencies

    `$ npm install`

2. Start the **insecure.ts** server

    `$ npx ts-node insecure.ts`

3. In the browser, send a GET request

    ```
        http://localhost:3000/send-form
    ```

4. Try different UserIds and see which one gives you authorized access to change the role of that user.

## For you to do

Answer the following:

1. Briefly explain the potential vulnerabilities in **insecure.ts**

<ul>
<li>The application uses a simulated authentication mechanism that directly checks the user's role based on their input without verifying session or identity.</li>
<li>There is no actual authentication mechanism to ensure the user making the request is logged in or has valid credentials.</li>
<li>Authorization is insecure because it assumes that providing a valid `userId` and `role` in the request is sufficient for access control.</li>
<li>The application does not use any secure session or token-based mechanism to verify the user's identity and role.</li>
</ul>

2. Briefly explain how a malicious attacker can exploit them.

<ul>
<li>An attacker can send a crafted POST request with a valid `userId` (e.g., of an admin) and malicious data for `newRole` to escalate their privileges.</li>
<li>Without authentication, an attacker can impersonate another user simply by knowing or guessing a valid `userId`.</li>
<li>The absence of proper session handling means anyone can modify roles if they know the endpoint and format of the request.</li>
</ul>

3. Briefly explain the defensive techniques used in **secure.ts** to prevent the privilege escalation vulnerability?
<ul>
<li>Session-based authentication is implemented to ensure only logged-in users can make requests. The session data includes the logged-in user's ID.</li>
<li>Authorization checks are improved by ensuring the logged-in user is an admin before allowing role updates.</li>
<li>Cookies are configured with `httpOnly` and `sameSite=strict` to prevent client-side manipulation and CSRF attacks.</li>
<li>User input is validated to ensure `userId` and `newRole` are properly formatted, reducing the risk of injection attacks.</li>
<li>Access control ensures only authorized users with an admin role can modify the roles of other users.</li></ul>