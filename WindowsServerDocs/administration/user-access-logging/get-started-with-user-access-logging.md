---
title: Prise en main utilisateur journalisation des accès
desctription: Describes the User Access Logging feature and how to start using it.
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: manage-user-access-logging
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5c395b8b-3b35-4042-b9cc-07e438f86d50
author: brentfor
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8656bf278519b48f8d26008fd98e46428106e511
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861500"
---
# <a name="get-started-with-user-access-logging"></a>Prise en main utilisateur journalisation des accès

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

(Journalisation des accès utilisateur) est la fonctionnalité de Windows Server qui agrège les données d’utilisation client par rôle et des produits sur un serveur local. Il aide les administrateurs de Windows server à quantifier les demandes des ordinateurs clients pour les rôles et services sur un serveur local.  
  
Journalisation des accès utilisateur sont installé et activé par défaut et collecte les données dans quasiment en temps réel. Aucune configuration n’est nécessaire pour l’administrateur. La journalisation des accès utilisateur peut toutefois être désactivée ou activée. Pour plus d'informations, voir [Manage User Access Logging](Manage-User-Access-Logging.md). Le service de journalisation des accès utilisateur regroupe les données d’utilisation client par rôle et par produit dans les fichiers de base de données locale.  Les administrateurs informatiques peuvent ensuite utiliser les applets de commande WMI et Windows PowerShell pour récupérer les quantités et les instances par rôle de serveur (ou produit logiciel), par utilisateur, par périphérique, par serveur local et par date.  
  
