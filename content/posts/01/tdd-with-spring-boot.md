---
weight: 1
title: "Test Driven Development In Spring-Boot"
date: 2020-03-06T21:29:01+08:00
lastmod: 2020-03-06T21:29:01+08:00
draft: false
author: "Yuksel Ozdemir"
authorLink: "https://yukselcodingwithyou.github.io/resume"
description: "Test Driven Development In Spring-Boot"
images: []
resources:
- name: "featured-image"
  src: "featured-image.png"

tags: ["tdd", "development", "spring-boot"]
categories: ["blog-posts"]

lightgallery: true

toc:
  auto: false
---

This weekend, I decided to build a sample Spring-Boot application and learn development concepts, principles, and specifically to see how Test Driven Development can be achieved in Spring-Boot by doing some actual work.

## Why Testing?
Testing in necessary in any field, because human beings are very likely to make mistakes anytime. Therefore, we need to check and verify that our work is completed correctly.
So why testing in software is important explained below.

Point out defects and errors during development phases
1. Reliability of organization increases in customers' perspective
2. Ensuring quality of the product.
3. Provides effective performance of product
4. Foresee any failures that will cost much more in the next phases.

```I think, in this 5 bullet points the most important one is the fifth one. In my opinion, in a software project we must foresee any defect or failure, only then we can create a time and cost effective product.```

## Testing Types
There are many testing types used by people developing software, but I will emphasize **unit testing** and **integration testing** in this context.

### Integration Testing
This type of test is applied to see whether different components of system work well together. For more information you can see this [stackoverflow link](https://stackoverflow.com/questions/5357601/whats-the-difference-between-unit-tests-and-integration-tests).

### Unit Testing
This type of test is applied by programmer to see any unit of code is working properly. For more information you can see this [stackoverflow link](https://stackoverflow.com/questions/5357601/whats-the-difference-between-unit-tests-and-integration-tests).

## Building The Application
While building application, I used [spring-initializr](https://start.spring.io/), added some dependencies, you can also start a maven project and add dependencies to your `pom.xml`

### Added Dependencies
- `spring-boot-starter-web`  : to create web layer of Spring-Boot application.
- `spring-boot-starter-test` : to use features related testing comes in bundle with Spring-Boot.
- `spring-boot-starter-data-jpa`: to create entities and database layer in Spring-Boot application.
- `junit-jupiter-engine`: to use test functionalities of JUnit 5.
- `lombok` : to save some boilerplate code.
- `com.h2database` : in-memory database.


First, I added an integration test to test happy path in the whole system. I will not dive into implementation details, but fundamentally, I used same annotations to make the integration test work. While we write integration tests, we should add these two annotations below to the top of the class, as you know, Spring-Boot uses annotation-driven approach :)
<br/>

```@SpringBootTest(webEnvironment=RANDOM_PORT)```

<br/>
In Spring-Boot, @SpringBootTest annotation searches for the main application context, and creates a similar context to that. This annotation loads all of the application context, therefore it is suitable to use when writing integration tests.

**important**: Since the whole application context is loaded, frequent usage of @SpringBootTest annotation causes long running test suites.

<br/>

```@ExtendWith(SpringExtension.class)```

This @ExtendWith(SpringExtension.class) annotation is used to enable Spring support of JUnit 5, in JUnit 4 @RunWith(SpringRunner.class) annotation is used.

<br/>

```@Autowired private TestRestTemplate testRestTemplate;```

<br/>

As Spring Documentation insists by saying, Convenient alternative of RestTemplate that is suitable for integration tests.
We use TestRestTemplate in integration tests as part of the spring-boot-starter-test. You can inject this to your test class as you see above.
While you are writing tests, you simply should/can follow this convention below.


```
    @Test
    @DisplayName("Tests happy path of hello POST method")
    public void testHello_Returns200() {
        // arrange
         ...
        // act
         ...
        // assert
         ...
    }
```

    
After I wrote integration tests, to test the whole process. Then I added tests for the web layer, so called controller.
While testing the controller, as a difference from integration test, @WebMvcTest annotation is added instead of @SpringBootTest annotation. Since @SpringBootTest loads all of the application context. We do not want the test take too long, since it is a unit test. @WebMvcTest loads only the beans required to test a web controller to the application context.

```
@ExtendWith(SpringExtension.class)
@WebMvcTest(HelloController.class)
```


You can add these two annotations above to your controller test class. @WebMvcTest annotation takes controller as parameter, if no parameter given as controller, includes all of the controllers in the context.

<br/>

```
@Autowired
private MockMvc mockMvc;
```

<br/>

While we write tests for controller layer, as the documentation implies as "Main entry point for server-side Spring MVC test support." It is a good practice to use MockMvc to simulate HTTP requests.

<br/>

```
@MockBean
private HelloService service;
```
<br/>

Any service, component or repository that our controller is dependent on by dependency injection is separately included, and mocked using @MockBean annotation. This annotation adds the mocked object, in spring context it is a bean, to the application context. For a detailed information see this [stackoverflow link](https://stackoverflow.com/questions/44200720/difference-between-mock-mockbean-and-mockito-mock).


After I passed all the test and refactored my code, I started to write unit test for the service layer. In this layer, I did not use any spring related context or annotation, because there was no need to load the application context. I only added the annotation below to my service test class.

<br/>

```
@ExtendWith(MockitoExtension.class)
```
<br/>

Since there is no need to use Spring Application context in this layer, I preferred to use the MockitoExtension.class to test lightweight.

<br/>
```
@Mock
private HelloRepository repository;
```
<br/>

Since I only used MockitoExtension.class I also used @Mock to inject mocked dependent services, components, or repositories as you can see above. In case you want to use @MockBean instead of @Mock, you need to use @ExtendWith(SpringExtension.class)
For a detailed information, when to use SpringExtension and MockitoExtension see this [stackoverflow link](https://stackoverflow.com/questions/61433806/junit-5-with-spring-boot-when-to-use-extendwith-spring-or-mockito).
After testing service layer, I continued to test data layer in the project, so called repository in Spring-Boot. While testing data layer in Spring-Boot, use these annotations below at the top of your test class.

<br/>

```
@ExtendWith(SpringExtension.class)
@DataJpaTest
@AutoConfigureTestDatabase(replace = NONE, connection = EmbeddedDatabaseConnection.H2)
```
<br/>

Since Spring related application context should be loaded, I used @ExtendWith(SpringExtension.class) to tell JUnit 5 to use spring extension. Also @DataJpaTest will load not whole spring application context but only JPA related context. Apart from these, I used @AutoConfigureTestDatabase annotation to configure H2 in-memory database automatically for the repository test.
<br/>

```
@Autowired
private TestEntityManager entityManager;
```
<br/>

In addition to these I also used TestEntityManager in repository test to test the actual persistence to the database, if we use only the repository's save method, we do not actually fully test the behavior, jpa save method only saves to cache, therefore it is healthy to use TestEntityManager.

## TDD (Test-Driven-Development)
All along, I tried to apply TDD approach while writing code. That approach is:
1. Add a test
2. Run all tests and see if the new test fails
3. Write the code
4. Run tests
5. Refactor code
6. Repeat

To read more about TDD, see this [wikipedia link](https://en.wikipedia.org/wiki/Test-driven_development).
While I was learning how to code testing in Spring Boot I harnessed some tutorials below.

- [link 1](https://reflectoring.io/unit-testing-spring-boot/)
- [link 2](https://www.youtube.com/watch?v=s9vt6UJiHg4&t=1436s&ab_channel=SpringDeveloper)

You can see these useful links and more..

<br/>

Happy Coding :-)