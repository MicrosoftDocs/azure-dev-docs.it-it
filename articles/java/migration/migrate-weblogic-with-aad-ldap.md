---
title: Autorizzazione e autenticazione degli utenti finali con Azure Active Directory per la migrazione di app Java in WebLogic Server ad Azure
description: Questa guida descrive come configurare Oracle WebLogic Server per la connessione con Azure Active Directory Domain Services tramite LDAP
author: edburns
ms.author: edburns
ms.topic: tutorial
ms.date: 08/10/2020
ms.openlocfilehash: 58a36de1e52415fc563b294215818b1860cd7ba2
ms.sourcegitcommit: b0a119a624e9cb6b76d968951543a414bd08eaa0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/05/2021
ms.locfileid: "102220380"
---
# <a name="end-user-authorization-and-authentication-for-migrating-java-apps-on-weblogic-server-to-azure"></a>Autorizzazione e autenticazione degli utenti finali per la migrazione di app Java in WebLogic Server ad Azure

Questa guida illustra come abilitare l'autenticazione e l'autorizzazione degli utenti finali di livello aziendale per le app Java in WebLogic Server usando Azure Active Directory.

Gli sviluppatori Java EE si aspettano che i [meccanismi di sicurezza della piattaforma standard](https://javaee.github.io/tutorial/security-intro.html#BNBWJ) funzionino, anche quando trasferiscono i loro carichi di lavoro in Azure.  Le [applicazioni Azure Oracle WebLogic Server (WLS)](/azure/virtual-machines/workloads/oracle/oracle-weblogic) consentono di inserire gli utenti di Azure Active Directory Domain Services (Azure AD DS) nell'area di autenticazione di sicurezza predefinita.  Usare l'elemento `<security-role>` standard nelle applicazioni Java EE in Azure. Le informazioni utente vengono trasferite da Azure AD DS tramite LDAP (Lightweight Directory Access Protocol).

Questa guida è divisa in due parti.  Se si ha già Azure AD DS con LDAP sicuro esposto, è possibile passare direttamente alla seconda parte.

* [Configurazione di Azure Active Directory](#azure-active-directory-configuration)
* [Configurazione di WLS](#wls-configuration)

Questa guida illustra come eseguire queste operazioni:

> [!div class="checklist"]
> * Creare e configurare un dominio gestito di Azure Active Directory Domain Services
> * Configurare LDAP (Lightweight Directory Access Protocol) sicuro per un dominio gestito di Azure AD DS
> * Abilitare WebLogic Server per accedere a LDAP come area di autenticazione di sicurezza predefinita

Questa guida non illustra come riconfigurare una distribuzione di Azure AD esistente, ma dovrebbe essere possibile seguirla per verificare quali passaggi possono essere ignorati.

## <a name="prerequisites"></a>Prerequisiti

* Una sottoscrizione di Azure attiva.
  * Se non si ha una sottoscrizione di Azure, [creare un account gratuito](https://azure.microsoft.com/free/).
* La possibilità di distribuire una delle applicazioni Azure WLS incluse nell'elenco [Applicazioni Azure Oracle WebLogic Server](/azure/virtual-machines/workloads/oracle/oracle-weblogic).

## <a name="migration-context"></a>Contesto di migrazione

Ecco alcuni fattori da considerare per la migrazione di installazioni WLS locali e Azure AD.

* Se si ha già un tenant di Azure AD senza servizi di dominio esposti tramite LDAP, questa guida illustra come esporre la funzionalità LDAP e integrarla con WLS.
* Se lo scenario include una foresta di Active Directory locale, è consigliabile implementare una soluzione ibrida di gestione delle identità con Azure AD.  Per altre informazioni, vedere la [documentazione della gestione ibrida delle identità](/azure/active-directory/hybrid/)
* Se si ha già una distribuzione di Active Directory Domain Services (AD DS) locale, vedere [Confrontare soluzioni Active Directory Domain Services autogestite, Azure Active Directory e Azure Active Directory Domain Services gestite](/azure/active-directory-domain-services/compare-identity-solutions) per esplorare i percorsi di migrazione.
* Se si esegue l'ottimizzazione per il cloud, questa guida illustra come iniziare da zero con LDAP e WLS in Azure AD DS.
* Per una panoramica completa della migrazione di WebLogic Server a Macchine virtuali di Azure, vedere [Eseguire la migrazione di applicazioni WebLogic alle macchine virtuali di Azure](migrate-weblogic-to-virtual-machines.md).

## <a name="azure-active-directory-configuration"></a>Configurazione di Azure Active Directory

Questa sezione include tutti i passaggi necessari per configurare un'istanza di Azure AD DS integrata con WLS.  Azure Active Directory non supporta direttamente il protocollo LDAP (Lightweight Directory Access Protocol) o LDAP sicuro. Il supporto viene invece abilitato tramite l'istanza di Azure AD Domain Services (Azure AD DS) nel tenant di Azure AD.

>[!NOTE]
> Questa guida usa la funzionalità degli account utente "solo cloud" di Azure AD DS.  Sono supportati altri tipi di account utente, ma non vengono descritti nella guida.

### <a name="create-and-configure-an-azure-active-directory-domain-services-managed-domain"></a>Creare e configurare un dominio gestito di Azure Active Directory Domain Services

Questa sezione fa riferimento a un'esercitazione distinta per configurare un dominio gestito di Azure AD DS.

Completare l'esercitazione [Creare e configurare un dominio gestito Azure di Active Directory Domain Services](/azure/active-directory-domain-services/tutorial-create-instance) fino alla sezione [Abilitare gli account utente per Azure AD DS](/azure/active-directory-domain-services/tutorial-create-instance#enable-user-accounts-for-azure-ad-ds) esclusa.  Questa sezione richiede un trattamento speciale nel contesto di questa esercitazione, come descritto nella sezione successiva.  Assicurarsi di completare interamente e nel modo corretto le azioni per il DNS.

Prendere nota del valore specificato quando si completa il passaggio "Immettere un nome di dominio DNS per il dominio gestito".  Questo valore sarà usato più avanti in questo articolo.

### <a name="create-users-and-reset-passwords"></a>Creare gli utenti e reimpostare le password

Questa sezione include i passaggi per creare gli utenti e cambiare le relative password, che è necessario eseguire per propagare correttamente gli utenti tramite LDAP.  Se si ha già un'installazione di Azure AD DS, questo passaggio potrebbe non essere necessario.

1. All'interno del portale di Azure assicurarsi che la directory corrispondente al tenant di Azure AD sia la directory attiva.  Per informazioni su come selezionare la directory corretta, vedere [Associare o aggiungere una sottoscrizione di Azure al tenant di Azure Active Directory](/azure/active-directory/fundamentals/active-directory-how-subscriptions-associated-directory).  Se si seleziona la directory non corretta, gli utenti non potranno essere creati oppure verranno creati nella directory errata.
1. Nella casella di ricerca nella parte superiore del portale di Azure immettere "Utenti".
1. Selezionare **Nuovo utente**.
1. Assicurarsi che l'opzione **Crea utente** sia selezionata.
1. Immettere i valori per nome utente, nome e cognome.  Lasciare i valori predefiniti negli altri campi.
1. Selezionare **Crea**.
1. Selezionare l'utente appena creato nella tabella.
1. Selezionare **Reimposta password**.
1. Nel pannello visualizzato selezionare **Reimposta password**.
1. Prendere nota della password temporanea.
1. In una finestra del browser in "incognito" passare al [portale di Azure](https://portal.azure.com/) e accedere con le credenziali e la password dell'utente.
1. Modificare la password quando viene richiesto.  Prendere nota della nuova password.  Verrà usata più avanti.
1. Disconnettersi e chiudere la finestra in "incognito".

Ripetere i passaggi da"Selezionare **Nuovo utente**" fino a "Disconnettersi e chiudere" per ogni utente da abilitare.

### <a name="allow-ldap-in-azure-ad-ds"></a>Autorizzare LDAP in Azure AD DS

Questa sezione fa riferimento a un'esercitazione distinta per estrarre i valori da usare per la configurazione di WLS.

Aprire prima di tutto l'esercitazione [Configurare LDAP sicuro per un dominio gestito di Azure Active Directory Domain Services](/azure/active-directory-domain-services/tutorial-configure-ldaps) in una finestra del browser distinta, in modo da poter esaminare le variazioni seguenti durante l'esecuzione dell'esercitazione.  

Quando si raggiunge la sezione [Esportare un certificato per i computer client](/azure/active-directory-domain-services/tutorial-configure-ldaps#export-a-certificate-for-client-computers), prendere nota del percorso in cui si salva il file del certificato con estensione *cer*.  Il certificato verrà usato come input per la configurazione di WLS.

Quando si raggiunge la sezione [Bloccare l'accesso LDAP sicuro su Internet](/azure/active-directory-domain-services/tutorial-configure-ldaps#lock-down-secure-ldap-access-over-the-internet), specificare **Qualsiasi** come origine.  La regola di sicurezza verrà rafforzata con un indirizzo IP specifico più avanti in questa guida.

Prima di eseguire i passaggi descritti in [Testare le query sul dominio gestito](/azure/active-directory-domain-services/tutorial-configure-ldaps#test-queries-to-the-managed-domain), seguire questa procedura per assicurarsi che i test vengano superati.

   1. Nel portale passare alla pagina di panoramica relativa all'istanza di Azure AD Domain Services.
   1. Nell'area Impostazioni selezionare **Proprietà**.
   1. A destra della pagina scorrere in basso fino a visualizzare **Gruppo Amministratori**.  In questa sezione deve essere presente un collegamento ad **Amministratori di AAD DC**.  Selezionare il collegamento.
   1. Nella sezione **Gestisci** selezionare **Membri**.
   1. Selezionare **Aggiungi membri**.
   1. Nel campo di testo **Cerca** immettere alcuni caratteri per individuare uno degli utenti creati in un passaggio precedente.
   1. Selezionare l'utente e quindi attivare il pulsante **Seleziona**.
   1. Questo è l'utente che è necessario usare quando si eseguono i passaggi della sezione **Testare le query sul dominio gestito**.
   >[!NOTE]
   >
   > Ecco alcuni suggerimenti relativi all'esecuzione di query sui dati LDAP, da completare per raccogliere alcuni valori necessari per la configurazione di WLS.
   >
   > * L'esercitazione consiglia di usare il programma Windows *LDP.exe*.  Questo programma è disponibile solo per Windows.  Per gli utenti non Windows, è anche possibile usare [Apache Directory Studio](https://directory.apache.org/studio/downloads.html) per lo stesso scopo.
   > * Quando si accede a LDAP con *LDP.exe*, il nome utente corrisponde solo alla parte prima della chiocciola @.  Se ad esempio l'utente è `alice@contoso.onmicrosoft.com`, il nome utente per l'azione di binding di *LDP.exe* sarà `alice`.  Inoltre, lasciare *LDP.exe* in esecuzione e connesso per l'uso nei passaggi successivi.
   >
Nella sezione [Configurare la zona DNS per l'accesso esterno](/azure/active-directory-domain-services/tutorial-configure-ldaps#configure-dns-zone-for-external-access) prendere nota del valore dell'**indirizzo IP esterno di LDAP sicuro**.  Verrà usato più avanti.

Se il valore dell'**indirizzo IP esterno per LDAP sicuro** non è immediatamente evidente, attenersi alla procedura seguente per ottenere l'indirizzo IP.

1. Nel portale trovare il gruppo di risorse che contiene la risorsa Azure AD Domain Services.
1. Nell'elenco di risorse selezionare la risorsa IP pubblico per la risorsa Azure AD Domain Services, come illustrato di seguito.  L'IP pubblico inizierà probabilmente con `aads`.
   :::image type="content" source="media/migrate-weblogic-with-aad-ldap/alternate-secure-ip-address-technique.png" alt-text="Browser che Mostra come selezionare l'indirizzo IP pubblico.":::
1. L'indirizzo IP pubblico viene visualizzato accanto all'etichetta **Indirizzo IP**.

Non eseguire la procedura descritta in [Pulire le risorse](/azure/active-directory-domain-services/tutorial-configure-ldaps#clean-up-resources) fino a quando non viene richiesto in questa guida.

Considerando tutti questi fattori, completare la sezione [Configurare l'accesso LDAP sicuro per un dominio gestito di Azure Active Directory Domain Services](/azure/active-directory-domain-services/tutorial-configure-ldaps).  È ora possibile raccogliere i valori necessari da specificare per la configurazione di WLS.

>[!NOTE]
> Attendere il completamento dell'elaborazione della configurazione LDAP sicura prima di procedere alla sezione successiva.

### <a name="disable-weak-tls-v1"></a>Disabilitare il protocollo TLS v1 vulnerabile

Per impostazione predefinita, Azure Active Directory Domain Services (Azure AD DS) abilita l'uso di TLS v1, che è considerato un protocollo vulnerabile e non supportato in WebLogic Server 14 e versioni successive. 

Questa sezione illustra in modo dettagliato la procedura per la disabilitazione della crittografia TLS v1.

Ottenere prima di tutto l'ID della risorsa dell'istanza di Azure Domain Service che abilita LDAP. L'esempio seguente recupera l'ID di un'istanza di Azure Domain Service denominata `aaddscontoso.com` in un gruppo di risorse denominato `aadds-rg`.

```azurecli
AADDS_ID=$(az resource show --resource-group aadds-rg --resource-type "Microsoft.AAD/DomainServices" --name aaddscontoso.com --query "id" --output tsv)
```

Eseguire questo comando per disabilitare TLS v1:

```azurecli
az resource update --ids $AADDS_ID --set properties.domainSecuritySettings.tlsV1=Disabled
```

L'output mostrerà `"tlsV1": "Disabled"` per `domainSecuritySettings`, come illustrato nell'esempio seguente:

```text
"domainSecuritySettings": {
      "ntlmV1": "Enabled",
      "syncKerberosPasswords": "Enabled",
      "syncNtlmPasswords": "Enabled",
      "syncOnPremPasswords": "Enabled",
      "tlsV1": "Disabled"
}
```

Per altre informazioni, vedere [Disabilitare le crittografie vulnerabili e la sincronizzazione degli hash delle password per proteggere un dominio gestito di Azure Active Directory Domain Services](/azure/active-directory-domain-services/secure-your-domain).

## <a name="wls-configuration"></a>Configurazione di WLS

Questa sezione illustra come raccogliere i valori dei parametri dall'istanza di Azure AD DS distribuita in precedenza.

Quando si distribuiscono le applicazioni Azure incluse nell'elenco [Applicazioni Azure Oracle WebLogic Server](/azure/virtual-machines/workloads/oracle/oracle-weblogic), è possibile scegliere di impostare la distribuzione in modo che si connetta automaticamente a un server LDAP preesistente.  In alternativa, è possibile configurare la connessione a LDAP in un secondo momento richiamando il sottomodello di integrazione di Active Directory.  Questo approccio viene descritto nell'appendice A della [documentazione ufficiale](https://wls-eng.github.io/arm-oraclelinux-wls/).  In entrambi i casi, è necessario avere i valori dei parametri necessari da passare al modello di Resource Manager.

| Nome parametro | Descrizione   | Dettagli | 
|----------------|---------------|---------|
| `aadsServerHost` | Host del server | Questo valore è il nome DNS pubblico salvato al completamento della procedura [Creare e configurare un dominio gestito di Azure Active Directory Domain Services](/azure/active-directory-domain-services/tutorial-create-instance). |
| `aadsPublicIP` | Indirizzo IP esterno per LDAP sicuro | Questo valore corrisponde all'**indirizzo IP esterno di LDAP sicuro** salvato nella sezione [Configurare la zona DNS per l'accesso esterno](/azure/active-directory-domain-services/tutorial-configure-ldaps#configure-dns-zone-for-external-access).|
| `wlsLDAPPrincipal` | Server principale   | Tornare in *LDP.exe*.  Eseguire i passaggi seguenti per ottenere un valore aggiuntivo per `wlsLDAPPrincipal`. <ol><li>Scegliere **Albero** dal menu **Visualizza**.</li><li>Nella finestra di dialogo **Visualizzazione albero** lasciare vuoto il campo **BaseDN** e selezionare **OK**.</li><li>Fare clic con il pulsante destro del mouse nel riquadro destro e scegliere **Cancella output**.</li><li>Espandere la visualizzazione albero a sinistra e selezionare la voce che inizia con "OU=AADDC Users".</li><li>Scegliere **Cerca** dal menu **Sfoglia**.</li><li>Nella finestra di dialogo visualizzata accettare le impostazioni predefinite e selezionare **Esegui**.</li><li>Quando nel riquadro destro viene visualizzato l'output, selezionare **Chiudi** accanto a **Esegui**.</li><li>Cercare nell'output la voce **Dn** corrispondente all'utente aggiunto al gruppo "Amministratori di AAD DC".  Inizierà con **Dn: CN=&lt;nome utente&gt;OU=AADDC Users**.</li></ol> |
| `wlsLDAPGroupBaseDN` e `wlsLDAPUserBaseDN` | DN di base utente e DN di base gruppo | Ai fini di questa esercitazione, i valori per entrambe le proprietà sono uguali e corrispondono alla parte **wlsLDAPPrincipal** dopo la prima virgola.|
| `wlsLDAPPrincipalPassword` | Password per l'entità | Questo valore è la password dell'utente aggiunto al gruppo **Amministratori di AAD DC**. |
| `wlsLDAPProviderName` | Provider Name | Questo valore può essere lasciato vuoto.  Viene usato come nome del provider di autenticazione in WLS. |
| `wlsLDAPSSLCertificate` | Chiave pubblica per la connessione Azure AD DS LDAPs | File con estensione *cer* dei valori che è stato chiesto di salvare quando è stato completato il passaggio [Esportare un certificato per i computer client](/azure/active-directory-domain-services/tutorial-configure-ldaps#export-a-certificate-for-client-computers).

### <a name="integrating-azure-ad-ds-ldap-with-wls"></a>Integrazione di LDAP di Azure AD DS con WLS

Con i valori di configurazione precedenti a disposizione e LDAP di Azure AD DS distribuito, è ora possibile avviare la configurazione.  Questo processo può essere completato con due approcci.

#### <a name="during-wls-deployment"></a>Durante la distribuzione di WLS

Passare ad [Applicazioni Azure Oracle WebLogic Server](/azure/virtual-machines/workloads/oracle/oracle-weblogic) e selezionare l'amministrazione o una delle offerte di cluster.  Durante la distribuzione dell'offerta, una delle schede del processo di distribuzione sarà **Azure Active Directory**.  Impostare **Connessione ad Azure Active Directory** su **Sì**.  Compilare i valori in base alle informazioni raccolte nella sezione precedente.  Per il certificato, è necessario caricare direttamente il file `.cer`.

#### <a name="after-wls-deployment"></a>Dopo la distribuzione di WLS

Se l'opzione **Connessione ad Azure Active Directory** non è stata impostata su **Sì** in fase di distribuzione, è possibile usare i valori raccolti nella sezione precedente per eseguire la configurazione in un secondo momento.  Altre informazioni sono disponibili nella [documentazione ufficiale](https://wls-eng.github.io/arm-oraclelinux-wls/).

### <a name="validate-the-deployment"></a>Convalidare la distribuzione

Dopo aver distribuito WLS e configurato LDAP con uno dei due metodi precedenti, seguire questa procedura per verificare se l'integrazione è riuscita.

1. Passare alla console di amministrazione di WLS.
1. Nel riquadro di spostamento sinistro espandere l'albero per selezionare **Security Realms** -> **myrealm** -> **Providers** (Aree di autenticazione di sicurezza > area personale > Provider).
1. Se l'integrazione è riuscita, verrà visualizzato un provider di Azure AD, ad esempio `AzureActiveDirectoryProvider`.
1. Nel riquadro di spostamento sinistro espandere l'albero per selezionare **Security Realms** -> **myrealm** -> **Users and Groups** (Aree di autenticazione di sicurezza > area personale > Utenti e gruppi).
1. Se l'integrazione è riuscita, verranno visualizzati gli utenti del provider di Azure AD.

### <a name="lock-down-and-secure-ldap-access-over-the-internet"></a>Bloccare l'accesso LDAP sicuro su Internet

Durante la configurazione di LDAP sicuro nei passaggi precedenti, l'origine è stata impostata su **Qualsiasi** per la regola `AllowLDAPS` nel gruppo di sicurezza di rete.  Ora che il server di amministrazione di WLS è stato distribuito e connesso a LDAP, ottenere il relativo indirizzo IP pubblico usando il portale di Azure.  Vedere [Bloccare l'accesso LDAP sicuro tramite Internet](/azure/active-directory-domain-services/tutorial-configure-ldaps#lock-down-secure-ldap-access-over-the-internet) e sostituire **Qualsiasi** con l'indirizzo IP specifico del server di amministrazione di WLS.

## <a name="clean-up-resources"></a>Pulire le risorse

A questo punto è possibile seguire la procedura descritta nella sezione [Pulire le risorse](/azure/active-directory-domain-services/tutorial-configure-ldaps#clean-up-resources) nell'articolo [Configurare LDAP sicuro per un dominio gestito di Azure Active Directory Domain Services](/azure/active-directory-domain-services/tutorial-configure-ldaps#clean-up-resources).

## <a name="next-steps"></a>Passaggi successivi

Esplorare altri aspetti della migrazione delle app WebLogic Server ad Azure.

> [!div class="nextstepaction"]
> [Eseguire la migrazione di applicazioni WebLogic Server alle macchine virtuali di Azure](./migrate-weblogic-to-virtual-machines.md)
