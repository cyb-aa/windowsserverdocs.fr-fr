---
ms.assetid: ef4ef4a9-8969-4ad0-bd17-b2bb24f36ef6
title: Sélection du domaine racine de forêt
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: abc02bec101b39a66a78da871f838d2585d89377
ms.sourcegitcommit: af1cf89632d62a94943d3ad9f6b5234b88499278
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81524924"
---
# <a name="selecting-the-forest-root-domain"></a>Sélection du domaine racine de forêt

> S’applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Le premier domaine que vous déployez dans une forêt Active Directory est appelé domaine racine de la forêt. Ce domaine reste le domaine racine de la forêt pour le cycle de vie du déploiement AD DS.

Le domaine racine de la forêt contient les groupes Administrateurs de l’entreprise et administrateurs du schéma. Ces groupes d’administrateurs de service sont utilisés pour gérer les opérations au niveau de la forêt, telles que l’ajout et la suppression de domaines, ainsi que l’implémentation des modifications apportées au schéma.

La sélection du domaine racine de la forêt implique de déterminer si l’un des domaines Active Directory dans votre conception de domaine peut fonctionner en tant que domaine racine de la forêt ou si vous devez déployer un domaine racine de forêt dédié.

Pour plus d’informations sur le déploiement d’un domaine racine de forêt, voir [déploiement d’un domaine racine de forêt Windows Server 2008](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731174(v=ws.10)).

## <a name="choosing-a-regional-or-dedicated-forest-root-domain"></a>Choix d’un domaine racine de forêt régional ou dédié

Si vous appliquez un modèle de domaine unique, le domaine unique fonctionne comme le domaine racine de la forêt. Si vous appliquez un modèle à plusieurs domaines, vous pouvez choisir de déployer un domaine racine de forêt dédié ou de sélectionner un domaine régional pour qu’il fonctionne en tant que domaine racine de la forêt.

### <a name="dedicated-forest-root-domain"></a>Domaine racine dédié à la forêt

Un domaine racine de forêt dédié est un domaine créé spécifiquement pour fonctionner en tant que racine de la forêt. Il ne contient pas de comptes d’utilisateur autres que les comptes d’administrateur de service pour le domaine racine de forêt. En outre, il ne représente pas une région géographique dans votre structure de domaine. Tous les autres domaines de la forêt sont des enfants du domaine racine de la forêt dédiée.

L’utilisation d’une racine de forêt dédiée offre les avantages suivants :

- Séparation opérationnelle des administrateurs de service de forêt des administrateurs de service de domaine. Dans un environnement à domaine unique, les membres des groupes Administrateurs de domaine et administrateurs intégrés peuvent utiliser des outils et des procédures standard pour se faire être membres des groupes Administrateurs de l’entreprise et administrateurs du schéma. Dans une forêt qui utilise un domaine racine de forêt dédié, les membres des groupes Administrateurs de domaine et administrateurs intégrés dans les domaines régionaux ne peuvent pas être eux-mêmes membres des groupes d’administrateurs de service au niveau de la forêt à l’aide des outils et procédures standard.
- Protection contre les modifications opérationnelles dans d’autres domaines. Un domaine racine de forêt dédié ne représente pas une région géographique particulière dans votre structure de domaine. Pour cette raison, elle n’est pas affectée par les réorganisations ou autres modifications qui entraînent le changement de nom ou la restructuration des domaines.
- Sert de racine neutre afin qu’aucun pays ou région ne semble être subordonné à une autre région. Certaines organisations préfèrent éviter d’avoir l’impression qu’un pays ou une région est subordonné à un autre pays ou région de l’espace de noms. Lorsque vous utilisez un domaine racine de forêt dédié, tous les domaines régionaux peuvent être des pairs dans la hiérarchie de domaine.

Dans un environnement à plusieurs domaines régionaux dans lequel une racine de forêt dédiée est utilisée, la réplication du domaine racine de la forêt a un impact minimal sur l’infrastructure réseau. Cela est dû au fait que la racine de la forêt héberge uniquement les comptes d’administrateur de service. La majorité des comptes d’utilisateur dans la forêt et d’autres données spécifiques au domaine sont stockées dans les domaines régionaux.

L’un des inconvénients de l’utilisation d’un domaine racine de forêt dédié est qu’il crée une charge de gestion supplémentaire pour prendre en charge le domaine supplémentaire.

### <a name="regional-domain-as-a-forest-root-domain"></a>Domaine régional en tant que domaine racine de la forêt

Si vous choisissez de ne pas déployer un domaine racine de forêt dédié, vous devez sélectionner un domaine régional pour qu’il fonctionne en tant que domaine racine de la forêt. Ce domaine est le domaine parent de tous les autres domaines régionaux et sera le premier domaine que vous déployez. Le domaine racine de la forêt contient des comptes d’utilisateur et est géré de la même façon que les autres domaines régionaux. La principale différence est qu’elle comprend également les groupes Administrateurs de l’entreprise et administrateurs du schéma.

L’avantage de sélectionner un domaine régional pour fonctionner comme domaine racine de la forêt est qu’il ne crée pas la surcharge de gestion supplémentaire qui gère un domaine supplémentaire. Sélectionnez un domaine régional approprié comme racine de la forêt, par exemple le domaine qui représente votre siège social ou la région qui dispose des connexions réseau les plus rapides. S’il est difficile pour votre organisation de sélectionner un domaine régional comme domaine racine de la forêt, vous pouvez choisir d’utiliser un modèle racine de forêt dédié à la place.

## <a name="assigning-the-forest-root-domain-name"></a>Attribution du nom de domaine racine de la forêt

