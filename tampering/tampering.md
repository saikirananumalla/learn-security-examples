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
   The server is vulnerable to XSS because it doesn’t sanitize or validate user input and directly injects it into HTML with res.send, allowing malicious scripts to be executed.
2. Briefly explain how a malicious attacker can exploit them.
   An attacker can inject scripts via the name field to run malicious JavaScript, redirect users to harmful sites,
   or display fake forms for phishing—all triggered when someone views the page.
3. Briefly explain why **secure.ts** does not have the same vulnerabilties?
   secure.ts sanitizes input using escapeHTML(), which converts unsafe characters to their HTML-safe versions.
   This ensures injected scripts are treated as plain text, preventing XSS attacks.