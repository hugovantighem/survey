# survey


#################################################################
#
#       Sujets
#
#       
#
#################################################################

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
* ORM / Hibernate / JPA

Git




#################################################################
#
#       pull request avec merge oublié
#
#       217047390b70b11b3ccb83fdc5684e993ea48c61
#
#################################################################


    /**
<<<<<<< HEAD
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
=======
>>>>>>> remove base64 Picsure entry point
     * Initialize header with security
     * @return
     */
    private HttpHeaders getHeaders() {
        HttpHeaders headers = new HttpHeaders();
        headers.setAcceptLanguageAsLocales(Arrays.asList(Locale.FRENCH));
        headers.set("Authorization", "Bearer " + picsureSettings.getToken());
        return headers;
    }








#################################################################
#
#       spring security
#
#       fd25dcf652c28881eeb7e8ecdc72fd2e3cd8f8a2
#
#################################################################


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



#################################################################
#
#       rien n'est testé... sauf mockito
#
#       86f87f241777d3ac1c27e0614512f0a0827bd0df
#
#################################################################






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



#################################################################
#
#       RGBD test / client
#
#       
#
#################################################################

Spec client MySQL

Tests automatisés environnement de build MySQL

Environnement de dev test MySQL

Environnement de dev test H2

Avec une base cible client MySQL, si on lance les tests en environnement local en H2, l'environnement de build doit exécuter les tests sous H2

Avec une base cible client MySQL, si on lance les tests en environnement local en H2, l'environnement de build doit exécuter les tests sous MySQL




#################################################################
#
#       Code review
#
#       merge / rebase
#
#################################################################


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




#################################################################
#
#       Exception handling
#
#       ca7ebc412423b7e969eba6b5ff47426d8074e288
#
#################################################################
 

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

#################################################################
#
#       Conditions
#
#       && ||
#
#################################################################


// pseudo code 

getTrue(){
    display("True")
    return true
}

getFalse(){
    display("False")
    return false
}

getTrue() && getFalse()

True False
False True
True 
False
