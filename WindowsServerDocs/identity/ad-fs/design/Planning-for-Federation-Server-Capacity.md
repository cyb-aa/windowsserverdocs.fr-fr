---
ms.assetid: 7013fc21-9ced-4f9d-9588-cb04d6d60924
title: Planification de la capacité des serveurs de fédération
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 5801196921c1f7632725dfddb2a5c8c2bf4ae2b5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858622"
---
# <a name="planning-for-federation-server-capacity"></a>Planification de la capacité des serveurs de fédération

La planification de la capacité pour les serveurs de Fédération vous aide à estimer :  
  
-   Les facteurs qui augmentent la taille de la base de données de configuration AD FS.  
  
-   La configuration matérielle requise pour chaque serveur de Fédération.  
  
-   Nombre de serveurs de Fédération à placer dans chaque organisation.  
  
Les serveurs de Fédération émettent des jetons de sécurité pour les utilisateurs. Ces jetons sont présentés à une partie de confiance à des fins de consommation. Les serveurs de Fédération émettent des jetons de sécurité après l’authentification d’un utilisateur ou après la réception d’un jeton de sécurité précédemment émis par un partenaire service FS (Federation Service). Un jeton de sécurité est demandé à un service FS (Federation Service) lorsque les utilisateurs se connectent initialement à des applications fédérées ou lorsque leurs jetons de sécurité expirent alors qu’ils accèdent à des applications fédérées.  
  
Les serveurs de Fédération sont conçus pour prendre en charge des configurations de batterie de serveurs haute\-Availability qui utilisent l’équilibrage de charge réseau Microsoft \(la technologie NLB\). Les serveurs de Fédération dans une configuration de batterie de serveurs peuvent traiter les demandes indépendamment, sans accéder aux composants de la batterie de serveurs courants pour chaque demande. Par conséquent, il y a peu de surcharge impliquée dans la montée en charge d’un déploiement de serveur de Fédération.  
  
**Relatives**  
  
-   Pour les déploiements de disponibilité\-critique ou haute\-, nous vous recommandons de créer une batterie de serveurs de Fédération de petite taille dans chaque organisation partenaire, avec au moins deux serveurs de Fédération par batterie pour fournir une tolérance de panne.  
  
-   Avec la nécessité d’une haute disponibilité et la facilité de mise à l’échelle des serveurs de Fédération, la montée en charge est la méthode recommandée pour gérer un grand nombre de demandes par seconde pour une service FS (Federation Service) particulière. Il est peu probable que la mise à l’échelle supérieure à la configuration de base de ce guide produise des gains significatifs en matière de gestion de la capacité.  
  
## <a name="ad-fs-configuration-database-size-and-growth"></a>Taille et croissance de la base de données de configuration AD FS  
La taille de la base de données de configuration AD FS est généralement considérée comme petite, et la taille de la base de données n’est pas considérée comme une préoccupation majeure dans les déploiements AD FS.  La taille exacte de la base de données de configuration AD FS peut dépendre en grande partie du nombre de relations d’approbation et de la confiance associée\-les métadonnées associées, telles que les revendications, les règles de revendication et les paramètres de surveillance configurés pour chaque approbation. À mesure que le nombre d’entrées d’approbations dans la base de données de configuration augmente, le besoin d’espace disque est plus grand.  
  
Pour plus d’informations sur le déploiement de la base de données de configuration de AD FS, consultez Considérations sur la [topologie de déploiement AD FS](AD-FS-Deployment-Topology-Considerations.md).  
  
## <a name="memory-cpu-and-disk-space-requirements"></a>Mémoire, UC et espace disque requis  
Heureusement, la mémoire, l’UC et l’espace disque requis pour les serveurs de Fédération sont modestes, et ils ne sont pas susceptibles d’être un facteur déterminant en matière de prise de décision matérielle. Pour plus d’informations sur la configuration matérielle requise, consultez l' [annexe A : examen de la configuration requise pour AD FS](Appendix-A--Reviewing-AD-FS-Requirements.md).  
  
> [!NOTE]  
> Dans les tests qui ont été effectués par l’équipe de produit AD FS à l’aide d’une batterie de serveurs de Fédération configurée avec un SQL Server dédié pour stocker la base de données de configuration AD FS, la charge globale sur le SQL Server a tendance à être faible. Dans un test utilisant une batterie de serveurs de\-de Fédération\-qui a été configurée pour utiliser un seul SQL Server, l’utilisation du processeur ne dépassait pas 10% en dépit du test qui permettait aux serveurs de Fédération de cibler l’utilisation.  
  
## <a name="estimate-the-number-of-federation-servers-for-your-organization"></a><a name="bk_estimatefs"></a>Estimer le nombre de serveurs de Fédération pour votre organisation  
Dans le but de rationaliser le processus de planification matérielle pour les serveurs de Fédération, l’équipe de produit AD FS a développé le AD FS feuille de calcul de dimensionnement de la capacité de planification. Cette feuille de calcul Excel comprend des fonctionnalités de calculatrice\-telles que les fonctionnalités attendues que vous fournissez sur les utilisateurs de votre organisation et retournent un nombre optimal recommandé de serveurs de Fédération pour votre environnement de production AD FS.  
  
> [!NOTE]  
> Le nombre de serveurs de Fédération que cette feuille de calcul recommande est basé sur les spécifications matérielles et réseau utilisées par l’équipe de produit AD FS lors des tests. Par conséquent, le nombre de serveurs de Fédération recommandés par la feuille de calcul doit être compris dans ce contexte.  Pour plus d’informations sur les spécifications utilisées pendant le test, consultez la rubrique [planification de la capacité de AD FS Server](Planning-for-AD-FS-Server-Capacity.md).  
  
