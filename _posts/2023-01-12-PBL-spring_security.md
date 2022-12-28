---
toc: true
comments: false
layout: post
title:  Spring Security, Roles and JWT concepts
description: Some tips on building Spring Security into your Web Application(s)
categories: []
type: pbl
week: 18
---

## Security Starters
Last year Teacher spent time in Spring Security System for csa.nighthawkcodingsociety.com.  The implementation allows users (Person POJO) to login with specific roles (Role POJO). Two roles are present currently, ROLE_ADMIN and ROLE_STUDENT. In this implementation the ROLE_STUDENT is NOT able to delete users,  you must be ROLE_ADMIN to perform this task. 

Teacher took inspiration form [YouTube](https://www.youtube.com/watch?v=VVn9OG9nfH0&t=6350s).  This video covers the detail shared below in about 2 hours.

My issue with this starter code is I never had time to get to JWT and have a true web implementation.  Some thoughts I have on that topic are at bottom and courtesy of ChatGPT.

## Main Ideas

Spring Security Code is focused in these areas: [Database Folder](https://github.com/nighthawkcoders/nighthawk_csa/tree/master/src/main/java/com/nighthawk/csa/mvc/database), [Security Folder](https://github.com/nighthawkcoders/nighthawk_csa/tree/master/src/main/java/com/nighthawk/csa/mvc/security), [Login Page](https://github.com/nighthawkcoders/nighthawk_csa/blob/master/src/main/resources/templates/login.html) and [Navbar fragment](https://github.com/nighthawkcoders/nighthawk_csa/blob/master/src/main/resources/templates/fragments/nav.html#L116-L126).

- Backend - Using Spring Security to Protect 
    - Defining POJOs
    - Back End Spring Security Overrides to use Person and Role POJOs
    - Authentication (LOGIN), requiring login to access certain parts of site
    - Authorization (ROLE), differentiation of permission of ROLE_ADMIN and ROLE_ADMIN on
    - Training Spring Security to work with POJOs and Security Config 
- Frontend - Building Configuration and Overrides to support Spring Security
    - Login Page (customizing your own)
    - Navbar with Control Login/Logout buttons
    - Error Page for 403 authorization failure


## Backend - Using Spring Security to Protect
The key work of the backend and Spring Security is to associate user (Person) and their (Roles) with access to Get and Post Mappings.  Planning needs to go into your POJO to have role(s) associated with each person.  

### Person POJO security relevant fragments.  Observe email, password, and role definitions.
```java
@Entity
public class Person {
    // automatic unique identifier for Person record
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    private Long id;

    // email, password, roles are key to login and authentication
    @NotEmpty
    @Size(min=5)
    @Column(unique=true)
    @Email
    private String email;

    @NotEmpty
    private String password;

    // roles are stored in a "related" table
    @ManyToMany(fetch = EAGER)
    private Collection<Role> roles = new ArrayList<>();

    ... 
}
```
### Role POJO contains String to store ROLE_ADMIN, ROLE_STUDENT as distinct rows.  All users share same roles
```java
@Entity
public class Role {
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    private Long id;

    @Column(unique=true)
    private String name;
}
```

## Back End Code - Authentication and Authorization
### [SecurityConfig](https://github.com/nighthawkcoders/nighthawk_csa/blob/master/src/main/java/com/nighthawk/csa/mvc/security/SecurityConfig.java) provided through Spring Security enables developer to Authorize and Authentication different portions of the site based on Login and Roles.  Additionally, you are able to direct the next page on operations like login and logout.  Review .antmatchers statements.  This file is being established to support http for both Web viewing and API access.

``` java
protected void configure(HttpSecurity http) throws Exception { //THis is going to be altered to use the JWT
        // security rules
        http
            .authorizeRequests()
                .antMatchers(POST, "/api/person/post/**").hasAnyAuthority("ROLE_STUDENT")
                .antMatchers(DELETE, "/api/person/delete/**").hasAnyAuthority("ROLE_ADMIN")
                .antMatchers("/database/personupdate/**").hasAnyAuthority("ROLE_STUDENT")
                .antMatchers("/database/persondelete/**").hasAnyAuthority("ROLE_ADMIN")
                .antMatchers( "/api/person/**").permitAll()
                .antMatchers( "/api/refresh/token/**").permitAll()
                .antMatchers("/", "/starters/**", "/frontend/**", "/mvc/**", "/database/person/**", "/database/personcreate", "/database/scrum/**", "/course/**").permitAll()
                .anyRequest().authenticated()
                .and()
            .formLogin()
                .loginPage("/login")
                .permitAll()
                .and()
            .logout()
                .logoutRequestMatcher(new AntPathRequestMatcher("/logout"))
                .logoutSuccessUrl("/database/person")
                .permitAll()
        ;
        // Cross-Site Request Forgery needs to be disabled to allow activation of JS Fetch URIs
        http.csrf().disable();
    }
```

## Backend Code - Training Spring Security to work with POJO and Security Config 
### [ModelRepo](https://github.com/nighthawkcoders/nighthawk_csa/blob/master/src/main/java/com/nighthawk/csa/mvc/database/ModelRepository.java) implements UserDetailsService.  This associates user (Person) and roles (Role) with Spring Security as you can see with return values from method.

```java
public class ModelRepository implements UserDetailsService
```

```java
/* UserDetailsService Overrides and maps Person & Roles POJO into Spring Security */
    @Override
    public org.springframework.security.core.userdetails.UserDetails loadUserByUsername(String email) throws UsernameNotFoundException {
        Person person = personJpaRepository.findByEmail(email); // setting variable user equal to the method finding the username in the database
        if(person==null){
            throw new UsernameNotFoundException("User not found in database");
        }
        Collection<SimpleGrantedAuthority> authorities = new ArrayList<>();
        person.getRoles().forEach(role -> { //loop through roles
            authorities.add(new SimpleGrantedAuthority(role.getName())); //create a SimpleGrantedAuthority by passed in role, adding it all to the authorities list, list of roles gets past in for spring security
        });
        return new org.springframework.security.core.userdetails.User(person.getEmail(), person.getPassword(), authorities);
    }
```

### Password Encryption style (BCrypt) is also managed with this file
```java
    // Setup Password style for Database storing and lookup
    @Autowired  // Inject PasswordEncoder
    private PasswordEncoder passwordEncoder;
    @Bean  // Sets up password encoding style
    PasswordEncoder passwordEncoder(){
        return new BCryptPasswordEncoder();
    }
```

### Password is encoded before JPA save.  Spring Security does decoding within its system, later we will build login to send user and password to Spring Security.
```java
public void save(Person person) {
        person.setPassword(passwordEncoder.encode(person.getPassword()));
        personJpaRepository.save(person);
    }
```

### Frontend - Building Configuration and Overrides to support Spring Security

#### Login Page (customizing your own)
If you override the form, it needs to be built returning a username and password.  The override was setup in [MvcConfig](https://github.com/nighthawkcoders/nighthawk_csa/blob/master/src/main/java/com/nighthawk/csa/mvc/security/MvcConfig.java).  The purpose of override is so it follows Style guideline from the site.

```html
<form name="f" th:action="@{/login}" method="POST">
    <table>
        <tr th:if="${param.error}" class="alert alert-error">
            Invalid username and password.
        </tr>
        <tr th:if="${param.logout}" class="alert alert-success">
            You have been logged out.
        </tr>
        <!-- 'email' is mapped to 'username' for Spring Security -->
        <tr><th><label for="username">Email</label></th></tr>
        <tr><td><input type="email" id='username' name="username" size="20" required></td></tr>
        <tr><th><label for="password">Password</label></th></tr>
        <tr><td><input type="password" id="password" name="password" size="20" required></td></tr>
        <tr><th><input type="submit" value="Submit"></th></tr>
        <tr><td><a th:href="@{/database/personcreate}">Sign Up</a></td></tr>
</table>
```

#### Navbar Customizations
Using Thymeleaf and Spring Security the Navbar toggles between Login/Logout based off of Spring Security status.  
```html
<!-- Login/Logout -->
    <th:block sec:authorize="isAnonymous()">
    <div class="px-3">
        <a th:href='@{/login}' class="link-dark">Login</a>
    </div>
    </th:block>
    <th:block sec:authorize="isAuthenticated()">
    <div class="px-3">
       <a th:href='@{/logout}' class="link-dark">Logout</a>
    </div>
    </th:block>
```

#### Authorization error - 403
Following standard of 404 errors, a 403 page is inserted to provide better user experience versus Spring Boot error page.  [403.html](https://github.com/nighthawkcoders/nighthawk_csa/blob/master/src/main/resources/templates/error/403.html).  Main purpose of this page is to direct the user back into the website with href.

```html
<div class="jumbotron jumbotron-fluid" style="text-align: center; ">
    <h2>You don't have permission to do that! (Insufficient Role)</h2>
        <a href="/">Go Home</a>
    </div>
</div>
```

## JWT concepts via ChatGPT
JSON Web Token (JWT) is a popular way to authenticate users in a web application. It is a compact, URL-safe means of representing claims to be transferred between two parties. The claims in a JWT are encoded as a JSON object that is digitally signed using JSON Web Signature (JWS).
Here is an example of how you might use JWT for authentication in a JavaScript application:
1. The client sends a login request to the server with the user's credentials (e.g., username and password).
2. If the credentials are valid, the server generates a JWT and sends it back to the client.
3. The client stores the JWT in a cookie or in local storage.
4. For subsequent requests, the client sends the JWT in the Authorization header.
5. The server verifies the JWT and, if it is valid, allows the request to proceed.

### Authorization header example
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c

The JWT consists of three parts, separated by dots (.). The first part is the header, which specifies the algorithm used to sign the JWT (e.g., HS256). The second part is the payload, which contains the claims. The third part is the signature, which is used to verify that the sender of the JWT is who it claims to be and to ensure that the message wasn't changed along the way.

It is important to use HTTPS when transmitting JWTs to ensure that the JWT is not intercepted by an attacker. It is also a good idea to use short-lived JWTs (e.g., with an expiration time of one hour) and to refresh them frequently to reduce the risk of unauthorized access.

### Storing JWT
There are a few different options for storing a JWT in a JavaScript application:
1. Cookies: You can store the JWT in a cookie and send it back to the server with each request. This is a simple and widely-supported option, but it has some limitations. For example, you can't access cookies from JavaScript on a different domain, and some users may have cookies disabled in their browser settings.
2. Local storage: You can store the JWT in the browser's local storage (localStorage) or session storage (sessionStorage). This option allows you to access the JWT from JavaScript on the same domain, but it is vulnerable to cross-site scripting (XSS) attacks, where an attacker can inject malicious code into your application and steal the JWT from the storage.
3. HttpOnly cookie: You can store the JWT in an HttpOnly cookie, which is a cookie that can only be accessed by the server and not by client-side JavaScript. This option provides some protection against XSS attacks, but it is still vulnerable to other types of attacks, such as cross-site request forgery (CSRF).

It is generally recommended to use a combination of options to provide the best security for your application. For example, you could store the JWT in an HttpOnly cookie and also in local storage, and use JavaScript to send the JWT from local storage to the server with each request. This way, you can still access the JWT from JavaScript on the same domain, while also protecting against XSS attacks


### Obtain JWT in a JavaScript application:
// Send a login request to the server with the user's credentials
const loginResponse = await fetch('/login', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({ username: 'user1', password: 'pass123' }),
});

// If the login was successful, the server will return a JWT
if (loginResponse.ok) {
  // Extract the JWT from the response
  const jwt = loginResponse.headers.get('Authorization').split(' ')[1];

  // Store the JWT in a cookie or in local storage
  document.cookie = `jwt=${jwt}`;
  // or
  localStorage.setItem('jwt', jwt);
}

### Authenticate with JWT in a JavaScript application:
This example sends a POST request to the /login endpoint with the user's credentials in the request body. If the login was successful, the server will return a 200 OK response with the JWT in the Authorization header. The JWT is then extracted from the header and stored in a cookie or in local storage.

You can then use the JWT for authentication in subsequent requests by sending it in the Authorization header:

// Send a request to a protected endpoint
const response = await fetch('/protected', {
  headers: {
    // Send the JWT in the Authorization header
    Authorization: `Bearer ${localStorage.getItem('jwt')}`,
  },
});

// If the JWT is valid, the server will allow the request to proceed
if (response.ok) {
  // ...
}

Keep in mind that this is just a simple example, and you should consider using a library like axios or fetch-intercept to handle the details of sending and receiving HTTP requests and responses. You should also consider using HTTPS to encrypt the communication between the client and the server, and use short-lived JWTs with frequent refresh to reduce the risk of unauthorized access
