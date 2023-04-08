This is the reflection for the node.js course from ZTM. I ccame into this course with pretty broad knowledge of React, some node, Terraform, linux commands/ file system, etc.

This course was very helpful because it made me understand what node.js is comprised of, what it's strengths/weaknesses are, and building a production level api. The course starts off with node basicsand even has us build a server using the built in http module. Eventually we learn the benefits of using a framework like express to abstract some complexities of using node by itself. 

We learned the model, view, controller design for node servers. This is a structuring of the modules in our code base, controllers are functions that are responding to the incoming requests (typically they are calling utility functions to perform the action the request is trying to do), model is the module that is in charge of connecting to the database and creating a schema if applicable, view is the area that the data from the database is showing to the user (typically this is a front end react app).

Express works very well with middlewares, it uses middlewares to do many things in between a request coming in and a response being send back. We can cretae router's for our API, this allows us to join together in the code base code that is related to the same thing. For example we can crate a router that is mounted onto the /products route, within the router we can define sub-routes based on the route defined when mounted as the middleware. So now inside of the router area, we define the endpoint and the controller that will be invoked responding to the request.

Our API can be tested using the Jest package, all we need to do is create a file with the .test.js at the end and inside we can write whatever tests we need. It's recommended to create a testing database so that we can include a test that makes changes to a database. This ensures that our code will work in production because instead of mocking the database where we don't actually know if it is going to work the way we need it to in production. Tests are done using describe blocks (describes the nature of tests inside of this block, as well as having functions like beforeAll and afterAll which allow us to create a db connection before any tests are fired), inside of the describe blocks we include it blocks where we name what the test should do and inside of a callback function define what actions will take place for the test. Supertest is another package we can use to actually make requests to our api for our tests, this is very useful! Note we can use 2 verisons of expect, one from supertest package and one from jest itself with some little differences.

Some performance upgrades that can be done with node is to avoid code that blocks the event loop. Remember node is best used for performing asynchronous I/O requests. Node can be run in clusters along with load balancers to direct incoming requests to the servers with the least amount of load or to a server based on round robin. Clusters can be made with a couple of packages and the important thing to know is leveraging node to it's highest potential by using its worker threads.

Next we learned about databases, the difference between sql and nosql, benefits and tradeoffs of each. For the course nasa project we used mongo db because of the data that was being stored for the application. It did not need the sql approach because there were not many connections between sets of data, plus it added some complexity that may not be needed for the project. This worked well for me personally because I have experience with postgres from another project so I already know how to create a container for it, create a table and then query that data using an ORM like knex. The ORM that's widely used for mongoDB is mongoose. Mongoose allows us to create a model which is the object representation of the document data inside of mongodb, mongodb uses bson which is binary json, its used as a performance improvement and probably to save memory - anyway we use mongoose to abstract querying and converting the data into the form we need for our app to use it. Schemas can be created for our data to have some rigitity like a sql db, then a model is created in which we can insert data into the model. A common pattern that arises from using mongodb is the upsert operation, it is a combination of insert and update and is used because its common to want to look for some document and either update it with new data or if the document does not exist, create it. This is accomplished by using the findOneAndUpdate function and passing in 3 objects - the first is describing what document we are looking for (the filer), second is an object describing what we want to update inside of the document or what we want to populate the document we are creating, the third object describes the options for this query in which we include upsert:true and everything works as needed. 

We worked with real time data using the spacex api from github - mapping its data to our applications needs to store into our database and learned about using a post request for querying data (similar to graphql), and paginating our api. Pagination is the practice of splitting the data returned form a request into pages containing x amount of data per page, this is done as a performance upgrade because as more data is returned there is more latency for the user. 

Authentication and authorization is a common task handled by the backend of an application. SSL and TLS are certificates needed to be generated by a certificate authority in order to be able to handle https traffic of the website, we can sign our own certificate but there will be blocks/popups from chrome. The digital certificates can be purchased online and usually are tired to a domain name for the website. Helmet.js is a package commonly used to hide potentially dangerous data from getting sent from our express server back to the client. As programmers a good principle to have is to provider the least amount of privilege necessary to complete a request/task. Helmet.js is a middleware that understands the potential vulnerabilities that are produced from the defualt express package and protects the server by not providing too much information back to the client. Authentication is the act of verifying a user is who they say they are (usually by prompting them to insert username and password and verifying the information provided). Authorization is the act of allowing or dissalowing an authenticated user from having access to a resource. Social sign in is a modern feature of the web that allows users to use another company's authentication information to create an account / sign in to our application. This is called the OAuth standard and is a workflow for authentication thats becoming more and more popular. In a nutshell it works like this: instead of creating an authentication flow inside of our application from scratch (prone to human errors/vulnurabilities with sensitive information from the client), we hand off that flow to a 3rd party like Google for example, they handle the authentication and after user if auth, they get redirected to our application and receieve a cookie or a token authenticating them. Our server now just needs to ensure the token exists for a user trying to access a resource and thats it. Passport.js is a package commonly used to help create this OAuth flow, it is comprised of Strategies for different auth workflows we are trying to acheive (like local, OAuth2.0 for google, github, facebook etc), it becomes a middleware that does the auth workflow behind the scenes. Lastly cookies or JWT tokens can be used to implementing sessions for our web app.

CI/CD is a system created to efficiently go from code being written on a local computer, to transfered to a central repo that can be shared to others and run through tests to ensure the code is ready for production. CD is the act of having the code ready to be delivered to final authorization or optionally automatically deployed into production after all of the tests pass. This improves the software development lifecycle and is crucial when collaborating on code with a team. Some common tools for CI/CD include, github actions, circleci, jenkins.

Docker and AWS can be combined to deploy our application easily and quickly. Docker is a tool used to create contianers (complete environments for our application to run) which are lightweight and quick to spin up or down on a computer. EC2 is a rentable VM from amazon. The 2 combined allow us to have a server, with an ip address and have our final source code running on the contianer which is serving our application's needs. Diving a little deeper into Docker, a Dockerfile is a file recognized by docker that creates an image from a base image, this is commonly used to customize our application and crate an image for it that can be uploaded to docker hub (image cloud hub) and installed to any computer with an internet connection. The base image is typically the runtime our app needs along with a lightweight OS like alpine. Once the ec2 instance is created, and the image is downloaded and run as a container our app is deployed quick and easy with relatively low cost.

Graphql and web sockets were the last 2 sections of the course and because of slightly outdated information I was not able to follow along and learn them in depth. But other than that the course was great and because of this course I think I have more interest in backend engineering over the front end. This is because the backend is the system underlying the entire application, its what the front end relies on in order to make the website feel the way it does. It provides the application with a system for databases, authentication, and other 3rd party communications. Glad I took the course and I am excited at what I will be able to build using the knowledge from all of the courses I have taken.
