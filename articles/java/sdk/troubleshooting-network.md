---
title: Risolvere i problemi di rete quando si usa Azure SDK per Java
description: Panoramica su come risolvere i problemi di rete correlati all'uso di Azure SDK per Java
author: alzimmermsft
ms.date: 02/02/2021
ms.topic: conceptual
ms.custom: devx-track-java
ms.author: alzimmer
ms.openlocfilehash: 868f15d7f8e0791ea190b77a6679f88c59d363e4
ms.sourcegitcommit: 576c878c338d286060010646b96f3ad0fdbcb814
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/04/2021
ms.locfileid: "102118209"
---
# <a name="troubleshoot-networking-issues"></a>Risolvere i problemi di rete

Questo articolo descrive alcuni strumenti che consentono di diagnosticare i problemi di rete di varie complessità. Questi problemi includono scenari che variano dalla risoluzione dei problemi di un valore di risposta imprevisto da un servizio, alla radice che causa un'eccezione di chiusura della connessione.

Per la risoluzione dei problemi lato client, le librerie client di Azure per Java offrono una storia di registrazione coerente e affidabile, come descritto in [configurare la registrazione in Azure SDK per Java](logging-overview.md). Tuttavia, le librerie client effettuano chiamate di rete su diversi protocolli, che possono causare scenari di risoluzione dei problemi che si estendono al di fuori dell'ambito di risoluzione dei problemi fornito. Quando si verificano questi problemi, la soluzione consiste nell'usare gli strumenti esterni descritti in questo articolo per diagnosticare i problemi di rete.

## <a name="fiddler"></a>Fiddler

