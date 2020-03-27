---
title: Windows Internet Name Service (WINS)
description: Cette rubrique fournit des informations sur la désaffectation de WINS et l’utilisation de DNS pour les services de résolution de noms sur votre réseau.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 32eabe7d-1130-4001-a79a-8ddb31993e5b
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 54e3f69500ccb3dbf6b2dfe47f6dd035e0a20511
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80315164"
---
#  <a name="windows-internet-name-service-wins"></a>Windows Internet Name Service (WINS)

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Le service WINS (Windows Internet Name Service) représente un service hérité d’inscription et de résolution de noms d’ordinateurs qui mappe les noms NetBIOS d’ordinateurs à des adresses IP.

Si WINS n’est pas déjà déployé sur votre réseau, ne déployez pas WINS-à la place, déployez Domain Name System \(DNS\). DNS fournit également des services de résolution et d’inscription de nom d’ordinateur, et inclut de nombreux avantages supplémentaires par rapport à WINS, tels que l’intégration à Active Directory Domain Services.

Pour plus d’informations, consultez [DNS (Domain Name System)](https://docs.microsoft.com/windows-server/networking/dns/dns-top) .

Si vous avez déjà déployé WINS sur votre réseau, il est recommandé de déployer le DNS, puis de le mettre hors service.
