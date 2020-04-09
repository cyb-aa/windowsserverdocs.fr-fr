---
title: 'Récupération de la forêt Active Directory : FAQ'
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: ac9e5a3d-8b1e-41b7-8e02-f64b7acf1359
ms.technology: identity-adds
ms.openlocfilehash: f32111cf7cc81f8f49b7b1058cc1a0ccc780da7f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80824002"
---
# <a name="ad-forest-recovery---faq"></a>Récupération de la forêt Active Directory : FAQ

>S’applique à : Windows Server 2016, Windows Server 2012 et 2012 R2, Windows Server 2008 et 2008 R2, Windows Server 2003

Ce document contient les questions fréquemment posées (FAQ) concernant la récupération de la forêt :  

## <a name="general-recovery"></a>Récupération générale

**Q : que puis-je faire pour accélérer la récupération ?**

Bien que la vitesse de récupération ne soit pas l’objectif principal de ce guide, vous pouvez obtenir des temps de récupération plus courts en procédant de la façon suivante :  
  
- Créer un plan de récupération de forêt détaillé, le mettre à jour régulièrement et le mettre en pratique dans un environnement de test simulé d’une taille raisonnable au moins une fois par an  
- Utilisation du clonage du contrôleur de domaine virtualisé  
   - Le clonage de contrôleur de domaine virtualisé accélère le processus d’exécution de contrôleurs de domaine supplémentaires lorsqu’un contrôleur de domaine est restauré à partir d’une sauvegarde dans chaque domaine. Il est possible de cloner des contrôleurs de clés virtualisés supplémentaires au lieu d’attendre la fin de l’installation de AD DS de longue durée et la fin de la réplication non critique après l’installation.  
   - Les forêts où les contrôleurs de logiciels virtuels sont hébergés dans un nombre relativement faible de centres de données correctement connectés peuvent tirer le meilleur parti du clonage pendant la récupération. Toutefois, tout environnement dans lequel plusieurs contrôleurs de domaine virtualisés pour le même domaine sont colocalisés sur le même hôte hyperviseur devrait en bénéficier.  
- Déploiement de contrôleurs de domaine en lecture seule (RODC)  
   - Les contrôleurs de réseau en lecture seule peuvent assurer la continuité de l’activité pendant le processus de récupération, car ils n’ont pas besoin d’être déconnectés du réseau en tant que tâches accessibles en écriture Les RODC n’effectuent pas de réplication sortante. Par conséquent, ils ne présentent pas le même risque que les contrôleurs de l’accès en écriture pour répliquer les données nuisibles dans l’environnement récupéré.  
  
Les autres facteurs qui affectent la durée du processus de récupération de forêt sont les suivants :  
  
- Lorsque vous restaurez des contrôleurs de service à partir de sauvegardes, il faut du temps pour :  
   - Localisez le support de sauvegarde physique, tel que les bandes.  
   - Réinstallez le système d’exploitation.  
   - Restaurez les données à partir d’un support de sauvegarde.  
      - Vous pouvez réduire le temps nécessaire à la réinstallation du système d’exploitation et à la restauration des données à partir de la sauvegarde en effectuant une récupération complète du serveur au lieu de restaurer l’état du système. Étant donné que la récupération complète du serveur est basée sur des binaires, elle se termine beaucoup plus rapidement que la restauration de l’état du système.  
      - Toutefois, si le serveur contient des données qui sont exclues des données d’État du système que vous ne souhaitez pas restaurer, la récupération complète du serveur peut ne pas être une alternative viable à la restauration de l’état du système. Considérez les avantages liés à l’exécution d’une récupération complète du serveur au lieu d’une restauration de l’état du système pour vos serveurs, et préparez-vous en conséquence en exécutant le type de sauvegarde approprié que vous envisagez de restaurer ultérieurement.  
- Lorsque vous régénérez des contrôleurs de service, il faut du temps pour répliquer les données en vue d’une promotion réseau.  
   - Vous pouvez réduire le temps nécessaire à la restauration des contrôleurs de code en procédant comme suit :  
