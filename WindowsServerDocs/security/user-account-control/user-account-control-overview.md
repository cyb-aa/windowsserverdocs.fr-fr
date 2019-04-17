---
title: "Vue d’ensemble du contrôle de compte utilisateur"
description: "Sécurité de Windows Server"
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
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="user-account-control-overview"></a>Vue d’ensemble du contrôle de compte utilisateur
Utilisateur contrôle de compte \(UAC\) est un composant fondamental de la vision globale de sécurité de Microsoft.  Compte d’utilisateur permet d’atténuer l’impact des programmes malveillants.

## <a name="BKMK_OVER"></a>Description de la fonctionnalité
IL autorise tous les utilisateurs à ouvrir une session sur leurs ordinateurs à l’aide d’un compte d’utilisateur standard. Les processus démarrés à l’aide d’un jeton d’utilisateur standard peuvent effectuer des tâches à l’aide de droits d’accès accordés à un utilisateur standard. Par exemple, l’Explorateur Windows hérite automatiquement des autorisations de niveau utilisateur standard. En outre, tous les programmes qui sont exécutés à l’aide de l’Explorateur Windows \ (par exemple, en cliquant sur le double-cliqué une shortcut\ application) également exécuter avec le jeu d’autorisations d’utilisateur standard. De nombreuses applications, y compris ceux qui sont inclus dans le système d’exploitation lui-même, sont conçues pour fonctionner correctement de cette façon.

D’autres applications, notamment celles qui n’ont pas été conçues spécifiquement avec les paramètres de sécurité à l’esprit, requièrent souvent des autorisations supplémentaires pour s’exécuter correctement. Ces types de programmes sont appelés des applications héritées. En outre, les actions telles que l’installation de nouveaux logiciels et d’apporter des modifications de configuration à des programmes tels que le pare-feu Windows, requièrent plus d’autorisations que ce qui est disponible pour un compte d’utilisateur standard.

Quand un besoins d’applications à exécuter avec les droits d’utilisateur standard plus de, UAC peut restaurer des groupes d’utilisateurs supplémentaires au jeton. Cela permet à l’utilisateur d’avoir un contrôle explicite des programmes qui apportent des modifications au niveau du système à leur ordinateur ou le périphérique.

## <a name="BKMK_APP"></a>Applications pratiques
Mode d’approbation administrateur dans le compte d’utilisateur vous aide à empêcher les programmes malveillants à partir de l’installation en mode silencieux sans une connaissance d’un administrateur. Il permet également de protéger contre les modifications accidentelles de transcription à l’échelle du. Enfin, il peut être utilisé pour appliquer un niveau supérieur de conformité où les administrateurs doivent activement de consentement ou fournir des informations d’identification pour chaque processus d’administration.



