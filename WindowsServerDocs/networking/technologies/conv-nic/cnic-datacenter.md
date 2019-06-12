---
title: Carte d’interface réseau convergé dans une configuration de la carte d’interface réseau associées (datacenter)
description: Dans cette rubrique, nous fournissons des instructions permettant de déployer la carte de réseau convergé dans une configuration de la carte d’interface réseau associées avec l’association de cartes SET (Switch Embedded).
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: f01546f8-c495-4055-8492-8806eee99862
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/17/2018
ms.openlocfilehash: 58c4483c092c30a892ea6bdde20794270340fa8e
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447026"
---
# <a name="converged-nic-in-a-teamed-nic-configuration-datacenter"></a>Carte d’interface réseau convergé dans une configuration de la carte d’interface réseau associées (datacenter)

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Dans cette rubrique, nous vous fournissons des instructions pour déployer la carte de réseau convergé dans une configuration de la carte d’interface réseau associées avec Switch Embedded Teaming \(définir\). 

L’exemple de configuration dans cette rubrique décrit deux hôtes Hyper-V, **hôte 1 Hyper-V** et **hôte 2 Hyper-V**. Les deux hôtes ont deux cartes réseau. Sur chaque hôte, une carte est connectée au sous-réseau 192.168.1.x/24, et une carte est connectée au sous-réseau 192.168.2.x/24.

![Ordinateurs hôtes Hyper-V](../../media/Converged-NIC/1-datacenter-test-conn.jpg)

## <a name="step-1-test-the-connectivity-between-source-and-destination"></a>Étape 1. Tester la connectivité entre la source et destination

Vérifiez que physique carte d’interface réseau peut se connecter à l’hôte de destination.  Ce test démontre la connectivité à l’aide de couche 3 \(L3\) - ou la couche IP - ainsi que la couche 2 \(L2\) réseaux locaux virtuels \(VLAN\).

1. Afficher les propriétés d’adaptateur réseau premier.

   ```PowerShell
   Get-NetAdapter -Name "Test-40G-1" | ft -AutoSize
   ```

   _**Résultats :** _


   |    Nom    |           InterfaceDescription           | ifIndex | État |    MacAddress     | LinkSpeed |
   |------------|------------------------------------------|---------|--------|-------------------|-----------|
   | Test-40G-1 | Carte Ethernet Pro Mellanox ConnectX-3 |   11    |   Up (Haut)   | E4-1D-2D-07-43-D0 |  40 Gbits/s  |

   ---

2. Afficher les propriétés supplémentaires pour la première carte, y compris l’adresse IP. 

   ```PowerShell
   Get-NetIPAddress -InterfaceAlias "Test-40G-1"
   Get-NetIPAddress -InterfaceAlias "TEST-40G-1" | Where-Object {$_.AddressFamily -eq "IPv4"} | fl InterfaceAlias,IPAddress
   ```

   _**Résultats :** _


   |   Paramètre    |    Value    |
   |----------------|-------------|
   |   AdresseIP    | 192.168.1.3 |
   | InterfaceIndex |     11      |
   | InterfaceAlias | Test-40G-1  |
   | AddressFamily  |    IPv4     |
   |      type      |   Monodiffusion   |
   |  PrefixLength  |     24      |

   ---

3. Afficher les propriétés d’adaptateur réseau deuxième.

   ```PowerShell
   Get-NetAdapter -Name "Test-40G-2" | ft -AutoSize
   ```

   _**Résultats :** _


   |    Nom    |          InterfaceDescription           | ifIndex | État |    MacAddress     | LinkSpeed |
   |------------|-----------------------------------------|---------|--------|-------------------|-----------|
   | TEST-40G-2 | Ethernet Pro Mellanox ConnectX-3 A... n ° 2 |   13    |   Up (Haut)   | E4-1D-2D-07-40-70 |  40 Gbits/s  |

   ---

4. Afficher les propriétés supplémentaires pour la deuxième carte, y compris l’adresse IP.

   ```PowerShell
   Get-NetIPAddress -InterfaceAlias "Test-40G-2"
   Get-NetIPAddress -InterfaceAlias "Test-40G-2" | Where-Object {$_.AddressFamily -eq "IPv4"} | fl InterfaceAlias,IPAddress
   ```

   _**Résultats :** _


   |   Paramètre    |    Value    |
   |----------------|-------------|
   |   AdresseIP    | 192.168.2.3 |
   | InterfaceIndex |     13      |
   | InterfaceAlias | TEST-40G-2  |
   | AddressFamily  |    IPv4     |
   |      type      |   Monodiffusion   |
   |  PrefixLength  |     24      |

   ---

5. Vérifiez que pNICs autres membre association de cartes réseau ou un ensemble dispose d’une adresse IP valide.<p>Utiliser un sous-réseau distinct, \(xxx.xxx. **2**.xxx vs xxx.xxx. **1**.xxx\), afin de faciliter l’envoi à partir de cet adaptateur à la destination. Sinon, si vous recherchez les deux pNICs sur le même sous-réseau, la charge de la pile TCP/IP de Windows équilibre entre les interfaces et une validation simple devient plus complexe.


## <a name="step-2-ensure-that-source-and-destination-can-communicate"></a>Étape 2. Vérifiez que la source et destination peuvent communiquer

Dans cette étape, nous utilisons le **Test-NetConnection** commande Windows PowerShell, mais si vous pouvez utiliser la **ping** commande si vous préférez. 

1. Vérifiez la communication bidirectionnelle.

   ```PowerShell
   Test-NetConnection 192.168.1.5
   ```

   _**Résultats :** _


   |        Paramètre         |    Value    |
   |--------------------------|-------------|
   |       ComputerName       | 192.168.1.5 |
   |      RemoteAddress       | 192.168.1.5 |
   |      InterfaceAlias      | Test-40G-1  |
   |      SourceAddress       | 192.168.1.3 |
   |      PingSucceeded       |    False    |
   | PingReplyDetails \(RTT\) |    0 ms     |

   ---

   Dans certains cas, vous devrez peut-être désactiver le pare-feu Windows avec fonctions avancées de sécurité pour effectuer ce test. Si vous désactivez le pare-feu, n’oubliez pas sécurité et vérifiez que votre configuration répond aux exigences de sécurité de votre organisation.

2. Désactiver tous les profils de pare-feu.

   ```PowerShell
   Set-NetFirewallProfile -All -Enabled False
   ```

3. Après avoir désactivé les profils de pare-feu, testez à nouveau la connexion. 

   ```PowerShell
   Test-NetConnection 192.168.1.5
   ```

   _**Résultats :** _


   |        Paramètre         |    Value    |
   |--------------------------|-------------|
   |       ComputerName       | 192.168.1.5 |
   |      RemoteAddress       | 192.168.1.5 |
   |      InterfaceAlias      | Test-40G-1  |
   |      SourceAddress       | 192.168.1.3 |
   |      PingSucceeded       |    False    |
   | PingReplyDetails \(RTT\) |    0 ms     |

   ---


4. Vérifiez la connectivité pour les cartes réseau supplémentaires. Répétez les étapes précédentes pour tous les pNICs suivants inclus dans l’équipe de la carte réseau ou un ensemble.

   ```PowerShell    
   Test-NetConnection 192.168.2.5
   ```

   _**Résultats :** _


   |        Paramètre         |    Value    |
   |--------------------------|-------------|
   |       ComputerName       | 192.168.2.5 |
   |      RemoteAddress       | 192.168.2.5 |
   |      InterfaceAlias      | Test-40G-2  |
   |      SourceAddress       | 192.168.2.3 |
   |      PingSucceeded       |    False    |
   | PingReplyDetails \(RTT\) |    0 ms     |

   ---

## <a name="step-3-configure-the-vlan-ids-for-nics-installed-in-your-hyper-v-hosts"></a>Étape 3. Configurer l’ID de VLAN pour les cartes réseau installées dans vos hôtes Hyper-V

