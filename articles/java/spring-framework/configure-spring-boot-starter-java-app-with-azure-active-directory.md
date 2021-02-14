---
title: Come usare l'utilità di avvio Spring Boot per Azure Active Directory
description: Informazioni su come configurare un'app Spring Boot Initializer con l'utilità di avvio Azure Active Directory.
services: active-directory
documentationcenter: java
ms.date: 10/14/2020
ms.service: active-directory
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: identity
ms.custom: devx-track-java
adobe-target: true
ms.openlocfilehash: d98355c37767c3d39de5cfc1f8b47030ad4f53be
ms.sourcegitcommit: b380f6e637b47e6e3822b364136853e1d342d5cd
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/14/2021
ms.locfileid: "100395306"
---
# <a name="tutorial-secure-a-java-web-app-using-the-spring-boot-starter-for-azure-active-directory"></a>Esercitazione: Proteggere un'app Web Java con l'utilità di avvio Spring Boot per Azure Active Directory

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
1. Aggiungere **dipendenze** per il client **Spring Web**, **Azure Active Directory** e **OAuth2**.
1. Nella parte inferiore della pagina selezionare il pulsante **GENERA**.
   
   >[!div class="mx-imgBorder"]
   >![Specificare i nomi del gruppo e dell'artefatto, selezionare dipendenze][create-spring-app-01]


1. Quando richiesto, scaricare il progetto in un percorso nel computer locale.

## <a name="create-azure-active-directory-instance"></a>Creare l'istanza di Azure Active Directory

### <a name="create-the-active-directory-instance"></a>Creare l'istanza di Active Directory

1. Accedere a <https://portal.azure.com>.

1. Selezionare **Crea una risorsa**, quindi **Identità** e infine **Azure Active Directory**.
   
   >[!div class="mx-imgBorder"]
   >![Crea nuovo Azure Active Directory instance_step1][create-directory-00]

   >[!div class="mx-imgBorder"]
   >![Crea nuovo Azure Active Directory instance_step2][create-directory-01]

1. Immettere il **Nome organizzazione** e il **Nome di dominio iniziale**. Copiare l'URL completo della directory. L'URL verrà usato per aggiungere account utente più avanti in questa esercitazione,
 ad esempio: `azuresampledirectory.onmicrosoft.com`. 

    Copiare l'URL completo della directory. L'URL verrà usato per aggiungere account utente più avanti in questa esercitazione, Ad esempio: azuresampledirectory.onmicrosoft.com.

    Al termine, selezionare **Crea**. La creazione della nuova risorsa richiederà alcuni minuti.
   
   >[!div class="mx-imgBorder"]
   >![Specificare i nomi di Azure Active Directory][create-directory-02]

1. Al termine, selezionare per accedere alla nuova directory.
   
   >[!div class="mx-imgBorder"]
   >![Selezionare il nome dell'account Azure][create-directory-03]

1. Copiare l'**ID tenant** perché questo valore verrà usato per configurare il file *application.properties* più avanti in questa esercitazione.
   
   >[!div class="mx-imgBorder"]
   >![Copiare l'ID tenant][create-directory-04]

### <a name="add-an-application-registration-for-your-spring-boot-app"></a>Aggiungere una registrazione per l'app Spring Boot

1. Selezionare **Registrazioni app** dal menu del portale e quindi selezionare **Registra un'applicazione**.
   
   >[!div class="mx-imgBorder"]
   >![Aggiungere una nuova registrazione per l'app][create-app-registration-01]

1. Selezionare l'applicazione e quindi **Registra**.
   
   >[!div class="mx-imgBorder"]
   >![Creare una nuova registrazione di app][create-app-registration-02]

1. Quando viene visualizzata la pagina per la registrazione dell'app, copiare i valori di **ID applicazione** e **ID tenant**, perché verranno usati per configurare il file *application.properties* più avanti in questa esercitazione.
   
   >[!div class="mx-imgBorder"]
   >![Copiare le chiavi della registrazione per l'app][create-app-registration-03]

1. Fare clic su **Certificati e segreti** nel riquadro di spostamento sinistro.  Selezionare infine **Nuovo segreto client**.
   
   >[!div class="mx-imgBorder"]
   >![Creare le chiavi della registrazione per l'app][create-app-registration-03-5]

1. Aggiungere una **Descrizione** e selezionare durata nell'elenco **Scade**.  Scegliere **Aggiungi**. Il valore della chiave verrà compilato automaticamente.
   
   >[!div class="mx-imgBorder"]
   >![Specificare i parametri della chiave di registrazione per l'app][create-app-registration-04]

1. Copiare e salvare il valore del segreto client per configurare il file *application.properties* più avanti in questa esercitazione. Non sarà possibile recuperare questo valore in un secondo momento.
   
   >[!div class="mx-imgBorder"]
   >![Copiare il valore della chiave di registrazione dell'app][create-app-registration-04-5]

1. Fare clic su **Autorizzazioni API** nel riquadro di spostamento sinistro. 

1. Fare clic su **Microsoft Graph**, quindi selezionare **Accede alla directory come utente registrato** e **Accedi e leggi il profilo di un altro utente**. Fare clic su **Concedi autorizzazioni** e quindi su **Sì** quando richiesto.
   
   >[!div class="mx-imgBorder"]
   >![Aggiungere le autorizzazioni di accesso][create-app-registration-08]
   
1. Fare clic su **Concedi consenso amministratore per l'esempio di Azure** e selezionare **Sì**.
   
   >[!div class="mx-imgBorder"]
   >![Concedere le autorizzazioni di accesso][create-app-registration-05]

1. Nella pagina principale della registrazione dell'app selezionare **Autenticazione** e quindi **Aggiungi una piattaforma**.  Selezionare quindi **Applicazioni Web**.
   
   >[!div class="mx-imgBorder"]
   >![Modificare gli URL di risposta][create-app-registration-09]

1. Immettere *http://localhost:8080/login/oauth2/code/azure* come nuovo **URI di reindirizzamento** e quindi selezionare **Configura**.
   
   >[!div class="mx-imgBorder"]
   >![Aggiungere un nuovo URL di risposta][create-app-registration-10]

### <a name="add-a-user-account-to-your-directory-and-add-that-account-to-a-group"></a>Aggiungere un account utente alla directory e quindi aggiungere tale account al gruppo

1. Nella pagina **Panoramica** di Active Directory selezionare **Utenti** e quindi **Nuovo utente**.
   
   >[!div class="mx-imgBorder"]
   >![Aggiungere un nuovo account utente][create-user-01]

1. Quando viene visualizzato il pannello **Utente**, immettere i valori per **Nome utente** e **Nome**.  Selezionare quindi **Crea**.
   
   >[!div class="mx-imgBorder"]
   >![Immettere le informazioni sull'account utente][create-user-02]

   > [!NOTE]
   > È necessario specificare l'URL della directory salvato in precedenza in questa esercitazione quando si immette il nome utente. Ad esempio:
   >
   > `test-user@azuresampledirectory.onmicrosoft.com`

1. Nella pagina **Panoramica** di Active Directory selezionare **Gruppi**, quindi **Nuovo gruppo** per creare un gruppo da usare per l'autorizzazione nell'applicazione.

1. Selezionare **Nessun membro selezionato**. Ai fini di questa esercitazione verrà creato un gruppo denominato *Group1*.  Cercare l'utente creato nel passaggio precedente.  Scegliere **Seleziona** per aggiungere l'utente al gruppo.  Quindi selezionare **Crea** per creare il nuovo gruppo.

   >[!div class="mx-imgBorder"]
   >![Selezionare l'utente per il gruppo][create-user-03]

1. Tornare nel pannello **Utenti**, selezionare l'utente di test, selezionare **Reimposta password**, quindi copiare la password, che verrà usata per l'accesso all'applicazione più avanti in questa esercitazione.

   >[!div class="mx-imgBorder"]
   >![Mostrare la password][create-user-04]

## <a name="configure-and-compile-your-app"></a>Configurare e compilare l'app

1. Estrarre i file da dall'archivio di progetti creato e scaricato in precedenza in questa esercitazione in una directory.

1. Passare alla cartella *src/main/Resources* nel progetto, quindi aprire il file *Application. Properties* in un editor di testo.

1. Specificare le impostazioni per la registrazione dell'app usando i valori creati in precedenza, ad esempio:

   ```properties
   # Specifies your Active Directory ID:
   azure.activedirectory.tenant-id=22222222-2222-2222-2222-222222222222
   # Specifies your App Registration's Application ID:
   azure.activedirectory.client-id=11111111-1111-1111-1111-1111111111111111
   # Specifies your App Registration's secret key:
   azure.activedirectory.client-secret=AbCdEfGhIjKlMnOpQrStUvWxYz==
   # Specifies the list of Active Directory groups to use for authorization:
   azure.activedirectory.user-group.allowed-groups=group1
   ```

   Dove:

   | Parametro | Descrizione |
   |---|---|
   | `azure.activedirectory.tenant-id` | Contiene il valore di **ID directory** di Active Directory copiato in precedenza. |
   | `azure.activedirectory.client-id` | Contiene il valore di **ID applicazione** della registrazione dell'app completata in precedenza. |
   | `azure.activedirectory.client-secret` | Contiene il **valore** della chiave di registrazione dell'app completata in precedenza. |
   | `azure.activedirectory.user-group.allowed-groups` | Contiene un elenco di gruppi di Active Directory da usare per l'autorizzazione. |

   > [!NOTE]
   > Per un elenco completo dei valori disponibili nel file *application.properties*, vedere l'[esempio Spring Boot per Azure Active Directory][AAD Spring Boot Sample] in GitHub.

1. Salvare e chiudere il file *application.properties*.

1. Creare una cartella denominata *controller* nella cartella di origine Java per l'applicazione, ad esempio *src/main/java/com/wingtiptoys/security/controller*.

1. Creare un nuovo file Java denominato *HelloController.java* nella cartella *controller* e aprirlo in un editor di testo.

1. Immettere il codice seguente, quindi salvare e chiudere il file:

   ```java
   package com.wingtiptoys.security;

   import org.springframework.web.bind.annotation.GetMapping;
   import org.springframework.web.bind.annotation.ResponseBody;
   import org.springframework.web.bind.annotation.RestController;
   import org.springframework.security.access.prepost.PreAuthorize;
   
   @RestController
   public class HelloController {
   
      @GetMapping("group1")
      @ResponseBody
      @PreAuthorize("hasRole('ROLE_group1')")
      public String group1() {
         return "Hello Group 1 Users!";
      }
    
      @GetMapping("group2")
      @ResponseBody
      @PreAuthorize("hasRole('ROLE_group2')")
      public String group2() {
         return "Hello Group 2 Users!";
      }
   }
   ```

   > [!NOTE]
   > Il nome di gruppo specificato nel metodo `@PreAuthorize("hasRole('')")` deve contenere uno dei gruppi specificati nel campo `azure.activedirectory.user-group.allowed-groups` del file *application.properties*.
   >
   > È anche possibile specificare impostazioni di autorizzazione diverse per mapping di richieste diversi, ad esempio:
   >
   > ``` java
   > public class HelloController {
   >
   >    @PreAuthorize("hasRole('ROLE_Users')")
   >    @RequestMapping("/")
   >    public String helloWorld() {
   >       return "Hello Users!";
   >    }
   >    @PreAuthorize("hasRole('ROLE_group1')")
   >    @RequestMapping("/Group1")
   >    public String groupOne() {
   >       return "Hello Group 1 Users!";
   >    }
   >    @PreAuthorize("hasRole('ROLE_group2')")
   >    @RequestMapping("/Group2")
   >    public String groupTwo() {
   >       return "Hello Group 2 Users!";
   >    }
   > }
   > ```

1. Aprire la classe dell'applicazione in un editor di testo.

1. Aggiungere `@EnableWebSecurity` e `@EnableGlobalMethodSecurity(prePostEnabled = true)` nella classe dell'applicazione come illustrato nell'esempio seguente, quindi salvare e chiudere il file:

    ```java
    package com.wingtiptoys;

    import org.springframework.boot.SpringApplication;
    import org.springframework.boot.autoconfigure.SpringBootApplication;
    import org.springframework.security.config.annotation.method.configuration.EnableGlobalMethodSecurity;
    import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
    
    @EnableWebSecurity
    @EnableGlobalMethodSecurity(prePostEnabled = true)
    @SpringBootApplication
    public class SpringBootSampleActiveDirectoryApplication {   
        public static void main(String[] args) {
            SpringApplication.run(SpringBootSampleActiveDirectoryApplication.class, args);
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
   
   >[!div class="mx-imgBorder"]
   >![Compilare l'applicazione][build-application]

1. Al termine della compilazione e dell'avvio dell'applicazione tramite Maven, aprire `http://localhost:8080/group1` in un Web browser. Verranno richiesti un nome utente e una password.
   
   >[!div class="mx-imgBorder"]
   ![Accesso all'applicazione][application-login]

   > [!NOTE]
   > È possibile che venga richiesta la modifica della password se questo è il primo accesso per un nuovo account utente.

   >[!div class="mx-imgBorder"]
   >![Modifica della password][update-password]

1. Dopo aver eseguito l'accesso, verrà visualizzato l'esempio "Hello Group 1 Users!". testo del controller.

   >[!div class="mx-imgBorder"]
   >![Authorized_group1][hello-group1]

   > [!NOTE]
   > Agli account utente non autorizzati verrà visualizzato il messaggio **HTTP 403** di accesso non autorizzato.

   >[!div class="mx-imgBorder"]
   >![UnAuthorized_group2][Unauthorized-group2]
## <a name="summary"></a>Summary

In questa esercitazione si è creata una nuova applicazione Web Java con l'utilità di avvio per Azure Active Directory, si è configurato un nuovo tenant di Azure AD registrandovi quindi una nuova applicazione e infine si è configurata l'applicazione in modo da usare le annotazioni e le classi Spring per proteggere l'app Web.

## <a name="see-also"></a>Vedere anche
* Per informazioni sulle nuove opzioni dell'interfaccia utente, vedere [Guida di formazione per la registrazione di app nel portale di Azure](/azure/active-directory/develop/app-registrations-training-guide-for-app-registrations-legacy-users)

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni su Spring e Azure, passare al centro di documentazione di Spring in Azure.

   >[!div class="nextstepaction"]
   >[Spring in Azure](./index.yml)

<!-- URL List -->

[Azure Active Directory Documentation]: /azure/active-directory/
[AAD app manifest]: /azure/active-directory/develop/active-directory-application-manifest
[Get started with Azure AD]: /azure/active-directory/get-started-azure-ad
[Azure for Java Developers]: ../index.yml
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[Working with Azure DevOps and Java]: /azure/devops/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/
[AAD Spring Boot Sample]: https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/spring/azure-spring-boot-samples/

<!-- IMG List -->

[create-spring-app-01]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-spring-app-01.png

[create-directory-00]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-directory-00.png
[create-directory-01]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-directory-01.png
[create-directory-02]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-directory-02.png
[create-directory-03]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-directory-03.png
[create-directory-04]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-directory-04.png

[create-app-registration-01]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-01.png
[create-app-registration-02]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-02.png
[create-app-registration-03]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-03.png
[create-app-registration-03-5]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-03-5.png
[create-app-registration-04]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-04.png
[create-app-registration-04-5]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-04-5.png
[create-app-registration-05]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-05.png
[create-app-registration-08]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-08.png
[create-app-registration-09]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-09.png
[create-app-registration-10]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-10.png

[create-user-01]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-user-01.png
[create-user-02]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-user-02.png
[create-user-03]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-user-03.png
[create-user-04]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-user-04.png

[application-login]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/application-login.png
[build-application]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/build-application.png
[hello-group1]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/hello-group1.png
[update-password]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/update-password.png
[Unauthorized-group2]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/unauthorized-group2.png
