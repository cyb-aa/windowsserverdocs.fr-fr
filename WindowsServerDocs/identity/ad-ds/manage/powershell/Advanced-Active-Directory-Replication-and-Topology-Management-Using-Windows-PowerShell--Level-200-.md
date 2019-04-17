---
ms.assetid: fe05e52c-cbf8-428b-8176-63407991042f
title: "Gestion avancée de la réplication Active Directory et topologie à l’aide de Windows PowerShell (niveau 200)"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 1e05616b4b594ae54fcaa3ec6496c0917ecde38b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="advanced-active-directory-replication-and-topology-management-using-windows-powershell-level-200"></a>Gestion avancée de la réplication Active Directory et topologie à l’aide de Windows PowerShell (niveau 200)

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Cette rubrique décrit la nouvelle réplication AD DS et les applets de commande de gestion topologie plus en détail et fournit des exemples supplémentaires. Pour obtenir une présentation, voir [Introduction à la réplication Active Directory et gestion à l’aide de Windows PowerShell de topologie & #40; Niveau 100 & #41; ](../../../ad-ds/manage/powershell/Introduction-to-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-100-.md).  
  
1.  [Introduction](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_Intro)  
  
2.  [La réplication et métadonnées](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_Repl)  
  
3.  [Get-ADReplicationAttributeMetadata](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_ReplAttrMD)  
  
4.  [Get-ADReplicationPartnerMetadata](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_PartnerMD)  
  
5.  [Get-ADReplicationFailure](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_ReplFail)  
  
6.  [Get-ADReplicationQueueOperation et Get-ADReplicationUpToDatenessVectorTable](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_ReplQueue)  
  
7.  [La synchronisation-ADObject](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_Sync)  
  
8.  [Topologie](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_Topo)  
  
## <a name="BKMK_Intro"></a>Introduction  
Windows Server 2012 étend le module Active Directory pour Windows PowerShell avec vingt-cinq nouvelles applets de commande pour gérer la réplication et la topologie de forêt. Avant cette version, vous deviez utiliser générique **\*-AdObject** noms ou appeler des fonctions .NET.  
  
Comme toutes les applets de commande Active Directory Windows PowerShell, cette nouvelle fonctionnalité nécessite l’installation du [Service passerelle de gestion Active Directory](https://www.microsoft.com/download/details.aspx?displaylang=en&id=2852) au moins un contrôleur de domaine (et de préférence, tous les contrôleurs de domaine).  
  
Le tableau suivant répertorie les applets de commande de topologie ajoutées au module Active Directory pour Windows PowerShell et de nouvelle réplication.  
  
|||  
|-|-|  
|Applet de commande|Explication|  
|Get-ADReplicationAttributeMetadata|Retourne les métadonnées de réplication pour un objet d’attribut|  
|Get-ADReplicationConnection|Retourne des informations sur les objets de connexion de contrôleur de domaine|  
|Get-ADReplicationFailure|Renvoie le plus récent Échec de réplication pour un contrôleur de domaine|  
|Get-ADReplicationPartnerMetadata|Retourne la configuration de la réplication d’un contrôleur de domaine|  
|Get-ADReplicationQueueOperation|Renvoie la liste d’attente de réplication en cours|  
|Get-ADReplicationSite|Retourne les informations de site|  
|Get-ADReplicationSiteLink|Retourne les informations de lien de site|  
|Get-ADReplicationSiteLinkBridge|Retourne les informations de pont lien de site|  
|Get-ADReplicationSubnet|Retourne les informations de sous-réseau Active Directory|  
|Get-ADReplicationUpToDatenessVectorTable|Retourne le vecteur jour (utd) pour un contrôleur de domaine|  
|Get-ADTrust|Renvoie des informations sur une approbation inter-domaines ou inter-forêts|  
|New-ADReplicationSite|Crée un nouveau site|  
|Nouvelle ADReplicationSiteLink|Crée un lien de site|  
|Nouvelle ADReplicationSiteLinkBridge|Crée un pont entre liens|  
|Nouvelle ADReplicationSubnet|Crée un nouveau sous-réseau Active Directory|  
|Remove-ADReplicationSite|Supprime un site|  
|Remove-ADReplicationSiteLink|Supprime un lien de sites|  
|Remove-ADReplicationSiteLinkBridge|Supprime un pont entre liens de site|  
|Remove-ADReplicationSubnet|Supprime un sous-réseau Active Directory|  
|Set-ADReplicationConnection|Modifie une connexion|  
|Set-ADReplicationSite|Modifie un site|  
|Set-ADReplicationSiteLink|Modifie un lien de sites|  
|Set-ADReplicationSiteLinkBridge|Modifie un pont entre liens de site|  
|Set-ADReplicationSubnet|Modifie un sous-réseau Active Directory|  
|La synchronisation-ADObject|Force la réplication d’un objet unique|  
  
La plupart de ces applets de commande ont leur base Repadmin.exe. Autres applets de commande (non répertoriées) gèrent des fonctionnalités comme le contrôle d’accès dynamique et le groupe de comptes de Service administrés.  
  
Pour obtenir une liste complète de toutes les applets de commande Active Directory Windows PowerShell, exécutez:  
  
```  
Get-command -module ActiveDirectory  
```  
  
Pour obtenir une liste complète de tous les arguments d’applet de commande Active Directory Windows PowerShell, consultez l’aide. Par exemple:  
  
```  
Get-help New-ADReplicationSite  
  
```  
  
Utilisez le `Update-Help` applet de commande pour télécharger et installer les fichiers d’aide  
  
### <a name="BKMK_Repl"></a>La réplication et métadonnées  
Repadmin.exe valide l’intégrité et la cohérence de réplication Active Directory. Repadmin.exe offre des options de manipulation simple des données: certains arguments prennent en charge les sorties CSV, par exemple - mais automatisation a généralement exigé une analyse des sorties de fichiers texte. Le module Active Directory pour Windows PowerShell est la première tentative offre une option qui permet un contrôle réel sur les données retournées; avant cette version, vous deviez créer des scripts ou utiliser des outils tiers.  
  
En outre, les applets de commande suivantes implémentent un nouveau jeu de paramètres **cible**, **étendue**, et **EnumerationServer**:  
  
-   **Get-ADReplicationFailure**  
  
-   **Get-ADReplicationPartnerMetadata**  
  
-   **Get-ADReplicationUpToDatenessVectorTable**  
  
Le **cible** argument accepte une liste séparée par des virgules de chaînes qui identifient les serveurs cibles, sites, des domaines ou forêts spécifiés par le **étendue** argument. Un astérisque (\ *) est également autorisé et signifie que tous les serveurs de l’étendue spécifiée. Si aucune étendue n’est spécifié, il implique tous les serveurs de la forêt de l’utilisateur actuel. Le **étendue** argument spécifie la latitude de la recherche. Les valeurs acceptables sont **Server**, **Site**, **domaine**, et **forêt**. Le **EnumerationServer** Spécifie le serveur qui énumère la liste des contrôleurs de domaine spécifiés dans **cible** et **étendue**. Il fonctionne comme le **Server** argument et nécessite le serveur spécifié exécute le Service Web Active Directory.  
  
Pour introduire les nouvelles applets de commande, ici sont des exemples de scénarios illustrant capacités impossibles Repadmin.exe; Grâce à ces illustrations, les possibilités d’administration deviennent évidentes. Consultez l’aide de l’applet de commande pour des besoins spécifiques.  
  
### <a name="BKMK_ReplAttrMD"></a>Get-ADReplicationAttributeMetadata  
Cette applet de commande est similaire à **repadmin.exe /showobjmeta**. Il vous permet de retourner les métadonnées de réplication, telles que le changement d’un attribut, le contrôleur de domaine origine, la version informations USN et des données d’attribut. Cette applet de commande est utile pour vérifier où et quand une modification s’est produite.  
  
Contrairement à Repadmin, Windows PowerShell donne flexible recherche et sortie de contrôle. Par exemple, vous pouvez exporter les métadonnées de l’objet Admins du domaine, sous forme de liste lisible:  
  
```  
Get-ADReplicationAttributeMetadata -object "cn=domain admins,cn=users,dc=corp,dc=contoso,dc=com" -server dc1.corp.contoso.com -showalllinkedvalues | format-list  
  
```  
  
![gestion avancée avec powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplAttrMd.png)  
  
Vous pouvez également vous pouvez réorganiser les données comme dans repadmin, dans un tableau:  
  
```  
Get-ADReplicationAttributeMetadata -object "cn=domain admins,cn=users,dc=corp,dc=contoso,dc=com" -server dc1.corp.contoso.com -showalllinkedvalues | format-table -wrap  
  
```  
  
![gestion avancée avec powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplAttrMdTable.png)  
  
Vous pouvez également obtenir des métadonnées pour une classe entière d’objets, par le traitement en pipeline les **Get-Adobject** applet de commande avec un filtre, par exemple, tous les groupes - qui combinent puis avec une date spécifique. Le pipeline est un canal utilisé entre plusieurs applets de commande pour transmettre des données. Pour afficher tous les groupes de modification d’une certaine façon le 13 janvier 2012:  
  
```  
get-adobject -filter 'objectclass -eq "group"' | Get-ADReplicationAttributeMetadata -server dc1.corp.contoso.com | where-object {$_.lastoriginatingchangetime -like "*1/13/2012*" -and $_.attributename -eq "name"} | format-table object  
```  
  
![gestion avancée avec powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplAttrMdClass.png)  
  
Pour plus d’informations sur d’autres opérations Windows PowerShell avec des pipelines, voir [et utilisation des pipelines dans Windows PowerShell](https://technet.microsoft.com/library/ee176927.aspx).  
  
Vous pouvez également, pour en savoir chaque groupe qui comporte Tony Wang en tant que membre et le groupe de dernière modification:  
  
```  
get-adobject -filter 'objectclass -eq "group"' | Get-ADReplicationAttributeMetadata -server dc1.corp.contoso.com -showalllinkedvalues | where-object {$_.attributevalue -like "*tony wang*"} | format-table object,LastOriginatingChangeTime,version -auto  
  
```  
  
![gestion avancée avec powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplAttrMdFilter.png)  
  
Sinon, pour rechercher tous les objets faisant autorité restauré à l’aide d’une sauvegarde de l’état système dans le domaine, en fonction de leur version arbitrairement haute:  
  
```  
get-adobject -filter 'objectclass -like "*"' | Get-ADReplicationAttributeMetadata -server dc1.corp.contoso.com | where-object {$_.version -gt "100000" -and $_.attributename -eq "name"} | format-table object,LastOriginatingChangeTime  
```  
  
![gestion avancée avec powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplAttrMdFilter2.png)  
  
Vous pouvez également envoyer toutes les métadonnées de l’utilisateur vers un fichier CSV pour le consulter ultérieurement dans Microsoft Excel:  
  
```  
get-adobject -filter 'objectclass -eq "user"' | Get-ADReplicationAttributeMetadata -server dc1.corp.contoso.com -showalllinkedvalues | export-csv allgroupmetadata.csv  
```  
  
### <a name="BKMK_PartnerMD"></a>Get-ADReplicationPartnerMetadata  
Cette applet de commande retourne des informations sur la configuration et l’état de réplication d’un contrôleur de domaine, ce qui vous permet de surveiller, créer un inventaire ou résoudre les problèmes. À la différence de Repadmin.exe, à l’aide de Windows PowerShell permet de qu'afficher uniquement les données qui sont importantes pour vous, dans le format souhaité.  
  
Par exemple, l’état de réplication lisible d’un contrôleur de domaine unique:  
  
```  
Get-ADReplicationPartnerMetadata -target dc1.corp.contoso.com  
```  
  
![gestion avancée avec powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplPartnerMd.png)  
  
Vous pouvez également la dernière fois qu’un contrôleur de domaine répliqué entrant et ses partenaires, dans une table de format:  
  
```  
Get-ADReplicationPartnerMetadata -target dc1.corp.contoso.com | format-table lastreplicationattempt,lastreplicationresult,partner -auto  
```  
  
![gestion avancée avec powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplPartnerMdTable.png)  
  
Vous pouvez également contacter tous les contrôleurs de domaine dans la forêt et afficher tout dont dernière tentative de réplication a échoué pour une raison quelconque:  
  
```  
Get-ADReplicationPartnerMetadata -target * -scope server | where {$_.lastreplicationresult -ne "0"} | ft server,lastreplicationattempt,lastreplicationresult,partner -auto  
  
```  
  
![gestion avancée avec powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplPartnerMdFail.png)  
  
### <a name="BKMK_ReplFail"></a>Get-ADReplicationFailure  
Cette applet de commande peut être utilisé pour renvoie des informations sur les erreurs récentes de réplication. Elle est analogue à **Repadmin.exe /showreplsum**, mais à nouveau, avec beaucoup plus contrôlent grâce à Windows PowerShell.  
  
Par exemple, vous pouvez revenir échecs les plus récents d’un contrôleur de domaine et les partenaires qu’il n’a pas pu contacter:  
  
```  
Get-ADReplicationFailure dc1.corp.contoso.com  
```  
  
![gestion avancée avec powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplFail.png)  
  
Vous pouvez également renvoyer un affichage tableau de tous les serveurs dans un site logique Active Directory spécifique, classé pour faciliter la consultation et contenant uniquement les données essentielles:  
  
```  
Get-ADReplicationFailure -scope site -target default-first-site-name | format-table server,firstfailuretime,failurecount,lasterror,partner -auto  
  
```  
  
![gestion avancée avec powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplFailScoped.png)  
  
### <a name="BKMK_ReplQueue"></a>Get-ADReplicationQueueOperation et Get-ADReplicationUpToDatenessVectorTable  
Les deux de ces applets de commande renvoient les aspects du contrôleur de domaine «jusqu'à futurs», qui inclut la réplication et les informations de vecteur de version en attente.  
  
### <a name="BKMK_Sync"></a>La synchronisation-ADObject  
Cette applet de commande ressemble à exécuter **Repadmin.exe /replsingleobject**. Il est très utile lorsque vous apportez des modifications qui nécessitent une réplication hors bande, notamment pour corriger un problème.  
  
Par exemple, si un utilisateur supprimé le compte d’utilisateur du PDG et restauré avec la Corbeille Active Directory, vous pouvez qu'il répliqués sur tous les contrôleurs de domaine immédiatement. Vous voulez également probablement sans forcer la réplication de tous les autres objets modifications; Après tout, c’est pourquoi vous avez un calendrier de réplication, pour éviter la surcharge des liaisons WAN.  
  
```  
Get-ADDomainController -filter * | foreach {Sync-ADObject -object "cn=tony wang,cn=users,dc=corp,dc=contoso,dc=com" -source dc1 -destination $_.hostname}  
  
```  
  
![gestion avancée avec powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSSyncAD.png)  
  
### <a name="BKMK_Topo"></a>Topologie  
Repadmin.exe est bon de renvoyer des informations sur la topologie de réplication comme des sites, liens de sites, ponts entre liens de site et les connexions, il ne dispose pas d’un ensemble complet d’arguments pour apporter des modifications. En fait, il n’a jamais été intégré, scriptable, utilitaire Windows conçu spécifiquement pour les administrateurs à créer et modifier la topologie AD DS. Comme Active Directory a évolué dans des millions d’environnements de clients, de modifier le besoin en bloc Active Directory des informations logiques apparaît.  
  
Par exemple, après une expansion rapide de nouvelles succursales, associée à la consolidation des autres, vous devrez peut-être une centaine de modifications pour vous en fonction sur des emplacements physiques, les modifications du réseau et les nouvelles exigences de capacité. Au lieu d’utiliser Dssites.msc et Adsiedit.msc pour apporter des modifications, vous pouvez automatiser. Cela est particulièrement intéressante lorsque vous démarrez avec une feuille de calcul des données fournies par vos équipes de réseau et les installations.  
  
Le **Get-Adreplication\ *** applets de commande retourne des informations sur la topologie de réplication et sont utiles pour traiter en pipeline les **définir-Adreplication\ *** applets de commande en bloc. **Obtenir** applets de commande ne modifient pas les données, elles affichent uniquement les données ou pour Windows PowerShell de créer des objets de session qui peuvent être traités en pipeline vers **définir-Adreplication\ *** applets de commande. Le **New** et **supprimer** applets de commande sont utiles pour créer ou supprimer des objets de la topologie Active Directory.  
  
Par exemple, vous pouvez créer de nouveaux sites à l’aide d’un fichier CSV:  
  
```  
import-csv -path C:\newsites.csv | new-adreplicationsite  
```  
  
![gestion avancée avec powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSNewSitesCSV.png)  
  
![gestion avancée avec powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSImportCSV.png)  
  
Vous pouvez également créer un lien de site entre deux sites existants avec un coût d’intervalle et un site de réplication personnalisée:  
  
```  
new-adreplicationsitelink -name "chicago<-->waukegan" -sitesincluded chicago,waukegan -cost 50 -replicationfrequencyinminutes 15  
```  
  
![gestion avancée avec powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSNewADReplSite.png)  
  
Vous pouvez également rechercher tous les sites dans la forêt et remplacer leurs **Options** attributs avec l’indicateur pour activer intersite notification de modifications, afin de répliquer avec une compression:  
  
```  
get-adreplicationsitelink -filter * | set-adobject -replace @{options=$($_.options -bor 1)}  
```  
  
![gestion avancée avec powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSNewADReplSiteLink.gif)  
  
> [!IMPORTANT]  
> Définissez **- bor 5** pour désactiver la compression sur les liens de sites.  
  
Vous pouvez également rechercher tous les sites affectations manquantes de sous-réseaux, pour rapprocher la liste avec les sous-réseaux réels de ces emplacements:  
  
```  
get-adreplicationsite -filter * -property subnets | where-object {!$_.subnets -eq "*"} | format-table name  
```  
  
![gestion avancée avec powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSNewADReplSiteFiltrer.png)  
  
## <a name="see-also"></a>Voir aussi  
[Introduction à la réplication ActiveDirectory et gestion de la topologie à l’aide de Windows PowerShell et #40; niveau 100 et #41;](../../../ad-ds/manage/powershell/Introduction-to-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-100-.md)  
  

