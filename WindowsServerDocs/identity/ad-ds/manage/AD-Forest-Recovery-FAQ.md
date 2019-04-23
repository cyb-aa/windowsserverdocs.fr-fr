---
title: 'Récupération de la forêt Active Directory : FAQ'
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: ac9e5a3d-8b1e-41b7-8e02-f64b7acf1359
ms.technology: identity-adds
ms.openlocfilehash: 36c9560b490cc28f006770c869bdcdf2b152dd74
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878450"
---
# <a name="ad-forest-recovery---faq"></a>Récupération de la forêt Active Directory : FAQ

>S'applique à : Windows Server 2016, Windows Server 2012 et 2012 R2, Windows Server 2008 et 2008 R2, Windows Server 2003

Ce document contient des questions fréquemment posées (FAQ) concernant la récupération de forêt :  

## <a name="general-recovery"></a>Généraux de récupération

**Q : Que puis-je faire pour accélérer la récupération ?**

Bien que la vitesse de récupération n’est pas le principal objectif de ce guide, vous pouvez obtenir des temps de récupération plus courts par :  
  
- Création d’un plan de récupération de forêt détaillées, sa mise à jour régulièrement et pratique dans un environnement de test simulées de taille raisonnable au moins une fois par an  
- À l’aide de clonage de contrôleur de domaine virtualisé  
   - Clonage de contrôleur de domaine virtualisé accélère le processus pour obtenir des contrôleurs de domaine supplémentaires en cours d’exécution après un que contrôleur de domaine est restauré à partir de la sauvegarde dans chaque domaine. Les contrôleurs de domaine virtualisés supplémentaires peuvent être clonés au lieu d’attendre potentiellement longue AD DS des installations se terminent et la fin de la réplication non critique après l’installation.  
   - Les forêts où les contrôleurs de domaine virtuels sont hébergées dans un nombre relativement faible de centres de données bien connectés potentiellement bénéficient plus de clonage lors de la récupération. Toutefois, n’importe quel environnement où plusieurs contrôleurs de domaine virtualisés pour le même domaine sont localisés sur le même hôte hyperviseur doit bénéficier.  
- Déploiement de contrôleurs de domaine en lecture seule (RODC)  
   - Pouvez procurent la continuité d’activité pendant le processus de récupération, car ils n’ont pas à être déconnecté du réseau comme contrôleurs de domaine accessible en écriture. RODC n’effectue pas de réplication sortante. Par conséquent, ils ne présentent pas le même risque de posent des contrôleurs de domaine accessible en écriture pour la réplication des données dangereuses dans l’environnement restauré.  
  
Autres facteurs qui affectent la durée du processus de récupération de forêt sont les suivantes :  
  
- Lorsque vous restaurez des contrôleurs de domaine à partir de sauvegardes, de temps :  
   - Localisez le support de sauvegarde physique, tels que des bandes.  
   - Réinstallez le système d’exploitation.  
   - Restaurer des données à partir de supports de sauvegarde.  
      - Vous pouvez réduire le temps nécessaire pour réinstaller le système d’exploitation et restaurer des données à partir de la sauvegarde en effectuant la récupération complète du serveur au lieu de la restauration de l’état système. Étant donné que la récupération complète du serveur est basé sur le fichier binaire, il termine beaucoup plus rapide que la restauration de l’état système.  
      - Toutefois, si le serveur contient des données qui sont exclues des données d’état système que vous ne souhaitez pas restaurer, récupération de serveur complète peut revêtir une alternative viable à la restauration de l’état système. Peser les avantages de l’exécution d’une récupération complète du serveur au lieu d’une restauration d’état système pour vos serveurs en particulier et préparer en conséquence en effectuant le type approprié de sauvegarde que vous envisagez de restaurer ultérieurement.  
- Lorsque vous reconstruisez les contrôleurs de domaine, il faut de temps pour répliquer les données pour des promotions basées sur le réseau.  
   - Vous pouvez réduire le temps nécessaire pour la restauration des contrôleurs de domaine en effectuant les étapes suivantes :  
