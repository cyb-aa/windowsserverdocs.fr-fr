---
title: Prise en main de Windows Admin Center
description: Prise en main de Windows Admin Center
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 02/15/2019
ms.openlocfilehash: ff1f949c764473a63eafa25346949d710699dbd1
ms.sourcegitcommit: 8ba2c4de3bafa487a46c13c40e4a488bf95b6c33
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/25/2019
ms.locfileid: "66222719"
---
# <a name="get-started-with-windows-admin-center"></a>Prise en main de Windows Admin Center

>S'applique à : Windows Admin Center, version préliminaire de Windows Admin Center

> [!Tip]
> Vous débutez dans Windows Admin Center ?
> [Découvrez-en davantage sur Windows Admin Center](../understand/windows-admin-center.md) ou [téléchargez maintenant](https://aka.ms/windowsadmincenter).

## <a name="windows-admin-center-installed-on-windows-10"></a>Windows Admin Center est installé sur Windows 10

> [!IMPORTANT]
> Vous devez être membre du groupe des administrateurs locaux d’utiliser Windows Admin Center sur Windows 10

### <a name="selecting-a-client-certificate"></a>Sélection d’un certificat client

La première fois que vous ouvrez Windows Admin Center sur Windows 10, veillez à sélectionner le *Windows Admin Center Client* certificat (sinon vous obtiendrez une erreur HTTP 403 indiquant « Impossible d’accéder à cette page »).

Dans Microsoft Edge, lorsque vous êtes invité à cette boîte de dialogue :
 
1. Cliquez sur **plus de choix**

    ![](../media/launch-cert-1.png)

2. Sélectionnez le certificat étiqueté **Windows Admin Center Client** et cliquez sur **OK**

    ![](../media/launch-cert-2.png)

3. Assurez-vous que **toujours autoriser l’accès** est sélectionné, cliquez sur **autoriser**

    ![](../media/launch-cert-3.png)

## <a name="connecting-to-managed-nodes-and-clusters"></a>Connexion aux nœuds gérés et aux clusters

Une fois que vous avez terminé l’installation de Windows Admin Center, vous pouvez ajouter des serveurs ou clusters à gérer à partir de la page de présentation principale.

 **Ajouter un seul serveur ou un cluster comme un nœud géré**

 1. Cliquez sur **+ ajouter** sous **toutes les connexions**.

    ![](../media/launch/addserver0.png)

 2. Choisissez cette option pour ajouter une connexion de serveur, Cluster de basculement ou Hyper-Converged Cluster :
    
    ![](../media/launch/addserver1.png)

 3. Tapez le nom du serveur ou du cluster pour gérer et cliquez sur **envoyer**. Le serveur ou le cluster sera ajouté à votre liste des connexions dans la page Vue d’ensemble.

    ![](../media/launch/addserver2.png)

   **--OU--**

**Plusieurs serveurs d’importation en bloc**

 1. Sur le **ajouter une connexion serveur** page, choisissez le **importation serveurs** onglet.

    ![](../media/launch/import-servers.png)

 2. Cliquez sur **Parcourir** et sélectionnez un fichier texte qui contient une virgule ou séparées de la nouvelle ligne, la liste des noms de domaine complets pour les serveurs que vous souhaitez ajouter.

    **--OU--**

**Ajouter des serveurs en recherchant dans Active Directory**

 1. Sur le **ajouter une connexion serveur** page, choisissez le **recherche Active Directory** onglet.

    ![](../media/launch/search-ad.png)

 2. Entrez vos critères de recherche et cliquez sur **recherche**. Les caractères génériques (*) sont pris en charge.

 3. Une fois la recherche terminée - sélectionnez un ou plusieurs des résultats, si vous le souhaitez ajouter des balises, puis cliquez sur **ajouter**.

## <a name="authenticate-with-the-managed-node"></a>S’authentifier avec le nœud géré ##

Windows Admin Center prend en charge plusieurs mécanismes pour l’authentification auprès d’un nœud géré. L’authentification unique est la valeur par défaut.

**Single Sign-on**

Vous pouvez utiliser vos informations d’identification Windows actuelles pour s’authentifier avec le nœud géré. C’est la valeur par défaut, et Windows Admin Center tente l’authentification lorsque vous ajoutez un serveur. 

**L’authentification unique lors du déploiement en tant que Service sur Windows Server**

Si vous avez installé Windows Admin Center sur Windows Server, une configuration supplémentaire est requise pour l’authentification unique.  [Configurer votre environnement pour la délégation](..\configure\user-access-control.md)

**--OU--**

**Utilisez *gérer en tant que* spécifier des informations d’identification**

Sous **toutes les connexions**, sélectionnez un serveur dans la liste et choisissez **gérer en tant que** pour spécifier les informations d’identification que vous allez utiliser pour s’authentifier au nœud géré :

![](../media/launch-use-6.png)

Si Windows Admin Center s’exécute en mode de service sur Windows Server, mais vous n’avez pas configurée la délégation Kerberos, vous devez entrer à nouveau vos informations d’identification Windows :

![](../media/launch-use-7.png)

Vous pouvez appliquer les informations d’identification pour toutes les connexions, ce qui les met en cache pour cette session de navigateur spécifique. Si vous rechargez votre navigateur, vous devez entrer à nouveau votre **gérer en tant que** informations d’identification.

**Solution de mot de passe administrateur local (LAP)**

Si votre environnement utilise [LAPS](https://technet.microsoft.com/mt227395.aspx)et que vous avez Windows Admin Center est installé sur votre PC Windows 10, vous pouvez utiliser les informations d’identification LAPS pour s’authentifier avec le nœud géré. **Si vous utilisez ce scénario, veuillez** [fournir des commentaires](http://aka.ms/WACFeedback).

## <a name="using-tags-to-organize-your-connections"></a>À l’aide de balises pour organiser vos connexions

Vous pouvez utiliser des balises pour identifier et à filtrer les serveurs associés dans votre liste de connexion.  Cela vous permet de voir un sous-ensemble de vos serveurs dans la liste des connexions.  Cela est particulièrement utile si vous avez plusieurs connexions.

### <a name="edit-tags"></a>Modifier les balises

* Sélectionnez un ou plusieurs serveurs dans la liste de toutes les connexions
* Sous **toutes les connexions**, cliquez sur **modifier les balises**

![](../media/launch/tags-5.png)

Le **modifier les balises de connexion** volet permet d’ajouter, modifier ou supprimer des balises à partir de vos connexions sélectionnées :

* Pour ajouter une nouvelle balise à votre connexion (s) sélectionné, sélectionnez **ajouter une balise** et entrez le nom de balise que vous souhaitez utiliser.

* Pour marquer les connexions sélectionnées avec un nom de balise existant, cochez la case en regard du nom de balise que vous souhaitez appliquer.

* Pour supprimer une balise à partir de connexions sélectionnées, désactivez la case en regard de la balise que vous souhaitez supprimer.

* Si une balise est appliquée à un sous-ensemble des connexions sélectionnées, la case à cocher est affichée dans un état intermédiaire. Vous pouvez cliquer sur la case pour vérifier et appliquer l’étiquette à toutes les connexions sélectionnées ou cliquez à nouveau pour désactiver et supprimer la balise de toutes les connexions sélectionnées.

![](../media/launch/tags-6.png)

### <a name="filter-connections-by-tag"></a>Filtrer les connexions par balise

Une fois que les balises ont été ajoutées à un ou plusieurs connexions au serveur, vous pouvez afficher les étiquettes dans la liste des connexions et filtrer la liste des connexions par balises.

* Pour filtrer par une balise, sélectionnez l’icône de filtre en regard de la zone de recherche.
![](../media/launch/tags-7.png)
* Vous pouvez sélectionner « ou », « et » ou « non » pour modifier le comportement de filtre des balises sélectionnées.
![](../media/launch/tags-8.png)

## <a name="use-powershell-to-import-or-export-your-connections-with-tags"></a>Utiliser PowerShell pour importer ou exporter vos connexions (avec balises)

> S'applique à : Windows Admin Center Preview

Version préliminaire de Windows Admin Center inclut un module PowerShell pour importer ou exporter votre liste de connexion.

```powershell
# Load the module
Import-Module "$env:ProgramFiles\windows admin center\PowerShell\Modules\ConnectionTools"
# Available cmdlets: Export-Connection, Import-Connection

# Export connections (including tags) to .csv files
Export-Connection "https://wac.contoso.com" -fileName "WAC-connections.csv"
# Import connections (including tags) from .csv files
Import-Connection "https://wac.contoso.com" -fileName "WAC-connections.csv"
```

### <a name="csv-file-format-for-importing-connections"></a>Format de fichier CSV pour l’importation des connexions

Le format du fichier CSV démarre avec les en-têtes de quatre ```"name","type","tags","groupId"```, suivie de chaque connexion sur une nouvelle ligne.

**nom** est le nom de domaine complet de la connexion

**type** est le type de connexion. Pour les connexions par défaut incluses avec Windows Admin Center, vous allez utiliser une des opérations suivantes :

| Type de connexion | Chaîne de connexion |
|------|-------------------------------|
| Windows Server | msft.sme.connection-type.server |
| PC Windows 10 | msft.sme.connection-type.windows-client |
| Cluster de basculement | msft.sme.connection-type.cluster |
| Cluster Hyperconvergé | msft.sme.connection-type.hyper-converged-cluster |

**balises** sont séparées par un canal.

**groupId** est utilisé pour les connexions partagées. Utilisez la valeur ```global``` dans cette colonne pour faire une connexion partagée.

### <a name="example-csv-file-for-importing-connections"></a>Exemple de fichier CSV pour l’importation des connexions

```
"name","type","tags","groupId"
"myServer.contoso.com","msft.sme.connection-type.server","hyperv"
"myDesktop.contoso.com","msft.sme.connection-type.windows-client","hyperv"
"teamcluster.contoso.com","msft.sme.connection-type.cluster","legacyCluster|WS2016","global"
"myHCIcluster.contoso.com,"msft.sme.connection-type.hyper-converged-cluster","myHCIcluster|hyperv|JIT|WS2019"
"teamclusterNode.contoso.com","msft.sme.connection-type.server","legacyCluster|WS2016","global"
"myHCIclusterNode.contoso.com","msft.sme.connection-type.server","myHCIcluster|hyperv|JIT|WS2019"
```

## <a name="import-rdcman-connections"></a>Connexions d’importation RDCman

Utilisez le script ci-dessous pour exporter les connexions enregistrées dans [RDCman](https://blogs.technet.microsoft.com/rmilne/2014/11/19/remote-desktop-connection-manager-download-rdcman-2-7/) dans un fichier. Vous pouvez ensuite importer le fichier dans Windows Admin Center, maintenance de votre hiérarchie RDCMan regroupement à l’aide de balises. Essayez-le !

1. Copiez et collez le code ci-dessous dans votre session PowerShell :

   ```powershell
   #Helper function for RdgToWacCsv
   function AddServers {
    param (
    [Parameter(Mandatory = $true)]
    [Xml.XmlLinkedNode]
    $node,
    [Parameter()]
    [String[]]
    $tags,
    [Parameter(Mandatory = $true)]
    [String]
    $csvPath
    )
    if ($node.LocalName -eq 'server') {
        $serverName = $node.properties.name
        $tagString = $tags -join "|"
        Add-Content -Path $csvPath -Value ('"'+ $serverName + '","msft.sme.connection-type.server","'+ $tagString +'"')
    } 
    elseif ($node.LocalName -eq 'group' -or $node.LocalName -eq 'file') {
        $groupName = $node.properties.name
        $tags+=$groupName
        $currNode = $node.properties.NextSibling
        while ($currNode) {
            AddServers -node $currNode -tags $tags -csvPath $csvPath
            $currNode = $currNode.NextSibling
        }
    } 
    else {
        # Node type isn't relevant to tagging or adding connections in WAC
    }
    return
   }

   <#
   .SYNOPSIS
   Convert an .rdg file from Remote Desktop Connection Manager into a .csv that can be imported into Windows Admin Center, maintaining groups via server tags. This will not modify the existing .rdg file and will create a new .csv file

    .DESCRIPTION
    This converts an .rdg file into a .csv that can be imported into Windows Admin Center.

    .PARAMETER RDGfilepath
    The path of the .rdg file to be converted. This file will not be modified, only read.

    .PARAMETER CSVdirectory
    Optional. The directory you wish to export the new .csv file. If not provided, the new file is created in the same directory as the .rdg file.

    .EXAMPLE
    C:\PS> RdgToWacCsv -RDGfilepath "rdcmangroup.rdg"
    #>
   function RdgToWacCsv {
    param(
        [Parameter(Mandatory = $true)]
        [String]
        $RDGfilepath,
        [Parameter(Mandatory = $false)]
        [String]
        $CSVdirectory
    )
    [xml]$RDGfile = Get-Content -Path $RDGfilepath
    $node = $RDGfile.RDCMan.file
    if (!$CSVdirectory){
        $csvPath = [System.IO.Path]::GetDirectoryName($RDGfilepath) + [System.IO.Path]::GetFileNameWithoutExtension($RDGfilepath) + "_WAC.csv"
    } else {
        $csvPath = $CSVdirectory + [System.IO.Path]::GetFileNameWithoutExtension($RDGfilepath) + "_WAC.csv"
    }
    New-item -Path $csvPath
    Add-Content -Path $csvPath -Value '"name","type","tags"'
    AddServers -node $node -csvPath $csvPath
    Write-Host "Converted $RDGfilepath `nOutput: $csvPath"
   }
   ```

2. Pour créer un. Fichier CSV, exécutez la commande suivante :

   ```powershell
   RdgToWacCsv -RDGfilepath "path\to\myRDCManfile.rdg"
   ```

3. Importer le résultat. Fichier CSV dans Windows Admin Center et votre hiérarchie de regroupement de RDCMan seront représenté par des balises dans la liste des connexions. Pour plus d’informations, consultez [utiliser PowerShell pour importer ou exporter vos connexions (avec balises)](#use-powershell-to-import-or-export-your-connections-with-tags).

## <a name="view-powershell-scripts-used-in-windows-admin-center"></a>Afficher le script PowerShell utilisés dans Windows Admin Center

Une fois que vous êtes connecté à un serveur, un cluster ou un PC, vous pouvez examiner les scripts PowerShell qui alimentent les actions d’interface utilisateur disponibles dans Windows Admin Center. À partir de dans un outil, cliquez sur l’icône PowerShell située dans la barre d’application supérieure. Sélectionnez une commande qui vous intéresse dans la liste déroulante pour accéder au script PowerShell correspondant.

![](../media/launch/showscript.png)