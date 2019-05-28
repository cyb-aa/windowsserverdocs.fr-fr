---
title: Gérer Data Center Bridging (DCB)
description: Cette rubrique fournit des instructions sur l’utilisation des commandes Windows PowerShell pour gérer Data Center Bridging dans Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 1575cc7c-62a7-4add-8f78-e5d93effe93f
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: daed746fe798ae253956d0977827d0e205bb8b3e
ms.sourcegitcommit: 21165734a0f37c4cd702c275e85c9e7c42d6b3cb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2019
ms.locfileid: "65034577"
---
# <a name="manage-data-center-bridging-dcb"></a>Gérer Data Center Bridging (DCB)

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique fournit des instructions sur l’utilisation des commandes Windows PowerShell pour configurer un pontage de centre de données \(DCB\) sur un DCB\-carte réseau compatible est installé sur un ordinateur qui exécute Windows Server 2016 ou Windows 10.

## <a name="install-dcb-in-windows-server-2016-or-windows-10"></a>Installer DCB dans Windows Server 2016 ou Windows 10

Pour plus d’informations sur la configuration requise pour l’utilisation et comment installer DCB, consultez [installer Data Center Bridging (DCB) dans Windows Server 2016 ou Windows 10](dcb-install.md).


## <a name="dcb-configurations"></a>Configurations de DCB 

Avant Windows Server 2016, toutes les configurations de DCB a été appliquée universellement à toutes les cartes réseau prises en charge DCB. 

Dans Windows Server 2016, vous pouvez appliquer des configurations de DCB pour le Store de stratégie globale ou Store de stratégie individuels\(s\). Lorsque des stratégies sont appliquées, elles remplacent tous les paramètres de stratégie globale.

Les configurations de trafic classe, le PFC et application d’affectation de priorité au niveau du système n’est pas appliquée sur les cartes réseau jusqu'à ce que vous procédez comme suit.

1. Activer le bit de prêt DCBX sur false

