<b>Step I:</b> Open the Command Prompt in the folder where you want to create the project and run the given command.</br>
<style="background-color: #FFFF00" >
mvn archetype:generate -DarchetypeGroupId=org.apache.camel.archetypes -DarchetypeArtifactId=camel-archetype-spring-boot -DarchetypeVersion=3.12.0 -DgroupId=com.tatacapital.loans -DartifactId=ESBTataCapitalLoans -Dversion=1.0-SNAPSHOT -DinteractiveMode=false
</style>

<b>Step II:</b> Create a file inside the resource folder "spring/camel-context.xml" and paste the given content.</br>
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:spring="http://camel.apache.org/schema/spring"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://camel.apache.org/schema/spring http://camel.apache.org/schema/spring/camel-spring.xsd">


    <camelContext id="camel" xmlns="http://camel.apache.org/schema/spring">
        <route>
            <from uri="timer:foo?period=3000" />
            <log message="Hello world" />
        </route>
    </camelContext>
</beans>


<b>Step III:</b> Import the camel-context.xml file into the main Java file</br>
<style="background-color: #FFFF00" >
@ImportResource({"classpath:spring/camel-context.xml"})
</style>


<b>Step I:</b> Now run our program</br>
<style="background-color: #FFFF00" >
mvn spring-boot:run
<style>
