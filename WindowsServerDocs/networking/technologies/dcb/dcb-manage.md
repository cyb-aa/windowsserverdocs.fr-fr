---
title: Gérer Data Center Bridging (DCB)
description: Cette rubrique fournit des instructions sur l’utilisation des commandes Windows PowerShell pour gérer le pontage des centres de données dans Windows Server 2016.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 1575cc7c-62a7-4add-8f78-e5d93effe93f
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d635f96516040fcb30504f752c8194b0323c63f3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405778"
---
# <a name="manage-data-center-bridging-dcb"></a>Gérer Data Center Bridging (DCB)

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Cette rubrique fournit des instructions sur l’utilisation des commandes Windows PowerShell pour configurer Data Center Bridging \(DCB\) sur une carte\-réseau compatible DCB installée sur un ordinateur exécutant l’un des deux Windows Server 2016 ou Windows 10.

## <a name="install-dcb-in-windows-server-2016-or-windows-10"></a>Installer DCB dans Windows Server 2016 ou Windows 10

Pour plus d’informations sur les conditions préalables à l’utilisation de et sur l’installation de DCB, consultez [installer le pontage de centre de données (DCB) dans Windows Server 2016 ou Windows 10](dcb-install.md).


## <a name="dcb-configurations"></a>Configurations DCB 

Avant Windows Server 2016, toute la configuration DCB était appliquée universellement à toutes les cartes réseau qui prenait en charge DCB. 

Dans Windows Server 2016, vous pouvez appliquer des configurations DCB au magasin de stratégies global ou à des\(savesets\)de stratégie individuels. Lorsque des stratégies individuelles sont appliquées, elles remplacent tous les paramètres de stratégie globaux.

Les configurations de la classe de trafic, PFC et l’attribution de priorité d’application au niveau du système ne sont pas appliquées sur les cartes réseau tant que vous n’effectuez pas les opérations suivantes.

1. Transformez le bit prêt DCBX en false

