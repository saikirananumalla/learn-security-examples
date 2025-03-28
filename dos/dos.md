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
   The insecure.ts file has a few serious weak spots that could be exploited to launch a Denial of Service (DoS) attack:

   Unvalidated User Input:
   The id passed in the query isn’t validated. If someone sends an invalid or malformed ID that doesn't follow MongoDB's ObjectId format, it can cause the server to crash when the database tries (and fails) to process it.
   
   No Input Sanitization:
   There's no sanitization of the id, which opens the door to NoSQL injection attacks or unexpected behavior that could degrade performance or destabilize the server.
   
   Missing Error Handling:
   The code doesn't have a try-catch block around the database call. So if something goes wrong—like an invalid query—the error can bubble up and crash the server.
   
   No Rate Limiting:
   There’s nothing in place to stop someone from sending a flood of requests. Without rate limiting, it’s easy for an attacker to overwhelm the server with traffic.


2. Briefly explain how a malicious attacker can exploit them.
   A malicious actor could easily abuse these flaws to take down the server:

   Forcing Server Crashes:
   By sending invalid or malformed id values (like ?id=123), the attacker can cause MongoDB to throw errors. Without proper error handling, these can crash the server—and doing it repeatedly can keep the service down.
   
   Spamming with Bad Requests:
   Since there's no validation or sanitization, the attacker can keep hitting the server with harmful or junk input that the server can’t handle properly.
   
   Overwhelming the Server:
   No rate limiting means an attacker can send tons of requests in a short time, draining resources and making the app unresponsive to real users.

3. Briefly explain the defensive techniques used in **secure.ts** to prevent the DoS vulnerability?

   The secure.ts version fixes these issues with a few important safeguards:

   Sanitizing Input:
   It sanitizes the id before using it in a database query, which helps prevent malformed input and potential injection attacks.
   
   Adding Rate Limiting:
   It uses express-rate-limit middleware to restrict how many requests each IP can make in a short window (like 5 seconds). This helps prevent abuse and keeps the app more resilient to spamming.
   
   Using Try-Catch for Error Handling:
   The database call is wrapped in a try-catch block. If anything goes wrong, the server handles it gracefully instead of crashing, keeping things stable for everyone else.