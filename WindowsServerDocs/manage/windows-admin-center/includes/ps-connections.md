```powershell
# Load the module
Import-Module "$env:ProgramFiles\windows admin center\PowerShell\Modules\ConnectionTools"
# Available cmdlets: Export-Connection, Import-Connection

# Export connections (including tags) to a .csv file
Export-Connection "https://wac.contoso.com" -fileName "WAC-connections.csv"
# Import connections (including tags) from a .csv file
Import-Connection "https://wac.contoso.com" -fileName "WAC-connections.csv"
# Import connections (including tags) from .csv files, and remove any connections that are not explictly in the imported file using the -prune switch parameter 
Import-Connection "https://wac.contoso.com" -fileName "WAC-connections.csv" -prune
```
### <a name="csv-file-format-for-importing-connections"></a>Format de fichier CSV pour l’importation de connexions

Le format du fichier CSV commence par les quatre en-têtes ```"name","type","tags","groupId"```, suivis par chaque connexion sur une nouvelle ligne.

**name** est le nom de domaine complet de la connexion.

**type** est le type de connexion. Pour les connexions par défaut incluses dans Windows Admin Center, vous devez utiliser l’une des options suivantes :

| Type de connexion | Chaîne de connexion |
|------|-------------------------------|
| Windows Server | msft.sme.connection-type.server |
| PC Windows 10 | msft.sme.connection-type.windows-client |
| Cluster de basculement | msft.sme.connection-type.cluster |
| Cluster hyperconvergé | msft.sme.connection-type.hyper-converged-cluster |

Les étiquettes (**tags**) sont séparées par des barres verticales.

**groupId** est utilisé pour les connexions partagées. Utilisez la valeur ```global``` dans cette colonne pour en faire une connexion partagée.

> [!NOTE]
> La modification des connexions partagées est limitée aux administrateurs de passerelle : tout utilisateur peut utiliser PowerShell pour modifier sa liste de connexions personnelle.

### <a name="example-csv-file-for-importing-connections"></a>Exemple de fichier CSV pour l’importation de connexions

```
"name","type","tags","groupId"
"myServer.contoso.com","msft.sme.connection-type.server","hyperv"
"myDesktop.contoso.com","msft.sme.connection-type.windows-client","hyperv"
"teamcluster.contoso.com","msft.sme.connection-type.cluster","legacyCluster|WS2016","global"
"myHCIcluster.contoso.com,"msft.sme.connection-type.hyper-converged-cluster","myHCIcluster|hyperv|JIT|WS2019"
"teamclusterNode.contoso.com","msft.sme.connection-type.server","legacyCluster|WS2016","global"
"myHCIclusterNode.contoso.com","msft.sme.connection-type.server","myHCIcluster|hyperv|JIT|WS2019"
```

## <a name="import-rdcman-connections"></a>Importer des connexions RDCman

Utilisez le script ci-dessous pour exporter dans un fichier des connexions enregistrées dans [RDCman](https://blogs.technet.microsoft.com/rmilne/2014/11/19/remote-desktop-connection-manager-download-rdcman-2-7/). Ensuite, vous pouvez importer ce fichier dans Windows Admin Center, en conservant votre hiérarchie de regroupement RDCMan à l’aide d’étiquettes. Faites un essai.

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

2. Pour créer un fichier CSV, exécutez la commande suivante :

   ```powershell
   RdgToWacCsv -RDGfilepath "path\to\myRDCManfile.rdg"
   ```

3. Importez le fichier CSV résultant dans Windows Admin Center et toute votre hiérarchie de regroupement RDCMan sera représentée par des étiquettes dans la liste de connexions. Pour plus d’informations, consultez [Utiliser PowerShell pour importer ou exporter vos connexions (avec étiquettes)](#use-powershell-to-import-or-export-your-connections-with-tags).