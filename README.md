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
* [Cycle](#cycle)
    * Spec
    * Test
    * Implémentation
    * Architecture
    * API
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
* [Spring configuration](#configurationProperties) : @Value @ConfigurationProperties
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


Misc
* Awesome skills here !
* Or the bad ones



### Knowledge

* :worried: Ne connais pas
* :star: Connaissance faible
* :star::star: A l'aise
* :star::star::star: Forte connaissance



---
##       Conditions       

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

<details> 
  <summary>Expectations </summary>
   
```
True
```
</details>

### Knowledge

* :worried: Ne connais pas
* :star: Connaissance faible
* :star::star: A l'aise
* :star::star::star: Forte connaissance



---
##  Pull Request    
       217047390b70b11b3ccb83fdc5684e993ea48c61

### Statement

La personne qui fait la code review découvre le code suivant, que dire du code analysé ?

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

<details> 
  <summary>Expectations </summary>
   
```
Le développeur a mal résolu les conflits
```
</details>








### Knowledge

* :worried: Ne connais pas
* :star: Connaissance faible
* :star::star: A l'aise
* :star::star::star: Forte connaissance



---
##       spring security
       fd25dcf652c28881eeb7e8ecdc72fd2e3cd8f8a2

### Statement

Que dire du code suivant retrouvé dans une pull request ?

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

<details> 
  <summary>Expectations </summary>
   
```
La sécurité est désactivée.
Eviter de commiter du code commenté.
Les éléments "/api/picsure/**" et "/swagger-ui.html**" devraient être inclus dans AUTH_LIST
Code style (Indentation)
```
</details>

### Knowledge

* :worried: Ne connais pas
* :star: Connaissance faible
* :star::star: A l'aise
* :star::star::star: Forte connaissance



---
##       Mockito
       86f87f241777d3ac1c27e0614512f0a0827bd0df

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

}
```

### Response

<details> 
  <summary>Expectations </summary>
   
```
Rien n'est testé !
```
</details>

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
       merge / rebase

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
 @e-altran e-altran requested review from hv and cc 2 minutes ago
```


### Response

<details> 
  <summary>Expectations </summary>
   
```
Historique sale
Le développeur fait des merges, non des rebase
Messages de commit non explicites
```
</details>

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

<details> 
  <summary>Expectations </summary>
   
```
La gestion de l'exception n'est pas très bonne.
Il faudrait logguer l'exception et la traiter.
```
</details>

### Knowledge

* :worried: Ne connais pas
* :star: Connaissance faible
* :star::star: A l'aise
* :star::star::star: Forte connaissance



---
##       Git workflow

### Statement

Vous intégrez une équipe pour travailler sur un projet en cours avec une gestion de sources sous git. Vous allez être en charge du développement d'une fonctionnalité 'Gestion des moyens de paiement', comment procédez vous ?

### Response

### Knowledge

* :worried: Ne connais pas
* :star: Connaissance faible
* :star::star: A l'aise
* :star::star::star: Forte connaissance



---
##       Git commands

### Statement

Que montrent ces lignes ? 

```bash
april	git@gitlab.april-waf.local:innovation/chooz-api.git (fetch)
april	git@gitlab.april-waf.local:innovation/chooz-api.git (push)
origin	git@github.com:ALTRAN-MONTPELLIER/chooz-back.git (fetch)
origin	git@github.com:ALTRAN-MONTPELLIER/chooz-back.git (push)
perso	git@github.com:hugovantighem/chooz-back.git (fetch)
perso	git@github.com:hugovantighem/chooz-back.git (push)

```

```bash
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	modified:   chooz-web/package-lock.json

Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	deleted:    chooz-launcher/src/test/resources/storage/.gitignore

Untracked files:
  (use "git add <file>..." to include in what will be committed)

	chooz-launcher/src/test/resources/storage/6cb39468-a88d-49a8-866b-f1c2120968a9.txt

```


### Response

### Knowledge

* :worried: Ne connais pas
* :star: Connaissance faible
* :star::star: A l'aise
* :star::star::star: Forte connaissance



---
##       Git History


Que peut-on dire des historiques git suivants ?

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
* |   7260e79 Merge pull request #21 from cc/master

```

### Response

<details> 
  <summary>Expectations </summary>
   
```
1 => les développeurs mergent master vers sa branche de travail locale ou pull puis mergent ensuite dans la master
2 => les développeurs utilisent ensuite la commande `git rebase`
les messages de commit sont dupliqués (mauvais historique)
 26e1fb8 add web structure
 e49f0f4 add web structure
 e7f96e1 first commit
 0734811 first commit
les messages de commit ne sont pas explicites
 7b823aa undo
 c42ce1f essai
les messages de commit référencent un ticket
 b9e7691 CHOOZ-75 Empty entity classes
 f539b1e CHOOZ-72 inid db data for tests + helper (category, subcategory)
 5ac1e26 CHOOZ-75 Entity relationships

```
</details>




### Knowledge

* :worried: Ne connais pas
* :star: Connaissance faible
* :star::star: A l'aise
* :star::star::star: Forte connaissance



---
##       ConfigurationProperties

### Statement

A quoi sert ce code ?

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

Que se passe-t'il si l'on démarre l'application avec les lignes suivantes dans application.properties

```
swaven.api.host=192.168.3.1
swaven.api.accessKey=123456
swaven.api.searchEndpoint=/search
swaven.api.headerApiKey=
```

### Response

<details> 
  <summary>Expectations </summary>
   
```
fichier prenant en charge les propriété externalisées pour le préfix 'swaven.api'
l'application ne démarre pas car le Bean 'SwavenSettings' ne peut pas être créé (swaven.api.headerApiKey ne doit pas être vide)
```
</details>



### Knowledge

* :worried: Ne connais pas
* :star: Connaissance faible
* :star::star: A l'aise
* :star::star::star: Forte connaissance



---
##       Simple-architecture
##       description

### Statement

Décrire une architecture web et couremment utilisée découpée en différentes couches.

<details> 
  <summary>Hints </summary>

```
Presentation

API

Business Logic

DAO

Database
```
</details>


### Response

<details> 
  <summary>Expectations </summary>

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
</details>





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
##       Database
       description

### Statement

Votre client qui vends des burgers souhaite pouvoir saisir les commandes via une application web.

### Specs 

Un client peut effectuer une commande regroupant différents burgers.

Une commande portera un numéro de commande.

Un client peut associer un code promo à sa commande.

Un code promo ne peut être utilisé que pour une seule commande.

Un client possède une liste d'adresses (ex: domicile, travail).

Proposer un schéma de la base de données associée.

<details> 
  <summary>Database script </summary>
   
```sql


create table customer (
  id int(11) NOT NULL AUTO_INCREMENT,
  name varchar(45) DEFAULT NULL,
  PRIMARY KEY (id)
);

create table address (
  id int(11) NOT NULL AUTO_INCREMENT,
  `desc` varchar(45) DEFAULT NULL,
  customer_id int(11) DEFAULT NULL,
  PRIMARY KEY (id)
);

create table burger (
  id int(11) NOT NULL AUTO_INCREMENT,
  name varchar(45) DEFAULT NULL,
  PRIMARY KEY (id)
);

create table `order` (
  id int(11) NOT NULL AUTO_INCREMENT,
  number varchar(45) DEFAULT NULL,
  code varchar(45) DEFAULT NULL,
  PRIMARY KEY (id)
);

create table order_line (
  customer_id int(11) NOT NULL,
  burger_id int(11) NOT NULL,
  order_id int(11) NOT NULL,
  qte int(11) NOT NULL,
  PRIMARY KEY (customer_id, burger_id, order_id)
);


ALTER TABLE `address` ADD CONSTRAINT `fk_address_customer` FOREIGN KEY (`customer_id`) REFERENCES `customer` (`id`);
ALTER TABLE `order_line` ADD CONSTRAINT `fk_order_line_customer` FOREIGN KEY (`customer_id`) REFERENCES `customer` (`id`);
ALTER TABLE `order_line` ADD CONSTRAINT `fk_order_line_burger` FOREIGN KEY (`burger_id`) REFERENCES `burger` (`id`);
ALTER TABLE `order_line` ADD CONSTRAINT `fk_order_line_order` FOREIGN KEY (`order_id`) REFERENCES `order` (`id`);

ALTER TABLE `order` ADD CONSTRAINT uc_order_number UNIQUE (number);
ALTER TABLE `order` ADD CONSTRAINT uc_order_code UNIQUE (code);

```
</details>


<details> 
  <summary>ERD 1 </summary>
<pre>
+--------------+         +---------------+       +----------------+           +----------------+
|    Address   |         |   Customer    |       |    Order       |           |     Burger     |
|              |         |               |       |                |      +--> |  id            |
|  customer_id +-------> |  id           | <--+  |   id           |      |    |                |
|              |         |               |    |  |   number       |      |    |  name          |
|  desc        |         |  name         |    |  |   code         |      |    |                |
|              |         |               |    +--+   customer_id  |      |    |                |
|              |         |               |       |   burger_id    +------+    |                |
+--------------+         +---------------+       +----------------+           +----------------+

</pre>
</details>


<details> 
  <summary>ERD 2 </summary>
<pre>
                                                                              +----------------+
                                                                              |     Burger     |
                                                                        +---> |  id            |
                                                                        |     |                |
                                                                        |     |  name          |
 +--------------+         +---------------+       +-----------------+   |     |                |
 |    Address   |         |   Customer    |       |    Order_line   |   |     |                |
 |              |         |               |       |                 |   |     +----------------+
 |  customer_id +-------> |  id           | <-----+   customer_id   |   |
 |              |         |               |       |   burger_id     +---+
 |  desc        |         |  name         |       |   order_id      +---+     +----------------+
 |              |         |               |       |   quantity      |   |     |    Order       |
 +--------------+         +---------------+       +-----------------+   |     |                |
                                                                        +---> |   id           |
                                                                              |   number       |
                                                                              |   code         |
                                                                              |                |
                                                                              +----------------+





</pre>
</details>

### Response

### Knowledge

* :worried: Ne connais pas
* :star: Connaissance faible
* :star::star: A l'aise
* :star::star::star: Forte connaissance




---
##       Cycle

### Specifications

Damier 8x8 cases

```
+-----------------------+
|xx|  |xx|  |xx|  |xx|  |
+-----------------------+
|  |xx|  |xx|  |xx|  |xx|
+-----------------------+
|xx|  |xx|  |xx|  |xx|  |
+-----------------------+
|  |xx|  |xx|  |xx|  |xx|
+-----------------------+
|xx|  |xx|  |xx|  |xx|  |
+-----------------------+
|  |xx|  |xx|  |xx|  |xx|
+-----------------------+
|xx|  |xx|  |xx|  |xx|  |
+-----------------------+
|  |xx|  |xx|  |xx|  |xx|
+-----------------------+

'xx' -> "black
'  ' -> "white


```

Classe `Checkerboard` permettant de retrouver la couleur d'une case correspondant à la ligne et colonne données en paramètres via une méthode `getColor(line, col)`.

Prévu d'être accessible via une application web.

D'autres fonctionnalités sont prévues pour la gestion d'un jeux d'échecs.

```java
pseudo code

considering the first index for row and columns to be 0

line = 2
col = 4
color = getColor(line, col)

// color == "black"

```

<details> 
  <summary>Hint</summary>
    <pre>
        Interface ICheckerboard {
            function getColor (int line, int column): string;
        }
    </pre>
</details>

<details> 
  <summary>T </summary>
    <pre>

        Class CheckerboardTest {
            ICheckerboard checkerboard

            function specification_case_test (){
                color = checkerboard.getColor(2, 4)

                assert(color == 'black')
            }

            function limit_test (){
                color = checkerboard.getColor(0, 0)
                assert(color == 'black')

                color = checkerboard.getColor(0, 7)
                assert(color == 'white')

                color = checkerboard.getColor(7, 0)
                assert(color == 'white')

                color = checkerboard.getColor(7, 7)
                assert(color == 'black')
            }

            function out_limit_test (){
                color = checkerboard.getColor(-1, 0)
                assert(color == null)

                color = checkerboard.getColor(0, -1)
                assert(color == null)

                color = checkerboard.getColor(8, 0)
                assert(color == null)

                color = checkerboard.getColor(0, 8)
                assert(color == null)
            }
        }

    </pre>
</details>

<details> 
  <summary>I </summary>
    <pre>

        Class Checkerboard implements ICheckerboard {
            MIN_LINE = 0
            MAX_LINE = 0
            MIN_COLUMN = 7
            MAX_COLUMN = 7

            /**
            * Return case color according to line and column
            * line and column first index is 0
            * return 'black' or 'white'
            * return null when if line and/or column is out of bounds
            */
            function getColor (int line, int column){
                if(line < MIN_LINE || line > MAX_LINE ){
                    // line out of bounds
                    return null
                }
                if(column < MIN_COLUMN || column > MAX_COLUMN ){
                    // column out of bound
                    return null
                }
                // check if sum of line and column is even or odd
                return (line + column) % 2 == 0 ? 'black' : 'white'
            }
        }

    </pre>
</details>

### Architecture

Front : SPA + REST / Websocket

Proposer une architecture (Backend)

Proposer une API pour interagir avec la partie métier

Proposer les tests associés

<details> 
  <summary>API </summary>
    <pre>GET /api/color/line/column</pre>
    <pre>GET /api/color?line={line}&column={column}</pre>
</details>

Imaginons que l'on peut puisse jouer une partie.

Commenter la commande suivante, à quoi peut elle correspondre ?

```bash
curl \
    --header 'Content-Type: application/json' \
    -H "X-auth: W001066"\
    -X POST  \
    -d '{"line_from": 3, "col_from": 6, "line_to": 2,"col_to": 7}'\
    'http://chess-game/api/move/4'
```


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



