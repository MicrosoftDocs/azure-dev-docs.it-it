---
title: 'Procedura dettagliata, parte 7: Autenticare le app Python con i servizi di Azure'
description: Un esame del codice per l'endpoint API dell'app principale, che usa l'endpoint API di terze parti e scrive un messaggio nell'Archiviazione code di Azure.
ms.date: 08/24/2020
ms.topic: conceptual
ms.custom: devx-track-python
ms.openlocfilehash: b6a54f51c53889ba95f86ba194232262f31c2d99
ms.sourcegitcommit: 29b161c450479e5d264473482d31e8d3bf29c7c0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/06/2020
ms.locfileid: "91764697"
---
# <a name="part-7-main-application-api-endpoint"></a>Parte 7: Endpoint API dell'applicazione principale

[Parte precedente: Codice di avvio dell'app principale](walkthrough-tutorial-authentication-06.md)

L'API */api/v1/getcode* dell'app genera una risposta JSON che contiene un codice alfanumerico e un timestamp.

In primo luogo, l'elemento Decorator `@app.route` indica a Flask che la funzione `get_code` gestisce le richieste all'URL */api/v1/getcode*.

```python
@app.route('/api/v1/getcode', methods=['GET'])
def get_code():
```

Viene quindi chiamata l'API di terze parti, il cui URL si trova in `number_url`, specificando la chiave di accesso recuperata dall'insieme di credenziali delle chiavi nell'intestazione.

```python
    headers = {
        'Content-Type': 'application/json',
        'x-functions-key': access_key
        }

    r = requests.get(url = number_url, headers = headers)

    if (r.status_code != 200):
        return "Could not get you a code.", r.status_code
```

La proprietà `x-functions-key` nell'intestazione rappresenta specificamente il modo in cui Funzioni di Azure (dove viene distribuita questa API di terze parti di esempio) prevede che una chiave di accesso venga visualizzata in un'intestazione. Per altre informazioni, vedere [Trigger HTTP di Funzioni di Azure - Chiavi di autorizzazione](/azure/azure-functions/functions-bindings-http-webhook-trigger?tabs=csharp#authorization-keys). Se per qualsiasi motivo la chiamata all'API non riesce, viene restituito un messaggio di errore e il codice di stato.

Supponendo che la chiamata API abbia esito positivo e restituisca un valore numerico, verrà creato un codice più complesso usando tale numero più alcuni caratteri casuali (usando la funzione `random_char`).

```python
    data = r.json()
    chars1 = random_char(3)
    chars2 = random_char(3)
    code_value = f"{chars1}-{data['value']}-{chars2}"
    code = { "code": code_value, "timestamp" : str(datetime.utcnow()) }
```

La variabile `code` qui contiene la risposta JSON completa per l'API dell'app, che include il valore del codice e un timestamp. Una risposta di esempio potrebbe esser: `{"code":"ojE-161-pTv","timestamp":"2020-04-15 16:54:48.816549"}`.

Prima di restituire tale risposta, tuttavia, viene scritto un messaggio nella coda di archiviazione usando il metodo [`send_message`](/python/api/azure-storage-queue/azure.storage.queue.queueclient#send-message-content----kwargs-) del client di accodamento:

```python
    queue_client.send_message(code)

    return jsonify(code)
```

## <a name="processing-queue-messages"></a>Elaborazione dei messaggi della coda

I messaggi archiviati nella coda possono essere visualizzati e gestiti tramite il [portale di Azure](/azure/storage/queues/storage-quickstart-queues-portal#view-message-properties) o con il comando dell'interfaccia della riga di comando di Azure [`az storage message get`](/cli/azure/storage/message#az-storage-message-get). Il repository di esempio include uno script (*test.cmd* e *test.sh*) per richiedere un codice all'endpoint dell'app e quindi controllare la coda di messaggi. È anche disponibile uno script per cancellare la coda usando il comando [`az storage message clear`](/cli/azure/storage/message#az-storage-message-clear).

In genere, un'app come quella di questo esempio può avere un altro processo che estrae in modo asincrono i messaggi dalla coda per un'ulteriore elaborazione. Come indicato in precedenza, la risposta generata da questo endpoint API potrebbe essere usata altrove nell'app con l'autenticazione utente a due fattori. In tal caso, l'app deve invalidare il codice dopo un determinato periodo di tempo, ad esempio 10 minuti. Un modo semplice per eseguire questa attività consiste nel mantenere una tabella di codici di autenticazione a due fattori validi, che vengono usati dalla procedura di accesso dell'utente. L'app avrà quindi un semplice processo di controllo della coda con la logica seguente (in pseudo-codice):

<pre>
pull a message from the queue and retrieve the code.

if (code is already in the table):
    remove the code from the table, thereby invalidating it
else:
    add the code to the table, making it valid
    call queue_client.send_message(code, visibility_timeout=600)
</pre>

Questo pseudo-codice usa il parametro facoltativo `visibility_timeout` del metodo [`send_message`](/python/api/azure-storage-queue/azure.storage.queue.queueclient#send-message-content----kwargs-) che specifica il numero di secondi prima che il messaggio diventi visibile nella coda. Poiché il timeout predefinito è zero, i messaggi inizialmente scritti dall'endpoint API diventano immediatamente visibili al processo di controllo della coda. Di conseguenza, il processo li archivia immediatamente nella tabella codici valida. Accodando di nuovo lo stesso messaggio con il timeout, il processo sa che riceverà nuovamente il codice 10 minuti dopo, a quel punto lo rimuoverà dalla tabella.

## <a name="implementing-the-main-app-api-endpoint-in-azure-functions"></a>Implementazione dell'endpoint API dell'app principale in Funzioni di Azure

Il codice illustrato in precedenza in questo articolo usa il framework Web Flask per creare il relativo endpoint API. Poiché Flask deve essere eseguito con un server Web, è necessario distribuire tale codice nel Servizio app di Azure o in una macchina virtuale.

Un'opzione di distribuzione alternativa è l'ambiente serverless di Funzioni di Azure. In questo caso, tutto il codice di avvio e il codice dell'endpoint API sarebbero contenuti all'interno della stessa funzione associata a un trigger HTTP. Come nel Servizio app, è possibile usare le [impostazioni dell'applicazione per le funzioni](/azure/azure-functions/functions-how-to-use-azure-function-app-settings#settings) per creare le variabili di ambiente per il codice.

Una parte dell'implementazione che diventa più semplice è l'autenticazione con l'Archiviazione code. Anziché ottenere un oggetto `QueueClient` usando l'URL della coda e un oggetto credenziale, si crea un *binding di archiviazione delle code* per la funzione. Il binding gestisce tutte le autenticazioni in background. Con tale binding, alla funzione viene assegnato un oggetto client pronto per l'uso come parametro. Per altre informazioni e per il codice di esempio, vedere [Connettere Funzioni di Azure all'Archiviazione code di Azure](/azure/azure-functions/functions-add-output-binding-storage-queue-cli?tabs=bash%2Cbrowser&pivots=programming-language-python).

## <a name="next-steps"></a>Passaggi successivi

Con questo esempio si è appreso come le app eseguono l'autenticazione con altri servizi di Azure e come le app possono usare Azure Key Vault per archiviare eventuali altri segreti necessari per le API di terze parti.

Lo stesso modello illustrato qui con Azure Key Vault e Archiviazione di Azure si applica a tutti gli altri servizi di Azure. Il passaggio cruciale consiste nell'impostare le autorizzazioni per i ruoli corrette per l'app all'interno della pagina del servizio nel portale di Azure o tramite l'interfaccia della riga di comando di Azure. Vedere [Come assegnare le autorizzazioni per i ruoli](/azure/role-based-access-control/role-assignments-steps). Assicurarsi di controllare la documentazione del servizio per sapere se è necessario configurare altri criteri di accesso.

Tenere sempre presente che è necessario assegnare gli stessi ruoli e criteri di accesso a qualsiasi entità servizio usata per lo sviluppo locale.

In breve, dopo aver completato questa procedura dettagliata, è possibile applicare le proprie conoscenze a qualsiasi altro servizi di Azure e a qualsiasi altro servizio esterno.

Un argomento che non è stato toccato qui è l'autenticazione degli *utenti*. Per esplorare quest'area per le app Web, iniziare con [Autenticare e autorizzare gli utenti end-to-end nel Servizio app di Azure](/azure/app-service/tutorial-auth-aad?pivots=platform-linux).

## <a name="see-also"></a>Vedere anche

- [Come autenticare e autorizzare le app Python in Azure](azure-sdk-authenticate.md)
- Esempio di procedura dettagliata: [github.com/Azure-Samples/python-integrated-authentication](https://github.com/Azure-Samples/python-integrated-authentication)
- [Documentazione di Azure Active Directory](/azure/active-directory)
- [Documentazione di Azure Key Vault](/azure/key-vault)
