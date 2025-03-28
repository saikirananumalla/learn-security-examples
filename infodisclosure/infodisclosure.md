# Tampering

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
   The file logs sensitive info, lacks input sanitization, and has no access control on the /userinfo route. This exposes the app to information leaks, NoSQL injection, and unauthorized access to user data.
2. Briefly explain how a malicious attacker can exploit them.
   An attacker can send crafted inputs like ?username[$ne]= to bypass query logic and retrieve all users, or simply access /userinfo with any username to fetch private data due to the missing authentication checks.
3. Briefly explain the defensive techniques used in **secure.ts** to prevent the information disclosure vulnerability?
   It validates that username is a string and sanitizes it by removing non-alphanumeric characters, preventing injection attacks and ensuring only clean, expected input is processed.