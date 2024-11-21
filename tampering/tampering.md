# Tampering

This example demonstrates tampering through script injection.

## Steps to reproduce

1. Install all dependencies

    `npm install`

2. Start the **insecure.ts** server

    `npx ts-node insecure.ts`

3. In the browser, type a potentially malicious script in the name field of the form

    ```
        <script> document.body.innerHTML = "<a href='https://google.com'> Gotcha </a>"</script>
    ```

4. Do you see the potentially malicious hyperlink being injected into the form?

## For you to do

Answer the following:

1. Briefly explain the potential vulnerabilities in **insecure.ts**

<ul>
<li>User input is not sanitized or validated, allowing attackers to inject malicious scripts into the application.</li>
<li>The server directly assigns user-provided data (e.g., `req.body.name`) to the session without any checks, making it vulnerable to Cross-Site Scripting (XSS) attacks.</li>
<li>The lack of input sanitization allows attackers to inject harmful HTML or JavaScript into the application, potentially compromising user data or application integrity.</li>
</ul>

2. Briefly explain how a malicious attacker can exploit them.

<ul>
<li>An attacker can inject malicious scripts (e.g., `<`script>` tags) into the input fields, which are then stored in the session or displayed back to other users, executing the script in their browsers.</li>
<li>Injected scripts could manipulate the DOM, redirect users to malicious sites, steal sensitive session cookies, or perform unauthorized actions on behalf of the user.</li>
<li>The server’s lack of safeguards could expose all users interacting with the application to potential threats if the malicious input is reused or displayed.</li>
</ul>

3. Briefly explain why **secure.ts** does not have the same vulnerabilties?

<ul>
<li>User inputs are sanitized using the `escapeHTML` function, which replaces potentially harmful characters (e.g., `<`, `>`, `&`, `"`, `'`) with their HTML-safe equivalents, preventing XSS attacks.</li>
<li>The session cookie is configured with `httpOnly` and `sameSite: 'strict'` attributes, reducing the risk of script-based attacks accessing session cookies or performing unauthorized cross-site requests.</li>
<li>Proper input handling ensures that only safe, sanitized data is stored in the session and displayed back to the users, maintaining application integrity and user safety.</li>
</ul>