2. Activez DCB sur les cartes réseau. Consultez [activer et afficher les paramètres de DCB sur les cartes réseau](#bkmk_enabledcb).

>[!NOTE]
>Si vous souhaitez configurer DCB à partir d’un commutateur via DCBX, consultez [les paramètres DCBX](#dcb-configuration-on-network-adapters).

Le bit de prêt DCBX est décrite dans la spécification de DCB. Si le bit disposé sur un appareil est défini sur true, l’appareil est prêt à accepter les configurations à partir d’un appareil à distance via DCBX. Si le bit disposé sur un appareil est défini sur false, l’appareil rejette toutes les tentatives de configuration à partir d’appareils à distance et appliquer uniquement les configurations locales.

Si DCB n’est pas installé dans Windows Server 2016 la valeur du bit prêt est sans importance autant que le système d’exploitation est concerné, car le système d’exploitation n’a aucun paramètre local s’appliquent aux cartes réseau. Une fois DCB est installé, la valeur par défaut du prêt bit a la valeur true. Cette conception permet des cartes réseau pour conserver les configurations peuvent avoir reçu de leurs homologues distants.

Si une carte réseau ne prend pas en charge DCBX, il recevra jamais des configurations d’un périphérique distant. Il reçoit les configurations du système d’exploitation, mais uniquement après la DCBX disposé bit est défini sur false.

## <a name="set-the-willing-bit"></a>Définir le bit de prêt

Pour appliquer des configurations de système d’exploitation de la classe de trafic, le PFC et affectation de priorité d’application sur les cartes réseau, ou pour simplement remplacer les configurations d’appareils distants \, le cas échéant \ — vous pouvez exécuter la commande suivante.

>[!NOTE]
>Les noms de commande PowerShell de Windows DCB incluent « QoS » au lieu de « DCB » dans la chaîne de nom. Il s’agit, car la qualité de service et DCB sont intégrés dans Windows Server 2016 pour fournir une expérience de gestion de QoS transparente.

    
    Set-NetQosDcbxSetting -Willing $FALSE
    
    Confirm
    Are you sure you want to perform this action?
    Set-NetQosDcbxSetting -Willing $false
    [Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "Y"):
    

Pour afficher l’état du bit disposé définition, vous pouvez utiliser la commande suivante :

    
    Get-NetQosDcbxSetting
    
    Willing PolicySetIfIndex IfAlias
    ------- ---------------- -------
    False   Global  
    

## <a name="dcb-configuration-on-network-adapters"></a>Configuration de DCB sur les cartes réseau

L’activation de DCB sur une carte réseau vous permet de voir la configuration propagée à partir d’un commutateur à la carte réseau.

Configurations de DCB comprennent les étapes suivantes.

1.  Configurer les paramètres de DCB au niveau du système, ce qui inclut :

    a. Gestion de classe de trafic
    
    b. Paramètres de contrôle (PFC) de flux de priorité
    
    c. Attribution de priorité d’application
    
    d. Paramètres de DCBX

2. Configurer DCB sur la carte réseau.



##  <a name="dcb-traffic-class-management"></a>Gestion de la classe de trafic DCB

Voici des exemples de commandes Windows PowerShell pour la gestion de classe de trafic.

### <a name="create-a-traffic-class"></a>Créer une classe de trafic

Vous pouvez utiliser la **New-NetQosTrafficClass** commande pour créer une classe de trafic.

    
    New-NetQosTrafficClass -Name SMB -Priority 4 -BandwidthPercentage 30 -Algorithm ETS
    
    Name Algorithm Bandwidth(%) Priority PolicySetIfIndex IfAlias
    ---- --------- ------------ -------- ---------------- -------
    SMB  ETS   30   4Global
      

Par défaut, toutes les valeurs de 802 .1p sont mappées à une classe de trafic par défaut, qui a 100 % de la bande passante du lien physique. Le **New-NetQosTrafficClass** commande crée une nouvelle classe de trafic, à qui un paquet est marqué avec la priorité 802 .1p valeur 4 est mappé. L’algorithme de sélection de Transmission \(TSA\) est ETS et a 30 % de la bande passante.

Vous pouvez créer jusqu'à 7 nouvelles classes de trafic. Y compris la classe de trafic par défaut, il peut y avoir au maximum 8 classes de trafic dans le système. Toutefois, une carte de réseau compatibles DCB peut prend pas en charge plusieurs classes dans le matériel de trafic. Si vous créez plusieurs classes de trafic que peut être placé sur une carte réseau et que vous activez DCB sur cette carte réseau, le pilote de miniport signale une erreur du système d’exploitation. L’erreur est enregistrée dans le journal des événements.

La somme des réservations de bande passante pour toutes les classes de trafic créé ne peut pas dépasser 99 % de la bande passante. La classe de trafic par défaut a toujours au moins 1 % de la bande passante réservée pour lui-même.

### <a name="display-traffic-classes"></a>Afficher les Classes de trafic

Vous pouvez utiliser la **Get-NetQosTrafficClass** commande pour afficher les classes de trafic.

    Get-NetQosTrafficClass
    
    NameAlgorithm Bandwidth(%) Priority PolicySetIfIndex IfAlias
    ------------- ------------ -------- ---------------- -------
    [Default]   ETS   70   0-3,5-7  Global
    SMB ETS   30   4Global  
    
### <a name="modify-a-traffic-class"></a>Modifier une classe de trafic

Vous pouvez utiliser la **Set-NetQosTrafficClass** commande pour créer une classe de trafic. 

    Set-NetQosTrafficClass -Name SMB -BandwidthPercentage 50

Vous pouvez ensuite utiliser le **Get-NetQosTrafficClass** commande pour afficher les paramètres.

    Get-NetQosTrafficClass
    
    NameAlgorithm Bandwidth(%) Priority PolicySetIfIndex IfAlias
    ------------- ------------ -------- ---------------- -------
    [Default]   ETS   50   0-3,5-7  Global
    SMB ETS   50   4Global   
    

Après avoir créé une classe de trafic, vous pouvez modifier ses paramètres de manière indépendante. Les paramètres que vous pouvez modifier sont les suivantes :

1. Allocation de bande passante \(- BandwidthPercentage\)

2. TSA (\-algorithme\)

3. Mappage de priorité \(-priorité\)

### <a name="remove-a-traffic-class"></a>Supprimer une classe de trafic

Vous pouvez utiliser la **Remove-NetQosTrafficClass** commande pour supprimer une classe de trafic.

>[!IMPORTANT]
>Vous ne pouvez pas supprimer la classe de trafic par défaut.


    Remove-NetQosTrafficClass -Name SMB

Vous pouvez ensuite utiliser le **Get-NetQosTrafficClass** commande pour afficher les paramètres.
    
    Get-NetQosTrafficClass
    
    NameAlgorithm Bandwidth(%) Priority PolicySetIfIndex IfAlias
    ------------- ------------ -------- ---------------- -------
    [Default]   ETS   100  0-7  Global
    

Après avoir supprimé une classe de trafic, la valeur 802 .1p mappé à ce que la classe de trafic est remappé à la classe de trafic par défaut. Toute la bande passante réservée pour une classe de trafic est renvoyée à l’allocation de classe de trafic par défaut lors de la classe de trafic est supprimée.

## <a name="per-network-interface-policies"></a>Stratégies d’Interface réseau

Tous les exemples ci-dessus définir des stratégies globales. Voici quelques exemples de la façon dont vous pouvez définir et obtenir les stratégies par cartes d’interface réseau. 

Le champ « PolicySet » remplace Global à AdapterSpecific. Lorsque les stratégies AdapterSpecific sont affichés, l’Index d’Interface \(ifIndex\) et le nom de l’Interface \(ifAlias\) sont également affichés.

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

## <a name="priority-flow-control-settings"></a>Paramètres de contrôle de flux de priorité :

Voici des exemples de commandes pour les paramètres de contrôle de flux de priorité. Ces paramètres peuvent également être spécifiés pour les adaptateurs individuels.

### <a name="enable-and-display-priority-flow-control-for-global-and-interface-specific-use-cases"></a>Cas d’usage activer et afficher le contrôle de flux de priorité pour Global et d’Interface spécifique

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

##  <a name="application-priority-assignment"></a>Attribution de priorité d’application

Voici quelques exemples d’affectation de priorité.

### <a name="create-qos-policy"></a>Créer une stratégie QoS

```
PS C:\> New-NetQosPolicy -Name "SMB Policy" -PriorityValue8021Action 4

Name           : SMB Policy
Owner          : Group Policy (Machine)
NetworkProfile : All
Precedence     : 127
JobObject      :
PriorityValue  : 4

```

La commande précédente crée une nouvelle stratégie pour SMB. – SMB est un filtre de la boîte de réception qui correspond au port TCP 445 (réservé pour SMB). Si un paquet est envoyé vers le port TCP 445, qu'il sera indiqué par le système d’exploitation avec une valeur 802 .1p sur 4 avant de transmettre le paquet à un pilote de miniport de réseau.

En plus de – SMB, autres filtres par défaut incluent – iSCSI (port TCP correspondant 3260), - NFS (port TCP correspondant 2049), - LiveMigration (correspondance le port TCP 6600), - FCOE (correspondance EtherType 0x8906) et – NetworkDirect.

NetworkDirect est une couche abstraite que nous créons sur n’importe quelle implémentation RDMA sur une carte réseau. NetworkDirect – doit être suivie d’un port réseau directe.

En plus des filtres par défaut, vous pouvez classer le trafic par nom de l’exécutable de l’application (comme dans le premier exemple ci-dessous), ou par adresse IP, port ou protocole (comme indiqué dans le deuxième exemple) :

**Par nom de l’exécutable**

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

Vous pouvez modifier les stratégies de QoS comme indiqué ci-dessous.


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

Configuration de DCB sur les cartes réseau est indépendante de configuration DCB au niveau du système décrite ci-dessus. 

Quelle que soit la non DCB est installée dans Windows Server 2016, vous pouvez toujours exécuter les commandes suivantes. 

Si vous configurez DCB à partir d’un commutateur s’appuient sur DCBX pour propager les configurations à des cartes réseau, vous pouvez examiner les configurations sont reçues et appliquées sur les cartes réseau à partir de la partie du système d’exploitation une fois que vous activez DCB sur les cartes réseau.

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
## <a name="bkmk_wps"></a>Commandes PowerShell de Windows pour DCB

Il existe des commandes de DCB Windows PowerShell pour Windows Server 2016 et Windows Server 2012 R2. Vous pouvez utiliser toutes les commandes pour Windows Server 2012 R2 dans Windows Server 2016.

### <a name="windows-server-2016-windows-powershell-commands-for-dcb"></a>Commandes de Windows Server 2016 et Windows PowerShell pour DCB

La rubrique suivante pour Windows Server 2016 fournit les descriptions d’applet de commande Windows PowerShell et la syntaxe pour tous les Data Center Bridging \(DCB\) qualité de Service \(QoS\)\-applets de commande spécifique. Elle répertorie les applets de commande par ordre alphabétique en fonction du verbe situé au début de l’applet de commande.

- [DcbQoS Module](https://technet.microsoft.com/itpro/powershell/windows/dcbqos/dcbqos)

### <a name="windows-server-2012-r2-windows-powershell-commands-for-dcb"></a>Commandes Windows Server 2012 R2 Windows PowerShell pour DCB

La rubrique suivante pour Windows Server 2012 R2 fournit des descriptions d’applet de commande Windows PowerShell et la syntaxe pour tous les Data Center Bridging \(DCB\) qualité de Service \(QoS\)\-applets de commande spécifique. Elle répertorie les applets de commande par ordre alphabétique en fonction du verbe situé au début de l’applet de commande.

- [Data Center Bridging (DCB) de qualité de Service (QoS) des applets de commande dans Windows PowerShell](https://technet.microsoft.com/library/hh967440.aspx)
