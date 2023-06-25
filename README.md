# Explanation of Config-Manager Wrapper Library

CONFIG SERVER:

• The ConfigServerApplication class is the entry point of the Config Server application.

• It is annotated with @SpringBootApplication, which enables Spring Boot auto-configuration.

• The @EnableConfigServer annotation enables the Config Server functionality.

• The main method starts the Spring Boot application.



CONFIG CLIENT:

• The ConfigClientApplication class is the entry point of the Config Client application.

• It is annotated with @SpringBootApplication, which enables Spring Boot auto-configuration.

• The main method starts the Spring Boot application.

For configuration:
• The application.yml file is used for configuring the server and client applications.
• In the Config Server's application.yml, the server is configured to run on port 8888.
• The Config Server is set to use a Git repository (https://github.com/parthvats/configrepo) as the source for configuration, with the default branch as master.
• In the Config Client's application.yml, the client is configured to import the configuration from the Config Server (http://localhost:8888).
• The management.endpoints.web.exposure.include property is set to *, allowing all endpoints to be exposed for management.

For the GreetingController:
• The GreetingController class is a REST controller that handles requests for the /greeting endpoint.
• It is annotated with @RestController, indicating that it provides RESTful endpoints.
• The @RefreshScope annotation enables dynamic reloading of the bean's properties when changes are detected.
• The message field is injected with a value from the configuration property my.greeting.
• The dbSettings field is autowired, which injects an instance of the DbSettings class configured with values from the configuration properties prefixed with db.
• The getValue method returns a concatenation of the message value and the values from dbSettings (connection, host, and port).

For the DbSettings:
• The DbSettings class is a configuration class that maps the properties with the prefix db to its corresponding fields.
• It is annotated with @Configuration to indicate that it is a configuration bean.
• The @ConfigurationProperties("db") annotation binds the properties with the prefix db to this class.
• It has connection, host, and port fields with their respective getters and setters.

Summary:

The code sets up a Config Server that pulls configuration from a Git repository and a Config Client that retrieves the configuration from the Config Server. The Config Client uses the retrieved configuration properties in the GreetingController and DbSettings classes to provide a dynamic greeting message and database connection details. The @RefreshScope annotation allows for dynamic reloading of the configuration properties without restarting the client application. And to achieve this you have to send a POST request to "localhost:8080/actuator/refresh" after changing the configuration details.
