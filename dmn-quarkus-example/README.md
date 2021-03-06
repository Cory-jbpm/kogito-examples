# DMN + Quarkus example

## Description

A simple DMN service to evaluate a traffic violation.

Demonstrates DMN on Kogito capabilities, including REST interface code generation.

## Installing and Running

### Prerequisites
 
You will need:
  - Java 11+ installed 
  - Environment variable JAVA_HOME set accordingly
  - Maven 3.6.2+ installed

When using native image compilation, you will also need: 
  - [GraalVM 19.3.1](https://github.com/oracle/graal/releases/tag/vm-19.3.1) installed 
  - Environment variable GRAALVM_HOME set accordingly
  - Note that GraalVM native image compilation typically requires other packages (glibc-devel, zlib-devel and gcc) to be installed too.  You also need 'native-image' installed in GraalVM (using 'gu install native-image'). Please refer to [GraalVM installation documentation](https://www.graalvm.org/docs/reference-manual/aot-compilation/#prerequisites) for more details.

### Compile and Run in Local Dev Mode

```
mvn clean package quarkus:dev    
```

### Compile and Run in JVM mode

```
mvn clean package 
java -jar target/dmn-quarkus-example-runner.jar  
```

or on Windows

```
mvn clean package
java -jar target\dmn-quarkus-example-runner.jar
```

### Compile and Run using Local Native Image
Note that this requires GRAALVM_HOME to point to a valid GraalVM installation

```
mvn clean package -Pnative
```
  
To run the generated native executable, generated in `target/`, execute

```
./target/dmn-quarkus-example-runner 
```

Note: This does not yet work on Windows, GraalVM and Quarkus should be rolling out support for Windows soon.

## Example Usage

Once the service is up and running, you can use the following example to interact with the service.

### POST /Traffic Violation

Returns penalty information from the given inputs -- driver and violation:

```sh
curl -X POST -H 'Accept: application/json' -H 'Content-Type: application/json' -d '{"Driver":{"Points":2},"Violation":{"Type":"speed","Actual Speed":120,"Speed Limit":100}}' http://localhost:8080/Traffic%20Violation
```
or on Windows

```sh
curl -X POST -H "Accept: application/json" -H "Content-Type: application/json" -d "{\"Driver\":{\"Points\":2},\"Violation\":{\"Type\":\"speed\",\"Actual Speed\":120,\"Speed Limit\":100}}" http://localhost:8080/Traffic%20Violation
```

As response, penalty information is returned.

Example response:
```json
{
  "Violation":{
    "Type":"speed",
    "Speed Limit":100,
    "Actual Speed":120
  },
  "Driver":{
    "Points":2
  },
  "Fine":{
    "Points":3,
    "Amount":500
  },
  "Should the driver be suspended?":"No"
}
```
