---
title: Compatibilité des applications Microsoft Server et de Windows Server 2019
description: Tableau de compatibilité des applications serveur Microsoft et Windows Server 2019
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2afe7c32-1fda-4441-989b-4115dabdcd34
author: coreyp
ms.author: coreyp-at-msft
manager: jasgroce
ms.localizationpriority: medium
ms.openlocfilehash: 8dcaff6ab8a296790158f59035bd4a5c1a093cbd
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66442373"
---
# <a name="windows-server-2019-and-microsoft-server-application-compatibility"></a>Compatibilité des applications Microsoft Server et de Windows Server 2019

>S'applique à : Windows Server 2019

Ce tableau répertorie les applications serveur Microsoft qui prennent en charge d’installation et les fonctionnalités sur la fenêtre Server 2019. Ces informations servent de référence rapide et ne sont pas destinées à remplacer les spécifications produits, les configurations requises, les annonces individuels ou les communications générales pour chaque application serveur. Reportez-vous à la documentation officielle de chaque produit afin de mieux comprendre la compatibilité et les options.

Si vous êtes un partenaire fournisseur de logiciel vous recherchez plus d’informations sur la compatibilité de Windows Server avec des applications non Microsoft, visitez le [portail de Certification des applications commerciales](https://commercialappcertification.microsoft.com/).

| **Produit**                                                  | **Prise en charge sur Server Core**             |   | **Prise en charge sur le serveur avec expérience utilisateur** | **Publié ?** |   | **Lien Web de produit**                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
|--------------------------------------------------------------|------------------------------------------|---|-------------------------------------------------|---------------|---|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Exchange Server 2019                                         | Oui                                      |   | Oui                                             | Oui           |   | [Requise pour Exchange Server](https://docs.microsoft.com/Exchange/plan-and-deploy/system-requirements?view=exchserver-2019)                                                                        |
| Host Integration Server 2016, CU3                            | Oui                                      |   | Oui                                             | Oui            |   | [Configuration système requise pour Host Integration Server](https://docs.microsoft.com/host-integration-server/install-and-config-guides/system-requirements)                                                            |
| Visual Studio Team Foundation Server 2017                    | Oui\*                                    |   | Oui                                             | Oui           |   | [Team Foundation Server 2017](https://docs.microsoft.com/tfs/server/requirements?view=vsts)                                                                                                                |
| Visual Studio Team Foundation Server 2018                    | Oui\*                                    |   | Oui                                             | Oui           |   | [Team Foundation Server 2018](https://docs.microsoft.com/tfs/server/requirements?view=vsts)                                                                                                                  |
| Microsoft SQL Server 2014                                    | Oui\*                                    |   | Oui                                             | Oui           |   | [Matérielle et logicielle requise pour l’installation de SQL Server 2014](https://docs.microsoft.com/sql/sql-server/install/hardware-and-software-requirements-for-installing-sql-server?view=sql-server-2014)   |
| Microsoft SQL Server 2016                                    | Oui\*                                    |   | Oui                                             | Oui           |   | [Matérielle et logicielle requise pour l’installation de SQL Server 2016](https://docs.microsoft.com/sql/sql-server/install/hardware-and-software-requirements-for-installing-sql-server?view=sql-server-2016)   |
| Microsoft SQL Server 2017                                    | Oui\*                                    |   | Oui                                             | Oui           |   | [Matérielle et logicielle requise pour l’installation de SQL Server 2017](https://docs.microsoft.com/sql/sql-server/install/hardware-and-software-requirements-for-installing-sql-server?view=sql-server-2017) |
| Microsoft System Center Configuration Manager (version 1806) | Oui en tant que client géré, non en tant que serveur de site |   | Oui en tant que client géré, non en tant que serveur de site        | Oui           |   | [Quelles sont les nouveautés dans la version 1806 de System Center Configuration Manager](https://docs.microsoft.com/sccm/core/plan-design/changes/whats-new-in-version-1806)                                                    |
| Microsoft System Center Operations Manager 2019              | Oui\*                                    |   | Oui                                             | Oui           |   | [Configuration système requise pour System Center Operations Manager](https://docs.microsoft.com/system-center/scom/plan-system-requirements)                                                                                                      |
| Microsoft System Center Virtual Machine Manager 2019         | Oui\*                                    |   | Oui                                             | Oui           |   | [Configuration système requise pour System Center Virtual Machine Manager](https://docs.microsoft.com/system-center/vmm/system-requirements)                                                                                                      |
| Microsoft System Center Data Protection Manager 2019         | Non                                       |   | Oui                                             | Oui           |   | [Préparation de votre environnement pour System Center Data Protection Manager](https://docs.microsoft.com/system-center/dpm/prepare-environment-for-dpm?view=sc-dpm-2019)                                                                                                      |
| SharePoint Server 2016                                       | Non                                       |   | Oui                                             | Oui           |   | [Configuration matérielle et logicielle pour SharePoint Server 2016](https://docs.microsoft.com/SharePoint/install/hardware-and-software-requirements)                                                                |
| SharePoint Server 2019                                       | Non                                       |   | Oui                                             | Oui           |   | [Matérielle et logicielle requise pour SharePoint Server 2019](https://docs.microsoft.com/sharepoint/install/hardware-and-software-requirements-2019)                                                       |
| Project Server 2016                                          | Non                                       |   | Oui                                             | Oui           |   | [Configuration logicielle requise pour Project Server 2016](https://docs.microsoft.com/project/software-requirements-for-project-server-2016)                                                                                |
| Project Server 2019                                          | Non                                       |   | Oui                                             | Oui           |   | [Configuration logicielle requise pour Project Server 2019](https://docs.microsoft.com/project/software-requirements-for-project-server-2019)                                                                          |
| Skype pour entreprise 2019                                      | Non                                       |   | Oui                                             | Oui           |   | [Installez les composants requis pour Skype pour Business Server](https://docs.microsoft.com/skypeforbusiness/deploy/install/install-prerequisites)                                                                          |

\*Peut avoir des limitations ou peut nécessiter la [fonctionnalité compatibilité sur la demande (DOM) de Server Core application](install-fod-19.md).
Reportez-vous à la documentation de DOM ou de produit spécifique.
