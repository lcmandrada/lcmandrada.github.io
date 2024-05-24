# Web Development

## REST
Representational State Transfer  
Set of guides and standards on how client and server communicate with each other, and perform CRUD operations on a resource  
Main idea is to treat server data as resources that can be CRUDed  
Uses HTTP methods

**CRUD** - Create, Read, Update and Delete  
**RESTful** - term to describe systems that follow REST

```
index       GET /comments               list all comments
new         GET /comments/new           create a new comment via form
create      POST /comments              create a new comment
show        GET /comments/:id           list more info about a comment
edit        GET /comments/:id/edit      update a comment via form
update      PATCH /comments/:id         update a comment
destroy     DELETE /comments/:id        delete a comment
```

**HTTP Methods**

1. `GET`
    - retrieve information
    - uses query string
2. `POST`
    - submit data to server
    - write, create, update data
    - uses request body (e.g. json)
3. `PUT` - update the entire resource
4. `PATCH` - partially update a resource
5. `DELETE` - destroy a resource

HTML can only send `GET` and `POST` requests  
Method override may be necessary for other requests (e.g. `PATCH`, `DELETE`)



## Process

1. **Planning**  
    - Requirement analysis

1. **Defining**  
    - Sofware Requirement Specification (SRS)

1. **Designing**  
    - System architecture  
    - Design Document Specification (DDS)  

    - Define RESTful routes  
    - Define data models and relationships

1. **Development**  
    - Server and database setup  
    - Resource CRUD operations

    - Front-end webpages

    - Client-side validation
        - Forms

    - Server-side validation
        - Request body
        - Query string
        - Database validation

    - Error handling  
    - Error alerts

    - Authentication  
    - Authorization

    - Security issues

1. **Testing**  
    - Format  
    - Lint  
    - Tests

1. **Deployment**  
    - Deployment  
    - Automation

1. **Maintenance**  
    - Metrics and Analytics



## Tech Stack

### Microservice
Front-end Services  
Back-end Services

### Server
Amazon Elastic Compute Cloud (EC2)  
Amazon Elastic Kubernetes Service (EKS)  
Google Cloud Platform (GCP)  
Heroku

### Database
SQL

- MySQL
- PostgreSQL

NoSQL

- MongoDB

Platform

- MongoDB Atlas
- Amazon Relational Database Service (RDS)
    - PostgreSQL

### Storage
Minio  
NextCloud

### Session Data
redis  
MongoDB

### Broker
redis  
Kafka

### Container
Docker  
Kubernetes

Platform

- Amazon Elastic Container Registry (ECR)

### Monitor
Amazon CloudWatch

- Metrics
- Alarms

Sentry

- Alerts

### CICD
GitHub Actions  
Jenkins

### Development
Skaffold

### Repository
Nexus

### Third Party
Customer.io - automated messaging platform  
Segment - Customer Data Platform (CDP)



## Database

### Why use a database?
- handle huge data efficiently and compactly
- easier CRUD operations
- offer security and access features
- scale well

### SQL vs. NoSQL
**SQL** is the programming language used to interface with relational databases.  
**NoSQL** is a class of DBMs that are non-relational and generally do not use SQL.

| SQL | NoSQL |
| -- | -- |
| relational | non-relational |
| table-based | can be document based, key-value pairs, graph databases |
| vertically scalable by increasing the processing power of existing hardware | master-slave architecture which scales better horizontally with additional servers or nodes |
| predefined schema | dynamic schema for unstructured data |
| ACID | BASE |

**When SQL**

- Analyzing behavioral related and customized sessions
- It allows you to store and gets data from the database quickly
- Preferred when you want to use joins and execute complex queries
- Conceptually modeled as tabular
- In systems where consistency is critical

Small business’ accounting systems, sales databases, or transactional systems like payment processing in e-commerce.  

**When NoSQL**

- When ACID support is not needed
- When traditional RDBMS model is not enough
- Data which need a flexible schema
- Constraints and validations logic not required to be implemented in database
- Logging data from distributed sources
- It should be used to store temporary data like shopping carts, wish list and session data
- Data sets which are both large and mutate significantly
- Businesses growing extremely fast but lacking data schema

