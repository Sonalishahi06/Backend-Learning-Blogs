# What Happens Before Your API Receives a Request in Spring Security?

## Introduction

When I first added Spring Security to my Spring Boot project, something surprising happened.

All my APIs were suddenly protected, and I could no longer access them without authentication.

At first, it felt like magic. 😄

But then I wondered:

**What actually happens when a request enters a Spring Security application?**

In this blog, we'll follow the complete journey of a request and understand how Spring Security protects our application behind the scenes.

---

## A Request's Journey Through Spring Security

Let's say a user sends a request to access:

```http
GET /api/customers
```

You might think the request goes directly to the controller.

But that's not true.

Before your controller sees the request, Spring Security steps in and starts asking a few important questions.

---

## First Question: "Who Are You?"

The request arrives at Spring Security's filter chain.

At this point, Spring Security tries to identify the user.

It looks for credentials such as:

* Username and Password
* JWT Token
* OAuth Credentials

If no credentials are found, Spring Security stops the request right there.

The controller never gets a chance to handle it.

---

## Second Question: "Are These Credentials Valid?"

If credentials are present, Spring Security verifies them.

For example:

* Is the username correct?
* Does the password match?
* Is the JWT token valid and not expired?

If the verification fails, the request is rejected.

Only valid users can move to the next stage.

---

## Third Question: "What Are You Allowed To Access?"

Now Spring Security knows who the user is.

But being authenticated doesn't automatically mean you can access everything.

Suppose a user tries to access an admin endpoint:

```text
/admin/users
```

Spring Security checks the user's roles and permissions.

If the user has the required authority, the request moves forward.

Otherwise, access is denied.

---

## Finally, The Request Reaches The Controller

Only after passing all security checks does the request reach your controller.

```text
Request
   ↓
Spring Security
   ↓
Authentication
   ↓
Authorization
   ↓
Controller
```

At this point, your business logic starts executing and a response is generated.

---

## The Big Takeaway

One thing that surprised me while learning Spring Security was that the controller is actually the last stop in the journey.

Before a request reaches your API, Spring Security has already:

✅ Identified the user

✅ Verified the credentials

✅ Checked permissions

✅ Decided whether the request should be allowed

That's why Spring Security is such an important part of modern backend applications.

It acts as a protective layer between users and your business logic, making sure only the right people can access the right resources.

---

### Thanks for Reading! 🚀

If you're learning Spring Boot and Spring Security, I hope this article helped you understand what's happening behind the scenes.

Feel free to connect with me and share your thoughts or suggestions for future Spring Security topics.
