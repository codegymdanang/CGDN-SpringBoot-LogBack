# CGDN-SpringBoot-LogBack
## Chú ý
+ Spring Boot sẽ tự động load file configure của LogBack nếu ta đặt đúng tên như sau 
logback-spring.xml <br>
logback.xml <br>
logback-spring.groovy <br>
logback.groovy <br>

## Có các cách configure logback để ghi vào file hoặc in ra console tiện cho việc debug trên production

#### Console

 <configuration>

    <appender name="CONSOLE" class="ch.qos.logback.core.ConsoleAppender">
        <layout class="ch.qos.logback.classic.PatternLayout">
            <Pattern>
                %d{HH:mm:ss.SSS} [%t] %-5level %logger{36} - %msg%n
            </Pattern>
        </layout>
    </appender>

    <logger name="codegym.danang" level="debug" additivity="false">
        <appender-ref ref="CONSOLE"/>
    </logger>

    <root level="error">
        <appender-ref ref="CONSOLE"/>
    </root>

</configuration>

#### Send logs to File + Rotate File

<configuration>

    <property name="HOME_LOG" value="logs/app.log"/>

    <appender name="FILE-ROLLING" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <file>${HOME_LOG}</file>

        <rollingPolicy class="ch.qos.logback.core.rolling.SizeAndTimeBasedRollingPolicy">
            <fileNamePattern>logs/archived/app.%d{yyyy-MM-dd}.%i.log.gz</fileNamePattern>
            <!-- each archived file, size max 10MB -->
            <maxFileSize>10MB</maxFileSize>
            <!-- total size of all archive files, if total size > 20GB, it will delete old archived file -->
            <totalSizeCap>20GB</totalSizeCap>
            <!-- 60 days to keep -->
            <maxHistory>60</maxHistory>
        </rollingPolicy>

        <encoder>
            <pattern>%d %p %c{1.} [%t] %m%n</pattern>
        </encoder>
    </appender>

    <logger name="codegym.danang" level="debug" additivity="false">
        <appender-ref ref="FILE-ROLLING"/>
    </logger>

    <root level="error">
        <appender-ref ref="FILE-ROLLING"/>
    </root>

</configuration>

#### Send error logs to email
<configuration>

    <appender name="CONSOLE" class="ch.qos.logback.core.ConsoleAppender">
        <layout class="ch.qos.logback.classic.PatternLayout">
            <Pattern>
                %d{HH:mm:ss.SSS} [%t] %-5level %logger{36} - %msg%n
            </Pattern>
        </layout>
    </appender>

    <appender name="EMAIL" class="ch.qos.logback.classic.net.SMTPAppender">
        <smtpHost>smtp.mailgun.org</smtpHost>
        <smtpPort>25</smtpPort>
        <username>123</username>
        <password>123</password>
        <to>TO_EMAIL</to>
        <to>RO_ANOTHER_EMAIL</to>
        <from>FROM_EMAIL</from>
        <subject>TESTING: %logger{20} - %m</subject>

        <layout class="ch.qos.logback.classic.html.HTMLLayout"/>
    </appender>

    <logger name="codegymdanang" level="error" additivity="false">
        <appender-ref ref="EMAIL"/>
    </logger>

    <root level="error">
        <appender-ref ref="CONSOLE"/>
    </root>

</configuration>

##### Tham khao them tai

https://lankydanblog.com/2019/01/09/configuring-logback-with-spring-boot/ <br>
https://www.mkyong.com/logging/logback-xml-example/ <br>