Le nom de domaine racine de la forêt est également le nom de la forêt. Le nom de la racine de la forêt est un nom DNS (Domain Name System) qui se compose d’un préfixe et d’un suffixe au format préfixe. suffixe. Par exemple, une organisation peut avoir le nom racine de la forêt corp.contoso.com. Dans cet exemple, Corp est le préfixe et contoso.com est le suffixe.

Sélectionnez le suffixe dans une liste de noms existants sur votre réseau. Pour le préfixe, sélectionnez un nouveau nom qui n’a pas été utilisé précédemment sur votre réseau. En joignant un nouveau préfixe à un suffixe existant, vous créez un espace de noms unique. La création d’un espace de noms pour Active Directory Domain Services (AD DS) garantit que toute infrastructure DNS existante n’a pas besoin d’être modifiée pour prendre en charge les AD DS.

### <a name="selecting-a-suffix"></a>Sélection d’un suffixe

Pour sélectionner un suffixe pour le domaine racine de la forêt :

1. Contactez le propriétaire DNS de l’Organisation pour obtenir la liste des suffixes DNS inscrits en cours d’utilisation sur le réseau qui hébergera AD DS. Notez que les suffixes utilisés sur le réseau interne peuvent être différents de ceux utilisés en externe. Par exemple, une organisation peut utiliser contosopharma.com sur Internet et contoso.com sur le réseau interne de l’entreprise.

2. Consultez le propriétaire DNS pour sélectionner un suffixe à utiliser avec AD DS. S’il n’existe aucun suffixe approprié, inscrivez un nouveau nom auprès d’une autorité d’attribution de noms Internet.

Nous vous recommandons d’utiliser des noms DNS inscrits auprès d’une autorité Internet dans l’espace de noms Active Directory. Seuls les noms enregistrés sont garantis globalement uniques. Si une autre organisation inscrit ultérieurement le même nom de domaine DNS (ou si votre organisation fusionne avec, acquiert ou est acquise par une autre société qui utilise le même nom DNS), les deux infrastructures ne peuvent pas interagir les unes avec les autres.

> [!CAUTION]
> N’utilisez pas de noms DNS en une partie. Pour plus d’informations, consultez ([déploiement et fonctionnement de Active Directory domaines configurés à l’aide de noms DNS en une partie](https://go.microsoft.com/fwlink/?LinkId=106631)). En outre, nous vous déconseillons d’utiliser des suffixes non enregistrés, tels que. local.

### <a name="selecting-a-prefix"></a>Sélection d’un préfixe

Si vous avez choisi un suffixe inscrit qui est déjà en cours d’utilisation sur le réseau, sélectionnez un préfixe pour le nom de domaine racine de la forêt en utilisant les règles de préfixe dans le tableau ci-dessous. Ajoutez un préfixe qui n’est pas actuellement utilisé pour créer un nouveau nom subordonné. Par exemple, si le nom de votre racine DNS est contoso.com, vous pouvez créer le Active Directory nom de domaine racine de la forêt concorp.contoso.com si l’espace de noms concorp.contoso.com n’est pas déjà utilisé sur le réseau. Cette nouvelle branche de l’espace de noms sera dédiée à AD DS et peut être intégrée facilement avec l’implémentation DNS existante.

Si vous avez sélectionné un domaine régional pour qu’il fonctionne comme un domaine racine de forêt, vous devrez peut-être sélectionner un nouveau préfixe pour le domaine. Étant donné que le nom de domaine racine de la forêt affecte tous les autres noms de domaine de la forêt, un nom basé sur une région peut ne pas être approprié. Si vous utilisez un nouveau suffixe qui n’est pas en cours d’utilisation sur le réseau, vous pouvez l’utiliser comme nom de domaine racine de la forêt sans choisir de préfixe supplémentaire.

Le tableau suivant répertorie les règles de sélection d’un préfixe pour un nom DNS enregistré.

|Règle|Explication|
|--------|---------------|
|Sélectionnez un préfixe qui n’est pas susceptible de devenir obsolète.|Évitez les noms tels qu’une gamme de produits ou un système d’exploitation qui pourraient changer à l’avenir. Nous vous recommandons d’utiliser des noms génériques tels que Corp ou DS.|
|Sélectionnez un préfixe qui contient uniquement des caractères Internet standard.|A-Z, a-z, 0-9 et (-), mais pas entièrement numériques.|
|Incluez 15 caractères ou moins dans le préfixe.|Si vous choisissez une longueur de préfixe de 15 caractères ou moins, le nom NetBIOS est le même que le préfixe.|

Il est important que le propriétaire DNS Active Directory collabore avec le propriétaire DNS de l’Organisation pour obtenir la propriété du nom qui sera utilisé pour l’espace de noms Active Directory. Pour plus d’informations sur la conception d’une infrastructure DNS pour prendre en charge des AD DS, consultez [création d’une conception d’infrastructure DNS](../../ad-ds/plan/Creating-a-DNS-Infrastructure-Design.md).

## <a name="documenting-the-forest-root-domain-name"></a>Documentation du nom de domaine racine de la forêt

Documentez le préfixe et le suffixe DNS que vous sélectionnez pour le domaine racine de forêt. À ce stade, identifiez le domaine qui sera la racine de la forêt. Vous pouvez ajouter les informations du nom de domaine racine de la forêt à la feuille de calcul « planification de domaine » que vous avez créée pour documenter votre plan pour les domaines nouveaux et mis à niveau et vos noms de domaine. Pour l’ouvrir, téléchargez Job_Aids_Designing_and_Deploying_Directory_and_Security_Services. zip à partir des [outils de gestion des travaux pour le kit de déploiement Windows Server 2003](https://www.microsoft.com/download/details.aspx?id=9608) et ouvrez « planification de domaine » (DSSLOGI_5. doc).
