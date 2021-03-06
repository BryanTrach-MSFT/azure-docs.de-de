---
title: Konsistenzebenen und Azure Cosmos DB-APIs
description: Grundlegendes zu den Konsistenzebenen von APIs in Azure Cosmos DB.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/23/2018
ms.reviewer: sngun
ms.openlocfilehash: b620ca76cfea296e504afffd91852308a01575db
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/11/2019
ms.locfileid: "56001978"
---
# <a name="consistency-levels-and-azure-cosmos-db-apis"></a>Konsistenzebenen und Azure Cosmos DB-APIs

Die fünf in Azure Cosmos DB bereitgestellten Konsistenzmodelle werden nativ von der SQL-API unterstützt. Bei Verwendung von Azure Cosmos DB ist die SQL-API die Standard-API. 

Azure Cosmos DB bietet auch native Unterstützung für Wire Protocol-kompatible APIs für gängige Datenbanken. Dazu zählen MongoDB, Apache Cassandra, Gremlin und Azure Table Storage. Diese Datenbanken bieten keine genau definierten Konsistenzmodelle und keine von der SLA unterstützten Garantien für Konsistenzebenen. Sie umfassen in der Regel nur eine Teilmenge der fünf in Azure Cosmos DB bereitgestellten Konsistenzmodelle. Für die SQL-API, die Gremlin-API und die Tabellen-API wird die im Azure Cosmos-Konto konfigurierte Standardkonsistenzebene verwendet. 

Die folgenden Abschnitte zeigen das Mapping zwischen der von einem OSS-Clienttreiber angeforderten Datenkonsistenz für Apache Cassandra und MongoDB bzw. MongoDB und den entsprechenden Azure Cosmos DB-Konsistenzebenen.

## <a id="cassandra-mapping"></a>Zuordnung zwischen Apache Cassandra- und Azure Cosmos DB-Konsistenzebenen

In der nachfolgenden Tabelle werden die verschiedenen Konsistenzkombination, die mit der Cassandra-API verwendet werden können, und die entsprechende native Konsistenzebene von Cosmos DB beschrieben. Alle Kombination von Schreib- und Lesemodi in Apache Cassandra werden von Cosmos DB nativ unterstützt. Cosmos DB garantiert für jede Kombination von Apache Cassandra-Konsistenzmodellen für das Schreiben und Lesen mindestens eine ebenso hohe Konsistenz wie Apache Cassandra. Darüber hinaus bietet Cosmos DB eine höhere Dauerhaftigkeit als Apache Cassandra – und das selbst im schwächsten Schreibmodus.

Die folgende Tabelle zeigt die **Schreibkonsistenzzuordnung** zwischen Azure Cosmos DB und Cassandra:

| Cassandra | Azure Cosmos DB | Garantie |
| - | - | - |
|ALL|STARK (Strong)  | Linearisierbarkeit |
| EACH_QUORUM   | STARK (Strong)    | Linearisierbarkeit | 
| QUORUM, SERIAL |  STARK (Strong) |    Linearisierbarkeit |
| LOCAL_QUORUM, THREE, TWO, ONE, LOCAL_ONE, ANY | Präfixkonsistenz |Globale Präfixkonsistenz |
| EACH_QUORUM   | STARK (Strong)    | Linearisierbarkeit |
| QUORUM, SERIAL |  STARK (Strong) |    Linearisierbarkeit |
| LOCAL_QUORUM, THREE, TWO, ONE, LOCAL_ONE, ANY | Präfixkonsistenz | Globale Präfixkonsistenz |
| QUORUM, SERIAL | STARK (Strong)   | Linearisierbarkeit |
| LOCAL_QUORUM, THREE, TWO, ONE, LOCAL_ONE, ANY | Präfixkonsistenz | Globale Präfixkonsistenz |
| LOCAL_QUORUM, LOCAL_SERIAL, TWO, THREE    | Begrenzte Veraltung (Bounded staleness) | <ul><li>Begrenzte Veraltung</li><li>Um maximal K Versionen oder die Zeit t verzögert</li><li>Lesen des neuesten Werts in der Region, der committet wurde</li></ul> |
| ONE, LOCAL_ONE, ANY   | Präfixkonsistenz | Präfixkonsistenz pro Region |

Die folgende Tabelle zeigt die **Lesekonsistenzzuordnung** zwischen Azure Cosmos DB und Cassandra:

