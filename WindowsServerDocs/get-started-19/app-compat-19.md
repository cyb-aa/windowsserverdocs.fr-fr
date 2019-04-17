---
title: Compatibilité des applications Windows Server 2019 et Microsoft Server
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
ms.openlocfilehash: e8bb8cda003ca151216e3b86d5884dfd05e73e6d
ms.sourcegitcommit: 5d55c3ebd1ceee7229ea9a9989c69cf93757ed88
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/21/2019
ms.locfileid: "9256942"
---
# Compatibilité des applications Windows Server 2019 et Microsoft Server

>S'applique à: WindowsServer2019

Le tableau suivant répertorie les applications serveur Microsoft qui prennent en charge l’installation et fonctionnalités sur Window Server 2019. Ces informations servent de référence rapide et ne sont pas destinées à remplacer les spécifications produits, les configurations requises, les annonces individuels ou les communications générales pour chaque application serveur. Reportez-vous à la documentation officielle de chaque produit afin de mieux comprendre la compatibilité et les options.

Si vous êtes un partenaire fournisseur de logiciels vous recherchez pour plus d’informations sur la compatibilité de Windows Server avec les applications non Microsoft, visitez le [portail de Certification des applications commerciales](https://commercialappcertification.microsoft.com/).
| **Produit**                                                  | **Prise en charge sur Server Core**             |   | **Prise en charge sur le serveur avec expérience utilisateur** | **Publié?** |   | **Lien produit**                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
|--------------------------------------------------------------|------------------------------------------|---|-------------------------------------------------|---------------|---|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Exchange Server 2019                                         | Oui                                      |   | Oui                                             | Oui           |   | [Configuration requise du système Exchange Server](https://docs.microsoft.com/Exchange/plan-and-deploy/system-requirements?view=exchserver-2019)                                                                        |
| Host Integration Server 2016, CU3                            | Oui                                      |   | Oui                                             | Non            |   | [Configuration système requise pour Host Integration Server](https://docs.microsoft.com/host-integration-server/install-and-config-guides/system-requirements)                                                            |
| VisualStudio TeamFoundationServer2017                    | Yes\ *                                    |   | Oui                                             | Oui           |   | [TeamFoundationServer2017](https://docs.microsoft.com/tfs/server/requirements?view=vsts)                                                                                                                |
| Visual Studio Team Foundation Server 2018                    | Yes\ *                                    |   | Oui                                             | Oui           |   | [Team Foundation Server 2018](https://docs.microsoft.com/tfs/server/requirements?view=vsts)                                                                                                                  |
| MicrosoftSQLServer2014                                    | Yes\ *                                    |   | Oui                                             | Oui           |   | [Configuration matérielle et logicielle requise pour l'installation de SQLServer2014](https://docs.microsoft.com/sql/sql-server/install/hardware-and-software-requirements-for-installing-sql-server?view=sql-server-2014)   |
| MicrosoftSQLServer2016                                    | Yes\ *                                    |   | Oui                                             | Oui           |   | [Matérielle et logicielle requise pour l’installation de SQL Server 2016](https://docs.microsoft.com/sql/sql-server/install/hardware-and-software-requirements-for-installing-sql-server?view=sql-server-2016)   |
| Microsoft SQL Server 2017                                    | Yes\ *                                    |   | Oui                                             | Oui           |   | [Matérielle et logicielle requise pour l’installation de SQL Server 2017](https://docs.microsoft.com/sql/sql-server/install/hardware-and-software-requirements-for-installing-sql-server?view=sql-server-2017) |
| Microsoft System Center Configuration Manager (version 1806) | Oui, en tant que client géré, non comme serveur de site |   | Oui, en tant que client géré, non comme serveur de site        | Oui           |   | [Quelles sont les nouveautés dans la version 1806 de System Center Configuration Manager](https://docs.microsoft.com/sccm/core/plan-design/changes/whats-new-in-version-1806)                                                    |
| Microsoft System Center Operations Manager 2019              | Yes\ *                                    |   | Oui                                             | Oui           |   | [Configuration système requise pour System Center Operations Manager](https://docs.microsoft.com/system-center/scom/plan-system-requirements)                                                                                                      |
| Microsoft System Center Virtual Machine Manager 2019         | Yes\ *                                    |   | Oui                                             | Oui           |   | [Configuration système requise pour System Center Virtual Machine Manager](https://docs.microsoft.com/system-center/vmm/system-requirements)                                                                                                      |
| Microsoft System Center Data Protection Manager 2019         | Non                                       |   | Oui                                             | Oui           |   | [Préparation de votre environnement pour System Center Data Protection Manager](https://docs.microsoft.com/system-center/dpm/prepare-environment-for-dpm?view=sc-dpm-2019)                                                                                                      |
| SharePoint Server2016                                       | Non                                       |   | Oui                                             | Oui           |   | [Configuration matérielle et logicielle requise pour une solution SharePoint Server2016](https://docs.microsoft.com/SharePoint/install/hardware-and-software-requirements)                                                                |
| SharePoint Server 2019                                       | Non                                       |   | Oui                                             | Oui           |   | [Configuration matérielle et logicielle pour SharePoint Server 2019](https://docs.microsoft.com/sharepoint/install/hardware-and-software-requirements-2019)                                                       |
| Project Server2016                                          | Non                                       |   | Oui                                             | Oui           |   | [Configuration logicielle requise pour ProjectServer2016](https://docs.microsoft.com/project/software-requirements-for-project-server-2016)                                                                                |
| Project Server 2019                                          | Non                                       |   | Oui                                             | Oui           |   | [Configuration logicielle requise pour Project Server 2019](https://docs.microsoft.com/project/software-requirements-for-project-server-2019)                                                                          |
| Skype pour les entreprises 2019                                      | Non                                       |   | Oui                                             | Oui           |   | [Installer les conditions préalables pour Skype entreprise Server](https://docs.microsoft.com/skypeforbusiness/deploy/install/install-prerequisites)                                                                          |

\*May présentent certaines limitations ou nécessiter le [fonctionnalité de compatibilité des applications Server Core sur la demande (DOM). ](install-fod-19.md)
Veuillez vous référer au produit spécifique ou de la documentation de la demande.
