---
title: Come usare Spring Data JPA con Database di Azure per MySQL
description: Informazioni su come configurare e usare Spring Data JPA con un database di Azure per MySQL.
documentationcenter: java
ms.date: 11/27/2019
ms.service: mysql
ms.tgt_pltfrm: multiple
ms.topic: conceptual
ms.openlocfilehash: 927cc72a526651be71a7983a298ca2c6718f4546
ms.sourcegitcommit: 2ad3f7ce8c87331f8aff759ac2a3dc1b29581866
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/15/2020
ms.locfileid: "76022093"
---
# <a name="how-to-use-spring-data-jpa-with-azure-database-for-mysql"></a>Come usare Spring Data JPA con Database di Azure per MySQL

Questo articolo illustra la creazione di un'applicazione di esempio che usa [Spring Data] per archiviare e recuperare le informazioni in un [database di Azure per MySQL](/azure/mysql/) con [JPA (Java Persistence API)](https://docs.oracle.com/javaee/7/tutorial/persistence-intro.htm).

## <a name="prerequisites"></a>Prerequisites

I prerequisiti seguenti sono necessari per completare le procedure disponibili in questo articolo:

* Sottoscrizione di Azure; se non si ha una sottoscrizione di Azure, è possibile attivare i [vantaggi per i sottoscrittori di MSDN] oppure iscriversi per ottenere un [account Azure gratuito].
* Java Development Kit (JDK) supportato. Per altre informazioni sulle versioni di JDK utilizzabili per lo sviluppo in Azure, vedere <https://aka.ms/azure-jdks>.
* [Apache Maven](http://maven.apache.org/), versione 3.0 o versione successiva.
* [Curl](https://curl.haxx.se/) o utilità HTTP simile per testare il funzionamento.
* Utilità da riga di comando [mysql](https://dev.mysql.com/downloads/).
* Un client [Git](https://git-scm.com/downloads).

## <a name="create-a-azure-database-for-mysql-server"></a>Creare un server Database di Azure per MySQL

### <a name="create-a-server-using-the-azure-portal"></a>Creare un server con il portale di Azure

> [!NOTE]
> 
> Per informazioni più dettagliate sulla creazione di database MySQL, vedere [Creare un server Database di Azure per MySQL nel portale di Azure](/azure/mysql/quickstart-create-mysql-server-database-using-azure-portal).

1. Aprire il [portale di Azure](https://portal.azure.com) ed effettuare l'accesso.

1. Selezionare **+Crea una risorsa**, quindi **Database** e infine **Database di Azure per MySQL**.

   ![Creare un database MySQL][MYSQL01]

1. Immettere le seguenti informazioni:

   - **Sottoscrizione** specificare la sottoscrizione di Azure da usare.
   - **Gruppo di risorse**: specificare se creare un nuovo gruppo di risorse o sceglierne uno esistente.
   - **Nome server**: scegliere un nome univoco per il server MySQL. Tale nome verrà usato per creare un nome di dominio completo, ad esempio *wingtiptoysmysql.mysql.database.azure.com*.
   - **Selezionare l'origine**: per questa esercitazione selezionare `Blank` per creare un nuovo database.
   - **Account di accesso amministratore server**: specificare il nome dell'amministratore del database.
   - **Password** e **Conferma password**: specificare la password dell'amministratore del database.
   - **Località**: specificare l'area geografica più vicina per il database.
   - **Versione**: specificare la versione del database più aggiornata.

   ![Creare le proprietà del database MySQL][MYSQL02]

1. Dopo aver immesso tutte le informazioni riportate sopra, fare clic su **Rivedi e crea**.

### <a name="configure-a-firewall-rule-for-your-server-using-the-azure-portal"></a>Configurare una regola del firewall per il server tramite il portale di Azure

1. Passare al portale di Azure all'indirizzo <https://portal.azure.com/> ed eseguire l'accesso.

1. Fare clic su **Tutte le risorse** e quindi sul database MySQL appena creato.

1. Fare clic su **Sicurezza delle connessioni**, creare una nuova regola specificando un nome univoco in **Regole del firewall**, immettere l'intervallo di indirizzi IP che dovrà avere accesso al database e quindi fare clic su **Salva**. Per questo esercizio l'indirizzo IP è quello del computer di sviluppo, che corrisponde al client.  È possibile usarlo sia per **Indirizzo IP iniziale** che per **Indirizzo IP finale**. Vedere anche la nota sotto il titolo *Creare un database usando l'utilità da riga di comando mysql*.

   ![Configurare la sicurezza delle connessioni][MYSQL04]

### <a name="retrieve-the-connection-string-for-your-server-using-the-azure-portal"></a>Recuperare la stringa di connessione per il server tramite il portale di Azure

1. Passare al portale di Azure all'indirizzo <https://portal.azure.com/> ed eseguire l'accesso.

1. Fare clic su **Tutte le risorse** e quindi sulle risorsa Database di Azure per MySQL appena creata.

1. Fare clic su **Stringhe di connessione** e copiare il valore del campo di testo **JDBC**.

   ![Recuperare la stringa di connessione JDBC][MYSQL05]

### <a name="create-a-database-using-the-mysql-command-line-utility"></a>Creare un database usando l'utilità da riga di comando `mysql`

1. Aprire una shell dei comandi e connettersi al server Database di Azure per MySQL immettendo un comando `mysql` come quello riportato nell'esempio seguente:

   ```shell
   mysql --host wingtiptoysmysql.mysql.database.azure.com --user wingtiptoysuser@wingtiptoysmysql -p
   ```
   Dove:

   | Parametro | Descrizione |
   |---|---|
   | `host` | Specifica il nome completo del server MySQL dei passaggi precedenti di questo articolo. |
   | `user` | Specifica l'amministratore MySQL e il nome abbreviato del server dei passaggi precedenti di questo articolo. |
   | `p` | Specifica di attendere la richiesta di una password. |


   La risposta visualizzata dal server MySQL sarà simile all'esempio seguente:

   ```shell
   Welcome to the MySQL monitor.  Commands end with ; or \g.
   Your MySQL connection id is 64552
   Server version: 5.6.39.0 MySQL Community Server (GPL)
   
   Copyright (c) 2000, 2016, Oracle and/or its affiliates. All rights reserved.
   
   Oracle is a registered trademark of Oracle Corporation and/or its
   affiliates. Other names may be trademarks of their respective
   owners.
   
   Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
   
   mysql>
   ```
   > Nota: se viene visualizzato un errore in cui si informa che il server non riconosce questo indirizzo IP, nell'errore sarà indicato l'indirizzo IP usato dal client.  Tornare indietro e assegnarlo come descritto in precedenza: *Configurare una regola del firewall per il server tramite il portale di Azure*.

1. Creare un database denominato *mysqldb* immettendo un comando `mysql` come quello riportato nell'esempio seguente:

   ```SQL
   CREATE DATABASE mysqldb;
   ```

   La risposta visualizzata dal server MySQL sarà simile all'esempio seguente:

   ```shell
   Query OK, 1 row affected (0.30 sec)
   ```

1. FACOLTATIVO: è possibile verificare la creazione del database immettendo un comando `mysql` come quello riportato nell'esempio seguente.

   ```SQL
   SHOW DATABASES;
   ```

   La risposta visualizzata dal server MySQL sarà simile all'esempio seguente:

   ```shell
   +--------------------+
   | Database           |
   +--------------------+
   | information_schema |
   | mysql              |
   | mysqldb            |
   | performance_schema |
   | sys                |
   +--------------------+
   ```

1. Immettere `\q` per uscire dall'utilità `mysql`.

## <a name="configure-the-sample-application"></a>Configurare l'applicazione di esempio

1. Aprire una shell dei comandi e clonare il progetto di esempio con un comando git come quello riportato nell'esempio seguente:

   ```shell
   git clone https://github.com/Azure-Samples/spring-data-jpa-on-azure.git
   ```

1. Individuare il file *application.properties* nella directory *resources* del progetto di esempio oppure creare il file se non esiste già.

1. Aprire il file *application.properties* in un editor di testo e aggiungere o configurare le righe seguenti nel file, sostituendo i valori di esempio con i valori appropriati dei passaggi precedenti:

   ```yaml
   spring.jpa.database-platform=org.hibernate.dialect.MySQL5InnoDBDialect
   spring.datasource.url=jdbc:mysql://wingtiptoysmysql.mysql.database.azure.com:3306/mysqldb?useSSL=true&requireSSL=false
   spring.datasource.username=wingtiptoysuser@wingtiptoysmysql
   spring.datasource.password=********
    ```
   Dove:

   | Parametro | Descrizione |
   |---|---|
   | `spring.jpa.database-platform` | Specifica la piattaforma di database JPA. |
   | `spring.datasource.url` | Specifica la stringa JDBC per MySQL dei passaggi precedenti di questo articolo. |
   | `spring.datasource.username` | Specifica il nome dell'amministratore MySQL dei passaggi precedenti di questo articolo, cui viene aggiunto il nome abbreviato del server. |
   | `spring.datasource.password` | Specifica la password dell'amministratore MySQL dei passaggi precedenti di questo articolo. |

1. Salvare e chiudere il file *application.properties*.

## <a name="package-and-test-the-sample-application"></a>Creare il pacchetto dell'applicazione di esempio e testarla 

1. Compilare l'applicazione di esempio con Maven. Ad esempio:

   ```shell
   mvn clean package -P mysql
   ```

1. Avviare l'applicazione di esempio. Ad esempio:

   ```shell
   java -jar target/spring-data-jpa-on-azure-0.1.0-SNAPSHOT.jar
   ```

1. Creare nuovi record usando `curl` al prompt dei comandi come negli esempi seguenti:

   ```shell
   curl -s -d '{"name":"dog","species":"canine"}' -H "Content-Type: application/json" -X POST http://localhost:8080/pets

   curl -s -d '{"name":"cat","species":"feline"}' -H "Content-Type: application/json" -X POST http://localhost:8080/pets
   ```

   L'applicazione dovrebbe restituire valori come i seguenti:

   ```shell
   Added Pet(id=1, name=dog, species=canine).

   Added Pet(id=2, name=cat, species=feline).
   ```

1. Recuperare tutti i record esistenti usando `curl` al prompt dei comandi come negli esempi seguenti:

   ```shell
   curl -s http://localhost:8080/pets
   ```
    
   L'applicazione dovrebbe restituire valori come i seguenti:

   ```json
   [{"id":1,"name":"dog","species":"canine"},{"id":2,"name":"cat","species":"feline"}]
   ```

## <a name="summary"></a>Summary

In questa esercitazione è stata creata un'applicazione Java di esempio che usa Spring Data per archiviare e recuperare le informazioni in un database di Azure per MySQL con JPA.

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni su Spring e Azure, passare al centro di documentazione di Spring in Azure.

> [!div class="nextstepaction"]
> [Spring in Azure](/azure/java/spring-framework)

### <a name="additional-resources"></a>Risorse aggiuntive

Per altre informazioni sull'uso di Azure con Java, vedere [Azure per sviluppatori Java] e la documentazione relativa all'[uso di Azure DevOps e Java].

<!-- URL List -->

[Azure per sviluppatori Java]: /azure/java/
[Account Azure gratuito]: https://azure.microsoft.com/pricing/free-trial/
[Uso di Azure DevOps e Java]: /azure/devops/
[vantaggi per i sottoscrittori di MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Data]: https://spring.io/projects/spring-data
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[MYSQL01]: media/configure-spring-data-jpa-with-azure-mysql/create-mysql-01.png
[MYSQL02]: media/configure-spring-data-jpa-with-azure-mysql/create-mysql-02.png
[MYSQL04]: media/configure-spring-data-jpa-with-azure-mysql/create-mysql-04.png
[MYSQL05]: media/configure-spring-data-jpa-with-azure-mysql/create-mysql-05.png
