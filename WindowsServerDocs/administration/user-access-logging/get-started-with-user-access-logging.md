---
title: Prise en main de la journalisation des accès utilisateur
desctription: Describes the User Access Logging feature and how to start using it.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: f69a1fe4f3c17123f91ade3b6aebdb5f7bab9982
ms.sourcegitcommit: 3f9bcd188dda12dc5803defb47b2c3a907504255
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/04/2020
ms.locfileid: "77001784"
---
# <a name="get-started-with-user-access-logging"></a>Prise en main de la journalisation des accès utilisateur

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La journalisation des accès utilisateur est une fonctionnalité de Windows Server qui agrège les données d’utilisation du client par rôle et par produit sur un serveur local. Il aide les administrateurs Windows Server à quantifier les requêtes des ordinateurs clients pour les rôles et les services sur un serveur local.  
  
La journalisation des accès ordinateur est installée et activée par défaut, et collecte les données quasiment en temps réel. Aucune configuration n’est nécessaire pour l’administrateur. La journalisation des accès utilisateur peut toutefois être désactivée ou activée. Pour plus d'informations, voir [Manage User Access Logging](Manage-User-Access-Logging.md). Le service de journalisation des accès utilisateur regroupe les données d’utilisation des clients par rôles et produits dans des fichiers de base de données locaux.  Les administrateurs informatiques peuvent ensuite utiliser les applets de commande WMI et Windows PowerShell pour récupérer les quantités et les instances par rôle de serveur (ou produit logiciel), par utilisateur, par périphérique, par serveur local et par date.  
  
