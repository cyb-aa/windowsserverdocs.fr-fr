---
ms.assetid: e34622ff-b2d0-4f81-8d00-dacd5d6c215e
title: Service de temps Windows
description: ''
author: shortpatti
ms.author: pashort
manager: dougkim
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: e3dbaa188426ac81073e706db3adc6ab0a655c01
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405187"
---
# <a name="windows-time-service-w32time"></a>Service de temps Windows (W32Time)

>S’applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10 ou version ultérieure

Le service de temps Windows (W32Time) synchronise la date et l’heure de tous les ordinateurs s’exécutant dans Active Directory Domain Services (AD DS). La synchronisation de l’heure est essentielle pour le bon fonctionnement de nombreux services Windows et d’applications métier. Le service de temps Windows utilise le protocole NTP (Network Time Protocol) pour synchroniser les horloges des ordinateurs sur le réseau. NTP garantit qu’une valeur d’horloge exacte, ou horodateur, peut être affectée à la validation réseau et aux demandes d’accès aux ressources.

Dans la rubrique Service de temps Windows (W32Time), le contenu suivant est disponible :
- **[Heure précise de Windows Server 2016](accurate-time.md).** La précision de la synchronisation de l’heure dans Windows Server 2016 a été considérablement améliorée, tout en conservant une compatibilité NTP complète avec les anciennes versions de Windows. Dans des conditions raisonnables, vous pouvez conserver une précision de 1 ms en ce qui concerne l’heure UTC ou une meilleure pour les membres du domaine de la mise à jour anniversaire de Windows Server 2016 et Windows 10.
- **[Limite de support pour les environnements haute précision](support-boundary.md).** Cet article décrit les limites de prise en charge du service de temps Windows (W32Time) dans les environnements qui nécessitent un temps système extrêmement précis et stable.
- **[Configuration des systèmes pour une haute précision](configuring-systems-for-high-accuracy.md).** La synchronisation de l’heure dans Windows 10 et Windows Server 2016 a été considérablement améliorée.  Dans des conditions raisonnables, les systèmes peuvent être configurés pour maintenir une précision de 1 ms (milliseconde) ou mieux (en ce qui concerne l’heure UTC).
- **[Temps Windows pour la traçabilité](windows-time-for-traceability.md).** Dans de nombreux secteurs, les réglementations requièrent que les systèmes soient traçables au format UTC.  Cela signifie que le décalage d’un système peut être sanctionné par rapport au temps universel coordonné (UTC).  Pour activer les scénarios de conformité réglementaire, Windows 10 et le serveur 2016 fournissent de nouveaux journaux des événements pour fournir une image du point de vue du système d’exploitation afin de comprendre les actions effectuées sur l’horloge système.  Ces journaux des événements sont générés en continu pour le service de temps Windows et peuvent être examinés ou archivés en vue d’une analyse ultérieure.
- **[Référence technique du service de temps Windows](windows-time-service-tech-ref.md).** Le service W32Time assure la synchronisation de l’horloge réseau pour les ordinateurs sans nécessiter de configuration étendue. Le service W32Time est essentiel au bon fonctionnement de l’authentification Kerberos V5 et, par conséquent, à l’authentification AD DS.
    - **[Fonctionnement du service de temps Windows](How-the-Windows-Time-Service-Works.md).** Bien que le service de temps Windows ne soit pas une implémentation exacte du protocole NTP (Network Time Protocol), il utilise la suite complexe d’algorithmes définie dans les spécifications NTP pour s’assurer que les horloges sur les ordinateurs d’un réseau sont aussi précises que possible.
    - **[Les paramètres et les outils du service de temps Windows](Windows-Time-Service-Tools-and-Settings.md).** La plupart des ordinateurs membres du domaine ont un type de client de temps NT5DS, ce qui signifie qu’ils synchronisent l’heure à partir de la hiérarchie de domaine. La seule exception classique est le contrôleur de domaine qui fonctionne comme le maître d’opérations de l’émulateur de contrôleur de domaine principal (PDC) du domaine racine de la forêt, qui est généralement configuré pour synchroniser l’heure avec une source de temps externe.


## <a name="related-topics"></a>Rubriques connexes
Pour plus d’informations sur la hiérarchie de domaine et le système de calcul de score, consultez [« qu’est-ce que le service de temps Windows ? »](https://blogs.msdn.microsoft.com/w32time/2007/07/07/what-is-windows-time-service/) billet de blog.

Le modèle de plug-in du fournisseur de temps Windows est [documenté sur TechNet](https://msdn.microsoft.com/library/windows/desktop/ms725475%28v=vs.85%29.aspx).

Un addendum référencé par l’article précis sur l’heure de Windows 2016 peut être téléchargé [ici](https://windocs.blob.core.windows.net/windocs/WindowsTimeSyncAccuracy_Addendum.pdf)

Pour une vue d’ensemble rapide du service de temps Windows, jetez un coup d’œil à cette [vidéo de présentation de haut niveau](https://aka.ms/WS2016TimeVideo).
