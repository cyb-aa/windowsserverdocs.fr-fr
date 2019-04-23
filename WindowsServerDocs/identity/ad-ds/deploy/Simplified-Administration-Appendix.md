---
ms.assetid: c911d6c6-98c6-4532-b1db-5724e1ceb96c
title: Annexe une Administration simplifiée
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 36cdacec27e64586c359146b858a9d68750e5026
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858260"
---
# <a name="simplified-administration-appendix"></a>Annexe une Administration simplifiée

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

  
-   [Le Gestionnaire de serveur ajouter des serveurs de boîte de dialogue (Active Directory)](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_AddServers)  
  
-   [État du Gestionnaire de serveur à distance serveur](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_ServerMgrStatus)  
  
-   [Lors du chargement du Module PowerShell de Windows](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_PSLoadModule)  
  
-   [RID de correctifs logiciels d’émission pour les systèmes d’exploitation précédents](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_Rid)  
  
-   [Ntdsutil.exe Install from Media Changes](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_IFM)  
  
## <a name="BKMK_AddServers"></a>Le Gestionnaire de serveur ajouter des serveurs de boîte de dialogue (Active Directory)  

Le **ajouter des serveurs** boîte de dialogue permet la recherche dans Active Directory pour les serveurs, par système d’exploitation, à l’aide des caractères génériques et par emplacement. La boîte de dialogue permet également à l’aide de requêtes DNS par nom de domaine complet ou nom de préfixe. Ces recherches utilisent des protocoles DNS et LDAP natives implémentées via .NET, pas AD Windows PowerShell par rapport à la passerelle de gestion AD via SOAP - ce qui signifie que les contrôleurs de domaine contactés par le Gestionnaire de serveur peuvent même exécuter Windows Server 2003. Vous pouvez également importer un fichier dont le nom de serveur pour l’approvisionnement à des fins.  
  
La recherche Active Directory utilise les filtres LDAP suivants :  
  
```  
(&(ObjectCategory=computer)  
  
(&(ObjectCategory=computer)(cn=dc*)(OperatingSystemVersion=6.2*))  
  
(&(ObjectCategory=computer)(OperatingSystemVersion=6.1*))  
  
(&(ObjectCategory=computer)(OperatingSystemVersion=6.0*))  
  
(&(ObjectCategory=computer)(|(OperatingSystemVersion=5.2*)(OperatingSystemVersion=5.1*)))  
  
```  
  
La recherche Active Directory retourne les attributs suivants :  
  
```  
( dnsHostName )( operatingSystem )( cn )  
  
```  
  
## <a name="BKMK_ServerMgrStatus"></a>État du Gestionnaire de serveur à distance serveur  
Le Gestionnaire de serveur teste l’accessibilité du serveur distant à l’aide du protocole de routage d’adresse. Tous les serveurs ne répond ne pas aux requêtes ARP ne sont pas répertoriés, même si elles se trouvent dans le pool.  
  
Si ARP répond, les connexions DCOM et WMI sont établies sur le serveur pour retourner des informations d’état. Si RPC, DCOM et WMI ne sont pas accessibles, le Gestionnaire de serveur ne peut pas gérer entièrement le serveur.  
  
## <a name="BKMK_PSLoadModule"></a>Lors du chargement du Module PowerShell de Windows  
Windows PowerShell 3.0 implémente le chargement de module dynamique. À l’aide de la **Import-Module** applet de commande en général, n’est plus nécessaire ; au lieu de cela, simplement l’appel de l’applet de commande, alias ou fonction automatiquement charge le module.  
  
Pour afficher les modules chargés, utilisez le **Get-Module** applet de commande.  
  
```  
Get-Module  
  
```  
  
![Administration simplifiée](media/Simplified-Administration-Appendix/ADDS_PSGetModule.gif)  
  
Pour afficher tous les modules installés avec leurs fonctions exportées et les applets de commande, utilisez :  
  
```  
Get-Module -ListAvailable  
  
```  
  
Le principal cas pour l’utilisation de la **import-module** commande est lorsque vous devez accéder à la « AD : » Lecteur virtuel Windows PowerShell et rien d’autre a déjà chargé le module. Par exemple, utilisez les commandes suivantes :  
  
```  
import-module activedirectory  
cd ad:  
dir  
  
```  
  
## <a name="BKMK_Rid"></a>RID de correctifs logiciels d’émission pour les systèmes d’exploitation précédents  
Consultez [une mise à jour est disponible pour détecter et éviter de trop la consommation du pool RID global sur un contrôleur de domaine qui exécute Windows Server 2008 R2](https://support.microsoft.com/kb/2618669).  
  
## <a name="BKMK_IFM"></a>Ntdsutil.exe Install from Media Changes  
Windows Server 2012 ajoute deux options supplémentaires à l’outil de ligne de commande Ntdsutil.exe pour le **IFM (la création du média IFM)** menu. Elles vous permettent de vous permettent de créer des magasins d’IFM sans effectuer au préalable une défragmentation hors connexion du fichier exporté NTDS. Fichier de base de données DIT. Lors de l’espace disque n’est pas une prime, cela fait gagner du temps de création la IFM.  
  
Le tableau suivant décrit les deux nouveaux éléments de menu :  
  
|||  
|-|-|  
|Élément de menu|Explication|  
|Créer NoDefrag complète %s|Créer des supports IFM sans défragmentation pour un contrôleur de domaine AD complet ou une/instance AD LDS dans le dossier %s|  
|Créer Sysvol intégral NoDefrag %s|Créer des supports IFM avec SYSVOL et sans défragmentation pour un contrôleur de domaine complet AD dans le dossier %s|  
  
![Administration simplifiée](media/Simplified-Administration-Appendix/ADDS_PSIFM.png)  
  
![Administration simplifiée](media/Simplified-Administration-Appendix/ADDS_PSIFMComplete.gif)  
  


