---
title: Vous familiariser avec Windows Admin Center
description: Vous familiariser avec Windows Admin Center
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 02/15/2019
ms.openlocfilehash: f4fd9f69e75ed80bbdb345b4041c2337c65ec2e6
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296651"
---
# Vous familiariser avec Windows Admin Center

>S’applique à: Windows Admin Center, Windows Admin Center Preview

> [!Tip]
> Vous débutez dans Windows Admin Center?
> [Découvrez-en davantage sur Windows Admin Center](../understand/windows-admin-center.md) ou [téléchargez maintenant](https://aka.ms/windowsadmincenter).

## Windows Admin Center est installé sur Windows 10

> [!IMPORTANT]
> Vous devez être membre du groupe de l’administrateur local pour utiliser Windows Admin Center sur Windows 10

### Sélection d’un certificat client

La première fois que vous ouvrez Windows Admin Center sur Windows 10, veillez à sélectionner le certificat *Client de Windows Admin Center* (sinon, vous obtiendrez un message d’erreur HTTP 403 indiquant «ne peut pas accéder à cette page»).

Dans Microsoft Edge, lorsque vous y êtes invité avec cette boîte de dialogue:
 
1. Cliquez sur le **plus grand choix**

    ![](../media/launch-cert-1.png)

2. Sélectionnez le certificat intitulé **Windows Admin Center Client** et cliquez sur **OK**

    ![](../media/launch-cert-2.png)

3. Veillez à **Que toujours autoriser l’accès** est sélectionné, puis cliquez sur **Autoriser**

    ![](../media/launch-cert-3.png)

## Connexion aux nœuds gérés et les clusters

Une fois que vous avez terminé l’installation de Windows Admin Center, vous pouvez ajouter des serveurs ou des clusters pour gérer à partir de la page de présentation principale.

 **Ajouter un seul serveur ou un cluster comme un nœud géré**

 1. Cliquez sur **+ Ajouter** sous **toutes les connexions**.

    ![](../media/launch/addserver0.png)

 2. Choisir d’ajouter une connexion serveur, Cluster de basculement ou un Cluster hyperconvergé:
    
    ![](../media/launch/addserver1.png)

 3. Tapez le nom du serveur ou cluster pour gérer et cliqué sur **Soumettre**. Le serveur ou cluster sera ajouté à votre liste de connexion sur la page de présentation.

    ![](../media/launch/addserver2.png)

   **-- OU --**

**Plusieurs serveurs d’importation en bloc**

 1. Sur la page **Ajouter une connexion serveur** , choisissez l’onglet **Des serveurs d’importation** .

    ![](../media/launch/import-servers.png)

 2. Cliquez sur **Parcourir** et sélectionner un fichier texte contenant une virgule ou la nouvelle ligne séparées, la liste des noms de domaines complets pour les serveurs que vous voulez ajouter.

    **-- OU --**

**Ajouter des serveurs en recherchant dans Active Directory**

 1. Sur la page **Ajouter une connexion serveur** , choisissez l’onglet **Rechercher dans Active Directory** .

    ![](../media/launch/search-ad.png)

 2. Entrez vos critères de recherche et cliquez sur **Rechercher**. Caractères génériques (*) sont pris en charge.

 3. Une fois la recherche terminée - sélectionnez un ou plusieurs des résultats, si vous le souhaitez ajouter des balises, puis cliquez sur **Ajouter**.

## S’authentifier avec le nœud géré ##

Windows Admin Center prend en charge plusieurs mécanismes pour l’authentification à un nœud géré. De l’authentification unique est la valeur par défaut.

**Single Sign-on**

Vous pouvez utiliser vos informations d’identification Windows actuelles pour s’authentifier avec le nœud géré. Il s’agit de la valeur par défaut, et Windows Admin Center tente l’ouverture de session lorsque vous ajoutez un serveur. 

**Authentification unique sur lorsqu’elles sont déployées en tant que Service sur Windows Server**

Si vous avez installé Windows Admin Center sur Windows Server, une configuration supplémentaire est nécessaire pour l’authentification unique.  [Configurer votre environnement pour la délégation](..\configure\user-access-control.md)

**-- OU --**

**Utilisez *Gérer en tant que* pour spécifier les informations d’identification**

Sous **Toutes les connexions**, sélectionnez un serveur dans la liste, puis choisissez **Gérer en tant que** pour spécifier les informations d’identification que vous allez utiliser pour s’authentifier sur le nœud géré:

![](../media/launch-use-6.png)

Si Windows Admin Center est en cours d’exécution en mode de service sur Windows Server, mais vous n’avez pas configurée la délégation Kerberos, vous devez ré-entrer vos informations d’identification Windows:

![](../media/launch-use-7.png)

Vous pouvez appliquer les informations d’identification à toutes les connexions, qui seront les mettre en cache pour cette session de navigateur spécifique. Si vous rechargez votre navigateur, vous devez ré-entrer vos informations d’identification **Gérer en tant que** .

**Solution de mot de passe administrateur local (LAPS)**

Si votre environnement utilise [LAPS](https://technet.microsoft.com/mt227395.aspx), vous pouvez utiliser les informations d’identification LAPS pour s’authentifier avec le nœud géré. **Si vous utilisez ce scénario, veuillez** [fournir des commentaires](http://aka.ms/WACFeedback).

## À l’aide de balises d’organiser vos connexions

Vous pouvez utiliser des balises pour identifier et filtrer des serveurs associés dans votre liste de connexion.  Cela vous permet de voir un sous-ensemble de vos serveurs dans la liste de connexion.  Cela est particulièrement utile si vous disposez de nombreuses connexions.

### Modifier les balises

* Sélectionnez un ou plusieurs serveurs dans la liste de toutes les connexions
* Sous **Toutes les connexions**, cliquez sur **Modifier les balises**

![](../media/launch/tags-5.png)

Le volet de **Modifier les balises de connexion** vous permet de vous permettent de modifier, ajouter ou supprimer des balises à partir de votre connexion sélectionné (s):

* Pour ajouter une nouvelle balise à votre connexion sélectionné (s), sélectionnez **Ajouter une balise** et entrez le nom de balise que vous souhaitez utiliser.

* Pour marquer les connexions sélectionnées avec un nom de balise existant, cochez la case en regard du nom de balise que vous souhaitez appliquer.

* Pour supprimer une balise de connexions sélectionnées, décochez la case en regard de la balise que vous souhaitez supprimer.

* Si une balise est appliquée à un sous-ensemble des connexions sélectionnées, la case à cocher s’affiche dans un état intermédiaire. Vous pouvez cliquer sur la case pour vérifier s’il existe et appliquer la balise à toutes les connexions sélectionnées ou cliquez à nouveau pour désactiver et supprimer la balise de toutes les connexions sélectionnées.

![](../media/launch/tags-6.png)

### Filtrer les connexions par balise

Une fois que les balises ont été ajoutées à un ou plusieurs connexions du serveur, vous pouvez afficher les balises dans la liste de connexion et filtrer la liste des connexions de balises.

* Pour filtrer par une balise, sélectionnez l’icône de filtre en regard de la zone de recherche.
![](../media/launch/tags-7.png)
* Vous pouvez sélectionner «ou», «et» ou «non» pour modifier le comportement de filtre des balises sélectionnés.
![](../media/launch/tags-8.png)

## Utiliser PowerShell pour importer ou exporter vos connexions (avec des balises)

> S’applique à: Windows Admin Center Preview

Windows Admin Center Preview inclut un module PowerShell pour importer ou exporter votre liste de connexion.

```powershell
# Load the module
Import-Module "$env:ProgramFiles\windows admin center\PowerShell\Modules\ConnectionTools"
# Available cmdlets: Export-Connection, Import-Connection

# Export connections (including tags) to .csv files
Export-Connection "https://wac.contoso.com" -fileName "WAC-connections.csv"
# Import connections (including tags) from .csv files
Import-Connection "https://wac.contoso.com" -fileName "WAC-connections.csv"
```

### Format de fichier CSV pour l’importation des connexions

Le format du fichier CSV commence par les en-têtes de quatre ```"name","type","tags","groupId"```, suivi de chaque connexion sur une nouvelle ligne.

**nom** est le nom de domaine complet de la connexion

**type** est le type de connexion. Pour les connexions par défaut incluses avec Windows Admin Center, vous allez utiliser une des opérations suivantes:

| Type de connexion | Chaîne de connexion |
|------|-------------------------------|
| WindowsServer | msft.SME.Connection-type.server |
| PC Windows 10 | msft.SME.Connection-type.windows-client |
| Cluster de basculement | msft.SME.Connection-type.cluster |
| Cluster Hyperconvergé | msft.SME.Connection-type.hyper-convergé-cluster |

**les balises** sont séparées par canal.

**groupId** est utilisé pour les connexions partagées. Utilisez la valeur ```global``` dans cette colonne de rendre une connexion partagée.

### Exemple de fichier CSV pour l’importation des connexions

```
"name","type","tags","groupId"
"myServer.contoso.com","msft.sme.connection-type.server","hyperv"
"myDesktop.contoso.com","msft.sme.connection-type.windows-client","hyperv"
"teamcluster.contoso.com","msft.sme.connection-type.cluster","legacyCluster|WS2016","global"
"myHCIcluster.contoso.com,"msft.sme.connection-type.hyper-converged-cluster","myHCIcluster|hyperv|JIT|WS2019"
"teamclusterNode.contoso.com","msft.sme.connection-type.server","legacyCluster|WS2016","global"
"myHCIclusterNode.contoso.com","msft.sme.connection-type.server","myHCIcluster|hyperv|JIT|WS2019"
```

## Importer des connexions RDCman

Utilisez le script ci-dessous pour exporter les connexions enregistrées dans [RDCman](https://blogs.technet.microsoft.com/rmilne/2014/11/19/remote-desktop-connection-manager-download-rdcman-2-7/) dans un fichier. Vous pouvez ensuite importer le fichier dans Windows Admin Center, maintenir votre hiérarchie de regroupement RDCMan à l’aide de balises. Tentez l’expérience!

1. Copiez et collez le code ci-dessous dans votre session PowerShell:

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

2. Pour créer un. Fichier CSV, exécutez la commande suivante:

   ```powershell
   RdgToWacCsv -RDGfilepath "path\to\myRDCManfile.rdg"
   ```

3. Importez résultant. Dans un fichier CSV pour Windows Admin Center et toutes les votre hiérarchie de regroupement RDCMan seront représenté par des balises dans la liste de connexion. Pour plus d’informations, voir [Utiliser PowerShell pour importer ou exporter vos connexions (avec des balises)](#use-powershell-to-import-or-export-your-connections-with-tags).

## Afficher les scripts PowerShell utilisés dans Windows Admin Center

Une fois que vous êtes connecté à un serveur, cluster ou PC, vous pouvez consulter les scripts PowerShell cette puissance les actions de l’interface utilisateur disponibles dans Windows Admin Center. À partir d’au sein d’un outil, cliquez sur l’icône de PowerShell dans la barre d’application supérieure. Sélectionnez une commande d’intérêt dans la liste déroulante pour naviguer vers le script PowerShell correspondant.

![](../media/launch/showscript.png)