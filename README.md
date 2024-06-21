

# Apache camel 

## What is Apache Camel?

Apache Camel is an open source integration framework that aims to make integrating systems easier. At the core of the Camel framework is a routing engine, or more precisely a routing-engine builder. It allows you to define your own routing rules, decide from which sources to accept messages, and determine how to process and send those messages to other destinations.
Camel has two main ways of defining routing rules:

- Java-based domain-specific language ( DSL )
- Spring XML configuration format.

`Java-based domain-specific language(DSL)`

A Java-based domain-specific language (DSL) is a programming language designed specifically to solve problems within a particular domain or application area. DSLs provide a way to express complex business rules or requirements in a more intuitive and natural way than is possible with general-purpose programming languages like Java.

Java-based DSLs are written in Java and typically use a fluent interface or a builder pattern to provide a high-level, domain-specific syntax for expressing complex computations or workflows. The DSL can be embedded within a Java application or library, or it can be a standalone language that is interpreted or compiled into Java bytecode.

`Spring XML configuration format.:-`

Spring XML configuration format is an XML-based format used to configure Spring Framework applications. It is the original way to configure Spring applications and is still widely used today, although it has been largely replaced by Java configuration and annotation-based configuration in recent versions of Spring.

The Spring XML configuration file typically has a root element <beans> which contains one or more nested bean definitions, each represented by a <bean> element. The <bean> element specifies the class or interface of the bean, along with any constructor arguments or property values that should be injected into the bean.



## Apache Camel use cases:-

- Enterprise application integration (EAI): - Camel can be used to integrate different applications and systems across an enterprise, allowing data and messages to be exchanged between different systems in a seamless and consistent way

 - Message-driven architecture (MDA): - Camel can be used to implement a message-driven architecture, where applications and services communicate through messages rather than direct method invocations.

- Microservices integration :- Camel can be used to integrate microservices by providing a flexible and lightweight message-based communication layer between different services.

- Data transformation: - Camel can be used to transform data from one format to another, such as transforming data from a database into a message format that can be sent to a messaging system.

- ETL (extract, transform, load): - Camel can be used to implement ETL workflows, where data is extracted from a source system, transformed into a different format, and loaded into a target system.

- API integration: - Camel can be used to integrate with APIs, providing a flexible and customizable way to manage and interact with APIs.

`Overall, Apache Camel is a powerful and flexible integration framework that can be used to implement a wide range of integration scenarios and workflows.`

## Key concepts and terminologies for apache camel:

- Route :-  A route in Apache Camel is a sequence of processing steps that a message follows from a source to a destination. A route typically consists of one or more endpoints, processors, and transformers.

- Endpoint:- An endpoint in Apache Camel represents a source or destination of messages. It can be a file, a database, a messaging system, or any other system that can send or receive messages.

- Processor:- A processor in Apache Camel is a component that can transform or manipulate messages as they pass through a route. Processors can be used to enrich, filter, aggregate, or transform messages, among other things.

- Component:- A component in Apache Camel is a pre-built module that provides integration with a specific system or protocol, such as HTTP, JMS, or FTP. Components can be used to easily integrate with external systems without having to write custom code.

- Exchange:- An exchange in Apache Camel represents a message and its associated metadata as it moves through a route. It contains information such as the message body, message headers, and properties.

- Message:- A message in Apache Camel represents a unit of data that is passed through a route. It can be any type of data, such as a file, a text message, or a Java object.

- RouteBuilder:- A RouteBuilder in Apache Camel is a Java class that is used to define routes and configure the CamelContext. It allows developers to define routes and configure components and processors in a fluent and easy-to-read manner.

- CamelContext:- A CamelContext in Apache Camel represents the runtime environment for a set of routes. It manages the lifecycle of routes and provides services such as error handling and message routing.

- Predicate:- A predicate in Apache Camel is a component that can be used to evaluate whether a message should be processed or not. Predicates can be used to filter messages based on their content or metadata.

- Transformer:- A transformer in Apache Camel is a component that can be used to convert a message from one format to another. Transformers can be used to convert messages between XML, JSON, or other formats.

### What are various components in Apache camel? Some of them that I have  used?

