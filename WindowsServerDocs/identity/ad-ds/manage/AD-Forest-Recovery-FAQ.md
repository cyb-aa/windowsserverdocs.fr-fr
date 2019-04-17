---
title: "Récupération de la forêt ActiveDirectory - FAQ"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: ac9e5a3d-8b1e-41b7-8e02-f64b7acf1359
ms.technology: identity-adfs
ms.openlocfilehash: 839e01c88b7ced22501154d8752c6c71cc52b696
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="ad-forest-recovery---faq"></a>Récupération de la forêt ActiveDirectory - FAQ

>S’applique à: Windows Server2016, Windows Server2012 et 2012R2, Windows Server2008 et 2008R2, Windows Server2003

Ce document contient un forum aux questions (FAQ) concernant la récupération de la forêt:  
 
## <a name="general-recovery"></a>Généraux de récupération
  
 
**Q: que puis-je faire pour accélérer la récupération?** 
 
Bien que la vitesse de récupération n’est pas l’objectif principal de ce guide, vous pouvez obtenir des temps de récupération plus courts par:  
  
-   Création d’un plan de récupération de forêt détaillées, mettre à jour régulièrement et pratique dans un environnement de test simulées de taille raisonnable au moins une fois par an  
  
-   À l’aide de clonage de contrôleur de domaine virtualisé  
  
     Le clonage de contrôleur de domaine virtualisé accélère le processus pour obtenir d’autres contrôleurs de domaine en cours d’exécution après un que contrôleur de domaine est restauré à partir de la sauvegarde dans chaque domaine. Les contrôleurs de domaine virtualisés supplémentaires peuvent être clonés au lieu d’attendre pour les services ADDS potentiellement longue installations à remplir et l’achèvement de la réplication non critique après l’installation.  
  
     Les forêts où les contrôleurs de domaine virtuels sont hébergées dans un nombre relativement faible des centres de données correctement connecté potentiellement bénéficient le plus de clonage lors de la récupération. Toutefois, n’importe quel environnement où plusieurs contrôleurs de domaine virtualisés pour le même domaine sont colocalisés sur le même hôte hyperviseur doit bénéficier.  
  
-   Déploiement de contrôleurs de domaine en lecture seule (RODC)  
  
     RODC peut assurer la continuité pendant le processus de récupération car elles n’ont pas à être déconnecté du réseau comme contrôleurs de domaine accessible en écriture. RODC n’effectuez pas la réplication sortante. Par conséquent, ils ne présentent pas le même risque qui présentent des contrôleurs de domaine accessible en écriture pour la réplication des données causer des dommages dans l’environnement restauré.  
  
 D’autres facteurs qui affectent la durée du processus de récupération de forêt sont les suivants:  
  
-   Lorsque vous restaurez des contrôleurs de domaine à partir de sauvegardes, de temps:  
  
    -   Recherchez le support de sauvegarde physique, tels que des bandes.  
  
    -   Réinstallez le système d’exploitation.  
  
    -   Restaurer les données à partir du média de sauvegarde.  
  
     Vous pouvez réduire le temps nécessaire pour réinstaller le système d’exploitation et de restaurer des données à partir de la sauvegarde en effectuant la récupération complète du serveur au lieu de la restauration d’état du système. Étant donné que la récupération complète du serveur est basé sur le fichier binaire, il se termine beaucoup plus rapidement que la restauration d’état du système.  
  
     Toutefois, si le serveur contient des données qui sont exclues des données d’état système que vous ne souhaitez pas restaurer, récupération complète du serveur peut-être pas une solution alternative viable pour la restauration d’état du système. Prendre en compte les avantages de l’exécution d’une récupération complète du serveur au lieu d’une restauration d’état système spécifiquement pour vos serveurs et préparer en conséquence en effectuant le type approprié de sauvegarde que vous souhaitez restaurer ultérieurement.  
  
-   Lorsque vous recréez les contrôleurs de domaine, de temps pour répliquer des données pour des promotions basées sur le réseau.  
  
 Vous pouvez diminuer le temps nécessaire pour la restauration des contrôleurs de domaine en effectuant les étapes suivantes:  
  
