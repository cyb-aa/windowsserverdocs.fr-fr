---
ms.assetid: 7013fc21-9ced-4f9d-9588-cb04d6d60924
title: "Planification de la capacité de serveur de fédération"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 618dc9419be965dedaaf7dc946da436a5001f121
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="planning-for-federation-server-capacity"></a>Planification de la capacité de serveur de fédération

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Planification de capacité pour les serveurs de fédération vous permet d’évaluer:  
  
-   Les facteurs d’augmenter la taille de la base de données de configuration ADFS.  
  
-   La configuration matérielle appropriée pour chaque serveur de fédération.  
  
-   Le nombre de serveurs de fédération pour placer dans chaque organisation.  
  
Les serveurs de fédération émettent des jetons de sécurité pour les utilisateurs. Ces jetons sont présentés à une partie de confiance pour la consommation. Les serveurs de fédération émettent des jetons de sécurité après l’authentification d’un utilisateur ou après avoir reçu un jeton de sécurité émis par un Service de fédération du partenaire. Un jeton de sécurité est demandé à partir d’un Service de fédération lorsque les utilisateurs initialement se connecter à des applications fédérées ou l’expiration du délai de jetons de sécurité pendant qu’ils accèdent à des applications fédérées.  
  
Les serveurs de fédération sont conçus pour prendre en charge les configurations de batterie de serveurs serveur évolutifs disponibilité qui utilisent la technologie \(NLB\) l’équilibrage de charge réseau Microsoft. Les serveurs de fédération dans une configuration de batterie de serveurs peuvent traiter les demandes indépendamment, sans accéder à tous les composants de la batterie de serveurs courants pour chaque demande. Par conséquent, une faible surcharge est impliqué dans l’évolution d’un déploiement de serveurs de fédération.  
  
**Recommandations:**  
  
-   Pour mission\ critiques ou déploiements évolutifs disponibilité, nous vous recommandons de créer une batterie de serveurs de fédération petit dans chaque organisation partenaire, au moins deux serveurs de fédération par batterie de serveurs, pour fournir une tolérance de panne.  
  
-   Avec la nécessité d’une haute disponibilité et la facilité de mise à l’échelle des serveurs de fédération, évolution est la méthode recommandée pour gérer un nombre élevé de demandes par seconde pour un Service de fédération particulier. La montée en puissance au-delà de la configuration de base dans ce guide est peu susceptible d’importantes capacités gains de gestion.  
  
## <a name="ad-fs-configuration-database-size-and-growth"></a>Croissance et la taille de base de données configuration ADFS  
La taille de la base de données de configuration ADFS est généralement considérée comme petites et taille de la base de données ne pas ont tendance à être un facteur important dans les déploiements ADFS.  La taille exacte de la base de données de configuration ADFS peut dépendent en grande partie le nombre de relations d’approbation et les métadonnées associées trust\ liés, tels que les revendications, les règles de revendication et la surveillance des paramètres configurés pour chaque niveau de confiance. Au fur et à mesure du nombre d’entrées de confiance dans la base de données de configuration, par conséquent, ne le besoin de davantage d’espace disque.  
  
Pour plus d’informations de déploiement supplémentaires sur la base de données de configuration ADFS, consultez [considérations sur la topologie ADFS déploiement](AD-FS-Deployment-Topology-Considerations.md).  
  
## <a name="memory-cpu-and-disk-space-requirements"></a>La mémoire, processeur et espace disque requis  
Heureusement, la mémoire, processeur et espace disque requis pour les serveurs de fédération sont limités, et ils ne sont pas susceptibles d’être un facteur déterminant dans les décisions de matériel. Pour plus d’informations sur la configuration matérielle requise, voir [annexe a: examen des annonces de la configuration requise pour FS](Appendix-A--Reviewing-AD-FS-Requirements.md).  
  
> [!NOTE]  
> Dans les tests qui ont été effectuées par l’équipe de produit ADFS à l’aide d’une batterie de serveurs de fédération configurée avec un serveur SQL dédié pour stocker la base de données de configuration ADFS, la charge globale sur le serveur SQLServer avaient tendance à être faible. Dans un test à l’aide d’une batterie de serveurs four\-federation\-serveur qui a été configuré pour utiliser un seul serveur SQLServer, l’utilisation du processeur ne pas dépasser 10%, en dépit de tests de mettre les serveurs de fédération à l’utilisation de la cible.  
  
## <a name="bk_estimatefs"></a>Estimer le nombre de serveurs de fédération pour votre organisation  
Dans le but de rationaliser le processus pour les serveurs de fédération de planification de matériel, l’équipe du produit ADFS développé le ADFS la capacité de planification de la feuille de calcul sur. Cette feuille de calcul Excel inclut une fonctionnalité similaire calculator\ qui prendra utilisation attendue des données que vous fournissez sur les utilisateurs dans votre organisation et retourner un nombre optimal recommandé de serveurs de fédération pour votre environnement de production ADFS.  
  
> [!NOTE]  
> Le nombre de serveurs de fédération qui recommande cette feuille de calcul est basé sur les spécifications matérielles et réseau que l’équipe du produit ADFS utilisée au cours du test. Par conséquent, le nombre de serveurs de fédération qui recommande la feuille de calcul doit être compris dans ce contexte.  Pour plus d’informations sur les spécifications utilisées au cours du test, consultez la rubrique [planification de la capacité du serveur ADFS](Planning-for-AD-FS-Server-Capacity.md).  
  