[Fiddler](https://docs.telerik.com/fiddler-everywhere/introduction) è un proxy di debug HTTP che consente di registrare le richieste e le risposte trasmesse così come sono. Le richieste e le risposte non elaborate acquisite consentono di risolvere i problemi relativi agli scenari in cui il servizio riceve una richiesta imprevista oppure il client riceve una risposta imprevista. Per usare Fiddler, è necessario configurare la libreria client con un proxy HTTP. Se si usa HTTPS, è necessaria una configurazione aggiuntiva necessaria per esaminare i corpi della richiesta e della risposta decrittografati.

### <a name="add-an-http-proxy"></a>Aggiungere un proxy HTTP

Per aggiungere un proxy HTTP, seguire le istruzioni riportate in [configurare proxy in Azure SDK per Java](proxying.md). Assicurarsi di usare l'indirizzo Fiddler predefinito di `localhost` sulla porta 8888.

### <a name="enable-https-decryption"></a>Abilita decrittografia HTTPS

Per impostazione predefinita, Fiddler può acquisire solo il traffico HTTP. Se l'applicazione usa HTTPS, è necessario eseguire passaggi aggiuntivi per considerare attendibile il certificato di Fiddler per consentire l'acquisizione del traffico HTTPS. Per ulteriori informazioni, vedere [menu HTTPS](https://docs.telerik.com/fiddler-everywhere/user-guide/settings/https) nella documentazione di Fiddler.

Nelle procedure seguenti viene descritto come utilizzare il Java Runtime Environment (JRE) per considerare attendibile il certificato. Senza considerare attendibile il certificato, una richiesta HTTPS tramite Fiddler potrebbe non riuscire con avvisi di sicurezza.

**Linux/macOS**

1. Esporta il certificato del Fiddler.
1. Trovare lo strumento di ricerca di JRE (in genere `jre/bin` ).
1. Trovare il CAcert di JRE (in genere `jre/lib/security` ).
1. Aprire una finestra bash ed eseguire il comando seguente per importare il certificato:

   ```bash
   sudo keytool -import -file <location of Fiddler certificate> -keystore <location of cacert> -alias Fiddler
   ```

1. Immettere una password.
1. Considerare attendibile il certificato.

**Windows**

1. Esporta il certificato del Fiddler. Il certificato viene in genere esportato sul desktop.
1. Trovare lo strumento di ricerca di JRE (in genere `jre\bin` ).
1. Trovare il CAcert di JRE (in genere `jre\lib\security` ).
1. Aprire una finestra di PowerShell in modalità amministratore ed eseguire il comando seguente per importare il certificato:

   ```cmd
   keytool.exe -import -trustcacerts -file <location of Fiddler certificate> -keystore <location of cacert> -alias Fiddler
   ```

   Ad esempio, il comando seguente usa alcuni valori comuni:

   ```cmd
   keytool.exe -import -trustcacerts -file "C:\Users\username\Desktop\FiddlerRootCertificate.crt" -keystore "C:\Program Files\AdoptOpenJDK\jdk-8.0.275.1-hotspot\jre\lib\security\cacerts" -alias Fiddler
   ```

1. Immettere una password. Se la password non è stata impostata in precedenza, il valore predefinito è "changeit". Per ulteriori informazioni, vedere [utilizzo di certificati e SSL](https://docs.oracle.com/cd/E19830-01/819-4712/ablqw/index.html) nella documentazione di Oracle.
1. Considerare attendibile il certificato.

## <a name="wireshark"></a>Wireshark

[Wireshark](https://www.wireshark.org/) è un analizzatore del protocollo di rete in grado di acquisire il traffico di rete senza che sia necessario apportare modifiche al codice dell'applicazione. Wireshark è altamente configurabile ed è in grado di acquisire un'ampia gamma di traffico di rete di basso livello. Questa funzionalità è utile per la risoluzione dei problemi relativi a scenari quali un host remoto che chiude una connessione o che dispone di connessioni chiuse durante un'operazione. La GUI di Wireshark Visualizza le acquisizioni usando una combinazione di colori che identifica i case di acquisizione univoci, ad esempio una ritrasmissione TCP, RST e così via. È anche possibile filtrare le acquisizioni in fase di acquisizione o durante l'analisi.

### <a name="configure-a-capture-filter"></a>Configurare un filtro di acquisizione

I filtri di acquisizione riducono il numero di chiamate di rete acquisite per l'analisi. Senza i filtri di acquisizione, Wireshark acquisisce tutto il traffico che passa attraverso un'interfaccia di rete. Questo comportamento può produrre enormi quantità di dati, in cui la maggior parte di esso potrebbe essere un rumore per l'indagine. L'uso di un filtro di acquisizione consente di definire in modo preventivo l'ambito del traffico di rete acquisito per consentire l'individuazione di un'indagine Per ulteriori informazioni, vedere [acquisizione dei dati di rete in tempo reale](https://www.wireshark.org/docs/wsug_html_chunked/ChapterCapture.html) nella documentazione di Wireshark.

Nell'esempio seguente viene aggiunto un filtro di acquisizione per acquisire il traffico di rete inviato o ricevuto da un host specifico.

In Wireshark passare a **acquisisci > Acquisisci filtri** e aggiungere un nuovo filtro con il valore `host <host IP or hostname>` . Questo filtro acquisisce il traffico solo da e verso tale host. Se l'applicazione comunica a più host, è possibile aggiungere più filtri di acquisizione oppure è possibile aggiungere l'indirizzo IP/nome host dell'host con l'operatore "OR" per fornire filtri di acquisizione Looser.

### <a name="capture-to-disk"></a>Acquisisci su disco

Potrebbe essere necessario eseguire un'applicazione per molto tempo per riprodurre un'eccezione di rete imprevista e per vedere il traffico che conduce a tale applicazione. Inoltre, potrebbe non essere possibile mantenere tutte le acquisizioni in memoria. Per fortuna, Wireshark può registrare le acquisizioni sul disco in modo che siano disponibili per la post-elaborazione. Questo approccio evita il rischio di esaurire la memoria durante la riproduzione di un problema. Per ulteriori informazioni, vedere [input, output e stampa di file](https://www.wireshark.org/docs/wsug_html_chunked/ChapterIO.html) nella documentazione di Wireshark.

L'esempio seguente configura Wireshark per salvare in modo permanente le acquisizioni su disco con più file, in cui i file vengono suddivisi in 100.000 acquisizioni o 50 MB di dimensioni.

In Wireshark passare a **acquisisci > opzioni** e trovare la scheda **output** , quindi immettere un nome file da usare. Questa configurazione provocherà il salvataggio permanente di Wireshark in un singolo file. Per abilitare l'acquisizione in più file, selezionare **crea automaticamente un nuovo file** , quindi selezionare **dopo 100000 pacchetti** e **dopo 50 megabyte**. Con questa configurazione, Wireshark creerà un nuovo file quando viene trovato uno dei predicati. Ogni nuovo file utilizzerà lo stesso nome di base del nome file immesso e aggiungerà un identificatore univoco. Se si vuole limitare il numero di file che Wireshark può creare, selezionare **Usa un buffer circolare con i file X**. Questa opzione consente di limitare la registrazione di Wireshark solo con il numero specificato di file. Quando viene raggiunto il numero di file, Wireshark inizierà a sovrascrivere i file, a partire dal meno recente.

### <a name="filter-captures"></a>Acquisizioni di filtri

In alcuni casi non è possibile definire l'ambito del traffico acquisito da Wireshark, ad esempio se l'applicazione comunica con più host usando diversi protocolli. In questo scenario, in genere con l'acquisizione persistente descritta in precedenza, è più semplice eseguire l'analisi dopo l'acquisizione di rete. Wireshark supporta la sintassi di tipo filtro per l'analisi delle acquisizioni. Per ulteriori informazioni, vedere [utilizzo dei pacchetti acquisiti](https://www.wireshark.org/docs/wsug_html_chunked/ChapterWork.html) nella documentazione di Wireshark.

Nell'esempio seguente viene caricato un file di acquisizione permanente e viene filtrato in `ip.src_host==<IP>` .

In Wireshark passare a **File > aprire** e caricare un'acquisizione permanente dal percorso del file usato sopra. Una volta caricato il file sotto la barra dei menu, viene visualizzato un input di filtro. In input filtro immettere `ip.src_host==<IP>` . Questo filtro consente di limitare la visualizzazione di acquisizione in modo da visualizzare solo le acquisizioni in cui l'origine è stata dall'host con l'indirizzo IP `<IP>` .

## <a name="next-steps"></a>Passaggi successivi

Questo argomento illustra l'uso di diversi strumenti per diagnosticare i problemi di rete quando si lavora con Azure SDK per Java. Ora che si ha familiarità con gli scenari di utilizzo di alto livello, è possibile iniziare a esplorare l'SDK. Per ulteriori informazioni sulle API disponibili, vedere [Azure SDK per le librerie Java](azure-sdk-library-package-index.md).
