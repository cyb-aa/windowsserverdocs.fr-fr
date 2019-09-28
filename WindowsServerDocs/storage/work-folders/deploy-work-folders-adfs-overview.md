---
title: Déployer Dossiers de travail avec AD FS et vue d’ensemble du proxy d’application Web
ms.prod: windows-server
ms.technology: storage-work-folders
ms.topic: article
ms.assetid: ea19f0f0-6cc0-4322-b387-c0873f7795ad
manager: klaasl
ms.author: jeffpatt
author: JeffPatt24
ms.date: 4/5/2017
ms.openlocfilehash: 40cc953ce7393781497d957fc8e6690c5c9abc0b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71365924"
---
# <a name="deploy-work-folders-with-ad-fs-and-web-application-proxy-overview"></a>Déployer des dossiers de travail avec AD FS et le proxy d’application Web : Vue d'ensemble

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Les rubriques de cette section fournissent des instructions pour le déploiement de dossiers de travail avec les services de fédération Active Directory (AD FS) et le proxy d’application Web. Les instructions sont conçues pour vous aider à créer une installation complète de dossiers de travail qui fonctionne avec les ordinateurs clients prêts à utiliser des dossiers de travail, en local ou via Internet.  
  
Dossiers de travail est un composant introduit dans Windows Server 2012 R2 qui permet aux employés de synchroniser les fichiers de travail entre leurs appareils. Pour plus d’informations sur les dossiers de travail, voir la [vue d’ensemble des dossiers de travail](Work-Folders-Overview.md).  
  
Pour permettre aux utilisateurs de synchroniser Dossiers de travail sur Internet, vous devez les publier via un proxy inverse, ce qui les rend disponibles en externe sur Internet. Le proxy d’application Web, qui est inclus dans les services AD FS, est une option que vous pouvez utiliser pour fournir la fonctionnalité de proxy inverse. Le proxy d’application Web authentifie au préalable l’accès à l’application web de dossiers de travail à l’aide d’AD FS, afin que les utilisateurs de n’importe quel appareil puissent accéder aux dossiers de travail en dehors du réseau d’entreprise. 

> [!NOTE]
>   Les instructions décrites dans cette section sont adaptées à un environnement Windows Server 2016. Si vous utilisez Windows Server 2012 R2, suivez les [instructions pour Windows Server 2012 R2](https://technet.microsoft.com/library/dn747208(v=ws.11).aspx).
  
Ces rubriques fournissent également les informations suivantes :  
  
-   des instructions pas à pas concernant la configuration et le déploiement de dossiers de travail avec AD FS et le proxy d’application Web via l’interface utilisateur de Windows Server. Les instructions expliquent comment configurer un environnement de test simple avec des certificats auto-signés. Vous pouvez ensuite utiliser l’exemple de test comme un guide pour vous aider à créer un environnement de production qui utilise des certificats approuvés publiquement.  
  
## <a name="prerequisites"></a>Prérequis  
Pour suivre les procédures et les exemples de ces rubriques, les composants suivants doivent être prêts :  
  
-   Une forêt des services de domaine Active Directory avec des extensions de schéma dans Windows Server 2012 R2 pour prendre en charge le référencement automatique des PC et appareils sur le serveur de fichiers approprié lorsque vous utilisez plusieurs serveurs de fichiers. Il est préférable que le DNS soit activé dans la forêt, mais cela n’est pas nécessaire.  
  
-   Un contrôleur de domaine : Un serveur sur lequel le rôle AD DS est activé et est configuré avec un domaine (pour l’exemple de test, contoso.com).  
  
    Un contrôleur de domaine exécutant au moins Windows Server 2012 R2 est nécessaire pour prendre en charge l’inscription de l’appareil à Workplace Join. Si vous ne souhaitez pas utiliser Workplace Join, vous pouvez exécuter Windows Server 2012 sur le contrôleur de domaine.  
  
-   Deux serveurs qui sont joints au domaine (par exemple, contoso.com) et qui exécutent Windows Server 2016. Un seul serveur sera utilisé pour AD FS, et l’autre sera utilisé pour les dossiers de travail.  
  
-   Un serveur de domaine qui n’est pas joint à un domaine et qui exécute Windows Server 2016. Ce serveur exécutera le proxy d’application Web, et il doit avoir une carte réseau pour le domaine de réseau (par exemple, contoso.com) et une autre carte réseau pour le réseau externe.  
  
-   Un ordinateur client joint à un domaine qui exécute Windows 7 ou ultérieur.  
  
-   Un ordinateur client qui n’est pas joint à un domaine qui exécute Windows 7 ou ultérieur.  
  
Pour l’environnement de test que nous couvrons dans ce guide, votre topologie devrait être celle affichée dans le diagramme suivant. Les ordinateurs peuvent être des machines physiques ou virtuelles. 
  
![Diagramme montrant les segments réseau Contoso, le réseau de périmètre (DMZ) et Internet. Dans le segment Internet : CLIENT2 dans la zone DMZ : un serveur WAP ; dans le segment contoso : Serveur dossiers de travail, contrôleur de domaine, serveur AD FS et CLIENT1](media/deploy-work-folders-adfs/WF_ADFS_WAP_Diagram.png)

## <a name="deployment-overview"></a>Vue d’ensemble du déploiement  
Dans ce groupe de rubriques, vous allez étudier un exemple pas à pas de configuration AD FS, du proxy d’application Web et de dossiers de travail dans un environnement de test. Les composants seront configurés dans cet ordre :  
  
1.  Synchronisation complète AD  
  
2.  Dossiers de travail  
  
3.  Proxy d'application web  
  
4.  La station de travail jointe au domaine et station de travail non jointe au domaine  
  
Vous utiliserez également un Script Windows PowerShell pour créer des certificats auto-signés.  
  
## <a name="deployment-steps"></a>Étapes de déploiement  
Pour effectuer le déploiement à l’aide de l’interface utilisateur de Windows Server, suivez les étapes de ces rubriques :  
  
-   [Deploy les dossiers de travail avec AD FS et le proxy d’application Web : Étape 1 : configurer AD FS @ no__t-0  
  
-   [Deploy les dossiers de travail avec AD FS et le proxy d’application Web : Étape 2, AD FS travail postérieur à la configuration @ no__t-0  
  
-   [Deploy les dossiers de travail avec AD FS et le proxy d’application Web : Étape 3 : configurer les dossiers de travail @ no__t-0  
  
-   [Deploy les dossiers de travail avec AD FS et le proxy d’application Web : Étape 4, configurer le proxy d’application Web @ no__t-0  
  
-   [Deploy les dossiers de travail avec AD FS et le proxy d’application Web : Étape 5, configurer les clients @ no__t-0  

## <a name="see-also"></a>Voir aussi  
[Vue d’ensemble des dossiers de travail](Work-Folders-Overview.md)  
[Conception d’une implémentation de Dossiers de travail](Plan-Work-Folders.md)  
[Déploiement de Dossiers de travail](Deploy-Work-Folders.md)  
  

