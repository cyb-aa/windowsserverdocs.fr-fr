---
ms.assetid: c32606b4-2ee2-4df3-a704-8ac6723e188f
title: DNS et services AD DS
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 817c6ca168e33df1a0c59e151a303c4259313743
ms.sourcegitcommit: 11421f4005f9f3a3f6c0db95b1836d0f765a9fa3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81624297"
---
# <a name="dns-and-ad-ds"></a>DNS et services AD DS

> S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Active Directory Domain Services (AD DS) utilise des services de résolution de noms DNS (Domain Name System) pour permettre aux clients de localiser les contrôleurs de domaine et les contrôleurs de domaine qui hébergent le service d’annuaire pour communiquer entre eux.

AD DS permet d’intégrer facilement l’espace de noms Active Directory dans un espace de noms DNS existant. Les fonctionnalités telles que les zones DNS intégrées à Active Directory facilitent le déploiement de DNS en éliminant la nécessité de configurer des zones secondaires, puis de configurer des transferts de zone.

Pour plus d’informations sur la façon dont DNS prend en charge AD DS, consultez la section [prise en charge de DNS pour Active Directory informations techniques de référence](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc781627(v=ws.10)).

> [!NOTE]
> Si vous implémentez un espace de noms disjoint dans lequel le nom de domaine AD DS diffère du suffixe DNS principal utilisé par les clients, l’intégration AD DS avec DNS est plus complexe. Pour plus d’informations, consultez [espace de noms disjoint](Disjoint-Namespace.md).

## <a name="in-this-section"></a>Contenu de cette section

- [Emplacement des contrôleurs de domaine](Domain-Controller-Location.md)
- [Zones DNS intégrées à Active Directory](Active-Directory-Integrated-DNS-Zones.md)
- [Noms d’ordinateurs](Computer-Naming.md)
- [Espace de noms dissocié](Disjoint-Namespace.md)
