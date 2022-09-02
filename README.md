# Intercepting Loggers Lib

This library is interceptor of logs with main objective insert correlationId in all expose logs. 

# MDC Interceptor

The MDC (Mapped diagnostic context) manages contextual information on a per-thread basis.
Making possible interrupter of logs and inject uuid, this example, correlationID at logs.

# Advantage

It's not necessary UUID propagation in all logs of process. It's scalable and easy maintenance.

# How to use

- Download this project and open.
- Run command line mvn build to generate jar file of lib project.
    ```shell
        mvn build
    ```
- Open target directory and copy file interceptor-lib-1.0.jar
- Now in your project, create new folder in root path with name **'lib'**
- Paste file interceptor-lib-1.0.jar in **/lib**
- Open your pom.xml file at root path project.
- Insert in tag **dependencies** a new dependency
  ```xml
      <dependency>
        <groupId>br.com</groupId>
        <artifactId>interceptor-lib</artifactId>
        <systemPath>${basedir}/libs/interceptor-lib-1.0.jar</systemPath>
        <scope>system</scope>
        <version>1.0</version>
      </dependency>
  ```
- Run command line
  ```shell
      mvn install
  ```
- Create new class java in your config project
- Extended of class **WebMvcConfigurer**
- Implementation method addInterceptors
  ```java
  import br.com.interceptorlib.MdcInterceptor;
  import org.springframework.stereotype.Component;
  import org.springframework.web.servlet.config.annotation.InterceptorRegistry;
  import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;
  
  @Component
  public class WebMvcConfiguration implements WebMvcConfigurer {
  
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
      registry.addInterceptor(new MdcInterceptor());
    }
  }
  ```
- Add new variable **CorrelationId** in file logback.xml for expose correlation at logs
  ```xml
  <configuration>
    <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
      <encoder>
        <pattern>%d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} -correlationID=%X{CorrelationId} - %msg%n</pattern>
      </encoder>
    </appender>
  
    <root level="debug">
      <appender-ref ref="STDOUT" />
    </root>
  </configuration>
  ```
