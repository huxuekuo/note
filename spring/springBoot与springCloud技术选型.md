

# springBoot与springCloud技术选型



不是每一个springboot都可以死搭配springcloud进行使用,这里提供一个网站可以查询应该如何搭配

```url
https://start.spring.io/actuator/info
```

浏览器输入该网址以后,会返回搭配类

```json
{
    "git":{
        "commit":{
            "time":"2020-04-17T16:18:36Z",
            "id":"7f707c1"
        },
        "branch":"7f707c17bed34a13b5bc9d5d58c71fc5a901c335"
    },
    "build":{
        "version":"0.0.1-SNAPSHOT",
        "artifact":"start-site",
        "name":"start.spring.io website",
        "versions":{
            "initializr":"0.9.0.BUILD-SNAPSHOT",
            "spring-boot":"2.2.6.RELEASE"
        },
        "group":"io.spring.start",
        "time":"2020-04-17T16:19:53.303Z"
    },
    "bom-ranges":{
        "azure":{
            "2.0.10":"Spring Boot >=2.0.0.RELEASE and <2.1.0.RELEASE",
            "2.1.7":"Spring Boot >=2.1.0.RELEASE and <2.2.0.M1",
            "2.2.0":"Spring Boot >=2.2.0.M1"
        },
        "codecentric-spring-boot-admin":{
            "2.0.6":"Spring Boot >=2.0.0.M1 and <2.1.0.M1",
            "2.1.6":"Spring Boot >=2.1.0.M1 and <2.2.0.M1",
            "2.2.1":"Spring Boot >=2.2.0.M1"
        },
        "solace-spring-boot":{
            "1.0.0":"Spring Boot >=2.2.0.RELEASE and <2.3.0.M1"
        },
        "solace-spring-cloud":{
            "1.0.0":"Spring Boot >=2.2.0.RELEASE and <2.3.0.M1"
        },
        "spring-cloud":{
            "Finchley.M2":"Spring Boot >=2.0.0.M3 and <2.0.0.M5",
            "Finchley.M3":"Spring Boot >=2.0.0.M5 and <=2.0.0.M5",
            "Finchley.M4":"Spring Boot >=2.0.0.M6 and <=2.0.0.M6",
            "Finchley.M5":"Spring Boot >=2.0.0.M7 and <=2.0.0.M7",
            "Finchley.M6":"Spring Boot >=2.0.0.RC1 and <=2.0.0.RC1",
            "Finchley.M7":"Spring Boot >=2.0.0.RC2 and <=2.0.0.RC2",
            "Finchley.M9":"Spring Boot >=2.0.0.RELEASE and <=2.0.0.RELEASE",
            "Finchley.RC1":"Spring Boot >=2.0.1.RELEASE and <2.0.2.RELEASE",
            "Finchley.RC2":"Spring Boot >=2.0.2.RELEASE and <2.0.3.RELEASE",
            "Finchley.SR4":"Spring Boot >=2.0.3.RELEASE and <2.0.999.BUILD-SNAPSHOT",
            "Finchley.BUILD-SNAPSHOT":"Spring Boot >=2.0.999.BUILD-SNAPSHOT and <2.1.0.M3",
            "Greenwich.M1":"Spring Boot >=2.1.0.M3 and <2.1.0.RELEASE",
            "Greenwich.SR5":"Spring Boot >=2.1.0.RELEASE and <2.1.14.BUILD-SNAPSHOT",
            "Greenwich.BUILD-SNAPSHOT":"Spring Boot >=2.1.14.BUILD-SNAPSHOT and <2.2.0.M4",
            "Hoxton.SR3":"Spring Boot >=2.2.0.M4 and <2.3.0.BUILD-SNAPSHOT",
            "Hoxton.BUILD-SNAPSHOT":"Spring Boot >=2.3.0.BUILD-SNAPSHOT"
        },
        "spring-cloud-alibaba":{
            "2.2.0.RELEASE":"Spring Boot >=2.2.0.RELEASE and <2.3.0.M1"
        },
        "spring-cloud-services":{
            "2.0.3.RELEASE":"Spring Boot >=2.0.0.RELEASE and <2.1.0.RELEASE",
            "2.1.7.RELEASE":"Spring Boot >=2.1.0.RELEASE and <2.2.0.RELEASE",
            "2.2.3.RELEASE":"Spring Boot >=2.2.0.RELEASE and <2.3.0.M1"
        },
        "spring-statemachine":{
            "2.0.0.M4":"Spring Boot >=2.0.0.RC1 and <=2.0.0.RC1",
            "2.0.0.M5":"Spring Boot >=2.0.0.RC2 and <=2.0.0.RC2",
            "2.0.1.RELEASE":"Spring Boot >=2.0.0.RELEASE"
        },
        "vaadin":{
            "10.0.17":"Spring Boot >=2.0.0.M1 and <2.1.0.M1",
            "14.1.25":"Spring Boot >=2.1.0.M1"
        }
    },
    "dependency-ranges":{
        "okta":{
            "1.2.1":"Spring Boot >=2.1.2.RELEASE and <2.2.0.M1",
            "1.4.0":"Spring Boot >=2.2.0.M1"
        },
        "mybatis":{
            "2.0.1":"Spring Boot >=2.0.0.RELEASE and <2.1.0.RELEASE",
            "2.1.2":"Spring Boot >=2.1.0.RELEASE"
        },
        "geode":{
            "1.2.6.RELEASE":"Spring Boot >=2.2.0.M5 and <2.3.0.M1",
            "1.3.0.M3":"Spring Boot >=2.3.0.M1 and <2.3.0.BUILD-SNAPSHOT",
            "1.3.0.BUILD-SNAPSHOT":"Spring Boot >=2.3.0.BUILD-SNAPSHOT"
        },
        "camel":{
            "2.22.4":"Spring Boot >=2.0.0.M1 and <2.1.0.M1",
            "2.25.1":"Spring Boot >=2.1.0.M1 and <2.2.0.M1",
            "3.2.0":"Spring Boot >=2.2.0.M1"
        }
    }
}
```

