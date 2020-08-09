# spring-cloud-stream
Spring Cloud Stream -  kafka binder

pom.xml
-------
<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-stream</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-stream-binder-kafka</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.kafka</groupId>
			<artifactId>spring-kafka</artifactId>
	</dependency>

spring-cloud-stream-publisher
-----------------------------

@SpringBootApplication
@EnableBinding(Source.class)
@RestController
public class SpringCloudStreamPublisherApplication {
    @Autowired
    private MessageChannel output;

    @PostMapping("/publish")
    public Book publishEvent(@RequestBody Book book) {
        output.send(MessageBuilder.withPayload(book).build());
        return book;
    }

    public static void main(String[] args) {
        SpringApplication.run(SpringCloudStreamPublisherApplication.class, args);
    }


Application.yml
---------------
spring:
  cloud:
    stream:
      bindings:
        output:
          destination: rahuls-stream

rahuls-stream - which is the topic.

==================================================================================

spring-cloud-stream-consumer
----------------------------


@SpringBootApplication
@EnableBinding(Sink.class)
public class SpringCloudStreamConsumerApplication {

    private Logger logger = LoggerFactory.getLogger(SpringCloudStreamConsumerApplication.class);

    @StreamListener("input")
    public void consumeMessage(Book book) {
        logger.info("Consume payload : " + book);
    }


    public static void main(String[] args) {
        SpringApplication.run(SpringCloudStreamConsumerApplication.class, args);
    }

Application.ymal file
spring:
  cloud:
    stream:
      bindings:
        input:
          destination: rahuls-stream

