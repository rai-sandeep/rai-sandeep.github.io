---
title: Spring WebFlux and DB2 with rxjava2-jdbc
date: 2022-08-05 12:30:00 +0530
categories: [Spring WebFlux, DB2]
tags: [java, springboot, spring-webflux, reactive, db2, rxjava2-jdbc]
---

## 1. Overview

In this article, we're going to focus on connecting to a DB2 database from a Java Spring WebFlux application using non-blocking connection pools. We'll use [rxjava2-jdbc](https://github.com/davidmoten/rxjava2-jdbc) for this purpose.  
The primary reason for choosing rxjava2-jdbc is that there is no native implementation for a reactive DB2 driver in [r2dbc](https://r2dbc.io/drivers/) or [Spring Data R2DBC](https://spring.io/projects/spring-data-r2dbc). Thankfully, rxjava2-jdbc has the ability to take a normal JDBC driver and interact with it in a way that won’t block the application.  

## 2. Dependencies

To use rxjava2-jdbc with db2, add the following dependencies:
```xml
<dependency>
    <groupId>com.github.davidmoten</groupId>
    <artifactId>rxjava2-jdbc</artifactId>
    <version>0.2.11</version>
</dependency>
<dependency>
    <groupId>com.ibm.db2.jcc</groupId>
    <artifactId>db2jcc</artifactId>
    <version>db2jcc4</version>
</dependency>
```
See [mvnrepository](https://mvnrepository.com/artifact/com.github.davidmoten/rxjava2-jdbc) for latest version of rxjava2-jdbc.

## 3. Configuration

Create a [Database](https://github.com/davidmoten/rxjava2-jdbc/blob/master/rxjava2-jdbc/src/main/java/org/davidmoten/rx/jdbc/Database.java) bean with a non-blocking connection pool:
```java
@Bean
public Database database() {
    return Database.nonBlocking()
            .url("jdbc:db2://localhost:50000/testdb:currentSchema=DB2INST1;")
            .user("db2inst1")
            .password("testpass")
            .maxPoolSize(25)
            .build();
}
```
If you do not have a DB2 database for local testing, you can quickly setup one with Docker:
```console
docker run -itd --name db2 -e DBNAME=testdb -e DB2INST1_PASSWORD=testpass -e LICENSE=accept -p 50000:50000 --privileged=true ibmcom/db2
```
You may also want to connect to the database, create a table and insert some data for testing:
```sql
CREATE TABLE EMPLOYEE(ID INTEGER NOT NULL, NAME CHAR (30));
INSERT INTO DB2INST1.EMPLOYEE (ID, NAME) VALUES(10, 'James Bond');     
INSERT INTO DB2INST1.EMPLOYEE (ID, NAME) VALUES(20, 'Harry Potter');     
INSERT INTO DB2INST1.EMPLOYEE (ID, NAME) VALUES(30, 'Bruce Wayne');
```

## 4. Querying the Database

Use the `database` bean created above to run queries:
```java
public Flux<Employee> getAllEmployees() {
    String sql = "SELECT * FROM EMPLOYEE";

    Flowable<Employee> employeeFlowable = 
            database.select(sql).get(rs -> {
                return Employee.builder()
                        .id(rs.getInt("id"))
                        .name(rs.getString("name"))
                        .build();
    });

    return Flux.from(employeeFlowable);
}
```
Note that rxjava2-jdbc returns [Flowable](http://reactivex.io/RxJava/3.x/javadoc/io/reactivex/rxjava3/core/Flowable.html), but we return a Flux instead for compatibility with Spring WebFlux. For more examples, see [Spring WebFlux and rxjava2-jdbc - Medium](https://medium.com/netifi/spring-webflux-and-rxjava2-jdbc-83a94e71ba04).

## 5. Code Repository

You can find the full source code on [Github](https://github.com/rai-sandeep/java-reactive-api).

## 6. References

1. [davidmoten/rxjava2-jdbc - GitHub](https://github.com/davidmoten/rxjava2-jdbc)
2. [Introduction to rxjava-jdbc - Baeldung](https://www.baeldung.com/rx-java)
3. [Spring WebFlux and rxjava2-jdbc - Medium](https://medium.com/netifi/spring-webflux-and-rxjava2-jdbc-83a94e71ba04)