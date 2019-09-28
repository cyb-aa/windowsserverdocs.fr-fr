---
title: Récupération de la forêt Active Directory-redéploiement des contrôleurs de domaine restants
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: fbab907c5624a76540ab6a28c568afbd9192c028
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71390246"
---
# <a name="ad-forest-recovery---redeploy-remaining-dcs"></a>Récupération de la forêt Active Directory-redéploiement des contrôleurs de domaine restants

>S'applique à : Windows Server 2016, Windows Server 2012 et 2012 R2, Windows Server 2008 et 2008 R2

Les étapes jusqu’à ce point s’appliquent à toutes les forêts : recherchez une sauvegarde valide pour chaque domaine, récupérez les domaines de manière isolée, reconnectez-les, réinitialisez le catalogue global et nettoyez. Au cours de cette étape suivante, vous allez redéployer la forêt. La façon de procéder dépend beaucoup de la conception de votre forêt, de vos contrats de niveau de service, de votre structure de site, de la bande passante disponible et de nombreux autres facteurs. Vous devez concevoir votre propre plan de redéploiement en fonction des principes et des suggestions de cette section, d’une manière qui convient le mieux aux besoins de votre entreprise.  
  
L’étape suivante consiste à installer AD DS sur tous les contrôleurs de site qui étaient présents avant la récupération de la forêt. Si les contrôleurs de service existent toujours, le service de AD DS doit être supprimé de force, ou les contrôleurs de service peuvent être réinstallés. Les sauvegardes existantes de ces contrôleurs de contrôle d’État ne peuvent pas être réutilisées, car les métadonnées correspondantes ont été supprimées pendant la récupération de la forêt. Dans un environnement non complexe, ce processus de redéploiement peut être aussi simple que la reconnexion des contrôleurs de réseau récupérés au réseau de production, et la promotion de nouveaux contrôleurs de réseau en fonction des besoins.  
  
Dans une grande entreprise confrontée à une infrastructure mondiale, un plan plus sophistiqué est nécessaire. La première phase consiste généralement à restaurer l’annuaire Active Directory en tant que service ; Cela signifie qu’il est nécessaire d’installer des contrôleurs de service placés de manière stratégique, de sorte que toutes les applications et divisions d’entreprise critiques puissent commencer à fonctionner. Il peut être acceptable pour les succursales d’avoir provisoirement des performances réduites en conséquence. La seconde phase consiste à redéployer tous les contrôleurs de contrôle de l’ensemble des contrôleurs de contrôle.  
  
 Il existe deux méthodes pour installer des contrôleurs de contrôle supplémentaires, qui peuvent être automatisés :  
  