- Réduisez le temps nécessaire à la récupération des supports de sauvegarde :  
   - À l’aide de l’outil de montage de base de données Active Directory (DSAMAIN. exe) pour identifier la meilleure sauvegarde à utiliser pour les opérations de restauration. Pour plus d’informations sur l’utilisation de l’outil de montage de base de données Active Directory, consultez le [Guide pas à pas de l’outil de montage de base de données Active Directory](https://go.microsoft.com/fwlink/?LinkId=132577) (https://go.microsoft.com/fwlink/?LinkId=132577).  
   - Étiquetez clairement le support de sauvegarde et stockez-le de façon organisée à un emplacement pratique, mais sécurisé, qui permet une récupération rapide.  
   - L’utilisation de la Service VSS avec un réseau de zone de stockage (SAN) pour gérer les sauvegardes à partir de différents points dans le temps. Pour plus d’informations, consultez [Windows Server 2003 Active Directory récupération rapide avec service VSS et le service de disque virtuel](https://go.microsoft.com/fwlink/?LinkId=70781) (https://go.microsoft.com/fwlink/?LinkId=70781).  
- Forcez la suppression des AD DS du contrôleur de réseau au lieu de réinstaller le système d’exploitation. Si la cause de l’échec à l’échelle de la forêt a été identifiée comme étant purement dans le cadre de AD DS, il n’est pas nécessaire de réinstaller le système d’exploitation sur les contrôleurs de domaine.  
   - Pour plus d’informations sur le forçage de la suppression des AD DS à partir d’un contrôleur de domaine exécutant Windows Server 2008 ou une version ultérieure, consultez [forcer la suppression d’un contrôleur de domaine Windows server 2008](https://go.microsoft.com/fwlink/?LinkId=132627) (https://go.microsoft.com/fwlink/?LinkId=132627). Pour plus d’informations sur le forçage de la suppression des AD DS à partir d’un contrôleur de périphérique qui exécute Windows Server 2003, voir l' [article 332199](https://go.microsoft.com/fwlink/?LinkId=70780) de la base de connaissances Microsoft (https://go.microsoft.com/fwlink/?LinkId=70780).  
- Utilisez des lecteurs de bande plus rapides ou des sauvegardes sur disque pour réduire le temps nécessaire aux opérations de restauration.  
  
Vous pouvez également accélérer les installations de AD DS à l’aide de la fonctionnalité installer à partir du support (IFM) pour reconstruire les contrôleurs de domaine dans chaque domaine. L’IFM réduit la latence de réplication qui est survenue lorsque vous régénérez des contrôleurs de domaine dans chaque domaine.  
  
Les entreprises qui ont un contrat de niveau de service (SLA) plus agressif peuvent envisager de modifier les procédures de récupération de forêt pour accélérer la récupération.  
  
**Q : puis-je automatiser le processus de récupération de forêt ?**

En raison de la nature complexe et critique du processus de récupération de forêt, il n’existe actuellement aucune Automation de bout en bout. Le processus de récupération de forêt est plus un défi logistique et organisationnel de la restauration de la continuité des activités qu’un problème technique de l’automatisation des processus. Par conséquent, la personne qui administre l’environnement doit créer un plan de récupération de forêt spécifique à cet environnement, puis automatiser ses sections qui peuvent être automatisées avec succès.  
  
Vous pouvez effectuer la plupart des étapes de récupération de la forêt à l’aide d’outils en ligne de commande. Par conséquent, la plupart des étapes sont scriptables. Par exemple, Ntdsutil. exe est l’un des outils les plus fréquemment utilisés dans le processus de récupération de forêt.  
  
Bien que les scripts puissent accélérer la récupération, vous devez tester minutieusement ces scripts avant de les appliquer dans un environnement réel. En outre, vous devez les mettre à jour en fonction des modifications apportées à l’environnement Active Directory, telles que l’ajout d’un nouveau domaine ou d’un contrôleur de domaine, ou une nouvelle version de Active Directory.

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