-   Réduire le temps de récupération du support de sauvegarde par:  
  
    -   À l’aide de l’outil de montage de la base de données ActiveDirectory (Dsamain.exe) pour identifier la meilleure sauvegarde à utiliser pour les opérations de restauration. Pour plus d’informations sur l’utilisation de l’outil ActiveDirectory de base de données de montage, voir la [l’outil de montage de la base de données ActiveDirectory Step-by-Step Guide](https://go.microsoft.com/fwlink/?LinkId=132577) (https://go.microsoft.com/fwlink/?LinkId=132577).  
  
    -   Le support de sauvegarde d’étiquetage clairement et stocker le média dans une structure organisée en pratique, encore sécurisé et emplacement qui permet une récupération rapide.  
  
    -   À l’aide du Service de cliché instantané de Volume avec un réseau de zone de stockage (SAN) pour conserver les sauvegardes à partir de différents points dans le temps. Pour plus d’informations, voir [Windows Server2003 ActiveDirectory récupération rapide avec le Service de cliché instantané de Volume et le Service de disque virtuel](https://go.microsoft.com/fwlink/?LinkId=70781) (https://go.microsoft.com/fwlink/?LinkId=70781).  
  
-   Forcer la suppression des services ADDS à partir de contrôleurs de domaine au lieu de réinstaller le système d’exploitation. Si la cause de l’échec de la forêt a été identifiée pour être purement dans l’étendue de domaine ActiveDirectory, il est inutile de réinstaller le système d’exploitation sur les contrôleurs de domaine.  
  
     Pour plus d’informations sur la suppression forcée des services ADDS à partir d’un contrôleur de domaine qui exécute Windows Server2008 ou version ultérieure, consultez [forcer la suppression d’un contrôleur de domaine Windows Server2008](https://go.microsoft.com/fwlink/?LinkId=132627) (https://go.microsoft.com/fwlink/?LinkId=132627). Pour plus d’informations sur la suppression forcée des services ADDS à partir d’un contrôleur de domaine qui exécute Windows Server2003, voir [article 332199](https://go.microsoft.com/fwlink/?LinkId=70780) dans la Base de connaissances Microsoft (https://go.microsoft.com/fwlink/?LinkId=70780).  
  
-   Utilisez plus rapide sur bande ou sauvegardes sur disque pour réduire le temps requis pour les opérations de restauration.  
  
 Vous pouvez également aider à accélérer les installations des services ADDS à l’aide de l’installation à partir de la fonctionnalité de support (IFM) pour reconstruire les contrôleurs de domaine dans chaque domaine. IFM réduit la latence de réplication qui naît lorsque vous reconstruisez des contrôleurs de domaine dans chaque domaine.  
  
 Entreprises ayant un contrat de niveau de service (SLA) plus agressif peuvent envisager de modifier les procédures de récupération de forêt pour accélérer les restaurations.  
  

**Q: puis-je automatiser le processus de récupération de forêt?**  
 En raison de la nature complexe et critiques du processus de récupération de forêt, il n’existe actuellement aucune automatisation de bout en bout de celui-ci. Le processus de récupération de forêt est plus un défi logistique et d’organisation de continuité des activités à un problème technique de l’automatisation du processus de restauration. Par conséquent, la personne qui administre l’environnement doit créer un plan de récupération de forêt qui est spécifique à cet environnement et puis automatiser certaines sections qui peuvent être automatisées avec succès.  
  
 Vous pouvez effectuer la plupart des étapes de récupération de forêt à l’aide des outils de ligne de commande. Par conséquent, la plupart des étapes est scriptable. Par exemple, Ntdsutil.exe est un des outils plus fréquemment utilisés dans le processus de récupération de forêt.  
  
 Bien que les scripts peuvent accélérer la récupération, vous devez tester ces scripts avant de les appliquer dans un environnement réel. En outre, vous devez mettre à jour en fonction des modifications dans l’environnement ActiveDirectory, tels que l’ajout d’un nouveau domaine ou contrôleur de domaine ou une nouvelle version d’ActiveDirectory.

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
