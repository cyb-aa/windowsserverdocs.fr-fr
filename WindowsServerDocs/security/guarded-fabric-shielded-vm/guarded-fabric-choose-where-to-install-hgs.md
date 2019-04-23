---
title: Choisissez si vous souhaitez installer SGH dans sa propre nouvelle forêt ou dans une forêt bastion existante
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 4e02cd37391e629c9b947095fe32626bd15726ff
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827500"
---
# <a name="choose-whether-to-install-hgs-in-its-own-dedicated-forest-or-in-an-existing-bastion-forest"></a>Choisissez s’il faut installer SGH dans sa propre forêt dédiée ou dans une forêt bastion existante

>S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016


La forêt Active Directory pour SGH est sensible, car ses administrateurs ont accès aux clés de contrôle des machines virtuelles protégées. L’installation par défaut configurent une nouvelle forêt dédiée pour SGH et configurer d’autres dépendances. Cette option est recommandée, car l’environnement est autonome et connue pour être sécurisé lorsqu’il est créé. 

L’exigence technique uniquement pour l’installation de SGH dans une forêt existante est qu’il est ajouté au domaine racine ; les domaines non racine ne sont pas pris en charge. Mais il existe également les exigences opérationnelles et liées à la sécurité des meilleures pratiques pour l’utilisation d’une forêt existante. Forêts appropriés sont volontairement conçus pour répondre à une seule fonction sensible, telles que la forêt utilisée par [Privileged Access Management pour les services AD DS](https://docs.microsoft.com/microsoft-identity-manager/pam/privileged-identity-management-for-active-directory-domain-services) ou un [forêt Enhanced Security Administrative Environment (ESAE)](https://technet.microsoft.com/windows-server-docs/security/securing-privileged-access/securing-privileged-access-reference-material#ESAE_BM). Ces forêts présentent généralement les caractéristiques suivantes :

- Ils ont plusieurs administrateurs (des administrateurs d’infrastructure)
- Ils ont un faible nombre d’ouvertures de session
- Ils ne sont pas à usage général par nature 

Forêts à usage général tels que les forêts de production ne conviennent pas pour une utilisation par SGH. Les forêts de fabric sont également inadaptées parce que SGH doit être isolé des administrateurs de structure.

## <a name="next-step"></a>Étape suivante

Choisissez l’option d’installation qui répond le mieux à votre environnement :

- [Installer le service SGH dans sa propre forêt dédiée](guarded-fabric-install-hgs-default.md)
- [Installer le service SGH dans une forêt bastion existante](guarded-fabric-install-hgs-in-a-bastion-forest.md)