De nombreuses configurations réseau utilisent des réseaux locaux virtuels, et si vous envisagez d’utiliser des réseaux locaux virtuels dans votre réseau, vous devez répéter le test précédent avec les réseaux locaux virtuels configurés.

Pour cette étape, les cartes réseau sont dans **accès** mode. Toutefois, lorsque vous créez un commutateur virtuel Hyper-V \(vSwitch\) plus loin dans ce guide, les propriétés de réseau local virtuel sont appliquées au niveau du port de commutateur virtuel. 

Comme un commutateur peut héberger plusieurs réseaux locaux virtuels, il est nécessaire pour le Top of Rack \(ToR\) commutateur physique pour que le port que l’hôte est connecté à configurée en mode Trunk.

>[!NOTE]
>Pour obtenir des instructions sur la façon de configurer le mode Trunk sur le commutateur, consultez la documentation de votre commutateur ToR.

L’illustration suivante montre deux hôtes Hyper-V avec deux cartes réseau chaque qui ont les VLAN 101 et 102 de réseau local virtuel configuré dans les propriétés de la carte réseau.

![Configurer des réseaux locaux virtuels](../../media/Converged-NIC/2-datacenter-configure-vlans.jpg)


>[!TIP]
>En fonction de l’Institute of Electrical and Electronics Engineers \(IEEE\) normes, la qualité de Service de mise en réseau \(QoS\) agissent de propriétés dans la carte réseau physique sur l’en-tête de 802.1 p est incorporé dans le 802. 1 q \(VLAN\) en-tête lorsque vous configurez le VLAN ID.

1. Configurer l’ID VLAN sur la première carte réseau, Test-40G-1.

   ```PowerShell    
   Set-NetAdapterAdvancedProperty -Name "Test-40G-1" -RegistryKeyword VlanID -RegistryValue "101"
   Get-NetAdapterAdvancedProperty -Name "Test-40G-1" | Where-Object {$_.RegistryKeyword -eq "VlanID"} | ft -AutoSize
   ```

   _**Résultats :** _   


   |    Nom    | DisplayName | DisplayValue | RegistryKeyword | RegistryValue |
   |------------|-------------|--------------|-----------------|---------------|
   | TEST-40G-1 |   ID du réseau local virtuel   |     101      |     VlanID      |     {101}     |

   ---

2. Redémarrez la carte réseau pour appliquer le VLAN ID.

   ```PowerShell
   Restart-NetAdapter -Name "Test-40G-1"
   ```

3. Vérifiez l’état est **des**.

   ```PowerShell
   Get-NetAdapter -Name "Test-40G-1" | ft -AutoSize
   ```

   _**Résultats :** _


   |    Nom    |          InterfaceDescription           | ifIndex | État |    MacAddress     | LinkSpeed |
   |------------|-----------------------------------------|---------|--------|-------------------|-----------|
   | Test-40G-1 | Ethernet Pro Mellanox ConnectX-3 Ada... |   11    |   Up (Haut)   | E4-1D-2D-07-43-D0 |  40 Gbits/s  |

   ---

4. Configurer l’ID VLAN sur la deuxième carte réseau, Test-40G-2.

   ```PowerShell    
   Set-NetAdapterAdvancedProperty -Name "Test-40G-2" -RegistryKeyword VlanID -RegistryValue "102"
   Get-NetAdapterAdvancedProperty -Name "Test-40G-2" | Where-Object {$_.RegistryKeyword -eq "VlanID"} | ft -AutoSize
   ``` 

   _**Résultats :** _


   |    Nom    | DisplayName | DisplayValue | RegistryKeyword | RegistryValue |
   |------------|-------------|--------------|-----------------|---------------|
   | TEST-40G-2 |   ID du réseau local virtuel   |     102      |     VlanID      |     {102}     |

   ---

5. Redémarrez la carte réseau pour appliquer le VLAN ID.

   ```PowerShell
   Restart-NetAdapter -Name "Test-40G-2" 
   ```

6. Vérifiez l’état est **des**.

   ```PowerShell
   Get-NetAdapter -Name "Test-40G-1" | ft -AutoSize
   ```

   _**Résultats :** _


   |    Nom    |          InterfaceDescription           | ifIndex | État |    MacAddress     | LinkSpeed |
   |------------|-----------------------------------------|---------|--------|-------------------|-----------|
   | Test-40G-2 | Ethernet Pro Mellanox ConnectX-3 Ada... |   11    |   Up (Haut)   | E4-1D-2D-07-43-D1 |  40 Gbits/s  |

   ---

   >[!IMPORTANT]
   >Il peut prendre plusieurs secondes pour l’appareil pour redémarrer et sont disponibles sur le réseau. 

7. Vérifiez la connectivité pour la première carte réseau, Test-40G-1.<p>En cas de connectivité, inspecter le commutateur de participation de configuration ou la destination de réseau local virtuel dans le même réseau local virtuel. 

   ```PowerShell
   Test-NetConnection 192.168.1.5
   ```

   _**Résultats :** _   


   |        Paramètre         |    Value    |
   |--------------------------|-------------|
   |       ComputerName       | 192.168.1.5 |
   |      RemoteAddress       | 192.168.1.5 |
   |      InterfaceAlias      | Test-40G-1  |
   |      SourceAddress       | 192.168.1.5 |
   |      PingSucceeded       |    True     |
   | PingReplyDetails \(RTT\) |    0 ms     |

   ---

8. Vérifiez la connectivité pour la première carte réseau, Test-40G-2.<p>En cas de connectivité, inspecter le commutateur de participation de configuration ou la destination de réseau local virtuel dans le même réseau local virtuel.

   ```PowerShell    
   Test-NetConnection 192.168.2.5
   ```

   _**Résultats :** _    


   |        Paramètre         |    Value    |
   |--------------------------|-------------|
   |       ComputerName       | 192.168.2.5 |
   |      RemoteAddress       | 192.168.2.5 |
   |      InterfaceAlias      | Test-40G-2  |
   |      SourceAddress       | 192.168.2.3 |
   |      PingSucceeded       |    True     |
   | PingReplyDetails \(RTT\) |    0 ms     |

   ---

   >[!IMPORTANT]
   >Il n’est pas rare qu’un **Test-NetConnection** ou l’échec du ping à se produire immédiatement après avoir effectué **redémarrage-NetAdapter**.  Par conséquent, attendez que la carte réseau initialiser complètement et puis recommencez l’opération.
   >
   >Si les connexions de réseau local virtuel 101 fonctionnent, mais ne les connexions de réseau local virtuel 102, il est possible que le commutateur doit être configuré pour autoriser le trafic du port sur le réseau local virtuel souhaité. Pour cela, vous pouvez vérifier en temporairement définissant les adaptateurs d’échec à 101 de réseau local virtuel et en répétant le test de connectivité.


   L’illustration suivante montre vos hôtes Hyper-V après avoir correctement configuré les réseaux locaux virtuels.

   ![Configurer la qualité de Service](../../media/Converged-NIC/3-datacenter-configure-qos.jpg)


## <a name="step-4-configure-quality-of-service-qos"></a>Étape 4. Configurer la qualité de Service \(QoS\)

>[!NOTE]
>Vous devez effectuer toutes les étapes de configuration suivantes DCB et QoS sur tous les hôtes qui sont destinées à communiquer entre eux.

1. Installer Data Center Bridging \(DCB\) sur chacun de vos hôtes Hyper-V.

   - **Facultatif** pour les configurations de réseau qui utilisent iWarp.
   - **Requis** pour les configurations de réseau qui utilisent RoCE \(n’importe quelle version\) pour les services RDMA.

   ```PowerShell
   Install-WindowsFeature Data-Center-Bridging
   ```

   _**Résultats :** _


   | Opération réussie | Redémarrage nécessaire | Code de sortie |     Résultat de la fonctionnalité     |
   |---------|----------------|-----------|------------------------|
   |  True   |       Non       |  Opération réussie  | {Pontage de centre de données} |

   ---

