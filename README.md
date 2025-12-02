https://start.spring.io/

spring web 
```java
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-web</artifactId>
</dependency>
```
---
hello world

controller/HelloController.java

```java
@RestController
@RequestMapping("/api/hello")
public class HelloController {

    @RequestMapping(value = "", method = RequestMethod.GET)
    public ResponseEntity<?> hello(){
        return new ResponseEntity<>("Hello World!", HttpStatus.OK);
    }
    
}
```
commit - with controller

---
swagger
```java
<dependency>
	<groupId>io.springfox</groupId>
	<artifactId>springfox-swagger-ui</artifactId>
	<version>2.6.1</version>
</dependency><!-- https://mvnrepository.com/artifact/io.springfox/springfox-swagger2 -->
<dependency>
	<groupId>io.springfox</groupId>
	<artifactId>springfox-swagger2</artifactId>
	<version>2.6.1</version>
</dependency>
```
config/SwaggerConfig.java

```java
@Configuration
@EnableSwagger2
public class SwaggerConfig {
    @Bean
    public Docket api() {
        return new Docket(DocumentationType.SWAGGER_2)
                .select()
                .apis(RequestHandlerSelectors.any())
                .paths(PathSelectors.any())
                .build();
    }
}
```
http://localhost:8080/swagger-ui.html#

commit - with swagger

---
start docker
```java
docker run -d -p 5432:5432 -v postgresdata:/var/lib/postgresql/data -e POSTGRES_PASSWORD=postgres postgres
```
```java
docker ps
```
```java
docker logs [containerid]
```
---

kill docker

copy the container id and run
```java
docker kill [container id]
```
```java
docker ps
```
---

docker compose

add new file in main project
```java
docker-compose.yml
```
copy to docker-compose.yml
```java
version: "3"
services:
  db:
    image: postgres
    environment:
      POSTGRES_PASSWORD: postgres
    ports:
      - 5432:5432
    volumes:
      - ./postgresdata:/var/lib/postgresql/
    privileged: true
```
start docker compose
```java
docker-compose up -d
```
stop docker compose
```java
docker-compose down
```
commit - with docker compose

---
spring data

add to pom.xml
```java
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>

<dependency>
	<groupId>org.postgresql</groupId>
	<artifactId>postgresql</artifactId>
	<scope>runtime</scope>
</dependency>

<dependency>
	<groupId>org.hibernate</groupId>
	<artifactId>hibernate-validator</artifactId>
	<version>6.1.5.Final</version>
</dependency>
```
try to run the project, not run?

copy to application.properties
```java
spring.datasource.url=jdbc:postgresql://localhost:5432/postgres
spring.datasource.username=postgres
spring.datasource.password=postgres
spring.datasource.driver-class-name=org.postgresql.Driver

#JPA properties
spring.jpa.show-sql = true
spring.jpa.hibernate.ddl-auto = update
spring.jpa.database-platform=org.hibernate.dialect.PostgreSQLDialect
```
still not run? start docker
```java
docker-compose up -d
```
run, still no tables

---
create a table

add to com.first.project
```java
model/Student.java
```
add to Student.java
```java
@Entity
@Table(name="student")
public class Student implements Serializable {
    private static final long serialVersionUID = 1L;

    @Id
    @GeneratedValue(strategy= GenerationType.AUTO)
    private Long id;

    @NotEmpty
    @Length(max = 60)
    private String fullname;

    private Date birthDate;

    @Min(100)
    @Max(800)
    private Integer satScore;

    @Min(30)
    @Max(110)
    private Double graduationScore;

    @Length(max = 20)
    private String phone;

    @Length(max = 500)
    private String profilePicture;

}
```
commit - with data