`Apache camel provides us with a number of components. These components make interacting create endpoints with which a system can interact with other external systems. For example using an ActiveMQ component we expose an ActiveMQ endpoint for interaction with external systems. There are more than 100 components provided by Apache Camel. Some of them are MAIL, JETTY, JMS, FILE, TIMMER, MARSHAL, UNMARSHAL etc. Apache camel also allows users to create custom components.`

#### 1)Mail Component:- 

In Apache Camel, the Mail component is used to send and receive emails from different mail servers using the Simple Mail Transfer Protocol (SMTP) and Internet Message Access Protocol (IMAP) protocols.

To use the Mail component, you first need to define a mail endpoint which specifies the mail server, username, password, and other settings such as the protocol to use, port number, and security options. Here's an example of a simple mail endpoint definition.

- Dependency for mail component:-

```sh
<dependency>
    <groupId>org.apache.camel</groupId>
    <artifactId>camel-mail</artifactId>
    <version>x.x.x</version>
    <!-- use the same version as your Camel core version -->
</dependency>
```
`Here i am using  <version>3.20.1</version>`

### Example on mail Component

Main class for routeBuilder

```sh
import org.apache.camel.CamelContext;
import org.apache.camel.impl.DefaultCamelContext;
public class MainApp {
    @SuppressWarnings("resource")
	public static void main(String[] args) {
        SimpleRouteBuilder routeBuilder = new SimpleRouteBuilder();
        CamelContext ctx = new DefaultCamelContext();
        try {
            ctx.addRoutes(routeBuilder);
            ctx.start();
            Thread.sleep(5 * 60 * 1000);
            ctx.stop();
        }
        catch (Exception e) {
            e.printStackTrace();
        }

    }
}
```
Simple SimpleRouteBuilder class for mail component to send message

```sh
import org.apache.camel.builder.RouteBuilder;
public class SimpleRouteBuilder extends RouteBuilder {
@Override
public void configure() throws Exception {
    

	from("file:C:\\inputFolder?noop=true")
			.doTry().setHeader("subject", simple("AkashThakur-TestMessage"))
			.setHeader("to", simple("akashthakurthakur111@gmail.com,testouthworking@gmail.com"))
			.transform(body().convertTo(String.class))
			  .to("smtps://smtp.gmail.com:465?username=akashthakurthakur111@gmail.com&password=jvpsldlqkqzjmnct");
}
}
```
` In this SimpleRouteBuilder class, the text taking from inputFolder that is in C drive of mine pc  and reading the containt on that text file and sending back to the mail With header and and you can avoide the line .transform(body().convertTo(String.class)) this is to conevrt body in string if it is in other format `

`For Password :- Go to 1) Manage your google account 2) Security 3) Signing in to Google 3a) by two step verification you need to generate your mail password and give in place of password`



### Same Example on XMl DSl

Main class to run xml application
```sh
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.ImportResource;

@SpringBootApplication
@ImportResource({"classpath:spring/camel-context.xml"})
public class MailXmlApplication {

	public static void main(String[] args) {
		SpringApplication.run(MailXmlApplication.class, args);
	}

}
```
#### camel-context.xml file in src/main/resources, spring folder, in that camel-context.xml file.
```sh
<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="
         http://www.springframework.org/schema/beans
         http://www.springframework.org/schema/beans/spring-beans.xsd
         http://camel.apache.org/schema/spring
         http://camel.apache.org/schema/spring/camel-spring.xsd">

  <camelContext id="myCamel" xmlns="http://camel.apache.org/schema/spring">

    <route id="fileroute">
	  <from uri="file:C:\\inputFolder?noop=true"/>
	  <to uri='smtps://smtp.gmail.com:465?username=akashthakurthakur111@gmail.com&amp;password=jvpsldlqkqzjmnct&amp;to=akashthakurthakur111@gmail.com&amp;subject=Message from Jetty&amp;from=akashthakurthakur111@gmail.com&amp;debugMode=true'/>
	</route>

  </camelContext>

</beans>
```

