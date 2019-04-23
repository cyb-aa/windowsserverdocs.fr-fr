---
title: Présentation de liaison de jeton
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 4623a48c-cefd-4a27-9173-2af58ac212f2
manager: alanth
author: justinha
ms.technology: security-authentication
ms.date: 11/09/2016
ms.openlocfilehash: 2a0bb8c75fe3ca7f7befe0bd67eb3d363a5ad7a9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884230"
---
# <a name="introducing-token-binding"></a>Présentation de liaison de jeton

>S'applique à : Windows Server 2016 et Windows 10

Le protocole de liaison de jeton permet aux applications et services par chiffrement lier leurs jetons de sécurité à la couche TLS pour atténuer les vols de jeton et les attaques par relecture. Les liaisons de TLS [RFC5246] durables, identifiables de façon unique peuvent s’étendre sur plusieurs connexions et sessions TLS.

Versions prises en charge :

- Windows 10 version 1507 – désactivé par défaut
    - Jeton de liaison de protocole ajouté [[draft-ietf-tokbind-protocol-01]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-protocol/01/)
    - WinInet & HTTP. Prise en charge SYS de liaison de jeton sur HTTP [[draft-ietf-tokbind-https-01]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-https/01/)
- Windows 10, versions 1511 et 1607 et Windows Server 2016 – sur par défaut
    - Protocole de liaison de mise à jour du jeton [[draft-ietf-tokbind-protocol-01]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-protocol/01/)
    - Extension TLS pour la négociation de jeton de liaison ajoutée [[draft-popov-tokbind-négociation-00]](https://tools.ietf.org/html/draft-popov-tokbind-negotiation-00)
    - WinInet & HTTP. Prise en charge SYS de liaison de jeton sur HTTP mis à jour [[draft-ietf-tokbind-https-02]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-https/02/)
- Windows 10, version 1507 avec mise à jour de maintenance [KB4034668](https://support.microsoft.com/kb/KB4034668), Windows 10, version 1511 avec mise à jour de maintenance [KB4034660](https://support.microsoft.com/kb/KB4034660), Windows 10, version 1607 et Windows Server 2016 avec la maintenance de la mise à jour [KB4034658](https://support.microsoft.com/kb/KB4034658) prend en charge de version de protocole de liaison de jeton 0,10 – sur par défaut
    - Protocole de liaison de mise à jour du jeton [[draft-ietf-tokbind-protocol-10]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-protocol/10/)
    - Extension TLS pour la négociation de jeton de liaison ajoutée [[draft-ietf-tokbind-négociation-05]](https://tools.ietf.org/html/draft-ietf-tokbind-negotiation-05)
    - WinInet & HTTP. Prise en charge SYS de liaison de jeton sur HTTP mis à jour [[draft-ietf-tokbind-https-06]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-https/06/)
- Windows 10, version 1703 prend en charge la version de protocole de liaison de jeton 0.10 – sur par défaut
    - Protocole de liaison de mise à jour du jeton [[draft-ietf-tokbind-protocol-10]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-protocol/10/)
    - Extension TLS pour la négociation de jeton de liaison ajoutée [[draft-ietf-tokbind-négociation-05]](https://tools.ietf.org/html/draft-ietf-tokbind-negotiation-05)
    - WinInet & HTTP. Prise en charge SYS de liaison de jeton sur HTTP mis à jour [[draft-ietf-tokbind-https-06]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-https/06/)
    - Appareils Windows avec la sécurité basée sur la virtualisation activée conserve les clés de la liaison de jeton dans un environnement protégé qui est isolé à partir du système d’exploitation en cours d’exécution

Vous trouverez plus d’informations sur la prise en charge ASP .NET à le [.NET Framework Reference Source](https://referencesource.microsoft.com/#System.Web/ITlsTokenBindingInfo.cs,4a5e5668f5c31170). 

Pour plus d’informations sur .NET Framework, consultez les rubriques suivantes :

- [Améliorations de mise en réseau](https://blogs.msdn.microsoft.com/dotnet/2015/11/30/net-framework-4-6-1-is-now-available/#networking)

- [Classe de .NET TokenBinding](https://msdn.microsoft.com/library/system.security.authentication.extendedprotection.tokenbinding.aspx)
