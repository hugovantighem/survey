##       Sujets



Développement
* Héritage
* Interface
* Visibilité méthodes
* Code review
* environnement / outils dev
* découpler : DI / IoC

Testing
* TDD
* Mock
* Mockito

Développement web
* REST
    * API
    * graph de dépendances
* Architecture appli web
        Simple (Controllers, Business, DAL)
* CORS
* OAuth, SAML, OPEN-ID
* HTTPS certificats SSL
* CURL

Spring
* configuration : @Value @Configuration
* @Bean
* @Component
* @Service
* @RestController
* @Autowired
* @ConstraintValidator
* proxy

Git
    Historique

Base de Données
* ORM / Hibernate / JPA
* Lib de gestion de schéma / migration





---
##       pull request avec merge oublié
##       217047390b70b11b3ccb83fdc5684e993ea48c61

## Statement

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

## Response







---
##       spring security
##       fd25dcf652c28881eeb7e8ecdc72fd2e3cd8f8a2

## Statement

```java
@Override
     protected void configure(HttpSecurity http) throws Exception {
         http.cors().configurationSource(corsConfigurationSource()).and().csrf().disable().authorizeRequests()
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

## Response

---
##       rien n'est testé... sauf mockito
##       86f87f241777d3ac1c27e0614512f0a0827bd0df

## Statement





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

## Response

---
##       RGBD test / client#


## Statement

Spec client MySQL

Tests automatisés environnement de build MySQL

Environnement de dev test MySQL

Environnement de dev test H2

Avec une base cible client MySQL, si on lance les tests en environnement local en H2, l'environnement de build doit exécuter les tests sous H2

Avec une base cible client MySQL, si on lance les tests en environnement local en H2, l'environnement de build doit exécuter les tests sous MySQL



## Response

---
##       Code review
##       merge / rebase

## Statement

```bash
@emmanuel-altran
test
f9407b8
 @emmanuel-altran
big refacto
9b6cafa
 @emmanuel-altran
fusion ok
270c9d0
 @emmanuel-altran
add gitignore
0278cf4
 @emmanuel-altran
remove target dir
2cc4f22
 @emmanuel-altran
rename picsure endpoint
14086e4
 @emmanuel-altran
remove target files
f649fa1
 @emmanuel-altran
restore gitgnore
2e90a41
 @emmanuel-altran
change gitignore
837e417
 @emmanuel-altran
add module handling
e538878
 @emmanuel-altran
Merge branch 'master' of https://github.com/ALTRAN-MONTPELLIER/chooz-… …
1b9eba4
 @emmanuel-altran
restore gitognore
08087cc
 @emmanuel-altran
change Picsure Training homepage text
6d70b3b
 @emmanuel-altran
Merge branch 'master' of https://github.com/ALTRAN-MONTPELLIER/chooz-… …
afaa5c7
 @emmanuel-altran
change homepage picsure training
c75c7a9
 @emmanuel-altran
pull OK
115646d
 @emmanuel-altran emmanuel-altran requested review from hugovantighem and cchoisy 2 minutes ago
```


## Response

---
##       Exception handling
##       ca7ebc412423b7e969eba6b5ff47426d8074e288

## Statement
 
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

## Response

---
##       Conditions
##       && ||

## Statement

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

## Response

---
##       Conditions
##       && ||

## Statement

```java
*   43a8899 Merge pull request #11 from emmanuel-altran/master
|\  
| *   0e368c7 Merge branch 'master' of https://github.com/ALTRAN-MONTPELLIER/chooz-back
| |\  
| |/  
|/|   
* | 0e8bf0e Run tests while mvn install
* | a6fdc51 Mock authentication in tests
* |   fafd4c6 Merge pull request #10 from emmanuel-altran/master
|\ \  
* \ \   335dd6a Merge pull request #9 from emmanuel-altran/master
|\ \ \  
* \ \ \   206f3dc Merge pull request #7 from emmanuel-altran/master
|\ \ \ \  
* \ \ \ \   01db461 Merge pull request #6 from emmanuel-altran/master
|\ \ \ \ \  
* \ \ \ \ \   88e3a5c Merge pull request #5 from emmanuel-altran/master
|\ \ \ \ \ \  
| | | | | | * 4a771eb add lombox to Login class
| | | | | |/  
| | | | | * a5163de add missing classes
| | | | |/  
| | | | * ffab3b8 refacto style
| | | |/  
| | | * 9644da1 refacto security / add token managment
| | |/  
| | * 5be0760 add specific scripts for test DB
| |/  
| * 38cd7be add launcher module / set tests
| *   8ebd175 add launcher module
| |\  
| |/  
|/|   
* |   42806c8 Merge pull request #4 from hugovantighem/authentication
|\ \  
| * | 6109e00 (perso/authentication) Example of api specification test, authentication with secure endpoint
|/ /  
| * ee1e363 create launcher
|/
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
*   056808e Merge pull request #22 from emmanuel-altran/master
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

## Response









---
##       Subject
##       description

## Statement

```java

//code here

```

## Response