> [!NOTE]  
> La journalisation des accès utilisateur prend en charge [Microsoft Assessment and Planning Toolkit](https://go.microsoft.com/fwlink/?LinkID=111000)  
  
## <a name="BKMK_APP"></a>Applications pratiques  
La journalisation des événements globaux rassemble les événements de demande utilisateur et de périphérique client uniques qui sont enregistrés dans une base de données locale. Ces enregistrements sont accessibles (via une requête d’un administrateur de serveur) pour récupérer les quantités et les instances par rôle de serveur, par utilisateur, par périphérique, par serveur local et par date.  De plus, la journalisation des appareils a été étendue pour permettre aux développeurs de logiciels non-Microsoft d’instrumenter leurs événements de journalisation des événements de port pour qu’ils soient agrégés par Windows Server.  
  
La journalisation des exécutions peut effectuer les tâches suivantes :  
  
-   quantifier les demandes d’utilisateurs clients pour les serveurs virtuels ou physiques locaux ;  
  
-   quantifier les demandes d’utilisateurs clients pour les produits logiciels installés sur un ordinateur physique local ou un ordinateur virtuel ;  
  
-   récupérer des données sur un serveur local exécutant Hyper-V pour identifier les périodes de faible et de forte demande sur un ordinateur virtuel Hyper-V ;  
  
-   récupérer des données de journalisation des accès utilisateur à partir de plusieurs serveurs distants.  
  
En outre, les développeurs de logiciels peuvent instrumenter des événements de journalisation des appareils intégrés qui peuvent ensuite être agrégés et récupérés à l’aide des interfaces WMI et Windows PowerShell.  
  
Les rôles de serveur et services suivants sont pris en charge par la journalisation des accès utilisateur :  
  
-   Services de certificats Active Directory (AD CS)  
  
-   Services AD RMS (Active Directory Rights Management Services)  
  
-   BranchCache  
  
-   DNS (Domain Name System)  
  
    > [!NOTE]  
    > La journalisation des accès utilisateur collecte des données DNS toutes les 24 heures. Il existe également une autre applet de commande de journalisation des accès utilisateur pour ce scénario.  
  
-   Dynamic Host Configuration Protocol (DHCP)  
  
-   Serveur de télécopie  
  
-   Services de fichiers  
  
-   Serveur FTP  
  
-   Hyper-V  
  
    > [!NOTE]  
    > La journalisation des accès utilisateur collecte des données Hyper-V toutes les 24 heures. Il existe également une autre applet de commande de journalisation des accès utilisateur pour ce scénario.  
  
-   Serveur Web (IIS)  
  
    > [!WARNING]  
    > Pour utiliser la journalisation des accès utilisateur avec IIS, vous devez utiliser iisual.exe. Pour plus d’informations, voir [Analyse des données d’utilisation client avec la journalisation des accès utilisateur IIS](https://www.iis.net/learn/manage/configuring-security/analyzing-client-usage-data-with-iis-user-access-logging).  
  
-   Services MSMQ (Microsoft Message Queue)  
  
-   Network Policy and Access Services  
  
-   Print and Document Services  
  
-   Service Routage et accès à distance  
  
-   Services de déploiement Windows (WDS)  
  
-   Windows Server Update Services (WSUS)  
  
> [!IMPORTANT]  
> La journalisation des accès utilisateur n’est pas recommandée sur les serveurs connectés directement à Internet, tels que les serveurs web sur un espace d’adressage accessible via Internet. Elle est également déconseillée dans les scénarios où des performances extrêmement élevées constituent l’objectif principal du serveur (par exemple, dans les environnements de charge de travail HPC). La journalisation des accès aux entreprises est principalement destinée aux scénarios d’intranet de petite, moyenne et entreprise où un volume élevé est attendu, mais pas aussi élevé que les déploiements qui servent le volume de trafic Internet régulièrement.  
  
## <a name="BKMK_NEW"></a>Fonctionnalités importantes  
Le tableau suivant décrit les fonctionnalités principales de la journalisation des accès utilisateur et leur valeur potentielle.  
  
|Fonctionnalités|Value|  
|-----------------|---------|  
|Collecter et regrouper les données d’événements de demandes client quasiment en temps réel|L’équivalent de trois ans de données peut être enregistré. **Important :** Les administrateurs doivent mettre en œuvre la conformité des données collectées et des périodes de rétention des données avec la politique de confidentialité et les réglementations locales de l’organisation.|  
|Utiliser des requêtes de journalisation des accès utilisateur via l’interface WMI ou Windows PowerShell pour récupérer les données de demande client sur un serveur local ou distant|La journalisation des accès utilisateur permet d’afficher l’ensemble des données d’utilisation actuelles. Les administrateurs de serveur et d’entreprise peuvent récupérer ces données et collaborer avec les administrateurs des activités pour optimiser l’utilisation de leurs licences en volume.|  
|Activé par défaut.|Les administrateurs de serveur n’ont pas besoin de configurer cette fonctionnalité pour que toutes les fonctionnalités principales soient disponibles et opérationnelles.|  
  
## <a name="data-logged-with-ual"></a>Données consignées à l’aide de la journalisation des accès utilisateur  
Les données liées aux utilisateurs suivantes sont consignées à l’aide de la journalisation des accès utilisateur.  
  
|Données|Description|  
|--------|---------------|  
|**Nom d’utilisateur**|Nom d’utilisateur du client qui accompagne les entrées de journalisation des accès utilisateur provenant des rôles et produits installés, le cas échéant.|  
|**ActivityCount**|Nombre de fois qu’un utilisateur accède à un rôle ou un service.|  
|**FirstSeen**|Date et heure auxquelles un utilisateur accède pour la première fois à un rôle ou un service.|  
|**LastSeen**|Date et heure auxquelles un utilisateur accède pour la dernière fois à un rôle ou un service.|  
|**ProductName**|Nom du produit parent logiciel (par exemple, Windows) qui fournit les données de journalisation des accès utilisateur.|  
|**Valeur roleguid**|GUID affecté ou inscrit par la journalisation des accès utilisateur, et qui représente le rôle de serveur ou le produit installé.|  
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
|**Valeur roleguid**|GUID affecté ou inscrit par la journalisation des accès utilisateur, et qui représente le rôle de serveur ou le produit installé.|  
|**RoleName**|Nom du rôle, composant ou sous-produit qui fournit les données de journalisation des accès utilisateur. Ce nom est également associé à une valeur ProductName et à une valeur RoleGUID.|  
|**TenantIdentifier**|GUID unique du client d’un rôle ou d’un produit installé qui accompagne les données de journalisation des accès utilisateur, le cas échéant.|  
  
## <a name="BKMK_SOFT"></a>Configuration logicielle requise  
La journalisation des ordinateurs Windows peut être utilisée sur n’importe quel ordinateur exécutant une version de Windows Server postérieure à Windows Server 2012.  
  
## <a name="see-also"></a>Articles associés  
[Journalisation des accès utilisateur](https://msdn.microsoft.com/library/windows/desktop/hh437528(v=vs.85).aspx) sur MSDN.  
[Gérer la journalisation des accès utilisateur](Manage-User-Access-Logging.md)  
  

