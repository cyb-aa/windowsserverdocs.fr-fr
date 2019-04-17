---
title: "Déployer Dossiers de travail avec ADFS et vue d’ensemble du proxy d’application Web"
ms.prod: windows-server-threshold
ms.technology: storage-work-folders
ms.topic: article
ms.assetid: ea19f0f0-6cc0-4322-b387-c0873f7795ad
manager: klaasl
ms.author: jeffpatt
author: JeffPatt24
ms.date: 4/5/2017
ms.openlocfilehash: 48c7d771c7ec75a4bc340608a96410ea388418e9
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/17/2017
---
# <a name="deploy-work-folders-with-ad-fs-and-web-application-proxy-overview"></a>Déployer Dossiers de travail avec ADFS et le proxy d’application Web: vue d’ensemble

>S’applique à: WindowsServer (canal semi-annuel), WindowsServer2016

Les rubriques de cette section fournissent des instructions pour le déploiement de dossiers de travail avec les services de fédération Active Directory (ADFS) et le proxy d’application Web. Les instructions sont conçues pour vous aider à créer une installation complète de dossiers de travail qui fonctionne avec les ordinateurs clients prêts à utiliser des dossiers de travail, en local ou via Internet.  
  
Dossiers de travail est un composant introduit dans Windows Server2012R2 qui permet aux employés de synchroniser les fichiers de travail entre leurs appareils. Pour plus d’informations sur les dossiers de travail, voir la [vue d’ensemble des dossiers de travail](Work-Folders-Overview.md).  
  
Pour permettre aux utilisateurs de synchroniser leurs Dossiers de travail sur Internet, vous devez publier les Dossiers de travail via un proxy inverse, ce qui les rend disponibles en externe sur Internet. Le proxy d’application Web, qui est inclus dans les services ADFS, est une option que vous pouvez utiliser pour fournir la fonctionnalité de proxy inverse. Le proxy d’application Web authentifie au préalable l’accès à l’application web de dossiers de travail à l’aide d’ADFS, afin que les utilisateurs de n’importe quel appareil puissent accéder aux dossiers de travail en dehors du réseau d’entreprise. 

> [!NOTE]
>   Les instructions décrites dans cette section sont adaptées à un environnement Windows Server2016. Si vous utilisez Windows Server2012R2, suivez les [instructions pour Windows Server2012R2](https://technet.microsoft.com/library/dn747208(v=ws.11).aspx).
  
Ces rubriques fournissent également les informations suivantes:  
  
-   des instructions pas à pas concernant la configuration et le déploiement de dossiers de travail avec ADFS et le proxy d’application Web via l’interface utilisateur de Windows Server. Les instructions expliquent comment configurer un environnement de test simple avec des certificats auto-signés. Vous pouvez ensuite utiliser l’exemple de test comme un guide pour vous aider à créer un environnement de production qui utilise des certificats approuvés publiquement.  
  
## <a name="prerequisites"></a>Éléments prérequis  
Pour suivre les procédures et les exemples de ces rubriques, les composants suivants doivent être prêts:  
  
-   Une forêt des services de domaine Active Directory avec des extensions de schéma dans Windows Server2012 R2 pour prendre en charge le référencement automatique des PC et appareils sur le serveur de fichiers approprié lorsque vous utilisez plusieurs serveurs de fichiers. Il est préférable que le DNS soit activé dans la forêt, mais cela n’est pas nécessaire.  
  
-   Un contrôleur de domaine: un serveur dont le rôle ADDS est activé et est configuré avec un domaine (pour l’exemple de test, contoso.com).  
  
    Un contrôleur de domaine exécutant au moins Windows Server2012R2 est nécessaire pour prendre en charge l’inscription de l’appareil à Workplace Join. Si vous ne souhaitez pas utiliser Workplace Join, vous pouvez exécuter Windows Server2012 sur le contrôleur de domaine.  
  
-   Deux serveurs qui sont joints au domaine (par exemple, contoso.com) et qui exécutent Windows Server2016. Un seul serveur sera utilisé pour ADFS, et l’autre sera utilisé pour les dossiers de travail.  
  
-   Un serveur de domaine qui n’est pas joint à un domaine et qui exécute Windows Server2016. Ce serveur exécutera le proxy d’application Web, et il doit avoir une carte réseau pour le domaine de réseau (par exemple, contoso.com) et une autre carte réseau pour le réseau externe.  
  
-   Un ordinateur client joint à un domaine qui exécute Windows7 ou ultérieur.  
  
-   Un ordinateur client qui n’est pas joint à un domaine qui exécute Windows7 ou ultérieur.  
  
Pour l’environnement de test que nous couvrons dans ce guide, votre topologie devrait être celle affichée dans le diagramme suivant. Les ordinateurs peuvent être des machines physiques ou virtuelles. 
  
![Diagramme montrant les segments réseau Contoso, le réseau de périmètre (DMZ) et Internet. Dans le segment Internet: Client2; dans le réseau de périmètre (DMZ): un serveur WAP; dans le segment de Contoso: serveur dossiers de travail, un contrôleur de domaine, un serveur ADFS et Client1](media/deploy-work-folders-adfs/WF_ADFS_WAP_Diagram.png)

## <a name="deployment-overview"></a>Vue d’ensemble du déploiement  
Dans ce groupe de rubriques, vous allez étudier un exemple pas à pas de configuration ADFS, du proxy d’application Web et de dossiers de travail dans un environnement de test. Les composants seront configurés dans cet ordre:  
  
1.  Synchronisation complète AD  
  
2.  Dossiers de travail  
  
3.  Proxy d’application web  
  
4.  La station de travail jointe au domaine et station de travail non jointe au domaine  
  
Vous utiliserez également un Script Windows PowerShell pour créer des certificats auto-signés.  
  
## <a name="deployment-steps"></a>Étapes de déploiement  
Pour effectuer le déploiement à l’aide de l’interface utilisateur de Windows Server, suivez les étapes de ces rubriques:  
  
-   [Déployer Dossiers de travail avec ADFS et le proxy d’application Web: étape1, configurer ADFS](deploy-work-folders-adfs-step1.md)  
  
-   [Déployer Dossiers de travail avec ADFS et le proxy d’application Web: étape2, tâches post-configuration ADFS](deploy-work-folders-adfs-step2.md)  
  
-   [Déployer Dossiers de travail avec ADFS et le proxy d’application Web: étape3, configurer les dossiers de travail](deploy-work-folders-adfs-step3.md)  
  
-   [Déployer Dossiers de travail avec ADFS et le proxy d’application Web: étape4, configurer le proxy d’application Web](deploy-work-folders-adfs-step4.md)  
  
-   [Déployer Dossiers de travail avec ADFS et le proxy d’application Web: étape5, configurer les clients](deploy-work-folders-adfs-step5.md)  

## <a name="see-also"></a>Voir aussi  
[Vue d’ensemble des Dossiers de travail](Work-Folders-Overview.md)  
[Conception d’une implémentation de Dossiers de travail](Plan-Work-Folders.md)  
[Déploiement de Dossiers de travail](Deploy-Work-Folders.md)  
  

