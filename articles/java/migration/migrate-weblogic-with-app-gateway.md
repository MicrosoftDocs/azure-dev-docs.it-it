---
title: Esercitazione - Eseguire la migrazione di un cluster di WebLogic Server ad Azure con Gateway applicazione di Azure come servizio di bilanciamento del carico
description: Questa esercitazione illustra in modo dettagliato la distribuzione di WebLogic Server in Azure con Gateway applicazione di Azure come servizio di bilanciamento del carico
author: edburns
ms.author: edburns
ms.topic: tutorial
ms.date: 08/05/2020
ms.openlocfilehash: 166e6f90218eb519242da0d89ae6146a2d589863
ms.sourcegitcommit: b923aee828cd4b309ef92fe1f8d8b3092b2ffc5a
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/10/2020
ms.locfileid: "88057550"
---
# <a name="tutorial-migrate-a-weblogic-server-cluster-to-azure-with-azure-application-gateway-as-a-load-balancer"></a>Esercitazione: Eseguire la migrazione di un cluster di WebLogic Server ad Azure con Gateway applicazione di Azure come servizio di bilanciamento del carico

Questa esercitazione illustra in modo dettagliato il processo di distribuzione di WebLogic Server (WLS) con Gateway applicazione di Azure.  Descrive i passaggi specifici necessari per la creazione di un insieme di credenziali delle chiavi, l'archiviazione di un certificato SSL nell'insieme di credenziali delle chiavi e l'uso di tale certificato per la terminazione SSL.  Anche se per tutti questi elementi è disponibile documentazione specifica, questa esercitazione mostra in particolare il modo in cui è possibile combinare questi elementi per creare una soluzione semplice ma potente per il bilanciamento del carico per WLS in Azure.

:::image type="content" border="false" source="media/migrate-weblogic-with-app-gateway/weblogic-app-gateway-key-vault.png" alt-text="Diagramma che mostra la relazione tra WLS, Gateway applicazione di Azure e Key Vault.":::

Il bilanciamento del carico è una parte essenziale della migrazione del cluster Oracle WebLogic Server in Azure.  La soluzione più semplice consiste nell'usare il supporto predefinito per [Gateway applicazione di Azure](/azure/application-gateway/overview).  Gateway applicazione è incluso nel supporto dei cluster WebLogic in Azure.  Per una panoramica del supporto dei cluster WebLogic in Azure, vedere [Informazioni su Oracle WebLogic Server in Azure](/azure/virtual-machines/workloads/oracle/oracle-weblogic).

In questa esercitazione verranno illustrate le procedure per:

> [!div class="checklist"]
> * Creare un Azure Key Vault
> * Creare un certificato SSL
> * Archiviare il certificato SSL nell'insieme di credenziali delle chiavi
> * Distribuire WebLogic Server con Gateway applicazione di Azure in Azure
> * Convalidare l'esito positivo della distribuzione di WLS e Gateway applicazione

## <a name="prerequisites"></a>Prerequisiti

