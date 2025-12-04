## open project
open a new spring boot project -> https://start.spring.io/

open the project in intellij

add to pom.xml
```
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-web</artifactId>
</dependency>
```
---
## hello world
add to com.first.project new package
```
controller
```
add to controller new java class
```
HelloController
```
copy to HelloController.java
```
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
## swagger
add to pom.xml
```
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
add to com.first.project new package
```
config
```
add to config new java class
```
SwaggerConfig
```
copy to SwaggerConfig.java
```
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
run the project

go to -> http://localhost:8080/swagger-ui.html#

commit - with swagger

---
## start docker
open terminal in intellij

run this command
```
docker run -d -p 5432:5432 -v postgresdata:/var/lib/postgresql/data -e POSTGRES_PASSWORD=postgres postgres
```
run to make sure the container created
```
docker ps
```
run to see logs
```
docker logs [container id]
```
 kill docker
 
run to copy the container id
```
docker ps
```
copy the container id and run
```
docker kill [container id]
```
run again to make sure the docker killed
```java
docker ps
```
---

## docker compose
add new file in main project
```java
docker-compose.yml
```
copy to docker-compose.yml
```
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
run in terminal to start docker compose
```
docker-compose up -d
```
run stop docker compose
```
docker-compose down
```
commit - with docker compose

---
## spring data
add to pom.xml
```
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
```
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
```
docker-compose up -d
```
run, still no tables

---
## create a table

make sure the container is up
```
docker-compose up -d
```
add to com.first.project new package
```
model
```
add to model new java class
```
Student
```
copy to Student.java
```
@Entity
@Table(name="student")
public class Student implements Serializable {
    private static final long serialVersionUID = 1L;

    @Id
    @GeneratedValue(strategy= GenerationType.AUTO)
    private Long id;

    @NotEmpty
    @Length(max = 30)
    private String fullName;

    @Min(100)
    @Max(800)
    private int psycoScore;

    @Min(30)
    @Max(100)
    private Double graduationScore;

    @Length(max = 10)
    private String phone;

    @Length(max = 500)
    private String profilePicture;

}
```
run the project and make sure in compiler the table is created

open tableplus, now yue can see your table

commit - with data