| Cassandra | Azure Cosmos DB | Garantie |
| - | - | - |
| ALL, QUORUM, SERIAL, LOCAL_QUORUM, LOCAL_SERIAL, THREE, TWO, ONE, LOCAL_ONE | STARK (Strong)  | Linearisierbarkeit|
| ALL, QUORUM, SERIAL, LOCAL_QUORUM, LOCAL_SERIAL, THREE, TWO   |STARK (Strong) |   Linearisierbarkeit |
|LOCAL_ONE, ONE | Präfixkonsistenz | Globale Präfixkonsistenz |
| ALL, QUORUM, SERIAL   | STARK (Strong)    | Linearisierbarkeit |
| LOCAL_ONE, ONE, LOCAL_QUORUM, LOCAL_SERIAL, TWO, THREE |  Präfixkonsistenz   | Globale Präfixkonsistenz |
| LOCAL_ONE, ONE, TWO, THREE, LOCAL_QUORUM, QUORUM |    Präfixkonsistenz   | Globale Präfixkonsistenz |
| ALL, QUORUM, SERIAL, LOCAL_QUORUM, LOCAL_SERIAL, THREE, TWO   |STARK (Strong) |   Linearisierbarkeit |
| LOCAL_ONE, ONE    | Präfixkonsistenz | Globale Präfixkonsistenz|
| ALL, QUORUM, SERIAL – STARK (Strong) Linearisierbarkeit
LOCAL_ONE, ONE, LOCAL_QUORUM, LOCAL_SERIAL, TWO, THREE  |Präfixkonsistenz  | Globale Präfixkonsistenz |
|ALL    |STARK (Strong) |Linearisierbarkeit |
| LOCAL_ONE, ONE, TWO, THREE, LOCAL_QUORUM, QUORUM  |Präfixkonsistenz  |Globale Präfixkonsistenz|
|ALL, QUORUM, SERIAL – STARK (Strong) Linearisierbarkeit
LOCAL_ONE, ONE, LOCAL_QUORUM, LOCAL_SERIAL, TWO, THREE  |Präfixkonsistenz  |Globale Präfixkonsistenz |
|ALL    |STARK (Strong) | Linearisierbarkeit |
| LOCAL_ONE, ONE, TWO, THREE, LOCAL_QUORUM, QUORUM  | Präfixkonsistenz | Globale Präfixkonsistenz |
| QUORUM, LOCAL_QUORUM, LOCAL_SERIAL, TWO, THREE |  Begrenzte Veraltung (Bounded staleness)   | <ul><li>Begrenzte Veraltung</li><li>Um maximal K Versionen oder die Zeit t verzögert </li><li>Lesen des neuesten Werts in der Region, der committet wurde</li></ul>
| LOCAL_ONE, ONE |Präfixkonsistenz | Präfixkonsistenz pro Region |
| LOCAL_ONE, ONE, TWO, THREE, LOCAL_QUORUM, QUORUM  | Präfixkonsistenz | Präfixkonsistenz pro Region |


## <a id="mongo-mapping"></a>Zuordnung zwischen MongoDB 3.4- und Azure Cosmos DB-Konsistenzebenen

In der folgenden Tabelle ist die Zuordnung für das Leseinteresse zwischen dem MongoDB 3.4-Client und der Standardkonsistenzebene in Azure Cosmos DB aufgeführt. Es sind Bereitstellungen in mehreren Regionen sowie in einer einzelnen Region aufgeführt.

| **MongoDB 3.4** | **Azure Cosmos DB (mehrere Regionen)** | **Azure Cosmos DB (einzelne Region)** |
| - | - | - |
| Linearisierbar | STARK (Strong) | STARK (Strong) |
| Mehrheit | Bounded staleness | STARK (Strong) |
| Lokal | Konsistentes Präfix | Konsistentes Präfix |

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über die Konsistenzebenen und die Kompatibilität zwischen Azure Cosmos DB-APIs und Open-Source-APIs. Entsprechende Informationen finden Sie in den folgenden Artikeln:

* [Kompromisse in Bezug auf Verfügbarkeit und Leistung für verschiedene Konsistenzebenen](consistency-levels-tradeoffs.md)
* [MongoDB-Features, die von der API für MongoDB von Azure Cosmos DB unterstützt werden](mongodb-feature-support.md)
* [Apache Cassandra-Features, die von der Cassandra-API für Azure Cosmos DB unterstützt werden](cassandra-support.md)