2. Activez DCB sur les cartes réseau. Consultez [activer et afficher les paramètres DCB sur les cartes réseau](#bkmk_enabledcb).

>[!NOTE]
>Si vous souhaitez configurer DCB à partir d’un commutateur via DCBX, consultez [paramètres DCBX](#dcb-configuration-on-network-adapters).

Le bit DCBX consentant est décrit dans la spécification DCB. Si le bit prêt sur un appareil est défini sur true, l’appareil est disposé à accepter des configurations à partir d’un périphérique distant via DCBX. Si le bit prêt sur un appareil est défini sur false, l’appareil rejettera toutes les tentatives de configuration des appareils distants et appliquera uniquement les configurations locales.

Si DCB n’est pas installé dans Windows Server 2016, la valeur du bit consentant n’est pas pertinente en ce qui concerne le système d’exploitation, car le système d’exploitation n’a pas de paramètres locaux s’appliquent aux cartes réseau. Après l’installation de DCB, la valeur par défaut du bit consentant est true. Cette conception permet aux cartes réseau de conserver les configurations qu’elles ont pu recevoir de leurs homologues distants.

Si une carte réseau ne prend pas en charge DCBX, elle ne recevra jamais de configurations à partir d’un appareil distant. Il reçoit les configurations du système d’exploitation, mais uniquement après que le bit DCBX consentant a la valeur false.

## <a name="set-the-willing-bit"></a>Définir le bit disposé

Pour appliquer les configurations du système d’exploitation de la classe de trafic, PFC et l’affectation de priorité d’application sur les cartes réseau, ou pour remplacer simplement les configurations des appareils distants \ — si vous en avez, vous pouvez exécuter la commande suivante.

>[!NOTE]
>Les noms de commandes Windows PowerShell DCB incluent « QoS » au lieu de « DCB » dans la chaîne de nom. Cela est dû au fait que QoS et DCB sont intégrés dans Windows Server 2016 pour offrir une expérience de gestion de QoS transparente.

    
    Set-NetQosDcbxSetting -Willing $FALSE
    
    Confirm
    Are you sure you want to perform this action?
    Set-NetQosDcbxSetting -Willing $false
    [Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "Y"):
    

Pour afficher l’état du paramètre de bit consentant, vous pouvez utiliser la commande suivante :

    
    Get-NetQosDcbxSetting
    
    Willing PolicySetIfIndex IfAlias
    ------- ---------------- -------
    False   Global  
    

## <a name="dcb-configuration-on-network-adapters"></a>Configuration de DCB sur les cartes réseau

L’activation de DCB sur une carte réseau vous permet de voir la configuration propagée d’un commutateur à la carte réseau.

Les configurations DCB incluent les étapes suivantes.

1.  Configurez les paramètres DCB au niveau du système, notamment :

    a. Gestion de la classe du trafic
    
    b. Paramètres du contrôle de workflow de priorité (PFC)
    
    c. Affectation de priorité d’application
    
    d. Paramètres DCBX

2. Configurez DCB sur la carte réseau.



##  <a name="dcb-traffic-class-management"></a>Gestion de la classe de trafic DCB

Voici des exemples de commandes Windows PowerShell pour la gestion de classe de trafic.

### <a name="create-a-traffic-class"></a>Créer une classe de trafic

Vous pouvez utiliser la commande **New-NetQosTrafficClass** pour créer une classe de trafic.

    
    New-NetQosTrafficClass -Name SMB -Priority 4 -BandwidthPercentage 30 -Algorithm ETS
    
    Name Algorithm Bandwidth(%) Priority PolicySetIfIndex IfAlias
    ---- --------- ------------ -------- ---------------- -------
    SMB  ETS   30   4Global
      

Par défaut, toutes les valeurs p 802.1 sont mappées à une classe de trafic par défaut, qui a 100% de la bande passante de la liaison physique. La commande **New-NetQosTrafficClass** crée une nouvelle classe de trafic, à laquelle tout paquet balisé avec 802.1 p Priority value 4 est mappé. L’algorithme de sélection de transmission \(TSA @ no__t-1 est ETS et a 30% de la bande passante.

Vous pouvez créer jusqu’à 7 nouvelles classes de trafic. En incluant la classe de trafic par défaut, il peut y avoir au maximum 8 classes de trafic dans le système. Toutefois, une carte réseau compatible DCB peut ne pas prendre en charge de nombreuses classes de trafic dans le matériel. Si vous créez plusieurs classes de trafic qui peuvent être prises en charge sur une carte réseau et que vous activez DCB sur cette carte réseau, le pilote de miniport signale une erreur au système d’exploitation. L’erreur est consignée dans le journal des événements.

La somme des réservations de bande passante pour toutes les classes de trafic créées ne peut pas dépasser 99% de la bande passante. La classe de trafic par défaut a toujours au moins 1% de la bande passante réservée pour elle-même.

### <a name="display-traffic-classes"></a>Afficher les classes de trafic

Vous pouvez utiliser la commande **NetQosTrafficClass** pour afficher les classes de trafic.

    Get-NetQosTrafficClass
    
    NameAlgorithm Bandwidth(%) Priority PolicySetIfIndex IfAlias
    ------------- ------------ -------- ---------------- -------
    [Default]   ETS   70   0-3,5-7  Global
    SMB ETS   30   4Global  
    
### <a name="modify-a-traffic-class"></a>Modifier une classe de trafic

Vous pouvez utiliser la commande **Set-NetQosTrafficClass** pour créer une classe de trafic. 

    Set-NetQosTrafficClass -Name SMB -BandwidthPercentage 50

Vous pouvez ensuite utiliser la commande **NetQosTrafficClass** pour afficher les paramètres.

    Get-NetQosTrafficClass
    
    NameAlgorithm Bandwidth(%) Priority PolicySetIfIndex IfAlias
    ------------- ------------ -------- ---------------- -------
    [Default]   ETS   50   0-3,5-7  Global
    SMB ETS   50   4Global   
    

Après avoir créé une classe de trafic, vous pouvez modifier ses paramètres indépendamment. Les paramètres que vous pouvez modifier sont les suivants :

1. Allocation \(de bande passante-BandwidthPercentage\)

2. TSA (\-algorithme\)

3. Mappage \(de priorité-priorité\)

### <a name="remove-a-traffic-class"></a>Supprimer une classe de trafic

Vous pouvez utiliser la commande **Remove-NetQosTrafficClass** pour supprimer une classe de trafic.

>[!IMPORTANT]
>Vous ne pouvez pas supprimer la classe de trafic par défaut.


    Remove-NetQosTrafficClass -Name SMB

Vous pouvez ensuite utiliser la commande **NetQosTrafficClass** pour afficher les paramètres.
    
    Get-NetQosTrafficClass
    
    NameAlgorithm Bandwidth(%) Priority PolicySetIfIndex IfAlias
    ------------- ------------ -------- ---------------- -------
    [Default]   ETS   100  0-7  Global
    

Une fois que vous avez supprimé une classe de trafic, la valeur 802.1 p mappée à cette classe de trafic est remappée à la classe de trafic par défaut. Toute bande passante réservée pour une classe de trafic est retournée à l’allocation de classe de trafic par défaut lorsque la classe de trafic est supprimée.

## <a name="per-network-interface-policies"></a>Stratégies d’interface par réseau

Tous les exemples ci-dessus définissent des stratégies globales. Vous trouverez ci-dessous des exemples de la façon dont vous pouvez définir et récupérer des stratégies par carte réseau. 

Le champ « PolicySet » passe de global à AdapterSpecific. Lorsque les stratégies AdapterSpecific sont affichées, les ifIndex \(\) d’index d’interface \(et\) de nom d’interface ifAlias sont également affichés.

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

## <a name="priority-flow-control-settings"></a>Paramètres de contrôle de workflow de priorité :

Voici des exemples de commandes pour les paramètres de contrôle de Flow Priority. Ces paramètres peuvent également être spécifiés pour des adaptateurs individuels.

### <a name="enable-and-display-priority-flow-control-for-global-and-interface-specific-use-cases"></a>Activer et afficher le contrôle de workflow de priorité pour les cas d’usage globaux et spécifiques à l’interface

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


### <a name="disable-priority-flow-control-global-and-interface-specific"></a>Désactiver le contrôle de workflow de priorité (global et spécifique à l’interface)

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

Voici des exemples d’affectation de priorité.

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

La commande précédente crée une nouvelle stratégie pour SMB. – SMB est un filtre de boîte de réception qui correspond au port TCP 445 (réservé pour SMB). Si un paquet est envoyé au port TCP 445, il est marqué par le système d’exploitation avec la valeur p 802.1 p de 4 avant que le paquet soit transmis à un pilote de Miniport réseau.

En plus de – SMB, les autres filtres par défaut incluent : iSCSI (port TCP 3260 correspondant),-NFS (port TCP 2049 correspondant),-LiveMigration (port TCP 6600 correspondant),-FCOE (correspondance EtherType 0x8906) et – NetworkDirect.

NetworkDirect est une couche abstraite que nous créons en plus de toute implémentation RDMA sur une carte réseau. – NetworkDirect doit être suivi d’un port direct du réseau.

En plus des filtres par défaut, vous pouvez classifier le trafic en fonction du nom de l’exécutable de l’application (comme dans le premier exemple ci-dessous) ou de l’adresse IP, du port ou du protocole (comme indiqué dans le deuxième exemple) :

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


**Par port ou protocole d’adresse IP**

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

### <a name="display-qos-policy"></a>Afficher la stratégie QoS

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

### <a name="modify-qos-policy"></a>Modifier la stratégie QoS

Vous pouvez modifier les stratégies QoS comme indiqué ci-dessous.


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

### <a name="remove-qos-policy"></a>Supprimer la stratégie QoS

```
PS C:\> Remove-NetQosPolicy -Name "Network Management"

Confirm
Are you sure you want to perform this action?
Remove-NetQosPolicy -Name "Network Management" -Store GPO:localhost
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "Y"): y  

```

## <a name="dcb-configuration-on-network-adapters"></a>Configuration de DCB sur les cartes réseau

La configuration DCB sur les cartes réseau est indépendante de la configuration DCB au niveau du système décrit ci-dessus. 

Que DCB soit installé ou non dans Windows Server 2016, vous pouvez toujours exécuter les commandes suivantes. 

Si vous configurez DCB à partir d’un commutateur et que vous vous fiez à DCBX pour propager les configurations aux cartes réseau, vous pouvez examiner les configurations reçues et appliquées sur les cartes réseau du système d’exploitation, après avoir activé DCB sur les cartes réseau.

###  <a name="bkmk_enabledcb"></a>Activer et afficher les paramètres DCB sur les cartes réseau

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

Il existe des commandes Windows PowerShell DCB pour Windows Server 2016 et Windows Server 2012 R2. Vous pouvez utiliser toutes les commandes pour Windows Server 2012 R2 dans Windows Server 2016.

### <a name="windows-server-2016-windows-powershell-commands-for-dcb"></a>Commandes Windows PowerShell pour DCB Windows Server 2016

La rubrique suivante pour Windows Server 2016 fournit des descriptions et la syntaxe des applets de commande Windows \(PowerShell pour l’ensemble \(des\)applets de commande spécifiques à la\) qualité de service (QoS\-) de Data Center Bridg. Elle répertorie les applets de commande par ordre alphabétique en fonction du verbe situé au début de l’applet de commande.

- [Module DcbQoS](https://technet.microsoft.com/itpro/powershell/windows/dcbqos/dcbqos)

### <a name="windows-server-2012-r2-windows-powershell-commands-for-dcb"></a>Commandes Windows PowerShell pour DCB Windows Server 2012 R2

La rubrique suivante pour Windows Server 2012 R2 fournit des descriptions et la syntaxe des applets de commande Windows \(PowerShell pour l’ensemble \(des\)applets de commande spécifiques à la\) qualité de service (QoS\-) de Data Center Bridg. Elle répertorie les applets de commande par ordre alphabétique en fonction du verbe situé au début de l’applet de commande.

- [Applets de commande Quality of service (QoS) Data Center Bridging (QoS) dans Windows PowerShell](https://technet.microsoft.com/library/hh967440.aspx)
