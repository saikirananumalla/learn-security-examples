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
   The insecure version lacks request and response logging, making it difficult to monitor user activity, detect attacks, or debug errors due to poor visibility.
2. Briefly explain why the vulnerability is addressed in __secure.ts__.
   The secure version uses middleware to log each request’s method, URL, timestamp, and IP, storing this info in a log file for better debugging, auditing, and security tracking.
   However, logging should be async to avoid performance issues under load.
3. Which design pattern is used in the secure version to address the vulnerability? Briefly explain how it works?
   It uses the Middleware Pattern, where a logging function intercepts each request before it reaches the route handler.
   This keeps logging modular and separate from core logic, promoting clean and reusable code.