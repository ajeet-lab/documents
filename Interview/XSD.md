# What is xsd and why use use it?

**XSD (XML Schema Definition)** is like a set of rules or a blueprint that tells you how to build an XML document correctly. XML documents are used to store and transport data, and XSD helps ensure that the data is organized in a specific way.

### Why Use XSD?
1. **Ensures Data is Correct:** XSD checks if the data in the XML file is in the right format. For example, if you say an XML file should have a "name" that’s a string of text and an "age" that’s a number, XSD will check to make sure that’s true. This helps avoid mistakes, like putting letters where numbers should be.

2. **Keeps Data Consistent:** If two different systems (like a website and a database) need to share information, XSD ensures they both use the same format. This consistency makes it easier for systems to understand each other.

3. **Allows for Custom Rules:** With XSD, you can set specific rules for your data. For example, you can say that an "email" element must follow a certain pattern (like having an "@" symbol). This makes the data more reliable.

4. **Saves Time:** Many software tools can automatically check XML files against an XSD. This saves you from manually checking if the data is correct, reducing errors and speeding up development. 

In simple terms, **XSD** is used to make sure data stored in XML files is organized correctly and follows the rules you set, so that computers can easily understand and use it.

# Practicals
* **how call xsd in camel code:** in java **.to("validator:xsd/yourFileName.xsd")** and in xml **<to uri="validator:xsd/yourFileName.xsd" />**
### Without attribute
```
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema">
<xs:element name="Class">
    <xs:complexType>
        <xs:sequence>
            <xs:element name="Student" maxOccurs="unbounded" minOccurs="0">
                <xs:complexType>
                    <xs:sequence>
                        <xs:element name="firstname" type="xs:string"/>
                        <xs:element name="lastname" type="xs:string"/>
                        <xs:element name="age" type="xs:int"/>
                    </xs:sequence>
                </xs:complexType>
            </xs:element>
        </xs:sequence>
    </xs:complexType>
</xs:element>
</xs:schema>
```

### Request for without attribute
```
<Class>
    <Student>
        <firstname>Ajeet</firstname>
        <lastname>Ajeet</lastname>
        <age>34</age>
    </Student>
     <Student>
        <firstname>Ajeet</firstname>
        <lastname>Ajeet</lastname>
        <age>12</age>
    </Student>
</Class>
```


### With attribute
```
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema">
<xs:element name="Class">
    <xs:complexType>
        <xs:sequence>
            <xs:element name="Student" maxOccurs="unbounded" minOccurs="0">
                <xs:complexType>
                    <xs:sequence>
                        <xs:element name="firstname" type="xs:string"/>
                        <xs:element name="lastname" type="xs:string"/>
                        <xs:element name="age" type="xs:int"/>
                        
                        <xs:element name="city" >
                            <xs:complexType>
                                <xs:simpleContent>
                                    <xs:extension base="xs:string">
                                        <xs:attribute name="id" type="xs:string" use="optional" />
                                    </xs:extension>
                                </xs:simpleContent>
                            </xs:complexType>
                        </xs:element>

                    </xs:sequence>
                </xs:complexType>
            </xs:element>
        </xs:sequence>
    </xs:complexType>
</xs:element>
</xs:schema>
```
### Request for with attribute
```
<Class>
    <Student>
        <firstname>Ajeet</firstname>
        <lastname>Ajeet</lastname>
        <age>34</age>
        <city id="2">Gauspur</city>
    </Student>
     <Student>
        <firstname>Ajeet</firstname>
        <lastname>Ajeet</lastname>
        <age>12</age>
        <city id="2">Gauspur</city>
    </Student>
</Class>
```
