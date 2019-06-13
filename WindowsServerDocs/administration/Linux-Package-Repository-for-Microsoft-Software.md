---
title: Référentiel de logiciels Linux pour les produits Microsoft
description: Ce document décrit comment utiliser et installer les packages de logiciels Linux pour les produits Microsoft.
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
ms.openlocfilehash: 77b309739125a2114ef4ada4adb305f4dd169b06
ms.sourcegitcommit: 927adf32faa6052234ad08f21125906362e593dc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/12/2019
ms.locfileid: "67033323"
---
# <a name="linux-software-repository-for-microsoft-products"></a>Référentiel de logiciels Linux pour les produits Microsoft

## <a name="overview"></a>Vue d'ensemble
Microsoft génère et prend en charge une variété de produits logiciels pour les systèmes Linux et les rend disponibles via des référentiels de package APT et YUM standards. Ce document décrit comment configurer le référentiel sur votre système Linux, afin que vous pouvez ensuite installation/mise à niveau Linux logiciels Microsoft à l’aide des outils de gestion de package standard de votre distribution.

Référentiel de logiciels de Microsoft Linux se compose de plusieurs référentiels secondaires :

 - prod – la Production sous-référentiel est désigné pour les packages destinés à utiliser en production. Ces packages sont pris en charge dans le commerce par Microsoft sous les termes du contrat de support applicable ou programme que vous avez avec Microsoft.

 - MSSQL-server - ces référentiels contiennent des packages pour Microsoft SQL Server sur Linux - Voir également : [SQL Server sur Linux](https://www.microsoft.com/en-us/sql-server/sql-server-vnext-including-Linux).

> [!Note]
> Les packages dans les référentiels de logiciels Linux sont soumis aux termes du contrat de licence situé dans les packages. Veuillez lire les termes du contrat de licence avant d’utiliser le package. Votre installation et l’utilisation du package, vous acceptez ces termes. Si vous n’acceptez pas les termes du contrat de licence, n’utilisez pas le package.


## <a name="configuring-the-repositories"></a>Configuration des référentiels
Référentiels peuvent être configurées automatiquement en installant le package de Linux qui s’applique à votre distribution Linux et la version. Le package installe la configuration du référentiel, ainsi que la clé publique GPG utilisée par des outils tels qu’apt/yum/zypper pour valider les packages signés et/ou les métadonnées du référentiel.

### <a name="enterprise-linux-rhel-and-variants"></a>Enterprise Linux (RHEL et variantes)

 - Enterprise Linux 6 (EL6)

        sudo rpm -Uvh https://packages.microsoft.com/config/rhel/6/packages-microsoft-prod.rpm

 - Enterprise Linux 7 (EL7)

        sudo rpm -Uvh https://packages.microsoft.com/config/rhel/7/packages-microsoft-prod.rpm


### <a name="ubuntu"></a>Ubuntu

 - Ubuntu 14.04 (fidèle)

        curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
        sudo apt-add-repository https://packages.microsoft.com/ubuntu/14.04/prod
        sudo apt-get update

 - Ubuntu 16.04 (Xenial)

        curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
        sudo apt-add-repository https://packages.microsoft.com/ubuntu/16.04/prod
        sudo apt-get update

 - Ubuntu 18.04 (bionique)

        curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
        sudo apt-add-repository https://packages.microsoft.com/ubuntu/18.04/prod
        sudo apt-get update

 - Ubuntu 18.10 (Cosmic)

        curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
        sudo apt-add-repository https://packages.microsoft.com/ubuntu/18.10/prod
        sudo apt-get update

 - Ubuntu 19.04 (Disco)

        curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
        sudo apt-add-repository https://packages.microsoft.com/ubuntu/19.04/prod
        sudo apt-get update

### <a name="suse-linux-enterprise-12"></a>SUSE Linux Enterprise 12

        sudo rpm -Uvh https://packages.microsoft.com/config/sles/12/packages-microsoft-prod.rpm


## <a name="manual-configuration"></a>Configuration manuelle
Les fichiers de configuration du référentiel sont disponibles à partir de [packages.microsoft.com/config](https://packages.microsoft.com/config/). Le nom et l’emplacement de ces fichiers peuvent être déterminés à l’aide de la convention d’affectation de noms URI suivante :

        https://packages.microsoft.com/config/<Distribution>/<Version>/prod.(repo|list)

**Clé de package et la signature de référentiel**

 - Clé de Microsoft GPG publique peut être téléchargé ici : [https://packages.microsoft.com/keys/microsoft.asc](https://packages.microsoft.com/keys/microsoft.asc)
 - ID de clé publique : Microsoft (signature de version) <gpgsecurity@microsoft.com>
 - Empreinte digitale de clé publique : `BC52 8686 B50D 79E3 39D3 721C EB3E 94AD BE12 29CF`

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



