---
title: Come usare l'utilità di avvio Spring Boot per Azure Active Directory
description: Informazioni su come configurare un'app Spring Boot Initializer con l'utilità di avvio Azure Active Directory.
services: active-directory
documentationcenter: java
ms.date: 03/05/2020
ms.service: active-directory
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: identity
ms.custom: devx-track-java
ms.openlocfilehash: 2714d4d4b8a614bcdbf951eb2a9dc4c2dc78dda2
ms.sourcegitcommit: 44016b81a15b1625c464e6a7b2bfb55938df20b6
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/14/2020
ms.locfileid: "86379425"
---
# <a name="tutorial-secure-a-java-web-app-using-the-spring-boot-starter-for-azure-active-directory"></a>Esercitazione: Proteggere un'app Web Java con l'utilità di avvio Spring Boot per Azure Active Directory

## <a name="overview"></a>Panoramica

Questo articolo illustra la creazione di un'app Java con **[Spring Initializr]** , che usa l'utilità di avvio Spring Boot per Azure Active Directory (Azure AD).

In questa esercitazione verranno illustrate le procedure per:

 * Creare un'applicazione Java con Spring Initializr
 * Configurare Azure Active Directory
 * Proteggere l'applicazione con le classi e le annotazioni Spring Boot
 * Compilare e testare l'applicazione Java

Se non si ha una sottoscrizione di Azure, creare un [account gratuito](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) prima di iniziare.

## <a name="prerequisites"></a>Prerequisiti

I prerequisiti seguenti sono necessari per completare le procedure disponibili in questo articolo:

