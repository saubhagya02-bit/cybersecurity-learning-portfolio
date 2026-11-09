# Week 7 — Discussion Prompts

## 1. What is the advantage of replaying a baseline before modifying anything?

A baseline provides a reference for the application's normal response. By comparing a modified request against an unchanged request, I can identify which differences are associated with my controlled change.

It also helps avoid making assumptions about unexpected application behaviour.

## 2. How can stale sessions, cached responses, or anti-CSRF tokens mislead an investigation?

A stale session may cause an authentication failure even when the original request worked. Cached responses may return previously stored content rather than a fresh result. Anti-CSRF tokens may expire or become invalid when a request is replayed.

These conditions can create differences that are unrelated to the parameter being investigated.

## 3. What is the difference between observing a parameter and proving it is security sensitive?

Observing a parameter only shows that the application accepts or processes a value. It does not establish that the parameter controls access, permissions, identity, or protected data.

Additional authorised evidence is needed to establish its security significance.

## 4. How would you reproduce an observation using only DevTools or curl?

Browser Developer Tools can capture requests and inspect their headers, parameters, and responses. An equivalent request can be reproduced in a controlled environment using curl, provided the required headers and request data are included.

