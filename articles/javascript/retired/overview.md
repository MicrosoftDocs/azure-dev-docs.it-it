---
title: Moduli di Azure per JavaScript
description: Panoramica dei moduli di gestione e di servizi di Azure per JavaScript
ms.date: 06/17/2017
ms.topic: article
ms.openlocfilehash: 193e2d3c92a9c2b8e3970e7a130246947a7cc4da
ms.sourcegitcommit: 553da4e9aa988e5bb823364244ea81961cee5bc7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/01/2020
ms.locfileid: "85792561"
---
# <a name="azure-modules-for-javascript"></a>Moduli di Azure per JavaScript

Gestire le risorse di Azure e connettersi ai servizi dalle applicazioni JavaScript con i moduli di Azure per JavaScript. Il codice è disponibile come moduli npm](/api/?view=azure-node-latest.md) da usare nei progetti.

## <a name="manage-azure-resources"></a>Gestire le risorse di Azure

Usare i moduli di gestione per creare ed eseguire query sulle risorse dalle app o per creare i propri strumenti di automazione di Azure.

Ad esempio, per creare una VM Linux usando un'interfaccia di rete esistente, scrivere il codice seguente:

```javascript
const msRestAzure = require('ms-rest-azure');
const ComputeManagementClient = require('azure-arm-compute');

// read in service principal values from env variables
const clientId = process.env['CLIENT_ID'];
const domain = process.env['DOMAIN'];
const secret = process.env['APPLICATION_SECRET'];
const subscriptionId = process.env['AZURE_SUBSCRIPTION_ID'];

msRestAzure.loginWithServicePrincipalSecret(clientId, secret, domain, function (err, credentials, subscriptions) {
    if (err) return console.log(err);
    const computeClient = new ComputeManagementClient(credentials, subscriptionId);
    // customize the VM
    const vmParameters = {
        location: "eastus",
        osProfile: {
            computerName: "newLinuxVM",
            adminUsername: adminUsername,
            adminPassword: adminPassword
        },
        linuxConfiguration: {
            ssh: {
                publicKeys: [mySshKey]
            }
        },
        hardwareProfile: {
            vmSize: 'Basic_A1'
        },
        networkProfile: {
            networkInterfaces: [
                {
                    id: myNetworkInterfaceId,
                    primary: true
                }
            ]
        },
        storageProfile: {
            imageReference: {
                publisher: 'Canonical',
                offer: 'UbuntuServer',
                sku: '16.04-LTS',
                version: 'latest'
            },
        }
    };

    // create the VM
    computeClient.virtualMachines.createOrUpdate("myResourceGroup", "newLinuxVM", vmParameters, function (err, data) {
        if (err) return console.log(err);
    });

});
```

Vedere le [istruzioni di installazione](/api/?view=azure-node-latest) per ottenere un elenco completo dei moduli e l'[articolo introduttivo](../index.yml) per configurare l'autenticazione ed eseguire il codice di esempio per creare e aggiornare le risorse nella propria sottoscrizione di Azure.

## <a name="connect-to-azure-services"></a>Connettersi ai servizi di Azure

Oltre a usare i moduli di Azure per creare e gestire risorse in Azure, è possibile usare pacchetti per connettersi ai servizi cloud di Azure e usarli nelle app. È ad esempio possibile aggiornare una tabella di un database SQL o caricare file in Archiviazione di Azure. Selezionare il pacchetto necessario per un servizio specifico dall'[elenco completo](/api/?view=azure-node-latest) e passare al [centro per sviluppatori JavaScript](https://azure.microsoft.com/develop/nodejs/) per ottenere esercitazioni e codice di esempio e apprendere l'uso dei moduli nelle app.

Per stampare, ad esempio, i contenuti di ogni BLOB in un contenitore di archiviazione di Azure:

```javascript
var azure = require('azure-storage');
var blobService = azure.createBlobService(storageConnectionString);
blobService.listBlobsSegmented('testcontainer', null, function(error, result, response) {
   if (err) return console.log(err);
   console.log(result);
});
```

## <a name="sample-code-and-reference"></a>Codice di esempio e informazioni di riferimento

Gli esempi seguenti sono relativi ad attività comuni con i moduli di gestione di Azure e includono codice pronto per l'uso nelle app:

- [Macchine virtuali](/samples/browse/?languages=javascript%2Cnodejs)
- [App Web](/samples/browse/?languages=javascript%2Cnodejs&products=azure-functions%2Cazure-app-service%2Cazure-logic-apps)
- [Database SQL](/samples/browse/?languages=javascript%2Cnodejs&products=azure-cosmos-db%2Cazure-sql-database)

Le [informazioni di riferimento](/javascript/api) sono disponibili per tutti i moduli nei moduli di gestione e di servizi. Le nuove funzionalità, le modifiche di rilievo e le istruzioni per la migrazione dalle versioni precedenti sono disponibili nelle [note sulla versione](https://github.com/Azure/azure-sdk-for-node/releases).
