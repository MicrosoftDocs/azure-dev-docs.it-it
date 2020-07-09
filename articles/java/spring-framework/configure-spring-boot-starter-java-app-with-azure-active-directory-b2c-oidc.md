---
title: Usare Spring Boot Starter per Azure Active Directory B2C
description: Informazioni su come configurare un'app Spring Boot Initializer con l'utilità di avvio per Azure Active Directory B2C.
services: active-directory-b2c
documentationcenter: java
author: panli
manager: kevinzha
ms.author: edburns
ms.date: 06/04/2020
ms.service: active-directory-b2c
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: identity
ms.openlocfilehash: dfd5dc1e81a5d25f1bb08f373bafae7e3c2fe61e
ms.sourcegitcommit: e9accb9d82b5c633dffffd148974911398f2d096
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/06/2020
ms.locfileid: "86018632"
---
# <a name="tutorial-secure-a-java-web-app-using-the-spring-boot-starter-for-azure-active-directory-b2c"></a>Esercitazione: Proteggere un'app Web Java con Spring Boot Starter per Azure Active Directory B2C.

## <a name="overview"></a>Panoramica

Questo articolo illustra come creare un'app Java con [Spring Initializr](https://start.spring.io/), che usa Spring Boot Starter per Azure Active Directory (Azure AD).

In questa esercitazione verranno illustrate le procedure per:

> [!div class="checklist"]
> * Creare un'applicazione Java con Spring Initializr
> * Configurare Azure Active Directory B2C
> * Proteggere l'applicazione con le classi e le annotazioni Spring Boot
> * Compilare e testare l'applicazione Java