### <a name="using-the-ad-fs-capacity-planning-sizing-spreadsheet"></a>Utilisation de la feuille de calcul de dimensionnement de la planification de la capacité AD FS  
Lorsque vous utilisez cette feuille de calcul, vous devez sélectionner une valeur \(**40%** , **60%** ou **80%** \) qui représente au mieux le pourcentage du nombre total d’utilisateurs que vous prévoyez d’envoyer des demandes d’authentification à vos serveurs de Fédération pendant les périodes d’utilisation maximale.  
  
Ensuite, vous devez sélectionner une valeur \(**1 minute**, **15 minutes**ou **1 heure**\) qui représente le plus la durée attendue de la période d’utilisation maximale. Par exemple, vous pouvez estimer 40% comme valeur du nombre total d’utilisateurs qui se connecteront dans un délai de 15 minutes, ou que 60% des utilisateurs se connecteront dans une période de 1 heure. Ensemble, ces valeurs définissent le profil de charge de pointe par lequel vos recommandations de dimensionnement seront calculées.  
  
Ensuite, vous devez spécifier le nombre total d’utilisateurs qui nécessitent une authentification unique\-sur l’accès à l’application prenant en charge les revendications cibles\-, selon que les utilisateurs sont :  
  
-   Connexion à Active Directory à partir d’un ordinateur local connecté physiquement à votre réseau d’entreprise \(via l’authentification intégrée de Windows\)  
  
-   Connexion à Active Directory à distance à partir d’un ordinateur qui n’est pas physiquement connecté à votre réseau d’entreprise \(via l’authentification intégrée Windows ou le nom d’utilisateur et le mot de passe\)  
  
-   À partir d’une autre organisation et tente d’accéder à l’application prenant en charge les revendications cibles\-à partir d’un partenaire approuvé  
  
-   À partir d’un fournisseur d’identité SAML 2,0 et tente d’accéder à l’application prenant en charge les revendications cibles\-  
  
#### <a name="how-to-use-this-spreadsheet"></a>Utilisation de cette feuille de calcul  
Vous pouvez utiliser les étapes suivantes pour chaque instance de batterie de serveurs de Fédération que vous prévoyez de déployer pour déterminer le nombre recommandé de serveurs de Fédération.  
  
1.  Téléchargez, puis ouvrez la [feuille de calcul de dimensionnement de la planification de la capacité de AD FS pour Windows server 2012 R2](https://adfsdocs.blob.core.windows.net/adfs/ADFSCapacityPlanning.xlsx) ou la [feuille de calcul de dimensionnement de la capacité AD FS pour Windows Server 2016](https://adfsdocs.blob.core.windows.net/adfs/ADFSCapacity2016.xlsx).
  
2.  Dans la cellule située à droite du **au cours de la période de pic d’utilisation du système, je m’attend à ce que le pourcentage de mes utilisateurs s’authentifie** , puis à cliquer sur la cellule et à utiliser les flèches de déplacement\-vers le bas pour sélectionner le niveau d’utilisation estimé du système, soit **40%** , **60%** ou **80%** pour le déploiement.  
  
3.  Dans la cellule située à droite de la cellule **dans la période de temps suivante** , cliquez sur la cellule, puis utilisez les flèches de déplacement\-vers le bas pour sélectionner **1 minute**, **15 minutes**ou **1 heure** pour sélectionner la durée du pic de charge.  
  
4.  Dans la cellule située à droite de l' **entrée estimation du nombre d’applications internes \(telles que SharePoint \(2007 ou 2010\) ou les applications Web prenant en charge les revendications\)** cellule, tapez le nombre d’applications internes que vous allez utiliser dans votre organisation.  
  
5.  Dans la cellule située à droite de la cellule **entrer le nombre estimé d’applications en ligne \(telles que Office 365 Exchange Online, SharePoint Online ou Lync online\)** cellule, tapez le nombre d’applications ou de services en ligne que vous allez utiliser dans votre organisation.  
  
6.  Dans la cellule intitulée **nombre d’utilisateurs**, tapez un nombre sur chaque ligne qui s’applique à un exemple de scénario d’application. vos utilisateurs ont besoin d’une authentification unique\-pour accéder à. Cette colonne doit contenir le nombre d’utilisateurs définis, et non le nombre maximal d’utilisateurs par seconde. Si les tentatives d’accès à l’application doivent d’abord passer par la page de découverte de domaine d’hébergement, tapez **Y**. Si vous n’êtes pas sûr de cette sélection, tapez **o**.  
  
7.  Passez en revue les valeurs recommandées suivantes qui sont fournies :  
  
    1.  Pour connaître le nombre total de serveurs de Fédération recommandés, consultez la cellule inférieure droite mise en surbrillance en gris.  
  
    2.  Pour le nombre de serveurs recommandés pour chaque exemple de scénario d’application, consultez la cellule sur la ligne mise en surbrillance en gris.  
  
> [!NOTE]  
> La valeur qui sera automatiquement calculée dans la cellule à droite de la cellule intitulée **nombre total de serveurs de Fédération recommandés** au bas de la feuille de calcul contient une formule qui ajoute une mémoire tampon supplémentaire de 20% à la somme totale de toutes les valeurs de chacune des lignes précédentes. La formule ajoutée à la cellule **nombre total de serveurs de Fédération recommandé** est générée dans ce tampon sur le nombre total recommandé de serveurs de Fédération déployés, afin de rendre très peu probable que la charge globale sur la batterie de serveurs atteigne le point de saturation.  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
