---
title: Eseguire la migrazione di applicazioni Spring Boot ad Azure Spring Cloud
description: Questa guida descrive gli aspetti da considerare per la migrazione di un'applicazione Spring Boot esistente da eseguire in Azure Spring Cloud.
author: yevster
ms.author: yebronsh
ms.topic: conceptual
ms.date: 5/26/2020
ms.custom: devx-track-java
ms.openlocfilehash: 4d2f84c6c77294c9a2d25028608e2feb712599f8
ms.sourcegitcommit: 4036ac08edd7fc6edf8d11527444061b0e4531ef
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/28/2020
ms.locfileid: "89062055"
---
# <a name="migrate-spring-boot-applications-to-azure-spring-cloud"></a>Eseguire la migrazione di applicazioni Spring Boot ad Azure Spring Cloud

Questa guida descrive gli aspetti da considerare per la migrazione di un'applicazione Spring Boot esistente da eseguire in Azure Spring Cloud.

## <a name="pre-migration"></a>Pre-migrazione

Per garantire una corretta migrazione, prima di iniziare completare i passaggi di valutazione e inventario descritti nelle sezioni seguenti.

Se non è possibile soddisfare questi requisiti di pre-migrazione, vedere le guide alla migrazione complementari seguenti:

* Eseguire la migrazione di applicazioni JAR eseguibili ai contenitori nel servizio Azure Kubernetes (indicazioni in pianificazione)
* Eseguire la migrazione di applicazioni JAR eseguibili alle macchine virtuali di Azure (indicazioni in pianificazione)

### <a name="inspect-application-components"></a>Esaminare i componenti dell'applicazione

[!INCLUDE [identify-local-state](includes/identify-local-state-azure-spring-cloud.md)]

[!INCLUDE [static-content-azure-spring-cloud](includes/determine-whether-and-how-the-file-system-is-used-azure-spring-cloud.md)]

#### <a name="determine-whether-any-of-the-services-contain-os-specific-code"></a>Determinare se uno dei servizi contiene codice specifico del sistema operativo

[!INCLUDE [determine-whether-your-application-contains-os-specific-code](includes/determine-whether-your-application-contains-os-specific-code-no-title.md)]

[!INCLUDE [switch-to-a-supported-platform-azure-spring-cloud](includes/switch-to-a-supported-platform-azure-spring-cloud.md)]

[!INCLUDE [identify-spring-boot-versions](includes/identify-spring-boot-versions.md)]

Per tutte le applicazioni che usano Spring Boot 1.x, seguire la [Guida alla migrazione di Spring Boot 2.0](https://github.com/spring-projects/spring-boot/wiki/Spring-Boot-2.0-Migration-Guide) per aggiornarle a una versione di Spring Boot supportata. Per le versioni supportate, vedere [Preparare un'app Java Spring per la distribuzione](/azure/spring-cloud/spring-cloud-tutorial-prepare-app-deployment#spring-boot-and-spring-cloud-versions).

[!INCLUDE [identify-logs-metrics-apm-azure-spring-cloud.md](includes/identify-logs-metrics-apm-azure-spring-cloud.md)]

### <a name="inventory-external-resources"></a>Inventario delle risorse esterne

Identificare le risorse esterne, ad esempio le origini dati, i broker dei messaggi JMS e gli URL di altri servizi. Nelle applicazioni Spring Boot la configurazione per tali risorse si trova di solito nella cartella *src/main/directory*, in un file generalmente denominato *application.properties* o *application.yml*.

[!INCLUDE [inventory-databases-spring-boot](includes/inventory-databases-spring-boot.md)]

[!INCLUDE [identify-jms-brokers-in-spring](includes/identify-jms-brokers-in-spring.md)]

Dopo aver identificato i broker in uso, trovare le impostazioni corrispondenti. Nelle applicazioni Spring Boot si trovano in genere nei file *application.properties* e *application.yml* nella directory dell'applicazione.

[!INCLUDE [jms-broker-settings-examples-in-spring](includes/jms-broker-settings-examples-in-spring.md)]

[!INCLUDE [identify-external-caches-azure-spring-cloud](includes/identify-external-caches-azure-spring-cloud.md)]

[!INCLUDE [inventory-identity-providers-spring-boot.md](includes/inventory-identity-providers-spring-boot.md)]

#### <a name="identify-any-clients-relying-on-a-non-standard-port"></a>Identificare i client che si basano su una porta non standard

Azure Spring Cloud sovrascrive l'impostazione di `server.port` nell'applicazione distribuita. Se eventuali client dipendono dalla disponibilità dell'applicazione su una porta diversa da 443, sarà necessario modificarli.

#### <a name="all-other-external-resources"></a>Tutte le altre risorse esterne

Non è possibile documentare tutte le possibili dipendenze esterne in questa guida. Dopo la migrazione, è responsabilità dell'utente verificare che sia possibile soddisfare tutte le dipendenze esterne dell'applicazione.

[!INCLUDE [inventory-configuration-sources-and-secrets-spring-boot](includes/inventory-configuration-sources-and-secrets-spring-boot.md)]

[!INCLUDE [inspect-the-deployment-architecture-spring-boot](includes/inspect-the-deployment-architecture-spring-boot.md)]

## <a name="migration"></a>Migrazione

[!INCLUDE [migrate-steps-spring-boot-azure-spring-cloud](includes/migrate-steps-spring-boot-azure-spring-cloud.md)]

## <a name="post-migration"></a>Post-migrazione

[!INCLUDE [post-migration-spring-boot-azure-spring-cloud](includes/post-migration-spring-boot-azure-spring-cloud.md)]
