---
ms.assetid: c911d6c6-98c6-4532-b1db-5724e1ceb96c
title: Annexe une Administration simplifiée
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: ffc2849fa5e18f7984814d6187cf83d68566409b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71369639"
---
# <a name="simplified-administration-appendix"></a>Annexe une Administration simplifiée

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

  
-   [Boîte de dialogue Gestionnaire de serveur ajouter des serveurs (Active Directory)](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_AddServers)  
  
-   [État du serveur Gestionnaire de serveur distant](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_ServerMgrStatus)  
  
-   [Chargement du module Windows PowerShell](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_PSLoadModule)  
  
-   [Correctifs d’émission RID pour les systèmes d’exploitation précédents](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_Rid)  
  
-   [Installation de Ntdsutil. exe à partir de modifications du support](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_IFM)  
  
## <a name="BKMK_AddServers"></a>Boîte de dialogue Gestionnaire de serveur ajouter des serveurs (Active Directory)  

La boîte de dialogue **Ajouter des serveurs** permet de rechercher des Active Directory pour les serveurs, par système d’exploitation, à l’aide de caractères génériques et par emplacement. La boîte de dialogue autorise également l’utilisation de requêtes DNS par nom de domaine complet ou par nom de préfixe. Ces recherches utilisent des protocoles DNS et LDAP natifs implémentés via .NET, et non Active Directory Windows PowerShell sur la passerelle de gestion Active Directory via SOAP, ce qui signifie que les contrôleurs de domaine contactés par Gestionnaire de serveur peuvent même exécuter Windows Server 2003. Vous pouvez également importer un fichier avec des noms de serveurs à des fins d’approvisionnement.  
  
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
  
## <a name="BKMK_ServerMgrStatus"></a>État du serveur Gestionnaire de serveur distant  
Gestionnaire de serveur teste l’accessibilité du serveur distant à l’aide du protocole de routage d’adresses. Les serveurs qui ne répondent pas aux demandes ARP ne sont pas répertoriés, même s’ils se trouvent dans le pool.  
  
Si le protocole ARP répond, les connexions DCOM et WMI sont établies au serveur pour retourner les informations d’État. Si RPC, DCOM et WMI sont inaccessibles, le gestionnaire de serveur ne peut pas gérer entièrement le serveur.  
  
## <a name="BKMK_PSLoadModule"></a>Chargement du module Windows PowerShell  
Windows PowerShell 3,0 implémente le chargement du module dynamique. L’utilisation de l’applet **de commande Import-Module** n’est généralement plus nécessaire. au lieu de cela, il suffit d’appeler l’applet de commande, l’alias ou la fonction pour charger automatiquement le module.  
  
Pour afficher les modules chargés, utilisez l’applet de commande **obtenir-module** .  
  
```  
Get-Module  
  
```  
  
![Administration simplifiée](media/Simplified-Administration-Appendix/ADDS_PSGetModule.gif)  
  
Pour afficher tous les modules installés avec leurs fonctions et applets de commande exportées, utilisez :  
  
```  
Get-Module -ListAvailable  
  
```  
  
Le cas principal d’utilisation de la commande **import-module** est lorsque vous avez besoin d’accéder au « AD : » Le lecteur virtuel Windows PowerShell et rien d’autre n’a déjà chargé le module. Par exemple, à l’aide des commandes suivantes :  
  
```  
import-module activedirectory  
cd ad:  
dir  
  
```  
  
## <a name="BKMK_Rid"></a>Correctifs d’émission RID pour les systèmes d’exploitation précédents  
Voir [une mise à jour est disponible pour détecter et empêcher une consommation excessive du pool RID global sur un contrôleur de domaine qui exécute Windows Server 2008 R2](https://support.microsoft.com/kb/2618669).  
  
## <a name="BKMK_IFM"></a>Installation de Ntdsutil. exe à partir de modifications du support  
Windows Server 2012 ajoute deux options supplémentaires à l’outil en ligne de commande Ntdsutil. exe pour le menu **IFM (création de supports IFM)** . Elles vous permettent de créer des magasins IFM sans effectuer au préalable une défragmentation hors connexion du NTDS exporté. Fichier de base de données DIT. Lorsque l’espace disque n’est pas une version Premium, cela permet de gagner du temps lors de la création de l’IFM.  
  
Le tableau suivant décrit les deux nouveaux éléments de menu :  
  
|||  
|-|-|  
|Élément de menu|Explication|  
|Créer une NoFragmentation complète% s|Créer un support IFM sans défragmentation pour un contrôleur de domaine Active Directory complet ou une instance AD/LDS dans le dossier% s|  
|Créer la sauvegarde complète SYSVOL% s|Créer un support IFM avec SYSVOL et sans défragmentation pour un contrôleur de domaine Active Directory complet dans le dossier% s|  
  
![Administration simplifiée](media/Simplified-Administration-Appendix/ADDS_PSIFM.png)  
  
![Administration simplifiée](media/Simplified-Administration-Appendix/ADDS_PSIFMComplete.gif)  
  


