---
ms.assetid: fe05e52c-cbf8-428b-8176-63407991042f
title: Gestion avancée de la topologie et de la réplication Active Directory avec Windows PowerShell (Niveau 200)
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: eeac84fb4e875ffe31b560bc72190895cd0527bc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402673"
---
# <a name="advanced-active-directory-replication-and-topology-management-using-windows-powershell-level-200"></a>Gestion avancée de la topologie et de la réplication Active Directory avec Windows PowerShell (Niveau 200)

>S’applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cette rubrique décrit plus en détail les applets de commande de gestion de la topologie et de la réplication des services de domaine Active Directory et fournit des exemples supplémentaires. Pour une présentation, consultez [Présentation de la gestion de la topologie et de la réplication Active Directory à l’aide de Windows PowerShell &#40;Level 100&#41;](../../../ad-ds/manage/powershell/Introduction-to-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-100-.md).  
  
1.  [Introduction](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_Intro)  
  
2.  [Réplication et métadonnées](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_Repl)  
  
3.  [ADReplicationAttributeMetadata](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_ReplAttrMD)  
  
4.  [ADReplicationPartnerMetadata](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_PartnerMD)  
  
5.  [ADReplicationFailure](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_ReplFail)  
  
6.  [ADReplicationQueueOperation et ADReplicationUpToDatenessVectorTable](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_ReplQueue)  
  
7.  [Synchronisation-ADObject](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_Sync)  
  
8.  [Topologie](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_Topo)  
  
## <a name="BKMK_Intro"></a>Présentation  
Windows Server 2012 étend le module Active Directory pour Windows PowerShell avec vingt-cinq nouvelles applets de commande pour gérer la réplication et la topologie de forêt. Avant cela, vous deviez utiliser les noms génériques **\*-ADObject** ou appeler des fonctions .net.  
  
Comme les autres applets de commande Active Directory pour Windows PowerShell, cette nouvelle fonctionnalité nécessite l’installation du [Service passerelle de gestion Active Directory](https://www.microsoft.com/download/details.aspx?displaylang=en&id=2852) sur un contrôleur de domaine au moins (et de préférence, sur tous les contrôleurs de domaine).  
  
Le tableau suivant répertorie les nouvelles applets de commande de topologie et de réplication qui ont été ajoutées au module Active Directory pour Windows PowerShell.  
  
|||  
|-|-|  
|Applet de commande|Explication|  
|Get-ADReplicationAttributeMetadata|Renvoie des métadonnées de réplication d'attribut pour un objet|  
|Get-ADReplicationConnection|Retourne les informations sur les objets de connexion du contrôleur de domaine|  
|Get-ADReplicationFailure|Renvoie l'échec de réplication le plus récent d'un contrôleur de domaine|  
|Get-ADReplicationPartnerMetadata|Retourne la configuration de réplication d'un contrôleur de domaine|  
|Get-ADReplicationQueueOperation|Renvoie la liste d'attente actuelle de la réplication|  
|Get-ADReplicationSite|Retourne les informations sur le site|  
|Get-ADReplicationSiteLink|Renvoie les informations sur les liens du site|  
|Get-ADReplicationSiteLinkBridge|Retourne les informations sur le pont lien de sites|  
|Get-ADReplicationSubnet|Renvoie les informations du sous-réseau Active Directory|  
|Get-ADReplicationUpToDatenessVectorTable|Retourne le vecteur de mise à jour (UTD) d'un contrôleur de domaine|  
|Get-ADTrust|Renvoie les informations relatives à l'approbation inter-domaines ou inter-forêts|  
|New-ADReplicationSite|Crée un site|  
|New-ADReplicationSiteLink|Crée un lien de sites|  
|New-ADReplicationSiteLinkBridge|Crée un pont lien de sites|  
|New-ADReplicationSubnet|Crée un sous-réseau Active Directory|  
|Remove-ADReplicationSite|Supprime un site|  
|Remove-ADReplicationSiteLink|Supprime un lien de sites|  
|Remove-ADReplicationSiteLinkBridge|Supprime un pont lien de sites|  
|Remove-ADReplicationSubnet|Supprime un sous-réseau Active Directory|  
|Set-ADReplicationConnection|Change une connexion|  
|Set-ADReplicationSite|Modifie un site|  
|Set-ADReplicationSiteLink|Change un lien de sites|  
|Set-ADReplicationSiteLinkBridge|Modifie un pont lien de sites|  
|Set-ADReplicationSubnet|Change un sous-réseau Active Directory|  
|Sync-ADObject|Force la réplication d'un objet unique|  
  
La base de la plupart des applets de commande se trouve dans Repadmin.exe. D'autres applets de commande (non répertoriées) gèrent des fonctionnalités comme le contrôle d'accès dynamique et les comptes de service administrés de groupe.  
  
Pour obtenir la liste complète de toutes les applets de commande Active Directory pour Windows PowerShell, exécutez :  
  
```  
Get-command -module ActiveDirectory  
```  
  
Pour obtenir la liste complète de tous les arguments d'applets de commande Active Directory pour Windows PowerShell, consultez l'aide. Par exemple :  
  
```  
Get-help New-ADReplicationSite  
  
```  
  
Utilisez l’applet de commande `Update-Help` pour télécharger et installer les fichiers d’aide  
  
### <a name="BKMK_Repl"></a>Réplication et métadonnées  
Repadmin.exe valide l'état et la cohérence de la réplication Active Directory. Repadmin.exe offre des options de manipulation simple des données (certains arguments prennent par exemple en charge les sorties CSV) mais l'automatisation a généralement exigé une analyse des sorties de fichiers texte. Le module Active Directory pour Windows PowerShell offre pour la première fois la possibilité de contrôler directement les données renvoyées. Auparavant, vous deviez créer des scripts ou utiliser des outils tiers.  
  
De plus, les applets de commande suivantes implémentent un nouveau jeu de paramètres **Target**, **Scope**et **EnumerationServer**:  
  
-   **ADReplicationFailure**  
  
-   **ADReplicationPartnerMetadata**  
  
-   **ADReplicationUpToDatenessVectorTable**  
  
L'argument **Target** accepte une liste CSV de chaînes qui identifient les serveurs, sites, domaines ou forêts cibles spécifiés par l'argument **Scope**. Un astérisque (\*) est également autorisé et signifie tous les serveurs dans l’étendue spécifiée. Si aucune étendue n’est spécifiée, elle implique tous les serveurs de la forêt de l’utilisateur actuel. L'argument **Scope** spécifie la latitude de la recherche. Les valeurs acceptables sont **Server**, **Site**, **Domain** et **Forest**. L'argument **EnumerationServer** indique le serveur qui énumère la liste des contrôleurs de domaine spécifiés dans **Target** et **Scope**. Il fonctionne comme l'argument **Server** et nécessite que le serveur spécifié exécute les services Web Active Directory.  
  
Pour présenter les nouvelles applets de commande, voici quelques exemples de scénarios démontrant ce que repadmin.exe ne peut pas réaliser. Avec ces illustrations, les possibilités d'administration deviennent évidentes. Consultez l'aide sur les applets de commande pour connaître les exigences spécifiques en matière d'utilisation.  
  
### <a name="BKMK_ReplAttrMD"></a>ADReplicationAttributeMetadata  
Cette applet de commande est semblable à **repadmin.exe /showobjmeta**. Elle vous permet de renvoyer des métadonnées de réplication, dans des situations telles que le changement d'un attribut, le contrôleur de domaine d'origine, les informations de version et numéro de séquence de mise à jour (USN, Update Sequence Number) et les données d'attribut. Cette applet de commande est utile pour vérifier où et quand le changement s'est produit.  
  
Contrairement à Repadmin, Windows PowerShell permet une recherche et un contrôle de sortie flexible. Par exemple, vous pouvez effectuer la sortie des métadonnées de l'objet Admins du domaine, sous forme de liste lisible :  
  
```  
Get-ADReplicationAttributeMetadata -object "cn=domain admins,cn=users,dc=corp,dc=contoso,dc=com" -server dc1.corp.contoso.com -showalllinkedvalues | format-list  
  
```  
  
![gestion avancée avec PowerShell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplAttrMd.png)  
  
Vous pouvez aussi réorganiser les données en tableau comme dans repadmin :  
  
```  
Get-ADReplicationAttributeMetadata -object "cn=domain admins,cn=users,dc=corp,dc=contoso,dc=com" -server dc1.corp.contoso.com -showalllinkedvalues | format-table -wrap  
  
```  
  
![gestion avancée avec PowerShell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplAttrMdTable.png)  
  
Sinon, vous pouvez obtenir les métadonnées pour une classe entière d'objets, en traitant en pipeline l'applet de commande **Get-Adobject** avec un filtre, tel que « tous les groupes », associé à une date spécifique. Le pipeline est un canal utilisé entre plusieurs applets de commande pour transmettre des données. Pour afficher tous les groupes modifiés d'une certaine façon le 13 janvier 2012 :  
  
```  
get-adobject -filter 'objectclass -eq "group"' | Get-ADReplicationAttributeMetadata -server dc1.corp.contoso.com | where-object {$_.lastoriginatingchangetime -like "*1/13/2012*" -and $_.attributename -eq "name"} | format-table object  
```  
  
![gestion avancée avec PowerShell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplAttrMdClass.png)  
  
Pour plus d’informations sur d’autres opérations Windows PowerShell avec des pipelines, consultez [Définition et utilisation des pipelines dans Windows PowerShell](https://technet.microsoft.com/library/ee176927.aspx).  
  
Vous pouvez aussi rechercher chaque groupe qui comporte le membre Tony Wang et la date à laquelle le groupe a été modifié pour la dernière fois :  
  
```  
get-adobject -filter 'objectclass -eq "group"' | Get-ADReplicationAttributeMetadata -server dc1.corp.contoso.com -showalllinkedvalues | where-object {$_.attributevalue -like "*tony wang*"} | format-table object,LastOriginatingChangeTime,version -auto  
  
```  
  
![gestion avancée avec PowerShell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplAttrMdFilter.png)  
  
Sinon, pour trouver tous les objets qui ont été restaurés à l'aide d'une sauvegarde de l'état du système dans le domaine, d'après leur version arbitrairement haute :  
  
```  
get-adobject -filter 'objectclass -like "*"' | Get-ADReplicationAttributeMetadata -server dc1.corp.contoso.com | where-object {$_.version -gt "100000" -and $_.attributename -eq "name"} | format-table object,LastOriginatingChangeTime  
```  
  
![gestion avancée avec PowerShell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplAttrMdFilter2.png)  
  
Vous pouvez aussi envoyer toutes les métadonnées de l'utilisateur dans un fichier CSV pour le consulter ultérieurement dans Microsoft Excel :  
  
```  
get-adobject -filter 'objectclass -eq "user"' | Get-ADReplicationAttributeMetadata -server dc1.corp.contoso.com -showalllinkedvalues | export-csv allgroupmetadata.csv  
```  
  
### <a name="BKMK_PartnerMD"></a>ADReplicationPartnerMetadata  
Cette applet de commande renvoie les informations sur la configuration et l'état de réplication d'un contrôleur de domaine pour surveiller, créer un inventaire ou résoudre un problème. À la différence de Repadmin.exe, Windows PowerShell permet d'afficher uniquement les données que vous considérez importantes, au format souhaité.  
  
Par exemple, l'état de réplication lisible d'un contrôleur de domaine unique :  
  
```  
Get-ADReplicationPartnerMetadata -target dc1.corp.contoso.com  
```  
  
![gestion avancée avec PowerShell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplPartnerMd.png)  
  
Ou la dernière réplication entrante d'un contrôleur de domaine et ses partenaires, dans un tableau :  
  
```  
Get-ADReplicationPartnerMetadata -target dc1.corp.contoso.com | format-table lastreplicationattempt,lastreplicationresult,partner -auto  
```  
  
![gestion avancée avec PowerShell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplPartnerMdTable.png)  
  
Ou encore, vous pouvez contacter tous les contrôleurs de domaine de la forêt et afficher celui dont la dernière tentative de réplication a échoué pour une raison quelconque :  
  
```  
Get-ADReplicationPartnerMetadata -target * -scope server | where {$_.lastreplicationresult -ne "0"} | ft server,lastreplicationattempt,lastreplicationresult,partner -auto  
  
```  
  
![gestion avancée avec PowerShell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplPartnerMdFail.png)  
  
### <a name="BKMK_ReplFail"></a>ADReplicationFailure  
Cette applet de commande peut être utilisée pour renvoyer des informations sur les erreurs récentes de réplication. Elle est analogue à **Repadmin.exe /showreplsum**, mais avec un niveau nettement plus élevé de contrôle grâce à Windows PowerShell.  
  
Par exemple, vous pouvez renvoyer les échecs les plus récents d'un contrôleur de domaine et les partenaires qu'il n'a pas réussi à contacter :  
  
```  
Get-ADReplicationFailure dc1.corp.contoso.com  
```  
  
![gestion avancée avec PowerShell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplFail.png)  
  
Vous pouvez également renvoyer un affichage sous forme de tableau pour tous les serveurs d'un site logique Active Directory spécifique, classés pour faciliter la consultation et ne contenant que les données essentielles :  
  
```  
Get-ADReplicationFailure -scope site -target default-first-site-name | format-table server,firstfailuretime,failurecount,lasterror,partner -auto  
  
```  
  
![gestion avancée avec PowerShell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplFailScoped.png)  
  
### <a name="BKMK_ReplQueue"></a>ADReplicationQueueOperation et ADReplicationUpToDatenessVectorTable  
Ces deux applets de commande renvoient les aspects futurs d'un contrôleur de domaine « de mise à jour », qui comprend des informations de réplication et de vecteur de version en attente.  
  
### <a name="BKMK_Sync"></a>Synchronisation-ADObject  
Cette applet de commande revient à exécuter **Repadmin.exe /replsingleobject**. Elle s'avère très utile pour apporter des changements qui nécessitent une réplication hors-bande, notamment pour corriger un problème.  
  
Par exemple, si quelqu'un a supprimé le compte d'utilisateur du PDG et l'a restauré avec la Corbeille Active Directory, vous souhaitez certainement qu'il soit immédiatement répliqué sur tous les contrôleurs de domaine sans forcer la réplication de toutes les autres modifications d'objet déjà effectuées. C'est pour cela que vous avez un calendrier de réplication, pour éviter la surcharge des liaisons réseau étendu.  
  
```  
Get-ADDomainController -filter * | foreach {Sync-ADObject -object "cn=tony wang,cn=users,dc=corp,dc=contoso,dc=com" -source dc1 -destination $_.hostname}  
  
```  
  
![gestion avancée avec PowerShell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSSyncAD.png)  
  
### <a name="BKMK_Topo"></a>Topologie  
Repadmin.exe est performant lorsqu'il s'agit de renvoyer des informations sur la topologie de réplication comme des sites, des liens de sites, des ponts liens de sites et des connexions, mais il ne dispose pas d'un ensemble complet d'arguments pour apporter des changements. En réalité, aucun utilitaire Windows intégré, scriptable, n'a été spécifiquement conçu pour des administrateurs en vue de créer et de modifier une topologie AD DS. La modification en bloc des informations logiques Active Directory devient nécessaire, car Active Directory a évolué dans des millions d'environnements de clients.  
  
Par exemple, après une expansion rapide de nouvelles succursales, associée à la consolidation d'autres succursales, vous pouvez être amené à apporter une centaine de modifications en fonction des emplacements physiques, des changements du réseau et des besoins en termes de nouvelle capacité. Au lieu d'utiliser Dssites.msc et Adsiedit.msc pour effectuer ces modifications, vous pouvez les automatiser. Ce changement s'impose alors que vous commencez avec une feuille de calcul de données fournies par votre réseau et les équipes des sites.  
  
Les applets de commande **Adreplication\\** * de l’applet de commande Set-retournent des informations sur la topologie de réplication et sont utiles pour le traitement en pipeline des applets de commande **Set-Adreplication\\** * en bloc. Les **applets** de commande d’accès ne modifient pas les données, elles affichent uniquement les données ou créent des objets de session Windows PowerShell qui peuvent être canalisés vers des applets de commande **Set-Adreplication\\** *. Les applets de commande **New** et **Remove** servent à créer et supprimer les objets de topologie Active Directory.  
  
Par exemple, vous pouvez créer de nouveaux sites à l'aide d'un fichier CSV :  
  
```  
import-csv -path C:\newsites.csv | new-adreplicationsite  
```  
  
![gestion avancée avec PowerShell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSNewSitesCSV.png)  
  
![gestion avancée avec PowerShell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSImportCSV.png)  
  
Vous pouvez également créer un lien de sites entre deux liens existants avec un intervalle de réplication et un coût de site personnalisés :  
  
```  
new-adreplicationsitelink -name "chicago<-->waukegan" -sitesincluded chicago,waukegan -cost 50 -replicationfrequencyinminutes 15  
```  
  
![gestion avancée avec PowerShell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSNewADReplSite.png)  
  
Sinon, vous pouvez rechercher chaque site dans la forêt et remplacer leurs attributs **Options** par le drapeau pour activer la notification de modifications inter-sites, afin de répliquer le plus rapidement possible avec une compression :  
  
```  
get-adreplicationsitelink -filter * | set-adobject -replace @{options=$($_.options -bor 1)}  
```  
  
![gestion avancée avec PowerShell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSNewADReplSiteLink.gif)  
  
> [!IMPORTANT]  
> Définissez **-bor 5** pour désactiver aussi la compression sur ces liens de sites.  
  
Vous pouvez encore rechercher toutes les affectations manquantes de sous-réseaux de sites, pour rapprocher la liste avec les sous-réseaux réels de ces emplacements :  
  
```  
get-adreplicationsite -filter * -property subnets | where-object {!$_.subnets -eq "*"} | format-table name  
```  
  
![gestion avancée avec PowerShell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSNewADReplSiteFiltrer.png)  
  
## <a name="see-also"></a>Voir aussi  
[Présentation de la gestion de la topologie et de la réplication Active Directory à l’aide de Windows PowerShell &#40;Level 100&#41;](../../../ad-ds/manage/powershell/Introduction-to-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-100-.md)  
  

