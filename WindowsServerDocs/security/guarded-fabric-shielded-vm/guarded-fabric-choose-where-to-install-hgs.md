---
title: Indiquez si vous souhaitez installer SGH dans sa propre forêt ou dans une forêt bastion existante.
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 52376a4e193b8021dc58214003e9b7579b096a79
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856882"
---
# <a name="choose-whether-to-install-hgs-in-its-own-dedicated-forest-or-in-an-existing-bastion-forest"></a>Indiquez si vous souhaitez installer SGH dans sa propre forêt dédiée ou dans une forêt bastion existante.

>S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016


La forêt Active Directory pour SGH est sensible, car ses administrateurs ont accès aux clés qui contrôlent les machines virtuelles protégées. L’installation par défaut permet de configurer une nouvelle forêt dédiée à SGH et de configurer d’autres dépendances. Cette option est recommandée, car l’environnement est autonome et connu pour être sécurisé lors de sa création. 

La seule exigence technique pour l’installation de SGH dans une forêt existante est qu’elle est ajoutée au domaine racine. les domaines non racine ne sont pas pris en charge. Toutefois, il existe également des exigences opérationnelles et des meilleures pratiques liées à la sécurité pour l’utilisation d’une forêt existante. Les forêts appropriées sont conçues spécialement pour servir une fonction sensible, telle que la forêt utilisée par [Privileged Access Management pour AD DS](https://docs.microsoft.com/microsoft-identity-manager/pam/privileged-identity-management-for-active-directory-domain-services) ou une [forêt esae (Enhanced Security Environment) améliorée](https://technet.microsoft.com/windows-server-docs/security/securing-privileged-access/securing-privileged-access-reference-material#ESAE_BM). De telles forêts présentent généralement les caractéristiques suivantes :

- Ils ont peu d’administrateurs (distincts des administrateurs de l’infrastructure)
- Ils ont un nombre faible d’ouvertures de session
- Ils ne sont pas à usage général par nature 

Les forêts à usage général, telles que les forêts de production, ne sont pas adaptées à une utilisation par SGH. Les forêts de l’infrastructure sont également inappropriées, car SGH doit être isolé des administrateurs de l’infrastructure.

## <a name="next-step"></a>Étape suivante

Choisissez l’option d’installation qui correspond le mieux à votre environnement :

- [Installer SGH dans sa propre forêt dédiée](guarded-fabric-install-hgs-default.md)
- [Installer SGH dans une forêt bastion existante](guarded-fabric-install-hgs-in-a-bastion-forest.md)


