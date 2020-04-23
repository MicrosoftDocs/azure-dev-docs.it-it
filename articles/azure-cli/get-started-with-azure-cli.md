---
title: Introduzione all'interfaccia della riga di comando di Azure
description: Introduzione all'uso dell'interfaccia della riga di comando di Azure con nozioni di base sui comandi.
author: dbradish-microsoft
ms.author: dbradish
manager: barbkess
ms.date: 01/30/2020
ms.topic: conceptual
ms.service: azure-cli
ms.devlang: azurecli
ms.openlocfilehash: bc9b86db6fb9c5b3731550df9dda96debcbfba9f
ms.sourcegitcommit: 36e02e96b955ed0531f98b9c0f623f4acb508661
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/22/2020
ms.locfileid: "82030723"
---
# <a name="get-started-with-azure-cli"></a>Introduzione all'interfaccia della riga di comando di Azure

Benvenuti nell'interfaccia della riga di comando di Azure.  Questo articolo presenta l'interfaccia della riga di comando e aiuta a completare le attività comuni.

> [!NOTE]
>
> Negli script e nel sito della documentazione Microsoft, gli esempi dell'interfaccia della riga di comando di Azure sono scritti per la shell `bash`. Gli esempi di una riga verranno eseguiti in qualsiasi piattaforma. Gli esempi più lunghi che includono continuazioni di riga (`\`) o l'assegnazione di variabili devono essere modificati per funzionare in altre shell, tra cui PowerShell.

## <a name="install-or-run-in-azure-cloud-shell"></a>Installazione o esecuzione in Azure Cloud Shell

Il modo più semplice per iniziare a usare l'interfaccia della riga di comando di Azure consiste nell'eseguirla in un ambiente Azure Cloud Shell tramite il browser. Per informazioni su Cloud Shell, vedere [Guida introduttiva a Bash in Azure Cloud Shell](/azure/cloud-shell/quickstart).

Quando si è pronti per installare l'interfaccia della riga di comando, vedere le [istruzioni di installazione](install-azure-cli.md).

Dopo aver installato l'interfaccia della riga di comando per la prima volta, eseguire `az --version` per verificare se è installata e se la versione è corretta.

