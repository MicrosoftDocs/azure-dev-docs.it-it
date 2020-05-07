---
title: Differenze tra prodotti dell'interfaccia della riga di comando di Azure
description: Informazioni su come vengono assegnati i nomi e le versioni ai prodotti dell'interfaccia della riga di comando di Azure e come eseguire l'aggiornamento.
author: dbradish-microsoft
ms.author: dbradish
manager: barbkess
ms.date: 07/12/2018
ms.topic: conceptual
ms.service: azure-cli
ms.devlang: azurecli
ms.openlocfilehash: 3cd61d2166d03f7b9d58db1ee1cee77d17b5b336
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/05/2020
ms.locfileid: "82030833"
---
# <a name="differences-between-azure-cli-products"></a>Differenze tra prodotti dell'interfaccia della riga di comando di Azure

A partire dalla fine di giugno 2018, i numeri di versione espliciti sono stati rimossi dai nomi dei prodotti dell'interfaccia della riga di comando di Azure. Con questa modifica si elimina la confusione a volte presente nella documentazione in cui agli utenti veniva indicato di usare la "interfaccia della riga di comando di Azure" senza chiarire a quale versione del prodotto si facesse riferimento. Se si ha familiarità con i nomi precedenti dei prodotti, di seguito sono indicate le relative modifiche:

* Le versioni 2.0 e successive dell'interfaccia della riga di comando di Azure sono ora denominate semplicemente "interfaccia della riga di comando di Azure".
* Le versioni meno recenti dell'interfaccia della riga di comando di Azure (1.x e precedenti) sono ora indicate come "interfaccia della riga di comando classica di Azure".

La modifica del nome in "interfaccia della riga di comando classica di Azure" indica chiaramente che questo strumento è destinato a essere usato solo con il modello di distribuzione classica. L'interfaccia della riga di comando classica non viene più aggiornata o gestita. Per questo motivo, e molti altri, è consigliabile trasferire qualsiasi distribuzione classica al modello di Azure Resource Manager ed eseguire la migrazione alla versione più recente disponibile dell'interfaccia della riga di comando di Azure.

Se si sta ancora usando l'interfaccia della riga di comando classica, è possibile acquisire informazioni sul processo di migrazione negli articoli seguenti:

* [Eseguire la migrazione dal modello di distribuzione classica ad Azure Resource Manager](/azure/virtual-machines/linux/migration-classic-resource-manager-overview)
* [Installare l'interfaccia della riga di comando di Azure](install-azure-cli.md)
* [Eseguire la migrazione dall'interfaccia della riga di comando classica di Azure all'interfaccia della riga di comando di Azure](https://github.com/Azure/azure-cli/blob/dev/doc/classic_cli_migration.md)