* [OpenSSL](https://www.openssl.org/) in un computer che esegue un ambiente della riga di comando analogo a UNIX.

   Sebbene siano disponibili altri strumenti per la gestione dei certificati, in questa esercitazione viene usato OpenSSL. È possibile trovare OpenSSL in aggregazione con molte distribuzioni di GNU/Linux, ad esempio Ubuntu.
* Una sottoscrizione di Azure attiva.
  * Se non si ha una sottoscrizione di Azure, [creare un account gratuito](https://azure.microsoft.com/free/).
* La possibilità di distribuire una delle applicazioni Azure WLS incluse nell'elenco [Applicazioni Azure Oracle WebLogic Server](/azure/virtual-machines/workloads/oracle/oracle-weblogic).

## <a name="migration-context"></a>Contesto di migrazione

Ecco alcuni fattori da considerare per la migrazione di installazioni WLS locali e Gateway applicazione di Azure.  Anche se i passaggi di questa esercitazione costituiscono la procedura più facile per configurare un servizio di bilanciamento del carico per il cluster WebLogic Server in Azure, sono disponibili molti altri approcci.  L'elenco mostra altri aspetti da prendere in considerazione.

* Se esiste già una soluzione per il bilanciamento del carico, assicurarsi che Gateway applicazione di Azure soddisfi o superi le rispettive funzionalità.  Per un riepilogo delle funzionalità di Gateway applicazione di Azure rispetto ad altre soluzioni di bilanciamento del carico di Azure, vedere [Panoramica delle opzioni di bilanciamento del carico in Azure](/azure/architecture/guide/technology-choices/load-balancing-overview).
* Se la soluzione di bilanciamento del carico esistente fornisce una protezione dagli exploit e dalle vulnerabilità più comuni, Gateway applicazione offre tutte le funzionalità necessarie. La funzionalità Web application firewall (WAF) predefinita di Gateway applicazione implementa il [set di regole OWASP (Open Web Application Security Project) di base](https://www.owasp.org/index.php/Category:OWASP_ModSecurity_Core_Rule_Set_Project).  Per altre informazioni sul supporto di Web application firewall in Gateway applicazione, vedere [Funzionalità di Gateway applicazione](/azure/application-gateway/features#web-application-firewall).
* Se la soluzione di bilanciamento del carico esistente richiede una crittografia SSL end-to-end, sarà necessario eseguire operazioni di configurazione aggiuntive dopo avere completato i passaggi disponibili in questa guida.  Vedere [Panoramica di terminazione TLS e TLS end-to-end con Gateway applicazione](/azure/application-gateway/ssl-overview#end-to-end-tls-encryption) e la documentazione di Oracle relativa alla [Configurazione di SSL in Oracle Fusion Middleware](https://docs.oracle.com/en/middleware/fusion-middleware/12.2.1.3/asadm/configuring-ssl1.html#GUID-623906C0-B1FD-423F-AE51-061B5800E927).
* Se si esegue l'ottimizzazione per il cloud, questa guida illustra come iniziare da zero con Gateway applicazione di Azure e WLS.
* Per una panoramica completa della migrazione di WebLogic Server a Macchine virtuali di Azure, vedere [Eseguire la migrazione di applicazioni WebLogic alle macchine virtuali di Azure](migrate-weblogic-to-virtual-machines.md).

## <a name="create-an-azure-key-vault"></a>Creare un Azure Key Vault

Questa sezione mostra come usare il portale di Azure per creare un'istanza di Azure Key Vault.

1. Nel menu del portale di Azure o dalla pagina **Home** selezionare **Crea una risorsa**.
1. Nella casella di ricerca immettere **Key Vault**.
1. Nell'elenco dei risultati scegliere **Key Vault**.
1. Nella sezione Key Vault scegliere **Crea**.
1. Nella pagina **Crea insieme di credenziali delle chiavi** specificare le informazioni seguenti:
    * **Sottoscrizione** Scegliere una sottoscrizione.
    * In **Gruppo di risorse** scegliere **Crea nuovo** e immettere il nome del gruppo di risorse.  Prendere nota del nome dell'insieme di credenziali delle chiavi.  *Sarà necessario in seguito durante la distribuzione di WLS.*
    * **Nome dell'insieme di credenziali delle chiavi**: è necessario un nome univoco.  Prendere nota del nome dell'insieme di credenziali delle chiavi.  *Sarà necessario in seguito durante la distribuzione di WLS.*
    > [!NOTE]
    > È possibile usare lo stesso nome per il **Gruppo di risorse** e il **Nome dell'insieme di credenziali delle chiavi**.
    * Scegliere un percorso nel menu a discesa **Percorso**.
    * Lasciare invariati i valori predefiniti delle altre opzioni.
1. Selezionare **Avanti: Criteri di accesso**.
1. In **Abilita l'accesso a** selezionare **Azure Resource Manager per la distribuzione di modelli**.
1. Selezionare **Rivedi e crea**.
1. Selezionare **Crea**.

La creazione di un insieme di credenziali delle chiavi è abbastanza semplice e viene in genere completata in meno di due minuti.  Al termine della distribuzione, selezionare **Vai alla risorsa** e passare alla sezione successiva.

## <a name="create-an-ssl-certificate"></a>Creare un certificato SSL

Questa sezione mostra come creare un certificato SSL autofirmato in un formato idoneo per l'uso da parte dell'istanza di Gateway applicazione distribuita con WebLogic in Azure.  Il certificato deve avere una password non vuota.  Se è già disponibile un certificato SSL valido con password non vuota in formato *PFX*, è possibile ignorare questa sezione e passare alla successiva.  Se il certificato SSL valido con password non vuota esistente non è in formato *PFX*, convertirlo prima di tutto in un file *PFX* prima di passare alla sezione successiva.  In alternativa, aprire una shell dei comandi e immettere i comandi seguenti.

> [!NOTE]
> Questa sezione mostra come applicare la codifica Base 64 al certificato prima di archiviarlo come segreto nell'insieme di credenziali delle chiavi.  Questa operazione è richiesta dalla distribuzione di Azure sottostante che crea WebLogic Server e Gateway applicazione.

Seguire questa procedura per creare e applicare la codifica Base 64 al certificato:

1. Creare una chiave `RSA PRIVATE KEY`

   ```bash
   openssl genrsa 2048 > private.pem
   ```
1. Creare una chiave pubblica corrispondente.

   ```bash
   openssl req -x509 -new -key private.pem -out public.pem
   ```

   Sarà necessario rispondere ad alcune domande quando richiesto dallo strumento OpenSSL.  Questi valori verranno inclusi nel certificato.  L'esercitazione usa un certificato autofirmato, quindi i valori sono irrilevanti.  I valori letterali seguenti sono sufficienti.
     1. Per **Nome paese** immettere un codice di due lettere.
     1. Per **Nome stato/regione/provincia** immettere WA.
     1. Per **Nome organizzazione** immettere Contoso.  Per Nome unità organizzativa immettere Fatturazione.
     1. Per **Nome comune** immettere Contoso.
     1. Per **Indirizzo di posta elettronica** immettere billing@contoso.com.

1. Esportare il certificato come file *PFX*

   ```bash
   openssl pkcs12 -export -in public.pem -inkey private.pem -out mycert.pfx
   ```

   Immettere due volte la password.  Prendere nota della password.  *Sarà necessaria in seguito durante la distribuzione di WLS.*

1. Applicare la codifica Base 64 al file *mycert.pfx*

   ```bash
   base64 mycert.pfx > mycert.txt
   ```

Dopo avere creato un insieme di credenziali delle chiavi e quando è disponibile un certificato SSL valido con password non vuota, è possibile archiviare il certificato nell'insieme di credenziali delle chiavi.

## <a name="store-the-ssl-certificate-in-the-key-vault"></a>Archiviare il certificato SSL nell'insieme di credenziali delle chiavi

Questa sezione mostra come archiviare il certificato e la rispettiva password nell'insieme di credenziali delle chiavi creato nelle sezioni precedenti.

Per archiviare il certificato, seguire questa procedura:

1. Dal portale di Azure posizionare il cursore nella barra di ricerca nella parte superiore della pagina e digitare il nome dell'insieme di credenziali delle chiavi creato in precedenza nell'esercitazione.
1. L'insieme di credenziali delle chiavi dovrebbe essere visualizzato sotto l'intestazione **Risorse**.  Selezionarla.
1. Nella sezione **Impostazioni** selezionare **Segreti**.
1. Selezionare **Genera/Importa**.
1. In **Opzioni di caricamento** lasciare il valore predefinito.
1. In **Nome** immettere `myCertSecretData` o il nome che si preferisce.
1. In **Valore** immettere il contenuto del file *mycert.txt*.  La lunghezza del valore e la presenza di nuove righe non costituiscono un problema per il campo di testo.
1. Non modificare i valori predefiniti rimanenti e selezionare **Crea**.

Per archiviare la password per il certificato, seguire questa procedura:

1. Verrà visualizzata di nuovo la pagina **Segreti**.  Selezionare **Genera/Importa**.
1. In **Opzioni di caricamento** lasciare il valore predefinito.
1. In **Nome** immettere `myCertSecretPassword` o il nome che si preferisce.
1. In **Valore** immettere la password per il certificato.
1. Non modificare i valori predefiniti rimanenti e selezionare **Crea**.
1. Verrà visualizzata di nuovo la pagina **Segreti**.

## <a name="deploy-weblogic-server-with-application-gateway-to-azure"></a>Distribuire WebLogic Server con Gateway applicazione in Azure

Questa sezione illustrerà come usare l'insieme di credenziali delle chiavi, il certificato SSL e la password creati nelle sezioni precedenti.  Verrà effettuato il provisioning di un cluster WLS con Gateway applicazione di Azure creato automaticamente come servizio di bilanciamento del carico per i nodi del cluster.  Gateway applicazione userà il certificato SSL specificato per la terminazione SSL.  Per informazioni dettagliate sulla terminazione SSL con Gateway applicazione, vedere [Panoramica della terminazione TLS e di TLS end-to-end con il gateway applicazione](/azure/application-gateway/ssl-overview).

Per creare il cluster WLS e un'istanza di Gateway applicazione, seguire questa procedura:

1. Iniziare a eseguire la procedura per il provisioning di un cluster WebLogic Server come illustrato nella [documentazione di Oracle](https://aka.ms/arm-oraclelinux-wls-cluster-oracle-docs), ma tornare a questa pagina quando si raggiunge il pannello **Gateway applicazione di Azure**, mostrato qui.

   :::image type="content" source="media/migrate-weblogic-with-app-gateway/weblogic-app-gateway-blade.png" alt-text="Diagramma che mostra la relazione tra WLS, Gateway applicazione di Azure e Key Vault.":::

1. Nel pannello **Gateway applicazione di Azure** selezionare **Sì**.
1. In **Resource group name in current subscription containing the KeyVault** (Nome del gruppo di risorse nella sottoscrizione corrente contenente l'insieme di credenziali delle chiavi) immettere il nome del gruppo di risorse contenente l'insieme di credenziali delle chiavi creato in precedenza.
1. In **Name of the Azure KeyVault containing secrets for the Certificate for SSL Termination** (Nome dell'istanza di Azure Key Vault contenente segreti per il certificato per la terminazione SSL) immettere il nome dell'insieme di credenziali delle chiavi.
1. In **The name of the secret in the specified KeyVault whose value is the SSL Certificate Data** (Nome del segreto nell'insieme di credenziali delle chiavi il cui valore corrisponde ai dati del certificato SSL) immettere `myCertSecretData` o il nome immesso in precedenza.
1. In **The name of the secret in the specified KeyVault whose value is the password for the SSL Certificate** (Nome del segreto nell'insieme di credenziali delle chiavi il cui valore corrisponde alla password del certificato SSL) immettere `myCertSecretData` o il nome immesso in precedenza.
1. Selezionare **Rivedi e crea**.
1. Selezionare **Crea**.  Verrà verificato se è possibile ottenere il certificato dall'insieme di credenziali delle chiavi e se la rispettiva password corrisponde al valore archiviato per la password nell'insieme di credenziali delle chiavi.  Se questo passaggio di convalida ha esito negativo, verificare le proprietà dell'insieme di credenziali delle chiavi, assicurarsi che il certificato sia stato immesso correttamente e assicurarsi che la password sia stata immessa correttamente.
1. Quando viene visualizzato il messaggio **Convalida superata** selezionare **Crea**.

Verrà avviato il processo di creazione del cluster WLS e del rispettivo Gateway applicazione front-end, che potrebbe richiedere circa 15 minuti.  Al termine della distribuzione selezionare **Vai al gruppo di risorse**. Dall'elenco di risorse nel gruppo di risorse selezionare **myAppGateway**.

## <a name="validate-successful-deployment-of-wls-and-app-gateway"></a>Convalidare l'esito positivo della distribuzione di WLS e Gateway applicazione

Questa sezione mostra una tecnica per convalidare rapidamente la distribuzione corretta del cluster WLS e di Gateway applicazione.

Se alla fine della sezione precedente è stato selezionato **Vai al gruppo di risorse** e quindi **myAppGateway**, verrà visualizzata una pagina di panoramica per Gateway applicazione.  In caso contrario, è possibile trovare questa pagina digitando `myAppGateway` nella casella di testo nella parte superiore del portale di Azure e quindi selezionando la pagina corretta tra quelle visualizzate.  Assicurarsi di selezionare la pagina inclusa nel gruppo di risorse creato per il cluster WLS.  Completare quindi i passaggi seguenti.

1. Nel riquadro di sinistra della pagina di panoramica per **myAppGateway** scorrere verso il basso fino alla sezione **Monitoraggio** e selezionare **Integrità back-end**.
1. Quando il messaggio **caricamento** non viene più visualizzato, dovrebbe essere mostrata al centro dello schermo una tabella che illustra i nodi del cluster configurati come nodi nel pool back-end.
1. Assicurarsi che lo stato sia **Integro** per ogni nodo.

## <a name="clean-up-resources"></a>Pulire le risorse

Se non si intende continuare a usare il cluster WLS, eliminare l'insieme di credenziali delle chiavi e il cluster WLS seguendo questa procedura:

1. Visitare la pagina di panoramica per **myAppGateway**, come mostrato nella sezione precedente.
1. Nella parte superiore della pagina selezionare il gruppo di risorse sotto il testo **Gruppo di risorse**.
1. Selezionare **Elimina gruppo di risorse**.
1. Lo stato attivo per l'input sarà impostato sul campo con etichetta **DIGITARE IL NOME DEL GRUPPO DI RISORSE**.  Digitare il nome del gruppo di risorse, come richiesto.
1. Il pulsante **Elimina** verrà abilitato.  Selezionare il pulsante **Elimina**.  L'operazione richiederà qualche minuto, ma è possibile procedere al passaggio successivo durante l'elaborazione dell'eliminazione.
1. Individuare l'insieme di credenziali delle chiavi seguendo il primo passaggio della sezione [Archiviare il certificato SSL nell'insieme di credenziali delle chiavi]().
1. Selezionare **Elimina**.
1. Selezionare **Elimina** nel riquadro visualizzato.

## <a name="next-steps"></a>Passaggi successivi

Continuare a esplorare le opzioni per l'esecuzione di WLS in Azure.
> [!div class="nextstepaction"]
> [Altre informazioni su Oracle WebLogic in Azure](/azure/virtual-machines/workloads/oracle/oracle-weblogic)