[Azure Active Directory](https://azure.microsoft.com/services/active-directory) è la soluzione aziendale di Microsoft per la gestione delle identità su scala cloud. [Azure Active Directory B2C](https://azure.microsoft.com/services/active-directory/external-identities/b2c/) integra il set di funzionalità dei Azure Active Directory, consentendo di gestire l'accesso di clienti, utenti e cittadini alle applicazioni B2C (business-to-consumer).

## <a name="prerequisites"></a>Prerequisiti

* Una sottoscrizione di Azure. Se non se ne ha già uno, creare un [account gratuito](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) prima di iniziare.
* Java Development Kit (JDK) supportato. Per altre informazioni sulle versioni di JDK utilizzabili per lo sviluppo in Azure, vedere <https://aka.ms/azure-jdks>.
* [Apache Maven](http://maven.apache.org/), versione 3.0 o versione successiva.

## <a name="create-an-app-using-spring-initializr"></a>Creare un'app con Spring Initializr

1. Passare a <https://start.spring.io/>.

2. Inserire i valori in base a queste istruzioni. Si noti che le etichette e il layout possono differire dall'immagine mostrata qui.

    * In **Progetto** selezionare **Progetto Maven**.
    * In **Linguaggio** selezionare **Java**.
    * In **Spring Boot**selezionare **2.2.7**.
    * In **Gruppo**, **Artefatto** e **Nome** immettere lo stesso valore, usando una stringa descrittiva breve. L'interfaccia utente potrebbe compilare automaticamente alcuni campi durante la digitazione.
    * Nel riquadro **Dipendenze** selezionare **Aggiungi dipendenze**. Usare l'interfaccia utente per aggiungere dipendenze da **Spring Web** e **Spring Security**.

   ![Immettere i valori per generare il progetto](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/si-n.png)

3. Selezionare **Genera progetto** e, quando richiesto, scaricare il progetto in un percorso nel computer locale. Spostare il file scaricato in una directory con lo stesso nome del progetto e decomprimerlo. Il layout del file avrà un aspetto simile al seguente, con il valore immesso per **Gruppo** al posto di `yourProject`.

    ```
    .
    ├── HELP.md
    ├── mvnw
    ├── mvnw.cmd
    ├── pom.xml
    └── src
        ├── main
        │   ├── java
        │   │   └── yourProject
        │   │       └── yourProject
        │   │           └── YourProjectApplication.java
        │   └── resources
        │       ├── application.properties
        │       ├── static
        │       └── templates
        └── test
            └── java
                └── yourProject
                    └── yourProject
                        └── YourProjectApplicationTests.java
    ```

## <a name="create-and-initialize-an-azure-active-directory-instance"></a>Creare e inizializzare un'istanza di Azure Active Directory

### <a name="create-the-active-directory-instance"></a>Creare l'istanza di Active Directory

1. Accedere a <https://portal.azure.com>.

2. Selezionare **Crea una risorsa**, quindi **Identità** e infine **Visualizza tutto**. Cercare **Azure Active Directory B2C**.

    ![Creare la nuova istanza di Azure Active Directory B2C](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/az-1-n.png)

3. Selezionare **Crea**.

    ![Ottenere il nome del tenant B2C](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/az-5-n.png)

4. Selezionare **Crea un nuovo tenant Azure AD B2C**.

    ![Creare la nuova istanza di Azure Active Directory](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/az-2-n.png)

5. Specificare i valori appropriati per **Nome organizzazione** e **Nome di dominio iniziale**, quindi selezionare **Crea**.

    ![Scegliere l'istanza di Azure Active Directory](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/az-3-n.png)

6. Dopo aver completato l'istanza di Active Directory, passare alla nuova directory. Oppure cercare `b2c` e selezionare **Azure AD B2C**.

    ![Individuare l'istanza di Azure Active Directory B2C](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/az-4-n.ng.png)

### <a name="add-an-application-registration-for-your-spring-boot-app"></a>Aggiungere una registrazione per l'app Spring Boot

1. Nel riquadro **Gestisci** a sinistra selezionare **Applicazioni** e quindi **Aggiungi**.

    ![Aggiungere una nuova registrazione per l'app](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/b2c1-n.png)

2. Nel campo **Nome** immettere il valore di **Gruppo** specificato in precedenza, quindi impostare il controllo **Includi app Web/API Web** su **Sì**.

3. Impostare **URL di risposta** su `http://localhost:8080/home`.

4. Mantenere i valori predefiniti per gli altri campi.

5. Selezionare **Crea**. Potrebbe essere necessario del tempo prima che l'applicazione venga visualizzata.

    ![Aggiungere l'URI di reindirizzamento dell'applicazione](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/b2c2-n.png)

6. Selezionare **Panoramica**, quindi **Applicazioni**.

7. Nella tabella delle applicazioni selezionare la riga con il nome del progetto.

8. Nel riquadro **Generale** selezionare Chiavi, quindi **Genera chiave**.

9. Impostare **Chiave dell'app** su `yourGroupIdkey`, sostituendo `yourGroupId` con il valore immesso prima per **Gruppo**.

10. Selezionare **Salva**. Attendere che la chiave venga visualizzata nella sezione Chiave dell'app, quindi copiarla per usarla più avanti in questo articolo.

    > [!NOTE]
    > Se si lascia la sezione **Chiavi** e si torna indietro, non sarà possibile visualizzare il valore della chiave. In tal caso, è necessario creare un'altra chiave e copiarla per un uso futuro.
    > In alcuni casi, la chiave generata può contenere caratteri problematici per l'inclusione nel file *application.yml*, ad esempio una barra rovesciata o un apice inverso. In tal caso, eliminare la chiave e generarne un'altra.

    ![Creare il segreto](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/b2c3-n.png)

11. Selezionare **Panoramica**.

12. Nella sezione **Criteri** del riquadro sinistro selezionare **Flussi utente** e quindi **Nuovo flusso utente**.

13. A questo punto si uscirà da questa esercitazione, se ne eseguirà un'altra e al termine si tornerà in questa. Ecco alcuni aspetti da tenere presenti quando si passa all'altra esercitazione.

    * Iniziare con il passaggio che richiede di selezionare **Nuovo flusso utente**.
    * Quando questa esercitazione fa riferimento a `webapp1`, usare invece il valore immesso per **Gruppo**.
    * Quando si selezionano le attestazioni da restituire dai flussi, assicurarsi che sia selezionata l'opzione **Nome visualizzato**. Senza questa attestazione, l'app creata in questa esercitazione non funzionerà.
    * Quando viene chiesto di eseguire i flussi utente, l'URL di reindirizzamento specificato in precedenza non è ancora attivo. È comunque possibile eseguire i flussi, ma il reindirizzamento non viene completato correttamente. Si tratta di un comportamento previsto.
    * Quando si raggiunge la sezione "Passaggi successivi", tornare in questa esercitazione.

    Eseguire tutti i passaggi indicati in [Esercitazione: Creare flussi utente in Azure Active Directory B2C](/azure/active-directory-b2c/tutorial-create-user-flows) per creare flussi utente per iscrizione e accesso, modifica del profilo e reimpostazione della password.

    AAD B2C supporta gli account locali e i provider di identità basati su social network. Per un esempio su come creare un provider di identità GitHub, vedere [Configurare l'iscrizione e l'accesso con un account GitHub tramite Azure Active Directory B2C](/azure/active-directory-b2c/identity-provider-github).

## <a name="configure-and-compile-your-app"></a>Configurare e compilare l'app

A questo punto, dopo aver creato l'istanza di AAD B2C e alcuni flussi utente, connettere l'app Spring all'istanza di AAD B2C.

1. Dalla riga di comando passare alla directory in cui è stato decompresso il file ZIP scaricato da Spring Initializr.

2. Passare alla cartella padre del progetto e aprire il file di progetto Maven *pom.xml* in un editor di testo.

3. Aggiungere le dipendenze per la sicurezza OAuth2 di Spring al file *pom.xml*:

    ```xml
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-active-directory-b2c-spring-boot-starter</artifactId>
        <version>See Below</version>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-thymeleaf</artifactId>
        <version>See Below</version>
    </dependency>
    <dependency>
        <groupId>org.thymeleaf.extras</groupId>
        <artifactId>thymeleaf-extras-springsecurity5</artifactId>
        <version>See Below</version>
    </dependency>
    ```

    Per `azure-active-directory-b2c-spring-boot-starter`, usare la versione più recente disponibile. Per cercarla, è possibile usare [mvnrepository.com](https://mvnrepository.com/ artifact/com.microsoft.azure/azure-active-directory-spring-boot-starter). Al momento della stesura di questo articolo, l'ultima versione è `2.2.4`.

    Per `spring-boot-starter-thymeleaf`, usare la versione corrispondente alla versione di Spring Boot selezionata in precedenza, ad esempio `2.2.7.RELASE`.

    Per `thymeleaf-extras-springsecurity5`, usare la versione più recente disponibile. Per cercarla, è possibile usare [mvnrepository.com](https://mvnrepository.com/artifact/org.thymeleaf.extras/thymeleaf-extras-springsecurity5). Al momento della stesura di questo articolo, l'ultima versione è `3.0.4.RELEASE`.

4. Salvare e chiudere il file *pom.xml*.

    * Verificare che le dipendenze siano corrette eseguendo `mvn -DskipTests clean install`. Se non viene visualizzato il messaggio `BUILD SUCCESS`, individuare e risolvere il problema prima di continuare.

5. Passare alla cartella *src/main/resources* del progetto e creare un file *application.yml* in un editor di testo.

6. Specificare le impostazioni per la registrazione dell'app usando i valori creati in precedenza, ad esempio:

    ```yaml
    azure:
      activedirectory:
        b2c:
          tenant: ejb0518domain
          client-id: ejb0518
          client-secret: '<yourAppKey>'
          reply-url: http://localhost:8080/home
          logout-success-url: http://localhost:8080/home
          user-flows:
            sign-up-or-sign-in: B2C_1_signupsignin1
            profile-edit: B2C_1_profileediting1
            password-reset: B2C_1_passwordreset1
    ```

    Si noti che il valore di `client-secret` è racchiuso tra virgolette singole. Ciò è necessario perché il valore di `<yourAppKey>` conterrà quasi certamente alcuni caratteri che devono essere racchiusi tra virgolette singole quando sono presenti in YAML.

    > [!NOTE]
    > Al momento della stesura di questo articolo, l'elenco completo dei valori di integrazione di Spring in Active Directory B2C disponibili per l'uso in *application.yml* è il seguente:
    >
    > ```
    > azure:
    >   activedirectory:
    >     b2c:
    >       tenant:
    >       oidc-enabled:
    >       client-id:
    >       client-secret:
    >       reply-url:  # should be absolute url.
    >       logout-success-url:
    >       user-flows:
    >         sign-up-or-sign-in:
    >         profile-edit: # optional
    >         password-reset: # optional
    > ```
    >
    > Il file *application.yml* è disponibile nell'[esempio di Spring Boot in Azure Active Directory B2C](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/spring/azure-spring-boot-samples/azure-spring-boot-sample-active-directory-b2c-oidc/src/main/resources/application.yml) in GitHub.

7. Salvare e chiudere il file *application.yml*.

8. Creare una cartella denominata *controller* in *src/main/Java/<yourGroupId>/<yourGroupId>* , sostituendo `<yourGroupId>` con il valore immesso per **Gruppo**.

9. Creare un nuovo file Java denominato *WebController.java* nella cartella *controller* e aprirlo in un editor di testo.

10. Immettere il codice seguente, sostituendo `yourGroupId` nel modo appropriato, quindi salvare e chiudere il file:

    ```java
    package yourGroupId.yourGroupId.controller;

    import org.springframework.security.oauth2.client.authentication.OAuth2AuthenticationToken;
    import org.springframework.security.oauth2.core.user.OAuth2User;
    import org.springframework.stereotype.Controller;
    import org.springframework.ui.Model;
    import org.springframework.web.bind.annotation.GetMapping;

    @Controller
    public class WebController {

        private void initializeModel(Model model, OAuth2AuthenticationToken token) {
            if (token != null) {
                final OAuth2User user = token.getPrincipal();

                model.addAttribute("grant_type", user.getAuthorities());
                model.addAllAttributes(user.getAttributes());
            }
        }

        @GetMapping(value = "/")
        public String index(Model model, OAuth2AuthenticationToken token) {
            initializeModel(model, token);

            return "home";
        }

        @GetMapping(value = "/greeting")
        public String greeting(Model model, OAuth2AuthenticationToken token) {
            initializeModel(model, token);

            return "greeting";
        }

        @GetMapping(value = "/home")
        public String home(Model model, OAuth2AuthenticationToken token) {
            initializeModel(model, token);

            return "home";
        }
    }
    ```

    Poiché ogni metodo nel controller chiama `initializeModel()` e tale metodo chiama `model.addAllAttributes(user.getAttributes());`, qualsiasi pagina HTML in *src/main/resources/templates* è in grado di accedere a uno di questi attributi, ad esempio `${name}`, `${grant_type}` o `${auth_time}`. I valori restituiti da `user.getAttributes()` sono in effetti le attestazioni di `id_token` per l'autenticazione. L'elenco completo delle attestazioni disponibili è elencato in [Token ID di Microsoft Identity Platform](/azure/active-directory/develop/id-tokens#payload-claims).

11. Creare una cartella denominata *security* in *src/main/Java/<yourGroupId>/<yourGroupId>* , sostituendo `yourGroupId` con il valore immesso per **Gruppo**.

12. Creare un nuovo file Java denominato *WebSecurityConfiguration.java* nella cartella *security* e aprirlo in un editor di testo.

13. Immettere il codice seguente, sostituendo `yourGroupId` nel modo appropriato, quindi salvare e chiudere il file:

    ```java
    package yourGroupId.yourGroupId.security;

    import com.microsoft.azure.spring.autoconfigure.b2c.AADB2COidcLoginConfigurer;
    import org.springframework.security.config.annotation.web.builders.HttpSecurity;
    import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
    import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;

    @EnableWebSecurity
    public class WebSecurityConfiguration extends WebSecurityConfigurerAdapter {

        private final AADB2COidcLoginConfigurer configurer;

        public WebSecurityConfiguration(AADB2COidcLoginConfigurer configurer) {
            this.configurer = configurer;
        }

        @Override
        protected void configure(HttpSecurity http) throws Exception {
            http
                    .authorizeRequests()
                    .anyRequest()
                    .authenticated()
                    .and()
                    .apply(configurer)
            ;
        }
    }
    ```

14. Copiare i file *greeting.html* e *home.html* dall'[esempio di Spring Boot in Azure AD B2C](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/spring/azure-spring-boot-samples/azure-spring-boot-sample-active-directory-b2c-oidc/src/main/resources/templates) in *src/main/Resources/templates* e sostituire `${your-profile-edit-user-flow}` e `${your-password-reset-user-flow}` con i nomi dei flussi utente creati in precedenza.

## <a name="build-and-test-your-app"></a>Compilare e testare l'app

1. Aprire un prompt dei comandi e cambiare la directory passando alla cartella in cui si trova il file *pom.xml* dell'app.

2. Compilare l'applicazione Spring Boot con Maven ed eseguirla, ad esempio:

    > [!NOTE]
    > È estremamente importante che l'orario secondo l'orologio di sistema in cui viene eseguita l'app Spring Boot locale sia accurato. Quando si usa OAuth 2.0, la tolleranza per lo sfasamento di orario è molto ridotta. Anche tre minuti di inesattezza possono causare l'esito negativo dell'accesso con un errore simile a `[invalid_id_token] An error occurred while attempting to decode the Jwt: Jwt used before 2020-05-19T18:52:10Z`. Al momento della stesura di questo articolo, [time.gov](https://time.gov/) include un indicatore della differenza tra il proprio orologio e l'orario effettivo. L'app è stata eseguita correttamente con uno sfasamento di + 0,019 secondi.

    ```shell
    mvn -DskipTests clean package
    mvn -DskipTests spring-boot:run
    ```

3. Al termine della compilazione e dell'avvio dell'applicazione tramite Maven, aprire `http://localhost:8080/` in un Web browser. Si dovrebbe essere reindirizzati alla pagina di accesso.

    ![Pagina di accesso](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/lo1-n.png)

4. Selezionare il collegamento con il testo relativo all'accesso. Si dovrebbe essere reindirizzati in Azure AD B2C per avviare il processo di autenticazione.

5. Al termine dell'accesso, dovrebbe essere visualizzato l'esempio di `home page` nel browser.

    ![Accesso riuscito](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/lo3-n.png)

## <a name="summary"></a>Summary

In questa esercitazione si è creata una nuova applicazione Web Java con l'utilità di avvio per Azure Active Directory B2C, si è configurato un nuovo tenant di Azure AD B2C registrandovi quindi una nuova applicazione e infine si è configurata l'applicazione in modo da usare le annotazioni e le classi Spring per proteggere l'app Web.

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni su Spring e Azure, passare al centro di documentazione di Spring in Azure.

> [!div class="nextstepaction"]
> [Spring in Azure](/azure/developer/java/spring-framework)
