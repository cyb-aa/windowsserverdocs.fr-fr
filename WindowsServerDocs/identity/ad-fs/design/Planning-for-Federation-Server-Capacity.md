---
ms.assetid: 7013fc21-9ced-4f9d-9588-cb04d6d60924
title: Planification de la capacité des serveurs de fédération
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 569bea74fe7750eaf2b410a552876e0862b1e24b
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191094"
---
# <a name="planning-for-federation-server-capacity"></a>Planification de la capacité des serveurs de fédération

Planification de capacité pour les serveurs de fédération permet d’évaluer :  
  
-   Les facteurs qui augmente la taille de la base de données de configuration AD FS.  
  
-   La configuration matérielle appropriée pour chaque serveur de fédération.  
  
-   Le nombre de serveurs de fédération à placer dans chaque organisation.  
  
Serveurs de fédération délivrent des jetons de sécurité aux utilisateurs. Ces jetons sont présentés à une partie de confiance pour la consommation. Serveurs de fédération délivrent des jetons de sécurité après l’avoir authentifié un utilisateur ou après la réception d’un jeton de sécurité précédemment émis par un Service de fédération du partenaire. Un jeton de sécurité est demandé à partir d’un Service de fédération quand les utilisateurs initialement se connectent aux applications fédérées ou expiration du délai de jetons de sécurité pendant qu’ils accèdent à des applications fédérées.  
  
Serveurs de fédération sont conçus pour prendre en charge la haute\-les configurations de batterie de serveurs de disponibilité qui utilisent l’équilibrage de charge réseau Microsoft \(NLB\) technologie. Serveurs de fédération dans une configuration de batterie de serveurs peuvent traiter les demandes de manière indépendante, sans accéder à n’importe quel composants communs de la batterie de serveurs pour chaque demande. Par conséquent, peu de surcharge est impliqué dans la montée en charge un déploiement de serveurs de fédération.  
  
**Recommandations :**  
  
-   Pour la mission\-critique ou élevé\-déploiements de disponibilité, nous vous recommandons de créer une batterie de serveurs de fédération petite dans chaque organisation partenaire, au moins deux serveurs de fédération par batterie de serveurs, pour fournir une tolérance de panne.  
  
-   Avec la nécessité d’une haute disponibilité et la facilité de mise à l’échelle des serveurs de fédération, la montée en charge est la méthode recommandée pour gérer un nombre élevé de demandes par seconde pour un Service de fédération particulier. Il est peu probable produire d’importantes capacités de gestion des gains de mise à l’échelle au-delà de la configuration de base dans ce guide.  
  
## <a name="ad-fs-configuration-database-size-and-growth"></a>Croissance et la taille de base de données configuration AD FS  
La taille de la base de données de configuration AD FS est généralement considéré comme small et taille de la base de données n’ont pas tendance à être un facteur important dans les déploiements AD FS.  La taille précise de la base de données de configuration AD FS peut dépendre largement le nombre de relations d’approbation et l’approbation associée\-métadonnées connexes, telles que les revendications, les règles de revendication et la surveillance des paramètres configurés pour chaque niveau de confiance. Au fur et à mesure du nombre d’entrées de confiance dans la base de données de configuration, tout comme le fait le besoin de davantage d’espace disque.  
  
Pour plus d’informations supplémentaires sur le déploiement sur la base de données de configuration AD FS, consultez [considérations sur la topologie AD FS déploiement](AD-FS-Deployment-Topology-Considerations.md).  
  
## <a name="memory-cpu-and-disk-space-requirements"></a>Mémoire, processeur et espace disque requis  
Heureusement, mémoire, processeur et espace disque requis pour les serveurs de fédération sont modestes, et ils ne sont pas susceptibles d’être un facteur important dans les décisions de matériel. Pour plus d’informations sur la configuration matérielle requise, consultez [annexe a : Examen des exigences en matière de AD FS](Appendix-A--Reviewing-AD-FS-Requirements.md).  
  
> [!NOTE]  
> Dans les tests qui ont été effectuées par l’équipe de produit AD FS à l’aide d’une batterie de serveurs de fédération configurée avec un serveur SQL dédié pour stocker la base de données de configuration AD FS, la charge globale sur le serveur SQL Server avaient tendance à être faible. Dans un test à l’aide de quatre\-fédération\-batterie de serveurs qui a été configuré pour utiliser un serveur SQL unique, l’utilisation du processeur n’a pas dépassé 10 %, en dépit de test que les serveurs de fédération à l’utilisation de la cible.  
  
## <a name="bk_estimatefs"></a>Estimer le nombre de serveurs de fédération pour votre organisation  
Dans le but de rationaliser le processus pour les serveurs de fédération de planification de matériel, l’équipe de produit AD FS développé l’AD FS capacité planification de la feuille de calcul sur. Cette feuille de calcul Excel inclut calculatrice\-à des fonctionnalités qui effectuera des données d’utilisation attendu vous fournir sur les utilisateurs de votre organisation et retourner un nombre optimal recommandé de serveurs de fédération pour votre environnement de production AD FS .  
  
> [!NOTE]  
> Le nombre de serveurs de fédération qui vous recommande de cette feuille de calcul est basé sur les spécifications matérielles et réseau que l’équipe de produit AD FS utilisée pendant le test. Par conséquent, le nombre de serveurs de fédération qui vous recommande de la feuille de calcul doit être compris dans ce contexte.  Pour plus d’informations sur les spécifications utilisées pendant le test, consultez la rubrique intitulée [planification de la capacité de serveur AD FS](Planning-for-AD-FS-Server-Capacity.md).  
  
