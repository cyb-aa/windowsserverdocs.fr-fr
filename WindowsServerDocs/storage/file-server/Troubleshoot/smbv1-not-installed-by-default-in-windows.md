---
title: SMBv1 n’est pas installé par défaut dans Windows 10 version 1709, Windows Server version 1709 et versions ultérieures
description: Décrit le comportement du protocole SMBv1 dans Windows 10 automne Creators Update et Windows Server, version 1709 et versions ultérieures.
author: Deland-Han
manager: dcscontentpm
audience: ITPro
ms.topic: article
ms.author: delhan
ms.date: 12/25/2019
ms.openlocfilehash: 9a6e9778e0ce1e50a70e68832390321fb2d9f971
ms.sourcegitcommit: 8cf04db0bc44fd98f4321dca334e38c6573fae6c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/03/2020
ms.locfileid: "75654360"
---
# <a name="smbv1-is-not-installed-by-default-in-windows-10-version-1709-windows-server-version-1709-and-later-versions"></a>SMBv1 n’est pas installé par défaut dans Windows 10 version 1709, Windows Server version 1709 et versions ultérieures

## <a name="summary"></a>Récapitulatif

Dans Windows 10 automne Creators Update et Windows Server, version 1709 (RS3) et versions ultérieures, le protocole réseau SMBv1 (Server Message Block version 1) n’est plus installé par défaut. Il a été remplacé par SMBv2 et les protocoles ultérieurs à partir de 2007. Microsoft a déconseillé au public le protocole SMBv1 dans 2014. 

SMBv1 a le comportement suivant dans Windows 10 automne Creators Update et Windows Server, version 1709 (RS3) : 
 
- SMBv1 dispose maintenant de sous-fonctionnalités client et serveur qui peuvent être désinstallées séparément.    
- Windows 10 entreprise et Windows 10 éducation ne contiennent plus le client ou le serveur SMBv1 par défaut après une nouvelle installation.    
- Windows Server 2016 ne contient plus le client ou le serveur SMBv1 par défaut après une nouvelle installation.    
- Windows 10 famille et Windows 10 professionnel ne contiennent plus le serveur SMBv1 par défaut après une nouvelle installation.    
- Windows 10 famille et Windows 10 professionnel contiennent toujours le client SMBv1 par défaut après une nouvelle installation. Si le client SMBv1 n’est pas utilisé pendant 15 jours au total (à l’exception de l’ordinateur en cours de mise hors tension), il se désinstalle automatiquement.    
- Les mises à niveau sur place et les vols internes de Windows 10 famille et Windows 10 professionnel ne suppriment pas automatiquement les SMBv1. Si le client ou le serveur SMBv1 n’est pas utilisé pendant 15 jours au total (à l’exception de la durée pendant laquelle l’ordinateur est désactivé), chacun se désinstalle automatiquement.     
- Les mises à niveau sur place et les vols internes des éditions Windows 10 entreprise et Windows 10 Education ne suppriment pas automatiquement SMBv1. Un administrateur doit décider de désinstaller SMBv1 dans ces environnements gérés. Dans Windows 10, version 1809 (RS5) et versions ultérieures, un administrateur peut activer la suppression automatique de SMBv1 en activant la fonctionnalité « suppression automatique SMB 1.0/CIFS ».    
- La suppression automatique de SMBv1 après 15 jours est une opération unique. Si un administrateur réinstalle SMBv1, aucune autre tentative ne sera effectuée pour le désinstaller.
- Les fonctionnalités SMB version 2,02, 2,1, 3,0, 3,02 et 3.1.1 sont toujours entièrement prises en charge et incluses par défaut dans le cadre des binaires SMBv2.    
- Étant donné que le service Explorateur d’ordinateurs s’appuie sur SMBv1, le service est désinstallé si le client ou le serveur SMBv1 est désinstallé. Cela signifie que l’Explorateur networkcan n’affiche plus les ordinateurs Windows via la méthode d’exploration des datagrammes NetBIOS héritée.    
- SMBv1 peut toujours être réinstallé dans toutes les éditions de Windows 10 et Windows Server 2016.    
 
  > [!NOTE]
  > Windows 10, version 1803 (RS4) Professional gère SMBv1 de la même manière que Windows 10, version 1703 (RS2) et Windows 10, version 1607 (RS1). Ce problème a été résolu dans Windows 10, version 1809 (RS5). Vous pouvez toujours désinstaller SMBv1 manuellement. Toutefois, Windows ne désinstalle pas automatiquement SMBv1 après 15 jours dans les scénarios suivants : 
 
