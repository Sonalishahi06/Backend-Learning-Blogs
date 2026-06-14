# Why Do We Use PasswordEncoder in Spring Security?

#java #springboot #springsecurity #backend

When I first started learning Spring Security, I had a simple question:

> "If Spring Security can authenticate users using usernames and passwords, why do we need a PasswordEncoder?"

The answer lies in one important principle:

**Passwords should never be stored in plain text.**

Imagine if passwords were stored like this in the database:

```text
Username: sonali
Password: sonali123
```

If the database gets leaked, attackers can immediately see users' actual passwords.

That's a huge security risk.

## The Better Approach: Hashing Passwords

Instead of storing the original password, Spring Security allows us to store an encoded version of it.

For example:

```text
sonali123
↓
$2a$10$k9Y...
```

Even if someone gains access to the database, they cannot easily determine the original password.

---

## Enter PasswordEncoder

Spring Security provides the `PasswordEncoder` interface to handle password encoding and verification.

A commonly used implementation is:

* `BCryptPasswordEncoder`

To tell Spring Security to use BCrypt, we can define a bean like this:

```java
@Bean
public PasswordEncoder passwordEncoder() {
    return new BCryptPasswordEncoder();
}
```

---

## Encoding a Password

Before saving a user's password to the database, we encode it.

```java
String rawPassword = "sonali123";

String encodedPassword = passwordEncoder.encode(rawPassword);

System.out.println(encodedPassword);
```

The output might look something like this:

```text
$2a$10$X3YwL...
```

Interestingly, BCrypt generates a different encoded value each time, making it even more secure.

---

## But How Does Login Work?

A common question is:

> "If the encoded value changes every time, how does Spring Security verify passwords during login?"

When a user logs in:

1. The user enters a password.
2. Spring Security retrieves the encoded password from the database.
3. `PasswordEncoder.matches()` is used to compare them securely.

```java
boolean isMatch = passwordEncoder.matches(
        "sonali123",
        encodedPassword
);

System.out.println(isMatch);
```

Output:

```text
true
```

---

## What Actually Happens During Authentication?

```text
User enters password
          ↓
Spring fetches encoded password from database
          ↓
PasswordEncoder.matches()
          ↓
Authentication Success / Failure
```

One thing that surprised me while learning Spring Security was that it never compares plain text passwords directly.

Instead, it uses `PasswordEncoder` to securely verify whether the entered password matches the encoded password stored in the database.

---

## The Big Takeaway

Using PasswordEncoder isn't just a Spring Security feature—it's a security best practice.

As developers, we should never store passwords in plain text.

Instead, we should always encode them before saving them to the database.

Because protecting user data starts with protecting their passwords. 🔐
