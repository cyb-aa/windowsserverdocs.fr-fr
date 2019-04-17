---
title: "Récupération de la forêt ActiveDirectory - redéploiement restant des contrôleurs de domaine"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adfs
ms.openlocfilehash: 66b0bc65b3b8e5dfbf5f1a85350dab60ac3a11c8
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="ad-forest-recovery---redeploy-remaining-dcs"></a>Récupération de la forêt ActiveDirectory - redéploiement restant des contrôleurs de domaine

>S’applique à: Windows Server2016, Windows Server2012 et 2012R2, Windows Server2008 et 2008R2

 Les étapes jusqu'à ce point s’appliquent à toutes les forêts: rechercher une sauvegarde valide pour chaque domaine, récupérer les domaines d’isolation, les reconnecter, réinitialiser le catalogue global et nettoyer. Dans cette étape vous serez redéployez la forêt. La façon de procéder dépendent considérablement de votre conception de forêt, vos contrats de niveau de service, la structure du site, la bande passante disponible et de nombreux autres facteurs. Vous devez concevoir votre propre plan redéploiement basé sur les principes et les suggestions de cette section, d’une façon qui convient mieux aux besoins de votre entreprise.  
  
 L’étape suivante consiste à installer les services ADDS sur tous les contrôleurs de domaine qui étaient présents avant la récupération de la forêt a eu lieu. Si les contrôleurs de domaine existent toujours, le service ADDS devront être supprimés de force ou les contrôleurs de domaine peuvent être réinstallés. Toutes les sauvegardes existantes pour ces contrôleurs de domaine ne peut pas être réutilisés, car les métadonnées correspondantes a été supprimée pendant la récupération de forêt. Dans un environnement bien ce processus redéploiement peut être aussi simple que la reconnexion les contrôleurs de domaine restaurés au réseau de production et promotion de nouveaux contrôleurs de domaine en fonction des besoins.  
  
 Dans une grande entreprise face à une infrastructure dans le monde entier, un plan plus sophistiqué est nécessaire. La première phase consiste généralement à restaurer la publicité en tant que service; cela signifie pour installer stratégiquement placés contrôleurs de domaine tel que toutes les applications et les divisions critiques peuvent reprendre votre travail. Il peut être acceptable pour les succursales aux temporairement une diminution des performances cette situation. En deuxième phase, tous les autres et les contrôleurs de domaine moins critiques sont redéployés.  
  
 Il existe deux méthodes pour installer des contrôleurs de domaine supplémentaires, qui peuvent être automatisés:  
  