In terms of use cases, this might translate to social networks, online content management, streaming analytics, or mobile applications.

**ACID**

**Atomicity** - An "all or nothing" approach. If any statement in the transaction fails, the entire transaction is rolled back.

**Consistency** - The transaction must meet all protocols defined by the system. No half completed transactions.

**Isolation** - No transaction has access to any other transaction that is in an intermediate or unfinished state. Each transaction is independent.
**Durability** - Ensures that once a transaction commits to the database, it is preserved through the use of backups and transaction logs.


**BASE**

**Basically Available** - Guarantees the availability of the data. There will be a response to any request (can be failure too).

**Soft state** - The state of the system could change over time.

**Eventual consistency** - The system will eventually become consistent once it stops receiving input.

NoSQL databases give up the A, C and/or D requirements, and in return they improve scalability.

### MongoDB
- commonly used with Node.js and Express (MEAN and MERN stack)
- easy to get started with
- plays well with JS
- strong community

#### BSON vs. JSON
BSON

- binary JSON
- data used by MongoDB
- supports more data types

JSON

- slow
- not compact
- supports limited data types

#### Relationships
1. **One to Few** - embed the data directly in the document
2. **One to Many** - store data separately and reference the ID
3. **One to Bajillions** - embed parent reference inside the child document

#### Schema Design
1. Favor embedding unless there is a compelling reason not to.
2. Needing to access an object on its own is a compelling reason not to embed it.
3. Arrays should not grow without bound.
    - if more than a couple hundred, don't embed
    - if more than a few thousand, don't embed object ID references, use one-to-bajillions relationship
4. Don't be afraid of application-level joins.
5. Consider write/read ratio when denormalizing.
6. Data model should depend on application's data access pattern.

**Denormalization** - making fields available on multiple locations for faster read

- base on data access flow
    - consider how often data are accessed
    - consider whether mongo will join/populate
- two way references - both parent and child have references to each other
- reference + important fields
    ```js
    [
        {
            _id- id,
            field- "value",
            ...
        },
        ...
    ]
    ```



## Architecture

### Serverless
Serverless computing is a method of providing backend services on an as-used basis.  
A serverless provider allows users to write and deploy code without the hassle of worrying about the underlying infrastructure.

**Traditional Servers**

- We are charged for keeping the server up even when we are not serving out any requests.
- We are responsible for uptime and maintenance of the server and all its resources.
- We are responsible for applying the appropriate security updates to the server.
- As our usage scales we need to manage scaling up our server as well.
- And as a result manage scaling it down when we don’t have as much usage.

**Advantages**

- No server management is necessary
- Developers are only charged for the server space they use, reducing cost
- Serverless architectures are inherently scalable
- Quick deployments and updates are possible
- Code can run closer to the end user, decreasing latency

**Disadvantages**

- Testing and debugging become more challenging
- Serverless computing introduces new security concerns (e.g. multitenancy)
- Serverless architectures are not built for long-running processes
- Performance may be affected (e.g. cold start)
- Vendor lock-in is a risk

**When to use**

- decrease go-to-market time
- build lightweight, flexible applications that can be expanded or updated quickly
- inconsistent usage, with peak periods alternating with times of little to no traffic
- push some or all of application functions close to end users for reduced latency

**When to avoid**

- large applications with a fairly constant, predictable workload
- difficult to migrate legacy applications to a new infrastructure with an entirely different architecture



## Concepts

### Cookies
Bits of information stored in user's browser  
Sent on every subsequent requests  
Allow stateful HTTP

Cons

- has a max size
- not as secure as sessions

Uses

- session management
- personalization
- tracking

**Cookie Signin**g - to verify that data has not been tampered  
**HMAC** - hash-based authentication code

### Session
Server side data store to allow stateful HTTP  
Why? Not practical and secure to store a lot of data in client-side cookies

Session and Cookie

1. session stores user data on a data store (e.g. MongoDB, redis)
2. cookie unlocks said session data

#### Flash
Special area in the session  
Messages are written to flash and is cleared after being displayed



### Auth
**Authentication** - who are you  
**Authorization** - what can you access, after authentication

