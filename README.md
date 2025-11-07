https://start.spring.io/ -> spring boot - 2.5.2

spring web 
```java
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>
```

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
commit - hello world

swagger

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

start docker
```java
docker run -d -p 5432:5432 -v postgresdata:/var/lib/postgresql/data -e POSTGRES_PASSWORD=postgres postgres
docker ps
docker logs [containerid]
```
docker compose - add new file in main project
```java
docker-compose.yml
```
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
      - ./postgresdata:/var/lib/postgresql/data
    privileged: true
```
start docker compose (kill the first docker)
```java
docker ps
```
copy the container id
```java
docker kill [container id]
```
run docker compose
```java
docker-compose up -d
```
commit - with docker compose
```java
docker-compose down
```
