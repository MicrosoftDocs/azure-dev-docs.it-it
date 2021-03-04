---
title: Configurare proxy in Azure SDK per Java
description: Panoramica dei concetti relativi all'inoltro di Azure SDK per Java
author: alzimmermsft
ms.date: 02/02/2021
ms.topic: conceptual
ms.custom: devx-track-java
ms.author: alzimmer
ms.openlocfilehash: 03acb768d348505b03419eb132c1d9623633b442
ms.sourcegitcommit: 576c878c338d286060010646b96f3ad0fdbcb814
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/04/2021
ms.locfileid: "102118169"
---
# <a name="configure-proxies-in-the-azure-sdk-for-java"></a>Configurare proxy in Azure SDK per Java

Questo articolo fornisce una panoramica su come configurare Azure SDK per Java per l'uso corretto dei proxy.

## <a name="http-proxy-configuration"></a>Configurazione proxy HTTP

Le librerie client di Azure per Java offrono diversi modi per configurare un proxy per un `HttpClient` .

Ogni metodo per fornire un proxy ha i propri vantaggi e svantaggi e fornisce livelli diversi di incapsulamento. Quando è stato configurato un proxy per un `HttpClient` , utilizzerà il proxy per il resto della sua durata. Se il proxy è associato a un utente singolo `HttpClient` , consente a un'applicazione di usare più `HttpClient` istanze in cui ognuno può usare un proxy diverso per soddisfare i requisiti di proxy di un'applicazione.

Le opzioni di configurazione del proxy sono:

* [Usare un proxy dell'ambiente](#use-an-environment-proxy)
* [Usare un proxy di configurazione](#use-a-configuration-proxy)
* [Usa un proxy esplicito](#use-an-explicit-proxy)

### <a name="use-an-environment-proxy"></a>Usare un proxy dell'ambiente

Per impostazione predefinita, i compilatori client HTTP verificheranno l'ambiente per le configurazioni proxy. Questo processo USA Azure SDK per le `Configuration` API Java. Quando il generatore crea un client, viene configurato con una copia della "configurazione globale" recuperata chiamando `Configuration.getGlobalConfiguration()` . Questa chiamata verrà letta in qualsiasi configurazione proxy HTTP dall'ambiente di sistema.

Quando il generatore controlla l'ambiente, Cerca le configurazioni di ambiente seguenti nell'ordine specificato:

1. `HTTPS_PROXY`
2. `HTTP_PROXY`
3. `https.proxy*`
4. `http.proxy*`

`*`Rappresenta le proprietà del proxy Java note. Per ulteriori informazioni, vedere la pagina relativa alla [rete e ai proxy Java](https://docs.oracle.com/javase/8/docs/technotes/guides/net/proxies.html) nella documentazione di Oracle.

Se il generatore trova una delle configurazioni dell'ambiente, viene creata un' `ProxyOptions` istanza chiamando `ProxyOptions.fromConfiguration(Configuration.getGlobalConfiguration())` . Questo articolo fornisce altri dettagli sul `ProxyOptions` tipo.

> [!Important]
> Per usare qualsiasi configurazione proxy, Java richiede di impostare la proprietà dell'ambiente `java.net.useSystemProxies` di sistema su `true` .

È anche possibile creare un'istanza del client HTTP che non usa alcuna configurazione proxy presente nelle variabili di ambiente del sistema. Per eseguire l'override del comportamento predefinito, impostare in modo esplicito un oggetto configurato in modo diverso `Configuration` nel generatore client http. Quando si imposta un oggetto `Configuration` nel generatore, non chiamerà più `Configuration.getGlobalConfiguration()` . Se ad esempio si chiama `configuration(Configuration)` usando `Configuration.NONE` , è possibile impedire in modo esplicito al generatore di controllare l'ambiente per la configurazione.

Nell'esempio seguente viene utilizzata la `HTTP_PROXY` variabile di ambiente con value `localhost:8888` per utilizzare Fiddler come proxy. Questo codice illustra la creazione di un client HTTP OkHttp e Netty. Per ulteriori informazioni sulla configurazione client HTTP, vedere [client e pipeline HTTP](http-client-pipeline.md).

```bash
export HTTP_PROXY=localhost:8888
```

```java
HttpClient nettyHttpClient = new NettyAsyncHttpClientBuilder().build();
HttpClient okhttpHttpClient = new OkHttpAsyncHttpClientBuilder().build();
```

Per impedire l'utilizzo del proxy di ambiente, configurare il generatore client HTTP con `Configuration.NONE` , come illustrato nell'esempio seguente:

```java
HttpClient nettyHttpClient = new NettyAsyncHttpClientBuilder()
    .configuration(Configuration.NONE)
    .build();

HttpClient okhttpHttpClient = new OkHttpAsyncHttpClientBuilder()
    .configuration(Configuration.NONE)
    .build();
```

### <a name="use-a-configuration-proxy"></a>Usare un proxy di configurazione

Anziché leggere dall'ambiente, è possibile configurare i generatori client HTTP per l'utilizzo di un personalizzato `Configuration` con le stesse impostazioni proxy già accettate dall'ambiente. Questa configurazione offre la possibilità di disporre di configurazioni riutilizzabili che hanno come ambito un caso d'uso limitato. Quando il generatore client HTTP Compila `HttpClient` , utilizzerà l'oggetto `ProxyOptions` restituito da `ProxyOptions.fromConfiguration(<Configuration passed into the builder>)` .

Nell'esempio seguente vengono `http.proxy*` usate le configurazioni impostate in un `Configuration` oggetto per usare un proxy che autentica Fiddler come proxy.

```java
Configuration configuration = new Configuration()
    .put("java.net.useSystemProxies", "true")
    .put("http.proxyHost", "localhost")
    .put("http.proxyPort", "8888")
    .put("http.proxyUser", "1")
    .put("http.proxyPassword", "1");

HttpClient nettyHttpClient = new NettyAsyncHttpClientBuilder()
    .configuration(configuration)
    .build();

HttpClient okhttpHttpClient = new OkHttpAsyncHttpClientBuilder()
    .configuration(configuration)
    .build();
```

### <a name="use-an-explicit-proxy"></a>Usa un proxy esplicito

Le librerie client Java vengono fornite con una `ProxyOptions` classe che funge da tipo di librerie client di Azure per la configurazione di un proxy. È possibile configurare `ProxyOptions` con il protocollo di rete utilizzato per inviare le richieste proxy, l'indirizzo proxy, le credenziali di autenticazione proxy e gli host non proxy. Sono necessari solo il protocollo di rete proxy e l'indirizzo proxy. Quando si usano le credenziali di autenticazione, è necessario impostare il nome utente e la password.

Nell'esempio seguente viene creata un' `ProxyOptions` istanza semplice che inoltra le richieste all'indirizzo Fiddler predefinito ( `localhost:8888` ):

```java
ProxyOptions proxyOptions = new ProxyOptions(ProxyOptions.Type.HTTP, new InetSocketAddress("localhost", 8888));
```

Nell'esempio seguente viene creato un oggetto autenticato `ProxyOptions` che proxy richieste a un'istanza di Fiddler che richiede l'autenticazione proxy:

```java
// Fiddler uses username "1" and password "1" with basic authentication as its proxy authentication requirement.
ProxyOptions proxyOptions = new ProxyOptions(ProxyOptions.Type.HTTP, new InetSocketAddress("localhost", 8888))
    .setCredentials("1", "1");
```

È possibile configurare i generatori client HTTP con `ProxyOptions` direttamente per indicare un proxy esplicito da usare. Questa configurazione è il modo più granulare per fornire un proxy e, in genere, non è flessibile come passare un oggetto `Configuration` che può essere modificato per aggiornare i requisiti di proxy.

Nell'esempio seguente viene `ProxyOptions` usato per usare Fiddler come proxy:

```java
ProxyOptions proxyOptions = new ProxyOptions(ProxyOptions.Type.HTTP, new InetSocketAddress("localhost", 8888));

HttpClient nettyHttpClient = new NettyAsyncHttpClientBuilder()
    .proxy(proxyOptions)
    .build();

HttpClient okhttpHttpClient = new OkHttpAsyncHttpClientBuilder()
    .proxy(proxyOptions)
    .build();
```

## <a name="next-steps"></a>Passaggi successivi

Ora che si ha familiarità con la configurazione del proxy in Azure SDK per Java, vedere [configurare la traccia in Azure SDK per Java](tracing.md) per comprendere meglio i flussi all'interno dell'applicazione e per facilitare la diagnosi dei problemi.
