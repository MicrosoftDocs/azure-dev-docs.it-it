---
title: Assegnare le autorizzazioni del ruolo a un'identità dell'app o a un'entità servizio
description: Come concedere le autorizzazioni a un'entità servizio o a un'identità dell'app usando l'interfaccia della riga di comando di Azure
ms.date: 05/12/2020
ms.topic: conceptual
ms.custom: devx-track-python, devx-track-azurecli
ms.openlocfilehash: facfa1663e6f62a7458f99ee20c86f66ee67b17d
ms.sourcegitcommit: 800c5e05ad3c0b899295d381964dd3d47436ff90
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/19/2020
ms.locfileid: "88614488"
---
# <a name="how-to-assign-role-permissions-to-an-app-identity-or-service-principal"></a>Come assegnare le autorizzazioni del ruolo a un'identità dell'app o a un'entità servizio

Il sistema di controllo degli accessi in base al ruolo di Azure gestisce autorizzazioni specifiche per un'ampia gamma di risorse. Un *ruolo* è essenzialmente una raccolta di autorizzazioni correlate generalmente necessarie insieme. Per abilitare le autorizzazioni, assegnare un ruolo a un'*entità di sicurezza* (un utente, un gruppo, un'entità servizio o un'identità dell'app) con un *ambito* specifico a cui si applica tale ruolo.

In pratica, assegnare sempre solo i ruoli che un'entità di sicurezza necessita effettivamente nell'ambito più specifico. Evitare di assegnare ruoli più ampi in ambiti più ampi anche se inizialmente sembra più pratico. Limitando i ruoli e gli ambiti, si limitano le risorse a rischio se l'entità di sicurezza viene compromessa, ovvero se le credenziali per tale entità sono esposte a una violazione dei dati o ad altri incidenti di sicurezza.

Poiché si usano entità di sicurezza diverse nello sviluppo e nella produzione, è necessario ripetere le assegnazioni di ruolo in ogni ambiente. Ovvero, durante lo sviluppo vengo in genere assegnati i ruoli all'entità servizio locale creata nella workstation (vedere [Configurare l'ambiente di sviluppo Python locale - Autenticazione](configure-local-development-environment.md#configure-authentication)). Nell'ambiente di produzione i ruoli vengono assegnati all'entità servizio o all'identità gestita dell'applicazione prima della distribuzione per assicurarsi che l'applicazione abbia accesso all'avvio. Per altre informazioni, vedere [Autenticazione - Identità dell'app durante l'esecuzione in Azure](azure-sdk-authenticate.md#identity-when-running-the-app-on-azure).

Per altre informazioni sul controllo degli accessi in base al ruolo in generale, vedere [Che cos'è il controllo degli accessi in base al ruolo di Azure?](/azure/role-based-access-control/overview).

## <a name="role-assignment-process"></a>Processo di assegnazione di ruolo

L'assegnazione di un ruolo prevede tre passaggi:

1. [Individuare il ruolo appropriato](#find-the-roles-you-need) per il tipo di risorsa in questione e le operazioni che si vuole autorizzare.

1. Identificare l'ambito necessario per il ruolo in questione, che descrive la portata delle risorse per le quali sono autorizzate le operazioni.

1. Assegnare il ruolo a un'entità di sicurezza.

Il passaggio 1 è identico sia per il portale di Azure che per l'interfaccia della riga di comando di Azure. I passaggi 2 e 3 sono diversi tra il portale e l'interfaccia della riga di comando e sono quindi combinati nelle sezioni che seguono: [Identificare l'ambito e assegnare un ruolo nel portale di Azure](#azure-portal) e [Identificare l'ambito e assegnare un ruolo tramite l'interfaccia della riga di comando di Azure](#azure-cli).

> [!NOTE]
> Se l'account utente non ha le autorizzazioni per assegnare un ruolo all'interno della sottoscrizione, verrà visualizzato un messaggio di errore che segnala che l'account non è autorizzato a eseguire l'azione 'Microsoft.Authorization/roleAssignments/write'. In questo caso, contattare gli amministratori della sottoscrizione in quanto possono assegnare le autorizzazioni per conto dell'utente.

## <a name="find-the-roles-you-need"></a>Individuare i ruoli necessari

1. Iniziare con l'articolo completo [Ruoli predefiniti di Azure](/azure/role-based-access-control/built-in-roles). La tabella all'inizio dell'articolo è un indice dei dettagli descritti più avanti nell'articolo.

1. In tale articolo passare alla categoria di servizio (calcolo, archiviazione, database e così via) per la risorsa a cui si vuole concedere le autorizzazioni. Il modo più semplice per trovare ciò che si sta cercando è in genere cercare nella pagina una parola chiave pertinente, ad esempio "blob", "macchina virtuale" e così via.

1. Esaminare i ruoli elencati per la categoria di servizio e identificare le operazioni specifiche necessarie. Ancora una volta, iniziare sempre con il ruolo più restrittivo.

    Se, ad esempio, un'entità di sicurezza deve leggere i BLOB in un account di archiviazione di Azure, ma non richiede l'accesso in scrittura, scegliere "Lettore dei dati dei BLOB di archiviazione" anziché "Collaboratore ai dati dei BLOB di archiviazione" (e sicuramente non il ruolo "Proprietario dei dati dei BLOB di archiviazione" a livello di amministratore). È sempre possibile aggiornare le assegnazioni di ruolo in un secondo momento, se necessario.

    Se l'entità di sicurezza deve anche accedere all'archiviazione code, è possibile usare ruoli quali "Lettore dei dati della coda di archiviazione" e "Collaboratore ai dati della coda di archiviazione". Anche in questo caso, cercare di essere quanto più specifici possibile anziché assegnare un ruolo ampio come "Lettore e accesso ai dati", che fornisce l'accesso alle chiavi dell'account tramite cui l'entità può accedere a qualsiasi elemento nell'intero account di archiviazione.

1. Se non si trova un ruolo appropriato, è possibile creare [ruoli personalizzati](/azure/role-based-access-control/custom-roles).

## <a name="identify-scope-and-assign-a-role-on-the-azure-portal"></a><a name="azure-portal"></a>Identificare l'ambito e assegnare un ruolo nel portale di Azure

1. Nel [portale di Azure](https://portal.azure.com) passare alla risorsa a cui si vuole assegnare un ruolo. L'ambito della risorsa determina l'ambito dell'assegnazione.

    Ad esempio, se si passa a un account di archiviazione, qualsiasi assegnazione di ruolo si applica all'intero account di archiviazione. Se si passa a un contenitore BLOB specifico all'interno di tale account di archiviazione, l'assegnazione di ruolo si applica solo a tale contenitore.

1. Selezionare il pannello **Controllo di accesso (IAM)** (IAM sta per "gestione delle identità e degli accessi").

1. All'interno di tale pannello è presente la sezione **Assegnazioni di ruolo**, in cui è possibile aggiungere e rimuovere ruoli assegnati a qualsiasi entità di sicurezza.

Per i dettagli completi e una procedura dettagliata dell'interfaccia utente, vedere [Aggiungere o rimuovere assegnazioni di ruolo di Azure](/azure/role-based-access-control/role-assignments-portal) nella documentazione relativa al controllo degli accessi in base al ruolo di Azure.

## <a name="identify-scope-and-assign-a-role-through-the-azure-cli"></a><a name="azure-cli"></a>Identificare l'ambito e assegnare un ruolo tramite l'interfaccia della riga di comando di Azure

L'assegnazione di ruolo con l'interfaccia della riga di comando di Azure usa il comando [`az role assignment`](/cli/azure/role/assignment?view=azure-cli-latest). Usare `az role assignment create` per aggiungere un'assegnazione e `az role assignment delete` per rimuoverla.

Sebbene il processo completo sia descritto in [Aggiungere o rimuovere assegnazioni di ruolo di Azure tramite l'interfaccia della riga di comando di Azure](/azure/role-based-access-control/role-assignments-cli), il riepilogo seguente fornisce esempi specifici relativi ad altri articoli in questo Centro per sviluppatori.

Il comando `az role assignment create` ha la sintassi seguente:

```azurecli
az role assignment create --assignee <assignee> --role <role> --scope <scope>
```

- `<assignee>` identifica l'entità di sicurezza. Per le entità servizio come quella usata durante lo sviluppo locale, l'assegnatario corrisponde all'ID client di tale entità. Per le applicazioni distribuite nel cloud, l'assegnatario corrisponde al nome dell'applicazione.
- `<role>` è il nome del ruolo da assegnare, ad esempio "Collaboratore ai dati dei BLOB di archiviazione" o il relativo GUID, ad esempio "ba92f5b4-2d11-453d-a403-e96b0029c9fe".
- `<scope>` è una stringa potenzialmente lunga che identifica l'ambito esatto dell'assegnazione.

L'ambito è costituito da una serie di identificatori separati dal carattere /. Questa stringa può essere considerata come espressione della seguente gerarchia, in cui il testo senza segnaposto (`<>`) è rappresentato da identificatori fissi:

<pre>
/subscriptions
  /&lt;subscription_id&gt;
    /resourcegroups
      /&lt;resource_group_name&gt;
        /providers
          /&lt;provider_name&gt;
            /&lt;resource_type&gt;
              /&lt;resource_sub_type_1&gt;
                /&lt;resource_sub_type_2&gt;
                  /&lt;resource_name&gt;
</pre>

- `<subscription_id>` è l'ID della sottoscrizione da usare (GUID).
- `<resources_group_name>` è il nome del gruppo di risorse che contiene la risorsa.
- `<provider_name>` è il nome del servizio che gestisce la risorsa, quindi `<resource_type>` e `<resource_sub_type_*>` identificano ulteriori livelli all'interno di tale servizio.
  
    Questi nomi e tipi sono disponibili nell'articolo [Ruoli predefiniti di Azure](/azure/role-based-access-control/built-in-roles). Individuare e selezionare il ruolo nella tabella in alto per passare alla descrizione specifica del ruolo. Sono disponibili le stringhe che contengono il nome del provider, il tipo di risorsa e i sottotipi di risorsa. Ad esempio, con il ruolo "Collaboratore ai dati dei BLOB di archiviazione" viene visualizzato "Microsoft.Storage/storageAccounts/blobServices/containers/". Per il "Ruolo Lettore dell'account Cosmos DB" viene visualizzato "Microsoft.DocumentDB/databaseAccounts/readonlykeys", che ha un solo sottotipo.

- `<resource_name>` è l'ultima parte della stringa che identifica una risorsa specifica.

L'ambito più ampio (meno specifico) è `/subscriptions/<subscription_id>`, che applica un'assegnazione nell'intera sottoscrizione. Ogni livello aggiuntivo di gerarchia rende più specifico l'ambito.

### <a name="examples"></a>Esempi

Gli esempi seguenti presuppongono le condizioni seguenti (vedere [Esempio: Effettuare il provisioning e usare Archiviazione di Azure](azure-sdk-example-storage.md)):

- L'ID sottoscrizione di Azure è contenuto in una variabile di ambiente denominata `AZURE_SUBSCRIPTION_ID`.
- L'ID client dell'entità servizio a cui si vuole assegnare un ruolo è contenuto in una variabile di ambiente denominata `AZURE_CLIENT_ID`.
- Nella sottoscrizione è presente un gruppo di risorse denominato "PythonAzureExample-Storage-rg".
- Il gruppo di risorse contiene un account di archiviazione di Azure denominato "pythonazurestorage-12345".
- Si dispone di un contenitore BLOB in tale account di archiviazione denominato "blob-container-01".
- Si vuole assegnare il ruolo "Collaboratore ai dati dei BLOB di archiviazione" all'entità servizio.

> [!TIP]
> La propagazione delle modifiche nelle assegnazioni di ruolo all'interno di Azure può richiedere un minuto. Di conseguenza, è possibile che il codice funzioni ancora per un breve periodo di tempo dopo la rimozione di un'autorizzazione. Se viene visualizzato un comportamento imprevisto, attendere un paio di minuti e riprovare.

#### <a name="grant-permissions-for-the-specific-container-only"></a>Concedere le autorizzazioni solo per il contenitore specifico

# <a name="cmd"></a>[cmd](#tab/cmd)

```azurecli
az role assignment create --assignee %AZURE_CLIENT_ID% ^
    --role "Storage Blob Data Contributor" ^
    --scope "/subscriptions/%AZURE_SUBSCRIPTION_ID%/resourceGroups/PythonAzureExample-Storage-rg/providers/Microsoft.Storage/storageAccounts/pythonazurestorage12345/blobServices/default/containers/blob-container-01"
```

# <a name="bash"></a>[Bash](#tab/bash)

```azurecli
az role assignment create --assignee $AZURE_CLIENT_ID \
    --role "Storage Blob Data Contributor" \
    --scope "/subscriptions/$AZURE_SUBSCRIPTION_ID/resourceGroups/PythonAzureExample-Storage-rg/providers/Microsoft.Storage/storageAccounts/pythonazurestorage12345/blobServices/default/containers/blob-container-01"
```

---

#### <a name="grant-permissions-for-all-blob-containers-in-the-storage-account"></a>Concedere le autorizzazioni per tutti i contenitori BLOB nell'account di archiviazione

# <a name="cmd"></a>[cmd](#tab/cmd)

```azurecli
az role assignment create --assignee %AZURE_CLIENT_ID% ^
    --role "Storage Blob Data Contributor" ^
    --scope "/subscriptions/%AZURE_SUBSCRIPTION_ID%/resourceGroups/PythonAzureExample-Storage-rg/providers/Microsoft.Storage/storageAccounts/pythonazurestorage12345"
```

# <a name="bash"></a>[Bash](#tab/bash)

```azurecli
az role assignment create --assignee $AZURE_CLIENT_ID \
    --role "Storage Blob Data Contributor" \
    --scope "/subscriptions/$AZURE_SUBSCRIPTION_ID/resourceGroups/PythonAzureExample-Storage-rg/providers/Microsoft.Storage/storageAccounts/pythonazurestorage12345"
```

---

#### <a name="grant-permissions-for-all-blob-containers-in-the-resource-group"></a>Concedere le autorizzazioni per tutti i contenitori BLOB nel gruppo di risorse

# <a name="cmd"></a>[cmd](#tab/cmd)

```azurecli
az role assignment create --assignee %AZURE_CLIENT_ID% ^
    --role "Storage Blob Data Contributor" ^
    --scope "/subscriptions/%AZURE_SUBSCRIPTION_ID%/resourceGroups/PythonAzureExample-Storage-rg"
```

In alternativa, è possibile specificare il gruppo di risorse con il parametro `--resource-group`:

```azurecli
az role assignment create --assignee %AZURE_CLIENT_ID% ^
    --role "Storage Blob Data Contributor" ^
    --resource-group "PythonAzureExample-Storage-rg"
```

# <a name="bash"></a>[Bash](#tab/bash)

```azurecli
az role assignment create --assignee $AZURE_CLIENT_ID \
    --role "Storage Blob Data Contributor" \
    --scope "/subscriptions/$AZURE_SUBSCRIPTION_ID/resourceGroups/PythonAzureExample-Storage-rg"
```

In alternativa, è possibile specificare il gruppo di risorse con il parametro `--resource-group`:

```azurecli
az role assignment create --assignee $AZURE_CLIENT_ID \
    --role "Storage Blob Data Contributor" \
    --resource-group "PythonAzureExample-Storage-rg"
```

---

#### <a name="grant-permissions-to-all-blob-containers-in-the-subscription"></a>Concedere le autorizzazioni a tutti i contenitori BLOB nella sottoscrizione

# <a name="cmd"></a>[cmd](#tab/cmd)

```azurecli
az role assignment create --assignee %AZURE_CLIENT_ID% ^
    --role "Storage Blob Data Contributor" ^
    --scope "/subscriptions/%AZURE_SUBSCRIPTION_ID%"
```

# <a name="bash"></a>[Bash](#tab/bash)

```azurecli
az role assignment create --assignee $AZURE_CLIENT_ID \
    --role "Storage Blob Data Contributor" \
    --scope "/subscriptions/$AZURE_SUBSCRIPTION_ID"
```

---
