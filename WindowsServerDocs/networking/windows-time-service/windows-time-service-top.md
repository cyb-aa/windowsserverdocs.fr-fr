---
ms.assetid: e34622ff-b2d0-4f81-8d00-dacd5d6c215e
title: Service de temps Windows
description: ''
author: shortpatti
ms.author: pashort
manager: brianlic
ms.date: 02/01/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: b997d1f26e8da82e0d595b1ce13763e0a87d6d03
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="windows-time-service-w32time"></a>Service de temps Windows (W32Time)

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Le service de temps Windows (W32Time) synchronise la date et l’heure sur tous les ordinateurs dans les Services de domaine ActiveDirectory (ADDS). Synchronisation de l’heure est essentielle pour le bon fonctionnement de nombreux services Windows et des applications line of business (LOB). Le service de temps Windows utilise le protocole NTP (Network Time) pour synchroniser l’horloge de l’ordinateur sur le réseau. NTP garantit qu’une valeur d’horloge précise, ou horodateur, peut être affectée réseau les demandes d’accès aux ressources et de validation.

Dans la rubrique de Service de temps Windows (W32Time), le contenu suivant est disponible:
- **[Heure précise Windows2016](accurate-time.md).** Précision de la synchronisation de temps dans Windows Server 2016 a été améliorée considérablement, tout en conservant une compatibilité NTP avec des versions antérieures de Windows.  Conditions de fonctionnement raisonnable, vous pouvez conserver un 1 précision ms en ce qui concerne l’heure UTC ou supérieure pour les membres du domaine Windows Server 2016 et mise à jour anniversaire Windows 10.
- **[Référence technique de service de temps Windows ](windows-time-service-tech-ref.md).** Le service W32Time fournit la synchronisation d’horloge de réseau pour les ordinateurs sans avoir besoin de configuration étendues. Le service W32Time est essentiel pour le bon fonctionnement de l’authentification Kerberos V5 et, par conséquent, pour l’authentification par ADDS.
    - **[Comment le service de temps Windows fonctionne ](How-the-Windows-Time-Service-Works.md).** Bien que le service de temps Windows n’est pas une implémentation exacte du protocole NTP (Network Time), il utilise la suite complexe d’algorithmes qui est définie dans les spécifications NTP pour vous assurer que les horloges sur les ordinateurs sur un réseau sont aussi précis que possible.
    - **[Paramètres et outils de service de temps Windows ](Windows-Time-Service-Tools-and-Settings.md).** La plupart des ordinateurs membres du domaine ont un type de client de temps de NT5DS, ce qui signifie qu’ils synchronisent l’heure à partir de la hiérarchie de domaine. L’exception uniquement par défaut est le contrôleur de domaine qui fonctionne en tant que le maître d’opérations émulateur contrôleur principal de domaine du domaine racine de forêt, qui est généralement configuré pour synchroniser l’heure avec une source de temps externe.

## <a name="related-topics"></a>Rubriques connexes
Pour plus d’informations sur la hiérarchie de domaine et du système de score, voir la ["Qu’est le Service de temps Windows?»](https://blogs.msdn.microsoft.com/w32time/2007/07/07/what-is-windows-time-service/) Billet de blog.

Le modèle de plug-in de fournisseur de temps windows est [documentées sur TechNet](https://msdn.microsoft.com/en-us/library/windows/desktop/ms725475%28v=vs.85%29.aspx).

Un addendum référencé par l’article Windows2016précis temps peut être téléchargé [ici](http://windocs.blob.core.windows.net/windocs/WindowsTimeSyncAccuracy_Addendum.pdf)

Pour obtenir une vue d’ensemble du service de temps Windows, jeter un coup de œil cela [vue d’ensemble vidéo ](https://aka.ms/WS2016TimeVideo).

<!-- In this guide
In this guide:
Windows Accurate Time
High Accuracy
Support Boundary
Configuration for High Accuracy
Traceability for Compliance
Best Practices
Technical Reference
How the Windows Time Service Works
Windows Time Service Tools and Settings
-->

