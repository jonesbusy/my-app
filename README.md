## Reproducing steps

quarkus create app my-app
cd my-app

quarkus extension add olm
quarkus extension add qosdk
quarkus ext add io.quarkiverse.minio:quarkus-minio

Add on application.properties
```properties
quarkus.minio.url=http://localhost
quarkus.minio.port=9000
quarkus.minio.secure=false
quarkus.minio.access-key=minioaccess
quarkus.minio.secret-key=miniosecret

quarkus.minio.devservices.enabled=false
quarkus.minio.produce-metrics=false
```

On `GreetingResource`

```java
@Inject
MinioClient minioClient;
```

Run

```
mvn clean install
```

This fail with

```
Caused by: jakarta.enterprise.inject.UnsatisfiedResolutionException: Unsatisfied dependency for type io.micrometer.core.instrument.MeterRegistry and qualifiers [@Default]
        - injection target: parameter 'meterRegistry' of io.quarkiverse.minio.client.WithMetricsHttpClientProducer constructor
        - declared on CLASS bean [types=[io.quarkiverse.minio.client.WithMetricsHttpClientProducer, io.quarkiverse.minio.client.OptionalHttpClientProducer, java.lang.Object, java.util.function.Function<java.lang.String, java.util.Optional<okhttp3.OkHttpClient>>], qualifiers=[@Default, @Any], target=io.quarkiverse.minio.client.WithMetricsHttpClientProducer]
        at io.quarkus.arc.processor.Beans.resolveInjectionPoint(Beans.java:547)
        at io.quarkus.arc.processor.BeanInfo.init(BeanInfo.java:689)
        at io.quarkus.arc.processor.BeanDeployment.init(BeanDeployment.java:323)
        ... 12 more

```
