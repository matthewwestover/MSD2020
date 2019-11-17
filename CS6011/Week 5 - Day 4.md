## Week 5 - Day 4
### Spring
Professional grade tool to help quick start Java projects  
Free opensource java base (java kotlin and ruby)  
Get a production grade application up and running in minutes  
Out of the box you get a tomcat server running on port 8080  
Can use Gradle or Maven  
Annotation-based development  
    Spring will then do automatic under the hood work  
Has auto-configuration

Refers to things as @Bean annotation  
There are many types of Beans or you can define new beans. 
Spring scans through the project and allocates objects into a central container  
Any objected annotated with a bean annotation, it will be created in the spring container  
When you need to use a instance of an object, spring will check the container

Dependency Injection is one of the most important parts of Spring. 
Spring injects objects into other objects that rely on them

Beans are objects that are managed in a Spring Container  
Objects defines its dependencies without creating them  
@Bean is the top level, but there are ones that inherit from @Bean,
@Component, @Server

Endpoints are the /index.html of the url  
@Autowired in java controller to avoid new keyword  
@GetMapping(“/something”) sets the endpoint  

For web sockets include  
```Implementation ‘org.springframework.boot:spring-boot-starter-websocket'```