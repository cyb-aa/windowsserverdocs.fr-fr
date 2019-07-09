---
title: Compatibilité des applications serveur Microsoft et Windows Server 2019
description: Tableau de compatibilité des applications serveur Microsoft et Windows Server 2019
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
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/17/2019
ms.locfileid: "66442373"
---
# <a name="windows-server-2019-and-microsoft-server-application-compatibility"></a>Compatibilité des applications serveur Microsoft et Windows Server 2019

>S'applique à : Windows Server 2019

Ce tableau répertorie les applications serveur Microsoft qui prennent en charge l’installation et les fonctionnalités de Windows Server 2019. Ces informations servent de référence rapide et ne sont pas destinées à remplacer les spécifications produits, les configurations requises, les annonces individuels ou les communications générales pour chaque application serveur. Reportez-vous à la documentation officielle de chaque produit afin de mieux comprendre la compatibilité et les options.

Si vous êtes partenaire fournisseur de logiciels et que vous souhaitez obtenir plus d’informations sur la compatibilité de Windows Server avec les applications non Microsoft, consultez le [portail de certification des applications commerciales](https://commercialappcertification.microsoft.com/).

| **Produit**                                                  | **Pris en charge sur Server Core**             |   | **Pris en charge sur Server avec Expérience utilisateur** | **Publié ?** |   | **Lien web vers le produit**                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
|--------------------------------------------------------------|------------------------------------------|---|-------------------------------------------------|---------------|---|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Exchange Server 2019                                         | Oui                                      |   | Oui                                             | Oui           |   | [Configuration système pour Exchange Server](https://docs.microsoft.com/Exchange/plan-and-deploy/system-requirements?view=exchserver-2019)                                                                        |
| Host Integration Server 2016, CU3                            | Oui                                      |   | Oui                                             | Oui            |   | [Configuration requise pour Host Integration Server](https://docs.microsoft.com/host-integration-server/install-and-config-guides/system-requirements)                                                            |
| Visual Studio Team Foundation Server 2017                    | \*Oui                                    |   | Oui                                             | Oui           |   | [Team Foundation Server 2017](https://docs.microsoft.com/tfs/server/requirements?view=vsts)                                                                                                                |
| Visual Studio Team Foundation Server 2018                    | \*Oui                                    |   | Oui                                             | Oui           |   | [Team Foundation Server 2018](https://docs.microsoft.com/tfs/server/requirements?view=vsts)                                                                                                                  |
| Microsoft SQL Server 2014                                    | \*Oui                                    |   | Oui                                             | Oui           |   | [Configuration matérielle et logicielle requise pour l’installation de SQL Server 2014](https://docs.microsoft.com/sql/sql-server/install/hardware-and-software-requirements-for-installing-sql-server?view=sql-server-2014)   |
| Microsoft SQL Server 2016                                    | \*Oui                                    |   | Oui                                             | Oui           |   | [Configuration matérielle et logicielle requise pour l’installation de SQL Server 2016](https://docs.microsoft.com/sql/sql-server/install/hardware-and-software-requirements-for-installing-sql-server?view=sql-server-2016)   |
| Microsoft SQL Server 2017                                    | \*Oui                                    |   | Oui                                             | Oui           |   | [Configuration matérielle et logicielle requise pour l’installation de SQL Server 2017](https://docs.microsoft.com/sql/sql-server/install/hardware-and-software-requirements-for-installing-sql-server?view=sql-server-2017) |
| Microsoft System Center Configuration Manager (version 1806) | Oui en tant que client géré, Non en tant que serveur de site |   | Oui en tant que client géré, Non en tant que serveur de site        | Oui           |   | [Nouveautés de la version 1806 de System Center Configuration Manager](https://docs.microsoft.com/sccm/core/plan-design/changes/whats-new-in-version-1806)                                                    |
| Microsoft System Center Operations Manager 2019              | \*Oui                                    |   | Oui                                             | Oui           |   | [Configuration requise pour System Operations Manager](https://docs.microsoft.com/system-center/scom/plan-system-requirements)                                                                                                      |
| Microsoft System Center Virtual Machine Manager 2019         | \*Oui                                    |   | Oui                                             | Oui           |   | [Configuration système requise pour System Center Virtual Machine Manager](https://docs.microsoft.com/system-center/vmm/system-requirements)                                                                                                      |
| Microsoft System Center Data Protection Manager 2019         | Non                                       |   | Oui                                             | Oui           |   | [Préparation de votre environnement pour System Center Data Protection Manager](https://docs.microsoft.com/system-center/dpm/prepare-environment-for-dpm?view=sc-dpm-2019)                                                                                                      |
| SharePoint Server 2016                                       | Non                                       |   | Oui                                             | Oui           |   | [Configuration matérielle et logicielle requise pour SharePoint Server 2016](https://docs.microsoft.com/SharePoint/install/hardware-and-software-requirements)                                                                |
| SharePoint Server 2019                                       | Non                                       |   | Oui                                             | Oui           |   | [Configuration matérielle et logicielle requise pour une solution SharePoint Server 2019](https://docs.microsoft.com/sharepoint/install/hardware-and-software-requirements-2019)                                                       |
| Project Server 2016                                          | Non                                       |   | Oui                                             | Oui           |   | [Configuration logicielle requise pour Project Server 2016](https://docs.microsoft.com/project/software-requirements-for-project-server-2016)                                                                                |
| Project Server 2019                                          | Non                                       |   | Oui                                             | Oui           |   | [Configuration logicielle requise pour Project Server 2019](https://docs.microsoft.com/project/software-requirements-for-project-server-2019)                                                                          |
| Skype Entreprise 2019                                      | Non                                       |   | Oui                                             | Oui           |   | [Conditions d’installation pour Skype Entreprise Server](https://docs.microsoft.com/skypeforbusiness/deploy/install/install-prerequisites)                                                                          |

\*Possibles limitations ou peut nécessiter la [Fonctionnalité de compatibilité des applications Server Core à la demande (FOD)](install-fod-19.md).
Consultez la documentation de DOM ou la documentation du produit.
