---
title: Récupération de forêt AD - redéploiement restant des contrôleurs de domaine
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: af9ec02a480b35e573edebcdd5928451174b6c1d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812000"
---
# <a name="ad-forest-recovery---redeploy-remaining-dcs"></a>Récupération de forêt AD - redéploiement restant des contrôleurs de domaine

>S'applique à : Windows Server 2016, Windows Server 2012 et 2012 R2, Windows Server 2008 et 2008 R2

Les étapes jusqu'à ce point s’appliquent à toutes les forêts : rechercher une sauvegarde valide pour chaque domaine, récupérer les domaines de manière isolée, les reconnecter, réinitialiser le catalogue global et nettoyer. Redéployer la forêt dans l’étape suivante. La façon de procéder varie considérablement sur la conception de votre forêt, vos contrats de niveau de service, la structure du site, la bande passante disponible et divers autres facteurs. Vous devez concevoir votre propre plan de redéploiement basé sur les principes et les suggestions de cette section, d’une manière qui convient mieux aux besoins de votre entreprise.  
  
L’étape suivante consiste à installer les services AD DS sur tous les contrôleurs de domaine qui étaient présents avant la récupération de forêt a eu lieu. Si les contrôleurs de domaine existent toujours, le service AD DS est doivent être supprimés de force, ou les contrôleurs de domaine peuvent être réinstallés. Les sauvegardes existantes pour ces contrôleurs de domaine ne peuvent pas être réutilisés, car les métadonnées correspondantes a été supprimée lors de la récupération de forêt. Dans un environnement fluide, ce processus de redéploiement peut être aussi simple que la reconnexion les contrôleurs de domaine restaurés vers le réseau de production et la promotion de nouveaux contrôleurs de domaine en fonction des besoins.  
  
Dans une grande entreprise face à une infrastructure dans le monde entier, un plan plus sophistiqué est nécessaire. La première phase consiste généralement à restaurer la publicité en tant que service ; Cela signifie que pour installer stratégiquement placés contrôleurs de domaine tels que toutes les applications et les divisions commerciales critiques peuvent commencer à nouveau. Il peut être acceptable pour les succursales à temporairement une diminution des performances suite à cela. Comme une deuxième phase, tous les autres et les contrôleurs de domaine moins critiques sont redéployés.  
  
 Il existe deux méthodes pour installer des contrôleurs de domaine supplémentaires, qui peuvent être automatisés :  
  
