---
title: Vue d’ensemble du contrôle de compte d’utilisateur
description: Sécurité de Windows Server
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: security-tpm
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1b7a39cd-fc10-4408-befd-4b2c45806732
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: bdc9f4dc4b8e19d62288f12a4f2b4e8c86b93b68
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403316"
---
# <a name="user-account-control-overview"></a>Vue d’ensemble du contrôle de compte d’utilisateur
Le contrôle de compte d’utilisateur \(\) UAC est un composant fondamental de la vision globale de la sécurité de Microsoft.  Le contrôle de compte d’utilisateur permet d’atténuer l’impact d’un programme malveillant.

## <a name="BKMK_OVER"></a>Description de la fonctionnalité
Il autorise tous les utilisateurs à se connecter à leurs ordinateurs à l’aide d’un compte d’utilisateur standard. Les processus démarrés via un jeton d’utilisateur standard peuvent exécuter des tâches en utilisant des droits d’accès accordés à un utilisateur standard. Par exemple, l’Explorateur Windows hérite automatiquement des autorisations de niveau utilisateur standard. En outre, tous les programmes qui sont exécutés à l’aide de l’Explorateur Windows \(par exemple, en double\-cliquant sur un raccourci d’application\) également s’exécuter avec le jeu d’autorisations utilisateur standard. De nombreuses applications, notamment celles qui sont incluses avec le système d’exploitation lui-même, sont conçues pour fonctionner correctement de cette manière.

D’autres applications, en particulier celles qui n’ont pas été spécifiquement conçues avec les paramètres de sécurité à l’esprit, nécessitent souvent des autorisations supplémentaires pour s’exécuter correctement. Ces types de programmes sont appelés applications héritées. En outre, des actions telles que l’installation de nouveaux logiciels et l’apport de modifications à la configuration des programmes tels que le pare-feu Windows nécessitent davantage d’autorisations que celles disponibles pour un compte d’utilisateur standard.

Quand une application doit s’exécuter avec plus de droits d’utilisateur standard, le contrôle de compte d’utilisateur peut restaurer des groupes d’utilisateurs supplémentaires sur le jeton. Cela permet à l’utilisateur de disposer d’un contrôle explicite des programmes qui effectuent des modifications au niveau du système sur leur ordinateur ou appareil.

## <a name="BKMK_APP"></a>Applications pratiques
Le mode d’approbation Administrateur dans le contrôle de compte d’utilisateur permet d’empêcher les programmes malveillants de s’installer sans aucune connaissance d’un administrateur. Il permet également de protéger le système par inadvertance\-des modifications importantes. Enfin, il peut être utilisé pour appliquer un niveau supérieur de conformité dans la mesure où les administrateurs doivent effectuer une action pour donner leur consentement ou fournir des informations d’identification pour chaque processus d’administration.



