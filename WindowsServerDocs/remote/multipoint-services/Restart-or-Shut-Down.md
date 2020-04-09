---
title: Redémarrer ou arrêter
description: En savoir plus sur le redémarrage ou l’arrêt complet d’un système dans MultiPoint services
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: fc9ce813-6ecb-4422-8f4b-5226386823f3
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: d6c09f6b7809bdcae19afbb6babc75887e1b57d8
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855672"
---
# <a name="restart-or-shut-down"></a>Redémarrer ou arrêter
Si cela est recommandé, vous devrez peut-être redémarrer l’ordinateur hôte et toutes les *stations* de votre système MultiPoint Services après avoir installé du matériel, des logiciels et des mises à jour logicielles. Si vous avez ajouté de nouveaux appareils sur une station, vous pouvez également associer les appareils à cette station. Pour plus d’informations sur la façon d’*associer des stations*, voir la rubrique [Changer de mode](Switch-Between-Modes.md).  
  
Pour désactiver l’ordinateur de votre système MultiPoint services en toute sécurité, l’ordinateur doit effectuer un processus d’arrêt qui ferme tous les programmes ouverts, arrête Windows et éteint votre ordinateur et les *stations*qui lui sont associées. Pour éteindre l’ordinateur, il ne faut pas simplement le débrancher ou appuyer sur le bouton **Marche/Arrêt**. Vous devez éteindre votre ordinateur à la fin de la journée et quand vous devez installer un nouveau matériel dans le boîtier de l’ordinateur.  Si vous ajoutez un autre matériel au système, vous devez également arrêter ou redémarrer le serveur.  
  
> [!NOTE]  
> Avant de redémarrer ou d’arrêter l’ordinateur qui exécute MultiPoint Services, toutes les *sessions* utilisateur doivent avoir été terminées.  
  
## <a name="restart-the-computer"></a>Redémarrer l'ordinateur  
  
1.  Terminez toutes les sessions utilisateur. Pour plus d’informations sur la manière de terminer des sessions utilisateur, voir la rubrique [Terminer une session utilisateur](End-a-User-Session.md).  
  
2.  Dans le gestionnaire MultiPoint, cliquez sur **démarrage**, puis sur **redémarrer l’ordinateur**.  
  
## <a name="shut-down-the-computer"></a>Arrêter l’ordinateur  
  
1.  Terminez toutes les sessions utilisateur. Pour plus d’informations sur la manière de terminer des sessions utilisateur, voir la rubrique [Terminer une session utilisateur](End-a-User-Session.md).  
  
2.  Dans le gestionnaire MultiPoint, cliquez sur l’onglet dossier de **démarrage** , puis sur **arrêter l’ordinateur**.  
  
## <a name="see-also"></a>Voir aussi  
[Terminer une session utilisateur](End-a-User-Session.md)  
[Gérer des tâches système à l’aide du Gestionnaire MultiPoint](Manage-System-Tasks-Using-MultiPoint-Manager.md)  
[Basculer entre les modes](Switch-Between-Modes.md)  
[Déconnecter des sessions utilisateur](Log-off-or-Disconnect-User-Sessions.md)