> [!NOTE]  
> La journalisation des accès utilisateur prend en charge [Microsoft Assessment and Planning Toolkit](https://go.microsoft.com/fwlink/?LinkID=111000)  
  
## <a name="BKMK_APP"></a>Applications pratiques  
Journalisation des accès utilisateur regroupe les clients uniques APPAREIL et utilisateur demande événements qui sont enregistrés dans une base de données locale. Ces enregistrements sont accessibles (via une requête d’un administrateur de serveur) pour récupérer les quantités et les instances par rôle de serveur, par utilisateur, par périphérique, par serveur local et par date.  En outre, la journalisation des accès utilisateur a été étendu pour permettre aux développeurs de logiciels non Microsoft d’instrumenter les événements de journalisation des accès utilisateur doivent être regroupés par Windows Server.  
  
Journalisation des accès utilisateur peuvent effectuer les tâches suivantes :  
  
-   quantifier les demandes d’utilisateurs clients pour les serveurs virtuels ou physiques locaux ;  
  
-   quantifier les demandes d’utilisateurs clients pour les produits logiciels installés sur un ordinateur physique local ou un ordinateur virtuel ;  
  
-   récupérer des données sur un serveur local exécutant Hyper-V pour identifier les périodes de faible et de forte demande sur un ordinateur virtuel Hyper-V ;  
  
-   récupérer des données de journalisation des accès utilisateur à partir de plusieurs serveurs distants.  
  
En outre, les développeurs de logiciels peuvent instrumenter des événements de journalisation des accès utilisateur qui peuvent être agrégées et récupérées à l’aide des interfaces WMI et Windows PowerShell.  
  
Les rôles de serveur et services suivants sont pris en charge par la journalisation des accès utilisateur :  
  
-   Services de certificats Active Directory (AD CS)  
  
-   Services AD RMS (Active Directory Rights Management Services)  
  
-   BranchCache  
  
-   DNS (Domain Name System)  
  
    > [!NOTE]  
    > La journalisation des accès utilisateur collecte des données DNS toutes les 24 heures. Il existe également une autre applet de commande de journalisation des accès utilisateur pour ce scénario.  
  
-   Protocole DHCP (Dynamic Host Configuration Protocol)  
  
-   Serveur de télécopie  
  
-   Services de fichiers  
  
-   Serveur FTP  
  
-   Hyper-V  
  
    > [!NOTE]  
    > La journalisation des accès utilisateur collecte des données Hyper-V toutes les 24 heures. Il existe également une autre applet de commande de journalisation des accès utilisateur pour ce scénario.  
  
-   Serveur Web (IIS)  
  
    > [!WARNING]  
    > Pour utiliser la journalisation des accès utilisateur avec IIS, vous devez utiliser iisual.exe. Pour plus d’informations, voir [Analyse des données d’utilisation client avec la journalisation des accès utilisateur IIS](http://www.iis.net/learn/manage/configuring-security/analyzing-client-usage-data-with-iis-user-access-logging).  
  
-   Services MSMQ (Microsoft Message Queue)  
  
-   Services de stratégie et d'accès réseau  
  
-   Services d'impression et de numérisation de document  
  
-   Service Routage et accès à distance  
  
-   Services de déploiement Windows (WDS)  
  
-   Windows Server Update Services (WSUS)  
  
> [!IMPORTANT]  
> La journalisation des accès utilisateur n’est pas recommandée sur les serveurs connectés directement à Internet, tels que les serveurs web sur un espace d’adressage accessible via Internet. Elle est également déconseillée dans les scénarios où des performances extrêmement élevées constituent l’objectif principal du serveur (par exemple, dans les environnements de charge de travail HPC). Journalisation des accès utilisateur sont principalement destinée aux petites, moyennes et scénarios intranet d’entreprise où un volume élevé est attendu, mais pas aussi élevé que les déploiements qui servent de volume de trafic sur Internet de manière régulière.  
  
## <a name="BKMK_NEW"></a>Fonctionnalités importantes  
Le tableau suivant décrit les fonctionnalités principales de la journalisation des accès utilisateur et leur valeur potentielle.  
  
|Fonctionnalité|Value|  
|-----------------|---------|  
|Collecter et regrouper les données d’événements de demandes client quasiment en temps réel|L’équivalent de trois ans de données peut être enregistré. **Important :** Les administrateurs doivent s’assurer que les données collectées et les périodes de rétention des données sont conformes à la politique de confidentialité de l’entreprise et aux réglementations locales.|  
|Utiliser des requêtes de journalisation des accès utilisateur via l’interface WMI ou Windows PowerShell pour récupérer les données de demande client sur un serveur local ou distant|La journalisation des accès utilisateur permet d’afficher l’ensemble des données d’utilisation actuelles. Les administrateurs de serveur et d’entreprise peuvent récupérer ces données et collaborer avec les administrateurs des activités pour optimiser l’utilisation de leurs licences en volume.|  
|Activé par défaut.|Les administrateurs de serveur n’ont pas besoin de configurer cette fonctionnalité pour que toutes les fonctionnalités principales soient disponibles et opérationnelles.|  
  
## <a name="data-logged-with-ual"></a>Données consignées à l’aide de la journalisation des accès utilisateur  
Les données liées aux utilisateurs suivantes sont consignées à l’aide de la journalisation des accès utilisateur.  
  
|Données|Description|  
|--------|---------------|  
|**UserName**|Nom d’utilisateur du client qui accompagne les entrées de journalisation des accès utilisateur provenant des rôles et produits installés, le cas échéant.|  
|**ActivityCount**|Nombre de fois qu’un utilisateur accède à un rôle ou un service.|  
|**FirstSeen**|Date et heure auxquelles un utilisateur accède pour la première fois à un rôle ou un service.|  
|**LastSeen**|Date et heure auxquelles un utilisateur accède pour la dernière fois à un rôle ou un service.|  
|**ProductName**|Nom du produit parent logiciel (par exemple, Windows) qui fournit les données de journalisation des accès utilisateur.|  
|**RoleGUID**|GUID affecté ou inscrit par la journalisation des accès utilisateur, et qui représente le rôle de serveur ou le produit installé.|  
|**RoleName**|Nom du rôle, composant ou sous-produit qui fournit les données de journalisation des accès utilisateur. Ce nom est également associé à une valeur ProductName et à une valeur RoleGUID.|  
|**TenantIdentifier**|GUID unique du client d’un rôle ou d’un produit installé qui accompagne les données de journalisation des accès utilisateur, le cas échéant.|  
  
Les données liées aux périphériques suivantes sont consignées à l’aide de la journalisation des accès utilisateur.  
  
|Données|Description|  
|--------|---------------|  
|**IPAddress**|Adresse IP d’un périphérique client qui est utilisé pour accéder à un rôle ou un service.|  
|**ActivityCount**|Nombre de fois qu’un utilisateur accède à un rôle ou un service.|  
|**FirstSeen**|Date et heure auxquelles une adresse IP est utilisée pour la première fois pour accéder à un rôle ou un service.|  
|**LastSeen**|Date et heure auxquelles une adresse IP est utilisée pour la dernière fois pour accéder à un rôle ou un service.|  
|**ProductName**|Nom du produit parent logiciel (par exemple, Windows) qui fournit les données de journalisation des accès utilisateur.|  
|**RoleGUID**|GUID affecté ou inscrit par la journalisation des accès utilisateur, et qui représente le rôle de serveur ou le produit installé.|  
|**RoleName**|Nom du rôle, composant ou sous-produit qui fournit les données de journalisation des accès utilisateur. Ce nom est également associé à une valeur ProductName et à une valeur RoleGUID.|  
|**TenantIdentifier**|GUID unique du client d’un rôle ou d’un produit installé qui accompagne les données de journalisation des accès utilisateur, le cas échéant.|  
  
## <a name="BKMK_SOFT"></a>Configuration logicielle requise  
Journalisation des accès utilisateur peuvent être utilisé sur n’importe quel ordinateur exécutant des versions de Windows Server depuis Windows Server 2012.  
  
## <a name="see-also"></a>Voir aussi  
[Journalisation des accès utilisateur](https://msdn.microsoft.com/library/windows/desktop/hh437528(v=vs.85).aspx) sur MSDN.  
[Gérer l’utilisateur journalisation des accès](Manage-User-Access-Logging.md)  
  

