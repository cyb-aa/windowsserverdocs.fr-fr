---
title: Windows Internet Name Service (WINS)
description: Cette rubrique fournit des informations sur la désaffectation WINS et DNS pour les services de résolution de noms sur votre réseau.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 32eabe7d-1130-4001-a79a-8ddb31993e5b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5a3d132ada7b1ede83b046499058399a9da12190
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
#  <a name="windows-internet-name-service-wins"></a>Windows Internet Name Service (WINS)

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

WINS Windows Internet Name Service () est un service de nom ordinateur hérité inscription et de résolution qui mappe les noms d’ordinateur NetBIOS en adresses IP.

Si vous ne disposez pas déjà déployés sur votre réseau de WINS, ne pas déployer WINS - au lieu de cela, déployer le système de nom de domaine \(DNS\). DNS fournit des services d’inscription et la résolution du nom ordinateur et comprend de nombreux avantages supplémentaires sur WINS, telles que l’intégration avec les Services de domaine Active Directory.

Pour plus d’informations, voir [système DNS (Domain Name)](https://docs.microsoft.com/windows-server/networking/dns/dns-top)

Si vous avez déjà déployé WINS sur votre réseau, il est recommandé de déployer le système DNS et de le retirer puis WINS.
