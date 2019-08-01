---
title: Lors de la connexion, l’utilisateur reçoit le message Les Services Bureau à distance sont actuellement occupés
description: Résolution de l’erreur Les Services Bureau à distance sont actuellement occupés se produisant quand des utilisateurs établissent une connexion Bureau à distance.
audience: itpro
ms.custom: na
ms.reviewer: rklemen
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: troubleshooting
ms.assetid: ''
author: kaushika-msft
manager: ''
ms.author: delhan
ms.date: 07/24/2019
ms.localizationpriority: medium
ms.openlocfilehash: 889fd83925081ac1dce386b1cd18fbef59586eb5
ms.sourcegitcommit: f6503e503d8f08ba8000db9c5eda890551d4db37
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68529899"
---
# <a name="on-connecting-user-receives-remote-desktop-service-is-currently-busy-message"></a>Lors de la connexion, l’utilisateur reçoit un message « Les Services Bureau à distance sont actuellement occupés »

Pour déterminer comment répondre au mieux à ce problème, reportez-vous aux informations ci-après :

- Le service Services Bureau à distance cesse-t-il de répondre ? (Par exemple, le client Bureau à distance semble « se bloquer » à l’écran d’accueil.)  
   - Si le service ne répond pas, consultez [Problème de mémoire du serveur RDSH](#rdsh-server-memory-issue).
   - Si le client semble interagir normalement avec le service, passez à l’étape suivante.
- Si un ou plusieurs utilisateurs déconnectent leurs sessions Bureau à distance, peuvent-ils se reconnecter ?  
   - Si le service continue à refuser les connexions quel que soit le nombre d’utilisateurs qui déconnectent leurs sessions, consultez [Problème d’écouteur de Bureau à distance](#rd-listener-issue).
   - Si le service commence à réaccepter les connexions après que plusieurs utilisateurs ont déconnecté leurs sessions, [vérifiez la stratégie de limite du nombre de connexions](#check-the-connection-limit-policy).

## <a name="rdsh-server-memory-issue"></a>Problème de mémoire du serveur RDSH

Une fuite de mémoire a été constatée sur certains serveurs RDSH Windows Server 2012 R2. Avec le temps, ces serveurs commencent à refuser les connexions Bureau à distance et les connexions de console locale, avec des messages tels que les suivants :

> La tâche que vous essayez de réaliser ne peut pas se terminer car le service Bureau à distance est actuellement occupé. Réessayez dans quelques minutes. Cela ne devrait pas empêcher d’autres utilisateurs d’ouvrir une session.

Les clients Bureau à distance qui tentent de se connecter cessent également de répondre.

Pour contourner ce problème, redémarrez le serveur RDSH.

Pour résoudre ce problème, appliquez aux serveurs RDSH le correctif KB 4093114, [10 avril 2018 — KB4093114 (correctif cumulatif mensuel)](https://support.microsoft.com/help/4093114/).

## <a name="rd-listener-issue"></a>Problème d’écouteur de Bureau à distance

Un problème a été observé sur certains serveurs RDSH qui ont été mis à niveau directement de Windows Server 2008 R2 vers Windows Server 2012 R2 ou Windows Server 2016. Quand un client Bureau à distance se connecte au serveur RDSH, celui-ci crée un écouteur de Bureau à distance pour la session utilisateur. Les serveurs affectés comptabilisent les écouteurs de Bureau à distance, dont le nombre augmente à mesure que des utilisateurs se connectent mais ne diminue jamais.

Vous pouvez contourner ce problème à l’aide des méthodes suivantes :

  - Redémarrer le serveur RDSH pour réinitialiser le nombre d’écouteurs de Bureau à distance
  - Modifier la stratégie de limite du nombre de connexions, en lui affectant une valeur très élevée. Pour plus d’informations sur la gestion de la stratégie de limite du nombre de connexions, consultez [Vérifier la stratégie de limite du nombre de connexions](#check-the-connection-limit-policy).

Pour résoudre ce problème, appliquez les mises à jour suivantes aux serveurs RDSH :

  - Windows Server 2012 R2 : KB 4343891, [30 août 2018—KB4343891 (préversion du correctif cumulatif mensuel)](https://support.microsoft.com/help/4343891/windows-81-update-kb4343891)
  - Windows Server 2016 : KB 4343884, [30 août 2018—KB4343884 (build du système d’exploitation 14393.2457)](https://support.microsoft.com/help/4343884/windows-10-update-kb4343884)

## <a name="check-the-connection-limit-policy"></a>Vérifier la stratégie de limite du nombre de connexions

Vous pouvez définir la limite du nombre de connexions Bureau à distance simultanées au niveau de l’ordinateur ou en configurant un objet de stratégie de groupe. Par défaut, la limite n’est pas définie.

Pour vérifier les paramètres actuels et identifier les objets de stratégie de groupe existants sur le serveur RDSH, ouvrez une fenêtre d’invite de commandes en tant qu’administrateur et entrez la commande suivante :
  
```cmd
gpresult /H c:\gpresult.html
```
   
Une fois cette commande terminée, ouvrez **gpresult.html**. Dans **Configuration ordinateur\\Modèles d’administration\\Composants Windows\\Services Bureau à distance\\Hôte de session Bureau à distance\\Connexions**, recherchez la stratégie **Limiter le nombre de connexions**.

  - Si le paramètre de cette stratégie est **Désactivé**, cela signifie que la stratégie de groupe ne limite pas le nombre de connexions RDP.
  - Si le paramètre de cette stratégie est **Activé**, vérifiez **OSG gagnant**. Si vous avez besoin de supprimer ou de changer la limite du nombre de connexions, modifiez cet objet de stratégie de groupe.

Pour appliquer les modifications de stratégie, ouvrez une fenêtre d’invite de commandes sur l’ordinateur affecté et entrez la commande suivante :
  
```cmd
gpupdate /force
```
  
