---
title: Présentation de la liaison de jeton
ms.prod: windows-server
ms.topic: article
ms.assetid: 4623a48c-cefd-4a27-9173-2af58ac212f2
manager: alanth
author: justinha
ms.technology: security-authentication
ms.date: 11/09/2016
ms.openlocfilehash: d067db04fe881193143104ce9f75a0c9932907e7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855892"
---
# <a name="introducing-token-binding"></a>Présentation de la liaison de jeton

>S’applique à : Windows Server 2016 et Windows 10

Le protocole de liaison de jeton permet aux applications et aux services de lier par chiffrement leurs jetons de sécurité à la couche TLS afin d’atténuer les attaques par relecture et vol de jetons. Les liaisons TLS [RFC5246] à long terme, identifiables de manière unique, peuvent s’étendre sur plusieurs sessions et connexions TLS.

Prise en charge des versions :

- Windows 10, version 1507 – désactivé par défaut
    - Protocole de liaison de jeton ajouté [[draft-ietf-tokbind-Protocol-01]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-protocol/01/)
    - WinInet & HTTP. Prise en charge SYS de la liaison de jeton sur HTTP [[draft-ietf-tokbind-https-01]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-https/01/)
- Windows 10, versions 1511 et 1607, et Windows Server 2016 – activé par défaut
    - Protocole de liaison de jeton mis à jour [[draft-ietf-tokbind-Protocol-01]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-protocol/01/)
    - Extension TLS pour la négociation de liaison de jeton ajoutée [[Draft-Popov-tokbind-Negotiation-00]](https://tools.ietf.org/html/draft-popov-tokbind-negotiation-00)
    - WinInet & HTTP. Prise en charge SYS de la liaison de jeton sur HTTP mise à jour [[draft-ietf-tokbind-https-02]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-https/02/)
- Windows 10, version 1507 avec maintenance de la mise à jour [KB4034668](https://support.microsoft.com/kb/KB4034668), Windows 10, version 1511 avec maintenance mise à jour [KB4034660](https://support.microsoft.com/kb/KB4034660), windows 10, version 1607 et Windows Server 2016 avec maintenance mise à jour [KB4034658](https://support.microsoft.com/kb/KB4034658) prendre en charge le protocole de liaison de jeton version 0,10 – activé par défaut
    - Protocole de liaison de jeton mis à jour [[draft-ietf-tokbind-Protocol-10]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-protocol/10/)
    - Extension TLS pour la négociation de liaison de jeton ajoutée [[draft-ietf-tokbind-Negotiation-05]](https://tools.ietf.org/html/draft-ietf-tokbind-negotiation-05)
    - WinInet & HTTP. Prise en charge SYS de la liaison de jeton sur HTTP mise à jour [[draft-ietf-tokbind-https-06]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-https/06/)
- Windows 10, version 1703 prend en charge le protocole de liaison de jeton version 0,10 (activé par défaut)
    - Protocole de liaison de jeton mis à jour [[draft-ietf-tokbind-Protocol-10]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-protocol/10/)
    - Extension TLS pour la négociation de liaison de jeton ajoutée [[draft-ietf-tokbind-Negotiation-05]](https://tools.ietf.org/html/draft-ietf-tokbind-negotiation-05)
    - WinInet & HTTP. Prise en charge SYS de la liaison de jeton sur HTTP mise à jour [[draft-ietf-tokbind-https-06]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-https/06/)
    - Les appareils Windows sur lesquels la sécurité basée sur la virtualisation est activée conservent les clés de liaison de jeton dans un environnement protégé qui est isolé du système d’exploitation en cours d’exécution

Vous trouverez des informations sur la prise en charge d’ASP .NET dans le [.NET Framework source de référence](https://referencesource.microsoft.com/#System.Web/ITlsTokenBindingInfo.cs,4a5e5668f5c31170). 

Pour plus d’informations sur .NET Framework, consultez les rubriques suivantes :

- [Améliorations de la mise en réseau](https://blogs.msdn.microsoft.com/dotnet/2015/11/30/net-framework-4-6-1-is-now-available/#networking)

- [.NET TokenBinding, classe](https://msdn.microsoft.com/library/system.security.authentication.extendedprotection.tokenbinding.aspx)