2. Définir les stratégies de QoS à SMB Direct :

   - **Facultatif** pour les configurations de réseau qui utilisent iWarp.
   - **Requis** pour les configurations de réseau qui utilisent RoCE \(n’importe quelle version\) pour les services RDMA.

   Dans l’exemple de commande ci-dessous, la valeur « 3 » est arbitraire. Vous pouvez utiliser n’importe quelle valeur comprise entre 1 et 7, que vous utilisez régulièrement la même valeur tout au long de la configuration des stratégies de QoS.

   ```PowerShell
   New-NetQosPolicy "SMB" -NetDirectPortMatchCondition 445 -PriorityValue8021Action 3
   ```

   _**Résultats :** _


   |   Paramètre    |          Value           |
   |----------------|--------------------------|
   |      Nom      |           SMB            |
   |     Propriétaire      | Stratégie de groupe \(Machine\) |
   | NetworkProfile |           Tous            |
   |   Priorité   |           127            |
   |   JobObject    |          &nbsp;          |
   | NetDirectPort  |           445            |
   | PriorityValue  |            3             |

   ---

3. Définir des stratégies de qualité de service supplémentaires pour le reste du trafic sur l’interface.   

   ```PowerShell
   New-NetQosPolicy "DEFAULT" -Default -PriorityValue8021Action 0
   ```

   _**Résultats :** _   


   |   Paramètre    |          Value           |
   |----------------|--------------------------|
   |      Nom      |         PAR DÉFAUT          |
   |     Propriétaire      | Stratégie de groupe \(Machine\) |
   | NetworkProfile |           Tous            |
   |   Priorité   |           127            |
   |    Modèle    |         Par défaut          |
   |   JobObject    |          &nbsp;          |
   | PriorityValue  |            0             |

   ---

4. Activer **contrôle de flux de priorité** pour le trafic SMB, qui n’est pas requis pour iWarp.

   ```PowerShell
   Enable-NetQosFlowControl -priority 3
   Get-NetQosFlowControl
   ```

   _**Résultats :** _


   | Priority | Enabled | PolicySet | IfIndex | IfAlias |
   |----------|---------|-----------|---------|---------|
   |    0     |  False  |  Globale   | &nbsp;  | &nbsp;  |
   |    1     |  False  |  Globale   | &nbsp;  | &nbsp;  |
   |    2     |  False  |  Globale   | &nbsp;  | &nbsp;  |
   |    3     |  True   |  Globale   | &nbsp;  | &nbsp;  |
   |    4     |  False  |  Globale   | &nbsp;  | &nbsp;  |
   |    5     |  False  |  Globale   | &nbsp;  | &nbsp;  |
   |    6     |  False  |  Globale   | &nbsp;  | &nbsp;  |
   |    7     |  False  |  Globale   | &nbsp;  | &nbsp;  |

   ---

   >**IMPORTANT** si vos résultats ne correspondent pas ces résultats, car les autres éléments que 3 ont une valeur Enabled True, vous devez désactiver **contrôle de flux** pour ces classes.
   >
   >```PowerShell
   >Disable-NetQosFlowControl -priority 0,1,2,4,5,6,7
   >Get-NetQosFlowControl
   >```
   >Sous les configurations plus complexes, les autres classes de trafic peuvent nécessiter le contrôle de flux, mais ces scénarios sont en dehors de la portée de ce guide.


5. Activer la qualité de service pour la première carte réseau, Test-40G-1.

   ```PowerShell
   Enable-NetAdapterQos -InterfaceAlias "Test-40G-1"
   Get-NetAdapterQos -Name "Test-40G-1"

   Name: TEST-40G-1 
   Enabled: True
   ```
   _**Fonctionnalités**:_   


   |      Paramètre      |   Matériel   |   Actuel    |
   |---------------------|--------------|--------------|
   |    MacSecBypass     | NotSupported | NotSupported |
   |     DcbxSupport     |     Aucune     |     Aucune     |
   | NumTCs(Max/ETS/PFC) |    8/8/8     |    8/8/8     |

   ---

   _**OperationalTrafficClasses**:_    


   | TC |  TSA   | Bande passante | Priorités |
   |----|--------|-----------|------------|
   | 0  | Strict |  &nbsp;   |    0-7     |

   ---

   _**OperationalFlowControl**:_  

   Priorité 3 activé  

   _**OperationalClassifications**:_  


   | Protocol  | Type de port / | Priority |
   |-----------|-----------|----------|
   |  Par défaut  |  &nbsp;   |    0     |
   | NetDirect |    445    |    3     |

   ---

6. Activer la qualité de service pour la deuxième carte réseau, Test-40G-2.

   ```PowerShell
   Enable-NetAdapterQos -InterfaceAlias "Test-40G-2"
   Get-NetAdapterQos -Name "Test-40G-2"

   Name: TEST-40G-2 
   Enabled: True 
   ```

   _**Fonctionnalités**:_ 


   |      Paramètre      |   Matériel   |   Actuel    |
   |---------------------|--------------|--------------|
   |    MacSecBypass     | NotSupported | NotSupported |
   |     DcbxSupport     |     Aucune     |     Aucune     |
   | NumTCs(Max/ETS/PFC) |    8/8/8     |    8/8/8     |

   ---

   _**OperationalTrafficClasses**:_  


   | TC |  TSA   | Bande passante | Priorités |
   |----|--------|-----------|------------|
   | 0  | Strict |  &nbsp;   |    0-7     |

   ---

    _**OperationalFlowControl**:_  

    Priorité 3 activé  

   _**OperationalClassifications**:_  


   | Protocol  | Type de port / | Priority |
   |-----------|-----------|----------|
   |  Par défaut  |  &nbsp;   |    0     |
   | NetDirect |    445    |    3     |

   ---


7. Réserver la moitié de la bande passante à SMB Direct \(RDMA\)

   ```PowerShell
   New-NetQosTrafficClass "SMB" -priority 3 -bandwidthpercentage 50 -algorithm ETS
   ```

   _**Résultats :** _  


   | Nom | Algorithm | Bandwidth(%) | Priority | PolicySet | IfIndex | IfAlias |
   |------|-----------|--------------|----------|-----------|---------|---------|
   | SMB  |    ETS    |      50      |    3     |  Globale   | &nbsp;  | &nbsp;  |

   ---

8. Afficher les paramètres de réservation de bande passante :   

   ```PowerShell
   Get-NetQosTrafficClass | ft -AutoSize
   ```

   _**Résultats :** _  


   |   Nom    | Algorithm | Bandwidth(%) | Priority | PolicySet | IfIndex | IfAlias |
   |-----------|-----------|--------------|----------|-----------|---------|---------|
   | [Default] |    ETS    |      50      | 0-2,4-7  |  Globale   | &nbsp;  | &nbsp;  |
   |    SMB    |    ETS    |      50      |    3     |  Globale   | &nbsp;  | &nbsp;  |

   ---

9. (Facultatif) Créer deux classes de trafic supplémentaire pour le trafic IP de client. 

   >[!TIP]
   >Vous pouvez omettre les valeurs « IP1 » et « IP2 ».

   ```PowerShell
   New-NetQosTrafficClass "IP1" -Priority 1 -bandwidthpercentage 10 -algorithm ETS
   ```

   _**Résultats :** _


   | Nom | Algorithm | Bandwidth(%) | Priority | PolicySet | IfIndex | IfAlias |
   |------|-----------|--------------|----------|-----------|---------|---------|
   | IP1  |    ETS    |      10      |    1     |  Globale   | &nbsp;  | &nbsp;  |

   ---

   ```PowerShell
   New-NetQosTrafficClass "IP2" -Priority 2 -bandwidthpercentage 10 -algorithm ETS
   ```

   _**Résultats :** _


   | Nom | Algorithm | Bandwidth(%) | Priority | PolicySet | IfIndex | IfAlias |
   |------|-----------|--------------|----------|-----------|---------|---------|
   | IP2  |    ETS    |      10      |    2     |  Globale   | &nbsp;  | &nbsp;  |

   ---

