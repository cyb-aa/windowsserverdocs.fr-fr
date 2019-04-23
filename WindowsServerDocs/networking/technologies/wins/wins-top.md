---
title: Service WINS (Windows Internet Name Service)
description: Cette rubrique fournit des informations sur le retrait de WINS et à l’aide de DNS pour les services de résolution de noms sur votre réseau.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 32eabe7d-1130-4001-a79a-8ddb31993e5b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: bbc1871d29021aa3c99f14368a4711dac63f4cee
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843630"
---
#  <a name="windows-internet-name-service-wins"></a>Service WINS (Windows Internet Name Service)

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Le service WINS (Windows Internet Name Service) représente un service hérité d’inscription et de résolution de noms d’ordinateurs qui mappe les noms NetBIOS d’ordinateurs à des adresses IP.

Si vous n’avez pas déjà WINS déployés sur votre réseau, ne pas déployer WINS - au lieu de cela, déployez Domain Name System \(DNS\). DNS fournit des services de l’inscription et la résolution de nom ordinateur et comprend de nombreux avantages supplémentaires sur WINS, tels que l’intégration avec les Services de domaine Active Directory.

Pour plus d’informations, consultez [système DNS (Domain Name)](https://docs.microsoft.com/windows-server/networking/dns/dns-top)

Si vous avez déjà déployé WINS sur votre réseau, il est recommandé de déployer le système DNS et de retrait puis de WINS.
