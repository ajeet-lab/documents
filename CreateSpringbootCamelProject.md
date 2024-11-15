<b>Step I:</b> Open the Command Prompt in the folder where you want to create the project and run the given command.</br>
```
mvn archetype:generate -DarchetypeGroupId=org.apache.camel.archetypes -DarchetypeArtifactId=camel-archetype-spring-boot -DarchetypeVersion=3.12.0 -DgroupId=com.tatacapital.loans -DartifactId=ESBTataCapitalLoans -Dversion=1.0-SNAPSHOT -DinteractiveMode=false
```

<b>Step II:</b> Create a file inside the resource folder "spring/camel-context.xml" and paste the given content.</br>
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans 
	https://www.springframework.org/schema/beans/spring-beans.xsd
	http://camel.apache.org/schema/spring 
	https://camel.apache.org/schema/spring/camel-spring-2.23.3.xsd">
	
	<camelContext id="camel" xmlns="http://camel.apache.org/schema/spring">
		<route>
			<from uri="timer:foo?period=3000" />
			<log message="Hello world" />
		</route>
	</camelContext>

</beans>
```

<b>Step III:</b> Import the camel-context.xml file into the main Java file</br>
``` 
@ImportResource({"classpath:spring/camel-context.xml"}) 
```


<b>Step I:</b> Now run our program</br>
```
mvn spring-boot:run
```