#### Some other Libraryes for both (xml dsl) and (java dsl).
```sh
        <dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>
		<dependency>
			<groupId>org.apache.camel.springboot</groupId>
			<artifactId>camel-spring-boot-starter</artifactId>
			<version>3.20.1</version>
		</dependency>
        <dependency>
		    <groupId>org.apache.camel</groupId>
		    <artifactId>camel-mail</artifactId>
		    <version>3.20.1</version>
		</dependency>
```

### 2)Jetty Component

The Jetty component provides HTTP-based endpoints for consuming and producing HTTP requests. That is, the Jetty component behaves as a simple Web server.

- dependency for jetty Component 

```sh
<dependency>
    <groupId>org.apache.camel</groupId>
    <artifactId>camel-jetty</artifactId>
    <version>x.x.x</version>
    <!-- use the same version as your Camel core version -->
</dependency>
```
Simple examples using jetty component

### JAVA DSl Example

MainApp class for to run application
```sh
import org.apache.camel.CamelContext;
import org.apache.camel.impl.DefaultCamelContext;
public class MainApp {
    @SuppressWarnings("resource")
	public static void main(String[] args) {
        SimpleRouteBuilder routeBuilder = new SimpleRouteBuilder();
        CamelContext ctx = new DefaultCamelContext();
        try {
            ctx.addRoutes(routeBuilder);
            ctx.start();
            Thread.sleep(5 * 60 * 1000);
            ctx.stop();
        }
        catch (Exception e) {
            e.printStackTrace();
        }

    }
}
```
SimpleRouteBuilder class for jetty component
```sh
import org.apache.camel.builder.RouteBuilder;
public class SimpleRouteBuilder extends RouteBuilder {
@Override
public void configure() throws Exception {
    

	from("jetty:http://localhost:8081/jetty")
			.doTry().setHeader("subject", simple("AkashThakur-TestMessage"))
			.setHeader("to", simple("akashthakurthakur111@gmail.com,testouthworking@gmail.com"))
			.transform(body().convertTo(String.class))
			  .to("smtps://smtp.gmail.com:465?username=akashthakurthakur111@gmail.com&password=jvpsldlqkqzjmnct");
}
}
```
`Use  postman to post a body of the message, Once you hit the send icon on post man in applicetuion console you will see it is connecting to smpt gmail to deliver a body of the message once connected it deliver the message to you mail id.`

 ### XML DSl Example  on Jetty component 

This is MailXmlApplication for to run appllication

```sh
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.ImportResource;

@SpringBootApplication
@ImportResource({"classpath:spring/camel-context.xml"})
public class MailXmlApplication {

	public static void main(String[] args) {
		SpringApplication.run(MailXmlApplication.class, args);
	}

}
```
The XMl file for Jetty
```sh
<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="
         http://www.springframework.org/schema/beans
         http://www.springframework.org/schema/beans/spring-beans.xsd
         http://camel.apache.org/schema/spring
         http://camel.apache.org/schema/spring/camel-spring.xsd">

  <camelContext id="myCamel" xmlns="http://camel.apache.org/schema/spring">

    <route id="apiroute">
	  <from uri="jetty:http://localhost:8081/jetty"/>
	  <setBody>
	    <simple>Hello, email recipient! This message was received from Jetty.</simple>
	  </setBody>
	  <to uri='smtps://smtp.gmail.com:465?username=akashthakurthakur111@gmail.com&amp;password=jvpsldlqkqzjmnct&amp;to=akashthakurthakur111@gmail.com&amp;subject=Message from Jetty&amp;from=akashthakurthakur111@gmail.com&amp;debugMode=true'/>
	</route>

  </camelContext>

</beans>
```
`Use  postman to post a body of the message, Once you hit the send icon on post man in applicetuion console you will see it is connecting to smpt gmail to deliver a body of the message once connected it deliver the message to you mail id.`

### JMS Component

This component allows messages to be sent to (or consumed from) a JMS Queue or Topic. It uses Spring’s JMS support for declarative transactions, including Spring’s JmsTemplate for sending and a MessageListenerContainer for consuming.

- Using ActiveMQ:- If you are using Apache ActiveMQ, you should prefer the ActiveMQ component as it has been optimized for ActiveMQ. All of the options and samples on this page are also valid for the ActiveMQ component.

- Transacted and caching:- See section Transactions and Cache Levels below if you are using transactions with JMS as it can impact performance.

