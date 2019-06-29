---
ms.assetid: e34622ff-b2d0-4f81-8d00-dacd5d6c215e
title: Service de temps Windows
description: ''
author: shortpatti
ms.author: pashort
manager: dougkim
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking
ms.openlocfilehash: 3233434403594ef9e2555c0329c4791d1fb99709
ms.sourcegitcommit: 63926404009f9e1330a4a0aa8cb9821a2dd7187e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67469590"
---
# <a name="windows-time-service-w32time"></a>Service de temps Windows (W32Time)

>S’applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10 ou version ultérieure

Le service de temps de Windows (W32Time) synchronise la date et l’heure pour tous les ordinateurs en cours d’exécution dans les Services de domaine Active Directory (AD DS). Synchronisation date / heure est essentielle pour le bon fonctionnement de nombreux services de Windows et les applications line of business (LOB). Le service de temps de Windows utilise le protocole NTP (Network Time) pour synchroniser les horloges d’ordinateur sur le réseau. NTP garantit qu’une valeur d’horloge précise, ou horodateur, peut être affecté aux demandes d’accès aux ressources et de validation du réseau.

Dans la rubrique de Service de temps Windows (W32Time), le contenu suivant est disponible :
- **[Heure précise de Windows Server 2016](accurate-time.md).** Précision de synchronisation de temps dans Windows Server 2016 a été considérablement améliorée, tout en conservant une compatibilité NTP avec les versions antérieures de Windows. Conditions d’exploitation raisonnable, vous pouvez conserver un 1 précision ms en ce qui concerne l’heure UTC ou mieux pour les membres de domaine Windows Server 2016 et de mise à jour anniversaire de Windows 10.
- **[Limites de prise en charge pour les environnements de haute-précision](support-boundary.md).** Cet article décrit les limites de prise en charge pour le service de temps de Windows (W32Time) dans les environnements nécessitant un temps système très précis et plus stable.
- **[Configuration des systèmes de haute précision](configuring-systems-for-high-accuracy.md).** Synchronisation date / heure dans Windows 10 et Windows Server 2016 a été considérablement améliorée.  Conditions raisonnable d’exploitation, les systèmes peuvent être configurés pour mettre à jour de 1 ms (millisecondes) ou mieux (par rapport à UTC).
- **[Heure de Windows pour la traçabilité](windows-time-for-traceability.md).** Réglementations dans de nombreux secteurs requièrent des systèmes garantissant la traçabilité en temps UTC.  Cela signifie que vous pouvez attesté par décalage d’un système en ce qui concerne l’heure UTC.  Pour activer les scénarios de conformité aux réglementations, Windows 10 et Server 2016 fournit les nouveaux journaux des événements pour fournir une image du point de vue du système d’exploitation pour former une compréhension des actions effectuées sur l’horloge système.  Ces journaux des événements sont générés en continu pour le service de temps de Windows et peut être examiné ou archivé pour une analyse ultérieure.
- **[Référence technique du service Windows temps](windows-time-service-tech-ref.md).** Le service W32Time fournit la synchronisation d’horloge de réseau pour les ordinateurs sans nécessiter de configuration étendues. Le service W32Time est essentiel pour le bon fonctionnement de l’authentification Kerberos V5 et, par conséquent, pour l’authentification basée sur le service d’annuaire AD.
    - **[Fonctionnement du service de temps de Windows](How-the-Windows-Time-Service-Works.md).** Bien que le service de temps de Windows n’est pas une implémentation exacte du protocole NTP (Network Time), il utilise la suite d’algorithmes complexe qui est définie dans les spécifications NTP pour vous assurer que les horloges des ordinateurs à travers un réseau soient aussi précis que possible.
    - **[Outils de service Windows et les paramètres](Windows-Time-Service-Tools-and-Settings.md).** La plupart des ordinateurs membres du domaine sont de type client heure NT5DS, ce qui signifie que lorsqu’ils synchronisent à temps à partir de la hiérarchie de domaine. L’exception à cette règle uniquement classique est le contrôleur de domaine qui fonctionne comme le principal (PDC) émulateur maître d’opérations PDC du domaine racine de forêt, qui est généralement configuré pour synchroniser l’heure avec une source externe.


## <a name="related-topics"></a>Rubriques connexes
Pour plus d’informations sur la hiérarchie de domaine et le système de calcul de score, consultez le [« Quel est le Service de temps Windows ? »](https://blogs.msdn.microsoft.com/w32time/2007/07/07/what-is-windows-time-service/) billet de blog.

Le modèle de plug-in de fournisseur de temps windows est [documentée sur TechNet](https://msdn.microsoft.com/library/windows/desktop/ms725475%28v=vs.85%29.aspx).

Un addenda référencé par l’article Windows 2016 précise temps peut être téléchargé [ici](https://windocs.blob.core.windows.net/windocs/WindowsTimeSyncAccuracy_Addendum.pdf)

Pour une présentation rapide du service de temps de Windows, examinons ce [vidéo de présentation de haut niveau](https://aka.ms/WS2016TimeVideo).
