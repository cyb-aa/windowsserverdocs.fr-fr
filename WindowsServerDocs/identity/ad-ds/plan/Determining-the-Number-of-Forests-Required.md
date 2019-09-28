---
ms.assetid: 173b72c1-ac83-4f42-abab-cf58f43769f0
title: Détermination du nombre de forêts nécessaires
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 21bece55ef64a552ddc641befd94d3ce19e78db6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408884"
---
# <a name="determining-the-number-of-forests-required"></a>Détermination du nombre de forêts nécessaires

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Pour déterminer le nombre de forêts que vous devez déployer, vous devez identifier et évaluer soigneusement les exigences d’isolation et d’autonomie pour chaque groupe de votre organisation et mapper ces spécifications aux modèles de conception de forêt appropriés.  
  
Lorsque vous déterminez le nombre de forêts à déployer pour votre organisation, tenez compte des points suivants :  
  
-   Les exigences d’isolation limitent vos choix de conception. Par conséquent, si vous identifiez les exigences d’isolation, assurez-vous que les groupes requièrent réellement l’isolation des données et que l’autonomie des données n’est pas suffisante pour répondre à leurs besoins. Veillez à ce que les différents groupes de votre organisation comprennent clairement les concepts d’isolation et d’autonomie.  
  
-   La négociation de la conception peut être un processus long. Il peut être difficile pour les groupes d’arriver à un accord sur la propriété et les utilisations des ressources disponibles. Veillez à laisser suffisamment de temps aux groupes de votre organisation pour effectuer des recherches adéquates afin d’identifier leurs besoins. Définir des échéances fermes pour les décisions de conception et obtenir le consensus de toutes les parties sur les échéances établies.  
  
-   La détermination du nombre de forêts à déployer implique l’équilibrage des coûts par rapport aux avantages. Un modèle à forêt unique est l’option la plus rentable et requiert le moins de charge administrative. Bien qu’un groupe de l’Organisation puisse préférer des opérations de service autonomes, il peut être plus rentable pour l’organisation de s’abonner à la livraison de service à partir d’un groupe informatique centralisé et approuvé. Cela permet au groupe de gérer ses données sans créer les coûts supplémentaires de la gestion des services. L’équilibrage des coûts par rapport aux avantages peut nécessiter l’intervention du sponsor exécutif.  
  
    Une seule forêt est la configuration la plus simple à gérer. Cela permet une collaboration maximale au sein de l’environnement, car :  
  
    -   Tous les objets d’une même forêt sont répertoriés dans le catalogue global. Par conséquent, aucune synchronisation entre les forêts n’est requise.  
  
    -   La gestion d’une infrastructure en double n’est pas obligatoire.  
  
-   Nous ne recommandons pas la copropriété d’une forêt unique par deux organisations informatiques distinctes et autonomes. À l’avenir, les objectifs des deux groupes informatiques peuvent changer, afin qu’ils ne puissent plus accepter le contrôle partagé.  
  
-   Nous ne recommandons pas l’administration du service d’externalisation à plusieurs partenaires externes. Les organisations multinationales qui possèdent des groupes dans des pays ou régions différents peuvent choisir d’externaliser l’administration des services à un autre partenaire externe pour chaque pays ou région. Étant donné que plusieurs partenaires extérieurs ne peuvent pas être isolés les uns des autres, les actions d’un partenaire peuvent affecter le service de l’autre, ce qui rend difficile la responsabilité des partenaires envers leurs contrats de niveau de service.  
  
-   Une seule instance d’un domaine Active Directory doit exister à tout moment. Microsoft ne prend pas en charge le clonage, le fractionnement ou la copie de contrôleurs de domaine à partir d’un domaine lors d’une tentative d’établissement d’une deuxième instance du même domaine. Pour plus d’informations sur cette limitation, consultez la section suivante.  
  
## <a name="restructuring-limitations"></a>Limitations de la restructuration  
Lorsqu’une entreprise acquiert une autre société, une unité commerciale ou une gamme de produits, elle souhaitera peut-être également acquérir des ressources informatiques correspondantes auprès du vendeur. Plus précisément, l’acheteur peut souhaiter acquérir une partie ou la totalité des contrôleurs de domaine qui hébergent les comptes d’utilisateur, les comptes d’ordinateurs et les groupes de sécurité qui correspondent aux ressources de l’entreprise qui doivent être acquises. Les seules méthodes prises en charge pour que l’acheteur obtienne les ressources informatiques stockées dans la forêt Active Directory du vendeur sont les suivantes :  
  
1.  Acquérir la seule instance de la forêt, y compris tous les contrôleurs de domaine et les données d’annuaire de l’ensemble de la forêt du vendeur.  
  
2.  Migrez les données d’annuaire nécessaires de la forêt ou des domaines du vendeur vers un ou plusieurs des domaines de l’acheteur. La cible d’une telle migration peut être une toute nouvelle forêt ou un ou plusieurs domaines existants déjà déployés dans la forêt de l’acheteur.  
  
Cette limitation de support existe, car :  
  
-   Chaque domaine d’une forêt Active Directory se voit affecter une identité unique lors de la création de la forêt. La copie de contrôleurs de domaine d’un domaine d’origine vers un domaine cloné compromet la sécurité des domaines et de la forêt. Les menaces pesant sur le domaine d’origine et le domaine cloné sont les suivantes :  
  
    -   Partage des mots de passe qui peuvent être utilisés pour accéder aux ressources  
  
    -   Insight sur les comptes et les groupes d’utilisateurs privilégiés  
  
    -   Mappage d’adresses IP aux noms d’ordinateurs  
  
    -   Ajouts, suppressions et modifications d’informations d’annuaire si des contrôleurs de domaine dans un domaine cloné établissent déjà une connectivité réseau avec les contrôleurs de domaine du domaine d’origine  
  
-   Les domaines clonés partagent une identité de sécurité commune ; par conséquent, les relations d’approbation ne peuvent pas être établies entre elles, même si l’un des domaines ou les deux ont été renommés.  
  
## <a name="in-this-section"></a>Dans cette section  
  
-   [Modèles de conception de forêt](https://technet.microsoft.com/library/cc770439.aspx)  
  
-   [Mappage des exigences de conception aux modèles de conception de forêt](Forest-Design-Models.md)  
  
-   [Utilisation du modèle de forêt de domaine d’organisation](../../ad-ds/plan/Using-the-Organizational-Domain-Forest-Model.md)  
  


