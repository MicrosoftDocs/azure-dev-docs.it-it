---
title: Gestire un pool elastico di database SQL con Java | Microsoft Docs
description: Codice di esempio per creare e configurare database SQL di Azure con Azure SDK per Java
author: rloutlaw
ms.assetid: 9b461de8-46bc-4650-8e9e-59531f4e2a53
ms.topic: article
ms.date: 3/30/2017
ms.reviewer: asirveda
ms.openlocfilehash: 1bc80d0f4c6ad0beff86bfa22fec59b3389ced03
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/05/2020
ms.locfileid: "82105032"
---
# <a name="manage-azure-sql-databases-in-elastic-pools-from-your-java-applications"></a>Gestire database SQL di Azure nei pool elastici dalle applicazioni Java

[Questo esempio](https://github.com/Azure-Samples/sql-database-java-manage-sql-dbs-in-elastic-pool) crea un server di database SQL con un [pool elastico](/azure/sql-database/sql-database-elastic-pool) per gestire e ridimensionare le risorse per più database in un singolo piano.

## <a name="run-the-sample"></a>Eseguire l'esempio

Creare un [file di autenticazione](https://docs.microsoft.com/azure/java/java-sdk-azure-authenticate#mgmt-file) e impostare una variabile di ambiente `AZURE_AUTH_LOCATION` con il percorso completo del file nel computer. Eseguire quindi:

```
git clone https://github.com/Azure-Samples/sql-database-java-manage-sql-dbs-in-elastic-pool
cd sql-database-java-manage-sql-dbs-in-elastic-pool
mvn clean compile exec:java
```

[Visualizzare l'esempio di codice completo in GitHub](https://github.com/Azure-Samples/sql-database-java-manage-sql-dbs-in-elastic-pool)

## <a name="authenticate-with-azure"></a>Eseguire l'autenticazione con Azure

[!INCLUDE [auth-include](includes/java-auth-include.md)]

## <a name="create-a-sql-database-server-with-an-elastic-pool"></a>Creare un server di database SQL con un pool elastico

```java
SqlServer sqlServer = azure.sqlServers().define(sqlServerName)
                    .withRegion(Region.US_EAST)
                    .withNewResourceGroup(rgName)
                    .withAdministratorLogin(administratorLogin)
                    .withAdministratorPassword(administratorPassword)
                    // Use ElasticPoolEditions.STANDARD as the edition
                    // and create two databases
                    .withNewElasticPool(elasticPoolName, ElasticPoolEditions.STANDARD, 
                        database1Name, database2Name)
                    .create();
```

Vedere le [informazioni di riferimento sulla classe ElasticPoolEditions](/java/api/com.microsoft.azure.management.sql.elasticpooleditions) per i valori dell'edizione corrente. Vedere la [documentazione dei pool elastici di database SQL](/azure/sql-database/sql-database-elastic-pool) per confrontare le caratteristiche delle risorse delle edizioni. 

## <a name="change-database-transaction-unit-dtu-settings-in-an-elastic-pool"></a>Modificare le impostazioni dell'unità di transazione di database (DTU) in un pool elastico

```java
// Set an pool of 200 eDTUs to share between the databases
// and ensure that a database always has 10 DTUs available but never uses more than 50
elasticPool = elasticPool.update()
                    .withDtu(200)
                    .withDatabaseDtuMin(10)
                    .withDatabaseDtuMax(50)
                    .apply();
```

Vedere la [documentazione relativa a DTU ed eDTU](/azure/sql-database/sql-database-what-is-a-dtu) per altre informazioni sull'allocazione delle risorse ai database SQL.

## <a name="create-a-new-database-and-add-it-to-an-elastic-pool"></a>Creare un nuovo database e aggiungerlo a un pool elastico

```java
// Add a newly created database to the elastic pool
SqlDatabase anotherDatabase = sqlServer.databases().define(anotherDatabaseName).create();
elasticPool.update().withExistingDatabase(anotherDatabase).apply();            
```

L'API crea `anotherDatabase` al [livello S0](/azure/sql-database/sql-database-service-tiers) nella prima istruzione. Spostando `anotherDatabase` nel pool elastico, le risorse di database vengono assegnate in base alle impostazioni del pool.

## <a name="remove-a-database-from-an-elastic-pool"></a>Rimuovere un database da un pool elastico
```java
// Assign an edition since the database resources are no longer managed in the pool 
anotherDatabase = anotherDatabase.update()
                     .withoutElasticPool()
                     .withEdition(DatabaseEditions.STANDARD)
                     .apply();
```

Vedere le [informazioni di riferimento sulla classe DatabaseEditions](/java/api/com.microsoft.azure.management.sql.databaseeditions) per i valori da passare a `withEdition()`.

## <a name="list-current-database-activities-in-an-elastic-pool"></a>Elencare le attività di database correnti in un pool elastico
```java
// get a list of in-flight elastic operations on databases in the pool and log them 
for (ElasticPoolDatabaseActivity databaseActivity : elasticPool.listDatabaseActivities()) {
    System.out.println("Database " + databaseActivity.databaseName() 
        + " performing operation " + databaseActivity.operation() + 
        " with objective " + databaseActivity.requestedServiceObjective());
}
```

Le attività di database includono lo spostamento di un database all'interno o all'esterno di un pool elastico e l'aggiornamento dei database in un pool elastico.


## <a name="list-databases-in-an-elastic-pool"></a>Elencare i database in un pool elastico
```java
// Log a list of databases in the elastic pool 
for (SqlDatabase databaseInServer : elasticPool.listDatabases()) {
    System.out.println(databaseInServer.name());
}
```

Esaminare i metodi in [com.microsoft.azure.management.sql.SqlDatabase](/java/api/com.microsoft.azure.management.sql.sqldatabase) per eseguire query sui database in modo più dettagliato.

## <a name="delete-an-elastic-pool"></a>Eliminare un pool elastico
```java
sqlServer.elasticPools().delete(elasticPoolName);
```

Il pool elastico deve essere vuoto prima di eliminarlo.

## <a name="sample-code-summary"></a>Riepilogo del codice di esempio

L'esempio crea un server SQL con due database gestiti in un singolo pool elastico. I limiti delle risorse del pool elastico vengono aggiornati e quindi vengono aggiunti altri database al pool. Il codice legge quindi la configurazione e lo stato del pool elastico. 

L'esempio elimina tutte le risorse create prima di uscire.

| Classe usata nell'esempio | Note |
|-------|-------|
| [SqlServer](/java/api/com.microsoft.azure.management.sql.sqlserver) | Server di database SQL in Azure creato dalla catena Fluent `azure.sqlServers().define()...create()`. Fornisce metodi per creare e usare pool elastici e database. 
| [SqlDatabase](/java/api/com.microsoft.azure.management.sql.sqldatabase) | Oggetto lato client che rappresenta un database SQL. Viene creato tramite `sqlServer().define()...create()`. 
| [DatabaseEditions](/java/api/com.microsoft.azure.management.sql.databaseeditions) | Campi statici costanti usati per impostare le risorse di database quando si crea un database fuori da un pool elastico o quando si sposta un database all'esterno di un pool elastico  
| [SqlElasticPool](/java/api/com.microsoft.azure.management.sql.sqlelasticpool) | Creato dalla sezione `withNewElasticPool()` della catena Fluent che ha creato l'oggetto SqlServer in Azure. Fornisce metodi per impostare i limiti delle risorse per i database in esecuzione nel pool elastico e per il pool elastico stesso. 
| [ElasticPoolEditions](/java/api/com.microsoft.azure.management.sql.elasticpooleditions) | Classe di campi costanti che definisce le risorse disponibili per un pool elastico. Vedere la [documentazione dei pool elastici di database SQL](/azure/sql-database/sql-database-elastic-pool) per i dettagli sui livelli. 
| [ElasticPoolDatabaseActivity](/java/api/com.microsoft.azure.management.sql.elasticpooldatabaseactivity) | Recuperato da `SqlElasticPool.listDatabaseActivities()`. Ogni oggetto di questo tipo rappresenta un'attività eseguita su un database nel pool elastico.
| [ElasticPoolActivity](/java/api/com.microsoft.azure.management.sql.elasticpoolactivity) | Recuperato in un elenco da `SqlElasticPool.listActivities()`. Ogni oggetto nell'elenco rappresenta un'attività eseguita sul pool elastico, non sui database del pool elastico.

## <a name="next-steps"></a>Passaggi successivi

[!INCLUDE [next-steps](includes/java-next-steps.md)]