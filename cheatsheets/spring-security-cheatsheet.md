# Spring Security Cheatsheet: 20 Most Common Configurations

While learning Spring Security, I often found myself searching for the same configurations repeatedly.

To make revision easier, I created this cheatsheet containing some of the most commonly used Spring Security configurations that I frequently encounter while building projects.

This document is intended as a quick reference guide rather than a complete tutorial.

---

## 1. Define a SecurityFilterChain Bean

```java
@Bean
public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
    return http.build();
}
```

---

## 2. Allow Public Endpoints

Use `permitAll()` when an endpoint should be accessible without authentication.

```java
.requestMatchers("/login", "/signup").permitAll()
```

---

## 3. Require Authentication for All Requests

Use `authenticated()` when users must log in before accessing an endpoint.

```java
.anyRequest().authenticated()
```

---

## 4. Restrict Access to ADMIN Users

```java
.requestMatchers("/admin/**").hasRole("ADMIN")
```

---

## 5. Allow Multiple Roles

```java
.requestMatchers("/dashboard/**")
.hasAnyRole("USER", "ADMIN")
```

---

## 6. Disable CSRF (Common for REST APIs)

```java
http.csrf(csrf -> csrf.disable());
```

---

## 7. Enable Form Login

```java
http.formLogin(Customizer.withDefaults());
```

---

## 8. Custom Login Page

```java
http.formLogin(form ->
    form.loginPage("/login")
);
```

---

## 9. Enable HTTP Basic Authentication

```java
http.httpBasic(Customizer.withDefaults());
```

---

## 10. Stateless Session (JWT)

Used in JWT-based applications where the server does not maintain user sessions.

```java
http.sessionManagement(session ->
    session.sessionCreationPolicy(SessionCreationPolicy.STATELESS)
);
```

---

## 11. Add a Custom JWT Filter

```java
http.addFilterBefore(
    jwtAuthFilter,
    UsernamePasswordAuthenticationFilter.class
);
```

---

## 12. Define a Password Encoder

Never store passwords in plain text.

```java
@Bean
public PasswordEncoder passwordEncoder() {
    return new BCryptPasswordEncoder();
}
```

---

## 13. Create a UserDetailsService Bean

Used when loading user details from a database during authentication.

```java
@Bean
public UserDetailsService userDetailsService() {
    return username -> userRepository
            .findByUsername(username)
            .orElseThrow(() ->
                new UsernameNotFoundException("User not found"));
}
```

---

## 14. Expose AuthenticationManager

```java
@Bean
public AuthenticationManager authenticationManager(
        AuthenticationConfiguration config)
        throws Exception {

    return config.getAuthenticationManager();
}
```

---

## 15. Authenticate User Programmatically

```java
authenticationManager.authenticate(
    new UsernamePasswordAuthenticationToken(
        username,
        password
    )
);
```

---

## 16. Get the Currently Logged-in User

```java
Authentication auth =
    SecurityContextHolder.getContext().getAuthentication();

String username = auth.getName();
```

---

## 17. Method-Level Security

Secure specific methods using annotations.

```java
@PreAuthorize("hasRole('ADMIN')")
public void deleteUser() {
}
```

Enable it using:

```java
@EnableMethodSecurity
```

---

## 18. Custom Access Denied Page

```java
http.exceptionHandling(ex ->
    ex.accessDeniedPage("/access-denied")
);
```

---

## 19. Logout Configuration

```java
http.logout(logout ->
    logout.logoutUrl("/logout")
);
```

---

## 20. Example Security Configuration for Beginners

```java
@Bean
public SecurityFilterChain securityFilterChain(HttpSecurity http)
        throws Exception {

    http
        .csrf(csrf -> csrf.disable())

        .authorizeHttpRequests(auth -> auth
            .requestMatchers("/login", "/signup")
            .permitAll()

            .requestMatchers("/admin/**")
            .hasRole("ADMIN")

            .anyRequest()
            .authenticated()
        )

        .httpBasic(Customizer.withDefaults());

    return http.build();
}
```

---

# How to Think About Spring Security

| Requirement      | Configuration           |
| ---------------- | ----------------------- |
| Public API       | `permitAll()`           |
| Login required   | `authenticated()`       |
| ADMIN only       | `hasRole("ADMIN")`      |
| Multiple roles   | `hasAnyRole(...)`       |
| REST API         | Disable CSRF            |
| JWT project      | Stateless Session       |
| Hash passwords   | `BCryptPasswordEncoder` |
| Add JWT filter   | `addFilterBefore()`     |
| Get current user | `SecurityContextHolder` |
| Secure methods   | `@PreAuthorize`         |

---

## Rule to Remember

> First think:
>
> **"What do I want?"**
>
> Then ask:
>
> **"Which Spring Security configuration provides that behavior?"**

Nobody memorizes Spring Security in one day. Most developers learn these configurations gradually while building projects.

---

## Next Steps

Once you're comfortable with these configurations, explore the following topics:

* UserDetailsService
* AuthenticationManager
* Password Encoding with BCrypt
* JWT Authentication
* Custom Security Filters
* Method-Level Security

Spring Security can feel overwhelming at first, but understanding these common configurations will give you a strong foundation.

