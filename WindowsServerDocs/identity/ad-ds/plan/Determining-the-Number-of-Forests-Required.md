---
ms.assetid: 173b72c1-ac83-4f42-abab-cf58f43769f0
title: "Détermination du nombre de forêts nécessaires"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: a461dbb2b5bf9d2ca1bb6a336cb11ace775fb1cd
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="determining-the-number-of-forests-required"></a>Détermination du nombre de forêts nécessaires

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Pour déterminer le nombre de forêts que vous devez déployer, vous devez soigneusement identifier et évaluer les exigences d’isolation et d’autonomie pour chaque groupe de votre organisation et mapper ces exigences pour les modèles de conception de forêt appropriée.  
  
Lorsque vous déterminez le nombre de forêts à déployer pour votre organisation, procédez comme suit:  
  
-   Configuration requise pour l’isolation limiter vos choix de conception. Par conséquent, si vous identifiez les besoins d’isolation, assurez-vous que les groupes ont réellement besoin d’isolation des données et que l’autonomie des données n’est pas suffisante pour leurs besoins. Vérifiez que les différents groupes de votre organisation comprennent clairement les concepts d’isolation et d’autonomie.  
  
-   Négociation de la conception peut être un processus long. Il peut être difficile pour les groupes à propos de la propriété d’un contrat et utilise des ressources disponibles. Assurez-vous d’allouer suffisamment de temps pour les groupes dans votre organisation pour effectuer des recherches adéquates pour identifier leurs besoins. Définissez des échéances rigide pour les décisions de conception et obtenir de consensus à partir de toutes les parties sur les délais établies.  
  
-   Détermination du nombre de forêts pour déployer implique les coûts contre les avantages de l’équilibrage de. Un modèle de forêt unique est l’option la plus rentable et nécessite le moins de surcharge administrative. Même si un groupe dans l’organisation pouvez préférer les opérations de service autonome, il peut être plus rentable pour l’organisation pour vous abonner à la prestation de services d’un groupe de technologies de l’informations centralisée et approuvées. Cela permet au groupe de gestion des données propres sans avoir à créer des coûts supplémentaires pour la gestion des services. Avantages des coûts d’équilibrage de la peut nécessiter l’entrée à partir de soutien de la direction.  
  
    Une seule forêt est la configuration plus simple à gérer. Il permet une collaboration maximale au sein de l’environnement, car:  
  
    -   Tous les objets dans une forêt unique sont répertoriés dans le catalogue global. Par conséquent, aucune synchronisation entre les forêts n’est nécessaire.  
  
    -   Gestion d’une infrastructure en double n’est pas nécessaire.  
  
-   Nous ne recommandons pas de co-propriété d’une forêt unique par deux organisations informatiques distinctes et autonomes. À l’avenir, les objectifs de deux groupes informatiques peuvent changer, afin qu’ils ne peuvent plus accepter un contrôle partagé.  
  
-   Nous ne recommandons pas l’administration des services externalisation à plusieurs en dehors du partenaire. Internationaux organisations qui ont des groupes dans différents pays ou régions peuvent choisir de sous-traiter l’administration des services vers un autre partenaire externe pour chaque pays ou région. Étant donné que plusieurs partenaires externes ne peut pas être isolées les unes des autres, les actions d’un partenaire peuvent affecter le service de l’autre, ce qui rend difficile contenir les partenaires envers leurs contrats de niveau de service.  
  
-   Une seule instance d’un domaine ActiveDirectory doit déjà exister à tout moment. Microsoft ne gère pas le clonage, le fractionnement ou la copie des contrôleurs de domaine d’un domaine de tenter d’établir une deuxième instance du même domaine. Pour plus d’informations sur cette limitation, consultez la section suivante.  
  
## <a name="restructuring-limitations"></a>Limitations de restructuration  
Lorsqu’une entreprise acquiert une autre société, division, ou gamme de produits, la société peut également acquérir des ressources informatiques correspondants au vendeur. Plus précisément, l’acheteur souhaiterez acquérir certains ou tous les contrôleurs de domaine qui hébergent les comptes d’utilisateurs, les comptes d’ordinateurs et les groupes de sécurité qui correspondent aux ressources d’entreprise qui doivent être acquises. Les méthodes prises en charge uniquement pour l’acheteur d’acquérir les ressources informatiques qui sont stockés dans la forêt ActiveDirectory du vendeur sont les suivantes:  
  
1.  Acquérir l’instance de la forêt, y compris tous les contrôleurs de domaine et les données d’annuaire de forêt le vendeur.  
  
2.  Migrer les données de répertoire requis à partir de la forêt ou des domaines du vendeur à un ou plusieurs des domaines de l’acheteur. La cible pour cette migration peut être un entièrement nouvelle forêt ou un ou plusieurs domaines existants qui sont déjà déployées dans la forêt de l’acheteur.  
  
Cette limitation existe, car:  
  
-   Chaque domaine dans une forêt ActiveDirectory est attribué une identité unique lors de la création de la forêt. Copie des contrôleurs de domaine à partir d’un domaine d’origine à une compromission de domaine cloné la sécurité des domaines et de la forêt. Menaces pour le domaine d’origine et le domaine cloné sont les suivantes:  
  
    -   Partage de mots de passe peuvent être utilisés pour accéder aux ressources  
  
    -   Pour mieux concernant les comptes d’utilisateurs privilégiés et groupes  
  
    -   Mappage d’adresses IP aux noms d’ordinateur  
  
    -   Ajouts, suppressions et les modifications des informations d’annuaire si les contrôleurs de domaine dans un domaine cloné jamais à établir la connectivité réseau avec des contrôleurs de domaine du domaine d’origine  
  
-   Domaines clonés partagent une identité commune de sécurité; par conséquent, les relations d’approbation ne peut pas être établies entre eux, même si une ou les deux domaines ont été renommés.  
  
## <a name="in-this-section"></a>Dans cette section  
  
-   [Modèles de conception de forêt](https://technet.microsoft.com/library/cc770439.aspx)  
  
-   [Mappage des exigences de conception aux modèles de conception de forêt](Forest-Design-Models.md)  
  
-   [L’utilisation du modèle de forêt de domaines d’organisation](../../ad-ds/plan/Using-the-Organizational-Domain-Forest-Model.md)  
  


