---
ms.assetid: ef4ef4a9-8969-4ad0-bd17-b2bb24f36ef6
title: "Sélection du domaine racine de forêt"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: d87273df1e42e1fa88df09e0b86d4c86ea75ee3f
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="selecting-the-forest-root-domain"></a>Sélection du domaine racine de forêt

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Le premier domaine que vous déployez dans une forêt ActiveDirectory est appelé le domaine racine de forêt. Ce domaine reste le domaine racine de forêt pour le cycle de vie du déploiement des services ADDS.  
  
Le domaine racine de forêt contient les groupes Administrateurs de l’entreprise et administrateurs du schéma. Ces groupes d’administrateurs de service sont utilisés pour gérer les opérations telles que l’ajout et la suppression de domaines et l’implémentation des modifications au schéma au niveau de la forêt.  
  
Sélection du domaine racine de forêt implique de déterminer si l’un des domaines ActiveDirectory dans votre conception de domaine puisse fonctionner en tant que le domaine racine de forêt, ou si vous avez besoin déployer un domaine racine de forêt dédiée.  
  
Pour plus d’informations sur le déploiement d’un domaine racine de forêt, consultez [déploiement d’un domaine racine de forêt Windows Server2008](https://technet.microsoft.com/library/cc731174.aspx).  
  
## <a name="choosing-a-regional-or-dedicated-forest-root-domain"></a>Choix d’un domaine racine de forêt dédiée ou régionales  
Si vous appliquez un modèle de domaine unique, le domaine unique fonctionne comme le domaine racine de forêt. Si vous appliquez un modèle à plusieurs domaines, vous pouvez choisir de déployer un domaine racine de forêt dédiée ou de sélectionner un domaine régional de fonctionner en tant que le domaine racine de forêt.  
  
### <a name="dedicated-forest-root-domain"></a>Domaine racine de forêt dédiée  
Un domaine racine de forêt dédiée est un domaine qui est créé spécifiquement pour fonctionner en tant que racine de la forêt. Il ne contient pas les comptes d’utilisateurs autres que les comptes d’administrateur de service pour le domaine racine de forêt. En outre, il ne représente pas une région géographique dans votre structure de domaine. Tous les autres domaines dans la forêt sont des enfants du domaine racine de forêt dédiée.  
  
À l’aide d’une racine de forêt dédiée offre les avantages suivants:  
  
-   Séparation opérationnelle forêt des administrateurs de service à partir des administrateurs de services de domaine. Dans un environnement de domaine unique, membres du groupe Administrateurs intégré et Admins du domaine peuvent utiliser standards outils et procédures eux-mêmes comme membres des groupes Administrateurs de l’entreprise et administrateurs du schéma. Dans une forêt qui utilise un domaine racine de forêt dédiée, membres du groupe Administrateurs intégré dans les domaines régionaux et Admins du domaine ne peut pas se faire connaître membres des groupes d’administrateur de service au niveau de la forêt à l’aide des procédures et des outils standards.  
  
-   Protection contre les modifications opérationnelles dans d’autres domaines. Un domaine racine de forêt dédiée ne représente pas une région géographique particulière dans votre structure de domaine. Pour cette raison, il n’est pas affecté par réorganisations ou d’autres modifications entraîner la modification du nom ou la restructuration de domaines.  
  
-   Fait Office d’une racine neutre afin qu’aucun pays ou la région ne semble être subordonnée à une autre région. Certaines organisations préfèrent afin d’éviter l’apparence qu’un pays ou région est subordonnée à un autre pays ou région dans l’espace de noms. Lorsque vous utilisez un domaine racine de forêt dédiée, tous les domaines régionaux peuvent être homologues dans la hiérarchie de domaine.  
  
Dans un environnement de domaines multiples-régionales dans lequel une racine de forêt dédiée est utilisée, la réplication du domaine racine de forêt a une incidence minimale sur l’infrastructure réseau. Il s’agit dans la mesure où la racine de forêt héberge uniquement les comptes d’administrateur de service. La plupart des comptes d’utilisateur dans la forêt et d’autres données spécifiques à un domaine sont stockés dans les domaines régionaux.  
  
Un des inconvénients de l’utilisation d’un domaine racine de forêt dédiée sont qu’il crée des frais de gestion supplémentaires pour prendre en charge le domaine supplémentaire.  
  
### <a name="regional-domain-as-a-forest-root-domain"></a>Domaine régional comme un domaine racine de forêt  
Si vous choisissez de ne pas déployer un domaine racine de forêt dédiée, vous devez sélectionner un domaine régional de fonctionner en tant que le domaine racine de forêt. Ce domaine est le domaine parent de tous les autres domaines régionaux et sera le premier domaine que vous déployez. Le domaine racine de forêt contienne des comptes d’utilisateur et géré de la même manière que les autres domaines régionaux sont gérés. La principale différence est qu’il inclut également les groupes Administrateurs de l’entreprise et administrateurs du schéma.  
  
L’avantage de la sélection d’un domaine régional de la fonction comme le domaine racine de forêt est que qu’il ne crée pas de la charge de gestion supplémentaires que conserver un domaine supplémentaire crée. Sélectionnez un domaine régional correspondant à la racine de forêt, telles que le domaine qui représente votre siège social ou la région qui a les connexions réseau plus rapides. S’il est difficile de votre organisation sélectionner un domaine régional pour être le domaine racine de forêt, vous pouvez choisir d’utiliser un modèle de racine de forêt dédiée à la place.  
  
## <a name="assigning-the-forest-root-domain-name"></a>Affectation de nom de domaine de la racine de forêt  
Le nom de domaine de racine de forêt est également le nom de la forêt. Le nom racine de forêt est un nom de système DNS (Domain Name) qui se compose d’un préfixe et un suffixe sous la forme de prefix.suffix. Par exemple, une organisation peut disposer le nom racine de forêt corp.contoso.com. Dans cet exemple, corp est le préfixe et contoso.com est le suffixe.  
  
Sélectionnez le suffixe dans la liste des noms existants sur votre réseau. Pour le préfixe, sélectionnez un nouveau nom n’a pas déjà été utilisé sur votre réseau. En attachant un nouveau préfixe pour un suffixe existant, vous créez un espace de noms unique. Création d’un nouvel espace de noms pour les Services de domaine ActiveDirectory (ADDS) garantit que toute infrastructure DNS existante n’a pas besoin être modifié pour prendre en charge les services ADDS.  
  
### <a name="selecting-a-suffix"></a>Sélection d’un suffixe  
Pour sélectionner un suffixe du domaine racine de forêt:  
  
1.  Contactez le propriétaire DNS pour l’organisation pour obtenir la liste des suffixes DNS inscrits qui sont en cours d’utilisation sur le réseau qui hébergera les services ADDS. Notez que les suffixes utilisés sur le réseau interne peuvent être différentes de celle les suffixes utilisés en externe. Par exemple, une organisation peut utiliser contosopharma.com sur Internet et contoso.com sur le réseau d’entreprise interne.  
  
2.  Consultez le propriétaire DNS pour sélectionner un suffixe pour une utilisation avec les services ADDS. Si aucun suffixe approprié n’existe, enregistrer un nouveau nom via une connexion Internet naming autorité.  
  
Nous vous recommandons d’utiliser des noms DNS qui sont enregistrés avec une autorité Internet dans l’espace de noms ActiveDirectory. Uniquement les noms enregistrés sont garantis pour être globalement uniques. Si une autre organisation ultérieurement inscrit le même nom de domaine DNS (ou si votre organisation fusionne avec, acquiert ou a été achetée par une autre entreprise qui utilise le même nom DNS), les deux infrastructures ne peut pas interagir avec eux.  
  
> [!CAUTION]  
> N’utilisez pas les noms DNS en une partie. Pour plus d’informations, voir les informations sur la configuration de Windows pour les domaines avec des noms DNS en une partie ([https://go.microsoft.com/fwlink/?LinkId=106631](https://go.microsoft.com/fwlink/?LinkId=106631)). En outre, nous ne recommandons pas à l’aide des suffixes non inscrits, tels que .local.  
  
### <a name="selecting-a-prefix"></a>En sélectionnant un préfixe  
Si vous avez choisi un suffixe inscrit qui est déjà en cours d’utilisation sur le réseau, sélectionnez un préfixe de nom de domaine de la racine de forêt en utilisant les règles de préfixe dans le tableau ci-dessous. Ajouter un préfixe qui n’est pas actuellement en cours d’utilisation pour créer un nouveau nom secondaire. Par exemple, si votre nom de racine DNS est contoso.com, vous pouvez créer le nom de domaine racine de la forêt ActiveDirectory concorp.contoso.com si l’espace de noms concorp.contoso.com n’est pas déjà en cours d’utilisation sur le réseau. Cette nouvelle branche de l’espace de noms sera consacrée aux services ADDS et peut être intégrée facilement avec l’implémentation de DNS existante.  
  
Si vous avez sélectionné un domaine régional de fonctionner comme un domaine racine de forêt, vous devrez peut-être sélectionner un nouveau préfixe pour le domaine. Étant donné que le nom de domaine de racine de forêt affecte tous les autres noms de domaine dans la forêt, un nom de base régionale n’est peut-être pas approprié. Si vous utilisez un nouveau suffixe qui n’est pas actuellement en cours d’utilisation sur le réseau, vous pouvez l’utiliser en tant que nom de domaine de la racine de forêt sans choisir un préfixe supplémentaire.  
  
Le tableau suivant répertorie les règles pour la sélection d’un préfixe pour un nom DNS enregistré.  
  
|Règle|Explication|  
|--------|---------------|  
|Sélectionnez un préfixe qui n’est pas susceptible de devenir obsolètes.|Évitez les noms comme une gamme de produits ou le système d’exploitation qui peuvent être modifiées à l’avenir. Nous vous recommandons d’utiliser des noms génériques comme corp ou ds.|  
|Sélectionnez un préfixe qui inclut uniquement des caractères standards d’Internet.|0-9 A-Z, a-z et (-), mais pas entièrement numérique.|  
|Comprendre 15caractères ou moins dans le préfixe.|Si vous choisissez une longueur de préfixe de moins de 15caractères, le nom NetBIOS est le même que le préfixe.|  
  
Il est important pour le propriétaire du DNS d’ActiveDirectory fonctionner avec le propriétaire DNS de l’organisation pour obtenir la propriété du nom qui sera utilisé pour l’espace de noms ActiveDirectory. Pour plus d’informations sur la conception d’une infrastructure DNS pour prendre en charge les services ADDS, voir [création d’une conception d’Infrastructure DNS](../../ad-ds/plan/Creating-a-DNS-Infrastructure-Design.md).  
  
## <a name="documenting-the-forest-root-domain-name"></a>Documentation de nom de domaine de la racine de forêt  
Documentez le préfixe DNS et le suffixe que vous sélectionnez le domaine racine de forêt. À ce stade, identifiez le domaine sera la racine de forêt. Vous pouvez ajouter les informations de nom de domaine racine forêt à la feuille de calcul «La planification du domaine» que vous avez créé pour documenter votre plan de nouveaux et mis à niveau des domaines et de vos noms de domaine. Pour l’ouvrir, télécharger Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip à partir de la tâche des identificateurs d’applet pour Windows Server2003 Deployment Kit ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558)) et ouvrez "Planification domaine «(DSSLOGI_5.doc).  
  


