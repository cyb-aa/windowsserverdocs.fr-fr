---
ms.assetid: ef4ef4a9-8969-4ad0-bd17-b2bb24f36ef6
title: Sélection du domaine racine de forêt
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: c669a5c8103fba52140147957823db978f8b6955
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59871350"
---
# <a name="selecting-the-forest-root-domain"></a>Sélection du domaine racine de forêt

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Le premier domaine que vous déployez dans une forêt Active Directory est appelé le domaine racine de forêt. Ce domaine reste le domaine racine de forêt pour le cycle de vie du déploiement d’AD DS.  
  
Le domaine racine contient les groupes Administrateurs de l’entreprise et administrateurs du schéma. Ces groupes d’administrateurs de service sont utilisés pour gérer les opérations de niveau de la forêt telles que l’ajout et la suppression de domaines et l’implémentation des modifications au schéma.  
  
En sélectionnant le domaine racine de forêt implique de déterminer si un des domaines Active Directory dans votre conception de domaine puisse fonctionner en tant que le domaine racine de forêt, ou si vous avez besoin déployer un domaine racine de forêt dédiée.  
  
Pour plus d’informations sur le déploiement d’un domaine racine de forêt, consultez [déploiement d’un domaine racine de forêt Windows Server 2008](https://technet.microsoft.com/library/cc731174.aspx).  
  
## <a name="choosing-a-regional-or-dedicated-forest-root-domain"></a>Choix d’un domaine racine de forêt régional ou dédié

Si vous appliquez un modèle de domaine unique, le seul domaine fonctionne en tant que le domaine racine de forêt. Si vous appliquez un modèle à plusieurs domaines, vous pouvez choisir de déployer un domaine racine de forêt dédiée ou sélectionnez un domaine régional pour fonctionner en tant que le domaine racine de forêt.  
  
### <a name="dedicated-forest-root-domain"></a>Domaine racine de forêt dédiée

Un domaine racine de forêt dédiée est un domaine qui est créé spécifiquement pour fonctionner en tant que racine de la forêt. Il ne contient pas les comptes d’utilisateurs autres que les comptes d’administrateur de service pour le domaine racine de forêt. En outre, il ne représente pas n’importe quelle région géographique dans votre structure de domaine. Tous les autres domaines dans la forêt sont les enfants du domaine racine de forêt dédiée.  

À l’aide d’une racine de forêt dédiée offre les avantages suivants :  

- Séparation opérationnelle forêt des administrateurs de service auprès des administrateurs de service de domaine. Dans un environnement de domaine unique, membres du groupe Administrateurs intégré et Admins du domaine peuvent utiliser des outils standard et des procédures eux-mêmes comme membres des groupes Administrateurs de l’entreprise et administrateurs du schéma. Dans une forêt qui utilise un domaine racine de forêt dédiée, membres du groupe Administrateurs intégré dans les domaines régionaux et Admins du domaine ne peut pas faire connaître les membres des groupes d’administrateur de service de niveau de la forêt à l’aide des procédures et des outils standard.  
- Protection contre les changements opérationnels dans d’autres domaines. Un domaine racine de forêt dédiée ne représente pas une région géographique spécifique dans votre structure de domaine. Pour cette raison, il n’est pas affectée par les réorganisations et autres modifications qui entraînent le changement de nom ou de restructurer les domaines.  
- Sert une racine neutre afin qu’aucun pays ou la région n’apparaisse comme subordonnée à une autre région. Certaines organisations préfèrent afin d’éviter l’apparence qu’un pays ou région est subordonnée à un autre pays ou région dans l’espace de noms. Lorsque vous utilisez un domaine racine de forêt dédiée, tous les domaines régionaux peuvent être égaux dans la hiérarchie de domaine.  

Dans un environnement de domaines multiples-régional dans lequel une racine de forêt dédiée est utilisée, la réplication de domaine racine de forêt a un impact minimal sur l’infrastructure réseau. Il s’agit, car la racine de forêt héberge uniquement les comptes d’administrateur de service. La majorité des comptes d’utilisateur dans la forêt et d’autres données spécifiques à un domaine sont stockés dans les domaines régionaux.  
  
Un des inconvénients à l’aide d’un domaine racine de forêt dédiée sont qu’il crée des frais de gestion supplémentaires pour prendre en charge le domaine supplémentaire.  
  
### <a name="regional-domain-as-a-forest-root-domain"></a>Domaine régional comme un domaine racine de forêt

Si vous choisissez de ne pas déployer un domaine racine de forêt dédiée, vous devez sélectionner un domaine régional pour fonctionner en tant que le domaine racine de forêt. Ce domaine est le domaine parent de tous les autres domaines régionaux et sera le premier domaine que vous déployez. Le domaine racine de forêt contienne des comptes d’utilisateur et est géré de la même façon que les autres domaines régionaux sont gérés. La principale différence est qu’il inclut également les groupes Administrateurs de l’entreprise et administrateurs du schéma.  
  
L’avantage de la sélection d’un domaine régional de la fonction que le domaine racine de forêt est qu’il ne crée pas la surcharge de gestion supplémentaires que gère un domaine supplémentaire crée. Sélectionnez un domaine régional approprié pour être la racine de forêt, tel que le domaine qui représente votre siège social ou la région qui a les connexions réseau le plus rapide. S’il est difficile pour votre organisation sélectionner un domaine régional pour être le domaine racine de forêt, vous pouvez choisir d’utiliser un modèle de racine de forêt dédiée à la place.  
  
## <a name="assigning-the-forest-root-domain-name"></a>Affectation de nom de domaine de la racine de forêt

Le nom de domaine de racine de forêt est également le nom de la forêt. Le nom de racine de forêt est un nom de système DNS (Domain Name) qui se compose d’un préfixe et un suffixe sous la forme de prefix.suffix. Par exemple, une organisation peut avoir le nom de racine de forêt corp.contoso.com. Dans cet exemple, corp est le préfixe et contoso.com est le suffixe.  
  
Sélectionnez le suffixe dans la liste des noms existants sur votre réseau. Pour le préfixe, sélectionnez un nouveau nom n’a pas déjà été utilisé sur votre réseau. En attachant un nouveau préfixe pour un suffixe existant, vous créez un espace de noms unique. Création d’un nouvel espace de noms pour les Services de domaine Active Directory (AD DS) garantit que n’importe quelle infrastructure DNS existante sans devoir être modifié pour prendre en charge les services AD DS.  
  
### <a name="selecting-a-suffix"></a>En sélectionnant un suffixe

Pour sélectionner un suffixe pour le domaine racine de forêt :  
  
1. Contactez le propriétaire DNS pour l’organisation pour obtenir la liste des suffixes DNS inscrits qui sont en cours d’utilisation sur le réseau qui hébergera les services AD DS. Notez que les suffixes utilisés sur le réseau interne peuvent être différentes de celle les suffixes utilisés en externe. Par exemple, une organisation peut utiliser contosopharma.com sur Internet et contoso.com sur le réseau d’entreprise interne.  
  
2. Consultez le propriétaire DNS pour sélectionner un suffixe pour une utilisation avec les services AD DS. Si aucun suffixe approprié n’existe, inscrire un nouveau nom avec un Internet autorité d’attribution de noms.  
  
Nous vous recommandons d’utiliser des noms DNS qui sont inscrits auprès d’une autorité Internet dans l’espace de noms Active Directory. Uniquement les noms enregistrés sont garantis être globalement unique. Si une autre organisation ultérieurement enregistre le nom de domaine DNS (ou si votre organisation fusionne avec, acquiert ou est acquis par une autre société qui utilise le même nom DNS), les deux infrastructures ne peut pas interagir avec eux.  
  
> [!CAUTION]  
> N’utilisez pas les noms DNS en une partie. Pour plus d’informations, consultez les informations sur la configuration de Windows pour les domaines avec des noms DNS en une partie ([https://go.microsoft.com/fwlink/?LinkId=106631](https://go.microsoft.com/fwlink/?LinkId=106631)). En outre, il est déconseillé à l’aide des suffixes non inscrits, telles que .local.  
  
### <a name="selecting-a-prefix"></a>En sélectionnant un préfixe

Si vous avez choisi un suffixe inscrit qui est déjà en cours d’utilisation sur le réseau, sélectionnez un préfixe pour le nom de domaine de racine de forêt en utilisant les règles de préfixe dans le tableau ci-dessous. Ajouter un préfixe qui n’est pas actuellement en cours d’utilisation pour créer un nouveau nom subordonné. Par exemple, si votre nom de racine DNS est contoso.com, vous pouvez créer le concorp.contoso.com nom domaine de racine de forêt Active Directory si l’espace de noms concorp.contoso.com n’est pas déjà en cours d’utilisation sur le réseau. Cette nouvelle branche de l’espace de noms sera consacrée aux services AD DS et peut être intégrée facilement à l’implémentation de DNS existante.  
  
Si vous avez sélectionné un domaine régional pour fonctionner comme un domaine racine de forêt, vous devrez peut-être sélectionner un nouveau préfixe pour le domaine. Étant donné que le nom de domaine de racine de forêt affecte tous les autres noms de domaine dans la forêt, un nom au niveau régional en ne sera peut-être pas approprié. Si vous utilisez un nouveau suffixe n’est pas actuellement en cours d’utilisation sur le réseau, vous pouvez l’utiliser en tant que nom de domaine de la racine de forêt sans choisir un préfixe supplémentaire.  
  
Le tableau suivant répertorie les règles pour la sélection d’un préfixe pour un nom DNS enregistré.  
  
|Règle|Explication|  
|--------|---------------|  
|Sélectionnez un préfixe qui n’est pas susceptible de devenir obsolètes.|Évitez les noms tels que d’une gamme de produits ou d’un système d’exploitation qui peut changer à l’avenir. Nous vous recommandons d’utiliser des noms génériques tels que corp ou ds.|  
|Sélectionnez un préfixe qui inclut uniquement les caractères standard Internet.|A-Z, a-z, 0-9 et (-), mais pas entièrement numérique.|  
|Comprendre 15 caractères ou moins dans le préfixe.|Si vous choisissez une longueur de préfixe de 15 caractères ou moins, le nom NetBIOS est le même que le préfixe.|  
  
Il est important pour le propriétaire de DNS d’Active Directory travailler avec le propriétaire DNS pour l’organisation pour obtenir la propriété du nom qui sera utilisé pour l’espace de noms Active Directory. Pour plus d’informations sur la conception d’une infrastructure DNS pour prendre en charge les services AD DS, consultez [création d’une conception d’Infrastructure DNS](../../ad-ds/plan/Creating-a-DNS-Infrastructure-Design.md).  
  
## <a name="documenting-the-forest-root-domain-name"></a>Documenter le nom de domaine de racine de forêt

Documentez le préfixe DNS et le suffixe que vous sélectionnez le domaine racine de forêt. À ce stade, identifiez quel domaine sera la racine de forêt. Vous pouvez ajouter les informations de nom de domaine racine forêt à la feuille de calcul « Planification de domaine » que vous avez créé pour documenter votre plan pour les domaines de nouveaux et mis à niveau et de vos noms de domaine. Pour l’ouvrir, télécharger Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip de [travail aides pour Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558) et ouvrez « Planification de domaine » (DSSLOGI_5.doc).
