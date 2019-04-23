---
title: Vue d’ensemble du contrôle de compte d’utilisateur
description: Sécurité de Windows Server
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 90ce72cb3d1850563d16a12d09a6872d107c0690
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887670"
---
# <a name="user-account-control-overview"></a>Vue d’ensemble du contrôle de compte d’utilisateur
Contrôle de compte d’utilisateur \(UAC\) est un composant fondamental de la vision de sécurité globale de Microsoft.  Le contrôle de compte d’utilisateur permet d’atténuer l’impact d’un programme malveillant.

## <a name="BKMK_OVER"></a>Description de la fonctionnalité
Il autorise tous les utilisateurs à se connecter à leurs ordinateurs à l’aide d’un compte d’utilisateur standard. Les processus démarrés via un jeton d’utilisateur standard peuvent exécuter des tâches en utilisant des droits d’accès accordés à un utilisateur standard. Par exemple, l’Explorateur Windows hérite automatiquement des autorisations de niveau utilisateur standard. En outre, tous les programmes qui sont exécutées à l’aide de l’Explorateur Windows \(, par exemple, en double\-en cliquant sur un raccourci de l’application\) également s’exécuter avec le jeu d’autorisations utilisateur standard. De nombreuses applications, y compris ceux qui sont inclus dans le système d’exploitation lui-même, sont conçues pour fonctionner correctement de cette façon.

Autres applications, en particulier ceux qui n’ont pas été spécifiquement conçues avec les paramètres de sécurité à l’esprit, nécessitent souvent des autorisations supplémentaires pour s’exécuter correctement. Ces types de programmes sont appelés applications héritées. En outre, les actions telles que l’installation de nouveaux logiciels et d’apporter des modifications de configuration pour les programmes, tels que le pare-feu Windows, nécessitent davantage d’autorisations que ce qui est disponible pour un compte d’utilisateur standard.

Quand un besoins d’applications à exécuter avec les droits d’utilisateur standard plus de, UAC peut restaurer des groupes d’utilisateurs supplémentaires au jeton. Cela permet à l’utilisateur d’avoir un contrôle explicite des programmes qui effectuent des modifications au niveau du système à leur ordinateur ou le périphérique.

## <a name="BKMK_APP"></a>Applications pratiques
Mode d’approbation administrateur en y apportant vous aide à empêcher les programmes malveillants à partir de l’installation en mode silencieux sans avoir connaissance de l’administrateur. Il contribue également à empêcher tout système par inadvertance\-modifications larges. Enfin, il peut être utilisé pour appliquer un niveau supérieur de conformité dans la mesure où les administrateurs doivent effectuer une action pour donner leur consentement ou fournir des informations d’identification pour chaque processus d’administration.



