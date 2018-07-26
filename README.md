Ctrl+k v

##       Sujets



Développement
* Héritage
* Interface
* Visibilité méthodes
* [Exception](#exception)
* [Code review](#code-review)
* [Pull Request](#pull-request)
* environnement / outils dev
* découpler : DI / IoC

Testing
* TDD
* Mock
* [Mockito](#mockito)

Développement web
* REST
    * API
    * graph de dépendances
* Architecture appli web
    * [Simple Architecture](#simple-architecture)
    * [Complex Architecture](#complex-architecture)
* CORS
* OAuth, SAML, OPEN-ID
* [Asymetric key encryption](#asymetric-key-encryption)
* HTTPS certificats SSL
* CURL

Spring
* configuration externe : @Value @ConfigurationProperties
* @Bean
* @Configuration
* @Component
* @Service
* @RestController
* @Autowired
* @ConstraintValidator
* proxy

Git
* [History](#git-history)

Base de Données
* ORM / Hibernate / JPA
* Lib de gestion de schéma / migration

Environnement
* Docker



### Knowledge

* :worried: Ne connais pas
* :star: Connaissance faible
* :star::star: A l'aise
* :star::star::star: Forte connaissance



---
##  Pull Request    
       217047390b70b11b3ccb83fdc5684e993ea48c61

### Statement

```java
    /**
<<<<<< HEAD
     *
     * @param file
     * @return
     */
    private UploadResponse upload(File file) {
        MultiValueMap<String, Object> bodyMap = new LinkedMultiValueMap<>();
        bodyMap.add("upload", new FileSystemResource(file));
        HttpHeaders headers = getHeaders();
        headers.setContentType(MediaType.MULTIPART_FORM_DATA);
        HttpEntity<MultiValueMap<String, Object>> requestEntity = new HttpEntity<>(bodyMap, headers);
        return restTemplate.postForEntity(picsureSettings.getUploadUrl(), requestEntity, UploadResponse.class).getBody();
    }

    /**
======
>>>>>> remove base64 Picsure entry point
     * Initialize header with security
     * @return
     */
    private HttpHeaders getHeaders() {
        HttpHeaders headers = new HttpHeaders();
        headers.setAcceptLanguageAsLocales(Arrays.asList(Locale.FRENCH));
        headers.set("Authorization", "Bearer " + picsureSettings.getToken());
        return headers;
    }
```

### Response







### Knowledge

* :worried: Ne connais pas
* :star: Connaissance faible
* :star::star: A l'aise
* :star::star::star: Forte connaissance



---
##       spring security
##       fd25dcf652c28881eeb7e8ecdc72fd2e3cd8f8a2

### Statement

```java
@Override
     protected void configure(HttpSecurity http) throws Exception {
         http.authorizeRequests()
                 .antMatchers("/").permitAll()
                  // permit swagger
                  .antMatchers(AUTH_LIST).permitAll()
                  .antMatchers(HttpMethod.GET, "/swagger-ui.html**").permitAll()
                  //.antMatchers("/api/**").authenticated()
                  .antMatchers("/api/picsure/**").permitAll()
                  .and()
                  //.addFilterBefore(identificationFilter, BasicAuthenticationFilter.class)
                  .sessionManagement().sessionCreationPolicy(SessionCreationPolicy.STATELESS);
          http.headers().cacheControl();
      }
```

### Response

### Knowledge

* :worried: Ne connais pas
* :star: Connaissance faible
* :star::star: A l'aise
* :star::star::star: Forte connaissance



---
##       Mockito
##       86f87f241777d3ac1c27e0614512f0a0827bd0df

### Statement



```java
@RunWith(SpringRunner.class)
public class PicsureServiceImplTest {

    private static String MOCKED_JSON_PATH = "src/test/resources/picsure/";

    private ObjectMapper mapper = new ObjectMapper();

    @Autowired
    private PicsureService picsureService;

    @TestConfiguration
    static class PicsureServiceImplTestContextConfiguration {
        @Bean
        public PicsureService picsureService() {
            return Mockito.mock(PicsureServiceImpl.class);
        }
    }

    @Test
    public void should_get_a_token_in_response_when_upload_an_image() throws IOException {
        File file = new File(MOCKED_JSON_PATH + "picsure_upload_ok.json");
        Mockito.when(picsureService.upload(file)).thenReturn(mapper.readValue(file, PicsureUploadResponse.class));
        PicsureUploadResponse uploadResponse = picsureService.upload(file);
        Assert.assertNotNull(uploadResponse.getToken());
    }

    @Test
    public void should_recognize_object_when_lookup_for_a_valid_image() throws IOException {
        File file = new File(MOCKED_JSON_PATH + "picsure_lookup_ok.json");
        Mockito.when(picsureService.lookup(Mockito.anyString())).thenReturn(mapper.readValue(file, PicsureLookupResponse.class));
        PicsureLookupResponse lookupResponse = picsureService.lookup("any_string");
        Assert.assertTrue(lookupResponse.getObjectRecognition().length > 0);
    }

    @Test
    public void should_not_detect_fraud_when_lookup_for_a_valid_image() throws IOException {
        File file = new File(MOCKED_JSON_PATH + "picsure_lookup_ok.json");
        Mockito.when(picsureService.lookup(Mockito.anyString())).thenReturn(mapper.readValue(file, PicsureLookupResponse.class));
        PicsureLookupResponse lookupResponse = picsureService.lookup("any_string");
        Assert.assertNull(lookupResponse.getFraudDetection());
    }

}
```

### Response

### Knowledge

* :worried: Ne connais pas
* :star: Connaissance faible
* :star::star: A l'aise
* :star::star::star: Forte connaissance



---
##       RGBD test / client#


### Statement

Spec client MySQL

Tests automatisés environnement de build MySQL

Environnement de dev test MySQL

Environnement de dev test H2

Avec une base cible client MySQL, si on lance les tests en environnement local en H2, l'environnement de build doit exécuter les tests sous H2

Avec une base cible client MySQL, si on lance les tests en environnement local en H2, l'environnement de build doit exécuter les tests sous MySQL



### Response

### Knowledge

* :worried: Ne connais pas
* :star: Connaissance faible
* :star::star: A l'aise
* :star::star::star: Forte connaissance



---
##       Code review
##       merge / rebase

### Statement

```bash
@e-altran
test
f9407b8
 @e-altran
big refacto
9b6cafa
 @e-altran
fusion ok
270c9d0
 @e-altran
add gitignore
0278cf4
 @e-altran
remove target dir
2cc4f22
 @e-altran
rename picsure endpoint
14086e4
 @e-altran
remove target files
f649fa1
 @e-altran
restore gitgnore
2e90a41
 @e-altran
change gitignore
837e417
 @e-altran
add module handling
e538878
 @e-altran
Merge branch 'master' of https://github.com/ALTRAN-MONTPELLIER/chooz-… …
1b9eba4
 @e-altran
restore gitognore
08087cc
 @e-altran
change Picsure Training homepage text
6d70b3b
 @e-altran
Merge branch 'master' of https://github.com/ALTRAN-MONTPELLIER/chooz-… …
afaa5c7
 @e-altran
change homepage picsure training
c75c7a9
 @e-altran
pull OK
115646d
 @e-altran e-altran requested review from hv and cchoisy 2 minutes ago
```


### Response

### Knowledge

* :worried: Ne connais pas
* :star: Connaissance faible
* :star::star: A l'aise
* :star::star::star: Forte connaissance



---
##       Exception
##       ca7ebc412423b7e969eba6b5ff47426d8074e288

### Statement
 
```java
    @Override
    public Long storeAttachment(MultipartFile file) {
        // Persist into database
        Attachment attachment = attachmentService.save(file);

        // Move file to the storage folder
        store(file, storagePath, attachment.getName());

        return attachment.getId();
    }


    /**
     *
     * @param file
     * @param toPath
     */
    private void store(MultipartFile file, String toPath, String newFileName) {
        try {
            file.transferTo(new File(toPath + File.separator + newFileName));
        } catch (IOException e) {
            e.printStackTrace();
        }

    }
```

### Response

### Knowledge

* :worried: Ne connais pas
* :star: Connaissance faible
* :star::star: A l'aise
* :star::star::star: Forte connaissance



---
##       Conditions
##       

### Statement

```java
// pseudo code 

getTrue(){
    display("True")
    return true
}

getFalse(){
    display("False")
    return false
}

getTrue() || getFalse()

True False
False True
True 
False
```

### Response

### Knowledge

* :worried: Ne connais pas
* :star: Connaissance faible
* :star::star: A l'aise
* :star::star::star: Forte connaissance



---
##       Git History

### Statement

```java
* |   42806c8 Merge pull request #4 from hv/authentication
|\ \  
| * | 6109e00 (perso/authentication) Example of api specification test, authentication with secure endpoint
|/ /  
| * ee1e363 create launcher
| *   cdf4e37 Merge branch 'master' of https://github.com/ALTRAN-MONTPELLIER/chooz-back
| |\  
| |/  
|/|   
* |   b5f9e22 Merge pull request #3 from hv/documentation
|\ \  
| * | d7a404f (perso/documentation) Source code management and worflow documentation
|/ /  
* |   f283e70 Merge pull request #2 from e-altran/master
|\ \  
| | * 21f3d2d add lombok library
| |/  
| * b44ff65 add first controller / security
| *   bbe0a1a Merge branch 'master' of https://github.com/ALTRAN-MONTPELLIER/chooz-back
| |\  
| |/  
|/|   
* | d24bce4 Update README.md
* |   5829503 Merge pull request #1 from e-altran/master
|\ \  
| | * 73ae832 add first REST API
| |/  
| *   12a0509 Merge branch 'master' of https://github.com/ALTRAN-MONTPELLIER/chooz-back
| |\  
| |/  
|/|   
* | bc6b772 Initial commit
 /  
* e7f96e1 first commit
* 0734811 first commit
```


```java

* 1507dbf CHOOZ-72 inid db data for tests (Contract, Claim)
* f539b1e CHOOZ-72 inid db data for tests + helper (category, subcategory)
* 5ac1e26 CHOOZ-75 Entity relationships
* b9e7691 CHOOZ-75 Empty entity classes
* 4c5c043 CHOOZ-75 Rename Catalog as CatalogEntry
* e3c909f CHOOZ-71 Update DB schema : Catalog, SubCategory, Category, Contract, Claim
* ab4a663 Use PicsureSettings class instead of @Value
* a44801b Catalog list extraction from user identifier
* 26e1fb8 add web structure
* e49f0f4 add web structure
* 83c8b03 remove unused spaces
* 86f87f2 add fraud detection / unit tests with mockito
* 641c754 add picsure
* a5fdf48 Security clean refacto
* 7522e16 Add identification header support
* 7b823aa undo
* c42ce1f essai
* d7da638 Errors must be logged
* f2bc272 Example of bean defined according to property (the purpose is to be able to lanch the application without processing actual requests to external APIs)
* e273241 Example of Wiremock usage for mock external api service calls
* 3a7a37b Clean storage directory after test
* 07e41a6 Overriding application.properties for tests
* a90674d Specific class example for exterlanized configuration
*   056808e Merge pull request #22 from e-altran/master
|\  
| * 15240b0 add unit tests, improve code
| * d1e7cb0 change conf properties of storage path
| * 8bc7a09 refacto upload feature with only one attachment type
| *   17f4988 Merge branch 'master' of https://github.com/ALTRAN-MONTPELLIER/chooz-back
| |\  
| |/  
|/|   
* |   7260e79 Merge pull request #21 from cchoisy/master

```

### Response




### Knowledge

* :worried: Ne connais pas
* :star: Connaissance faible
* :star::star: A l'aise
* :star::star::star: Forte connaissance



---
##       Spring
##       external properties

### Statement

```java

@Validated
@Configuration
@ConfigurationProperties(prefix = "swaven.api")
public class SwavenSettings {

    @Valid @NotEmpty
    private String host;

    @Valid @NotEmpty
    private String accessKey;

    @Valid @NotEmpty
    private String searchEndpoint;

    @Valid @NotEmpty
    private String headerApiKey;
}

```

### Response



### Knowledge

* :worried: Ne connais pas
* :star: Connaissance faible
* :star::star: A l'aise
* :star::star::star: Forte connaissance



---
##       Simple-architecture
##       description

### Statement

```

Presentation
Android, Angular.JS WebClient, OAUTHv2

API
REST, Jersey (JAX-RS), Jackson (JSON de-/serialisation), DTO-objects (different from business logic models)

Business Logic
Spring for DI and Event handling. DDD-ish approach of model objects. Longer running jobs are offloaded with SQS in worker-modules.

DAO
Repository model with Spring JDBC-templates to store Entities. Redis (JEDIS) for Leaderboards, using Ordered Lists. Memcache for Token Store.

Database
MySQL, Memcached, Redis

```

### Response





### Knowledge

* :worried: Ne connais pas
* :star: Connaissance faible
* :star::star: A l'aise
* :star::star::star: Forte connaissance



---
##       Complex-architecture
##       description

### Statement

![archi](https://cdn-images-1.medium.com/max/1600/1*K6M-x-6e39jMq_c-2xqZIQ.png)

### Response




### Knowledge

* :worried: Ne connais pas
* :star: Connaissance faible
* :star::star: A l'aise
* :star::star::star: Forte connaissance



---
##       Asymetric key encryption


### Statement

```java
Pub_A
priv_A

```
```java
Pub_B
priv_B

```

Expliquer l'échange sécurisé d'un message A --> B
Expliquer la vérification de l'authenticité de l'expéditeur B --> A


### Response






### Knowledge

* :worried: Ne connais pas
* :star: Connaissance faible
* :star::star: A l'aise
* :star::star::star: Forte connaissance



---
##       Subject
##       description

### Statement

```java

//code here

```

### Response

### Knowledge

* :worried: Ne connais pas
* :star: Connaissance faible
* :star::star: A l'aise
* :star::star::star: Forte connaissance