* Java Development Kit (JDK) supportato. Per altre informazioni sulle versioni di JDK utilizzabili per lo sviluppo in Azure, vedere <https://aka.ms/azure-jdks>.
* [Apache Maven](http://maven.apache.org/), versione 3.0 o versione successiva.

## <a name="create-an-app-using-spring-initializr"></a>Creare un'app con Spring Initializr

1. Passare a <https://start.spring.io/>.

1. Specificare che si vuole generare un progetto **Maven** con **Java**, quindi immettere i nomi in **Group** (Gruppo) e **Artifact** (Artefatto) per l'applicazione.

   ![Specificare i nomi del gruppo e dell'artefatto][create-spring-app-01]

1. Scorrere verso il basso e aggiungere **Dipendenze** per **Spring Web**, **Azure Active Directory** e **Spring Security**.

1. Nella parte inferiore della pagina fare clic sul collegamento **Genera**.

   ![Selezionare le utilità di avvio Security (Sicurezza), Web e Azure Active Directory][create-spring-app-02]

1. Quando richiesto, scaricare il progetto in un percorso nel computer locale.

## <a name="create-azure-active-directory-instance"></a>Creare l'istanza di Azure Active Directory

### <a name="create-the-active-directory-instance"></a>Creare l'istanza di Active Directory

1. Accedere a <https://portal.azure.com>.

1. Fare clic su **+Crea una risorsa**, quindi su **Identità** e infine su **Azure Active Directory**.

   ![Creare la nuova istanza di Azure Active Directory][create-directory-01]

1. Immettere il **Nome organizzazione** e il **Nome di dominio iniziale**. Copiare l'URL completo della directory. L'URL verrà usato per aggiungere account utente più avanti in questa esercitazione, ad esempio: `wingtiptoysdirectory.onmicrosoft.com`. 

    Copiare l'URL completo della directory. L'URL verrà usato per aggiungere account utente più avanti in questa esercitazione, Ad esempio: wingtiptoysdirectory.onmicrosoft.com.

    Al termine, fare clic su **Crea**. La creazione della nuova risorsa richiederà alcuni minuti.

   ![Specificare i nomi di Azure Active Directory][create-directory-02]

1. Al termine, fare clic per accedere alla nuova directory.

   ![Selezionare il nome dell'account Azure][create-directory-03]

1. Copiare l'**ID tenant** perché questo valore verrà usato per configurare il file *application.properties* più avanti in questa esercitazione.

   ![Copiare l'ID tenant][create-directory-05]

### <a name="add-an-application-registration-for-your-spring-boot-app"></a>Aggiungere una registrazione per l'app Spring Boot

1. Scegliere **Registrazioni per l'app** dal menu del portale e fare clic su **Registra un'applicazione**.

   ![Aggiungere una nuova registrazione per l'app][create-app-registration-01]

1. Selezionare l'applicazione e quindi fare clic su **Registra**.

   ![Creare una nuova registrazione di app][create-app-registration-02]

1. Quando viene visualizzata la pagina per la registrazione dell'app, copiare i valori di **ID applicazione** e **ID tenant**, perché verranno usati per configurare il file *application.properties* più avanti in questa esercitazione.

   ![Creare le chiavi della registrazione per l'app][create-app-registration-03]

1. Fare clic su **Certificati e segreti** nel riquadro di spostamento sinistro.  Quindi fare clic su **Nuovo segreto client**.

   ![Creare le chiavi della registrazione per l'app][create-app-registration-03-5]

1. Aggiungere una **Descrizione** e selezionare durata nell'elenco **Scade**.  Scegliere **Aggiungi**. Il valore della chiave verrà compilato automaticamente.

   ![Specificare i parametri della chiave di registrazione per l'app][create-app-registration-04]

1. Copiare e salvare il valore del segreto client per configurare il file *application.properties* più avanti in questa esercitazione. Non sarà possibile recuperare questo valore in un secondo momento.

   ![Specificare i parametri della chiave di registrazione per l'app][create-app-registration-04-5]

1. Fare clic su **Autorizzazioni API** nel riquadro di spostamento sinistro. 

1. Fare clic su **Microsoft Graph**, quindi selezionare **Accede alla directory come utente registrato** e **Accedi e leggi il profilo di un altro utente**. Fare clic su **Concedi autorizzazioni** e quindi su **Sì** quando richiesto.

   ![Concedere le autorizzazioni di accesso][create-app-registration-08]

1. Nella pagina principale della registrazione dell'app fare clic su **Autenticazione** e quindi su **Aggiungi una piattaforma**.  Fare quindi clic su **Applicazioni Web**.

    ![Modificare gli URL di risposta][create-app-registration-09]

1. Immettere <http:<span></span>//localhost:8080/login/oauth2/code/azure> come nuovo **URI di reindirizzamento**, quindi fare clic su **Configura**.

    ![Aggiungere un nuovo URL di risposta][create-app-registration-10]

1. Nella pagina principale per la registrazione dell'app fare clic su **Manifesto**, quindi impostare il valore del parametro `oauth2AllowImplicitFlow` su `true` e fare clic su **Salva**.

    ![Configurare il manifesto dell'app][create-app-registration-11]

    > [!NOTE]
    > Per altre informazioni sul parametro `oauth2AllowImplicitFlow` e su altre impostazioni dell'applicazione, vedere [Manifesto dell'applicazione Azure Active Directory][AAD app manifest].

### <a name="add-a-user-account-to-your-directory-and-add-that-account-to-a-group"></a>Aggiungere un account utente alla directory e quindi aggiungere tale account al gruppo

1. Nella pagina **Panoramica** di Active Directory fare clic su **Tutti gli utenti** e quindi su **Nuovo utente**.

   ![Aggiungere un nuovo account utente][create-user-01]

1. Quando viene visualizzato il pannello **Utente**, immettere i valori per **Nome utente** e **Nome**.  Fare quindi clic su **Crea**.

   ![Immettere le informazioni sull'account utente][create-user-02]

   > [!NOTE]
   > È necessario specificare l'URL della directory salvato in precedenza in questa esercitazione quando si immette il nome utente. Ad esempio:
   >
   > `wingtipuser@wingtiptoysdirectory.onmicrosoft.com`

1. Fare clic su **Gruppi** quindi su **Crea un nuovo gruppo** per creare un gruppo da usare per l'autorizzazione nell'applicazione.

1. Quindi fare clic su **Nessun membro selezionato**. Ai fini di questa esercitazione verrà creato un gruppo denominato *users*.  Cercare l'utente creato nel passaggio precedente.  Fare clic su **Seleziona** per aggiungere l'utente al gruppo.  Quindi fare clic su **Crea** per creare il nuovo gruppo.

   ![Selezionare l'utente per il gruppo][create-user-03]

1. Tornare nel pannello **Utenti**, selezionare l'utente di test, fare clic su **Reimposta password**, quindi copiare la password, che verrà usata per l'accesso all'applicazione più avanti in questa esercitazione.

   ![Mostrare la password][create-user-04]

## <a name="configure-and-compile-your-app"></a>Configurare e compilare l'app

1. Estrarre i file da dall'archivio di progetti creato e scaricato in precedenza in questa esercitazione in una directory.

1. Passare alla cartella padre per il progetto e aprire il file di progetto Maven `pom.xml` in un editor di testo.

1. Aggiungere le dipendenze per la sicurezza OAuth2 di Spring a `pom.xml`:

   ```xml
   <dependency>
      <groupId>org.springframework.security</groupId>
      <artifactId>spring-security-oauth2-client</artifactId>
   </dependency>
   <dependency>
      <groupId>org.springframework.security</groupId>
      <artifactId>spring-security-oauth2-jose</artifactId>
   </dependency>
   ```

1. Salvare e chiudere il file *pom.xml*.

1. Passare alla cartella *src/main/resources* nel progetto e aprire il file *application.properties* in un editor di testo.

1. Specificare le impostazioni per la registrazione dell'app usando i valori creati in precedenza, ad esempio:

   ```yaml
   # Specifies your Active Directory ID:
   azure.activedirectory.tenant-id=22222222-2222-2222-2222-222222222222

   # Specifies your App Registration's Application ID:
   spring.security.oauth2.client.registration.azure.client-id=11111111-1111-1111-1111-1111111111111111

   # Specifies your App Registration's secret key:
   spring.security.oauth2.client.registration.azure.client-secret=AbCdEfGhIjKlMnOpQrStUvWxYz==

   # Specifies the list of Active Directory groups to use for authorization:
   azure.activedirectory.active-directory-groups=Users
   ```

   Dove:

   | Parametro | Descrizione |
   |---|---|
   | `azure.activedirectory.tenant-id` | Contiene il valore di **ID directory** di Active Directory copiato in precedenza. |
   | `spring.security.oauth2.client.registration.azure.client-id` | Contiene il valore di **ID applicazione** della registrazione dell'app completata in precedenza. |
   | `spring.security.oauth2.client.registration.azure.client-secret` | Contiene il **valore** della chiave di registrazione dell'app completata in precedenza. |
   | `azure.activedirectory.active-directory-groups` | Contiene un elenco di gruppi di Active Directory da usare per l'autorizzazione. |

   > [!NOTE]
   > Per un elenco completo dei valori disponibili nel file *application.properties*, vedere l'[esempio Spring Boot per Azure Active Directory][AAD Spring Boot Sample] in GitHub.

1. Salvare e chiudere il file *application.properties*.

1. Creare una cartella denominata *controller* nella cartella di origine Java per l'applicazione, ad esempio *src/main/java/com/wingtiptoys/security/controller*.

1. Creare un nuovo file Java denominato *HelloController.java* nella cartella *controller* e aprirlo in un editor di testo.

1. Immettere il codice seguente, quindi salvare e chiudere il file:

   ```java
   package com.wingtiptoys.security;

   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.bind.annotation.RestController;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.security.access.prepost.PreAuthorize;
   import org.springframework.security.oauth2.client.OAuth2AuthorizedClient;
   import org.springframework.security.oauth2.client.authentication.OAuth2AuthenticationToken;
   import org.springframework.ui.Model;

   @RestController
   public class HelloController {
      @Autowired
      @PreAuthorize("hasRole('Users')")
      @RequestMapping("/")
      public String helloWorld() {
         return "Hello World!";
      }
   }
   ```

   > [!NOTE]
   > Il nome di gruppo specificato nel metodo `@PreAuthorize("hasRole('')")` deve contenere uno dei gruppi specificati nel campo `azure.activedirectory.active-directory-groups` del file *application.properties*.
   >
   > È anche possibile specificare impostazioni di autorizzazione diverse per mapping di richieste diversi, ad esempio:
   >
   > ``` java
   > public class HelloController {
   >    @Autowired
   >    @PreAuthorize("hasRole('Users')")
   >    @RequestMapping("/")
   >    public String helloWorld() {
   >       return "Hello Users!";
   >    }
   >    @PreAuthorize("hasRole('Group1')")
   >    @RequestMapping("/Group1")
   >    public String groupOne() {
   >       return "Hello Group 1 Users!";
   >    }
   >    @PreAuthorize("hasRole('Group2')")
   >    @RequestMapping("/Group2")
   >    public String groupTwo() {
   >       return "Hello Group 2 Users!";
   >    }
   > }
   > ```

1. Creare una cartella denominata *security* nella cartella di origine Java per l'applicazione, ad esempio *src/main/java/com/wingtiptoys/security/security*.

1. Creare un nuovo file Java denominato *WebSecurityConfig.java* nella cartella *security* e aprirlo in un editor di testo.

1. Immettere il codice seguente, quindi salvare e chiudere il file:

    ```java
    package com.wingtiptoys.security;

    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.security.config.annotation.method.configuration.EnableGlobalMethodSecurity;
    import org.springframework.security.config.annotation.web.builders.HttpSecurity;
    import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
    import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
    import org.springframework.security.oauth2.client.oidc.userinfo.OidcUserRequest;
    import org.springframework.security.oauth2.client.userinfo.OAuth2UserService;
    import org.springframework.security.oauth2.core.oidc.user.OidcUser;

    @EnableWebSecurity
    @EnableGlobalMethodSecurity(prePostEnabled = true)
    public class WebSecurityConfig extends WebSecurityConfigurerAdapter {
        @Autowired
        private OAuth2UserService<OidcUserRequest, OidcUser> oidcUserService;

        @Override
        protected void configure(HttpSecurity http) throws Exception {
            http
                .authorizeRequests()
                .anyRequest().authenticated()
                .and()
                .oauth2Login()
                .userInfoEndpoint()
                .oidcUserService(oidcUserService);
        }
    }
    ```

## <a name="build-and-test-your-app"></a>Compilare e testare l'app

1. Aprire un prompt dei comandi e cambiare la directory passando alla cartella in cui si trova il file *pom.xml* dell'app.

1. Compilare l'applicazione Spring Boot con Maven ed eseguirla, ad esempio:

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

   ![Compilare l'applicazione][build-application]

1. Al termine della compilazione e dell'avvio dell'applicazione tramite Maven, aprire <span></span>//localhost:8080 in un Web browser. Verranno richiesti un nome utente e una password.

   ![Accesso all'applicazione][application-login]

   > [!NOTE]
   > È possibile che venga richiesta la modifica della password se questo è il primo accesso per un nuovo account utente.
   >
   > ![Modifica della password][update-password]

1. Al termine dell'accesso, verrà visualizzato il testo "Hello World" di esempio dal controller.

   ![Accesso riuscito][hello-world]

   > [!NOTE]
   > Agli account utente non autorizzati verrà visualizzato il messaggio **HTTP 403** di accesso non autorizzato.

## <a name="summary"></a>Summary

In questa esercitazione si è creata una nuova applicazione Web Java con l'utilità di avvio per Azure Active Directory, si è configurato un nuovo tenant di Azure AD registrandovi quindi una nuova applicazione e infine si è configurata l'applicazione in modo da usare le annotazioni e le classi Spring per proteggere l'app Web.

## <a name="see-also"></a>Vedere anche
* Per informazioni sulle nuove opzioni dell'interfaccia utente, vedere [Guida di formazione per la registrazione di app nel portale di Azure](/azure/active-directory/develop/app-registrations-training-guide-for-app-registrations-legacy-users)

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni su Spring e Azure, passare al centro di documentazione di Spring in Azure.

> [!div class="nextstepaction"]
> [Spring in Azure](/azure/developer/java/spring-framework)

<!-- URL List -->

[Azure Active Directory Documentation]: /azure/active-directory/
[AAD app manifest]: /azure/active-directory/develop/active-directory-application-manifest
[Get started with Azure AD]: /azure/active-directory/get-started-azure-ad
[Azure for Java Developers]: /azure/developer/java/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[Working with Azure DevOps and Java]: /azure/devops/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/
[AAD Spring Boot Sample]: https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/spring/azure-spring-boot-samples/azure-spring-boot-sample-active-directory-backend

<!-- IMG List -->

[create-spring-app-01]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-spring-app-01.png
[create-spring-app-02]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-spring-app-02.png
[create-spring-app-03]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-spring-app-03.png

[create-directory-01]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-directory-01.png
[create-directory-02]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-directory-02.png
[create-directory-03]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-directory-03.png
[create-directory-04]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-directory-04.png
[create-directory-05]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-directory-05.png

[create-app-registration-01]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-01.png
[create-app-registration-02]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-02.png
[create-app-registration-03]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-03.png
[create-app-registration-03-5]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-03-5.png
[create-app-registration-04]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-04.png
[create-app-registration-04-5]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-04-5.png
[create-app-registration-05]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-05.png
[create-app-registration-06]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-06.png
[create-app-registration-07]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-07.png
[create-app-registration-08]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-08.png
[create-app-registration-09]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-09.png
[create-app-registration-10]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-10.png
[create-app-registration-11]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-11.png

[create-user-01]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-user-01.png
[create-user-02]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-user-02.png
[create-user-03]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-user-03.png
[create-user-04]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-user-04.png

[application-login]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/application-login.png
[build-application]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/build-application.png
[hello-world]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/hello-world.png
[update-password]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/update-password.png