> [!NOTE]
> Se si usa il modello di distribuzione classica di Azure, [installare l'interfaccia della riga di comando classica di Azure](install-classic-cli.md).

## <a name="sign-in"></a>Accesso

Prima di usare qualsiasi comando dell'interfaccia della riga di comando con un'installazione locale, è necessario eseguire l'accesso con [az login](/cli/azure/reference-index#az-login).

[!INCLUDE [interactive-login](includes/interactive-login.md)]

Dopo l'accesso, viene visualizzato un elenco di sottoscrizioni associate all'account Azure. Le informazioni della sottoscrizione con `isDefault: true` rappresentano la sottoscrizione attualmente attivata dopo l'accesso. Per selezionare un'altra sottoscrizione, usare il comando [az account set](/cli/azure/account#az-account-set) con l'ID sottoscrizione a cui passare. Per altre informazioni sulla selezione delle sottoscrizioni, vedere [Usare più sottoscrizioni di Azure](manage-azure-subscriptions-azure-cli.md).

È possibile accedere in modo non interattivo, come illustrato nei dettagli in [Accedere con l'interfaccia della riga di comando di Azure](authenticate-azure-cli.md).

## <a name="common-commands"></a>Comandi comuni

Questa tabella elenca alcuni comandi comuni usati nell'interfaccia della riga di comando e collegamenti alla relativa documentazione di riferimento.

| Tipo di risorsa | Gruppo di comandi dell'interfaccia della riga di comando di Azure |
|---------------|-------------------------|
| [Gruppo di risorse](/azure/azure-resource-manager/resource-group-overview) | [az group](/cli/azure/group) |
| [Macchine virtuali](/azure/virtual-machines) | [az vm](/cli/azure/vm) |
| [Account di archiviazione](/azure/storage/common/storage-introduction) | [az storage account](/cli/azure/storage/account) |
| [Insieme di credenziali di chiave](/azure/key-vault/key-vault-whatis) | [az keyvault](/cli/azure/keyvault) |
| [Applicazioni Web](/azure/app-service) | [az webapp](/cli/azure/webapp) |
| [Database SQL](/azure/sql-database) | [az sql server](/cli/azure/sql/server) |
| [Cosmos DB](/azure/cosmos-db) | [az cosmosdb](/cli/azure/cosmosdb) |

## <a name="finding-commands"></a>Ricerca dei comandi

I comandi dell'interfaccia della riga di comando sono organizzati come _comandi_ di _gruppi_. Ogni gruppo rappresenta un servizio di Azure e i comandi operano su tale servizio.

Per cercare i comandi, usare [az find](/cli/azure/reference-index#az-find). Per cercare i nomi dei comandi contenenti `secret`, ad esempio, usare il comando seguente:

```azurecli-interactive
az find secret
```

Usare l'argomento `--help` per ottenere un elenco completo di comandi e sottogruppi di un gruppo. Ad esempio, per trovare i comandi dell'interfaccia della riga di comando per l'uso di gruppi di sicurezza di rete:

```azurecli-interactive
az network nsg --help
```

L'interfaccia della riga di comando offre il completamento tramite tasto TAB per tutti i comandi nella shell bash.

## <a name="globally-available-arguments"></a>Argomenti disponibili a livello globale

Alcuni argomenti sono disponibili per tutti i comandi.

* `--help` stampa le informazioni di riferimento dell'interfaccia della riga di comando sui comandi e i relativi argomenti ed elenca i sottogruppi e i comandi disponibili.
* `--output` modifica il formato di output. I formati di output disponibili sono `json`, `jsonc` (codice JSON colorato), `tsv` (valori delimitati da tabulazioni), `table` (tabelle ASCII leggibili) e `yaml`. Per impostazione predefinita, l'output dell'interfaccia della riga di comando è in formato `json`. Per altre informazioni sui formati di output disponibili, vedere [Formati di output per l'interfaccia della riga di comando di Azure](format-output-azure-cli.md).
* `--query` usa il [linguaggio di query JMESPath](http://jmespath.org/) per filtrare l'output restituito dai servizi di Azure. Per altre informazioni sulle query, vedere [Eseguire query sui risultati dei comandi con l'interfaccia della riga di comando di Azure](query-azure-cli.md) e l'[esercitazione su JMESPath](http://jmespath.org/tutorial.html).
* `--verbose` stampa le informazioni sulle risorse create in Azure durante un'operazione e altre informazioni utili.
* `--debug` stampa una quantità ancora maggiore di informazioni sulle operazioni dell'interfaccia della riga di comando, che vengono usate a scopo di debug. Se si rilevano bug, fornire l'output generato con il flag `--debug` attivato quando si invia la segnalazione.

## <a name="interactive-mode"></a>Modalità interattiva

L'interfaccia della riga di comando offre una modalità interattiva che visualizza automaticamente le informazioni della Guida e facilita la selezione dei sottocomandi. La modalità interattiva viene attivata con il comando [az interactive](/cli/azure/reference-index#az-interactive).

```azurecli-interactive
az interactive
```

Per altre informazioni sulla modalità interattiva, vedere [Modalità interattiva dell'interfaccia della riga di comando di Azure](interactive-azure-cli.md).

È anche disponibile un [plug-in Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azurecli) che offre un'esperienza interattiva con completamento automatico e documentazione al passaggio del mouse.

## <a name="learn-cli-basics-with-quickstarts-and-tutorials"></a>Guide introduttive ed esercitazioni per apprendere le nozioni di base dell'interfaccia della riga di comando

Per iniziare a usare l'interfaccia della riga di comando di Azure, provare un'esercitazione dettagliata per configurare macchine virtuali e usare le funzionalità dell'interfaccia della riga di comando per eseguire query sulle risorse di Azure.

> [!div class="nextstepaction"]
> [Esercitazione sulla creazione di macchine virtuali con l'interfaccia della riga di comando di Azure](azure-cli-vm-tutorial.yml)

Sono anche disponibili guide introduttive per altri servizi noti.

* [Creare un account di archiviazione usando l'interfaccia della riga di comando di Azure](/azure/storage/common/storage-quickstart-create-storage-account-cli)
* [Trasferire oggetti da e verso l'archivio BLOB di Azure con l'interfaccia della riga di comando](/azure/storage/blobs/storage-quickstart-blobs-cli)
* [Creare un singolo database SQL di Azure usando l'interfaccia della riga di comando di Azure](/azure/sql-database/sql-database-get-started-cli)
* [Creare un database di Azure per il server MySQL tramite l'interfaccia della riga di comando di Azure](/azure/mysql/quickstart-create-mysql-server-database-using-azure-cli)
* [Creare un Database di Azure per PostgreSQL usando l'interfaccia della riga di comando di Azure](/azure/postgresql/quickstart-create-server-database-azure-cli)
* [Creare un'app Web Python in Azure](/azure/app-service/app-service-web-get-started-python)
* [Eseguire un'immagine dell'hub Docker personalizzata in App Web per contenitori di Azure](/azure/app-service/containers/quickstart-custom-docker-image)

## <a name="give-feedback"></a>Commenti e suggerimenti

I commenti e i suggerimenti degli utenti in merito all'interfaccia della riga di comando sono molto apprezzati, perché consentiranno di apportare miglioramenti e risolvere bug. È possibile [segnalare un problema in GitHub](https://github.com/azure/azure-cli/issues) oppure usare le funzionalità predefinite dell'interfaccia della riga di comando per lasciare un feedback generico con il comando [az feedback](/cli/azure/reference-index#az-feedback).

```azurecli-interactive
az feedback
```

## <a name="see-also"></a>Vedere anche

* [Servizi gestibili con l'interfaccia della riga di comando di Azure](azure-services-the-azure-cli-can-manage.md)
* [Elenco di riferimento completo dei comandi per l'interfaccia della riga di comando di Azure](/cli/azure/reference-index)
* [Articoli più diffusi sull'uso dell'interfaccia della riga di comando di Azure](popular-articles-using-the-azure-cli.md)
