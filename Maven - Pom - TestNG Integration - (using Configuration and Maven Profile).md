# Maven -> Pom -> TestNG Integration (! Configuration ve Profile)
Arkadaşlar Merhabalar

Bugün sizlere "TestNG Integration" ından bahsedeceğim. Ama bu kavramı mümkün olduğunca efektif bir şekilde sıradan kullanımın dışına çıkarak vermeye çalışacağım.

Normalde Maven projelerinin içinde testng.xml file larını run edebiliriz. Test projelerimizde testlerimizi belli test modulleri içinde gruplayıp onları da bir testng suite içine alarak toplu bir şekilde run etmemiz mümkün. Tabi bunun için pom.xml içerisinde bazı ayarlar yapmanız gerekiyor. Yapılan bu ayarla "mvn test" komut unu kullandığımızda istediğimiz testng.xml file çalıştırılabilir.

Bunu yapmanın - benim kullandığım - 3 yolu var :
1. Manual olarak Testng -  Right click - Run
2. maven command ı ile - Pom içinde configuration ayarı kullanarak
3. maven command ı ile - Pom içinde profile kullanarak -  mvn test –PRegression

Bunları anlattığım youtube videosuna şuradan erişebilirsiniz :

[![IMAGE ALT TEXT HERE](https://img.youtube.com/vi/gDnY0RSuqLs/0.jpg)](https://www.youtube.com/watch?v=gDnY0RSuqLs)


### regression.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE suite SYSTEM "https://testng.org/testng-1.0.dtd" >
<suite name="regression suite">
    <test name="integration regression">
        <classes>
            <class name="kodlanir.AbTest"></class>
            <class name="kodlanir.Cd"></class>
            <class name="kodlanir.EfTest"></class>
        </classes>
    </test>

</suite>
```

### smoke.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE suite SYSTEM "https://testng.org/testng-1.0.dtd" >
<suite name="smoke suite">

    <test name="integration smoke">
        <classes>
            <class name="kodlanir.Cd"></class>
        </classes>
    </test>

</suite>
```

### pom.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>kodlanir</groupId>
    <artifactId>mavendummy</artifactId>
    <version>1.0-SNAPSHOT</version>

    <name>mavendummy</name>
    <!-- FIXME change it to the project's website -->
    <url>http://www.kodlanir.com</url>


    <profiles>
        <profile>
            <id>Regression</id>
            <build>
                <pluginManagement>
                    <plugins>
                        <plugin>
                            <artifactId>maven-surefire-plugin</artifactId>
                            <version>2.22.1</version>
                            <configuration>
                                <suiteXmlFiles>
                                    <suiteXmlFile>regression.xml</suiteXmlFile>
                                </suiteXmlFiles>
                            </configuration>
                        </plugin>
                    </plugins>
                </pluginManagement>
            </build>
        </profile>

        <profile>
            <id>Smoke</id>
            <build>
                <pluginManagement>
                    <plugins>
                        <plugin>
                            <artifactId>maven-surefire-plugin</artifactId>
                            <version>2.22.1</version>
                            <configuration>
                                <suiteXmlFiles>
                                    <suiteXmlFile>smoke.xml</suiteXmlFile>
                                </suiteXmlFiles>
                            </configuration>
                        </plugin>
                    </plugins>
                </pluginManagement>
            </build>
        </profile>
    </profiles>



    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <maven.compiler.source>11</maven.compiler.source>
        <maven.compiler.target>11</maven.compiler.target>
    </properties>

    <dependencies>

        <!-- https://mvnrepository.com/artifact/org.testng/testng -->
        <dependency>
            <groupId>org.testng</groupId>
            <artifactId>testng</artifactId>
            <version>7.7.1</version>
        </dependency>


    </dependencies>

    <build>
        <pluginManagement><!-- lock down plugins versions to avoid using Maven defaults (may be moved to parent pom) -->
            <plugins>
                <!-- clean lifecycle, see https://maven.apache.org/ref/current/maven-core/lifecycles.html#clean_Lifecycle -->
                <plugin>
                    <artifactId>maven-clean-plugin</artifactId>
                    <version>3.1.0</version>
                </plugin>
                <!-- default lifecycle, jar packaging: see https://maven.apache.org/ref/current/maven-core/default-bindings.html#Plugin_bindings_for_jar_packaging -->
                <plugin>
                    <artifactId>maven-resources-plugin</artifactId>
                    <version>3.0.2</version>
                </plugin>
                <plugin>
                    <artifactId>maven-compiler-plugin</artifactId>
                    <version>3.8.0</version>
                </plugin>

                <plugin>
                    <artifactId>maven-surefire-plugin</artifactId>
                    <version>2.22.1</version>

                    <!--
                    <configuration>
                        <suiteXmlFiles>
                            <suiteXmlFile>regression.xml</suiteXmlFile>
                            <suiteXmlFile>smoke.xml</suiteXmlFile>
                        </suiteXmlFiles>
                    </configuration>
                    -->


                </plugin>


                <plugin>
                    <artifactId>maven-jar-plugin</artifactId>
                    <version>3.0.2</version>
                </plugin>
                <plugin>
                    <artifactId>maven-install-plugin</artifactId>
                    <version>2.5.2</version>
                </plugin>
                <plugin>
                    <artifactId>maven-deploy-plugin</artifactId>
                    <version>2.8.2</version>
                </plugin>
                <!-- site lifecycle, see https://maven.apache.org/ref/current/maven-core/lifecycles.html#site_Lifecycle -->
                <plugin>
                    <artifactId>maven-site-plugin</artifactId>
                    <version>3.7.1</version>
                </plugin>
                <plugin>
                    <artifactId>maven-project-info-reports-plugin</artifactId>
                    <version>3.0.0</version>
                </plugin>
            </plugins>
        </pluginManagement>
    </build>
</project>
```

[Contact me](https://www.linkedin.com/in/esmayilmazqa/)