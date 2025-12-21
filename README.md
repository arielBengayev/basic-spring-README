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
@Entity @Table(name="student2")
@Getter @Setter
public class Student implements Serializable {
    private static final long serialVersionUID = 1L;

    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    private Long id;

    @NotBlank @Length(min = 5, max = 30)
    private String fullName;

    @Min(100) @Max(800)
    private Integer psycoScore;

    @Min(30) @Max(100)
    private Double graduationScore;

    @NotBlank @Length(min = 10, max = 10)
    private String phone;

    @Length(max = 500)
    private String profilePicture;

    public Student(){}

    public Student(String fullName, Integer psycoScore, Double graduationScore, String phone, String profilePicture) {
        this.fullName = fullName;
        this.psycoScore = psycoScore;
        this.graduationScore = graduationScore;
        this.phone = phone;
        this.profilePicture = profilePicture;
}
```
run the project and make sure in compiler the table is created

open tableplus, now yue can see your table

commit - with data

---
## CRUD

add to com.first.project new package
```
repo
```
add to repo new java class
```
StudentRepository
```
copy to StudentRepository
```
import com.first.project.model.Student;
import org.springframework.data.repository.CrudRepository;

public interface StudentRepository extends CrudRepository<Student,Long> {

}
```
add to com.first.project new package
```
service
```
add to service new java class
```
StudentService
```
copy to StudentService
```
@Service
public class StudentService {
    @Autowired
    StudentRepository repository;

    public Iterable<Student> all() {
        return repository.findAll();
    }

    public Optional<Student> findById(Long id) {
        return repository.findById(id);
    }

    public Student save(Student student) {
        return repository.save(student);
    }

    public void delete(Student student) {
        repository.delete(student);
    }
}
```
add to com.first.project new package
```
dto
```
add to dto new java class
```
StudentIn
```
copy to StudentIn
```
@Getter @Setter
public class StudentIn implements Serializable {
    @NotBlank @Length(min = 5, max = 30)
    private String fullName;

    @Min(100) @Max(800)
    private Integer psycoScore;

    @Min(30) @Max(100)
    private Double graduationScore;

    @NotBlank @Length(min = 10, max = 10)
    private String phone;


    public Student toStudent(StudentIn studentIn) {
        return new Student(
                studentIn.getFullName(),
                studentIn.getPsycoScore(),
                studentIn.getGraduationScore(),
                studentIn.getPhone(),
                null
        );
    }

    public void updateStudent(Student student) {
        student.setFullName(fullName);
        student.setPsycoScore(psycoScore);
        student.setGraduationScore(graduationScore);
        student.setPhone(phone);
    }
}
```
commit - with CRUD

---
## REST

add to controller new class
```
StudentController
```
copy to StudentController
```
@RestController
@RequestMapping("/api/students")
public class StudentController {
    @Autowired
    StudentService studentService;

    @GetMapping("/all")
    public ResponseEntity<?> getAllStudents() {
        return new ResponseEntity<>(studentService.all(), HttpStatus.OK);
    }

    @GetMapping("/{id}")
    public ResponseEntity<?> getOneStudent(@PathVariable Long id) {
        return new ResponseEntity<>(studentService.findById(id), HttpStatus.OK);
    }

    @PostMapping
    public ResponseEntity<?> insertStudent(@RequestBody StudentIn studentIn) {
        Student student = studentIn.toStudent(studentIn);
        student = studentService.save(student);
        return new ResponseEntity<>(student, HttpStatus.OK);
    }

    @PutMapping(value = "/{id}")
    public ResponseEntity<?> updateStudent(@PathVariable Long id, @RequestBody StudentIn student) {
        Optional<Student> dbStudent = studentService.findById(id);
        student.updateStudent(dbStudent.get());
        Student updatedStudent = studentService.save(dbStudent.get());
        return new ResponseEntity<>(updatedStudent, HttpStatus.OK);
    }

    @DeleteMapping("/{id}")
    public ResponseEntity<?> deleteStudent(@PathVariable Long id) {
        Optional<Student> dbStudent = studentService.findById(id);
        studentService.delete(dbStudent.get());
        return new ResponseEntity<>("DELETED", HttpStatus.OK);
    }
}
```
commit - with REST