- Clonage  
   - Pour les environnements virtualisés qui exécutent Windows Server 2012, le clonage est le moyen le plus rapide et le plus simple de récupérer un grand nombre de contrôleurs de contrôle. Vous pouvez automatiser la récupération de tous les contrôleurs de domaine virtualisés dans un domaine après avoir restauré un contrôleur de domaine virtualisé unique à partir d’une sauvegarde.  
   - Pour plus d’informations sur le clonage et les conditions préalables, consultez [Présentation de la virtualisation de Active Directory Domain Services (AD DS) (niveau 100)](https://technet.microsoft.com/library/hh831734.aspx).  
- Réinstaller AD DS à l’aide de Windows PowerShell sur des serveurs qui exécutent Windows Server 2012 (ou Dcpromo. exe sur des serveurs qui exécutent des versions antérieures de Windows Server) ou à l’aide de l’interface utilisateur  
   - Pour accélérer la réinstallation de AD DS, vous pouvez utiliser l’option installer à partir du support (IFM) pour réduire le trafic de réplication lors de l’installation. Pour plus d’informations sur l’utilisation de la commande **Ntdsutil IFM** pour créer un support d’installation, consultez [installation de AD DS à partir d’un média](https://technet.microsoft.com/library/cc770654\(WS.10\).aspx).  

Prenez en compte les points supplémentaires suivants pour chaque contrôleur de périphérique de réplica récupéré dans la forêt par le clonage de contrôleur de stockage virtualisé ou par l’installation d’AD DS (par opposition à la restauration à partir d’une sauvegarde) :  
  
- Tous les logiciels d’un contrôleur de périphérique utilisés comme source pour le clonage doivent pouvoir être clonés. Les applications et les services qui ne peuvent pas être clonés doivent être supprimés avant le lancement du clonage. Si ce n’est pas possible, vous devez choisir un autre contrôleur de périphérique virtualisé comme source.  
- Si vous clonez des contrôleurs de contrôle supplémentaires virtualisés à partir du premier contrôleur de périphérique virtualisé à restaurer, le contrôleur de périphérique source doit être arrêté lors de la copie du fichier VHDX. Ensuite, il doit être en cours d’exécution et disponible en ligne lors du premier démarrage des contrôleurs de service virtuels clone. Si le temps d’arrêt requis par l’arrêt n’est pas acceptable pour le premier contrôleur de périphérique récupéré, déployez un contrôleur de périphérique virtualisé supplémentaire en installant AD DS pour agir en tant que source pour le clonage.  
- Il n’existe aucune restriction sur le nom d’hôte du contrôleur de périphérique virtualisé cloné ou sur le serveur sur lequel vous souhaitez installer AD DS. Vous pouvez utiliser un nouveau nom d’hôte ou le nom d’hôte qui était déjà utilisé. Pour plus d’informations sur la syntaxe du nom d’hôte DNS, voir [Creating DNS Computer Names](https://technet.microsoft.com/library/cc785282.aspx) ([https://go.microsoft.com/fwlink/?LinkId=74564](https://go.microsoft.com/fwlink/?LinkId=74564)).  
- Configurez chaque serveur avec le premier serveur DNS de la forêt (le premier contrôleur de domaine qui a été restauré dans le domaine racine) comme serveur DNS préféré dans les propriétés TCP/IP de sa carte réseau. Pour plus d’informations, consultez [Configurer TCP/IP pour utiliser DNS](https://technet.microsoft.com/library/cc779282.aspx).  
- Redéployez tous les contrôleurs de domaine en lecture seule dans le domaine, soit par clonage de contrôleur de domaine virtualisé si plusieurs contrôleurs de domaine en lecture seule sont déployés dans un emplacement central, soit par la méthode traditionnelle de reconstruction en supprimant et en réinstallant les AD DS s’ils sont déployés individuellement dans des emplacements isolés. comme les succursales.  
   - La reconstruction de RODC garantit qu’ils ne contiennent pas d’objets en attente et peuvent contribuer à empêcher les conflits de réplication de se produire ultérieurement. Lorsque vous supprimez AD DS d’un RODC, *Choisissez l’option permettant de conserver les métadonnées du contrôleur*de domaine. L’utilisation de cette option conserve le compte krbtgt du contrôleur de domaine en lecture seule et conserve les autorisations pour le compte d’administrateur RODC délégué et le Stratégie de réplication de mot de passe (PRP) et vous évite d’avoir à utiliser les informations d’identification d’administrateur de domaine pour supprimer et réinstaller AD DS sur contrôleur de domaine en lecture seule. Il conserve également les rôles de serveur DNS et de catalogue global s’ils sont installés sur le contrôleur de domaine en lecture seule à l’origine.  
   - Lorsque vous reconstruisez des contrôleurs de flux (RODC ou DC inscriptibles), le trafic de réplication peut être augmenté au cours de la réinstallation. Pour réduire cet impact, vous pouvez échelonner la planification des installations de RODC, et vous pouvez utiliser l’option installer à partir du support (IFM). Si vous utilisez l’option IFM, exécutez la commande **Ntdsutil IFM** sur un contrôleur de périphérique accessible en écriture dont vous faites confiance pour libérer des données endommagées. Cela permet d’éviter une altération possible de l’affichage sur le contrôleur de domaine en lecture seule une fois la réinstallation de AD DS terminée. Pour plus d’informations sur IFM, consultez [installation de AD DS à partir d’un média](https://technet.microsoft.com/library/cc770654\(WS.10\).aspx).  
   - Pour plus d’informations sur la reconstruction des contrôleurs de domaine en [lecture seule, consultez suppression et réinstallation de RODC](https://technet.microsoft.com/library/cc835490\(WS.10\).aspx).  
- Si un contrôleur de domaine exécute le service serveur DNS avant la défaillance de la forêt, installez et configurez le service serveur DNS lors de l’installation de AD DS. Sinon, configurez ses anciens clients DNS avec d’autres serveurs DNS.  
- Si vous avez besoin d’autres catalogues globaux pour partager l’authentification ou la charge des requêtes pour les utilisateurs ou les applications, vous pouvez ajouter le catalogue global au contrôleur de source virtualisé source avant le clonage, ou vous pouvez faire d’un contrôleur de périphérique un serveur de catalogue global lors de l’installation de AD DS.  
  
## <a name="next-steps"></a>Étapes suivantes

- [Récupération de la forêt Active Directory : conditions préalables](AD-Forest-Recovery-Prerequisties.md)  
- [Récupération de la forêt Active Directory-élaboration d’un plan de récupération de forêt personnalisé](AD-Forest-Recovery-Devising-a-Plan.md)  
- [Récupération de la forêt Active Directory-identifier le problème](AD-Forest-Recovery-Identify-the-Problem.md)
- [Récupération de la forêt Active Directory-déterminer comment récupérer](AD-Forest-Recovery-Determine-how-to-Recover.md)
- [Récupération de la forêt Active Directory-effectuer la récupération initiale](AD-Forest-Recovery-Perform-initial-recovery.md)  
- [Récupération de la forêt Active Directory : procédures](AD-Forest-Recovery-Procedures.md)  
- [Récupération de la forêt Active Directory-Forum aux questions](AD-Forest-Recovery-FAQ.md)  
- [Récupération de la forêt Active Directory-récupération d’un domaine unique au sein d’une forêt à plusieurs domaines](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
- [Récupération de forêt AD-récupération de forêt avec les contrôleurs de domaine Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md)