- Réduire le temps de récupération des supports de sauvegarde par :  
   - À l’aide de l’outil Active Directory de base de données de montage (Dsamain.exe) pour identifier les meilleures de sauvegarde à utiliser pour les opérations de restauration. Pour plus d’informations sur l’utilisation de l’outil de montage de Active Directory de base de données, consultez le [Active Directory de base de données de montage outil Guide pas à pas](https://go.microsoft.com/fwlink/?LinkId=132577) (https://go.microsoft.com/fwlink/?LinkId=132577).  
   - Étiquetage de clairement le support de sauvegarde et de stocker le support dans une structure organisée en pratique, encore sécurisés, emplacement qui permet une récupération rapide.  
   - À l’aide du Service de cliché instantané de Volume avec un réseau de zone de stockage (SAN) de conserver des sauvegardes à partir de différents points dans le temps. Pour plus d’informations, consultez [Windows Server 2003 Active Directory récupération rapide avec le Service VSS et le Service de disque virtuel](https://go.microsoft.com/fwlink/?LinkId=70781) (https://go.microsoft.com/fwlink/?LinkId=70781).  
- Forcer la suppression des services AD DS à partir de contrôleurs de domaine au lieu de la réinstallation du système d’exploitation. Si la cause de l’échec de la forêt a été identifiée pour être entièrement dans l’étendue des services AD DS, vous n’êtes pas obligé de réinstaller le système d’exploitation sur les contrôleurs de domaine.  
   - Pour plus d’informations sur la suppression forcée des services AD DS à partir d’un contrôleur de domaine qui exécute Windows Server 2008 ou version ultérieure, consultez [suppression forcée d’un contrôleur de domaine Windows Server 2008](https://go.microsoft.com/fwlink/?LinkId=132627) (https://go.microsoft.com/fwlink/?LinkId=132627). Pour plus d’informations sur la suppression forcée des services AD DS à partir d’un contrôleur de domaine qui exécute Windows Server 2003, consultez [article 332199](https://go.microsoft.com/fwlink/?LinkId=70780) dans la Base de connaissances Microsoft (https://go.microsoft.com/fwlink/?LinkId=70780).  
- Utiliser des périphériques à bandes plus rapides ou les sauvegardes sur disque pour réduire le temps requis pour les opérations de restauration.  
  
Vous pouvez également aider à accélérer les installations AD DS à l’aide de l’installation à partir de la fonctionnalité de support (IFM) pour reconstruire les contrôleurs de domaine dans chaque domaine. IFM réduit la latence de réplication qui implique la reconstruction de contrôleurs de domaine dans chaque domaine.  
  
Les entreprises qui ont un contrat de niveau de service (SLA) plus agressif peuvent envisager la modification les procédures de récupération de forêt pour accélérer les restaurations.  
  
**Q : Puis-je automatiser le processus de récupération de forêt ?**

En raison de la nature complexe et critique du processus de récupération de forêt, il n’existe actuellement aucune automation de bout en bout de celui-ci. Le processus de récupération de forêt est plus un défi logistique et de l’organisation de la continuité des activités à un problème technique d’automatisation de processus de restauration. Par conséquent, la personne qui administre l’environnement doit créer un plan de récupération de forêt qui est spécifique à cet environnement et puis automatiser certaines sections qui peuvent être automatisées avec succès.  
  
Vous pouvez effectuer la majeure partie de la procédure de récupération de forêt à l’aide des outils de ligne de commande. Par conséquent, la plupart des étapes est scriptable. Par exemple, Ntdsutil.exe est un des outils plus fréquemment utilisés dans le processus de récupération de forêt.  
  
Bien que les scripts peuvent accélérer la récupération, vous devez soigneusement tester ces scripts avant de les appliquer dans un environnement réel. En outre, vous devez les mettre à jour en fonction des modifications dans l’environnement Active Directory, telles que l’ajout d’un nouveau domaine ou contrôleur de domaine ou une nouvelle version d’Active Directory.

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
