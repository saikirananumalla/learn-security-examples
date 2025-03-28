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
   The main issue is missing authorization—the server trusts user-supplied data to determine admin access, without verifying session or identity, allowing anyone to perform sensitive actions like role updates.
2. Briefly explain how a malicious attacker can exploit them.
   An attacker can spoof a request with an 'admin' role in the body to escalate privileges or change user roles, as there’s no actual authentication or permission check in place.
3. Briefly explain the defensive techniques used in **secure.ts** to prevent the privilege escalation vulnerability?
   The secure version verifies the user's session and role before allowing role updates, ensuring only real admins can perform such actions.
   It also protects session cookies with httpOnly and sameSite flags to prevent CSRF and hijacking, though it still uses a hardcoded secret—which should be stored securely instead.