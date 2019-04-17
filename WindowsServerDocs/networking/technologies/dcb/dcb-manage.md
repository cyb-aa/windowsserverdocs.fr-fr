---
title: Gérer Data Center Bridging (DCB)
description: Cette rubrique fournit des instructions sur la façon d’utiliser les commandes Windows PowerShell pour gérer Data Center Bridging dans Windows Server2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 1575cc7c-62a7-4add-8f78-e5d93effe93f
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: dbe9e5e4af2ddd834b5b8f38e9ffd1b403e92793
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="manage-data-center-bridging-dcb"></a>Gérer Data Center Bridging (DCB)

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Cette rubrique fournit des instructions sur l’utilisation des commandes Windows PowerShell pour configurer \(DCB\) Data Center Bridging sur une carte réseau compatible DCB\ qui est installée sur un ordinateur qui exécute Windows Server2016 ou Windows10.

## <a name="install-dcb-in-windows-server-2016-or-windows-10"></a>Installer DCB dans Windows Server2016 ou Windows10

Pour plus d’informations sur la configuration requise pour l’utilisation et comment installer DCB, voir [installer Data Center Bridging (DCB) dans Windows Server2016 ou Windows10](dcb-install.md).


## <a name="dcb-configurations"></a>Configurations de DCB 

Avant Windows Server2016, la configuration DCB a été appliqués universellement à toutes les cartes réseau prises en charge DCB. 

Dans Windows Server2016, vous pouvez appliquer des configurations DCB pour le magasin de stratégies Global ou Store\(s\) de stratégie individuels. Lorsque des stratégies sont appliquées, ils remplacent tous les paramètres de stratégie globale.

Les configurations de trafic classe, le PFC et application d’affectation de priorité au niveau du système n’est pas appliqué sur les cartes réseau jusqu'à ce que vous procédez comme suit.

1. Activer le bit disposé DCBX sur false