- Request/Reply over JMS:- Make sure to read the section Request-reply over JMS further below on this page for important notes about request/reply, as Camel offers a number of options to configure for performance, and clustered environments.

```sh
<dependency>
    <groupId>org.apache.camel</groupId>
    <artifactId>camel-jms</artifactId>
    <version>x.x.x</version>
    <!-- use the same version as your Camel core version -->
</dependency>
```
`Here i am using ActiveMQ for jms, to use Activemq you need to install Activemq and set the path,I am using (5.17.3) and run by a command like ( Activemq start ).` 

### To connect with [Activemq] In Java dsl

```sh

    // Define the connection factory
    ActiveMQConnectionFactory connectionFactory = new ActiveMQConnectionFactory("tcp://localhost:61616");
   // Add the connection factory to the Camel context
    context.addComponent("jms", JmsComponent.jmsComponentAutoAcknowledge(connectionFactory));
```

### To connect with[activemq] In Xml dsl

```sh
 <bean id="jmsConnectionFactory" class="org.apache.activemq.ActiveMQConnectionFactory">
        <property name="brokerURL" value="tcp://localhost:61616" />
    </bean>

    <bean id="jmsConfig" class="org.apache.camel.component.jms.JmsConfiguration">
        <property name="connectionFactory" ref="jmsConnectionFactory" />
        <property name="concurrentConsumers" value="10" />
    </bean>
```

Required derpendency for Activemq to connect\

```sh
    <dependency>
		    <groupId>org.apache.camel</groupId>
		    <artifactId>camel-activemq</artifactId>
		    <version>3.20.1</version>
		</dependency>
		<dependency>
		    <groupId>org.apache.activemq</groupId>
		    <artifactId>activemq-camel</artifactId>
		    <version>5.16.5</version><!--$NO-MVN-MAN-VER$-->
		</dependency>
		<dependency>
		    <groupId>org.apache.activemq</groupId>
		    <artifactId>activemq-broker</artifactId>
		    <version>5.16.5</version><!--$NO-MVN-MAN-VER$-->
		</dependency>
```
## Example on Jms Mail Jetty Marshal UnMarshal Activemq All in one With Error handling.

`Before that will have a look at Marshal and UnMarshal`

`Camel has support for message transformation using several techniques. One such technique is Data Formats, where marshal and unmarshal comes from.
So in other words the Marshal and Unmarshal EIPs are used with Data Formats.`

### Marshal
`Transforms the message body (such as Java object) into a binary or textual format, ready to be wired over the network.`

### UnMarshal
`Transforms data in some binary or textual format (such as received over the network) into a Java object; or some other representation according to the data format being used.`

#### using Below code for dataFormats.

```sh
<dataFormats>
        <json id="jsonOrder" library="Jackson"/>
    </dataFormats>
```
## Example in Java DSl

```sh
import org.apache.activemq.ActiveMQConnectionFactory;
import org.apache.camel.CamelContext;
import org.apache.camel.LoggingLevel;
import org.apache.camel.impl.DefaultCamelContext;
import org.apache.camel.model.dataformat.JsonLibrary;
import org.springframework.core.annotation.Order;

import com.fasterxml.jackson.databind.SerializationFeature;

import org.apache.camel.builder.RouteBuilder;
import org.apache.camel.component.jackson.JacksonDataFormat;
import org.apache.camel.component.jms.JmsComponent;

public class CamelJettyJmsMailExample {

  @SuppressWarnings("resource")
public static void main(String[] args) throws Exception {
    CamelContext context = new DefaultCamelContext();

    // Define the connection factory
    ActiveMQConnectionFactory connectionFactory = new ActiveMQConnectionFactory("tcp://localhost:61616");
   // Add the connection factory to the Camel context
    context.addComponent("jms", JmsComponent.jmsComponentAutoAcknowledge(connectionFactory));

    JacksonDataFormat jacksonDataFormat = new JacksonDataFormat();
    jacksonDataFormat.disableFeature(SerializationFeature.FAIL_ON_EMPTY_BEANS);
    
    context.addRoutes(new RouteBuilder() {
    	
        public void configure() {
        // JacksonDataFormat jsonDataFormat = new JacksonDataFormat();
          from("jetty:http://localhost:8080/sendmail")
            .routeId("jettyroute")
          .doTry()
            .unmarshal().json()
            .to("jms:queue:testQueue")
          .doCatch(RuntimeException.class)
            .log("Runtime exception occurred: ${exception.message}")
            .endDoTry()
          .doCatch(Exception.class)
             .log("An error occurred: ${exception.message}")
          .endDoTry();
          ;}
      });
    
    context.addRoutes(new RouteBuilder() {
        public void configure() {
          from("jms:queue:testQueue")
          .routeId("mailroute")
          .doTry()
          .marshal().json()
            .to("log:mail?showBody=true")
            .to("smtps://smtp.gmail.com:465?username=akashthakurthakur111@gmail.com&password=jvpsldlqkqzjmnct&subject=Testmessage")
            .log("Email sent successfully")
          .doCatch(MessagingException.class)
          	.log(LoggingLevel.ERROR, "Error sending email: ${exception.message}")
          	.endDoTry();
          }
      });

    context.start();
    Thread.sleep(5  * 60 * 1000);
    context.stop();
  }
}
```
### Example in Xml DSL
  #### Main Class
