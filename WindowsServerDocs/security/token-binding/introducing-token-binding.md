---
title: "Présentation de la liaison de jeton"
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 4623a48c-cefd-4a27-9173-2af58ac212f2
manager: alanth
author: justinha
ms.technology: security-authentication
ms.date: 11/09/2016
ms.openlocfilehash: 2a0bb8c75fe3ca7f7befe0bd67eb3d363a5ad7a9
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="introducing-token-binding"></a>Présentation de la liaison de jeton

>S’applique à: Windows Server2016 et Windows10

Le protocole de jeton de liaison permet aux applications et services pour la liaison par chiffrement de jetons de sécurité à la couche TLS pour atténuer les vols de jeton et les attaques par relecture. Les liaisons de TLS [RFC5246] à long terme, identifiables de façon unique peuvent s’étendre sur plusieurs connexions et sessions TLS.

Prise en charge de la version:

- Windows10, version1507: désactivée par défaut
    - Jeton de liaison de protocole ajouté [[brouillon-ietf-tokbind-protocole-01]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-protocol/01/)
    - WinInet et HTTP.SYS prennent en charge de la liaison de jeton via HTTP [[brouillon-ietf-tokbind-https-01]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-https/01/)
- Windows10, versions 1511 et 1607 et Windows Server2016 – sur par défaut
    - Jeton de liaison de protocole de mise à jour [[brouillon-ietf-tokbind-protocole-01]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-protocol/01/)
    - Extension TLS pour la négociation de liaison de jeton ajoutée [[brouillon-popov-tokbind-négociation-00]](https://tools.ietf.org/html/draft-popov-tokbind-negotiation-00)
    - WinInet et HTTP.SYS prennent en charge de la liaison de jeton via HTTP mis à jour [[brouillon-ietf-tokbind-https-02]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-https/02/)
- Windows10, version1507 avec la mise à jour de maintenance [KB4034668](https://support.microsoft.com/kb/KB4034668), Windows10, version1511 avec la mise à jour de maintenance [KB4034660](https://support.microsoft.com/kb/KB4034660), Windows10, version1607 et Windows Server2016 avec la mise à jour de maintenance [KB4034658](https://support.microsoft.com/kb/KB4034658) prennent en charge de version du protocole de la liaison de jeton 0,10: sur par défaut
    - Jeton de liaison de protocole de mise à jour [[brouillon-ietf-tokbind-protocole-10]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-protocol/10/)
    - Extension TLS pour la négociation de liaison de jeton ajoutée [[brouillon-ietf-tokbind-négociation-05]](https://tools.ietf.org/html/draft-ietf-tokbind-negotiation-05)
    - WinInet et HTTP.SYS prennent en charge de la liaison de jeton via HTTP mis à jour [[brouillon-ietf-tokbind-https-06]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-https/06/)
- Windows10, version1703 prend en charge la version du protocole de la liaison de jeton 0,10: sur par défaut
    - Jeton de liaison de protocole de mise à jour [[brouillon-ietf-tokbind-protocole-10]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-protocol/10/)
    - Extension TLS pour la négociation de liaison de jeton ajoutée [[brouillon-ietf-tokbind-négociation-05]](https://tools.ietf.org/html/draft-ietf-tokbind-negotiation-05)
    - WinInet et HTTP.SYS prennent en charge de la liaison de jeton via HTTP mis à jour [[brouillon-ietf-tokbind-https-06]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-https/06/)
    - Les appareils Windows avec la sécurité basée sur la virtualisation activée conservera les clés de liaison de jeton dans un environnement protégé est isolé du système d’exploitation en cours d’exécution

Vous trouverez des informations sur la prise en charge ASP .NET à le [Source de référence .NETFramework](https://referencesource.microsoft.com/#System.Web/ITlsTokenBindingInfo.cs,4a5e5668f5c31170). 

Pour plus d’informations sur .NETFramework, voir les rubriques suivantes:

- [Améliorations de mise en réseau](https://blogs.msdn.microsoft.com/dotnet/2015/11/30/net-framework-4-6-1-is-now-available/#networking)

- [Classe TokenBinding .NET](https://msdn.microsoft.com/library/system.security.authentication.extendedprotection.tokenbinding.aspx)