10. Afficher les classes de trafic QoS.

    ```PowerShell
    Get-NetQosTrafficClass | ft -AutoSize
    ```

    _**Résultats :** _


    |   Nom    | Algorithm | Bandwidth(%) | Priority | PolicySet | IfIndex | IfAlias |
    |-----------|-----------|--------------|----------|-----------|---------|---------|
    | [Default] |    ETS    |      30      |  0,4-7   |  Globale   | &nbsp;  | &nbsp;  |
    |    SMB    |    ETS    |      50      |    3     |  Globale   | &nbsp;  | &nbsp;  |
    |    IP1    |    ETS    |      10      |    1     |  Globale   | &nbsp;  | &nbsp;  |
    |    IP2    |    ETS    |      10      |    2     |  Globale   | &nbsp;  | &nbsp;  |

    ---

11. (Facultatif) Remplacer le débogueur.<p>Par défaut, le débogueur attaché bloque NetQos. 

    ```PowerShell
    Set-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" AllowFlowControlUnderDebugger -type DWORD -Value 1 –Force
    Get-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" | ft AllowFlowControlUnderDebugger
    ```

    _**Résultats :** _  

    ```
    AllowFlowControlUnderDebugger
    -----------------------------
    1
    ```

## <a name="step-5-verify-the-rdma-configuration-mode-1"></a>Étape 5. Vérifier la configuration RDMA \(Mode 1\) 

Vous souhaitez vous assurer que l’infrastructure est correctement configuré avant la création d’un commutateur virtuel et en cours de transition RDMA \(Mode 2\).

L’illustration suivante montre l’état actuel des hôtes Hyper-V.

![Test RDMA](../../media/Converged-NIC/4-datacenter-test-rdma.jpg)


1. Vérifiez la configuration RDMA.

   ```PowerShell
   Get-NetAdapterRdma | ft -AutoSize
   ```

   _**Résultats :** _


   |    Nom    |        InterfaceDescription        | Enabled |
   |------------|------------------------------------|---------|
   | TEST-40G-1 | Carte Mellanox ConnectX-4 VPI #2 |  True   |
   | TEST-40G-2 |  Mellanox ConnectX-4 VPI adaptateur   |  True   |

   ---

2. Déterminer le **ifIndex** valeur de vos cartes cibles.<p>Vous utilisez cette valeur dans les étapes suivantes lorsque vous exécutez le script que vous téléchargez.   

   ```PowerShell
   Get-NetIPConfiguration -InterfaceAlias "TEST*" | ft InterfaceAlias,InterfaceIndex,IPv4Address
   ```

   _**Résultats :** _


   | InterfaceAlias | InterfaceIndex |  IPv4Address  |
   |----------------|----------------|---------------|
   |   TEST-40G-1   |       14       | {192.168.1.3} |
   |   TEST-40G-2   |       13       | {192.168.2.3} |

   ---