### <a name="using-the-ad-fs-capacity-planning-sizing-spreadsheet"></a>À l’aide de la planification de la feuille de calcul sur la capacité AD FS  
Lorsque vous utilisez cette feuille de calcul, vous devez sélectionner une valeur \(soit **40 %** , **60 %** , ou **80 %** \) que représente le mieux le pourcentage de Nombre total d’utilisateurs que vous prévoyez d’envoyer des demandes d’authentification à vos serveurs de fédération pendant les pics d’utilisation.  
  
Ensuite, vous devez sélectionner une valeur \(soit **1 minute**, **15 minutes**, ou **1 heure** \) que représente la durée pendant laquelle vous attendez le mieux la période d’utilisation de pointe pour durer. Par exemple, vous pourrez estimer 40 % comme valeur pour le nombre total d’utilisateurs qui permettra de vous connecter dans un délai de 15 minutes, ou qui permettra de vous connecter 60 % des utilisateurs dans un délai de 1 heure. Ensemble, ces valeurs définissent le profil de charge maximale par lequel votre recommandation de dimensionnement sera calculée.  
  
Ensuite, vous devez spécifier le nombre total d’utilisateurs qui nécessitent l’authentification unique\-sur l’accès aux revendications cible\-application prenant en charge, selon que les utilisateurs sont :  
  
-   Journalisation dans Active Directory à partir d’un ordinateur local qui est physiquement connecté au réseau d’entreprise \(via l’authentification intégrée de Windows\)  
  
-   Journalisation dans Active Directory à distance à partir d’un ordinateur qui n’est pas physiquement connecté au réseau d’entreprise \(via Windows intégré l’authentification ou nom d’utilisateur et mot de passe\)  
  
-   À partir d’une autre organisation et sont tente d’accéder aux revendications cible\-application prenant en charge à partir d’un partenaire de confiance  
  
-   À partir d’un fournisseur d’identité SAML 2.0 et vous êtes tente d’accéder aux revendications cible\-application prenant en charge  
  
#### <a name="how-to-use-this-spreadsheet"></a>Comment utiliser cette feuille de calcul  
Vous pouvez utiliser les étapes suivantes pour chaque instance de batterie de serveur de fédération que vous envisagez de déployer pour déterminer le nombre recommandé de serveurs de fédération.  
  
1.  Téléchargez et ouvrez le [AD FS planification dimensionnement feuille de calcul capacité pour Windows Server 2012 R2](https://adfsdocs.blob.core.windows.net/adfs/ADFSCapacityPlanning.xlsx) ou [AD FS planification dimensionnement feuille de calcul capacité pour Windows Server 2016](https://adfsdocs.blob.core.windows.net/adfs/ADFSCapacity2016.xlsx).
  
2.  Dans la cellule à droite de la **durant la période de l’utilisation de système de pointe, j’attends ce pourcentage de mes utilisateurs s’authentifient** de cellule, cliquez sur la cellule de la liste déroulante\-flèches de direction pour sélectionner l’utilisation de vos logiciels et matériels le niveau, soit **40 %** , **60 %** ou **80 %** pour le déploiement.  
  
3.  Dans la cellule à droite de la **au sein de la période de temps suivante** de cellule, cliquez sur la cellule de la liste déroulante\-flèches de direction pour sélectionner soit **1 minute**, **les15minutes**, ou **1 heure** pour sélectionner la durée de la charge maximale.  
  
4.  Dans la cellule à droite de la **entrée estimation du nombre d’applications internes \(tels que SharePoint \(2007 ou 2010\) ou les applications web prenant en charge des revendications\)**  de cellule, tapez le numéro des applications internes, vous allez utiliser dans votre organisation.  
  
5.  Dans la cellule à droite de la **entrée estimation du nombre d’applications en ligne \(tels que Office 365 Exchange Online, SharePoint Online ou Lync Online\)**  de cellule, tapez le nombre d’applications en ligne ou Services, vous allez utilisés dans votre organisation.  
  
6.  Sous la cellule intitulée **nombre d’utilisateurs**, tapez un nombre sur chaque ligne qui s’applique à un exemple de scénario d’application de vos utilisateurs est besoin l’authentification unique\-sur l’accès à. Cette colonne doit contenir le nombre d’utilisateurs définis, pas les utilisateurs de pointe par seconde. Si les tentatives d’accès à l’application doivent tout d’abord passer par la page de découverte du domaine d’accueil, tapez **Y**. Si vous ne connaissez pas cette sélection, tapez **Y**.  
  
7.  Passez en revue les conseils des valeurs qui sont fournies ci-dessous :  
  
    1.  Pour obtenir le nombre total de serveurs de fédération recommandée, reportez-vous à la cellule de droite inférieure qui est mis en surbrillance en gris.  
  
    2.  Pour le nombre de serveurs recommandés pour chaque exemple de scénario d’application, reportez-vous à la cellule sur la ligne qui est mis en surbrillance en gris.  
  
> [!NOTE]  
> La valeur qui sera calculée automatiquement dans la cellule à droite de la cellule intitulée **nombre Total de serveurs de fédération recommandé** en bas de la feuille de calcul contient une formule qui ajoutera un tampon de 20 % supplémentaires pour le somme totale de toutes les valeurs dans chacune des lignes individuelles qui la précède. La formule ajoutée à la **nombre Total de serveurs de fédération recommandé** génère de cellule dans cette mémoire tampon à votre total de serveurs de fédération déployé pour le rendre très peu probable que la charge globale sur la batterie de serveurs atteindra jamais le nombre recommandé son point de saturation.  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