- Clonage  
   - Pour des environnements virtualisés qui exécutent Windows Server 2012, le clonage est la façon la plus simple et rapide pour récupérer un grand nombre de contrôleurs de domaine. Vous pouvez automatiser la récupération de tous les contrôleurs de domaine virtualisés dans un domaine une fois que vous restaurez un contrôleur de domaine virtualisé unique à partir de la sauvegarde.  
   - Pour plus d’informations sur le clonage et la configuration requise, consultez [Introduction à la virtualisation des Services de domaine Active Directory (AD DS) (niveau 100)](https://technet.microsoft.com/library/hh831734.aspx).  
- Réinstallez les services AD DS à l’aide de Windows PowerShell sur les serveurs qui exécutent Windows Server 2012 (ou Dcpromo.exe sur les serveurs qui exécutent des versions antérieures de Windows Server) ou à l’aide de l’interface utilisateur  
   - Pour accélérer la réinstallation des services AD DS, vous pouvez utiliser l’installation à partir de l’option de support (IFM) afin de réduire le trafic de réplication lors de l’installation. Pour plus d’informations sur l’utilisation de la **ntdsutil ifm** commande pour créer le support d’installation, consultez [installation AD DS à partir du support](https://technet.microsoft.com/library/cc770654\(WS.10\).aspx).  

Tenez compte des points supplémentaires suivants pour chaque contrôleur de domaine réplica qui est récupérée dans la forêt par clonage de contrôleur de domaine virtualisé ou en installant AD DS (par opposition à la restauration à partir de la sauvegarde) :  
  
- Tous les logiciels sur un contrôleur de domaine qui est utilisé comme source pour le clonage doivent pouvoir être cloné. Applications et services qui ne peuvent pas être clonés doivent être supprimés avant le clonage est lancé. Si cela n’est pas possible, un autre contrôleur de domaine virtualisé doit être choisi comme source.  
- Si vous clonez les contrôleurs de domaine virtualisés supplémentaires à partir du premier contrôleur de domaine virtualisé à restaurer, le contrôleur de domaine source devrez arrêtés, tandis que son fichier VHDX est copié. Il devra être en cours d’exécution et disponibles en ligne lorsque le clone contrôleurs de domaine virtuels sont tout d’abord démarré. Si le temps d’arrêt requises par l’arrêt n’est pas acceptable pour le premier contrôleur de domaine restauré, déployez un contrôleur de domaine virtualisé supplémentaire en installant AD DS pour agir comme source pour le clonage.  
- Il n’existe aucune restriction sur le nom d’hôte du contrôleur de domaine virtualisé cloné ou le serveur sur lequel vous souhaitez installer les services AD DS. Vous pouvez utiliser un nom d’hôte ou le nom d’hôte qui était précédemment en cours d’utilisation. Pour plus d’informations sur la syntaxe de nom d’hôte DNS, consultez [création de noms d’ordinateur DNS](https://technet.microsoft.com/library/cc785282.aspx) ([https://go.microsoft.com/fwlink/?LinkId=74564](https://go.microsoft.com/fwlink/?LinkId=74564)).  
- Configurez chaque serveur avec le premier serveur DNS dans la forêt (le premier contrôleur de domaine qui a été restaurée dans le domaine racine) en tant que serveur DNS préféré dans les propriétés TCP/IP de sa carte réseau. Pour plus d’informations, consultez [configurer TCP/IP pour utiliser DNS](https://technet.microsoft.com/library/cc779282.aspx).  
- Redéployer tous les contrôleurs du domaine, si plusieurs contrôleurs sont déployés dans un emplacement central le clonage de contrôleur de domaine virtualisé, ou par la méthode classique de les recréer par la suppression et la réinstallation des services AD DS s’ils sont déployés individuellement dans des emplacements trouvés isolés tels que les succursales.  
   - La reconstruction RODC garantit qu’ils ne contiennent pas tous les objets en attente et peuvent aider à éviter les conflits de réplication plus tard. Lorsque vous supprimez les services AD DS à partir d’un RODC, *choisir l’option de conservation des métadonnées de contrôleur de domaine*. À l’aide de cette option conserve le compte krbtgt pour le RODC et conserve les autorisations pour le compte d’administrateur délégué RODC et la réplication de stratégie de mot de passe et vous évite d’avoir à utiliser les informations d’identification de l’administrateur de domaine à supprimer et réinstaller AD DS dans un RODC. Il conserve également le serveur DNS et les rôles de catalogue global s’ils sont installés sur le RODC à l’origine.  
   - Lorsque vous reconstruisez les contrôleurs de domaine (RODC ou contrôleurs de domaine accessible en écriture), il peut y avoir augmenté le trafic de réplication lors de leur réinstallation. Pour aider à limiter cet impact, vous pouvez échelonner les installations de RODC, et vous pouvez utiliser l’option d’installation à partir du média. Si vous utilisez l’option IFM, exécutez le **ntdsutil ifm** commande sur un contrôleur de domaine accessible en écriture auxquels vous faites confiance à être gratuit de données endommagées. Cela permet d’empêcher d’apparaître sur le RODC après la réinstallation des services AD DS est probablement endommagé. Pour plus d’informations sur IFM, consultez [installation AD DS à partir du support](https://technet.microsoft.com/library/cc770654\(WS.10\).aspx).  
   - Pour plus d’informations sur la reconstruction RODC, consultez [RODC suppression et la réinstallation](https://technet.microsoft.com/library/cc835490\(WS.10\).aspx).  
- Si un contrôleur de domaine était exécuté le service serveur DNS avant la défaillance de la forêt, installer et configurer le service serveur DNS pendant l’installation des services AD DS. Sinon, configurez ses anciens clients DNS avec d’autres serveurs DNS.  
- Si vous avez besoin des catalogues globaux supplémentaires pour partager l’authentification ou charge de requête pour les utilisateurs ou des applications, vous pouvez ajouter le catalogue global à la source de virtualiser le contrôleur de domaine avant le clonage ou vous pouvez rendre un contrôleur de domaine un serveur de catalogue global lors de l’installation des services AD DS.  
  
## <a name="next-steps"></a>Étapes suivantes

- [Récupération de forêt AD - conditions préalables](AD-Forest-Recovery-Prerequisties.md)  
- [Récupération de forêt AD - concevoir un plan de récupération personnalisée-forêt](AD-Forest-Recovery-Devising-a-Plan.md)  
- [Récupération de forêt AD - identifier le problème](AD-Forest-Recovery-Identify-the-Problem.md)
- [Récupération de forêt AD - déterminer comment récupérer](AD-Forest-Recovery-Determine-how-to-Recover.md)
- [Récupération de forêt AD - effectuer une récupération initiale](AD-Forest-Recovery-Perform-initial-recovery.md)  
- [Récupération de forêt AD - procédures](AD-Forest-Recovery-Procedures.md)  
- [Récupération de forêt AD - Forum aux Questions](AD-Forest-Recovery-FAQ.md)  
- [Récupération de forêt AD - récupération d’un domaine unique au sein d’une forêt Multidomain](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
- [Récupération de forêt AD - récupération de forêt avec les contrôleurs de domaine Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md)
