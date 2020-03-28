---
ms.assetid: e34622ff-b2d0-4f81-8d00-dacd5d6c215e
title: Service de temps Windows
description: ''
author: eross-msft
ms.author: lizross
manager: dougkim
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: 4b8b1e91f56ec4d6c070037a0f3cc5ec4d50c63e
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80314923"
---
# <a name="windows-time-service-w32time"></a>Service de temps Windows (W32Time)

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10 ou version ultérieure

Le service de temps Windows (W32Time) synchronise la date et l’heure de tous les ordinateurs s’exécutant dans Active Directory Domain Services (AD DS). La synchronisation de l’heure est essentielle au bon fonctionnement de nombreux services Windows et applications métier. Le service de temps Windows utilise le protocole NTP (Network Time Protocol) pour synchroniser les horloges des ordinateurs sur le réseau. NTP garantit qu’une valeur d’horloge exacte, ou horodatage, peut être affectée aux demandes de validation réseau et d’accès aux ressources.

La rubrique Service de temps Windows (W32Time) met à votre disposition le contenu suivant :
- **[Heure exacte Windows Server 2016](accurate-time.md).** La précision de la synchronisation du temps dans Windows Server 2016 a été considérablement améliorée, tout en conservant une compatibilité NTP complète avec les anciennes versions de Windows. Dans des conditions de fonctionnement raisonnables, vous pouvez conserver une précision de 1 ms par rapport à l’heure UTC, ou une meilleure précision, pour les membres du domaine Mise à jour anniversaire de Windows 10 et Windows Server 2016.
- **[Limite de prise en charge pour les environnements haute précision](support-boundary.md).** Cet article décrit les limites de prise en charge pour le service de temps Windows (W32Time) dans les environnements qui nécessitent une heure système extrêmement précise et stable.
- **[Configuration de systèmes de haute précision](configuring-systems-for-high-accuracy.md).** La synchronisation de l’heure dans Windows 10 et Windows Server 2016 a été considérablement améliorée.  Dans des conditions de fonctionnement raisonnables, les systèmes peuvent être configurés pour maintenir une précision de 1 ms (milliseconde), ou une meilleure précision, par rapport à l’heure UTC.
- **[Temps Windows pour la traçabilité](windows-time-for-traceability.md).** Dans de nombreux secteurs, les réglementations exigent que les systèmes soient traçables au format UTC.  Cela signifie que le décalage d’un système peut être attesté par rapport au temps universel coordonné (UTC).  Pour autoriser les scénarios de conformité réglementaire, Windows 10 et Windows Server 2016 fournissent de nouveaux journaux des événements afin de proposer une image du point de vue du système d’exploitation permettant de comprendre les actions effectuées sur l’horloge système.  Ces journaux des événements sont générés en continu pour le service de temps Windows et peuvent être examinés ou archivés pour les analyser ultérieurement.
- **[Informations techniques de référence sur le service de temps Windows](windows-time-service-tech-ref.md).** Le service W32Time assure la synchronisation de l’horloge réseau pour les ordinateurs sans nécessiter de configuration étendue. Le service W32Time est essentiel au bon fonctionnement de l’authentification Kerberos V5 et, donc, à l’authentification AD DS.
    - **[Fonctionnement du service de temps Windows](How-the-Windows-Time-Service-Works.md).** Bien que le service de temps Windows ne soit pas une implémentation exacte du protocole NTP (Network Time Protocol), il utilise la suite complexe d’algorithmes définie dans les spécifications NTP pour s’assurer que les horloges sur les ordinateurs d’un réseau soient aussi précises que possible.
    - **[Paramètres et outils du service de temps Windows](Windows-Time-Service-Tools-and-Settings.md).** La plupart des ordinateurs membres du domaine ont un type de client de temps NT5DS, ce qui signifie qu’ils synchronisent l’heure à partir de la hiérarchie de domaine. La seule exception classique est le contrôleur de domaine qui fonctionne en tant que maître d’opérations de l’émulateur du contrôleur de domaine principal du domaine racine de la forêt, qui est généralement configuré pour synchroniser l’heure avec une source de temps externe.


## <a name="related-topics"></a>Rubriques connexes
Pour plus d’informations sur la hiérarchie de domaine et le système d’évaluation, consultez le billet de blog [What is Windows Time Service?](https://blogs.msdn.microsoft.com/w32time/2007/07/07/what-is-windows-time-service/) .

Le modèle de plug-in du fournisseur de temps Windows est [documenté sur TechNet](https://msdn.microsoft.com/library/windows/desktop/ms725475%28v=vs.85%29.aspx).

Un addendum référencé par l’article Précision de l’heure dans Windows Server 2016 peut être téléchargé [ici](https://windocs.blob.core.windows.net/windocs/WindowsTimeSyncAccuracy_Addendum.pdf).

Pour une vue d’ensemble rapide du service de temps Windows, jetez un coup d’œil à cette [vidéo de présentation générale](https://aka.ms/WS2016TimeVideo).
