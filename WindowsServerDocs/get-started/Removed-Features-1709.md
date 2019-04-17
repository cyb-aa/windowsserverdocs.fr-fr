---
title: Fonctionnalités supprimées ou dont la suppression est prévue à compter de WindowsServer (version1709)
description: Fonctionnalités supprimées ou dont la suppression est prévue dans les prochaines versions.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 06/05/2018
author: jaimeo
ms.author: jaimeo
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: 47d90c32f705157af60b1d8ca38122b3c6363c0f
ms.sourcegitcommit: e84e328c13a701e8039b16a4824a6e58a6e59b0b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/22/2018
ms.locfileid: "4133845"
---
# Fonctionnalités supprimées ou dont la suppression est prévue à compter de WindowsServer, version1709

>S’applique à WindowsServer, version1709

Voici la liste des fonctionnalités de WindowsServer, version1709 qui ont été supprimées de cette version du produit ou dont la suppression est envisagée dans les prochaines versions. Ce produit s’adresse aux professionnels de l’informatique qui mettent à jour des systèmes d’exploitation dans un environnement commercial. **Cette liste peut faire l’objet de modifications dans des versions ultérieures. Il est donc possible que certaines fonctionnalités concernées n’y figurent plus.** 

## Fonctionnalités supprimées de WindowsServer, version1709
WindowsServer, version1709 contient les fonctionnalités déjà présentes dans WindowsServer2016. Toutefois, cette version propose des options d’installation différentes de celles de WindowsServer2016:

- En tant que version de canal semi-annuel, WindowsServer, version1709 propose uniquement l’option d’installation minimale. Pour plus d’informations, voir [Présentation du canal semi-annuel de Windows Server](semi-annual-channel-overview.md).
- À compter de cette version, NanoServer n’est plus disponible en tant que système d’exploitation hôte pouvant être installé. NanoServer est désormais disponible en tant que système d’exploitation de conteneur. Voir [Modifications apportées à NanoServer dans WindowsServer, version1709](nano-in-semi-annual-channel.md).
- À partir de cette version, Server Message Block (SMB) version 1 n’est plus installé par défaut. Pour plus d’informations, voir [SMBv1 n’est pas installé par défaut dans Windows 10 Fall Creators Update et Windows Server, version 1709 et versions ultérieures](https://support.microsoft.com/help/4034314/smbv1-is-not-installed-by-default-in-windows).


## Fonctionnalités dont la suppression est envisagée dans les prochaines versions

La suppression des fonctionnalités suivantes est envisagée dans les versions faisant suite à WindowsServer, version1709. Par la suite, elles peuvent être entièrement supprimées de l’image de produit installée et remplacées par d’autres fonctionnalités (ou être installables à partir d’autres sources), mais elles restent dans cette version avec, parfois, des caractéristiques qui ont été supprimées. Nous vous recommandons dès à présent de faire le nécessaire pour trouver une nouvelle solution de remplacement ou d’autres méthodes à employer si vos applications, votre code ou des modes d’utilisation dépendent de ces fonctionnalités.

Si vous voulez nous faire part de commentaires à propos de la proposition de suppression de certaines de ces fonctionnalités, vous pouvez utiliser l’[application Hub de commentaires](https://support.microsoft.com/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub-app). Bien que cette application s’exécute sur Windows10, vous pouvez également l’utiliser pour nous envoyer vos commentaires sur le produit (et la documentation) WindowsServer.

### Compatibilité avec la gestion IIS6
Les fonctionnalités spécifiques dont la suppression est envisagée sont:

- Compatibilité avec la métabaseIIS6 (Web-Metabase)
- Console de gestionIIS6 (Web-Lgcy-Mgmt-Console)
- Outils de scriptIIS6 (Web-Lgcy-Scripting)
- Compatibilité avec le service WMIIIS6 (Web-WMI)

Au lieu d’utiliser la compatibilité de métabase IIS6 (qui sert de couche d’émulation entre les scripts de métabase IIS6 et la configuration basée sur des fichiers utilisée par IIS7 ou versions ultérieures), il est recommandé de commencer la migration des scripts de gestion directement vers une configuration basée sur des fichiersIIS à l’aide d’outils tels que l’espace de noms Microsoft.Web.Administration.

Vous devez également commencer la migration à partir d’IIS6.0, ou versions antérieures, vers la dernière version d’IIS, qui est toujours disponible dans la version la plus récente de WindowsServer.


### Authentification DigestIIS
La suppression de cette méthode d’authentification est prévue. À la place, vous devez commencer à utiliser d’autres méthodes d’authentification telles que le mappage du certificat client (voir [Configuration de l’authentification de mappage de certificats clients un-à-un](https://docs.microsoft.com/iis/manage/configuring-security/configuring-one-to-one-client-certificate-mappings)) ou l’authentification Windows (voir [Paramètres d’application](https://docs.microsoft.com/iis-administration/configuration/appsettings.json)).

### Service iSNS (Internet Storage Name Service)
La suppression d’iSNS est envisagée. La fonctionnalité SMB (SMB) propose essentiellement les mêmes fonctionnalités, avec quelques ajouts. Voir [Protocole SMB (Server Message Block)](https://technet.microsoft.com/library/hh831795(v=ws.11).aspx) pour obtenir des informations d’ordre général sur cette fonctionnalité.

### Chiffrement RSA/AES pour IIS 
La suppression de cette méthode de chiffrement est envisagée, car la méthode Cryptography API: Next Generation (CNG), plus performante, est déjà disponible. Pour en savoir plus sur le chiffrement CNG, voir [À propos de CNG](https://msdn.microsoft.com/library/windows/desktop/aa375276(v=vs.85).aspx).

### WindowsPowerShell2.0
Cette ancienne version de WindowsPowerShell a été remplacée par plusieurs versions plus récentes. Pour bénéficier de fonctionnalités et de performances optimales, migrez vers WindowsPowerShell5.0 ou version ultérieure. Pour obtenir des informations détaillées, voir la [Documentation PowerShell](https://docs.microsoft.com/powershell/index?view=powershell-5.1).

