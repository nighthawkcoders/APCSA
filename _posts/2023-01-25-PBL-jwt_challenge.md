---
toc: true
comments: false
layout: post
title:  Spring Security using Java Web Tokens
description: Manage access and roles to a backend Java Spring Security Application using Java Web Tokens.
categories: []
type: pbl
week: 20
---

## Spring Security using Java Web Tokens Competition
- [JWT Hello Articles](https://www.javainuse.com/spring/boot-jwt)

### JWT concepts via ChatGPT with added illustrations
JSON Web Token (JWT) is a popular way to authenticate users in a web application. It is a compact, URL-safe means of representing claims to be transferred between two parties. The claims in a JWT are encoded as a JSON object that is digitally signed using JSON Web Signature (JWS).
Here is an example of how you might use JWT for authentication in a JavaScript application:
1. The client sends a login request to the server with the user's credentials (e.g., username and password).
    - Client/Origin: https://nighthawkcoders.github.io
    - Server/Host: spring.nighthawkcodingsociety.com
2. If the credentials are valid, the server generates a JWT and sends it back to the client.  Here ae some sample credentials.
    - Sec-Fetch-Mode: cors
    - Sec-Fetch-Site: cross-site
3. The client stores the JWT in a cookie. Here is cookie in Chrome Inspect properties
    - ![JWT Cookie]({{site.baseurl}}/images/jwt_cookie.png)
4. For subsequent requests, the client sends the JWT in the Authorization header.  Here is a sample JWT.
    - jwt=eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJqbTEwMjFAZ21haWwuY29tIiwiZXhwIjoxNjc1ODA0MTg2LCJpYXQiOjE2NzU3ODYxODZ9.rHoLxTcBJOBv36gH5qNI1VhgGv2Jub1OPtpddf1-fHd84BcL5MeGxiBhi2M0MpEJcuhTjeC2TYWVaOjT7ek0tg; Path=/; Max-Age=3600; Expires=Tue, 07 Feb 2023 17:09:46 GMT; Secure; HttpOnly; SameSite=None; Secure
5. The server verifies the JWT and, if it is valid, allows the request to proceed.  Here is successful response.
    - ![JWT Response]({{site.baseurl}}/images/jwt_response.png)

The JWT consists of three parts, separated by dots (.). The first part is the header, which specifies the algorithm used to sign the JWT (e.g., HS256). The second part is the payload, which contains the claims. The third part is the signature, which is used to verify that the sender of the JWT is who it claims to be and to ensure that the message wasn't changed along the way.

It is important to use HTTPS when transmitting JWTs to ensure that the JWT is not intercepted by an attacker. It is also a good idea to use short-lived JWTs (e.g., with an expiration time of one hour) and to refresh them frequently to reduce the risk of unauthorized access.

#### Storing JWT
There are a few different options for storing a JWT in a JavaScript application:
1. Cookies: You can store the JWT in a cookie and send it back to the server with each request. This is a simple and widely-supported option, but it has some limitations. For example, you can't access cookies from JavaScript on a different domain, and some users may have cookies disabled in their browser settings.
2. Local storage: You can store the JWT in the browser's local storage (localStorage) or session storage (sessionStorage). This option allows you to access the JWT from JavaScript on the same domain, but it is vulnerable to cross-site scripting (XSS) attacks, where an attacker can inject malicious code into your application and steal the JWT from the storage.
3. ***HttpOnly cookie***: You can store the JWT in an HttpOnly cookie, which is a cookie that can only be accessed by the server and not by client-side JavaScript. This option provides some protection against XSS attacks, but it is still vulnerable to other types of attacks, such as cross-site request forgery (CSRF).

ChatGPT says ... It is generally recommended to use a combination of options to provide the best security for your application. For example, you could store the JWT in an HttpOnly cookie and also in local storage, and use JavaScript to send the JWT from local storage to the server with each request. This way, you can still access the JWT from JavaScript on the same domain, while also protecting against XSS attacks.

However, for this implementation we have used *** #3 HttpOnly Cookie ***.


#### Obtain JWT in a JavaScript application:
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

#### Authenticate with JWT in a JavaScript application:
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

## Code from Previous Java Spring project
- Roles, [Person Collection of Roles sample](https://github.com/nighthawkcoders/nighthawk_csa/blob/master/src/main/java/com/nighthawk/csa/mvc/database/person/Person.java)
- Setup Roles, [Default Roles](https://github.com/nighthawkcoders/nighthawk_csa/blob/master/src/main/java/com/nighthawk/csa/mvc/database/ModelInit.java)
- Security, [Endpoint rules](https://github.com/nighthawkcoders/nighthawk_csa/tree/master/src/main/java/com/nighthawk/csa/mvc/security)

## Hacks
> This is first time that a nighthawkcoding society app is going under JWT.  There are some best practices, but here are some of my preliminary thoughts.  These can be done in your project or on mine.
- GitHub Pages Application
    - Make a Login and SignUp option in upper left corner of page.  To handle this well it may require some them adjustment.  Login or Name should alway be displayed in upper right corner, review [csa.nighthawkcodingsociety.com](https://csa.nighthawkcodingsociety.com/) for example. 
    - Only block or present login/signup page when someone fails on a fetch of something that is unauthorized.
- Spring Application
    - Add Roles to authentication
    - Bring JavaScript or Spring/Thymeleaf Admin operation into this page
    - Similarly, only block or present login/signup page when someone fails on a fetch of something that is unauthorized.
- Blog or Video on your successes and how you got there.

## Hack Helpers
> Since 1st introducing this project the Teacher has done several things to make this task easier.  However, IMO until you have looked at and played with the code any additions or organizations could just lead to confusion.  Reading article is a must.

* security/SecurityConfig.java.   
    * This code sets up BCrypt as password encoder, this is wired into Spring Security
    * HTTP security is changed so that you white list things that you want secure.  It is possible to do this either way, white list authenticated `.antMatchers("/api/person/**").authenticated()` or white list permitted `.antMatchers("/", "/frontend/**").permitAll()`.  In either case, it is valuable to have a convention on naming endpoints to simplify rules.

```java
/*
* To enable HTTP Security in Spring, extend the WebSecurityConfigurerAdapter. 
*/
@Configuration
@EnableWebSecurity  // Beans to enable basic Web security
@EnableGlobalMethodSecurity(prePostEnabled = true)
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    @Autowired
	private JwtAuthenticationEntryPoint jwtAuthenticationEntryPoint;

	@Autowired
	private JwtRequestFilter jwtRequestFilter;

	@Autowired
	private PersonDetailsService personDetailsService;
	
    @Bean  // Sets up password encoding style
    PasswordEncoder passwordEncoder(){
        return new BCryptPasswordEncoder();
    }

	@Autowired
	public void configureGlobal(AuthenticationManagerBuilder auth) throws Exception {
		// configure AuthenticationManager so that it knows from where to load
		// user for matching credentials
		// Use BCryptPasswordEncoder
		auth.userDetailsService(personDetailsService).passwordEncoder(passwordEncoder());
	}

	@Override
	@Bean
	public AuthenticationManager authenticationManagerBean() throws Exception {
		return super.authenticationManagerBean();
	}

    
    // Provide security configuration
	@Override
	protected void configure(HttpSecurity httpSecurity) throws Exception {
		httpSecurity
			// no CSRF for this example
			.csrf().disable()
			// list the requests/endpoints need to be authenticated
			.authorizeRequests()
			.antMatchers("/api/person/**").authenticated()
			.and().
			// make sure we use stateless session; 
			// session won't be used to store user's state.
			exceptionHandling().authenticationEntryPoint(jwtAuthenticationEntryPoint).and().sessionManagement()
			.sessionCreationPolicy(SessionCreationPolicy.STATELESS);

		// Add a filter to validate the tokens with every request
		httpSecurity.addFilterBefore(jwtRequestFilter, UsernamePasswordAuthenticationFilter.class);
	}
}

```

* mvc\person\PersonDetailsService.java, implements UserDetailsService from JWT.
    * UserDetailsService is how we train Spring Security to use Person and PersonRoles POJOs for security.
    * PersonDetails makes sure each save use password encoding
    * PersonDetails in my implementation is to build abstraction of all the Jpa complexities, allowing  API and MVC classes and methods to be simpler and reusable.

```java
@Service
@Transactional
public class PersonDetailsService implements UserDetailsService {  // "implements" ties ModelRepo to Spring Security
    // Encapsulate many object into a single Bean (Person, Roles, and Scrum)
    @Autowired  // Inject PersonJpaRepository
    private PersonJpaRepository personJpaRepository;
    @Autowired  // Inject RoleJpaRepository
    private PersonRoleJpaRepository personRoleJpaRepository;
    @Autowired  // Inject PasswordEncoder
    private PasswordEncoder passwordEncoder;

    /* UserDetailsService Overrides and maps Person & Roles POJO into Spring Security */
    @Override
    public org.springframework.security.core.userdetails.UserDetails loadUserByUsername(String email) throws UsernameNotFoundException {
        Person person = personJpaRepository.findByEmail(email); // setting variable user equal to the method finding the username in the database
        if(person==null) {
			    throw new UsernameNotFoundException("User not found with username: " + email);
        }
        Collection<SimpleGrantedAuthority> authorities = new ArrayList<>();
        person.getRoles().forEach(role -> { //loop through roles
            authorities.add(new SimpleGrantedAuthority(role.getName())); //create a SimpleGrantedAuthority by passed in role, adding it all to the authorities list, list of roles gets past in for spring security
        });
        // train spring security to User and Authorities
        return new org.springframework.security.core.userdetails.User(person.getEmail(), person.getPassword(), authorities);
    }


    // ....

    // encode password prior to sava
    public void save(Person person) {
        person.setPassword(passwordEncoder.encode(person.getPassword()));
        personJpaRepository.save(person);
    }

    // ....

    // custom JPA query to find anything containing term in name or email ignoring case
    public  List<Person>listLike(String term) {
        return personJpaRepository.findByNameContainingIgnoreCaseOrEmailContainingIgnoreCase(term, term);
    }

    // ....

}
```

* mvc\ModelInit.java, used to initialize database for testing
   * The `CommandLineRunner run()` occurs prior to web site port being available.  Typically, there is only one Bean of type without application.properties override.  Thus, you see jokes and person in same runner.  Splitting this and having Person initialization in person package is desireable. 
   * Class methods for Person (`Person.init`) is used to initialize object.  
   * Each object is checked and saved using `personService` methods.
   * Test Notes are added to ensure functionality.  Intention is to add notes into person package.

```java
@Component // Scans Application for ModelInit Bean, this detects CommandLineRunner
public class ModelInit {  
    @Autowired JokesJpaRepository jokesRepo;
    @Autowired NoteJpaRepository noteRepo;
    @Autowired PersonDetailsService personService;

    @Bean
    CommandLineRunner run() {  // The run() method will be executed after the application starts
        return args -> {

            // Joke database is populated with starting jokes
            String[] jokesArray = Jokes.init();
            for (String joke : jokesArray) {
                List<Jokes> jokeFound = jokesRepo.findByJokeIgnoreCase(joke);  // JPA lookup
                if (jokeFound.size() == 0)
                    jokesRepo.save(new Jokes(null, joke, 0, 0)); //JPA save
            }

            // Person database is populated with test data
            Person[] personArray = Person.init();
            for (Person person : personArray) {
                //findByNameContainingIgnoreCaseOrEmailContainingIgnoreCase
                List<Person> personFound = personService.list(person.getName(), person.getEmail());  // lookup
                if (personFound.size() == 0) {
                    personService.save(person);  // save

                    // Each "test person" starts with a "test note"
                    String text = "Test " + person.getEmail();
                    Note n = new Note(text, person);  // constructor uses new person as Many-to-One association
                    noteRepo.save(n);  // JPA Save                  
                }
            }

        };
    }
}

```

