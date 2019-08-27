---
title: Fonctionnalités supprimées ou dont le remplacement est prévu à compter de Windows Server (version 1709)
description: Fonctionnalités supprimées ou dont la suppression est prévue dans les prochaines versions.
ms.prod: windows-server-threshold
ms.technology: server-general
ms.topic: article
ms.date: 08/22/2019
author: jasongerend
ms.author: jgerend
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: b4303d05a87fe06e84df0cc55e2c1af8b34047e6
ms.sourcegitcommit: 6f8993e2180c4d3c177e3e1934d378959396b935
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/23/2019
ms.locfileid: "70000632"
---
# <a name="features-removed-or-planned-for-replacement-starting-with-windows-server-version-1709"></a>Fonctionnalités supprimées ou dont le remplacement est prévu à compter de Windows Server, version 1709

>S'applique à : Windows Server, version 1709

Voici la liste des fonctions et fonctionnalités de Windows Server, version 1709 qui ont été supprimées de cette version du produit ou dont la suppression est envisagée dans les prochaines versions. Ce produit s’adresse aux professionnels de l’informatique qui mettent à jour des systèmes d’exploitation dans un environnement commercial. **Cette liste est susceptible d’être modifiée dans des versions ultérieures. Il est donc possible que certaines fonctionnalités concernées n’y figurent plus.** 

> [!TIP]
> - Pour obtenir un accès en avant-première aux builds Windows Server, rejoignez le [programme Windows Insider](https://insider.windows.com) : c’est un excellent moyen de tester les changements apportés aux fonctionnalités.
> - Vous avez des questions sur d’autres versions ? Consultez [Fonctionnalités supprimées ou dont la suppression est prévue dans Windows Server](../get-started-19/removed-features.md).

## <a name="features-removed-from-windows-server-version-1709"></a>Fonctionnalités supprimées de Windows Server, version 1709

Windows Server, version 1709 contient les fonctionnalités déjà présentes dans Windows Server 2016. Toutefois, cette version propose des options d’installation différentes de celles de Windows Server 2016 :

- En tant que version de canal semi-annuel, Windows Server, version 1709 propose uniquement l’option d’installation Server Core. Pour plus d’informations, consultez [Comparaison des canaux de maintenance](../get-started-19/servicing-channels-19.md).
- À compter de cette version, Nano Server n’est plus disponible en tant que système d’exploitation hôte pouvant être installé. Nano Server est désormais disponible en tant que système d’exploitation de conteneur. Voir [Modifications apportées à Nano Server dans Windows Server, version 1709](nano-in-semi-annual-channel.md).
- À compter de cette version, Server Message Block (SMB) version 1 n’est plus installé par défaut. Pour plus d’informations, consultez [SMBv1 n’est pas installé par défaut dans la mise à jour Windows 10 Fall Creators ni le serveur Windows version 1709 et versions plus récentes](https://support.microsoft.com/help/4034314/smbv1-is-not-installed-by-default-in-windows).


## <a name="features-being-considered-for-replacement-starting-with-subsequent-releases"></a>Fonctionnalités dont la suppression est envisagée dans les prochaines versions

La suppression des fonctionnalités suivantes est envisagée dans les versions faisant suite à Windows Server, version 1709. Par la suite, elles pourront être entièrement supprimées de l’image de produit installée et remplacées par d’autres fonctionnalités (ou être installables à partir d’autres sources), mais elles restent dans cette version avec, parfois, des caractéristiques qui ont été supprimées. Nous vous recommandons dès à présent de faire le nécessaire pour trouver une nouvelle solution de remplacement ou d’autres méthodes à employer si vos applications, votre code ou des modes d’utilisation dépendent de ces fonctionnalités.

Si vous voulez nous faire part de commentaires à propos de la proposition de suppression de certaines de ces fonctionnalités, vous pouvez utiliser l’[application Hub de commentaires](https://support.microsoft.com/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub-app). Bien que cette application s’exécute sur Windows 10, vous pouvez également l’utiliser pour nous envoyer vos commentaires sur le produit (et la documentation) Windows Server.

### <a name="iis-6-management-compatibility"></a>Compatibilité avec la gestion IIS 6 :
Les fonctionnalités spécifiques dont la suppression est envisagée sont :

- Compatibilité avec la métabase IIS 6 (Web-Metabase)
- Console de gestion IIS 6 (Web-Lgcy-Mgmt-Console)
- Outils de script IIS 6 (Web-Lgcy-Scripting)
- Compatibilité avec le service WMI IIS 6 (Web-WMI)

Au lieu d’utiliser la compatibilité de métabase IIS 6 (qui sert de couche d’émulation entre les scripts de métabase IIS 6 et la configuration basée sur des fichiers utilisée par IIS 7 ou versions ultérieures), nous vous recommandons de commencer la migration des scripts de gestion directement vers une configuration basée sur des fichiers IIS à l’aide d’outils tels que l’espace de noms Microsoft.Web.Administration.

Vous devez également commencer la migration à partir d’IIS 6.0, ou versions antérieures, vers la dernière version d’IIS, qui est toujours disponible dans la version la plus récente de Windows Server.


### <a name="iis-digest-authentication"></a>Authentification Digest IIS
La suppression de cette méthode d’authentification est prévue. À la place, vous devez commencer à utiliser d’autres méthodes d’authentification telles que le mappage du certificat client (voir [Configuration de l’authentification de mappage de certificats clients un-à-un](https://docs.microsoft.com/iis/manage/configuring-security/configuring-one-to-one-client-certificate-mappings)) ou l’authentification Windows (voir [Paramètres d’application](https://docs.microsoft.com/iis-administration/configuration/appsettings.json)).

### <a name="internet-storage-name-service-isns"></a>Service iSNS (Internet Storage Name Service)
La suppression d’iSNS est envisagée. La fonctionnalité SMB (Server Message Block) propose essentiellement les mêmes fonctionnalités, avec quelques ajouts. Pour obtenir des informations d’ordre général sur cette fonctionnalité, consultez [Protocole SMB (Server Message Block)](https://technet.microsoft.com/library/hh831795(v=ws.11).aspx).

### <a name="rsaaes-encryption-for-iis"></a>Chiffrement RSA/AES pour IIS 
La suppression de cette méthode de chiffrement est envisagée, car la méthode Cryptography API : Next Generation (CNG), plus performante, est déjà disponible. Pour en savoir plus sur le chiffrement CNG, consultez [À propos de CNG](https://msdn.microsoft.com/library/windows/desktop/aa375276(v=vs.85).aspx).

### <a name="windows-powershell-20"></a>Windows PowerShell 2.0
Cette ancienne version de Windows PowerShell a été remplacée par plusieurs versions plus récentes. Pour bénéficier de fonctionnalités et de performances optimales, migrez vers Windows PowerShell 5.0 ou ultérieur. Pour des informations détaillées, consultez la [Documentation PowerShell](https://docs.microsoft.com/powershell/index?view=powershell-5.1).

