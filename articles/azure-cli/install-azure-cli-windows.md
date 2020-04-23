---
title: Installare l'interfaccia della riga di comando di Azure per Windows
description: Come installare l'interfaccia della riga di comando di Azure in Windows
author: dbradish-microsoft
ms.author: dbradish
manager: barbkess
ms.date: 05/01/2019
ms.topic: conceptual
ms.service: azure-cli
ms.devlang: azurecli
ms.openlocfilehash: c5e9118a04b0dc608309093866307fdc7083f591
ms.sourcegitcommit: 36e02e96b955ed0531f98b9c0f623f4acb508661
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/22/2020
ms.locfileid: "82030753"
---
# <a name="install-azure-cli-on-windows"></a>Installare l'interfaccia della riga di comando di Azure in Windows

Per Windows, l'interfaccia della riga di comando di Azure viene installata tramite un file MSI che consente di accedere all'interfaccia della riga di comando tramite il prompt dei comandi di Windows (CMD) o PowerShell.
Quando si esegue l'installazione per il sottosistema Windows per Linux (WSL), sono disponibili i pacchetti per la distribuzione Linux. Per un elenco di gestori di pacchetti supportati o per sapere come eseguire manualmente l'installazione in WSL, vedere la [pagina di installazione principale](install-azure-cli.md).

[!INCLUDE [current-version](includes/current-version.md)]

## <a name="install-or-update"></a>Eseguire l'installazione o l'aggiornamento

Per l'installazione o l'aggiornamento dell'interfaccia della riga di comando di Azure in Windows viene usato il programma di installazione MSI distribuibile. Non è necessario disinstallare le versioni correnti prima di usare il programma di installazione MSI.

> [!div class="nextstepaction"]
> [Scaricare il programma di installazione MSI](https://aka.ms/installazurecliwindows)

Quando il programma di installazione chiede il permesso di apportare modifiche al computer, selezionare la casella "Yes" (Sì).

È anche possibile installare l'interfaccia della riga di comando di Azure con PowerShell. Avviare PowerShell come amministratore ed eseguire il comando seguente:

   ```PowerShell
   Invoke-WebRequest -Uri https://aka.ms/installazurecliwindows -OutFile .\AzureCLI.msi; Start-Process msiexec.exe -Wait -ArgumentList '/I AzureCLI.msi /quiet'; rm .\AzureCLI.msi
   ```
Verrà scaricata e installata l'ultima versione dell'interfaccia della riga di comando di Azure per Windows. Se è già installata una versione, verrà aggiornata. Al termine dell'installazione, sarà necessario riaprire PowerShell per usare l'interfaccia della riga di comando di Azure.

È ora possibile eseguire l'interfaccia della riga di comando di Azure con il comando `az` dal prompt dei comandi di Windows o da PowerShell. PowerShell offre alcune funzionalità di completamento con tasto TAB non disponibili dal prompt dei comandi di Windows. Per accedere, eseguire il comando [az login](/cli/azure/reference-index#az-login).

[!INCLUDE [interactive-login](includes/interactive-login.md)]

Per altre informazioni sui diversi metodi di autenticazione, vedere [Accedere con l'interfaccia della riga di comando di Azure](authenticate-azure-cli.md).

## <a name="troubleshooting"></a>Risoluzione dei problemi

Ecco alcuni problemi comuni che possono verificarsi durante l'installazione in Windows. Se si verifica un problema non trattato in questo articolo, [segnalarlo in GitHub](https://github.com/Azure/azure-cli/issues).

### <a name="proxy-blocks-connection"></a>Il proxy blocca la connessione

Se non è possibile scaricare il programma di installazione MSI perché il proxy blocca la connessione, assicurarsi che il proxy sia configurato correttamente. Per Windows 10 queste impostazioni sono gestite nel riquadro `Settings > Network & Internet > Proxy`. Contattare l'amministratore di sistema per richiedere le impostazioni necessarie o in casi in cui il computer può essere gestito tramite configurazione o richiede una configurazione avanzata.

> [!IMPORTANT]
> Queste impostazioni sono inoltre necessarie per poter accedere ai servizi di Azure con l'interfaccia della riga di comando, sia da PowerShell che dal prompt dei comandi. Per eseguire questa operazione in PowerShell, usare il comando seguente:
>
> ```powershell
> (New-Object System.Net.WebClient).Proxy.Credentials = `
>   [System.Net.CredentialCache]::DefaultNetworkCredentials
> ```

Per ottenere il file MSI, il proxy deve consentire le connessioni HTTPS agli indirizzi seguenti:

* `https://aka.ms/`
* `https://azurecliprod.blob.core.windows.net/`

## <a name="uninstall"></a>Disinstallare

[!INCLUDE [uninstall-boilerplate.md](includes/uninstall-boilerplate.md)]

È possibile disinstallare l'interfaccia della riga di comando di Azure dall'elenco "App e funzionalità" di Windows. Per eseguire la disinstallazione:

| Piattaforma | Istruzioni |
|---|---|
| Windows 10 | Start > Impostazioni > App |
| Windows 8<br/>Windows 7 | Start > Pannello di controllo -> Programmi -> Disinstalla un programma |

In questo tipo di schermata digitare __interfaccia della riga di comando di Azure__ nella barra di ricerca dei programmi. Il programma per la disinstallazione è elencato come __Interfaccia della riga di comando di Microsoft Azure 2.0__. Selezionare questa applicazione e quindi fare clic sul pulsante `Uninstall`.

## <a name="next-steps"></a>Passaggi successivi

Ora che l'interfaccia della riga di comando di Azure è stata installata, passare a una breve presentazione delle relative funzionalità e dei comandi comuni.

> [!div class="nextstepaction"]
> [Introduzione all'interfaccia della riga di comando di Azure](get-started-with-azure-cli.md)