2. Activez DCB sur les cartes réseau. Voir [activer et afficher les paramètres de DCB sur les cartes réseau](#bkmk_enabledcb).

>[!NOTE]
>Si vous souhaitez configurer DCB à partir d’un commutateur via DCBX, voir [paramètres DCBX](#BKMK_DCBX_Settings)

Le bit disposé DCBX est décrite dans la spécification de DCB. Si le bit disposé sur un appareil est défini sur true, le périphérique est prêt à accepter les configurations d’un appareil à distance par le biais de DCBX. Si le bit disposé sur un appareil est défini sur false, le périphérique rejette toutes les tentatives de configuration à partir de périphériques à distance et appliquer uniquement les configurations locales.

Si DCB n’est pas installé dans Windows Server2016 la valeur du bit disposé est pertinente autant que le système d’exploitation est concerné, car le système d’exploitation n’a aucun paramètre local s’appliquent aux cartes réseau. Une fois DCB est installé, le bit acceptez la valeur par défaut est true. Cette conception permet de cartes réseau pour conserver les configurations peuvent avoir reçu de leurs homologues distants.

Si une carte réseau ne prend pas en charge DCBX, il ne recevra jamais les configurations à partir d’un appareil distant. Il reçoit des configurations à partir du système d’exploitation, mais uniquement après le DCBX disposé bits est défini sur false.

## <a name="set-the-willing-bit"></a>Définition du bit prêt

Pour appliquer des configurations de système d’exploitation de classe de trafic, le PFC et affectation de priorité d’applications sur les cartes réseau, ou simplement remplacer les configurations de périphériques distants \, s’il en existe \: vous pouvez exécuter la commande suivante.

>[!NOTE]
>Noms de commande Windows PowerShell de DCB incluent «QoS» au lieu de «DCB» dans la chaîne de nom. Il s’agit de la qualité de service et DCB sont intégrés dans Windows Server2016 pour fournir une expérience de gestion de la qualité de service transparente.

    
    Set-NetQosDcbxSetting -Willing $FALSE
    
    Confirm
    Are you sure you want to perform this action?
    Set-NetQosDcbxSetting -Willing $false
    [Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "Y"):
    

Pour afficher l’état du paramètre bit prêt, vous pouvez utiliser la commande suivante:

    
    Get-NetQosDcbxSetting
    
    Willing PolicySetIfIndex IfAlias
    ------- ---------------- -------
    False   Global  
    

## <a name="dcb-configuration-on-network-adapters"></a>Configuration de DCB sur les cartes réseau

L’activation de DCB sur une carte réseau vous permet de visualiser la configuration propagée à partir d’un commutateur à la carte réseau.

Les configurations de DCB incluent les étapes suivantes.

1.  Configurer les paramètres de DCB au niveau du système, qui inclut:

    un. Gestion de classe de trafic
    
    b. Paramètres de priorité flux de contrôle (PFC)
    
    c. Affectation de priorité d’application
    
    d. Paramètres DCBX

2. Configurer DCB sur la carte réseau.



##  <a name="dcb-traffic-class-management"></a>Gestion de la classe de trafic DCB

Voici des exemples de commandes Windows PowerShell pour la gestion de la classe de trafic.

### <a name="create-a-traffic-class"></a>Créer une classe de trafic

Vous pouvez utiliser le **New-NetQosTrafficClass** commande pour créer une classe de trafic.

    
    New-NetQosTrafficClass -Name SMB -Priority 4 -BandwidthPercentage 30 -Algorithm ETS
    
    Name Algorithm Bandwidth(%) Priority PolicySetIfIndex IfAlias
    ---- --------- ------------ -------- ---------------- -------
    SMB  ETS   30   4Global
      

Par défaut, toutes les valeurs 802 .1p sont mappés à une classe de trafic par défaut, ce qui a 100% de la bande passante du lien physique. Le **New-NetQosTrafficClass** commande crée une nouvelle classe de trafic, à n’importe quel des paquets que les balises de priorité 802 .1p de la valeur 4 est mappé. L’algorithme de sélection de Transmission \(TSA\) ETS et 30% de la bande passante.

Vous pouvez créer jusqu'à 7nouvelles classes de trafic. Y compris la classe de trafic par défaut, il peut exister au maximum 8classes de trafic dans le système. Toutefois, une carte de réseau compatibles DCB ne peut pas prendre en charge que bon nombre de trafic classes dans le matériel. Si vous créez plusieurs classes de trafic que peut être installée sur une carte réseau et que vous activez DCB sur cette carte réseau, le pilote miniport et signale une erreur du système d’exploitation. L’erreur est enregistrée dans le journal des événements.

La somme des réservations de bande passante pour toutes les classes de trafic créé ne peut pas dépasser 99% de la bande passante. La classe de trafic par défaut a toujours au moins 1% de la bande passante réservée pour lui-même.

### <a name="display-traffic-classes"></a>Affiche les Classes de trafic

Vous pouvez utiliser le **Get-NetQosTrafficClass** commande pour afficher les classes de trafic.

    Get-NetQosTrafficClass
    
    NameAlgorithm Bandwidth(%) Priority PolicySetIfIndex IfAlias
    ------------- ------------ -------- ---------------- -------
    [Default]   ETS   70   0-3,5-7  Global
    SMB ETS   30   4Global  
    
### <a name="modify-a-traffic-class"></a>Modifier une classe de trafic

Vous pouvez utiliser le **Set-NetQosTrafficClass** commande pour créer une classe de trafic. 

    Set-NetQosTrafficClass -Name SMB -BandwidthPercentage 50

Vous pouvez ensuite utiliser le **Get-NetQosTrafficClass** commande pour afficher les paramètres.

    Get-NetQosTrafficClass
    
    NameAlgorithm Bandwidth(%) Priority PolicySetIfIndex IfAlias
    ------------- ------------ -------- ---------------- -------
    [Default]   ETS   50   0-3,5-7  Global
    SMB ETS   50   4Global   
    

Après avoir créé une classe de trafic, vous pouvez modifier ses paramètres de manière indépendante. Les paramètres que vous pouvez modifier incluent:

1. La bande passante allocation \(-BandwidthPercentage\)

2. TSA (\-Algorithm\)

3. Priorité mappage \(-Priority\)

### <a name="remove-a-traffic-class"></a>Supprimer une classe de trafic

Vous pouvez utiliser le **Remove-NetQosTrafficClass** commande pour supprimer une classe de trafic.

>[!IMPORTANT]
>Vous ne pouvez pas supprimer la classe de trafic par défaut.


    Remove-NetQosTrafficClass -Name SMB

Vous pouvez ensuite utiliser le **Get-NetQosTrafficClass** commande pour afficher les paramètres.
    
    Get-NetQosTrafficClass
    
    NameAlgorithm Bandwidth(%) Priority PolicySetIfIndex IfAlias
    ------------- ------------ -------- ---------------- -------
    [Default]   ETS   100  0-7  Global
    

Une fois que vous supprimez une classe de trafic, la valeur 802 .1p mappé à ce que la classe de trafic est remappé à la classe de trafic par défaut. Toute la bande passante qui a été réservée pour une classe de trafic est renvoyée à l’allocation de classe de trafic par défaut lors de la classe de trafic est supprimée.

## <a name="per-network-interface-policies"></a>Stratégies d’Interface réseau

Tous les exemples ci-dessus de définir des stratégies globales. Voici des exemples de la façon dont vous pouvez définir et obtenir les stratégies par cartes réseau. 

Le champ «PolicySet» remplace Global AdapterSpecific. Lorsque les stratégies AdapterSpecific sont affichées, les \(ifIndex\) Index d’Interface et le nom de l’Interface \(ifAlias\) sont également affichés.

```
PS C:\> Get-NetQosTrafficClass

Name        Algorithm Bandwidth(%) Priority         PolicySet        IfIndex IfAlias
----        --------- ------------ --------         ---------        ------- -------
[Default]   ETS       100          0-7              Global


PS C:\> Get-NetQosTrafficClass -InterfaceAlias M1

Name        Algorithm Bandwidth(%) Priority         PolicySet        IfIndex IfAlias
----        --------- ------------ --------         ---------        ------- -------
[Default]   ETS       100          0-7              AdapterSpecific  4       M1


PS C:\> New-NetQosTrafficClass -Name SMBGlobal -BandwidthPercentage 30 -Priority 4 -Algorithm ETS

Name        Algorithm Bandwidth(%) Priority         PolicySet        IfIndex IfAlias
----        --------- ------------ --------         ---------        ------- -------
SMBGlobal   ETS       30           4                Global


PS C:\> New-NetQosTrafficClass -Name SMBforM1 -BandwidthPercentage 30 -Priority 4 -Algorithm ETS -Interfac


Name        Algorithm Bandwidth(%) Priority         PolicySet        IfIndex IfAlias
----        --------- ------------ --------         ---------        ------- -------
SMBforM1    ETS       30           4                AdapterSpecific  4       M1


PS C:\> Get-NetQosTrafficClass

Name        Algorithm Bandwidth(%) Priority         PolicySet        IfIndex IfAlias
----        --------- ------------ --------         ---------        ------- -------
[Default]   ETS       70           0-3,5-7          Global
SMBGlobal   ETS       30           4                Global


PS C:\> Get-NetQosTrafficClass -InterfaceAlias M1

Name        Algorithm Bandwidth(%) Priority         PolicySet        IfIndex IfAlias
----        --------- ------------ --------         ---------        ------- -------
[Default]   ETS       70           0-3,5-7          AdapterSpecific  4       M1
SMBforM1    ETS       30           4                AdapterSpecific  4       M1


```

## <a name="priority-flow-control-settings"></a>Paramètres de contrôle de flux de priorité:

Voici des exemples de commande pour les paramètres de contrôle de flux de priorité. Ces paramètres peuvent également être spécifiés pour les cartes individuels.

### <a name="enable-and-display-priority-flow-control-for-global-and-interface-specific-use-cases"></a>Activer et contrôle de flux affichage priorité pour globaux et spécifiques d’Interface cas d’utilisation

```
PS C:\> Enable-NetQosFlowControl -Priority 4
PS C:\> Enable-NetQosFlowControl -Priority 3 -InterfaceAlias M1
PS C:\> Get-NetQosFlowControl

Priority   Enabled    PolicySet        IfIndex IfAlias
--------   -------    ---------        ------- -------
0          False      Global
1          False      Global
2          False      Global
3          False      Global
4          True       Global
5          False      Global
6          False      Global
7          False      Global

PS C:\> Get-NetQosFlowControl -InterfaceAlias M1

Priority   Enabled    PolicySet        IfIndex IfAlias
--------   -------    ---------        ------- -------
0          False      AdapterSpecific  4       M1
1          False      AdapterSpecific  4       M1
2          False      AdapterSpecific  4       M1
3          True       AdapterSpecific  4       M1
4          False      AdapterSpecific  4       M1
5          False      AdapterSpecific  4       M1
6          False      AdapterSpecific  4       M1
7          False      AdapterSpecific  4       M1  

```


### <a name="disable-priority-flow-control-global-and-interface-specific"></a>Désactiver le contrôle de flux prioritaire (globaux et spécifiques à une Interface)

```
PS C:\> Disable-NetQosFlowControl -Priority 4
PS C:\> Disable-NetQosFlowControl -Priority 3 -InterfaceAlias m1
PS C:\> Get-NetQosFlowControl

Priority   Enabled    PolicySet        IfIndex IfAlias
--------   -------    ---------        ------- -------
0          False      Global
1          False      Global
2          False      Global
3          False      Global
4          False      Global
5          False      Global
6          False      Global
7          False      Global


PS C:\> Get-NetQosFlowControl -InterfaceAlias M1

Priority   Enabled    PolicySet        IfIndex IfAlias
--------   -------    ---------        ------- -------
0          False      AdapterSpecific  4       M1
1          False      AdapterSpecific  4       M1
2          False      AdapterSpecific  4       M1
3          False      AdapterSpecific  4       M1
4          False      AdapterSpecific  4       M1
5          False      AdapterSpecific  4       M1
6          False      AdapterSpecific  4       M1
7          False      AdapterSpecific  4       M1  

```

##  <a name="application-priority-assignment"></a>Affectation de priorité d’application

Voici des exemples de l’attribution de priorité.

### <a name="create-qos-policy"></a>Créer une stratégie de QoS

```
PS C:\> New-NetQosPolicy -Name "SMB Policy" -PriorityValue8021Action 4

Name           : SMB Policy
Owner          : Group Policy (Machine)
NetworkProfile : All
Precedence     : 127
JobObject      :
PriorityValue  : 4

```

La commande précédente crée une nouvelle stratégie pour SMB. – SMB est un filtre de boîte de réception qui correspond au port TCP 445 (réservé pour SMB). Si un paquet est envoyé au port TCP 445, qu'il sera indiqué par le système d’exploitation avec une valeur 802 .1p 4 avant le paquet est transmis à un pilote de miniport réseau.

En plus: SMB, d’autres filtres par défaut incluent – iSCSI (port TCP correspondant 3260), - NFS (port TCP correspondant 2049), - migration dynamique (correspondant le port TCP6600), - FCOE (correspondance EtherType 0x8906) et – NetworkDirect.

NetworkDirect est une couche abstraite que nous créons sur n’importe quelle implémentation RDMA sur une carte réseau. NetworkDirect – doit être suivi d’un port réseau Direct.

Outre les filtres par défaut, vous pouvez classer le trafic par le nom du fichier exécutable de l’application (comme dans le premier exemple ci-dessous), ou l’adresse IP, de port ou de protocole (comme illustré dans le deuxième exemple):

**Par nom d’exécutable**

```
PS C:\> New-NetQosPolicy -Name background -AppPathNameMatchCondition "C:\Program files (x86)\backup.exe" -PriorityValue8021Action 1

Name           : background
Owner          : Group Policy (Machine)
NetworkProfile : All
Precedence     : 127
AppPathName    : C:\Program files (x86)\backup.exe
JobObject      :
PriorityValue  : 1

```


**Par adresse IP port ou protocole**

```
PS C:\> New-NetQosPolicy -Name "Network Management" -IPDstPrefixMatchCondition 10.240.1.0/24 -IPProtocolMatchCondition both -NetworkProfile all -PriorityValue8021Action 7

Name           : Network Management
Owner          : Group Policy (Machine)
NetworkProfile : All
Precedence     : 127
JobObject      :
IPProtocol     : Both
IPDstPrefix    : 10.240.1.0/24
PriorityValue  : 7

```

### <a name="display-qos-policy"></a>Afficher la stratégie de QoS

```
PS C:\> Get-NetQosPolicy

Name           : background
Owner          : Group Policy (Machine)
NetworkProfile : All
Precedence     : 127
AppPathName    : C:\Program files (x86)\backup.exe
JobObject      :
PriorityValue  : 1

Name           : Network Management
Owner          : Group Policy (Machine)
NetworkProfile : All
Precedence     : 127
JobObject      :
IPProtocol     : Both
IPDstPrefix    : 10.240.1.0/24
PriorityValue  : 7

Name           : SMB Policy
Owner          : Group Policy (Machine)
NetworkProfile : All
Precedence     : 127
JobObject      :
PriorityValue  : 4

```

### <a name="modify-qos-policy"></a>Modifier la stratégie de QoS

Vous pouvez modifier les stratégies de QoS comme illustré ci-dessous.


```
PS C:\> Set-NetQosPolicy -Name "Network Management" -IPSrcPrefixMatchCondition 10.235.2.0/24 -IPProtocolMatchCondition both -PriorityValue8021Action 7
PS C:\> Get-NetQosPolicy

Name           : Network Management
Owner          : Group Policy (Machine)
NetworkProfile : All
Precedence     : 127
JobObject      :
IPProtocol     : Both
IPSrcPrefix    : 10.235.2.0/24
IPDstPrefix    : 10.240.1.0/24
PriorityValue  : 7


```

### <a name="remove-qos-policy"></a>Supprimer la stratégie de QoS

```
PS C:\> Remove-NetQosPolicy -Name "Network Management"

Confirm
Are you sure you want to perform this action?
Remove-NetQosPolicy -Name "Network Management" -Store GPO:localhost
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "Y"): y  

```

## <a name="dcb-configuration-on-network-adapters"></a>Configuration de DCB sur les cartes réseau

Configuration de DCB sur les cartes réseau est indépendante de configuration DCB au niveau du système décrit ci-dessus. 

Quelle que soit la que DCB est installé dans Windows Server2016, vous pouvez toujours exécuter les commandes suivantes. 

Si vous configurez DCB à partir d’un commutateur et s’appuient sur DCBX pour propager les configurations de cartes réseau, vous pouvez examiner les configurations sont reçues et appliquées sur les cartes réseau à partir du côté du système d’exploitation, une fois que vous activez DCB sur les cartes réseau.

###  <a name="bkmk_enabledcb"></a>Activer et afficher les paramètres de DCB sur les cartes réseau

```
PS C:\> Enable-NetAdapterQos M1
PS C:\> Get-NetAdapterQos

Name                       : M1
Enabled                    : True
Capabilities               :                       Hardware     Current
                                                   --------     -------
                             MacSecBypass        : NotSupported NotSupported
                             DcbxSupport         : None         None
                             NumTCs(Max/ETS/PFC) : 8/8/8        8/8/8

OperationalTrafficClasses  : TC TSA    Bandwidth Priorities
                             -- ---    --------- ----------
                              0 ETS    70%       0-3,5-7
                              1 ETS    30%       4

OperationalFlowControl     : All Priorities Disabled
OperationalClassifications : Protocol  Port/Type Priority
                             --------  --------- --------
                             Default             1


```

### <a name="disable-dcb-on-network-adapters"></a>Désactiver DCB sur les cartes réseau

```
PS C:\> Disable-NetAdapterQos M1
PS C:\> Get-NetAdapterQos M1

Name         : M1
Enabled      : False
Capabilities :                       Hardware     Current
                                     --------     -------
               MacSecBypass        : NotSupported NotSupported
               DcbxSupport         : None         None
               NumTCs(Max/ETS/PFC) : 8/8/8        0/0/0  

```
## <a name="bkmk_wps"></a>Commandes Windows PowerShell pour DCB

Il existe des commandes DCB Windows PowerShell pour Windows Server2016 et Windows Server2012R2. Vous pouvez utiliser toutes les commandes pour Windows Server2012R2 dans Windows Server2016.

### <a name="windows-server-2016-windows-powershell-commands-for-dcb"></a>Windows Server2016les commandes Windows PowerShell pour DCB

La rubrique suivante pour Windows Server2016 fournit une applet de commande Windows PowerShell et leur syntaxe pour tous les \(DCB\) Data Center Bridging qualité de Service \ applets de commande \-specific (QoS\). Elle répertorie les applets de commande dans l’ordre alphabétique en fonction du verbe au début de l’applet de commande.

- [Module DcbQoS](https://technet.microsoft.com/itpro/powershell/windows/dcbqos/dcbqos)

### <a name="windows-server-2012-r2-windows-powershell-commands-for-dcb"></a>Windows Server2012R2 les commandes Windows PowerShell pour DCB

La rubrique suivante pour Windows Server2012R2 fournit une applet de commande Windows PowerShell et leur syntaxe pour tous les \(DCB\) Data Center Bridging qualité de Service \ applets de commande \-specific (QoS\). Elle répertorie les applets de commande dans l’ordre alphabétique en fonction du verbe au début de l’applet de commande.

- [Data Center Bridging (DCB) de qualité de Service (QoS) des applets de commande dans Windows PowerShell](https://technet.microsoft.com/library/hh967440.aspx)
