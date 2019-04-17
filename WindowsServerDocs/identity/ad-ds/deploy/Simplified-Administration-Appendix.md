---
ms.assetid: c911d6c6-98c6-4532-b1db-5724e1ceb96c
title: "Annexe: Administration simplifiée"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 5de7431d0f3fe9a078432b11a63ce996d3abe447
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="simplified-administration-appendix"></a>Annexe: Administration simplifiée

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

  
-   [Le Gestionnaire de serveur ajouter la boîte de dialogue serveurs (ActiveDirectory)](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_AddServers)  
  
-   [État du Gestionnaire de serveur à distance serveur](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_ServerMgrStatus)  
  
-   [Chargement du Module PowerShell de Windows](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_PSLoadModule)  
  
-   [RID d’émission correctifs pour les systèmes d’exploitation précédents](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_Rid)  
  
-   [Ntdsutil.exe installer from Media Changes](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_IFM)  
  
## <a name="BKMK_AddServers"></a>Le Gestionnaire de serveur ajouter la boîte de dialogue serveurs (ActiveDirectory)  

Le **ajouter des serveurs** boîte de dialogue permet de recherche dans ActiveDirectory pour les serveurs, par le système d’exploitation, à l’aide de caractères génériques et par emplacement. La boîte de dialogue permet également à l’aide de requêtes DNS par nom de domaine complet ou le préfixe du nom. Ces recherches utilisent des protocoles DNS et LDAP natifs implémentées par le biais de .NET, pas ActiveDirectory Windows PowerShell par rapport à la passerelle de gestion ActiveDirectory par le biais de SOAP - ce qui signifie que les contrôleurs de domaine contactés par le Gestionnaire de serveur peuvent même exécuter Windows Server2003. Vous pouvez également importer un fichier avec des noms de serveur pour la configuration à des fins.  
  
La recherche ActiveDirectory utilise les filtres LDAP suivants:  
  
```  
(&(ObjectCategory=computer)  
  
(&(ObjectCategory=computer)(cn=dc*)(OperatingSystemVersion=6.2*))  
  
(&(ObjectCategory=computer)(OperatingSystemVersion=6.1*))  
  
(&(ObjectCategory=computer)(OperatingSystemVersion=6.0*))  
  
(&(ObjectCategory=computer)(|(OperatingSystemVersion=5.2*)(OperatingSystemVersion=5.1*)))  
  
```  
  
La recherche ActiveDirectory renvoie les attributs suivants:  
  
```  
( dnsHostName )( operatingSystem )( cn )  
  
```  
  
## <a name="BKMK_ServerMgrStatus"></a>État du Gestionnaire de serveur à distance serveur  
Le Gestionnaire de serveur teste l’accessibilité de serveur distant à l’aide du protocole de routage d’adresse. Tous les serveurs ne répond ne pas aux requêtes ARP ne sont pas répertoriés, même s’ils sont dans le pool.  
  
Si ARP répond, DCOM et WMI connexions sont apportées au serveur pour renvoyer les informations d’état. Si les RPC, DCOM et WMI sont inaccessibles, le Gestionnaire de serveur ne peut pas gérer entièrement le serveur.  
  
## <a name="BKMK_PSLoadModule"></a>Chargement du Module PowerShell de Windows  
WindowsPowerShell3.0 implémente un module dynamique du chargement. À l’aide de la **Import-Module** applet de commande généralement n’est plus nécessaire; au lieu de cela, simplement l’appel de l’applet de commande, alias ou fonction automatiquement charge le module.  
  
Pour afficher les modules chargés, utilisez le **Get-Module** applet de commande.  
  
```  
Get-Module  
  
```  
  
![Administration simplifiée](media/Simplified-Administration-Appendix/ADDS_PSGetModule.gif)  
  
Pour afficher tous les modules installés avec leurs fonctions exportées et les applets de commande, utilisez:  
  
```  
Get-Module -ListAvailable  
  
```  
  
Le cas principal pour l’utilisation de la **import-module** commande est lorsque vous devez accéder à la» AD: «lecteur virtuel de Windows PowerShell et rien d’autre a déjà chargé le module. Par exemple, en utilisant les commandes suivantes:  
  
```  
import-module activedirectory  
cd ad:  
dir  
  
```  
  
## <a name="BKMK_Rid"></a>RID d’émission correctifs pour les systèmes d’exploitation précédents  
Voir [une mise à jour est disponible pour détecter et empêcher trop la consommation du pool RID global sur un contrôleur de domaine qui exécute Windows Server2008R2](https://support.microsoft.com/kb/2618669).  
  
## <a name="BKMK_IFM"></a>Ntdsutil.exe Install from Media Changes  
Windows Server2012 ajoute deux options à l’outil de ligne de commande Ntdsutil.exe pour le **IFM (création de supports IFM)** menu. Ces permettent de créer des magasins d’IFM sans effectuer au préalable une défragmentation hors connexion de l’exportation NTDS.DIT de base de données de fichier. Lorsque l’espace disque n’est pas un premium, cela fait gagner du temps créer l’IFM.  
  
Le tableau suivant décrit les deux nouveaux éléments de menu:  
  
|||  
|-|-|  
|Élément de menu|Explication|  
|Créer NoDefrag complète %s|Créer des supports IFM sans défragmentation pour un contrôleur de domaine ActiveDirectory complète ou une instance AD/ADLDS dans le dossier %s|  
|Créer plein de Sysvol NoDefrag %s|Créer des supports IFM avec SYSVOL et sans défragmentation pour un contrôleur de domaine ActiveDirectory complète dans le dossier %s|  
  
![Administration simplifiée](media/Simplified-Administration-Appendix/ADDS_PSIFM.png)  
  
![Administration simplifiée](media/Simplified-Administration-Appendix/ADDS_PSIFMComplete.gif)  
  


