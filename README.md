Employees Example Application
=============================

Assuming that you have env setup i.e
1. Proper version of Java installed in the machine.
2. Maven is there for building a maven project.
3. Neo4j is running as a graph database.
----

Additionally you can leverage the convenient Spring-Data repositories to get interface-based DAO implementations injected into your Spring Components.

.EmployeeRepository.java
[source,java]
----
@RepositoryRestResource(collectionResourceRel = "employees", path = "employees")
public interface EmployeeRepository extends PagingAndSortingRepository<Employee, Long> {

	Employee findByName(@Param("name") String name);

	List<Employee> findAll();
  
	@Query("MATCH (m:Employee) RETURN m LIMIT {limit}")
	Collection<Employee> graph(@Param("limit") int limit);
}
----

In our case we use the repository from a `EmployeeService` to compute the graph representation for the visualization.
The service is then injected into our main Boot application, which also doubles as `@RestMvcController` which exposes the `/graph` endpoint.

The other two endpoints for finding multiple employees by title and loading a single employee are provided out of the box by the http://projects.spring.io/spring-data-rest/[Spring-Data-Rest project] which exposes our `EmployeeRepository` as REST endpoints.

The rendering of the employee objects (and related entities) happens automatically out of the box via Jackson mapping.

== The Stack

These are the components of our Web Application:

* Application Type:         Spring-Boot Java Web Application (Jetty)
* Web framework:            Spring-Boot enabled Spring-WebMVC, Spring-Data-Rest
* Persistence Access:       Spring-Data-Neo4j 4.2.x
* Database:                 Neo4j-Server 3.x

== Endpoints:


Run instructions
================

	1. Configure the database connection data from Code/src/main/resources/application.properties
	2. Go to the Code folder and run "mvn spring-boot:run".
	3. Go to http://localhost:8080 or http://ip-address:8080 if you access from outside.
APIs
====
Get Employees
-------------
// JSON object for single employee with cast
curl http://localhost:8080/employees

Save employee
-------------
curl http://localhost:8080/save?name=kalyan

JSON object for whole graph viz (nodes, links - arrays)
-------------
curl http://localhost:8080/graph
# repo