3. Téléchargez le [DiskSpd.exe utilitaire](https://aka.ms/diskspd) et extrayez-le dans C:\TEST\.

4. Téléchargez le [script Test-RDMA PowerShell](https://github.com/Microsoft/SDN/blob/master/Diagnostics/Test-Rdma.ps1) dans un dossier de test sur votre lecteur local, par exemple, C:\TEST\.

5. Exécutez le **Test-Rdma.ps1** script PowerShell pour passer la valeur ifIndex au script, ainsi que l’adresse IP de la première carte à distance sur le même réseau local virtuel.<p>Dans cet exemple, le script transmet la **ifIndex** valeur 14 sur l’adresse de l’adaptateur de réseau distant 192.168.1.5.

   ```PowerShell
   C:\TEST\Test-RDMA.PS1 -IfIndex 14 -IsRoCE $true -RemoteIpAddress 192.168.1.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
   ```

   _**Résultats :** _ 

   ```   
   VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\diskspd.exe
   VERBOSE: The adapter M2 is a physical adapter
   VERBOSE: Underlying adapter is RoCE. Checking if QoS/DCB/PFC is configured on each physical adapter(s)
   VERBOSE: QoS/DCB/PFC configuration is correct.
   VERBOSE: RDMA configuration is correct.
   VERBOSE: Checking if remote IP address, 192.168.1.5, is reachable.
   VERBOSE: Remote IP 192.168.1.5 is reachable.
   VERBOSE: Disabling RDMA on adapters that are not part of this test. RDMA will be enabled on them later.
   VERBOSE: Testing RDMA traffic now for. Traffic will be sent in a parallel job. Job details:
   VERBOSE: 0 RDMA bytes written per second
   VERBOSE: 0 RDMA bytes sent per second
   VERBOSE: 662979201 RDMA bytes written per second
   VERBOSE: 37561021 RDMA bytes sent per second
   VERBOSE: 1023098948 RDMA bytes written per second
   VERBOSE: 8901349 RDMA bytes sent per second
   VERBOSE: Enabling RDMA on adapters that are not part of this test. RDMA was disabled on them prior to sending RDMA traffic.
   VERBOSE: RDMA traffic test SUCCESSFUL: RDMA traffic was sent to 192.168.1.5
   ```

   >[!NOTE]
   >Si le trafic RDMA échoue, le cas RoCE en particulier, consultez votre configuration ToR Switch pour les paramètres PFC/ETS appropriés qui doivent correspondre aux paramètres de l’hôte. Reportez-vous à la section de la qualité de service dans ce document pour les valeurs de référence.

6. Exécutez le **Test-Rdma.ps1** script PowerShell pour passer la valeur ifIndex au script, ainsi que l’adresse IP de la deuxième carte à distance sur le même réseau local virtuel.<p>Dans cet exemple, le script transmet la **ifIndex** valeur de 13 sur l’adresse de l’adaptateur de réseau distant 192.168.2.5.

   ```PowerShell
   C:\TEST\Test-RDMA.PS1 -IfIndex 13 -IsRoCE $true -RemoteIpAddress 192.168.2.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
   ```

   _**Résultats :** _ 

   ```   
   VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\diskspd.exe
   VERBOSE: The adapter TEST-40G-2 is a physical adapter
   VERBOSE: Underlying adapter is RoCE. Checking if QoS/DCB/PFC is configured on each physical adapter(s)
   VERBOSE: QoS/DCB/PFC configuration is correct.
   VERBOSE: RDMA configuration is correct.
   VERBOSE: Checking if remote IP address, 192.168.2.5, is reachable.
   VERBOSE: Remote IP 192.168.2.5 is reachable.
   VERBOSE: Disabling RDMA on adapters that are not part of this test. RDMA will be enabled on them later.
   VERBOSE: Testing RDMA traffic now for. Traffic will be sent in a parallel job. Job details:
   VERBOSE: 0 RDMA bytes written per second
   VERBOSE: 0 RDMA bytes sent per second
   VERBOSE: 541185606 RDMA bytes written per second
   VERBOSE: 34821478 RDMA bytes sent per second
   VERBOSE: 954717307 RDMA bytes written per second
   VERBOSE: 35040816 RDMA bytes sent per second
   VERBOSE: Enabling RDMA on adapters that are not part of this test. RDMA was disabled on them prior to sending RDMA traffic.
   VERBOSE: RDMA traffic test SUCCESSFUL: RDMA traffic was sent to 192.168.2.5
   ``` 

## <a name="step-6-create-a-hyper-v-vswitch-on-your-hyper-v-hosts"></a>Étape 6. Créer un commutateur virtuel Hyper-V sur vos ordinateurs hôtes Hyper-V


L’illustration suivante montre l’hôte 1 Hyper-V avec un commutateur virtuel.

![Créer un commutateur virtuel Hyper-V](../../media/Converged-NIC/5-datacenter-create-vswitch.jpg)

1. Créer un commutateur virtuel en mode de jeu sur l’hôte Hyper-V 1.

   ```PowerShell
   New-VMSwitch –Name "VMSTEST" –NetAdapterName "TEST-40G-1","TEST-40G-2" -EnableEmbeddedTeaming $true -AllowManagementOS $true
   ```

   _**Résultat :** _


   |  Nom   | SwitchType | NetAdapterInterfaceDescription |
   |---------|------------|--------------------------------|
   | VMSTEST |  Externe  |        Interface associées        |

   ---

2. Afficher l’association de cartes physiques dans le jeu.

   ```PowerShell
   Get-VMSwitchTeam -Name "VMSTEST" | fl
   ```

   _**Résultats :** _  

   ```
   Name: VMSTEST  
   Id: ad9bb542-dda2-4450-a00e-f96d44bdfbec  
   NetAdapterInterfaceDescription: {Mellanox ConnectX-3 Pro Ethernet Adapter, Mellanox ConnectX-3 Pro Ethernet Adapter #2}  
   TeamingMode: SwitchIndependent  
   LoadBalancingAlgorithm: Dynamic   
   ```

3. Afficher deux vues de la carte réseau virtuelle hôte

   ```PowerShell
    Get-NetAdapter
   ```

   _**Résultats :** _


   |        Nom         |        InterfaceDescription         | ifIndex | État |    MacAddress     | LinkSpeed |
   |---------------------|-------------------------------------|---------|--------|-------------------|-----------|
   | vEthernet (VMSTEST) | Adaptateur Ethernet virtuel Hyper-V #2 |   28    |   Up (Haut)   | E4-1D-2D-07-40-71 |  80 Gbits/s  |

   ---

4. Afficher les propriétés supplémentaires de la carte réseau virtuelle hôte. 

   ```PowerShell
   Get-VMNetworkAdapter -ManagementOS
   ```

   _**Résultats :** _


   |  Nom   | IsManagementOs | VMName  |  SwitchName  | MacAddress | État | IPAddresses |
   |---------|----------------|---------|--------------|------------|--------|-------------|
   | VMSTEST |      True      | VMSTEST | E41D2D074071 |    {Ok}    | &nbsp; |             |

   ---


5. Testez la connexion réseau à l’adaptateur de réseau local virtuel 101 à distance.

   ```PowerShell
   Test-NetConnection 192.168.1.5 
   ```

   _**Résultats :** _  

   ```
   WARNING: Ping to 192.168.1.5 failed -- Status: DestinationHostUnreachable

   ComputerName   : 192.168.1.5
   RemoteAddress  : 192.168.1.5
   InterfaceAlias : vEthernet (CORP-External-Switch)
   SourceAddress  : 10.199.48.170
   PingSucceeded  : False
   PingReplyDetails (RTT) : 0 ms
   ```

## <a name="step-7-remove-the-access-vlan-setting"></a>Étape 7. Supprimez le paramètre VLAN Access

Dans cette étape, vous supprimez le paramètre de réseau local virtuel de l’accès à partir de la carte réseau physique et pour définir le VLANID à l’aide du commutateur virtuel.

Vous devez supprimer le paramètre VLAN d’accès pour empêcher les deux balisage automatique le trafic de sortie avec l’ID de VLAN incorrect et à partir de l’entrée de filtrage du trafic qui ne correspond pas à l’ID de VLAN ACCESS.

1. Supprimez le paramètre.

   ```PowerShell
   Set-NetAdapterAdvancedProperty -Name "Test-40G-1" -RegistryKeyword VlanID -RegistryValue "0"
   Set-NetAdapterAdvancedProperty -Name "Test-40G-2" -RegistryKeyword VlanID -RegistryValue "0"
   ```

2. Définir l’ID de VLAN.

   ```PowerShell
   Set-VMNetworkAdapterVlan -VMNetworkAdapterName "VMSTEST" -VlanId "101" -Access -ManagementOS
   Get-VMNetworkAdapterVlan -ManagementOS -VMNetworkAdapterName "VMSTEST"
   ```

   _**Résultats :** _  

   ```
   VMName VMNetworkAdapterName Mode   VlanList
   ------ -------------------- ----   --------
          VMSTEST              Access 101     
   ```


3. Tester la connexion réseau.

   ```PowerShell
   Test-NetConnection 192.168.1.5
   ```

   _**Résultats :** _   

   ```
   ComputerName   : 192.168.1.5
   RemoteAddress  : 192.168.1.5
   InterfaceAlias : vEthernet (VMSTEST)
   SourceAddress  : 192.168.1.3
   PingSucceeded  : True
   PingReplyDetails (RTT) : 0 ms
   ```

   >**IMPORTANT** si vos résultats ne sont pas similaires à ceux d’exemple et le test ping échoue avec le message « Avertissement : Échec du test ping à 192.168.1.5--état : DestinationHostUnreachable, » confirmer que le « vEthernet (VMSTEST) » a l’adresse IP appropriée.
   >
   >```PowerShell
   >Get-NetIPAddress -InterfaceAlias "vEthernet (VMSTEST)"
   >```
   >
   >Si l’adresse IP n’est pas définie, corrigez le problème.
   >
   >```PowerShell
   >New-NetIPAddress -InterfaceAlias "vEthernet (VMSTEST)" -IPAddress 192.168.1.3 -PrefixLength 24
   >
   >IPAddress : 192.168.1.3
   >InterfaceIndex: 37
   >InterfaceAlias: vEthernet (VMSTEST)
   >AddressFamily : IPv4
   >Type  : Unicast
   >PrefixLength  : 24
   >PrefixOrigin  : Manual
   >SuffixOrigin  : Manual
   >AddressState  : Tentative
   >ValidLifetime : Infinite ([TimeSpan]::MaxValue)
   >PreferredLifetime : Infinite ([TimeSpan]::MaxValue)
   >SkipAsSource  : False
   >PolicyStore   : ActiveStore
   >```  


4. Renommer la gestion de carte réseau.

   ```PowerShell
   Rename-VMNetworkAdapter -ManagementOS -Name “VMSTEST” -NewName “MGT”
   Get-VMNetworkAdapter -ManagementOS
   ```

   _**Résultats :** _ 


   |         Nom         | IsManagementOs | VMName |      SwitchName      |  MacAddress  | État | IPAddresses |
   |----------------------|----------------|--------|----------------------|--------------|--------|-------------|
   | Commutateur de CORP-externe |      True      | &nbsp; | Commutateur de CORP-externe | 001B785768AA |  {Ok}  |   &nbsp;    |
   |         MGT          |      True      | &nbsp; |       VMSTEST        | E41D2D074071 |  {Ok}  |   &nbsp;    |

   ---

5. Afficher les propriétés des cartes réseau supplémentaires.

   ```PowerShell
   Get-NetAdapter
   ```

   _**Résultats :** _


   |      Nom       |        InterfaceDescription         | ifIndex | État |    MacAddress     | LinkSpeed |
   |-----------------|-------------------------------------|---------|--------|-------------------|-----------|
   | vEthernet (gestion avancée) | Adaptateur Ethernet virtuel Hyper-V #2 |   28    |   Up (Haut)   | E4-1D-2D-07-40-71 |  80 Gbits/s  |

   ---

## <a name="step-8-test-hyper-v-vswitch-rdma"></a>Étape 8. Test RDMA de commutateur virtuel Hyper-V

L’illustration suivante montre l’état actuel de vos hôtes Hyper-V, y compris le commutateur virtuel sur l’hôte 1 Hyper-V.

![Tester le commutateur virtuel Hyper-V](../../media/Converged-NIC/6-datacenter-test-vswitch-rdma.jpg)

1. Définir la priorité sur la carte réseau virtuelle hôte afin de compléter les paramètres de réseau local virtuel précédents de balisage.

   ```PowerShell    
   Set-VMNetworkAdapter -ManagementOS -Name "MGT" -IeeePriorityTag on
   Get-VMNetworkAdapter -ManagementOS -Name "MGT" | fl Name,IeeePriorityTag
   ```

   _**Résultats :** _  

   nom : MGT  
   IeeePriorityTag :  Activé  

2. Créer deux cartes réseau virtuelles hôtes pour RDMA et les connecter à un commutateur virtuel VMSTEST.

   ```PowerShell    
   Add-VMNetworkAdapter –SwitchName "VMSTEST" –Name SMB1 –ManagementOS
   Add-VMNetworkAdapter –SwitchName "VMSTEST" –Name SMB2 –ManagementOS
   ```

3. Afficher les propriétés de la carte réseau de gestion.

   ```PowerShell    
   Get-VMNetworkAdapter -ManagementOS
   ```

   _**Résultats :** _ 


   |         Nom         | IsManagementOs |        VMName        |  SwitchName  | MacAddress | État | IPAddresses |
   |----------------------|----------------|----------------------|--------------|------------|--------|-------------|
   | Commutateur de CORP-externe |      True      | Commutateur de CORP-externe | 001B785768AA |    {Ok}    | &nbsp; |             |
   |         Gestion avancée          |      True      |       VMSTEST        | E41D2D074071 |    {Ok}    | &nbsp; |             |
   |         SMB1         |      True      |       VMSTEST        | 00155D30AA00 |    {Ok}    | &nbsp; |             |
   |         SMB2         |      True      |       VMSTEST        | 00155D30AA01 |    {Ok}    | &nbsp; |             |

   ---

## <a name="step-9-assign-an-ip-address-to-the-smb-host-vnics-vethernet-smb1-and-vethernet-smb2"></a>Étape 9 : Affecter une adresse IP à la vEthernet de cartes réseau virtuelles hôte SMB \(block1\) et vEthernet \(SMB2\)

Le TEST-40G-1 et les cartes physiques de TEST-40G-2 toujours ont un VLAN ACCESS de 101 et 102 configuré. Pour cette raison, les adaptateurs marquer le trafic - et ping réussit. Auparavant, vous configuré les deux ID de VLAN de carte réseau à zéro, puis définissez le commutateur virtuel VMSTEST 101 de réseau local virtuel. Après cela, vous pouviez toujours un test ping de l’adaptateur de réseau local virtuel 101 à distance à l’aide de la carte réseau virtuelle de gestion avancée, mais il n’existe actuellement aucun membre 102 de réseau local virtuel.



1. Supprimez le paramètre de réseau local virtuel de l’accès à partir de la carte réseau physique pour éviter qu’il les deux balisage automatique le trafic de sortie avec l’ID de VLAN incorrect et pour empêcher de filtrage du trafic d’entrée qui ne correspond pas à l’ID de VLAN ACCESS.

   ```PowerShell    
   New-NetIPAddress -InterfaceAlias "vEthernet (SMB1)" -IPAddress 192.168.2.111 -PrefixLength 24
   ```

   _**Résultats :** _  

   ```   
   IPAddress : 192.168.2.111
   InterfaceIndex: 40
   InterfaceAlias: vEthernet (SMB1)
   AddressFamily : IPv4
   Type  : Unicast
   PrefixLength  : 24
   PrefixOrigin  : Manual
   SuffixOrigin  : Manual
   AddressState  : Invalid
   ValidLifetime : Infinite ([TimeSpan]::MaxValue)
   PreferredLifetime : Infinite ([TimeSpan]::MaxValue)
   SkipAsSource  : False
   PolicyStore   : PersistentStore
   ```

2. Testez l’adaptateur de réseau local virtuel 102 à distance.

   ```PowerShell
   Test-NetConnection 192.168.2.5 
   ```

   _**Résultats :** _  

   ```
   ComputerName   : 192.168.2.5
   RemoteAddress  : 192.168.2.5
   InterfaceAlias : vEthernet (SMB1)
   SourceAddress  : 192.168.2.111
   PingSucceeded  : True
   PingReplyDetails (RTT) : 0 ms
   ```

3. Ajouter une nouvelle adresse IP pour l’interface vEthernet \(SMB2\).

   ```PowerShell
   New-NetIPAddress -InterfaceAlias "vEthernet (SMB2)" -IPAddress 192.168.2.222 -PrefixLength 24 
   ```

   _**Résultats :** _ 

   ```
   IPAddress : 192.168.2.222
   InterfaceIndex: 44
   InterfaceAlias: vEthernet (SMB2)
   AddressFamily : IPv4
   Type  : Unicast
   PrefixLength  : 24
   PrefixOrigin  : Manual
   SuffixOrigin  : Manual
   AddressState  : Invalid
   ValidLifetime : Infinite ([TimeSpan]::MaxValue)
   PreferredLifetime : Infinite ([TimeSpan]::MaxValue)
   SkipAsSource  : False
   PolicyStore   : PersistentStore
   ```

4. Testez à nouveau la connexion.    


5. Placez les cartes réseau virtuelles de l’hôte de RDMA sur la 102 VLAN préexistant.

   ```PowerShell
   Set-VMNetworkAdapterVlan -VMNetworkAdapterName "SMB1" -VlanId "102" -Access -ManagementOS
   Set-VMNetworkAdapterVlan -VMNetworkAdapterName "SMB2" -VlanId "102" -Access -ManagementOS

   Get-VMNetworkAdapterVlan -ManagementOS
   ```

   _**Résultats :** _ 

   ```   
   VMName VMNetworkAdapterName Mode VlanList
   ------ -------------------- ---- --------
      SMB1 Access   102 
      Mgt  Access   101 
      SMB2 Access   102 
      CORP-External-Switch Untagged
   ```

6. Inspecter le mappage de SMB1 et SMB2 aux cartes réseau physiques sous-jacente sous le commutateur virtuel défini une équipe.<p>L’association de carte réseau virtuelle hôte pour les cartes réseau physiques est aléatoire et susceptible d’être rééquilibrage pendant la création et la destruction. Dans ce cas, vous pouvez utiliser un mécanisme indirect pour vérifier l’association en cours. Les adresses MAC des SMB1 et SMB2 sont associés au membre de l’association de cartes réseau TEST-40G-2. Cela n’est pas idéale car Test-40G-1 ne dispose pas d’une carte réseau virtuelle hôte SMB associée et n’autorisera pas pour l’utilisation du trafic RDMA sur le lien jusqu'à ce qu’une carte réseau virtuelle hôte SMB est mappée à celui-ci.

   ```PowerShell    
   Get-NetAdapterVPort (Preferred)

   Get-NetAdapterVmqQueue
   ```

   _**Résultats :** _ 

   ```
   Name   QueueID MacAddressVlanID Processor VmFriendlyName
   ----   ------- ---------------- --------- --------------
   TEST-40G-1 1   E4-1D-2D-07-40-71 1010:17
   TEST-40G-2 1   00-15-5D-30-AA-00 1020:17
   TEST-40G-2 2   00-15-5D-30-AA-01 1020:17
   ```

7. Afficher les propriétés de l’adaptateur de réseau de machine virtuelle.

   ```PowerShell
   Get-VMNetworkAdapter -ManagementOS
   ```

   _**Résultats :** _ 

   ```
   Name IsManagementOs VMName SwitchName   MacAddress   Status IPAddresses
   ---- -------------- ------ ----------   ----------   ------ -----------
   CORP-External-Switch True  CORP-External-Switch 001B785768AA {Ok}  
   Mgt  True  VMSTEST  E41D2D074071 {Ok}  
   SMB1 True  VMSTEST  00155D30AA00 {Ok}  
   SMB2 True  VMSTEST  00155D30AA01 {Ok}  
   ```

8. Afficher le mappage d’équipe de carte réseau.<p>Les résultats ne doivent pas retourner d’informations, car vous n’avez pas encore effectué de mappage.

   ```PowerShell
   Get-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST -VMNetworkAdapterName SMB1
   Get-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST -VMNetworkAdapterName SMB2
   ```


9. Mapper SMB1 et SMB2 pour séparer les membres de l’équipe carte réseau physiques et pour afficher les résultats de vos actions.

   >[!IMPORTANT]
   >Assurez-vous que vous effectuez cette étape avant de continuer, ou votre implémentation échoue.

   ```PowerShell
   Set-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST -VMNetworkAdapterName "SMB1" -PhysicalNetAdapterName "Test-40G-1"
   Set-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST -VMNetworkAdapterName "SMB2" -PhysicalNetAdapterName "Test-40G-2"

   Get-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST
   ```

   _**Résultats :** _ 

   ```   
   NetAdapterName : Test-40G-1
   NetAdapterDeviceId : {BAA9A00F-A844-4740-AA93-6BD838F8CFBA}
   ParentAdapter  : VMInternalNetworkAdapter, Name = 'SMB1'
   IsTemplate : False
   CimSession : CimSession: .
   ComputerName   : 27-3145G0803
   IsDeleted  : False

   NetAdapterName : Test-40G-2
   NetAdapterDeviceId : {B7AB5BB3-8ACB-444B-8B7E-BC882935EBC8}
   ParentAdapter  : VMInternalNetworkAdapter, Name = 'SMB2'
   IsTemplate : False
   CimSession : CimSession: .
   ComputerName   : 27-3145G0803
   IsDeleted  : False
   ```

10. Vérifiez les associations de MAC créées précédemment.

    ```PowerShell    
    Get-NetAdapterVmqQueue
    ```

    _**Résultats :** _ 

    ```   
    Name   QueueID MacAddressVlanID Processor VmFriendlyName
    ----   ------- ---------------- --------- --------------
    TEST-40G-1 1   E4-1D-2D-07-40-71 1010:17
    TEST-40G-1 2   00-15-5D-30-AA-00 1020:17
    TEST-40G-2 1   00-15-5D-30-AA-01 1020:17
    ```


11. Tester la connexion à partir du système distant, car les deux cartes réseau virtuelles hôtes se trouvent sur le même sous-réseau et ont le même ID de VLAN \(102\).

    ```PowerShell 
    Test-NetConnection 192.168.2.111
    ```

    _**Résultats :** _   

    ```
    ComputerName   : 192.168.2.111
    RemoteAddress  : 192.168.2.111
    InterfaceAlias : Test-40G-2
    SourceAddress  : 192.168.2.5
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms
    ```

    ```PowerShell   
    Test-NetConnection 192.168.2.222
    ```

    _**Résultats :** _   

    ```
    ComputerName   : 192.168.2.222
    RemoteAddress  : 192.168.2.222
    InterfaceAlias : Test-40G-2
    SourceAddress  : 192.168.2.5
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms 
    ```
12. Définir le nom, nom du commutateur et les balises de priorité.   

    ```PowerShell
    Set-VMNetworkAdapter -ManagementOS -Name "SMB1" -IeeePriorityTag on
    Set-VMNetworkAdapter -ManagementOS -Name "SMB2" -IeeePriorityTag on
    Get-VMNetworkAdapter -ManagementOS -Name "SMB*" | fl Name,SwitchName,IeeePriorityTag,Status
    ```

    _**Résultats :** _   

    ```
    Name: SMB1
    SwitchName  : VMSTEST
    IeeePriorityTag : On 
    Status  : {Ok}

    Name: SMB2
    SwitchName  : VMSTEST
    IeeePriorityTag : On
    Status  : {Ok}
    ```

13. Afficher les propriétés de l’adaptateur de réseau vEthernet.

    ```PowerShell
    Get-NetAdapterRdma -Name "vEthernet*" | sort Name | ft -AutoSize
    ```

    _**Résultats :** _   

    ```
    Name  InterfaceDescription Enabled
    ----  -------------------- -------
    vEthernet (SMB2)  Hyper-V Virtual Ethernet Adapter #4  False  
    vEthernet (SMB1)  Hyper-V Virtual Ethernet Adapter #3  False  
    vEthernet (MGT)   Hyper-V Virtual Ethernet Adapter #2  False   
    ```

14. Activer les cartes réseau de vEthernet.  

    ```PowerShell
    Enable-NetAdapterRdma -Name "vEthernet (SMB1)"
    Enable-NetAdapterRdma -Name "vEthernet (SMB2)"
    Get-NetAdapterRdma -Name "vEthernet*" | sort Name | fl *
    ```

    _**Résultats :** _   

    ```
    Name  InterfaceDescription Enabled
    ----  -------------------- -------
    vEthernet (SMB2)  Hyper-V Virtual Ethernet Adapter #4  True   
    vEthernet (SMB1)  Hyper-V Virtual Ethernet Adapter #3  True  
    vEthernet (MGT)   Hyper-V Virtual Ethernet Adapter #2  False
    ```

## <a name="step-10-validate-the-rdma-functionality"></a>Étape 10. Valider la fonctionnalité RDMA.

Voulez-vous valider la fonctionnalité RDMA à partir du système à distance sur le système local, ce qui a un commutateur virtuel, pour les deux membres de l’équipe de jeu de commutateur virtuel.<p>Étant donné que les deux cartes réseau virtuelles hôtes \(SMB1 et SMB2\) sont affectés à 102 de réseau local virtuel, vous pouvez sélectionner l’adaptateur de réseau local virtuel 102 sur le système distant. <p>Dans cet exemple, la carte réseau Test-40G-2 fait de RDMA pour SMB1 (192.168.2.111) et SMB2 (192.168.2.222).

>[!TIP]
>Vous devrez peut-être désactiver le pare-feu sur ce système.  Pour plus d’informations, consultez votre stratégie d’infrastructure.
>
>```PowerShell
>Set-NetFirewallProfile -All -Enabled False
>    
>Get-NetAdapterAdvancedProperty -Name "Test-40G-2"
>```
>
>_**Résultats :** _ 
>   
>```
>Name  DisplayNameDisplayValue   RegistryKeyword RegistryValue  
>----  -----------------------   --------------- -------------  
> .
> .
>Test-40G-2VLAN ID102VlanID  {102} 
>```

1. Afficher les propriétés de la carte réseau.

   ```PowerShell
   Get-NetAdapter
   ```

   _**Résultats :** _ 

   ```
   Name  InterfaceDescriptionifIndex Status   MacAddress LinkSpeed
   ----  --------------------------- ------   ---------- ---------
   Test-40G-2Mellanox ConnectX-3 Pro Ethernet A...#3   3 Up   E4-1D-2D-07-43-D140 Gbps
   ```

2. Afficher la carte réseau d’informations RDMA.

   ```PowerShell
   Get-NetAdapterRdma
   ```

   _**Résultats :** _  

   ```
   Name  InterfaceDescription Enabled
   ----  -------------------- -------
   Test-40G-2Mellanox ConnectX-3 Pro Ethernet Adap... True   
   ```

3. Effectuer le test de trafic RDMA sur la première carte physique.

   ```PowerShell 
   C:\TEST\Test-RDMA.PS1 -IfIndex 3 -IsRoCE $true -RemoteIpAddress 192.168.2.111 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
   ```

   _**Résultats :** _ 

   ```
   VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\diskspd.exe
   VERBOSE: The adapter Test-40G-2 is a physical adapter
   VERBOSE: Underlying adapter is RoCE. Checking if QoS/DCB/PFC is configured on each physical adapter(s)
   VERBOSE: QoS/DCB/PFC configuration is correct.
   VERBOSE: RDMA configuration is correct.
   VERBOSE: Checking if remote IP address, 192.168.2.111, is reachable.
   VERBOSE: Remote IP 192.168.2.111 is reachable.
   VERBOSE: Disabling RDMA on adapters that are not part of this test. RDMA will be enabled on them later.
   VERBOSE: Testing RDMA traffic now for. Traffic will be sent in a parallel job. Job details:
   VERBOSE: 34251744 RDMA bytes sent per second
   VERBOSE: 967346308 RDMA bytes written per second
   VERBOSE: 35698177 RDMA bytes sent per second
   VERBOSE: 976601842 RDMA bytes written per second
   VERBOSE: Enabling RDMA on adapters that are not part of this test. RDMA was disabled on them prior to sending RDMA traffic.
   VERBOSE: RDMA traffic test SUCCESSFUL: RDMA traffic was sent to 192.168.2.111
   ```

4. Effectuer le test de trafic RDMA sur la deuxième carte physique.

   ```PowerShell
   C:\TEST\Test-RDMA.PS1 -IfIndex 3 -IsRoCE $true -RemoteIpAddress 192.168.2.222 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
   ```

   _**Résultats :** _ 

   ```
   VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\diskspd.exe
   VERBOSE: The adapter Test-40G-2 is a physical adapter
   VERBOSE: Underlying adapter is RoCE. Checking if QoS/DCB/PFC is configured on each physical adapter(s)
   VERBOSE: QoS/DCB/PFC configuration is correct.
   VERBOSE: RDMA configuration is correct.
   VERBOSE: Checking if remote IP address, 192.168.2.222, is reachable.
   VERBOSE: Remote IP 192.168.2.222 is reachable.
   VERBOSE: Disabling RDMA on adapters that are not part of this test. RDMA will be enabled on them later.
   VERBOSE: Testing RDMA traffic now for. Traffic will be sent in a parallel job. Job details:
   VERBOSE: 0 RDMA bytes written per second
   VERBOSE: 0 RDMA bytes sent per second
   VERBOSE: 485137693 RDMA bytes written per second
   VERBOSE: 35200268 RDMA bytes sent per second
   VERBOSE: 939044611 RDMA bytes written per second
   VERBOSE: 34880901 RDMA bytes sent per second
   VERBOSE: Enabling RDMA on adapters that are not part of this test. RDMA was disabled on them prior to sending RDMA traffic.
   VERBOSE: RDMA traffic test SUCCESSFUL: RDMA traffic was sent to 192.168.2.222
   ```

5. Test pour le trafic RDMA à partir de l’ordinateur local à l’ordinateur distant.

    ```PowerShell
    Get-NetAdapter | ft –AutoSize
    ```

    _**Résultats :** _ 

    ```
    Name  InterfaceDescriptionifIndex Status   MacAddress LinkSpeed
    ----  --------------------------- ------   ---------- ---------
    vEthernet (SMB2)  Hyper-V Virtual Ethernet Adapter #4  45 Up   00-15-5D-30-AA-0380 Gbps
    vEthernet (SMB1)  Hyper-V Virtual Ethernet Adapter #3  41 Up   00-15-5D-30-AA-0280 Gbps
    ```

6. Effectuer le test de trafic RDMA sur la première carte virtuelle.    

   ```
   C:\TEST\Test-RDMA.PS1 -IfIndex 41 -IsRoCE $true -RemoteIpAddress 192.168.2.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
   ```

   _**Résultats :** _ 

   ```
   VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\diskspd.exe
   VERBOSE: The adapter vEthernet (SMB1) is a virtual adapter
   VERBOSE: Retrieving vSwitch bound to the virtual adapter
   VERBOSE: Found vSwitch: VMSTEST
   VERBOSE: Found the following physical adapter(s) bound to vSwitch: TEST-40G-1, TEST-40G-2
   VERBOSE: Underlying adapter is RoCE. Checking if QoS/DCB/PFC is configured on each physical adapter(s)
   VERBOSE: QoS/DCB/PFC configuration is correct.
   VERBOSE: RDMA configuration is correct.
   VERBOSE: Checking if remote IP address, 192.168.2.5, is reachable.
   VERBOSE: Remote IP 192.168.2.5 is reachable.
   VERBOSE: Disabling RDMA on adapters that are not part of this test. RDMA will be enabled on them later.
   VERBOSE: Testing RDMA traffic now for. Traffic will be sent in a parallel job. Job details:
   VERBOSE: 0 RDMA bytes written per second
   VERBOSE: 0 RDMA bytes sent per second
   VERBOSE: 0 RDMA bytes written per second
   VERBOSE: 15250197 RDMA bytes sent per second
   VERBOSE: 896320913 RDMA bytes written per second
   VERBOSE: 33947559 RDMA bytes sent per second
   VERBOSE: 912160540 RDMA bytes written per second
   VERBOSE: 34091930 RDMA bytes sent per second
   VERBOSE: Enabling RDMA on adapters that are not part of this test. RDMA was disabled on them prior to sending RDMA traffic.
   VERBOSE: RDMA traffic test SUCCESSFUL: RDMA traffic was sent to 192.168.2.5
   ```

7. Effectuer le test de trafic RDMA sur la deuxième carte virtuelle.

   ```PowerShell
   C:\TEST\Test-RDMA.PS1 -IfIndex 45 -IsRoCE $true -RemoteIpAddress 192.168.2.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
   ```

   _**Résultats :** _ 

   ```
   VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\diskspd.exe
   VERBOSE: The adapter vEthernet (SMB2) is a virtual adapter
   VERBOSE: Retrieving vSwitch bound to the virtual adapter
   VERBOSE: Found vSwitch: VMSTEST
   VERBOSE: Found the following physical adapter(s) bound to vSwitch: TEST-40G-1, TEST-40G-2
   VERBOSE: Underlying adapter is RoCE. Checking if QoS/DCB/PFC is configured on each physical adapter(s)
   VERBOSE: QoS/DCB/PFC configuration is correct.
   VERBOSE: RDMA configuration is correct.
   VERBOSE: Checking if remote IP address, 192.168.2.5, is reachable.
   VERBOSE: Remote IP 192.168.2.5 is reachable.
   VERBOSE: Disabling RDMA on adapters that are not part of this test. RDMA will be enabled on them later.
   VERBOSE: Testing RDMA traffic now for. Traffic will be sent in a parallel job. Job details:
   VERBOSE: 0 RDMA bytes written per second
   VERBOSE: 0 RDMA bytes sent per second
   VERBOSE: 385169487 RDMA bytes written per second
   VERBOSE: 33902277 RDMA bytes sent per second
   VERBOSE: 907354685 RDMA bytes written per second
   VERBOSE: 33923662 RDMA bytes sent per second
   VERBOSE: Enabling RDMA on adapters that are not part of this test. RDMA was disabled on them prior to sending RDMA traffic.
   VERBOSE: RDMA traffic test SUCCESSFUL: RDMA traffic was sent to 192.168.2.5
   ```

La dernière ligne dans cette sortie, « test de trafic RDMA réussi : Le trafic RDMA a été envoyé à 192.168.2.5, » montre que vous avez correctement configuré convergé la carte réseau sur votre carte.

## <a name="related-topics"></a>Rubriques connexes 

- [Configuration de carte réseau convergé avec une seule carte réseau](cnic-single.md)
- [Configuration de commutateur physique pour la carte réseau convergé](cnic-app-switch-config.md)
- [Résolution des problèmes convergé des Configurations de carte réseau](cnic-app-troubleshoot.md)
