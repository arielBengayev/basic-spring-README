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
commit - with controller

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
```
```java
docker ps
```
```java
docker logs [containerid]
```
kill docker
```java
docker ps
```
copy the container id

run
```java
docker kill [container id]
```
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
      - ./postgresdata:/var/lib/postgresql/data
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
