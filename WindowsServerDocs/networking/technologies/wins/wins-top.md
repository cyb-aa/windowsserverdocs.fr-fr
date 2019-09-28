---
title: Service WINS (Windows Internet Name Service)
description: Cette rubrique fournit des informations sur la désaffectation de WINS et l’utilisation de DNS pour les services de résolution de noms sur votre réseau.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 32eabe7d-1130-4001-a79a-8ddb31993e5b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 219c313dfeb26319cd5f537df417724de7044648
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405243"
---
#  <a name="windows-internet-name-service-wins"></a>Service WINS (Windows Internet Name Service)

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Le service WINS (Windows Internet Name Service) représente un service hérité d’inscription et de résolution de noms d’ordinateurs qui mappe les noms NetBIOS d’ordinateurs à des adresses IP.

Si vous n’avez pas encore déployé WINS sur votre réseau, ne déployez pas WINS-à la place, déployez Domain Name System \(DNS @ no__t-1. DNS fournit également des services de résolution et d’inscription de nom d’ordinateur, et inclut de nombreux avantages supplémentaires par rapport à WINS, tels que l’intégration à Active Directory Domain Services.

Pour plus d’informations, consultez [DNS (Domain Name System)](https://docs.microsoft.com/windows-server/networking/dns/dns-top) .

Si vous avez déjà déployé WINS sur votre réseau, il est recommandé de déployer le DNS, puis de le mettre hors service.
