---
title: Référentiel de logiciels Linux pour les produits Microsoft
description: Ce document explique comment utiliser et installer des packages logiciels Linux pour les produits Microsoft.
ms.custom: na
ms.prod: windows-server-threshold
ms.service: na
manager: szark
ms.technology: compute
ms.topic: article
ms.assetid: b5387444-595f-4f38-abb7-163a70ea1895
author: szarkos
ms.author: szark
ms.date: 10/16/2017
ms.openlocfilehash: bade9fff306272188ac8d2b91a3d9921c80fe036
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70866890"
---
# <a name="linux-software-repository-for-microsoft-products"></a>Référentiel de logiciels Linux pour les produits Microsoft

## <a name="overview"></a>Vue d'ensemble
Microsoft crée et prend en charge un large éventail de produits logiciels pour les systèmes Linux et les met à disposition via des référentiels de packages APT et YUM standard. Ce document décrit comment configurer le référentiel sur votre système Linux, afin que vous puissiez installer/mettre à niveau le logiciel Linux de Microsoft à l’aide des outils de gestion des packages standard de votre distribution.

Le référentiel de logiciels Linux de Microsoft est constitué de plusieurs sous-référentiels :

 - Prod : le sous-dépôt de production est désigné pour les packages destinés à être utilisés en production. Ces packages sont pris en charge par Microsoft conformément aux termes du contrat de support applicable ou du programme que vous avez avec Microsoft.

 - MSSQL-Server : ces référentiels contiennent des packages pour Microsoft SQL Server sur Linux-voir aussi : [SQL Server sur Linux](https://www.microsoft.com/en-us/sql-server/sql-server-vnext-including-Linux).

> [!Note]
> Les packages dans les référentiels de logiciels Linux sont soumis aux termes du contrat de licence figurant dans les packages. Lisez les termes du contrat de licence avant d’utiliser le package. Le fait d’installer et d’utiliser le package implique l’acceptation de ces termes. Si vous n’acceptez pas les termes du contrat de licence, n’utilisez pas le package.


## <a name="configuring-the-repositories"></a>Configuration des référentiels
Les référentiels peuvent être configurés automatiquement en installant le package Linux qui s’applique à la distribution et à la version de Linux. Le package installe la configuration du référentiel avec la clé publique GPG utilisée par les outils tels que apt/yum/zypper pour valider les métadonnées de packages et/ou de référentiel signés.

### <a name="enterprise-linux-rhel-and-variants"></a>Enterprise Linux (RHEL et variants)

 - Enterprise Linux 6 (EL6)

        sudo rpm -Uvh https://packages.microsoft.com/config/rhel/6/packages-microsoft-prod.rpm

 - Enterprise Linux 7 (EL7)

        sudo rpm -Uvh https://packages.microsoft.com/config/rhel/7/packages-microsoft-prod.rpm


### <a name="ubuntu"></a>Ubuntu

 - Ubuntu 14,04 (confiance)

        curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
        sudo apt-add-repository https://packages.microsoft.com/ubuntu/14.04/prod
        sudo apt-get update

 - Ubuntu 16,04 (Xenial)

        curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
        sudo apt-add-repository https://packages.microsoft.com/ubuntu/16.04/prod
        sudo apt-get update

 - Ubuntu 18,04 (Bionic)

        curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
        sudo apt-add-repository https://packages.microsoft.com/ubuntu/18.04/prod
        sudo apt-get update

 - Ubuntu 18,10 (cosmique)

        curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
        sudo apt-add-repository https://packages.microsoft.com/ubuntu/18.10/prod
        sudo apt-get update

 - Ubuntu 19,04 (Disco)

        curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
        sudo apt-add-repository https://packages.microsoft.com/ubuntu/19.04/prod
        sudo apt-get update

### <a name="suse-linux-enterprise-12"></a>SUSE Linux Enterprise 12

        sudo rpm -Uvh https://packages.microsoft.com/config/sles/12/packages-microsoft-prod.rpm


## <a name="manual-configuration"></a>Configuration manuelle
Les fichiers de configuration du référentiel sont disponibles à partir de [packages.Microsoft.com/config](https://packages.microsoft.com/config/). Le nom et l’emplacement de ces fichiers peuvent être localisés à l’aide de la Convention d’affectation de noms d’URI suivante :

        https://packages.microsoft.com/config/<Distribution>/<Version>/prod.(repo|list)

**Clé de signature de package et de dépôt**

 - La clé publique GPG de Microsoft peut être téléchargée ici :[https://packages.microsoft.com/keys/microsoft.asc](https://packages.microsoft.com/keys/microsoft.asc)
 - ID de la clé publique : Microsoft (signature de version)<gpgsecurity@microsoft.com>
 - Empreinte digitale de la clé publique :`BC52 8686 B50D 79E3 39D3 721C EB3E 94AD BE12 29CF`

### <a name="examples"></a>Exemples :

 - RHEL/CentOS 7

        # Install repository configuration
        curl https://packages.microsoft.com/config/rhel/7/prod.repo > ./microsoft-prod.repo
        sudo cp ./microsoft-prod.repo /etc/yum.repos.d/

        # Install Microsoft's GPG public key
        curl https://packages.microsoft.com/keys/microsoft.asc > ./microsoft.asc
        sudo rpm --import ./microsoft.asc

 - Ubuntu 16.04

        # Install repository configuration
        curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > ./microsoft-prod.list
        sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/

        # Install Microsoft GPG public key
        curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
        sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/



