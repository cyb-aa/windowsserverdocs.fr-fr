---
title: Carte réseau convergée dans une configuration de carte réseau en collaboration (Datacenter)
description: Dans cette rubrique, nous vous fournissons des instructions pour déployer une carte réseau convergée dans une configuration de carte réseau associée à l’aide de switch Embedded Teaming (SET).
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: f01546f8-c495-4055-8492-8806eee99862
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/17/2018
ms.openlocfilehash: e4c305a7c8c4c4618b0df1e1b2a646356d8f821f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356119"
---
# <a name="converged-nic-in-a-teamed-nic-configuration-datacenter"></a>Carte réseau convergée dans une configuration de carte réseau en collaboration (Datacenter)

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Dans cette rubrique, nous vous fournissons des instructions pour déployer une carte réseau convergée dans une configuration de carte réseau associée à \(l'\)aide de switch Embedded Association Set. 

L’exemple de configuration de cette rubrique décrit deux hôtes Hyper-V, l' **hôte 1 Hyper-v** et l' **hôte 2 Hyper-v**. Les deux hôtes ont deux cartes réseau. Sur chaque ordinateur hôte, une carte est connectée au sous-réseau 192.168.1. x/24, et une carte est connectée au sous-réseau 192.168.2. x/24.

![Ordinateurs hôtes Hyper-V](../../media/Converged-NIC/1-datacenter-test-conn.jpg)

## <a name="step-1-test-the-connectivity-between-source-and-destination"></a>Étape 1. Tester la connectivité entre la source et la destination

Vérifiez que la carte réseau physique peut se connecter à l’ordinateur hôte de destination.  Ce test illustre la connectivité à l’aide \(de\) la couche 3 (L3) ou de la couche IP, \(ainsi que du réseau local virtuel\)(VLAN) de réseaux \(locaux L2\) de couche 2.

1. Affichez les propriétés de la première carte réseau.

   ```PowerShell
   Get-NetAdapter -Name "Test-40G-1" | ft -AutoSize
   ```

   _**About**_


   |    Nom    |           InterfaceDescription           | IfIndex | Statut |    macAddress     | LinkSpeed |
   |------------|------------------------------------------|---------|--------|-------------------|-----------|
   | Test-40G-1 | Adaptateur Ethernet Mellanox ConnectX-3 Pro |   11    |   Up (Haut)   | E4-1D-2D-07-43-D0 |  40 Gbits/s  |

   ---

2. Affichez les propriétés supplémentaires de la première carte, y compris l’adresse IP. 

   ```PowerShell
   Get-NetIPAddress -InterfaceAlias "Test-40G-1"
   Get-NetIPAddress -InterfaceAlias "TEST-40G-1" | Where-Object {$_.AddressFamily -eq "IPv4"} | fl InterfaceAlias,IPAddress
   ```

   _**About**_


   |   Paramètre    |    Value    |
   |----------------|-------------|
   |   AdresseIP    | 192.168.1.3 |
   | InterfaceIndex |     11      |
   | InterfaceAlias | Test-40G-1  |
   | AddressFamily  |    IPv4/IPv6     |
   |      Type      |   Monodiffusion   |
   |  PrefixLength  |     24      |

   ---

3. Affichez les propriétés de la deuxième carte réseau.

   ```PowerShell
   Get-NetAdapter -Name "Test-40G-2" | ft -AutoSize
   ```

   _**About**_


   |    Nom    |          InterfaceDescription           | IfIndex | Statut |    macAddress     | LinkSpeed |
   |------------|-----------------------------------------|---------|--------|-------------------|-----------|
   | TEST-40G-2 | Mellanox ConnectX-3 Ethernet Pro A... #2 |   13    |   Up (Haut)   | E4-1D-2D-07-40-70 |  40 Gbits/s  |

   ---

4. Affichez les propriétés supplémentaires pour le deuxième adaptateur, y compris l’adresse IP.

   ```PowerShell
   Get-NetIPAddress -InterfaceAlias "Test-40G-2"
   Get-NetIPAddress -InterfaceAlias "Test-40G-2" | Where-Object {$_.AddressFamily -eq "IPv4"} | fl InterfaceAlias,IPAddress
   ```

   _**About**_


   |   Paramètre    |    Value    |
   |----------------|-------------|
   |   AdresseIP    | 192.168.2.3 |
   | InterfaceIndex |     13      |
   | InterfaceAlias | TEST-40G-2  |
   | AddressFamily  |    IPv4/IPv6     |
   |      Type      |   Monodiffusion   |
   |  PrefixLength  |     24      |

   ---

5. Vérifiez que les autres associations de cartes réseau ou définir le membre pNICs ont une adresse IP valide.<p>Utilisez un sous-réseau \(distinct, xxx.xxx. **2**. xxx vs xxx.xxx. **1**. xxx\), pour faciliter l’envoi à partir de cet adaptateur vers la destination. Dans le cas contraire, si vous localisez les deux pNICs sur le même sous-réseau, la charge de la pile TCP/IP de Windows équilibre entre les interfaces et la validation simple devient plus complexe.


## <a name="step-2-ensure-that-source-and-destination-can-communicate"></a>Étape 2. S’assurer que la source et la destination peuvent communiquer

Dans cette étape, nous utilisons la commande **test-NetConnection** Windows PowerShell, mais si vous le souhaitez, vous pouvez utiliser la commande **ping** . 

1. Vérifiez la communication bidirectionnelle.

   ```PowerShell
   Test-NetConnection 192.168.1.5
   ```

   _**About**_


   |        Paramètre         |    Value    |
   |--------------------------|-------------|
   |       ComputerName       | 192.168.1.5 |
   |      RemoteAddress       | 192.168.1.5 |
   |      InterfaceAlias      | Test-40G-1  |
   |      SourceAddress       | 192.168.1.3 |
   |      PingSucceeded       |    False    |
   | RTT \(PingReplyDetails\) |    0 ms     |

   ---

   Dans certains cas, vous devrez peut-être désactiver le pare-feu Windows avec fonctions avancées de sécurité pour effectuer ce test. Si vous désactivez le pare-feu, gardez à l’esprit la sécurité et assurez-vous que votre configuration répond aux exigences de sécurité de votre organisation.

2. Désactivez tous les profils de pare-feu.

   ```PowerShell
   Set-NetFirewallProfile -All -Enabled False
   ```

3. Après avoir désactivé les profils de pare-feu, testez à nouveau la connexion. 

   ```PowerShell
   Test-NetConnection 192.168.1.5
   ```

   _**About**_


   |        Paramètre         |    Value    |
   |--------------------------|-------------|
   |       ComputerName       | 192.168.1.5 |
   |      RemoteAddress       | 192.168.1.5 |
   |      InterfaceAlias      | Test-40G-1  |
   |      SourceAddress       | 192.168.1.3 |
   |      PingSucceeded       |    False    |
   | RTT \(PingReplyDetails\) |    0 ms     |

   ---


4. Vérifiez la connectivité des cartes réseau supplémentaires. Répétez les étapes précédentes pour tous les pNICs suivants inclus dans la carte réseau ou définissez l’équipe.

   ```PowerShell    
   Test-NetConnection 192.168.2.5
   ```

   _**About**_


   |        Paramètre         |    Value    |
   |--------------------------|-------------|
   |       ComputerName       | 192.168.2.5 |
   |      RemoteAddress       | 192.168.2.5 |
   |      InterfaceAlias      | Test-40G-2  |
   |      SourceAddress       | 192.168.2.3 |
   |      PingSucceeded       |    False    |
   | RTT \(PingReplyDetails\) |    0 ms     |

   ---

## <a name="step-3-configure-the-vlan-ids-for-nics-installed-in-your-hyper-v-hosts"></a>Étape 3. Configuration des ID de réseau local virtuel pour les cartes réseau installées dans vos ordinateurs hôtes Hyper-V

De nombreuses configurations réseau utilisent des réseaux locaux virtuels. Si vous envisagez d’utiliser des réseaux locaux virtuels dans votre réseau, vous devez répéter le test précédent avec des réseaux locaux virtuels configurés.

Pour cette étape, les cartes réseau sont en mode d' **accès** . Toutefois, lorsque vous créez un commutateur \(virtuel Hyper-V vswitch\) plus loin dans ce guide, les propriétés du réseau local virtuel sont appliquées au niveau du port vswitch. 

Étant donné qu’un commutateur peut héberger plusieurs réseaux locaux virtuels, il est nécessaire que \(le\) commutateur physique du rack soit connecté au port auquel l’ordinateur hôte est connecté en mode Trunk.

>[!NOTE]
>Consultez la documentation de votre commutateur pour obtenir des instructions sur la configuration du mode Trunk sur le commutateur.

L’illustration suivante montre deux hôtes Hyper-V avec deux cartes réseau, chacune disposant de réseaux locaux virtuels 101 et VLAN 102 configurés dans les propriétés de la carte réseau.

![Configurer des réseaux locaux virtuels](../../media/Converged-NIC/2-datacenter-configure-vlans.jpg)


>[!TIP]
>Conformément aux normes de \(mise en réseau IEEE\) de l’Institute of Electrical and Electronics Engineers, les\) propriétés QoS de qualité de service \(de la carte réseau physique agissent sur l’en-tête p 802.1 qui est incorporé dans l’en- \(tête\) VLAN 802.1 q lorsque vous configurez l’ID de réseau local virtuel.

1. Configurez l’ID de réseau local virtuel sur la première carte d’interface réseau, test-40G-1.

   ```PowerShell    
   Set-NetAdapterAdvancedProperty -Name "Test-40G-1" -RegistryKeyword VlanID -RegistryValue "101"
   Get-NetAdapterAdvancedProperty -Name "Test-40G-1" | Where-Object {$_.RegistryKeyword -eq "VlanID"} | ft -AutoSize
   ```

   _**About**_   


   |    Nom    | DisplayName | DisplayValue | RegistryKeyword | RegistryValue |
   |------------|-------------|--------------|-----------------|---------------|
   | TEST-40G-1 |   ID du réseau local virtuel   |     101      |     VlanID      |     {101}     |

   ---

2. Redémarrez la carte réseau pour appliquer l’ID de réseau local virtuel.

   ```PowerShell
   Restart-NetAdapter -Name "Test-40G-1"
   ```

3. Assurez-vous que l’État est **up**.

   ```PowerShell
   Get-NetAdapter -Name "Test-40G-1" | ft -AutoSize
   ```

   _**About**_


   |    Nom    |          InterfaceDescription           | IfIndex | Statut |    macAddress     | LinkSpeed |
   |------------|-----------------------------------------|---------|--------|-------------------|-----------|
   | Test-40G-1 | Mellanox ConnectX-3 Pro Ethernet Ada... |   11    |   Up (Haut)   | E4-1D-2D-07-43-D0 |  40 Gbits/s  |

   ---

4. Configurez l’ID de réseau local virtuel sur la deuxième carte d’interface réseau, test-40G-2.

   ```PowerShell    
   Set-NetAdapterAdvancedProperty -Name "Test-40G-2" -RegistryKeyword VlanID -RegistryValue "102"
   Get-NetAdapterAdvancedProperty -Name "Test-40G-2" | Where-Object {$_.RegistryKeyword -eq "VlanID"} | ft -AutoSize
   ``` 

   _**About**_


   |    Nom    | DisplayName | DisplayValue | RegistryKeyword | RegistryValue |
   |------------|-------------|--------------|-----------------|---------------|
   | TEST-40G-2 |   ID du réseau local virtuel   |     102      |     VlanID      |     {102}     |

   ---

5. Redémarrez la carte réseau pour appliquer l’ID de réseau local virtuel.

   ```PowerShell
   Restart-NetAdapter -Name "Test-40G-2" 
   ```

6. Assurez-vous que l’État est **up**.

   ```PowerShell
   Get-NetAdapter -Name "Test-40G-1" | ft -AutoSize
   ```

   _**About**_


   |    Nom    |          InterfaceDescription           | IfIndex | Statut |    macAddress     | LinkSpeed |
   |------------|-----------------------------------------|---------|--------|-------------------|-----------|
   | Test-40G-2 | Mellanox ConnectX-3 Pro Ethernet Ada... |   11    |   Up (Haut)   | E4-1D-2D-07-43-D1 |  40 Gbits/s  |

   ---

   >[!IMPORTANT]
   >Le redémarrage de l’appareil peut prendre plusieurs secondes et être disponible sur le réseau. 

7. Vérifiez la connectivité de la première carte d’interface réseau, test-40G-1.<p>Si la connectivité échoue, inspectez la configuration du réseau local virtuel du commutateur ou la participation à la destination dans le même réseau local virtuel. 

   ```PowerShell
   Test-NetConnection 192.168.1.5
   ```

   _**About**_   


   |        Paramètre         |    Value    |
   |--------------------------|-------------|
   |       ComputerName       | 192.168.1.5 |
   |      RemoteAddress       | 192.168.1.5 |
   |      InterfaceAlias      | Test-40G-1  |
   |      SourceAddress       | 192.168.1.5 |
   |      PingSucceeded       |    True     |
   | RTT \(PingReplyDetails\) |    0 ms     |

   ---

8. Vérifiez la connectivité de la première carte d’interface réseau, test-40G-2.<p>Si la connectivité échoue, inspectez la configuration du réseau local virtuel du commutateur ou la participation à la destination dans le même réseau local virtuel.

   ```PowerShell    
   Test-NetConnection 192.168.2.5
   ```

   _**About**_    


   |        Paramètre         |    Value    |
   |--------------------------|-------------|
   |       ComputerName       | 192.168.2.5 |
   |      RemoteAddress       | 192.168.2.5 |
   |      InterfaceAlias      | Test-40G-2  |
   |      SourceAddress       | 192.168.2.3 |
   |      PingSucceeded       |    True     |
   | RTT \(PingReplyDetails\) |    0 ms     |

   ---

   >[!IMPORTANT]
   >Il n’est pas rare de rencontrer un échec de **test-NetConnection** ou de ping immédiatement après avoir exécuté **restart-NetAdapter**.  Par conséquent, attendez que la carte réseau soit complètement initialisée, puis réessayez.
   >
   >Si les connexions de réseau local virtuel 101 fonctionnent, mais que les connexions VLAN 102 ne le sont pas, le problème peut être que le commutateur doit être configuré pour autoriser le trafic de port sur le réseau local virtuel souhaité. Vous pouvez le vérifier en définissant temporairement les adaptateurs défaillants sur VLAN 101 et en répétant le test de connectivité.


   L’illustration suivante montre vos hôtes Hyper-V après avoir correctement configuré les réseaux locaux virtuels.

   ![Configurer la qualité de service](../../media/Converged-NIC/3-datacenter-configure-qos.jpg)


## <a name="step-4-configure-quality-of-service-qos"></a>Étape 4. Configurer la qualité de \(service (QoS)\)

>[!NOTE]
>Vous devez effectuer toutes les étapes de configuration DCB et QoS suivantes sur tous les ordinateurs hôtes qui sont destinés à communiquer entre eux.

1. Installez Data Center \(Bridging\) DCB sur chacun de vos ordinateurs hôtes Hyper-V.

   - **Facultatif** pour les configurations réseau qui utilisent iWarp.
   - **Requis** pour les configurations réseau qui utilisent RoCE \(n’importe\) quelle version pour les services RDMA.

   ```PowerShell
   Install-WindowsFeature Data-Center-Bridging
   ```

   _**About**_


   | Succès | Redémarrage nécessaire | Code de sortie |     Résultat de la fonctionnalité     |
   |---------|----------------|-----------|------------------------|
   |  True   |       Non       |  Succès  | {Data Center Bridging} |

   ---

2. Définir les stratégies QoS pour SMB-direct :

   - **Facultatif** pour les configurations réseau qui utilisent iWarp.
   - **Requis** pour les configurations réseau qui utilisent RoCE \(n’importe\) quelle version pour les services RDMA.

   Dans l’exemple de commande ci-dessous, la valeur « 3 » est arbitraire. Vous pouvez utiliser n’importe quelle valeur comprise entre 1 et 7 tant que vous utilisez régulièrement la même valeur tout au long de la configuration des stratégies QoS.

   ```PowerShell
   New-NetQosPolicy "SMB" -NetDirectPortMatchCondition 445 -PriorityValue8021Action 3
   ```

   _**About**_


   |   Paramètre    |          Value           |
   |----------------|--------------------------|
   |      Nom      |           SMB            |
   |     Propriétaire      | Ordinateur \(stratégie de groupe\) |
   | NetworkProfile |           Tous            |
   |   Priorité   |           127            |
   |   JobObject    |          &nbsp;          |
   | NetDirectPort  |           445            |
   | PriorityValue  |            3             |

   ---

3. Définissez des stratégies QoS supplémentaires pour d’autres trafics sur l’interface.   

   ```PowerShell
   New-NetQosPolicy "DEFAULT" -Default -PriorityValue8021Action 0
   ```

   _**About**_   


   |   Paramètre    |          Value           |
   |----------------|--------------------------|
   |      Nom      |         PAR DÉFAUT          |
   |     Propriétaire      | Ordinateur \(stratégie de groupe\) |
   | NetworkProfile |           Tous            |
   |   Priorité   |           127            |
   |    Modèle    |         Par défaut          |
   |   JobObject    |          &nbsp;          |
   | PriorityValue  |            0             |

   ---

4. Activez le **contrôle de flux Priority** pour le trafic SMB, ce qui n’est pas nécessaire pour iWarp.

   ```PowerShell
   Enable-NetQosFlowControl -priority 3
   Get-NetQosFlowControl
   ```

   _**About**_


   | Priority | Enabled | PolicySet | IfIndex | IfAlias |
   |----------|---------|-----------|---------|---------|
   |    0     |  False  |  Global   | &nbsp;  | &nbsp;  |
   |    1     |  False  |  Global   | &nbsp;  | &nbsp;  |
   |    2     |  False  |  Global   | &nbsp;  | &nbsp;  |
   |    3     |  True   |  Global   | &nbsp;  | &nbsp;  |
   |    4     |  False  |  Global   | &nbsp;  | &nbsp;  |
   |    5     |  False  |  Global   | &nbsp;  | &nbsp;  |
   |    6\.     |  False  |  Global   | &nbsp;  | &nbsp;  |
   |    7     |  False  |  Global   | &nbsp;  | &nbsp;  |

   ---

   >**Important** Si vos résultats ne correspondent pas à ces résultats, car les éléments autres que 3 ont une valeur Enabled égale à true, vous devez désactiver **FlowControl** pour ces classes.
   >
   >```PowerShell
   >Disable-NetQosFlowControl -priority 0,1,2,4,5,6,7
   >Get-NetQosFlowControl
   >```
   >Dans les configurations plus complexes, les autres classes de trafic peuvent nécessiter un contrôle de flux, mais ces scénarios ne sont pas abordés dans ce guide.


5. Activez la qualité de service pour la première carte d’interface réseau, test-40G-1.

   ```PowerShell
   Enable-NetAdapterQos -InterfaceAlias "Test-40G-1"
   Get-NetAdapterQos -Name "Test-40G-1"

   Name: TEST-40G-1 
   Enabled: True
   ```
   _**Fonctionnalités**:_   


   |      Paramètre      |   Matériel   |   Actuelle    |
   |---------------------|--------------|--------------|
   |    MacSecBypass     | NotSupported | NotSupported |
   |     DcbxSupport     |     Aucune     |     Aucune     |
   | NumTCs (max/ETS/PFC) |    8/8/8     |    8/8/8     |

   ---

   _**OperationalTrafficClasses**:_    


   | CT |  AUTORITÉ TSA   | La | Priorité |
   |----|--------|-----------|------------|
   | 0  | Strict |  &nbsp;   |    0-7     |

   ---

   _**OperationalFlowControl**:_  

   Priorité 3 activée  

   _**OperationalClassifications**:_  


   | Protocol  | Port/type | Priority |
   |-----------|-----------|----------|
   |  Par défaut  |  &nbsp;   |    0     |
   | NetDirect |    445    |    3     |

   ---

6. Activez la qualité de service pour la deuxième carte d’interface réseau, test-40G-2.

   ```PowerShell
   Enable-NetAdapterQos -InterfaceAlias "Test-40G-2"
   Get-NetAdapterQos -Name "Test-40G-2"

   Name: TEST-40G-2 
   Enabled: True 
   ```

   _**Fonctionnalités**:_ 


   |      Paramètre      |   Matériel   |   Actuelle    |
   |---------------------|--------------|--------------|
   |    MacSecBypass     | NotSupported | NotSupported |
   |     DcbxSupport     |     Aucune     |     Aucune     |
   | NumTCs (max/ETS/PFC) |    8/8/8     |    8/8/8     |

   ---

   _**OperationalTrafficClasses**:_  


   | CT |  AUTORITÉ TSA   | La | Priorité |
   |----|--------|-----------|------------|
   | 0  | Strict |  &nbsp;   |    0-7     |

   ---

    _**OperationalFlowControl**:_  

    Priorité 3 activée  

   _**OperationalClassifications**:_  


   | Protocol  | Port/type | Priority |
   |-----------|-----------|----------|
   |  Par défaut  |  &nbsp;   |    0     |
   | NetDirect |    445    |    3     |

   ---


7. Réserver la moitié de la bande passante à RDMA SMB direct \(\)

   ```PowerShell
   New-NetQosTrafficClass "SMB" -priority 3 -bandwidthpercentage 50 -algorithm ETS
   ```

   _**About**_  


   | Nom | Algorithm | Bande passante (%) | Priority | PolicySet | IfIndex | IfAlias |
   |------|-----------|--------------|----------|-----------|---------|---------|
   | SMB  |    STE    |      50      |    3     |  Global   | &nbsp;  | &nbsp;  |

   ---

8. Affichez les paramètres de réservation de bande passante :   

   ```PowerShell
   Get-NetQosTrafficClass | ft -AutoSize
   ```

   _**About**_  


   |   Nom    | Algorithm | Bande passante (%) | Priority | PolicySet | IfIndex | IfAlias |
   |-----------|-----------|--------------|----------|-----------|---------|---------|
   | Valeurs |    STE    |      50      | 0-2, 4-7  |  Global   | &nbsp;  | &nbsp;  |
   |    SMB    |    STE    |      50      |    3     |  Global   | &nbsp;  | &nbsp;  |

   ---

9. Facultatif Créez deux classes de trafic supplémentaires pour le trafic IP du locataire. 

   >[!TIP]
   >Vous pouvez omettre les valeurs « IP1 » et « IP2 ».

   ```PowerShell
   New-NetQosTrafficClass "IP1" -Priority 1 -bandwidthpercentage 10 -algorithm ETS
   ```

   _**About**_


   | Nom | Algorithm | Bande passante (%) | Priority | PolicySet | IfIndex | IfAlias |
   |------|-----------|--------------|----------|-----------|---------|---------|
   | IP1  |    STE    |      10      |    1     |  Global   | &nbsp;  | &nbsp;  |

   ---

   ```PowerShell
   New-NetQosTrafficClass "IP2" -Priority 2 -bandwidthpercentage 10 -algorithm ETS
   ```

   _**About**_


   | Nom | Algorithm | Bande passante (%) | Priority | PolicySet | IfIndex | IfAlias |
   |------|-----------|--------------|----------|-----------|---------|---------|
   | IP2  |    STE    |      10      |    2     |  Global   | &nbsp;  | &nbsp;  |

   ---

10. Affichez les classes de trafic QoS.

    ```PowerShell
    Get-NetQosTrafficClass | ft -AutoSize
    ```

    _**About**_


    |   Nom    | Algorithm | Bande passante (%) | Priority | PolicySet | IfIndex | IfAlias |
    |-----------|-----------|--------------|----------|-----------|---------|---------|
    | Valeurs |    STE    |      30      |  0,4-7   |  Global   | &nbsp;  | &nbsp;  |
    |    SMB    |    STE    |      50      |    3     |  Global   | &nbsp;  | &nbsp;  |
    |    IP1    |    STE    |      10      |    1     |  Global   | &nbsp;  | &nbsp;  |
    |    IP2    |    STE    |      10      |    2     |  Global   | &nbsp;  | &nbsp;  |

    ---

11. Facultatif Remplacez le débogueur.<p>Par défaut, le débogueur attaché bloque NetQos. 

    ```PowerShell
    Set-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" AllowFlowControlUnderDebugger -type DWORD -Value 1 –Force
    Get-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" | ft AllowFlowControlUnderDebugger
    ```

    _**About**_  

    ```
    AllowFlowControlUnderDebugger
    -----------------------------
    1
    ```

## <a name="step-5-verify-the-rdma-configuration-mode-1"></a>Étape 5. Vérifier le mode de \(configuration RDMA 1\) 

Vous voulez vous assurer que l’infrastructure est correctement configurée avant de créer un vswitch et de passer au \(mode RDMA\)2.

L’illustration suivante montre l’état actuel des ordinateurs hôtes Hyper-V.

![Tester RDMA](../../media/Converged-NIC/4-datacenter-test-rdma.jpg)


1. Vérifiez la configuration RDMA.

   ```PowerShell
   Get-NetAdapterRdma | ft -AutoSize
   ```

   _**About**_


   |    Nom    |        InterfaceDescription        | Enabled |
   |------------|------------------------------------|---------|
   | TEST-40G-1 | Mellanox ConnectX-4 adaptateur VPI #2 |  True   |
   | TEST-40G-2 |  Mellanox ConnectX-4 adaptateur VPI   |  True   |

   ---

2. Déterminez la valeur **ifIndex** de vos adaptateurs cibles.<p>Vous utilisez cette valeur dans les étapes suivantes lorsque vous exécutez le script que vous téléchargez.   

   ```PowerShell
   Get-NetIPConfiguration -InterfaceAlias "TEST*" | ft InterfaceAlias,InterfaceIndex,IPv4Address
   ```

   _**About**_


   | InterfaceAlias | InterfaceIndex |  IPv4Address  |
   |----------------|----------------|---------------|
   |   TEST-40G-1   |       14       | 192.168.1.3 |
   |   TEST-40G-2   |       13       | {192.168.2.3} |

   ---

3. Téléchargez l' [utilitaire DiskSpd. exe](https://aka.ms/diskspd) et extrayez-le dans C:\test\.

4. Téléchargez le [script PowerShell test-RDMA](https://github.com/Microsoft/SDN/blob/master/Diagnostics/Test-Rdma.ps1) dans un dossier de test sur votre disque local, par exemple, C:\test\.

5. Exécutez le script PowerShell **test-RDMA. ps1** pour transmettre la valeur ifIndex au script, ainsi que l’adresse IP de la première carte distante sur le même réseau local virtuel.<p>Dans cet exemple, le script transmet la valeur **ifIndex** de 14 à l’adresse IP de la carte réseau distante 192.168.1.5.

   ```PowerShell
   C:\TEST\Test-RDMA.PS1 -IfIndex 14 -IsRoCE $true -RemoteIpAddress 192.168.1.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
   ```

   _**About**_ 

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
   >Si le trafic RDMA échoue, pour le cas RoCE, consultez la configuration de votre commutateur pour obtenir les paramètres PFC/ETS appropriés qui doivent correspondre aux paramètres de l’ordinateur hôte. Reportez-vous à la section QoS dans ce document pour obtenir les valeurs de référence.

6. Exécutez le script PowerShell **test-RDMA. ps1** pour transmettre la valeur ifIndex au script, ainsi que l’adresse IP de la deuxième carte à distance sur le même réseau local virtuel.<p>Dans cet exemple, le script transmet la valeur **ifIndex** de 13 à l’adresse IP de la carte réseau distante 192.168.2.5.

   ```PowerShell
   C:\TEST\Test-RDMA.PS1 -IfIndex 13 -IsRoCE $true -RemoteIpAddress 192.168.2.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
   ```

   _**About**_ 

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

## <a name="step-6-create-a-hyper-v-vswitch-on-your-hyper-v-hosts"></a>Étape 6. Créer un vSwitch Hyper-V sur vos ordinateurs hôtes Hyper-V


L’illustration suivante montre l’hôte 1 Hyper-V avec un vSwitch.

![Créer un commutateur virtuel Hyper-V](../../media/Converged-NIC/5-datacenter-create-vswitch.jpg)

1. Créez un vSwitch en mode SET sur l’hôte 1 Hyper-V.

   ```PowerShell
   New-VMSwitch –Name "VMSTEST" –NetAdapterName "TEST-40G-1","TEST-40G-2" -EnableEmbeddedTeaming $true -AllowManagementOS $true
   ```

   _**Venir**_


   |  Nom   | SwitchType | Paramètre netadapterinterfacedescription |
   |---------|------------|--------------------------------|
   | VMSTEST |  Externe  |        Association-interface        |

   ---

2. Affichez l’Association de cartes physiques dans SET.

   ```PowerShell
   Get-VMSwitchTeam -Name "VMSTEST" | fl
   ```

   _**About**_  

   ```
   Name: VMSTEST  
   Id: ad9bb542-dda2-4450-a00e-f96d44bdfbec  
   NetAdapterInterfaceDescription: {Mellanox ConnectX-3 Pro Ethernet Adapter, Mellanox ConnectX-3 Pro Ethernet Adapter #2}  
   TeamingMode: SwitchIndependent  
   LoadBalancingAlgorithm: Dynamic   
   ```

3. Afficher deux vues de l’hôte carte réseau virtuelle

   ```PowerShell
    Get-NetAdapter
   ```

   _**About**_


   |        Nom         |        InterfaceDescription         | IfIndex | Statut |    macAddress     | LinkSpeed |
   |---------------------|-------------------------------------|---------|--------|-------------------|-----------|
   | vEthernet (VMSTEST) | Carte Ethernet virtuelle Hyper-V #2 |   28    |   Up (Haut)   | E4-1D-2D-07-40-71 |  80 Gbits/s  |

   ---

4. Affichez les propriétés supplémentaires de l’hôte carte réseau virtuelle. 

   ```PowerShell
   Get-VMNetworkAdapter -ManagementOS
   ```

   _**About**_


   |  Nom   | IsManagementOs | VMName  |  SwitchName  | macAddress | Statut | AdressesIP |
   |---------|----------------|---------|--------------|------------|--------|-------------|
   | VMSTEST |      True      | VMSTEST | E41D2D074071 |    OK    | &nbsp; |             |

   ---


5. Testez la connexion réseau à l’adaptateur réseau local virtuel 101 distant.

   ```PowerShell
   Test-NetConnection 192.168.1.5 
   ```

   _**About**_  

   ```
   WARNING: Ping to 192.168.1.5 failed -- Status: DestinationHostUnreachable

   ComputerName   : 192.168.1.5
   RemoteAddress  : 192.168.1.5
   InterfaceAlias : vEthernet (CORP-External-Switch)
   SourceAddress  : 10.199.48.170
   PingSucceeded  : False
   PingReplyDetails (RTT) : 0 ms
   ```

## <a name="step-7-remove-the-access-vlan-setting"></a>Étape 7. Supprimer le paramètre d’accès VLAN

Dans cette étape, vous supprimez le paramètre de réseau local virtuel d’accès de la carte réseau physique et vous définissez VLANID à l’aide de vSwitch.

Vous devez supprimer le paramètre d’accès VLAN pour empêcher le marquage automatique du trafic de sortie avec l’ID de réseau local virtuel incorrect et de filtrer le trafic d’entrée qui ne correspond pas à l’ID de réseau local virtuel d’accès.

1. Supprimez le paramètre.

   ```PowerShell
   Set-NetAdapterAdvancedProperty -Name "Test-40G-1" -RegistryKeyword VlanID -RegistryValue "0"
   Set-NetAdapterAdvancedProperty -Name "Test-40G-2" -RegistryKeyword VlanID -RegistryValue "0"
   ```

2. Définissez l’ID de réseau local virtuel.

   ```PowerShell
   Set-VMNetworkAdapterVlan -VMNetworkAdapterName "VMSTEST" -VlanId "101" -Access -ManagementOS
   Get-VMNetworkAdapterVlan -ManagementOS -VMNetworkAdapterName "VMSTEST"
   ```

   _**About**_  

   ```
   VMName VMNetworkAdapterName Mode   VlanList
   ------ -------------------- ----   --------
          VMSTEST              Access 101     
   ```


3. Testez la connexion réseau.

   ```PowerShell
   Test-NetConnection 192.168.1.5
   ```

   _**About**_   

   ```
   ComputerName   : 192.168.1.5
   RemoteAddress  : 192.168.1.5
   InterfaceAlias : vEthernet (VMSTEST)
   SourceAddress  : 192.168.1.3
   PingSucceeded  : True
   PingReplyDetails (RTT) : 0 ms
   ```

   >**Important** Si vos résultats ne sont pas similaires aux résultats de l’exemple et si le test ping échoue avec le message «AVERTISSEMENT : Échec de la commande ping sur 192.168.1.5--État : DestinationHostUnreachable, confirmez que « vEthernet (VMSTEST) » possède l’adresse IP appropriée.
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


4. Renommez la carte réseau de gestion.

   ```PowerShell
   Rename-VMNetworkAdapter -ManagementOS -Name “VMSTEST” -NewName “MGT”
   Get-VMNetworkAdapter -ManagementOS
   ```

   _**About**_ 


   |         Nom         | IsManagementOs | VMName |      SwitchName      |  macAddress  | Statut | AdressesIP |
   |----------------------|----------------|--------|----------------------|--------------|--------|-------------|
   | CORP-External-Switch |      True      | &nbsp; | CORP-External-Switch | 001B785768AA |  OK  |   &nbsp;    |
   |         MAGASIN          |      True      | &nbsp; |       VMSTEST        | E41D2D074071 |  OK  |   &nbsp;    |

   ---

5. Affichez les propriétés de carte réseau supplémentaires.

   ```PowerShell
   Get-NetAdapter
   ```

   _**About**_


   |      Nom       |        InterfaceDescription         | IfIndex | Statut |    macAddress     | LinkSpeed |
   |-----------------|-------------------------------------|---------|--------|-------------------|-----------|
   | vEthernet (gestion) | Carte Ethernet virtuelle Hyper-V #2 |   28    |   Up (Haut)   | E4-1D-2D-07-40-71 |  80 Gbits/s  |

   ---

## <a name="step-8-test-hyper-v-vswitch-rdma"></a>Étape 8. Tester le vSwitch RDMA Hyper-V

L’illustration suivante montre l’état actuel de vos ordinateurs hôtes Hyper-V, y compris le vSwitch sur l’hôte 1 Hyper-V.

![Tester le commutateur virtuel Hyper-V](../../media/Converged-NIC/6-datacenter-test-vswitch-rdma.jpg)

1. Définissez le balisage de priorité sur l’hôte carte réseau virtuelle pour compléter les paramètres VLAN précédents.

   ```PowerShell    
   Set-VMNetworkAdapter -ManagementOS -Name "MGT" -IeeePriorityTag on
   Get-VMNetworkAdapter -ManagementOS -Name "MGT" | fl Name,IeeePriorityTag
   ```

   _**About**_  

   nomme MAGASIN  
   IeeePriorityTag :  Activé  

2. Créez deux cartes réseau virtuelles hôtes pour RDMA et connectez-les au VMSTEST vSwitch.

   ```PowerShell    
   Add-VMNetworkAdapter –SwitchName "VMSTEST" –Name SMB1 –ManagementOS
   Add-VMNetworkAdapter –SwitchName "VMSTEST" –Name SMB2 –ManagementOS
   ```

3. Affichez les propriétés de la carte réseau de gestion.

   ```PowerShell    
   Get-VMNetworkAdapter -ManagementOS
   ```

   _**About**_ 


   |         Nom         | IsManagementOs |        VMName        |  SwitchName  | macAddress | Statut | AdressesIP |
   |----------------------|----------------|----------------------|--------------|------------|--------|-------------|
   | CORP-External-Switch |      True      | CORP-External-Switch | 001B785768AA |    OK    | &nbsp; |             |
   |         Magasin          |      True      |       VMSTEST        | E41D2D074071 |    OK    | &nbsp; |             |
   |         SMB1         |      True      |       VMSTEST        | 00155D30AA00 |    OK    | &nbsp; |             |
   |         SMB2         |      True      |       VMSTEST        | 00155D30AA01 |    OK    | &nbsp; |             |

   ---

## <a name="step-9-assign-an-ip-address-to-the-smb-host-vnics-vethernet-smb1-and-vethernet-smb2"></a>Étape 9 : Affecter une adresse IP à l’hôte SMB cartes réseau virtuelles vEthernet \(SMB1\) et vEthernet \(SMB2\)

Les adaptateurs physiques TEST-40G-1 et TEST-40G-2 disposent toujours d’un réseau local virtuel d’accès de 101 et de 102 configurés. Pour cette raison, les adaptateurs marquent le trafic et le test Ping s’effectue correctement. Auparavant, vous configurez les deux ID de réseau local virtuel pNIC sur zéro, puis vous définissez VMSTEST vSwitch sur VLAN 101. Après cela, vous avez toujours pu exécuter une commande ping sur la carte réseau local virtuel 101 à distance à l’aide de l’carte réseau virtuelle de gestion, mais il n’existe actuellement aucun membre VLAN 102.



1. Supprimez le paramètre de réseau local virtuel d’accès de la carte réseau physique pour empêcher le balisage automatique du trafic de sortie avec l’ID de réseau local virtuel incorrect et l’empêcher de filtrer le trafic d’entrée qui ne correspond pas à l’ID de réseau local virtuel d’accès.

   ```PowerShell    
   New-NetIPAddress -InterfaceAlias "vEthernet (SMB1)" -IPAddress 192.168.2.111 -PrefixLength 24
   ```

   _**About**_  

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

2. Testez l’adaptateur réseau local virtuel 102 distant.

   ```PowerShell
   Test-NetConnection 192.168.2.5 
   ```

   _**About**_  

   ```
   ComputerName   : 192.168.2.5
   RemoteAddress  : 192.168.2.5
   InterfaceAlias : vEthernet (SMB1)
   SourceAddress  : 192.168.2.111
   PingSucceeded  : True
   PingReplyDetails (RTT) : 0 ms
   ```

3. Ajoutez une nouvelle adresse IP pour l’interface \(vEthernet\)SMB2.

   ```PowerShell
   New-NetIPAddress -InterfaceAlias "vEthernet (SMB2)" -IPAddress 192.168.2.222 -PrefixLength 24 
   ```

   _**About**_ 

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


5. Placez le cartes réseau virtuelles d’hôte RDMA sur le réseau local virtuel préexistant 102.

   ```PowerShell
   Set-VMNetworkAdapterVlan -VMNetworkAdapterName "SMB1" -VlanId "102" -Access -ManagementOS
   Set-VMNetworkAdapterVlan -VMNetworkAdapterName "SMB2" -VlanId "102" -Access -ManagementOS

   Get-VMNetworkAdapterVlan -ManagementOS
   ```

   _**About**_ 

   ```   
   VMName VMNetworkAdapterName Mode VlanList
   ------ -------------------- ---- --------
      SMB1 Access   102 
      Mgt  Access   101 
      SMB2 Access   102 
      CORP-External-Switch Untagged
   ```

6. Inspectez le mappage entre SMB1 et SMB2 avec les cartes réseau physiques sous-jacentes dans l’équipe de jeu de vSwitch.<p>L’Association des carte réseau virtuelle hôtes aux cartes réseau physiques est aléatoire et peut faire l’objet d’un rééquilibrage lors de la création et de la destruction. Dans ce cas, vous pouvez utiliser un mécanisme indirect pour vérifier l’Association actuelle. Les adresses MAC des adresses SMB1 et SMB2 sont associées au membre de l’équipe de cartes réseau TEST-40G-2. Cette solution n’est pas idéale, car test-40G-1 n’a pas de carte réseau virtuelle d’hôte SMB associé et n’autorise pas l’utilisation du trafic RDMA sur le lien tant qu’un hôte SMB carte réseau virtuelle n’est pas mappé à celui-ci.

   ```PowerShell    
   Get-NetAdapterVPort (Preferred)

   Get-NetAdapterVmqQueue
   ```

   _**About**_ 

   ```
   Name   QueueID MacAddressVlanID Processor VmFriendlyName
   ----   ------- ---------------- --------- --------------
   TEST-40G-1 1   E4-1D-2D-07-40-71 1010:17
   TEST-40G-2 1   00-15-5D-30-AA-00 1020:17
   TEST-40G-2 2   00-15-5D-30-AA-01 1020:17
   ```

7. Affichez les propriétés de la carte réseau d’ordinateur virtuel.

   ```PowerShell
   Get-VMNetworkAdapter -ManagementOS
   ```

   _**About**_ 

   ```
   Name IsManagementOs VMName SwitchName   MacAddress   Status IPAddresses
   ---- -------------- ------ ----------   ----------   ------ -----------
   CORP-External-Switch True  CORP-External-Switch 001B785768AA {Ok}  
   Mgt  True  VMSTEST  E41D2D074071 {Ok}  
   SMB1 True  VMSTEST  00155D30AA00 {Ok}  
   SMB2 True  VMSTEST  00155D30AA01 {Ok}  
   ```

8. Affichez le mappage de l’équipe de cartes réseau.<p>Les résultats ne doivent pas retourner d’informations car vous n’avez pas encore effectué le mappage.

   ```PowerShell
   Get-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST -VMNetworkAdapterName SMB1
   Get-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST -VMNetworkAdapterName SMB2
   ```


9. Mappez SMB1 et SMB2 pour séparer les membres physiques de l’équipe de cartes réseau et afficher les résultats de vos actions.

   >[!IMPORTANT]
   >Veillez à effectuer cette étape avant de continuer, sinon votre implémentation échoue.

   ```PowerShell
   Set-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST -VMNetworkAdapterName "SMB1" -PhysicalNetAdapterName "Test-40G-1"
   Set-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST -VMNetworkAdapterName "SMB2" -PhysicalNetAdapterName "Test-40G-2"

   Get-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST
   ```

   _**About**_ 

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

10. Vérifiez les associations MAC créées précédemment.

    ```PowerShell    
    Get-NetAdapterVmqQueue
    ```

    _**About**_ 

    ```   
    Name   QueueID MacAddressVlanID Processor VmFriendlyName
    ----   ------- ---------------- --------- --------------
    TEST-40G-1 1   E4-1D-2D-07-40-71 1010:17
    TEST-40G-1 2   00-15-5D-30-AA-00 1020:17
    TEST-40G-2 1   00-15-5D-30-AA-01 1020:17
    ```


11. Testez la connexion à partir du système distant, car les deux hôtes cartes réseau virtuelles résident sur le même sous-réseau et \(ont\)le même ID de réseau local virtuel 102.

    ```PowerShell 
    Test-NetConnection 192.168.2.111
    ```

    _**About**_   

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

    _**About**_   

    ```
    ComputerName   : 192.168.2.222
    RemoteAddress  : 192.168.2.222
    InterfaceAlias : Test-40G-2
    SourceAddress  : 192.168.2.5
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms 
    ```
12. Définissez le nom, le nom du commutateur et les étiquettes de priorité.   

    ```PowerShell
    Set-VMNetworkAdapter -ManagementOS -Name "SMB1" -IeeePriorityTag on
    Set-VMNetworkAdapter -ManagementOS -Name "SMB2" -IeeePriorityTag on
    Get-VMNetworkAdapter -ManagementOS -Name "SMB*" | fl Name,SwitchName,IeeePriorityTag,Status
    ```

    _**About**_   

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

13. Affichez les propriétés de la carte réseau vEthernet.

    ```PowerShell
    Get-NetAdapterRdma -Name "vEthernet*" | sort Name | ft -AutoSize
    ```

    _**About**_   

    ```
    Name  InterfaceDescription Enabled
    ----  -------------------- -------
    vEthernet (SMB2)  Hyper-V Virtual Ethernet Adapter #4  False  
    vEthernet (SMB1)  Hyper-V Virtual Ethernet Adapter #3  False  
    vEthernet (MGT)   Hyper-V Virtual Ethernet Adapter #2  False   
    ```

14. Activez les cartes réseau vEthernet.  

    ```PowerShell
    Enable-NetAdapterRdma -Name "vEthernet (SMB1)"
    Enable-NetAdapterRdma -Name "vEthernet (SMB2)"
    Get-NetAdapterRdma -Name "vEthernet*" | sort Name | fl *
    ```

    _**About**_   

    ```
    Name  InterfaceDescription Enabled
    ----  -------------------- -------
    vEthernet (SMB2)  Hyper-V Virtual Ethernet Adapter #4  True   
    vEthernet (SMB1)  Hyper-V Virtual Ethernet Adapter #3  True  
    vEthernet (MGT)   Hyper-V Virtual Ethernet Adapter #2  False
    ```

## <a name="step-10-validate-the-rdma-functionality"></a>Étape 10. Validez la fonctionnalité RDMA.

Vous souhaitez valider la fonctionnalité RDMA à partir du système distant sur le système local, qui dispose d’un vSwitch, aux deux membres de l’équipe de jeu vSwitch.<p>Étant donné que les \(deux hôtes cartes réseau virtuelles\) SMB1 et SMB2 sont affectés au réseau local virtuel 102, vous pouvez sélectionner l’adaptateur VLAN 102 sur le système distant. <p>Dans cet exemple, la carte réseau test-40G-2 utilise RDMA pour SMB1 (192.168.2.111) et SMB2 (192.168.2.222).

>[!TIP]
>Vous devrez peut-être désactiver le pare-feu sur ce système.  Pour plus d’informations, consultez votre stratégie d’infrastructure.
>
>```PowerShell
>Set-NetFirewallProfile -All -Enabled False
>    
>Get-NetAdapterAdvancedProperty -Name "Test-40G-2"
>```
>
>_**About**_ 
>   
>```
>Name  DisplayNameDisplayValue   RegistryKeyword RegistryValue  
>----  -----------------------   --------------- -------------  
> .
> .
>Test-40G-2VLAN ID102VlanID  {102} 
>```

1. Affichez les propriétés de la carte réseau.

   ```PowerShell
   Get-NetAdapter
   ```

   _**About**_ 

   ```
   Name  InterfaceDescriptionifIndex Status   MacAddress LinkSpeed
   ----  --------------------------- ------   ---------- ---------
   Test-40G-2Mellanox ConnectX-3 Pro Ethernet A...#3   3 Up   E4-1D-2D-07-43-D140 Gbps
   ```

2. Affichez les informations RDMA de la carte réseau.

   ```PowerShell
   Get-NetAdapterRdma
   ```

   _**About**_  

   ```
   Name  InterfaceDescription Enabled
   ----  -------------------- -------
   Test-40G-2Mellanox ConnectX-3 Pro Ethernet Adap... True   
   ```

3. Exécutez le test de trafic RDMA sur la première carte physique.

   ```PowerShell 
   C:\TEST\Test-RDMA.PS1 -IfIndex 3 -IsRoCE $true -RemoteIpAddress 192.168.2.111 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
   ```

   _**About**_ 

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

4. Exécutez le test de trafic RDMA sur la seconde carte physique.

   ```PowerShell
   C:\TEST\Test-RDMA.PS1 -IfIndex 3 -IsRoCE $true -RemoteIpAddress 192.168.2.222 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
   ```

   _**About**_ 

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

5. Testez le trafic RDMA entre l’ordinateur local et l’ordinateur distant.

    ```PowerShell
    Get-NetAdapter | ft –AutoSize
    ```

    _**About**_ 

    ```
    Name  InterfaceDescriptionifIndex Status   MacAddress LinkSpeed
    ----  --------------------------- ------   ---------- ---------
    vEthernet (SMB2)  Hyper-V Virtual Ethernet Adapter #4  45 Up   00-15-5D-30-AA-0380 Gbps
    vEthernet (SMB1)  Hyper-V Virtual Ethernet Adapter #3  41 Up   00-15-5D-30-AA-0280 Gbps
    ```

6. Exécutez le test de trafic RDMA sur la première carte virtuelle.    

   ```
   C:\TEST\Test-RDMA.PS1 -IfIndex 41 -IsRoCE $true -RemoteIpAddress 192.168.2.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
   ```

   _**About**_ 

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

7. Exécutez le test de trafic RDMA sur la seconde carte virtuelle.

   ```PowerShell
   C:\TEST\Test-RDMA.PS1 -IfIndex 45 -IsRoCE $true -RemoteIpAddress 192.168.2.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
   ```

   _**About**_ 

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

La dernière ligne de cette sortie, «test du trafic RDMA réussie : Le trafic RDMA a été envoyé à 192.168.2.5, «indique que vous avez correctement configuré la carte réseau convergée sur votre carte.

## <a name="related-topics"></a>Rubriques connexes 

- [Configuration de carte réseau convergée avec une seule carte réseau](cnic-single.md)
- [Configuration du commutateur physique pour la carte réseau convergée](cnic-app-switch-config.md)
- [Résolution des problèmes de configuration de cartes réseau convergées](cnic-app-troubleshoot.md)