-  Vous effectuez une nouvelle installation de Windows 10, version 1803.     
-  Vous mettez à niveau Windows 10, version 1607 ou Windows 10, version 1703 vers Windows 10, version 1803, sans passer par la première mise à niveau vers Windows 10, version 1709.     
 
Si vous essayez de vous connecter à des appareils qui prennent uniquement en charge SMBv1, ou si ces appareils essaient de se connecter à vous, vous pouvez recevoir l’un des messages d’erreur suivants :     

```
You can't connect to the file share because it's not secure. This share requires the obsolete SMB1 protocol, which is unsafe and could expose your system to attack.
Your system requires SMB2 or higher. For more info on resolving this issue, see: https://go.microsoft.com/fwlink/?linkid=852747  
```

```
The specified network name is no longer available.
```

```
Unspecified error 0x80004005
```

```
System Error 64
```

```
The specified server cannot perform the requested operation.
```

```
Error 58
```    

Les événements suivants apparaissent lorsqu’un serveur distant nécessitait une connexion SMBv1 à partir de ce client, mais que SMBv1 est désinstallé ou désactivé sur le client.

```
Log Name:      Microsoft-Windows-SmbClient/Security
 Source:        Microsoft-Windows-SMBClient
 Date:          Date/Time
 Event ID:      32002
 Task Category: None
 Level:         Info
 Keywords:      (128)
 User:          NETWORK SERVICE
 Computer:      junkle.contoso.com
 Description:
 The local computer received an SMB1 negotiate response. 

Dialect: 
SecurityMode 
Server name: 

Guidance: 
SMB1 is deprecated and should not be installed nor enabled. For more information, see https://go.microsoft.com/fwlink/?linkid=852747.
```

```
Log Name:      Microsoft-Windows-SmbClient/Security
 Source:        Microsoft-Windows-SMBClient
 Date:          Date/Time
 Event ID:      32000
 Task Category: None
 Level:         Info
 Keywords:      (128)
 User:          NETWORK SERVICE
 Computer:      junkle.contoso.com
 Description: 
SMB1 negotiate response received from remote device when SMB1 cannot be negotiated by the local computer. 
Dialect: 
Server name: 

Guidance: 
The client has SMB1 disabled or uninstalled. For more information: https://go.microsoft.com/fwlink/?linkid=852747.     
```

Il est probable que ces appareils exécutent Windows. Il est plus probable qu’ils exécutent des versions antérieures de Linux, Samba ou d’autres types de logiciels tiers pour fournir des services SMB. Souvent, ces versions de Linux et Samba ne sont plus prises en charge. 

> [!NOTE]
> Windows 10, version 1709, est également appelé « automne Creators Update ».   

## <a name="more-information"></a>Plus d’informations

Pour contourner ce problème, contactez le fabricant du produit qui prend en charge uniquement SMBv1 et demandez une mise à jour logicielle ou de microprogramme qui prend en charge SMBv 2.02 ou une version ultérieure. Pour obtenir la liste actuelle des fournisseurs connus et leurs exigences SMBv1, consultez l’article suivant du blog de l’équipe d’ingénieurs du stockage Windows et Windows Server : 