Never store passwords in plain text

**Hash Passwords** - run passwords through a hashing function and store it in the database  
**Hashing Functions** - map input data of arbitrary size to fixed-size output values

Cryptographic Hash Functions (Password Safe)

1. One-way function which is infeasible to invert.
2. Small change in input yields large change in the output.
3. Deterministic - same input yields same output.
4. Unlikely to find two outputs with same value.
5. Password hash functions are deliberately slow.

**Salt** - extra safeguard from reverse lookup (e.g. map of hashed passwords with common passwords)  
**Password Salt** - random value added to the password before hashings

#### BCrypt
Password hashing function

1. generate salt
2. hash password with salt

#### OAuth
OAuth 2.0 is the industry-standard protocol for authorization (not authentication)

- authorization between services
- access delegation
- allow services limited access to your resources from another service on your behalf
- e.g. allow a service to upload photos from your google drive

**Authorization Token** - key for limited access

- contains user-allowed permissions
- trustable and cannot be tampered
- can be in JWT

**Resource** (R) - protected entity (e.g. photos)  
**Resource Owner** (RO) - entity capable of granting access to a protected resource (e.g. user)  
**Resource Server** (RS) - server hosting the protected resource (e.g. Google server)  
**Client** (C) - application making protected resource requests on behalf of the resource owner (e.g. photo printing app)  
**Authorization Server** (AS) - server issuing access tokens to the client (e.g. Google's authorization server)

Client would need to register with an OAuth 2.0 provider which will supply client credentials (ID and secret) to uniquely identify it.

**Authorization Code Flow**  
Safest  
Web application workflow  
The way the client passes the authorization token to gain the access token can be secured

1. RO requests service from C
2. C requests access from AS
3. AS asks permission from RO
4. RO consents
5. AS sends authorization token to C
6. C requests access token from AS with the authorization token
7. AS sends access token to C
8. C requests access to R from RS with the access token
9. RS provides access to C

**Implicit Flow**  
Simplified  
Not as secure  
Primarily used with short-lived access tokens  
Any service who obtains the access token can get access

1. RO requests service from C
2. C requests access from AS
3. AS asks permission from RO
4. RO consents
5. AS sends access token directly to the C
6. C requests access to R from RS with the access token
7. RS provides access to C

**Client Credentials Flow**  
Used for authorization between microservices  
When the Client is well trusted

1. S1 requests from AS
2. AS sends access token to S1
3. S1 requests from S2 with the access token
4. S2 provides access to S1

[OAuth 2.0 in Python](https://testdriven.io/blog/oauth-python/)

#### JWT
JSON Web Token  
Pronounced as jawt  
Provides secure communication between services  
Used for authorization  
[RFC 7519](https://datatracker.ietf.org/doc/html/rfc7519)

**Authorization Strategies**

1. **Session Token**
    - server keeps session data to keep track of user
    - can be unlocked by session ID (stored in cookies)
    - session ID + cookies is the most popular mechanism for authorization
    - reference token (reference to a state)
2. **JWT**
    - client is given a signed token that is sent for every request
    - value token (contains the values itself)

HTTP is a stateless protocol.  
Information must be sent in every request for dynamic web applications.

**Motivation for JWT**  
With a load balancer, session data may be available on one server instance but not the others.  
This can be solved by a shared cache (redis).  
However, this allows a single point of failure.  
When redis is down then all sessions are down.  
This can be solved by sticky session.  
Or by JWT which attaches necessary information for every request.



### HTTPS
Secure communication over the Internet

**Prerequisites**  
Need to trust that public key cryptography and signature works

1. Any message encrypted with the public key can only be decrypted with the private key.
2. Anyone with access to the public key can verify that a message (signature) could only have been created by someone with access to the private key

**Certificate Authority** (CA) - trusted issuer of digital certificates

**Secure Sockets Layer** (SSL) - public key encryption to secure data  
**SSL Certificate** - used to authenticate the identity of a website  
**Transport Layer Security** (TLS) - successor to SSL  
SSL and TLS can be used interchangeably

**SSL/TLS Handshake**

1. Browser requests website
2. Website sends a certificate signed by CA containing its public key
3. Browser knows and trusts CA, and can verify the certificate using CA's public key
4. Browser verifies and creates new secret encrypted with website's public key
5. Website decrypts the secret using its private key (only the trusted website can decrypt)
6. Browser and website are the only two machines who know the secret key, both will encrypt all further communications using the key

**Certificate Signing**

1. Web server creates a key pair
2. Web server creates a Certificate Signing Request (CSR) with the key pair and requests it to be signed by CA
3. CA signs the request with its private key. Anyone with CA's public key can verify the certificate

Trusted CA can prevent MITM attack as attacker's certificate is not signed by CA

**Self-signed Certificate**

- sometimes, it's not necessary to have a CA (e.g. staging environment)
- sign CSR with own key pair

Setup depends on the web server platform.



### CDN
Content Delivery Network  
A geographically distributed group of servers which work together to provide fast delivery of content  
Content includes cacheable static data such as HTML pages, javascript files, stylesheets, images, and videos  
Reduces the distance between users and server providing the content

**Benefits**

- Improves website load time
- Reduces bandwidth costs on main server
- Increases content availability and redundancy 
- Improves website security through obcurity

Setup depends on the CDN provider.



### CORS vs. CSP
**Cross-Origin Resource Sharing (CORS)** is a HTTP-header based mechanism that allows a server to indicate any origins other than its own from which a browser should permit loading of resources.

**The goal of Content Security Policy (CSP)** is to protect against Cross-Site Scripting (XSS) attacks by dictating which scripts should be trusted and which shouldn't.

**Rules**

1. You want to request a resource from another site - What CORS policies do they have in place?
2. Another site wants to request a resource from your site - what CORS policies do you have in place?
3. You want to load a resource (script, image, whatever) from another site -  
does your Content Security Policy allow you to load resources from that domain?

[Reference](https://dev.to/sophiekaelin/what-is-the-difference-between-cors-and-csp-i7n)



### File Upload
1. Set `enctype=multipart/form-data` to the form
2. Create `input` of `type="file"` element
3. Add encoder middleware in back-end
4. Consider constraints (e.g. max number of images, image size and resolution)



### Common Security Issues
#### SQL/NoSQL Injection
Injecting database operations via input  
Use a database sanitation library

#### Cross Site Scripting (XSS)
Injecting client side scripts on a web app (e.g. to steal data)

`<script>...</script>` on user input  
Adding a script to url that sends cookies to another server

Use an HTML sanitation library  
Set session cookies as `httpOnly` and secure (HTTPS)  
Set custom session cookie name instead of the default

#### Hide errors and traceback

#### Add security headers
Use a library that adds security headers



## Templates

### Python
[A cookiecutter for Python projects](https://github.com/lcmandrada/python-template)

Also includes the following templates

- `Makefile`
- `.gitignore`
- `.dockerignore`
- `Dockerfile`
- Pull Request
- GitHub Actions CI



## Deployment

### Heroku
Platform as a service  
Has a free plan, no credit card required

1. Install heroku cli
2. `heroku create`
3. Prepare env variables (e.g. database url, secrets)
4. Create start script and configure env port
5. Configure env variables either on Heroku website settings or Heroku cli
6. Whitelist Heroku ip in mongodb atlas
7. Push app code to Heroku

```bash
heroku login
heroku create // make space for the app; do in app's root directory
heroku logs -tail // troubleshoot heroku app

heroku config:set SECRET=VALUE
heroku restart

# push code to heroku
git add .
git commit -m "deploy"
git push heroku master
```



## Roadmap
1. Pick a framework - React
2. Master CSS
3. Explore back-end- Python
4. Learn SQL
5. Build something



## Best Practices

### Self-documenting code
### Create and follow code guidelines



## Notes

### Confidence comes with practice
### Developers are always learning
### Don't memorize
Use google  
Read documentations  
Copy good code

### Library vs. Framework
Library - you control the code  
Framework - you follow its structure

### Resource vs. Model
Resource - what the clients interface with via endpoints and HTTP methods  
Model - internal representation of entities as object (ORM/ODM)
