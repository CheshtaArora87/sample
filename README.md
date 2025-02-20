# Sample Service


## Contents

> [Description](#description)

> [Dependencies](#dependencies) 

> [How to Run](#how-to-run)

> [Installation & Setup](#installation--setup)

> [Logging & Debugging](#logging--debugging)



## Description
This Sample Service is created to test the Connector Library by integrating it as a dependency. On service startup, it sends the config.json content as a string to the Connector Library, which validates the structure and ensures that all required definitions are provided. Once validated, the Connector Library publishes the content along with the service name to a RabbitMQ queue, allowing the Configuration Service to update and manage the definitions dynamically.

During runtime, whenever the service attempts to fetch a configuration, the Connector Library checks whether the service had provided at least one process and definition for that configuration at startup. This ensures that no service can use a configuration unless it has explicitly defined its purpose at the beginning.

### Validation Process

The Connector Library enforces the following validation rules:

- configDefinitions must not be null or empty.
- Each configDefinition must have a non-empty configName.
- Each configDefinition must include at least one warehouse process with a corresponding definition.
- Each warehouseProcess must have both valid name and a non-empty definition.

If any of these conditions fail, the Connector Library logs an error and throws an exception, preventing invalid configurations from being processed.

#### Expected config.json Format
The config.json file must adhere to the following structure:
```
{
  "configDefinitions": [
    {
      "configName": "INVENTORY_DASHBOARD_CONFIGURATION",
      "warehouseProcesses": [
        {
          "warehouseProcess": "Cycle count",
          "definition": "To show visibility of the updated inventory"
        },
        {
          "warehouseProcess": "Stock adjustment",
          "definition": "To reflect manual inventory corrections"
        }
      ]
    }
  ]
}
```

### Runtime Validation
Whenever the service attempts to fetch a configuration, the Connector Library checks whether the service provided at least one valid process and definition for that configuration at startup. This ensures that no service can use a configuration unless it has explicitly defined its purpose at the beginning.



## Dependencies
- `Java` - 17
- `springboot` - 3.4.2
- `RabbitMQ`
- `slf4j`: 2.0.16

## How to Run

```
//Clone the repository

git clone https://gitlab.addverb.com/addverb/capstone2k25/.git

cd config-test-1 

// Build the project
mvn clean install

// Run using Docker Compose
docker-compose up -d

// Run locally using Spring Boot
mvn spring-boot:run

```

## Installation & Setup

### `Prerequisites`

>**Java 17+**

>**Maven**

>**Docker & Docker Compose**

>**RabbitMQ**

### To Include the Connector Library as a Dependency
Add the following Maven dependency in your pom.xml:
```
<dependency>
    <groupId>com.addverb</groupId>
    <artifactId>connector</artifactId>
    <version>0.0.1-SNAPSHOT</version>
</dependency>

```

## Logging & Debugging

- **`Trace-ID & Correlation-ID`** added for debugging.

- **`Structured logging`** using Spring Boot Logger.

- **`Logs`** stored in logs/connector.log.




