---
ms.assetid: 173b72c1-ac83-4f42-abab-cf58f43769f0
title: Détermination du nombre de forêts nécessaires
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 1721190bf592b6f7a1274d60d47bbc755eeff1c2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820650"
---
# <a name="determining-the-number-of-forests-required"></a>Détermination du nombre de forêts nécessaires

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Pour déterminer le nombre de forêts que vous devez déployer, vous devez soigneusement identifier et évaluer les exigences d’isolation et d’autonomie pour chaque groupe dans votre organisation et comment atteindre ces objectifs pour les modèles de conception de forêt appropriée.  
  
Lors de la détermination du nombre de forêts pour déployer dans votre organisation, considérez les points suivants :  
  
-   Exigences d’isolation limitent vos choix de conception. Par conséquent, si vous identifiez les exigences d’isolation, assurez-vous que les groupes nécessitent en fait d’isolation des données et que l’autonomie des données n’est pas suffisante pour leurs besoins. Vérifiez que les différents groupes de votre organisation comprennent clairement les concepts d’isolation et d’autonomie.  
  
-   Négociation de la conception peut prendre du temps. Il peut s’avérer difficile pour les groupes de parvenir à un accord sur la propriété et l’utilise pour les ressources disponibles. Assurez-vous d’allouer suffisamment de temps pour les groupes dans votre organisation pour effectuer des recherches adéquates pour identifier leurs besoins. Définissez des échéances fermes pour prendre des décisions de conception et obtenir un consensus à partir de toutes les parties sur les délais établis.  
  
-   Détermination du nombre de forêts pour déployer implique l’équilibrage des coûts par rapport à des avantages. Un modèle de forêt unique est l’option la plus rentable et nécessite le moins de surcharge administrative. Bien qu’un groupe dans l’organisation peut préférer des opérations de service autonome, il peut être plus rentable pour l’organisation pour vous abonner à la prestation de services d’un groupe d’informatique centralisé des informations fiables. Ainsi, le groupe de gestion des données propre sans créer de coûts supplémentaires pour la gestion des services. Équilibrage des coûts par rapport à des avantages peut nécessiter une entrée à partir du sponsor exécutif.  
  
    Une seule forêt est la configuration le plus simple à gérer. Il permet une collaboration maximale au sein de l’environnement, car :  
  
    -   Tous les objets dans une forêt unique sont répertoriés dans le catalogue global. Par conséquent, aucune synchronisation entre les forêts n’est nécessaire.  
  
    -   Gestion d’une infrastructure en double n’est pas nécessaire.  
  
-   Nous déconseillons copropriété d’une forêt unique par deux les organisations informatiques distinctes et autonomes. À l’avenir, les objectifs des deux groupes informatiques peuvent changer, afin qu’ils ne peuvent plus accepter contrôle partagé.  
  
-   Nous ne recommandons pas l’administration des services de sous-traitance à plusieurs en dehors du partenaire. Les organisations multinationales qui ont des groupes dans différents pays ou régions peuvent choisir externaliser l’administration du service à un autre partenaire externe pour chaque pays ou région. Étant donné que plusieurs partenaires extérieurs ne peut pas être isolés les uns des autres, les actions d’un partenaire peuvent affecter le service de l’autre, ce qui rend difficile contenir les partenaires envers leurs contrats de niveau de service.  
  
-   Une seule instance d’un domaine Active Directory doit exister à tout moment. Microsoft ne prend pas en charge le clonage, le fractionnement, copie contrôleurs de domaine ou d’un domaine dans une tentative pour établir une deuxième instance du même domaine. Pour plus d’informations sur cette limitation, consultez la section suivante.  
  
## <a name="restructuring-limitations"></a>Restructuration de limitations  
Quand une entreprise acquiert une autre société, unité commerciale, ou ligne de produits, la société peut également acquérir des ressources informatiques correspondantes du vendeur. Plus précisément, l’acheteur souhaiterez acquérir tout ou partie des contrôleurs de domaine qui hébergent les comptes d’utilisateur, les comptes d’ordinateurs et les groupes de sécurité qui correspondent aux ressources d’entreprise qui doivent être acquises. Les seules méthodes prises en charge pour l’acheteur acquérir les ressources informatiques qui sont stockés dans la forêt d’Active Directory du vendeur sont les suivantes :  
  
1.  Acquérir la seule instance de la forêt, y compris tous les contrôleurs de domaine et les données d’annuaire dans l’ensemble de la forêt du vendeur.  
  
2.  Migrer les données d’annuaire nécessaires à partir de la forêt ou des domaines du vendeur à un ou plusieurs des domaines de l’acheteur. La cible pour une telle migration peut être un entièrement nouvelle forêt ou un ou plusieurs domaines existants qui sont déjà déployées dans la forêt de l’acheteur.  
  
Cette limitation existe parce que :  
  
-   Chaque domaine dans une forêt Active Directory est affecté à une identité unique lors de la création de la forêt. Copie de contrôleurs de domaine à partir d’un domaine d’origine à une compromission de domaine cloné la sécurité des domaines et de la forêt. Menaces pour le domaine d’origine et le domaine cloné sont les suivantes :  
  
    -   Partage des mots de passe qui peuvent être utilisées pour accéder aux ressources  
  
    -   Informations concernant les comptes d’utilisateur disposant de privilèges et les groupes  
  
    -   Mappage d’adresses IP pour les noms d’ordinateurs  
  
    -   Ajouts, suppressions et les modifications des informations d’annuaire si les contrôleurs de domaine dans un domaine cloné jamais établissent une connectivité de réseau avec des contrôleurs de domaine du domaine d’origine  
  
-   Domaines clonés partagent une identité de sécurité commune ; Par conséquent, relations d’approbation ne peut pas être établies entre eux, même si un ou les deux domaines ont été renommées.  
  
## <a name="in-this-section"></a>Dans cette section  
  
-   [Modèles de conception de forêt](https://technet.microsoft.com/library/cc770439.aspx)  
  
-   [Mappage des exigences de conception aux modèles de conception de forêt](Forest-Design-Models.md)  
  
-   [À l’aide du modèle de forêt de domaines d’organisation](../../ad-ds/plan/Using-the-Organizational-Domain-Forest-Model.md)  
  