### <a name="using-the-ad-fs-capacity-planning-sizing-spreadsheet"></a>À l’aide de la planification des feuille de calcul sur de la capacité ADFS  
Lorsque vous utilisez cette feuille de calcul, vous devez sélectionner une valeur \ (soit **40%**, **60%**, ou **80%**\) que mieux représente le pourcentage du nombre total d’utilisateurs vous attendez enverra les demandes d’authentification à vos serveurs de fédération pendant les périodes d’utilisation.  
  
Ensuite, vous devez sélectionner une valeur \ (soit **1minute**, **15minutes**, ou **1heure**\) que les meilleures représente la durée pendant laquelle vous prévoyez la période de l’utilisation de pointe vers la dernière. Par exemple, vous posez estimer 40% comme valeur pour le nombre total d’utilisateurs qui se connectera dans un délai de 15minutes ou 60% des utilisateurs se connectera dans un délai de 1heure. Ensemble, ces valeurs définissent le profil de charge pointe par lequel votre recommandation de dimensionnement est calculée.  
  
Ensuite, vous devez spécifier le nombre total d’utilisateurs qui nécessitent un accès à connexion unique à l’application prenant en charge claims\ cible, si les utilisateurs sont:  
  
-   Connexion à ActiveDirectory à partir d’un ordinateur local qui est physiquement connecté au réseau d’entreprise \ (via authentication\ intégrée de Windows)  
  
-   La journalisation dans ActiveDirectory à distance à partir d’un ordinateur qui n’est pas physiquement connecté au réseau d’entreprise \ (par le biais de Windows intégré d’authentification ou nom d’utilisateur et password\)  
  
-   À partir d’une autre organisation et sont tente d’accéder à l’application prenant en charge claims\ cible à partir d’un partenaire de confiance  
  
-   À partir d’un fournisseur d’identité SAML 2.0 et le sont tente d’accéder à l’application prenant en charge claims\ cible  
  
#### <a name="how-to-use-this-spreadsheet"></a>Comment utiliser cette feuille de calcul  
Vous pouvez utiliser les étapes suivantes pour chaque instance de batterie de serveurs de serveur de fédération que vous prévoyez de déployer pour déterminer le nombre recommandé de serveurs de fédération.  
  
1.  Téléchargez et ouvrez le [ADFS capacité planification de la feuille de calcul sur pour Windows Server2012R2](https://adfsdocs.blob.core.windows.net/adfs/ADFSCapacityPlanning.xlsx) ou [ADFS capacité planification de la feuille de calcul sur pour Windows Server2016](https://adfsdocs.blob.core.windows.net/adfs/ADFSCapacity2016.xlsx).
  
2.  Dans la cellule à droite de la **attendre pendant la période de l’utilisation du système de pointe, ce pourcentage de mes utilisateurs s’authentifient** cellule, cliquez sur la cellule et puis utilisez les flèches déroulantes pour sélectionner votre niveau d’utilisation estimée système, soit **40%**, **60%** ou **80%** pour le déploiement.  
  
3.  Dans la cellule à droite de la **pendant la période de temps suivante** cellule, cliquez sur la cellule et puis utilisez les flèches déroulantes pour sélectionner soit **1minute**, **15minutes**, ou **1heure** pour sélectionner la durée de la charge maximale.  
  
4.  Dans la cellule à droite de la **entrée estimation du nombre d’applications internes \ (tels que SharePoint \(2007 or 2010\) ou applications\ web prenant en charge des revendications)** cellule, tapez le nombre d’applications internes que vous utiliserez dans votre organisation.  
  
5.  Dans la cellule à droite de la **entrée estimation du nombre d’applications en ligne \ (par exemple, Office 365 Exchange Online, SharePoint Online ou Lync Online\)** cellule, tapez le nombre d’applications en ligne ou services vous serez utilisés dans votre organisation.  
  
6.  Dans la cellule intitulée **nombre d’utilisateurs**, tapez un numéro sur chaque ligne qui s’applique à un exemple de scénario d’application de vos utilisateurs est besoin d’accès de connexion unique. Cette colonne doit contenir le nombre d’utilisateurs définis, pas les utilisateurs de pointe par seconde. Si les tentatives d’accès à l’application doivent d’abord passer par le biais de la page de découverte de domaine d’accueil, tapez **Y**. Si vous n’êtes pas sûr de cette sélection, tapez **Y**.  
  
7.  Passez en revue les conseils de valeurs qui sont fournies ci-dessous:  
  
    1.  Pour le nombre total de serveurs de fédération recommandée, reportez-vous à la cellule inférieure droite qui est mis en surbrillance en gris.  
  
    2.  Pour le nombre de serveurs recommandés pour chaque exemple de scénario d’application, reportez-vous à la cellule sur la ligne qui est mis en surbrillance en gris.  
  
> [!NOTE]  
> La valeur calculée automatiquement dans la cellule à droite de la cellule intitulée **nombre Total de serveurs de fédération recommandé** en bas de la feuille de calcul contient une formule qui ajoute un tampon de 20% supplémentaires à la somme totale de toutes les valeurs dans chacune des lignes individuelles précède. La formule ajoutée à la **nombre Total de serveurs de fédération recommandé** cellule builds dans cette mémoire tampon à votre total recommandé le nombre de serveurs de fédération déployés pour le rendre très peu de chances que la charge globale sur la batterie de serveurs sera atteint jamais son point de saturation.  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception ADFS dans Windows Server2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
