---
title: Microsoft Server Performance Advisor
description: Microsoft Server Performance Advisor
ms.assetid: 468ebcb3-9eaf-477c-ab10-e3f1b3ce63dc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.topic: article
ms.prod: windows-server
ms.technology: manage
ms.openlocfilehash: 49f6132cfe99d9d4b719aeeecf149ecb1d7b76f2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382992"
---
# <a name="microsoft-server-performance-advisor"></a>Microsoft Server Performance Advisor

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Téléchargez Microsoft Server Performance Advisor (SPA) pour faciliter le diagnostic des problèmes de performances dans un déploiement de Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 ou Windows Server 2008. SPA génère des rapports et des graphiques de diagnostic complets et fournit des recommandations pour vous aider à analyser rapidement les problèmes et à développer des actions correctives.

-   [Vue d’ensemble de Server Performance Advisor](#bkmk-aboutspa)

-   [Télécharger le conseiller de performances du serveur](#bkmk-downloadspa)

-   [Guide de l’utilisateur de Server Performance Advisor](server-performance-advisor-users-guide.md)

-   [Guide de développement de packs Server Performance Advisor](server-performance-advisor-pack-development-guide.md)

## <a href="" id="bkmk-aboutspa"></a>Vue d’ensemble de Server Performance Advisor

Server Performance Advisor est composé de deux parties : l’infrastructure SPA et les packs d’Advisor SPA.

### <a name="the-server-performance-advisor-framework"></a>Framework Server Performance Advisor

Moteur qui est responsable de la collecte des données désignées par les packs d’Advisor, de l’écriture des données collectées dans une base de données Microsoft SQL Server, de la création d’un environnement informatique pour exécuter des scripts pour les packs d’Advisor SPA et de l’indication des rapports finaux. Vous devez uniquement installer l’infrastructure SPA sur la console du SPA. La console du SPA peut être installée sur un ordinateur autonome pour accéder à distance aux serveurs testés ou être installée sur un serveur testé.

### <a name="server-performance-advisor-packs"></a>Packs Server Performance Advisor

Les packs d’Advisor SPA sont le centre de toutes les règles de paramétrage, qui consistent en une série de métadonnées et de fichiers de script SQL. SPA est fourni avec les packs d’aide Advisor suivants :

-   Le Pack de système d’exploitation de base analyse les performances des fonctions générales du système d’exploitation, indépendamment des rôles de serveur spécialisés.

-   Internet Information Server (IIS) Advisor Pack effectue le suivi des performances d’IIS.

-   Le pack d’avis Hyper-V analyse les performances générales du rôle de serveur Hyper-V.

    **Remarque** Le pack d’avis Hyper-V n’analyse pas les systèmes d’exploitation invités.

     

-   Active Directory Advisor Pack analyse les performances générales du rôle Active Directory.

SPA offre également un modèle extensible qui permet aux développeurs non-Microsoft d’écrire des packs d’Advisor en fonction de leurs besoins.

**Remarque** SPA ne peut pas comprendre tous les contextes de scénario matériel et utilisateur. Vous devez utiliser les recommandations fournies par l’outil pour vous aider à prendre des décisions et à comprendre les conséquences de toute modification potentielle apportée aux serveurs.

 

## <a href="" id="bkmk-downloadspa"></a>Télécharger le conseiller de performances du serveur


Utilisez les liens suivants pour télécharger Server Performance Advisor pour Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 ou Windows Server 2008 :

-   [Microsoft Server Performance Advisor 3,1 (32 bits)](https://go.microsoft.com/fwlink/p/?linkid=327751)

-   [Microsoft Server Performance Advisor 3,1 (64 bits)](https://go.microsoft.com/fwlink/p/?linkid=327752)

Vous pouvez extraire les fichiers du fichier CAB à l’aide des commandes suivantes :

-   pour la version x86 : `extrac32.exe /e /a /l  d:\SPA   d:\SPA\SPAPlus\_x86.cab`

-   pour la version x64 : `extrac32.exe /e /a /l  d:\SPA   d:\SPA\SPAPlus\_amd64.cab`

**Attention** Lorsque vous extrayez le fichier. cab, le SPA doit conserver la structure de répertoire hiérarchique pour fonctionner correctement. Selon les outils CAB installés sur votre serveur, l’extraction peut aboutir à une structure de répertoires non opérationnels. Pour conserver la structure hiérarchique des répertoires, vous pouvez utiliser un outil de l’utilitaire d’extraction de fichiers CAB qui extrait une structure de répertoires de fichiers.

Si l’outil d’extraction de fichiers CAB a correctement extrait les fichiers, les sous-dossiers s’affichent automatiquement dans le dossier cible d’extraction.

### <a name="spa-prerequisites"></a>Conditions préalables SPA

La console SPA requiert l’installation des logiciels suivants :

-   Microsoft .NET Framework 4

-   SQL Server 2008 R2 Express SP1 ou Microsoft SQL Server 2008 R2 SP1

Les versions plus récentes peuvent être compatibles. Toutes les incompatibilités connues avec la console SPA seront signalées.
