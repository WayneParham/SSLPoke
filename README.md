# SSLPoke

Written by the folks at Atlassian, this program is very useful for testing SSL connections.  If you are getting PKIX errors, this tool can be used to find out what connections in yoiur system are failing.

For more information, see http://confluence.atlassian.com/display/JIRA/Connecting+to+SSL+services

## Building


```
javac SSLPoke.java
```

>**Note** since Java 11, you can run it directly without compiling it first:

```
java --source 11 SSLPoke.java <host> <port>
```


## Usage:

1. Test SSL connection for failure: 

    ```java SSLPoke <host> 443```

    which will produce an exception like this:
    
    ```
    javax.net.ssl.SSLHandshakeException: sun.security.validator.ValidatorException: PKIX path building failed: sun.security.provider.certpath.SunCertPathBuilderException: unable to find valid certification path to requested target
    ```

1. Create new empty keystore and add exported certificate from server:

    ```
    keytool -import -file certificate.cert -alias certificate -keystore trustStore.keystore
    ```
    
2. Do the test again and specify trustStore with password:
 
    ```java -Djavax.net.ssl.trustStore=trustStore.keystore -Djavax.net.ssl.trustStorePassword=changeit SSLPoke <host> 443```

    which will affirm that the connection works:
    
    ```Successfully connected```
