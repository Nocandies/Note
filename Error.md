# Error

## Tomcat 启动时报错

>  Failed to start component [StandardEngine[Catalina].StandardHost[localhost].StandardContext

解决办法: 修改tomcat配置文件catalina.properties

```java
tomcat.util.scan.DefaultJarScanner.jarsToSkip=\ 						//值后面加  ",*"
```