-   Le clonage  
  
     Pour les environnements virtualisés qui exécutent Windows Server2012, le clonage est le moyen plus rapide et plus simple de récupérer un grand nombre de contrôleurs de domaine. Vous pouvez automatiser la récupération de tous les contrôleurs de domaine virtualisés dans un domaine après la restauration d’un contrôleur de domaine virtualisé unique à partir de la sauvegarde.  
  
     Pour plus d’informations sur le clonage et les conditions préalables, voir [Introduction aux Services de domaine ActiveDirectory (ADDS) Virtualization (Level 100)](https://technet.microsoft.com/library/hh831734.aspx).  
  
-   Réinstallez les services ADDS à l’aide de Windows PowerShell sur les serveurs qui exécutent Windows Server2012 (ou Dcpromo.exe sur les serveurs qui exécutent des versions antérieures de Windows Server) ou à l’aide de l’interface utilisateur  
  
     Pour accélérer la réinstallation des services ADDS, vous pouvez utiliser l’installation à partir de l’option de support (IFM) afin de réduire le trafic de réplication lors de l’installation. Pour plus d’informations sur l’utilisation de la **ntdsutil ifm** commande pour créer un support d’installation, voir [installation ADDS à partir du média](https://technet.microsoft.com/library/cc770654\(WS.10\).aspx).  
  
 Prenez en compte les points suivants pour chaque contrôleur de domaine réplica qui est récupérée dans la forêt par clonage de contrôleur de domaine virtualisé, ou en installant ADDS (par opposition à la restauration à partir de la sauvegarde):  
  
-   Tous les logiciels sur un contrôleur de domaine qui est utilisé comme source pour le clonage doivent être en mesure d’être cloné. Applications et services qui ne peuvent pas être clonés doivent être supprimés avant le clonage est lancé. Si cela n’est pas possible, un autre contrôleur de domaine virtualisé doit choisi comme source.  
  
-   Si vous clonez des contrôleurs de domaine virtualisés supplémentaires à partir du premier contrôleur de domaine virtualisé peut être restauré, le contrôleur de domaine source devrez arrêtés, tandis que son fichier VHDX est copié. Il doit être en cours d’exécution et disponibles en ligne lorsque le clonage des contrôleurs de domaine virtuels sont premier démarrage. Si le temps d’arrêt requis par l’arrêt n’est pas acceptable pour le premier contrôleur de domaine restauré, déployez un contrôleur de domaine virtualisé supplémentaire en installant ADDS pour agir en tant que la source du clonage.  
  
-   Il n’existe aucune restriction sur le nom d’hôte du contrôleur de domaine virtualisé cloné ou le serveur sur lequel vous souhaitez installer les services ADDS. Vous pouvez utiliser un nouveau nom d’hôte ou le nom d’hôte qui a été utilisé précédemment. Pour plus d’informations sur la syntaxe de nom d’hôte DNS, voir [création de noms d’ordinateur DNS](https://technet.microsoft.com/library/cc785282.aspx) ([https://go.microsoft.com/fwlink/?LinkId=74564](https://go.microsoft.com/fwlink/?LinkId=74564)).  
  
-   Configurez chaque serveur avec le premier serveur DNS dans la forêt (le premier contrôleur de domaine qui a été restaurée dans le domaine racine) comme serveur DNS préféré dans les propriétés TCP/IP de sa carte réseau. Pour plus d’informations, voir [configurer TCP/IP pour utiliser DNS](https://technet.microsoft.com/library/cc779282.aspx).  
  
-   Redéployez tous les contrôleurs du domaine, soit par le clonage du contrôleur de domaine virtualisé si plusieurs contrôleurs sont déployés dans un emplacement central, soit par la méthode traditionnelle de reconstruire les par la suppression et la réinstallation des services ADDS s’ils sont déployés individuellement dans des emplacements situés isolés tels que les succursales.  
  
     La reconstruction RODC permet de s’assurer qu’ils ne contiennent pas de tous les objets en attente et peuvent d’éviter les conflits de réplication ne se produise plus tard. Lorsque vous supprimez les services ADDS à partir d’un RODC, *choisir de conserver les métadonnées de contrôleur de domaine*. À l’aide de cette option conserve le compte krbtgt pour le RODC conserve les autorisations pour le compte d’administrateur délégué RODC et la stratégie de réplication de mot de passe (PRP) et vous évite d’avoir à utiliser les informations d’identification de l’administrateur de domaine à supprimer et réinstaller les services ADDS sur un RODC. Il conserve également les rôles de catalogue global et le serveur DNS s’ils sont installés à l’origine sur le RODC.  
  
     Lorsque vous reconstruisez des contrôleurs de domaine (RODC ou contrôleurs de domaine accessible en écriture), il peut y avoir augmenté le trafic de réplication au cours de leur réinstallation. Pour réduire les effets sur, vous pouvez échelonner les installations RODC, et vous pouvez utiliser l’option d’installation à partir de support (IFM). Si vous utilisez l’option IFM, exécutez le **ntdsutil ifm** commande sur un contrôleur de domaine accessible en écriture auxquels vous faites confiance à être libre de données endommagées. Cela permet d’empêcher la corruption possible s’affichent sur le RODC après que la réinstallation des services ADDS est terminée. Pour plus d’informations sur IFM, voir [installation ADDS à partir du média](https://technet.microsoft.com/library/cc770654\(WS.10\).aspx).  
  
     Pour plus d’informations sur la reconstruction RODC, voir [RODC suppression et une réinstallation](https://technet.microsoft.com/library/cc835490\(WS.10\).aspx).  
  
-   Si un contrôleur de domaine a été exécutant le service serveur DNS avant du dysfonctionnement de la forêt, installer et configurer le service serveur DNS pendant l’installation des services ADDS. Dans le cas contraire, configurez ses anciens clients DNS avec d’autres serveurs DNS.  
  
-   Si vous avez besoin d’ajouter des catalogues globaux de partager l’authentification ou la charge de la requête pour les utilisateurs ou les applications, vous pouvez ajouter le catalogue global à la source virtualisée du contrôleur de domaine avant le clonage ou vous pouvez rendre un serveur de catalogue global un contrôleur de domaine pendant l’installation des services ADDS.  
  
## <a name="next-steps"></a>Étapes suivantes
-   [Récupération de la forêt ActiveDirectory - Configuration requise](AD-Forest-Recovery-Prerequisties.md)  
-   [Récupération de la forêt ActiveDirectory - concevoir un plan de récupération de forêt personnalisé](AD-Forest-Recovery-Devising-a-Plan.md)  
- [Récupération de la forêt ActiveDirectory - identifier le problème](AD-Forest-Recovery-Identify-the-Problem.md)
-   [Récupération de la forêt ActiveDirectory - déterminer comment récupérer](AD-Forest-Recovery-Determine-how-to-Recover.md)
-   [Récupération de la forêt ActiveDirectory - effectuer une récupération initiale](AD-Forest-Recovery-Perform-initial-recovery.md)  
-   [Récupération de la forêt ActiveDirectory - procédures](AD-Forest-Recovery-Procedures.md)  
-   [Récupération de la forêt ActiveDirectory - Forum aux Questions](AD-Forest-Recovery-FAQ.md)  
-   [Récupération de la forêt ActiveDirectory - récupération d’un domaine unique au sein d’une forêt Multidomain](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
-   [Récupération de la forêt ActiveDirectory - récupération de forêt avec des contrôleurs de domaine Windows Server2003](AD-Forest-Recovery-Windows-Server-2003.md)  