[SMBv1 produit Clearinghouse](https://techcommunity.microsoft.com/t5/Storage-at-Microsoft/SMB1-Product-Clearinghouse/ba-p/426008) 
#### <a name="leasing-mode"></a>Mode de Leasing

Si SMBv1 est requis pour assurer la compatibilité des applications pour le comportement des logiciels hérités, par exemple pour la nécessité de désactiver oplocks, Windows fournit un nouvel indicateur de partage SMB appelé mode de bail. Cet indicateur spécifie si un partage désactive la sémantique SMB moderne, comme les baux et les oplocks.

Vous pouvez spécifier un partage sans utiliser oplocks ou leasing pour permettre à une application héritée de fonctionner avec SMBv2 ou une version ultérieure. Pour ce faire, utilisez les applets de commande PowerShell **New-SmbShare** ou **Set-SmbShare** avec le paramètre **-LeasingMode None** .

> [!NOTE]
> Vous devez utiliser cette option uniquement sur les partages requis par une application tierce pour la prise en charge héritée si le fournisseur stipule qu’il est nécessaire. Ne spécifiez pas le mode de bail sur des partages de données utilisateur ou des partages de l’autorité de certification utilisés par les serveurs de fichiers avec montée en puissance parallèle. Cela est dû au fait que la suppression des verrous optionnels et des baux entraîne une instabilité et une altération des données dans la plupart des applications. Le mode Leasing fonctionne uniquement en mode partage. Il peut être utilisé par n’importe quel système d’exploitation client.

#### <a name="explorer-network-browsing"></a>Exploration réseau de l’Explorateur

Le service Explorateur d’ordinateurs s’appuie sur le protocole SMBv1 pour remplir le nœud réseau de l’Explorateur Windows (également appelé « voisinage réseau »). Ce protocole hérité est obsolète, n’est pas routé et sa sécurité est limitée. Étant donné que le service ne peut pas fonctionner sans SMBv1, il est supprimé en même temps.

Toutefois, si vous devez toujours utiliser l’Explorateur Entrée réseau les environnements de groupe de travail de bureau à distance pour localiser des ordinateurs Windows, vous pouvez suivre ces étapes sur vos ordinateurs Windows qui n’utilisent plus SMBv1 : 
 
1. Démarrez les services « hôte du fournisseur de découverte de fonctions » et « publication des ressources de découverte de fonctions », puis définissez-les sur **automatique (début différé)** .

2. Lorsque vous ouvrez le réseau de l’Explorateur, activez la découverte du réseau lorsque vous y êtes invité.    
 
Tous les appareils Windows au sein de ce sous-réseau qui ont ces paramètres s’affichent désormais dans le réseau pour la navigation. Cela utilise le protocole WS-DISCOVERY. Contactez vos fournisseurs et fabricants si leurs appareils n’apparaissent toujours pas dans cette liste de parcours après l’apparition des appareils Windows. Il est possible que ce protocole soit désactivé ou qu’il ne prenne en charge que SMBv1.

> [!NOTE]
> nous vous recommandons de mapper les lecteurs et imprimantes au lieu d’activer cette fonctionnalité, ce qui nécessite toujours la recherche et la navigation sur leurs appareils. Les ressources mappées sont plus faciles à localiser, nécessitent moins de formation et sont plus sûres à utiliser. Cela est particulièrement vrai si ces ressources sont fournies automatiquement via stratégie de groupe. Un administrateur peut configurer des imprimantes pour l’emplacement à l’aide de méthodes autres que le service Explorateur d’ordinateurs hérité en utilisant des adresses IP, Active Directory Domain Services (AD DS), Bonjour, mDNS, uPnP, et ainsi de suite.

Si vous ne pouvez pas utiliser ces solutions de contournement ou si le fabricant de l’application ne peut pas fournir de versions prises en charge de SMB, vous pouvez réactiver manuellement SMBv1 en suivant les étapes décrites dans [Comment détecter, activer et désactiver SMBv1, SMBv2 et SMBv3 dans Windows](detect-enable-and-disable-smbv1-v2-v3.md).

> [!IMPORTANT]
> Nous vous recommandons vivement de ne pas réinstaller SMBv1. Cela est dû au fait que cet ancien protocole rencontre des problèmes de sécurité connus concernant les ransomware et les autres programmes malveillants.   

## <a name="references"></a>Références

[Arrêter d’utiliser SMB1](https://aka.ms/stopusingsmb1)