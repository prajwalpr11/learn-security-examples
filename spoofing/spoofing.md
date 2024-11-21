# Spoofing

This example demonstrates spoofind through two ways -- Stealing cookies programmatically and cross site request forgery (CSRF).

## Steps to reproduce the vulnerability

1. Install dependencies

    `$ npx install`

2. Start the **insecure.ts** server

    `npx ts-node insecure.ts`

3. Start the malicious server **mal.ts**

    `npx ts-node mal.ts`

4. Open __http://localhost:8000__ in a browser, type a name and Submit.

5. Open the __Application__ tab in the Browser's inspect pane. Find the __Cookies__ under __Storage__. You should see a __connect.sid__ cookie being set.

6. Open the HTML file __mal-steal-cookie.html__ file in the same browser (different tab). Open inspect and view the console.

7. Click the link in the HTML file. Do you see the cookie being stolen in the console?

8. Open the HTML file __mal-csrf.html__ file in the same browser (different tab). What do you see if the user has not logged out of **insecure.ts**? What do you see if the user has logged out? 


## For you to answer

1. Briefly explain the spoofing vulnerability in **insecure.ts**.

<ul>
<li>The session cookie (`connect.sid`) is not configured securely, with `httpOnly` and `sameSite` flags missing, making it vulnerable to theft through client-side scripts and cross-site attacks.</li>
<li>The server does not validate the origin or authenticity of requests, making it susceptible to Cross-Site Request Forgery (CSRF) attacks.</li>
<li>There is no mechanism to prevent malicious servers from tricking users into exposing sensitive session data.</li>
</ul>

2. Briefly explain different ways in which vulnerability can be exploited.

<ul>
<li>**Cookie Stealing Programmatically:** A malicious server (e.g., `mal.ts`) can execute JavaScript in the victim's browser to log and steal cookies, exposing session data.</li>
<li>**Cross-Site Request Forgery (CSRF):** A malicious HTML file (e.g., `mal-csrf.html`) can auto-submit requests to sensitive endpoints (e.g., `/sensitive`) using the victim's session, impersonating the user without their knowledge.</li>
<li>**Session Fixation:** If an attacker can predict or steal a session ID, they can use it to gain unauthorized access by sending requests with the stolen cookie.</li>
</ul>

3. Briefly explain why **secure.ts** does not have the spoofing vulnerability in **insecure.ts**.

<ul>
<li>The session cookie is configured securely with the `httpOnly` flag, preventing client-side scripts from accessing it, mitigating cookie theft.</li>
<li>The `sameSite` flag is enabled, which restricts the browser from sending cookies with cross-site requests, mitigating CSRF attacks.</li>
<li>Requests are authenticated based on the user's session, ensuring only legitimate users can perform sensitive operations.</li>
<li>The session secret is securely passed as a runtime argument, preventing exposure of sensitive configuration details.</li>
</ul>
