---
title: Modalità interattiva dell'interfaccia della riga di comando di Azure
description: Usare l'interfaccia della riga di comando di Azure in modalità interattiva.
author: dbradish-microsoft
ms.author: dbradish
manager: barbkess
ms.date: 09/09/2018
ms.topic: conceptual
ms.service: azure-cli
ms.devlang: azurecli
ms.openlocfilehash: 7b3ee1e284e7f771c661bb65bf8b8ab53dafd77f
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/05/2020
ms.locfileid: "82031173"
---
# <a name="azure-cli-interactive-mode"></a>Modalità interattiva dell'interfaccia della riga di comando di Azure

È possibile usare l'interfaccia della riga di comando di Azure in modalità interattiva eseguendo il comando `az interactive`.
Con questa modalità viene attivata una shell interattiva con completamento automatico, descrizioni dei comandi ed esempi.

![modalità interattiva](./media/interactive-azure-cli/webapp-create.png)

> [!NOTE]
> Qui non viene usato lo stile predefinito, che non risulta leggibile su sfondo nero.

Se l'utente non ha effettuato l'accesso al proprio account, usare il comando `login`.

## <a name="configure"></a>Configurare

La modalità interattiva mostra, facoltativamente, le descrizioni dei comandi, le descrizioni dei parametri ed esempi di comando.
Attivare o disattivare descrizioni ed esempi tramite `F1`.

![descrizioni ed esempi](./media/interactive-azure-cli/descriptions-and-examples.png)

È possibile attivare o disattivare la visualizzazione dei valori predefiniti dei parametri usando `F2`.

![valori predefiniti](./media/interactive-azure-cli/defaults.png)

`F3` attiva o disattiva la visualizzazione di alcuni movimenti chiave.

![movimenti](./media/interactive-azure-cli/gestures.png)

## <a name="scope"></a>Scope

È possibile definire un ambito della modalità interattiva per un gruppo di comandi specifici come `vm` o `vm image`.
Quando si esegue questa operazione, tutti i comandi vengono interpretati in tale ambito.
È una sintassi abbreviata ideale se si sta effettuando il lavoro in tale gruppo di comandi.

Anziché digitare i comandi seguenti:

```azurecli
az>> vm create -n myVM -g myRG --image UbuntuLTS
az>> vm list -o table
```

È possibile definire l'ambito per il gruppo di comandi della macchina virtuale e digitare i comandi seguenti:

```azurecli
az>> %%vm
az vm>> create -n myVM -g myRG --image UbuntuLTS
az vm>>list -o table
```

È possibile definire l'ambito anche per gruppi di comandi di livello inferiore.
È possibile definire l'ambito `vm image` usando `%%vm image`.
In questo caso, poiché è già stato definito l'ambito per `vm`, useremo `%%image`.

```azurecli
az vm>> %%image
az vm image>>
```

A questo punto, è possibile riportare l'ambito a `vm` usando `%%..` oppure è possibile definire l'ambito alla radice semplicemente con `%%`.

```azurecli
az vm image>> %%
az>>
```

## <a name="query"></a>Query

È possibile eseguire una query JMESPath sui risultati dell'ultimo comando eseguito usando `??` seguito da una query JMESPath.
Ad esempio, dopo aver creato un nuovo gruppo, è possibile recuperare il relativo ID.

```azurecli
az>> group create -n myRG -l westEurope
az>> "?? id"
```

È anche possibile usare questa sintassi per usare il risultato del comando precedente come argomento del comando successivo.* Ad esempio, dopo aver generato un elenco di tutti i gruppi, elencare tutte le risorse di tipo `virtualMachine` nel primo gruppo la cui località è Europa occidentale. 

```azurecli
az>> vm create --name myVM --resource-group myRG --image UbuntuLTS --no-wait -o json
az>> group list -o json
az>> resource list -g "?? [?location=='westeurope'].name | [0]" --query "[?type=='Microsoft.Compute/virtualMachines'].name
```

Per altre informazioni sull'esecuzione di query sui risultati dei comandi, vedere [Eseguire query sui risultati dei comandi con l'interfaccia della riga di comando di Azure](query-azure-cli.md).

## <a name="bash-commands"></a>Comandi Bash

È possibile eseguire i comandi della shell senza uscire dalla modalità interattiva tramite `#[cmd]`.

```azurecli
az>> #dir
```

## <a name="examples"></a>Esempi

Alcuni comandi hanno numerosi esempi.
È possibile passare alla pagina successiva di esempi usando `CTRL-N` e alla pagina precedente usando `CTRL-Y`.

![esempi](./media/interactive-azure-cli/examples.png)

È anche possibile vedere un esempio specifico usando `::#`.

```azurecli
az>> vm create ::8
```