```sh
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.ImportResource;

@SpringBootApplication
@ImportResource({"classpath:spring/camel-context.xml"})
public class CamelJmsXmlApplication {

	public static void main(String[] args) {
		SpringApplication.run(CamelJmsXmlApplication.class, args);
	}

}
```
#### Camel-context.xml file
```sh
<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="
         http://www.springframework.org/schema/beans
         http://www.springframework.org/schema/beans/spring-beans.xsd
         http://camel.apache.org/schema/spring
         http://camel.apache.org/schema/spring/camel-spring.xsd">
         
    <bean id="jmsConnectionFactory" class="org.apache.activemq.ActiveMQConnectionFactory">
        <property name="brokerURL" value="tcp://localhost:61616" />
    </bean>

    <bean id="jmsConfig" class="org.apache.camel.component.jms.JmsConfiguration">
        <property name="connectionFactory" ref="jmsConnectionFactory" />
        <property name="concurrentConsumers" value="10" />
    </bean>
         

  <camelContext id="myCamel" xmlns="http://camel.apache.org/schema/spring">
  
   <dataFormats>
        <json id="jsonOrder" library="Jackson"/>
    </dataFormats>

    <route id="apiroute">
	  <from uri="jetty:http://localhost:8081/sendmail"/>
	  <doTry>
	  
            <unmarshal>
                <json library="Jackson" disableFeatures="FAIL_ON_EMPTY_BEANS"/>
            </unmarshal>
            <to uri='jms:queue:testQueue'/>  
            <doCatch>
                <exception>java.lang.Exception</exception>
                <log message="Error occurred in application: ${exception.message}"/>
            </doCatch>
            <doCatch>
                <exception>java.lang.RuntimeException</exception>
                <log message="Error occurred at runtime: ${exception.message}"/>
            </doCatch>
            
        </doTry>
       <!--  <to uri='jms:queue:testQueue'/>  -->
	</route>
	
	<route id="mailroute">
	  <from uri="jms:queue:testQueue"/>
	  <doTry>
            <marshal>
                <json library="Jackson"/>
            </marshal>
             <to uri='smtps://smtp.gmail.com:465?username=akashthakurthakur111@gmail.com&amp;password=jvpsldlqkqzjmnct&amp;to=akashthakurthakur111@gmail.com&amp;subject=Message from Jetty&amp;from=akashthakurthakur111@gmail.com&amp;debugMode=true'/>
            <doCatch>
			    <exception>javax.mail.MessagingException</exception>
			    <log message="Error occurred while sending email: ${exception.message}"/>
			</doCatch>
         </doTry>
  <!--  <to uri='smtps://smtp.gmail.com:465?username=akashthakurthakur111@gmail.com&amp;password=jvpsldlqkqzjmnct&amp;to=akashthakurthakur111@gmail.com&amp;subject=Message from Jetty&amp;from=akashthakurthakur111@gmail.com&amp;debugMode=true'/>  -->
	</route>
  </camelContext>

</beans>
```

========================================================================================


