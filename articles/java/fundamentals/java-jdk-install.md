---
title: Installare il JDK Azul Zulu per Azure e Azure Stack
description: Come installare i JDK (Java Development Kit) Azul Zulu per lo sviluppo di Azure con Windows, Linux e Mac
ms.date: 04/19/2019
ms.topic: conceptual
ms.openlocfilehash: f5425bd94a0094fb16649bd3a8069599e30cdba7
ms.sourcegitcommit: 0af39ee9ff27c37ceeeb28ea9d51e32995989591
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/21/2020
ms.locfileid: "81670697"
---
# <a name="install-the-jdk-for-azure-and-azure-stack"></a>Installare JDK per Azure e Azure Stack

Le build di JDK Azul Zulu per Azure - Enterprise Edition sono distribuzioni di OpenJDK gratuite, multipiattaforma e pronte per la produzione per Azure e Azure Stack, supportate da Microsoft e Azul Systems. Contengono tutti i componenti necessari per compilare ed eseguire applicazioni Java SE.

Per ogni [sistema operativo client sono supportati più tipi di pacchetti di download](https://www.azul.com/downloads/azure-only/zulu/). È anche possibile [ottenere immagini di macchine virtuali dalla raccolta di Azure Marketplace](#get-virtual-machine-images-from-the-azure-marketplace-gallery).

> [!NOTE]
> Le istruzioni seguenti riguardano la versione Java 8 a 64 bit del JDK. Azul fornisce anche l'ambiente JRE (Java Run-time Environment) come installazione autonoma. Il JRE è incluso nell'installazione del JDK.
>
> I pacchetti di Java 11 sono disponibili anche nella [pagina di download Azure di Azul](https://www.azul.com/downloads/azure-only/zulu/).

## <a name="download-and-install-the-azul-zulu-for-azure---enterprise-edition-jdk-builds-for-windows"></a>Scaricare e installare le build di JDK Azul Zulu per Azure - Enterprise Edition per Windows

1. Scaricare Azul Zulu JDK 8 a 64 bit come file MSI:

   * [zulu-8-azure-jdk_8.44.0.11-8.0.242-win_x64.msi](http://repos.azul.com/azure-only/zulu/packages/zulu-8/8u242/zulu-8-azure-jdk_8.44.0.11-8.0.242-win_x64.msi)
   * In alternativa, cercare una [versione successiva per Zulu 8](http://repos.azul.com/azure-only/zulu/packages/zulu-8) del file *MSI*.

   Salvare il file in un percorso del client, ad esempio `C:\Users\<your_login>\Downloads`. I pacchetti ZIP sono disponibili anche nella [pagina di download Azure di Azul](https://www.azul.com/downloads/azure-only/zulu/).

2. Passare alla directory e fare doppio clic sul file MSI scaricato per avviare l'installazione.

## <a name="download-and-install-the-azul-zulu-for-azure---enterprise-edition-jdk-builds-for-mac"></a>Scaricare e installare le build di JDK Azul Zulu per Azure - Enterprise Edition per Mac

Per scaricare un file ZIP per Mac, seguire questa procedura. È anche disponibile una versione DMG.

1. Scaricare Azul Zulu JDK 8 a 64 bit come file ZIP:

   * [zulu-8-azure-jdk_8.44.0.11-8.0.242-macosx_x64.zip](http://repos.azul.com/azure-only/zulu/packages/zulu-8/8u242/zulu-8-azure-jdk_8.44.0.11-8.0.242-macosx_x64.zip)
   * In alternativa, cercare una [versione successiva per Zulu 8](http://repos.azul.com/azure-only/zulu/packages/zulu-8) del file *ZIP*.

   Salvare il file in un percorso nel client, ad esempio `/Library/Java/JavaVirtualMachines/`. I pacchetti DMG sono disponibili anche nella [pagina di download Azure di Azul](https://www.azul.com/downloads/azure-only/zulu/).

2. Avviare il Finder, passare alla directory di download e fare doppio clic sul file ZIP. In alternativa, è possibile avviare una finestra di comando del terminale, passare alla directory ed eseguire:

    ```cli
    unzip <name_of_zulu_package>.zip
    ```

## <a name="download-and-install-the-azul-zulu-for-azure---enterprise-edition-jdk-builds-for-alpine-linux"></a>Scaricare e installare le build di JDK Azul Zulu per Azure - Enterprise Edition per Alpine Linux

1. Scaricare Azul Zulu JDK 8 a 64 bit come file TAR

   * [zulu-8-azure-jdk_8.44.0.11-8.0.242-linux_x64.tar.gz](http://repos.azul.com/azure-only/zulu/packages/zulu-8/8u242/zulu-8-azure-jdk_8.44.0.11-8.0.242-linux_x64.tar.gz)
   * In alternativa, cercare una [versione successiva per Zulu 8](https://repos.azul.com/azure-only/zulu/packages/zulu-8) del file con estensione *tar.gz*.

   Salvare il file in un percorso del client, ad esempio `/usr/lib/jvm`. I pacchetti RPM e DEB sono disponibili anche nella [pagina di download Azure di Azul](https://www.azul.com/downloads/azure-only/zulu/).

2. Passare alla directory ed eseguire il comando seguente per decomprimere ed espandere il file:

    ```cli
    tar -xvf <name_of_zulu_package>.tar
    ```

## <a name="confirm-your-installation"></a>Verificare l'installazione

Per verificare l'installazione, eseguire `java -version` dalla riga di comando.

L'output del comando dovrebbe essere simile a quello dell'esempio seguente:

```cli
$ java -version

openjdk version "1.8.0_242"
OpenJDK Runtime Environment (Zulu 8.44.0.11-linux64)-Microsoft-Azure-restricted (build 1.8.0_242-b20)
OpenJDK 64-Bit Server VM (Zulu 8.44.0.11-linux64)-Microsoft-Azure-restricted (build 25.242-b20, mixed mode)
```

## <a name="get-virtual-machine-images-from-the-azure-marketplace-gallery"></a>Ottenere immagini di macchine virtuali dalla raccolta di Azure Marketplace

È possibile ottenere un'immagine di macchina virtuale per le piattaforme seguenti:

* [Azul Zulu: Java 8 in Ubuntu 18.04](https://azuremarketplace.microsoft.com/marketplace/apps/azul.azul-zulu8-ubuntu-1804)
* [Azul Zulu: Java 8 in Windows Server 2019](https://azuremarketplace.microsoft.com/marketplace/apps/azul.azul-zulu8-windows-2019)
* [Azul Zulu: Java 11 in Ubuntu 18.04](https://azuremarketplace.microsoft.com/marketplace/apps/azul.azul-zulu11-ubuntu-1804)
* [Azul Zulu: Java 11 in Windows Server 2019](https://azuremarketplace.microsoft.com/marketplace/apps/azul.azul-zulu11-windows-2019)

## <a name="download-and-install-the-azul-zulu-for-azure---enterprise-edition-jdks-from-a-yum-repository"></a>Scaricare e installare i JDK Azul Zulu per Azure - Enterprise Edition da un repository Yum

I JDK Azul Zulu vengono forniti in un [repository Yum](https://repos.azul.com/azure-only/zulu-azure.repo) da Azul.

**Per installare il JDK Azul Zulu per Java 8, eseguire i comandi seguenti dall'interfaccia della riga di comando:**

```cli
sudo rpm --import http://repos.azul.com/azul-repo.key
sudo curl http://repos.azul.com/azure-only/zulu-azure.repo -o /etc/yum.repos.d/zulu-azure.repo
sudo yum -q -y update
sudo yum -q -y install zulu-8-azure-jdk
```

Per Java 11, eseguire:

```cli
sudo rpm --import http://repos.azul.com/azul-repo.key
sudo curl http://repos.azul.com/azure-only/zulu-azure.repo -o /etc/yum.repos.d/zulu-azure.repo
sudo yum -q -y update
sudo yum -q -y install zulu-11-azure-jdk
```

Per Java 13 (anteprima), eseguire:

```cli
sudo rpm --import http://repos.azul.com/azul-repo.key
sudo curl http://repos.azul.com/azure-only/zulu-azure.repo -o /etc/yum.repos.d/zulu-azure.repo
sudo yum -q -y update
sudo yum -q -y install zulu-13-azure-jdk
```

**Per aggiornare un pacchetto JDK 8 Zulu da un repository Yum:**

```cli
sudo yum -q -y install zulu-8-azure-jdk
```

Se si usa una versione diversa, cambiare il numero di versione nel comando qui sopra.

**Per rimuovere un pacchetto JDK 8 Zulu da un repository Yum:**

```cli
sudo yum -y erase zulu-8-azure-jdk
```

Se si usa una versione diversa, cambiare il numero di versione nel comando qui sopra.

## <a name="download-and-install-the-azul-zulu-jdks-from-an-apt-get-repository"></a>Scaricare e installare i JDK Azul Zulu da un repository apt-get

I JDK Azul Zulu vengono anche forniti in un [repository apt-get](https://repos.azul.com/azure-only/zulu/apt) da Azul.

**Per installare il JDK Azul Zulu per Java 8 con apt-get, eseguire i comandi seguenti dall'interfaccia della riga di comando:**

```cli
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 0xB1998361219BD9C9
sudo apt-add-repository "deb http://repos.azul.com/azure-only/zulu/apt stable main"
sudo apt-get -q update
sudo apt-get -y install zulu-8-azure-jdk
```

Per Java 11, eseguire:

```cli
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 0xB1998361219BD9C9
sudo apt-add-repository "deb http://repos.azul.com/azure-only/zulu/apt stable main"
sudo apt-get -q update
sudo apt-get -y install zulu-11-azure-jdk
```

Per Java 13 (anteprima), eseguire:

```cli
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 0xB1998361219BD9C9
sudo apt-add-repository "deb http://repos.azul.com/azure-only/zulu/apt stable main"
sudo apt-get -q update
sudo apt-get -y install zulu-13-azure-jdk
```

**Per aggiornare un pacchetto JDK 8 Zulu da un repository apt-get:**

```cli
sudo apt-get -q update
sudo apt-get -y install zulu-8-azure-jdk
```

La versione precedente verrà rimossa automaticamente.
Se si usa una versione diversa, cambiare il numero di versione nel comando qui sopra.

**Per rimuovere un pacchetto JDK 8 Zulu da un repository apt-get:**

```cli
sudo apt-get -y purge zulu-8-azure-jdk
```

Se si usa una versione diversa, cambiare il numero di versione nel comando qui sopra.

Per le istruzioni più dettagliate su come preparare, installare e gestire i JDK Azul Zulu per lo sviluppo Azure, vedere la [documentazione ufficiale di Zulu](https://docs.azul.com/zulu/zuludocs/index.htm).
