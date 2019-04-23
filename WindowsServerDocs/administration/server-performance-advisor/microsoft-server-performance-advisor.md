---
title: Microsoft Server Performance Advisor
description: Microsoft Server Performance Advisor
ms.assetid: 468ebcb3-9eaf-477c-ab10-e3f1b3ce63dc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: manage
ms.openlocfilehash: ab124f3efabf2ac3ae8904157a81587c0c21ebb5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856330"
---
# <a name="microsoft-server-performance-advisor"></a>Microsoft Server Performance Advisor

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Téléchargez Microsoft Server Performance Advisor (SPA) pour aider à diagnostiquer les problèmes de performances dans un déploiement de Windows Server 2008, Windows Server 2012, de Windows Server 2008 R2 ou Windows Server 2012 R2. SPA génère des graphiques et des rapports de diagnostics détaillés et fournit des recommandations pour vous aider à rapidement analysent les problèmes et développement des actions correctives.

-   [Vue d’ensemble de Server Performance Advisor](#bkmk-aboutspa)

-   [Télécharger Server Performance Advisor](#bkmk-downloadspa)

-   [Guide d’utilisation de Server Performance Advisor](server-performance-advisor-users-guide.md)

-   [Guide de développement de Pack Server Performance Advisor](server-performance-advisor-pack-development-guide.md)

## <a href="" id="bkmk-aboutspa"></a>Vue d’ensemble de Server Performance Advisor

Server Performance Advisor est composée de deux parties : le framework SPA et les packs d’Advisor de SPA.

### <a name="the-server-performance-advisor-framework"></a>L’infrastructure Server Performance Advisor

Le moteur est responsable de la collecte de données qui sont désignées par les packs d’advisor, écrire les données collectées dans une base de données Microsoft SQL Server, création d’un environnement informatique pour exécuter des scripts pour les packs d’Advisor SPA et l’affichage les États finaux. Vous devez uniquement installer le framework SPA sur la console SPA. La console de l’application à page unique peut être installée sur un ordinateur autonome à accéder aux serveurs sous test à distance ou d’être installé sur un serveur de test.

### <a name="server-performance-advisor-packs"></a>Packs Server Performance Advisor

Les packs d’Advisor SPA sont au centre de toutes les règles de paramétrage, qui consistent en une série de métadonnées et les fichiers de script SQL. SPA est fourni avec les packs d’Advisor suivants :

-   Le pack de conseiller Core du système d’exploitation analyse les performances des fonctions du système d’exploitation général, indépendantes des rôles de serveur spécialisés.

-   Le pack de l’Assistant Internet Information Server (IIS) suit les performances d’IIS.

-   Le Pack de conseiller Hyper-V analyse les performances générales du rôle serveur Hyper-V.

    **Remarque** le Pack d’Advisor Hyper-V n’analyse pas les systèmes d’exploitation invités.

     

-   Le pack de l’Assistant active directory analyse les performances générales du rôle active directory.

SPA offre également un modèle extensible pour les développeurs non Microsoft d’écrire des packs d’advisor en fonction de leurs besoins.

**Remarque** SPA ne peut pas comprendre tous les contextes de scénario et du matériel. Vous devez utiliser les recommandations fournies par l’outil pour vous aider à prendre des décisions et comprendre les conséquences des modifications potentielles qui sont apportées aux serveurs.

 

## <a href="" id="bkmk-downloadspa"></a>Télécharger Server Performance Advisor


Utilisez les liens suivants pour télécharger Server Performance Advisor pour Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 ou Windows Server 2008 :

-   [Microsoft Server Performance Advisor 3.1 (32 bits)](https://go.microsoft.com/fwlink/p/?linkid=327751)

-   [Microsoft Server Performance Advisor 3.1 (64 bits)](https://go.microsoft.com/fwlink/p/?linkid=327752)

Vous pouvez extraire les fichiers dans le fichier CAB en utilisant les commandes suivantes :

-   pour le x86 version : `extrac32.exe /e /a /l  d:\SPA   d:\SPA\SPAPlus\_x86.cab`

-   pour le x64 version : `extrac32.exe /e /a /l  d:\SPA   d:\SPA\SPAPlus\_amd64.cab`

**Attention** lorsque vous extrayez le fichier .cab, SPA doive conserver la structure de répertoire hiérarchique pour fonctionner correctement. En fonction des outils CAB qui sont installés sur votre serveur, l’extraction peut entraîner une structure de répertoires non opérationnelle. Pour conserver la structure de répertoires hiérarchiques, vous pouvez utiliser un outil d’utilitaire CAB d’extraction qui extrait une structure de répertoire du fichier.

Si l’outil d’extraction de fichier CAB extrait correctement les fichiers, les sous-dossiers apparaissent automatiquement dans le dossier de la cible d’extraction.

### <a name="spa-prerequisites"></a>Conditions préalables SPA

La Console de SPA nécessite que les logiciels suivants sont installés :

-   Microsoft .NET Framework 4

-   Express de SQL Server 2008 R2 SP1 ou Microsoft SQL Server 2008 R2 SP1

Versions plus récentes peuvent être compatibles. Les incompatibilités connues produit avec la Console de SPA seront notées.
