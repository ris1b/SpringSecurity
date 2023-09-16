# Spring Security and it's brief description

[//]: # (## Package Structure:)

[//]: # (1. Controller Package)

[//]: # (2. Security Package)

[//]: # (   - Has a Configuration class called WebSecurityConfig)

[//]: # (     - WebSecurityConfig has an applicationSecurity Bean of type SecurityFilterChain.)

[//]: # (     - applicationSecurity&#40;&#41; takes in HttpSecurity)

[//]: # (## Agenda:)

[//]: # (1. Why spring security has so many components ?)

[//]: # (2. How do they work ? )

[//]: # (3. Why each one of them is needed ?)


![Spring Boot Security Architecture](./spring.jpg)

- To support various mechanism of authentication, Spring uses AuthenticationProvider concept.

- Each implementation of Provider is responsible for performing its own
type of Authentication.

- Which Provider to choose ? --> AuthenticationManager

    
      Providers <-- AuthenticationManager  <-- Filters
    
- AuthenticationManager's most common implementation is ProviderManager
- UserDetailsService is an interface which has loadUsername() method
  and returns UserDetail object
- Provider uses this UserDetail object to verify the user
- After the Provider is done verifying, it returns the Authentication Object to the AuthenticationManager
  which in turn returns back to the Filter.


--- Successfully Authenticated the User ---

Now SPRING CONTEXT comes into play :
- Once Authentication object was returned back to the filter, the filter would add a currently authenticated user
  to the Security Context.
- Authenticated user == Principle
- To access the Principle, Spring uses a wrapper around SecurityContext called SecurityContextHolder.

```plaintext
                                ---------------------------------
                                |    SecurityContextHolder	|
                                |				|
                                |       ------------------	|
                                |	| SecurityContext |	|
                                |	|                 |	|
                                |	|  -------------  |	|
                                |	|  | Principle |  |	|
                                |	|  -------------  |	|
                                |	|                 |	|
                                |       -------------------	|
                                |				|
                                |				|
                                ---------------------------------
```


| Component | Description                                                                             |
|---|-----------------------------------------------------------------------------------------|
| **SecurityContextHolder** | A global context for security information.                                              |
| **SecurityContext** | Contains the security context for a particular user i.e. Principle.                     |
| **Principal** | Represents a user or other security entity.                                             |
| **Relationship** | SecurityContextHolder contains SecurityContext, and SecurityContext contains Principal. |

SecurityContextHolder have a static getContexxt() and setContext() method, using which we can call to retrieve 
the Principle.


    SecurityContextHolder.getContext()
    SecurityContextHolder.getContext()

While using the session, once the user is authenticated, Spring will associate the User with the session.
So in subsequent requests, a special filter called SecurityContextHolder Filter will try to load our User from
the session.

Also, User need not Re-authenticate here.

