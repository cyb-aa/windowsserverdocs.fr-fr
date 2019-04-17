---
title: Configuration des cartes réseau associées de cartes réseau convergé
description: Cette rubrique fournit des instructions sur la configuration des cartes de réseau convergé dans une configuration de centre de données dans Windows Server2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: f01546f8-c495-4055-8492-8806eee99862
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1ac6c2915301b1cf64486f24c197ebbf22bb5e2e
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="converged-nic-teamed-nic-configuration"></a>Configuration des cartes réseau associées de cartes réseau convergé

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Les sections suivantes fournissent des instructions de déploiement convergé de cartes réseau dans une configuration de la carte d’interface réseau associées avec \(SET\) Switch Embedded Teaming. L’exemple de configuration dans ce guide représente deux hôtes Hyper-V, hôte 1 Hyper-V et l’hôte 2 Hyper-V.

## <a name="test-connectivity-between-source-and-destination"></a>Tester la connectivité entre la Source et de Destination

Cette section fournit les étapes requises pour tester la connectivité entre les ordinateurs hôtes Hyper-V source et de destination.

L’illustration suivante représente deux hôtes Hyper-V, **hôte 1 Hyper-V** et **hôte 2 Hyper-V**. 

Les deux hôtes Hyper-V ont deux cartes réseau. Sur chaque ordinateur hôte, une carte est connectée au sous-réseau 192.168.1.x/24 et une carte est connectée au sous-réseau 192.168.2.x/24.

![Ordinateurs hôtes Hyper-V](../../media/Converged-NIC/1-datacenter-test-conn.jpg)

### <a name="test-nic-connectivity-to-the-hyper-v-virtual-switch"></a>Tester la connectivité des cartes réseau au commutateur virtuel Hyper-V

À l’aide de cette étape, vous pouvez vous assurer que la carte réseau physique pour lequel vous allez créer ultérieurement un commutateur virtuel Hyper-V, permettre se connecter à l’hôte de destination. 

Ce test illustre la connectivité à l’aide de couche 3 \(L3\) - ou la couche IP - ainsi que les couche 2 \(L2\) réseaux locaux virtuels \(VLAN\).

Vous pouvez utiliser la commande Windows PowerShell suivante pour obtenir les propriétés de la carte réseau.

    Get-NetAdapter -Name "Test-40G-1" | ft -AutoSize
     
Voici les résultats de l’exemple de cette commande.

|Nom|InterfaceDescription|ifIndex|État|Adresse MAC|Vitesse de la connexion|
|-----|--------------------|-------|-----|----------|---------|
|
|Test-40G-1|Carte Ethernet Pro Mellanox ConnectX-3|11|À distance|E4-1D-2D-07-43-D0|40Gbits/s|

Vous pouvez utiliser une des commandes suivantes pour obtenir les propriétés de la carte supplémentaires, notamment l’adresse IP.

    Get-NetIPAddress -InterfaceAlias "Test-40G-1"

    Get-NetIPAddress -InterfaceAlias "TEST-40G-1" | Where-Object {$_.AddressFamily -eq "IPv4"} | fl InterfaceAlias,IPAddress
    
Voici les résultats de l’exemple de ces commandes.

|Paramètre|Valeur|
|---------|-----|
|
|Adresse IP| 192.168.1.3|
|InterfaceIndex|11|
|InterfaceAlias|Test-40G-1|
|AddressFamily|IPv4|
|Type| Monodiffusion|
|PrefixLength|24|

### <a name="verify-that-nic-team-member-has-a-valid-ip-address"></a>Vérifiez qu’une adresse IP valide membre de l’équipe de cartes réseau

Vous pouvez utiliser cette étape pour vérifier qu’autres équipe de cartes réseau ou équipe Embedded commutateur \(SET\) membre physique cartes réseau \(pNICs\) également avoir une adresse IP valide.

Pour cette étape, vous pouvez utiliser un sous-réseau distinct, \ (xxx.xxx.** 2**.xxx vs xxx.xxx. **1**. xxx\), afin de faciliter l’envoi à partir de cette carte pour la destination. 

Sinon, si vous recherchez les deux pNICs sur le même sous-réseau, la charge de la pile TCP/IP de Windows équilibre entre les interfaces et validation simple devient plus complexe.

Vous pouvez utiliser la commande Windows PowerShell suivante pour obtenir les propriétés de la deuxième carte réseau.

    Get-NetAdapter -Name "Test-40G-2" | ft -AutoSize

Voici les résultats de l’exemple de cette commande.

|Nom |InterfaceDescription |ifIndex |État |Adresse MAC |Vitesse de la connexion|
|----|--------------------|-------|------|----------|---------|
|
|TEST-40G-2 |Ethernet Pro Mellanox ConnectX-3 A... \ #2 |13 |À distance |E4-1D-2D-07-40-70 |40Gbits/s|

Vous pouvez utiliser une des commandes suivantes pour obtenir les propriétés de la carte supplémentaires, notamment l’adresse IP.

    Get-NetIPAddress -InterfaceAlias "Test-40G-2"

    Get-NetIPAddress -InterfaceAlias "Test-40G-2" | Where-Object {$\_.AddressFamily -eq "IPv4"} | fl InterfaceAlias,IPAddress

Voici les résultats de l’exemple de ces commandes.

|Paramètre|Valeur|
|---------|-----|
|
|Adresse IP|192.168.2.3|
|InterfaceIndex|13|
|InterfaceAlias|TEST-40G-2|
|AddressFamily|IPv4|
|Type|Monodiffusion|
|PrefixLength|24|

### <a name="verify-that-additional-nics-have-valid-ip-addresses"></a>Vérifiez que les adresses IP valides sur les cartes réseau supplémentaires

Vous pouvez utiliser cette étape pour vous assurer que l’autre carte réseau a une adresse IP valide.

Pour cette étape, vous pouvez utiliser un sous-réseau distinct, \ (xxx.xxx.** 2**.xxx vs xxx.xxx. **1**. xxx\), afin de faciliter l’envoi à partir de cette carte pour la destination. 

Sinon, si vous recherchez les deux pNICs sur le même sous-réseau, la charge de la pile TCP/IP de Windows équilibre entre les interfaces et validation simple devient plus complexe.

Vous pouvez utiliser la commande Windows PowerShell suivante pour obtenir les propriétés de la deuxième carte réseau.

    Get-NetAdapter -Name "Test-40G-2" | ft -AutoSize

Voici les résultats de l’exemple de cette commande.

|Nom |InterfaceDescription |ifIndex |État |Adresse MAC |Vitesse de la connexion|
|----|--------------------|-------|------|----------|---------|
|
|TEST-40G-2 |Ethernet Pro Mellanox ConnectX-3 A... \ #2 |13 |À distance |E4-1D-2D-07-40-70 |40Gbits/s|

Vous pouvez utiliser une des commandes suivantes pour obtenir les propriétés de la carte supplémentaires, notamment l’adresse IP.
    
    Get-NetIPAddress -InterfaceAlias "Test-40G-2"

    Get-NetIPAddress -InterfaceAlias "Test-40G-2" | Where-Object {$_.AddressFamily -eq "IPv4"} | fl InterfaceAlias,IPAddress

Voici les résultats de l’exemple de ces commandes.

|Paramètre|Valeur|
|---------|-----|
|
|Adresse IP|192.168.2.3|
|InterfaceIndex|13|
|InterfaceAlias|TEST-40G-2|
|AddressFamily|IPv4|
|Type|Monodiffusion|
|PrefixLength|24|

### <a name="ensure-that-source-and-destination-can-communicate"></a>Assurez-vous que la Source et Destination peuvent communiquer

Vous pouvez utiliser cette étape pour vérifier la communication bidirectionnelle \ (ping à partir de la source à destination et vice versa sur les deux systèmes d’exploitation\).  Dans l’exemple suivant, le **Test-NetConnection** commande Windows PowerShell est utilisé, mais si vous préférez que vous pouvez utiliser le **ping** commande. 

    Test-NetConnection 192.168.1.5

Voici les résultats de l’exemple de cette commande.

|Paramètre|Valeur|
|---------|-----|
|
|Nom_Ordinateur|192.168.1.5|
|RemoteAddress|192.168.1.5|
|InterfaceAlias|Test-40G-1|
|Adresse source|192.168.1.3|
|PingSucceeded|False (faux)|
|PingReplyDetails \(RTT\)|0ms|

Dans certains cas, vous devrez peut-être désactiver le pare-feu Windows avec fonctions avancées de sécurité pour effectuer ce test. Si vous désactivez le pare-feu, n’oubliez pas sécurité et vérifiez que votre configuration répond aux exigences de sécurité de votre organisation.

La commande suivante vous permet de désactiver tous les profils de pare-feu.

    Set-NetFirewallProfile -All -Enabled False
    
Une fois que vous désactivez le pare-feu, vous pouvez utiliser la commande suivante pour tester la connexion.

    Test-NetConnection 192.168.1.5

Voici les résultats de l’exemple de cette commande.

|Paramètre|Valeur|
|---------|-----|
|
|Nom_Ordinateur|192.168.1.5|
|RemoteAddress|192.168.1.5|
|InterfaceAlias|Test-40G-1|
|Adresse source|192.168.1.3|
|PingSucceeded|False (faux)|
|PingReplyDetails \(RTT\)|0ms|

### <a name="verify-connectivity-for-additional-nics"></a>Vérifiez la connectivité pour les cartes réseau supplémentaires

Avec cette étape, vous pouvez répéter les étapes précédentes pour tous les pNICs suivants qui sont inclus dans l’équipe de cartes réseau ou un ensemble.

Vous pouvez utiliser la commande suivante pour tester la connexion.
    
    Test-NetConnection 192.168.2.5

Voici les résultats de l’exemple de cette commande.

|Paramètre|Valeur|
|---------|-----|
|
|Nom_Ordinateur|192.168.2.5|
|RemoteAddress|192.168.2.5|
|InterfaceAlias|Test-40G-2|
|Adresse source|192.168.2.3|
|PingSucceeded|False (faux)|
|PingReplyDetails \(RTT\)|0ms|


## <a name="configure-vlans"></a>Configurer les réseaux locaux virtuels

Pour cette étape, les cartes réseau sont dans **accès** mode. Toutefois lorsque vous créez un commutateur virtuel Hyper-V de \(vSwitch\) plus loin dans ce guide, les propriétés de réseau local virtuel sont appliquées au niveau du port de commutateur virtuel. 

Dans la mesure où un commutateur peut héberger plusieurs réseaux locaux virtuels, il est nécessaire pour le commutateur physique de \(ToR\) Top of Rack à son port configuré en mode Trunk.

Pour obtenir des instructions sur la façon de configurer le mode Trunk sur le commutateur, consultez la documentation de votre commutateur ToR.

L’illustration suivante représente deux hôtes Hyper-V avec deux cartes réseau chaque ayant VLAN 101 et 102 de réseau local virtuel configuré dans les propriétés de la carte réseau.

![Configurer les réseaux locaux virtuels](../../media/Converged-NIC/2-datacenter-configure-vlans.jpg)

### <a name="configure-the-vlan-ids-for-both-nics"></a>Configurez les ID de VLAN pour les deux cartes réseau

Vous pouvez utiliser cette étape pour configurer les ID de réseau local virtuel pour les cartes réseau installées dans vos ordinateurs hôtes Hyper-V.

En fonction de l’Institute of Electrical et \(IEEE\) Electronics Engineers standards de mise en réseau, les propriétés \(QoS\) de qualité de Service dans la carte réseau physique NIC agir sur l’en-tête 802.1 p qui est incorporé dans le 802. 1 q \(VLAN\) en-tête lorsque vous configurez le VLAN ID.

#### <a name="configure-nic-test-40g-1"></a>Configurer les cartes réseau Test-40G-1

Avec les commandes suivantes, configurez l’ID de VLAN pour la première carte réseau, Test-40G-1, puis afficher la configuration obtenue.

    
    Set-NetAdapterAdvancedProperty -Name "Test-40G-1" -RegistryKeyword VlanID -RegistryValue "101"
    Get-NetAdapterAdvancedProperty -Name "Test-40G-1" | Where-Object {$_.RegistryKeyword -eq "VlanID"} | ft -AutoSize
    
Voici les résultats de l’exemple de cette commande.

|Nom |DisplayName| Valeur d’affichage| RegistryKeyword |RegistryValue|
|----|-----------|------------|---------------|-------------|
|
|TEST-40G-1|ID DE VLAN|101|VlanID|{101}|


Assurez-vous que l’ID de VLAN prend effet indépendamment de l’implémentation de la carte réseau à l’aide de la commande suivante pour redémarrer la carte réseau.

    Restart-NetAdapter -Name "Test-40G-1"

Vous pouvez utiliser la commande suivante pour vous assurer que l’état de la carte réseau est **des** avant de continuer.

    Get-NetAdapter -Name "Test-40G-1" | ft -AutoSize

Voici les résultats de l’exemple de cette commande.

|Nom|InterfaceDescription|ifIndex| État|Adresse MAC|Vitesse de la connexion|
|----|--------------------|-------|------|----------| ---------|
|
|Test-40G-1|Ethernet Pro Mellanox ConnectX-3 Ada...|11|À distance|E4-1D-2D-07-43-D0|40Gbits/s|

#### <a name="configure-nic-test-40g-2"></a>Configurer les cartes réseau Test-40G-2

Avec les commandes suivantes, configurez l’ID de VLAN pour la seconde carte réseau, Test-40G-2, puis afficher la configuration obtenue.

    
    Set-NetAdapterAdvancedProperty -Name "Test-40G-2" -RegistryKeyword VlanID -RegistryValue "102"
    Get-NetAdapterAdvancedProperty -Name "Test-40G-2" | Where-Object {$_.RegistryKeyword -eq "VlanID"} | ft -AutoSize
    

Voici les résultats de l’exemple de cette commande.

|Nom |DisplayName| Valeur d’affichage| RegistryKeyword |RegistryValue|
|----|-----------|------------|---------------|-------------|
|
|TEST-40G-2|ID DE VLAN|102|VlanID|{102}|

Assurez-vous que l’ID de VLAN prend effet indépendamment de l’implémentation de la carte réseau à l’aide de la commande suivante pour redémarrer la carte réseau.

`Restart-NetAdapter -Name "Test-40G-2"  `              

Vous pouvez utiliser la commande suivante pour vous assurer que l’état de la carte réseau est **des** avant de continuer.

    Get-NetAdapter -Name "Test-40G-1" | ft -AutoSize

Voici les résultats de l’exemple de cette commande.

|Nom|InterfaceDescription|ifIndex| État|Adresse MAC|Vitesse de la connexion|
|----|--------------------|-------|------|----------| ---------|
|
|Test-40G-2 |Ethernet Pro Mellanox ConnectX-3 Ada... |11 |À distance |E4-1D-2D-07-43-D1 |40Gbits/s|


### <a name="verify-connectivity"></a>Vérifiez la connectivité

Vous pouvez utiliser cette section pour vérifier la connectivité une fois que les cartes réseau sont redémarrés. Vous pouvez confirmer la connectivité après avoir appliqué la balise VLAN pour les deux cartes. En cas d’échec de la connectivité, vous pouvez examiner la participation de configuration ou de destination de réseau local virtuel dans le même réseau local virtuel du commutateur. 

>[!IMPORTANT]
>Après avoir effectué les étapes décrites dans la section précédente, il peut prendre plusieurs secondes pour le périphérique pour redémarrer et sont disponibles sur le réseau.

#### <a name="verify-connectivity-for-nic-test-40g-1"></a>Vérifiez la connectivité des cartes réseau Test-40G-1

Pour vérifier la connectivité pour la première carte réseau, vous pouvez exécuter la commande suivante.

    Test-NetConnection 192.168.1.5
    
Voici les résultats de l’exemple de cette commande.

|Paramètre|Valeur|
|---------|-----|
|
|Nom_Ordinateur|192.168.1.5|
|RemoteAddress|192.168.1.5|
|InterfaceAlias|Test-40G-1|
|Adresse source|192.168.1.5|
|PingSucceeded|True|
|PingReplyDetails \(RTT\)|0ms|

#### <a name="verify-connectivity-for-nic-test-40g-2"></a>Vérifiez la connectivité des cartes réseau Test-40G-2

Vous pouvez utiliser cette section pour tester la connectivité des cartes réseau Test-40G-2.

Vous pouvez utiliser la commande suivante pour tester la connectivité pour la deuxième carte réseau.
    
    Test-NetConnection 192.168.2.5
    
Voici les résultats de l’exemple de cette commande.

|Paramètre|Valeur|
|---------|-----|
|
|Nom_Ordinateur|192.168.2.5|
|RemoteAddress|192.168.2.5|
|InterfaceAlias|Test-40G-2|
|Adresse source|192.168.2.3|
|PingSucceeded|True|
|PingReplyDetails \(RTT\)|0ms|

Un **Test-NetConnection** défaillance ou ping qui se produit immédiatement après avoir effectué **redémarrage-NetAdapter** n’est pas rare, par conséquent, attendez la fin de la carte réseau entièrement initialiser, puis essayez à nouveau.

Si les connexions de réseau local virtuel 101 fonctionner, mais les connexions de réseau local virtuel 102 ne fonctionnent pas, le problème peut être que le commutateur doit être configuré pour autoriser le trafic du port sur le réseau local virtuel souhaité. Pour ce faire, vous pouvez vérifier en temporairement affectant les cartes défectueux 101 de réseau local virtuel et en répétant le test de connectivité.

## <a name="configure-quality-of-service-qos"></a>Configurer la qualité de Service \(QoS\)

L’étape suivante consiste à configurer la qualité de Service \(QoS\), ce qui nécessite que tout d’abord installer la fonctionnalité de Windows Server2016 \(DCB\) Data Center Bridging.

L’illustration suivante représente vos ordinateurs hôtes Hyper-V après avec succès la configuration de réseaux locaux virtuels à l’étape précédente.

![Configurer la qualité de Service](../../media/Converged-NIC/3-datacenter-configure-qos.jpg)

### <a name="install-data-center-bridging-dcb"></a>Installer Data Center Bridging \(DCB\)

Vous pouvez utiliser cette étape pour installer et activer DCB. Dans la plupart des cas, cette étape est facultative pour les implémentations iWarp, mais il est nécessaire à l’échelle de l’infrastructure, tels que pour les scénarios entre rack.

Vous pouvez utiliser la commande suivante pour installer DCB sur chacun de vos ordinateurs hôtes Hyper-V. 

    Install-WindowsFeature Data-Center-Bridging

Voici les résultats de l’exemple de cette commande.

|Réussite |Redémarrage nécessaire |Code de sortie|Résultat de la fonctionnalité|
|------- |-------------- |--------- |-------------- |
|
|True |N° |Réussite| {Data Center Bridging}|

### <a name="set-the-qos-policies-for-smb-direct"></a>Définir les stratégies de QoS pour SMB Direct 

Vous pouvez utiliser la commande suivante pour configurer des stratégies de QoS pour SMB Direct.

    New-NetQosPolicy "SMB" -NetDirectPortMatchCondition 445 -PriorityValue8021Action 3

Voici les résultats de l’exemple de cette commande.

|Paramètre|Valeur|
|---------|-----|
|
|Nom |SMB|
|Propriétaire|\(Machine\) stratégie de groupe|
|NetworkProfile|Tous|
|Priorité|127|
|CreateJobObject|&nbsp;| 
|NetDirectPort|445
|PriorityValue|3

### <a name="set-policies-for-other-traffic-on-the-interface"></a>Définir des stratégies pour le reste du trafic sur l’interface 

Vous pouvez utiliser la commande suivante pour définir des stratégies de QoS supplémentaires.

    New-NetQosPolicy "DEFAULT" -Default -PriorityValue8021Action 0
 
Voici les résultats de l’exemple de cette commande.

|Paramètre|Valeur|
|---------|-----|
|
|Nom | PAR DÉFAUT|
|Propriétaire|\(Machine\) stratégie de groupe|
|NetworkProfile|Tous|
|Priorité|127|
|Modèle| Par défaut|
|CreateJobObject| &nbsp;|
|PriorityValue|0|

### <a name="turn-on-flow-control-for-smb"></a>Activer le contrôle de flux pour SMB 

Vous pouvez utiliser les commandes suivantes pour activer le contrôle de flux SMB et afficher les résultats.

    Enable-NetQosFlowControl -priority 3
    Get-NetQosFlowControl

Voici les résultats de l’exemple de cette commande.

|Priorité|Activé|PolicySet|IfIndex|IfAlias|
|---------|-----|--------- |-------| -------|
|
|0 |False (faux) |Global|&nbsp;|&nbsp;|
|1 |False (faux) |Global|&nbsp;|&nbsp;|
|2 |False (faux) |Global|&nbsp;|&nbsp;|
|3 |True |Global|&nbsp;|&nbsp;|
|4 |False (faux) |Global|&nbsp;|&nbsp;|
|5 |False (faux) |Global|&nbsp;|&nbsp;|
|6 |False (faux) |Global|&nbsp;|&nbsp;|
|7 |False (faux) |Global|&nbsp;|&nbsp;|

Si vos résultats ne correspondent pas ces résultats dans la mesure où les éléments autres que 3 ont une activé la valeur True, vous devez effectuer l’étape suivante. Si vos résultats correspondent à ces résultats exemple et seul élément 3 a la valeur activé la valeur True, il est inutile d’effectuer la prochaine étape et pouvez passer vers le bas **activer la qualité de service pour les cartes réseau cible et de destination**.

### <a name="ensure-flow-control-is-disabled-for-other-traffic-classes-optional"></a>Assurez-vous que le contrôle de flux est désactivé pour les autres Classes de trafic \(Optional\)

Vous n’avez pas besoin d’effectuer cette étape si **contrôle de flux** n’est pas activé pour les classes de trafic 0,1,2,4,5,6 et 7.

Si **contrôle de flux** est déjà activé pour le trafic des classes autres que 3 \ (0,1,2,4,5,6 et 7\), vous devez désactiver **contrôle de flux** pour ces classes. 

>[!NOTE]
>Sous des configurations plus complexes, les autres classes de trafic peuvent nécessiter le contrôle de flux, mais ces scénarios sont en dehors de la portée de ce guide.

Vous pouvez utiliser les commandes suivantes pour désactiver le contrôle de flux SMB pour les classes de trafic 0,1,2,4,5,6 et 7 et pour afficher les résultats.

    Disable-NetQosFlowControl -priority 0,1,2,4,5,6,7
    Get-NetQosFlowControl

Voici les résultats de l’exemple de cette commande.

|Priorité|Activé|PolicySet|IfIndex|IfAlias|
|---------|-----|--------- |-------| -------|
|
|0 |False (faux) |Global|&nbsp;|&nbsp;|
|1 |False (faux) |Global|&nbsp;|&nbsp;|
|2 |False (faux) |Global|&nbsp;|&nbsp;|
|3 |True |Global|&nbsp;|&nbsp;|
|4 |False (faux) |Global|&nbsp;|&nbsp;|
|5 |False (faux) |Global|&nbsp;|&nbsp;|
|6 |False (faux) |Global|&nbsp;|&nbsp;|
|7 |False (faux) |Global|&nbsp;|&nbsp;|

### <a name="enable-qos-for-the-local-and-destination-network-adapters"></a>Activer la qualité de service pour les cartes réseau local et de destination 

Avec cette étape, vous pouvez activer la qualité de service pour les cartes réseau local et de destination. Vérifiez que vous exécutez ces commandes sur les deux hôtes Hyper-V.

#### <a name="enable-qos-for-nic-test-40g-1"></a>Activer la qualité de service pour les cartes réseau Test-40G-1

Vous pouvez utiliser les commandes suivantes pour activer la qualité de service et afficher les résultats de votre configuration.

    Enable-NetAdapterQos -InterfaceAlias "Test-40G-1"
    Get-NetAdapterQos -Name "Test-40G-1"

Voici les résultats de l’exemple de cette commande.

**Nom**: TEST-40G-1 **activé**: True **fonctionnalités**:   

|Paramètre|Matériel|En cours|
|---------|--------|-------|
|
|MacSecBypass|NotSupported|NotSupported|
|DcbxSupport|None|None|
|NumTCs(Max/ETS/PFC)|8/8/8|8/8/8|
 
**OperationalTrafficClasses**: 

|TC|TSA|Bande passante|Priorités|
|----|-----|--------|-------|
|
|0| Strict|&nbsp;|0-7|

**OperationalFlowControl**: priorité 3 activé **OperationalClassifications**:

|Protocole|Port/Type|Priorité|
|--------|---------|--------|
|
|Par défaut|&nbsp;|0|
|NetDirect| 445|3|

#### <a name="enable-qos-for-nic-test-40g-2"></a>Activer la qualité de service pour les cartes réseau Test-40G-2

Vous pouvez utiliser les commandes suivantes pour activer la qualité de service et afficher les résultats de votre configuration.

    Enable-NetAdapterQos -InterfaceAlias "Test-40G-2"
    Get-NetAdapterQos -Name "Test-40G-2"

 
Voici les résultats de l’exemple de cette commande.

**Nom**: TEST-40G-2 **activé**: True **fonctionnalités**:   

|Paramètre|Matériel|En cours|
|---------|--------|-------|
|
|MacSecBypass|NotSupported|NotSupported|
|DcbxSupport|None|None|
|NumTCs(Max/ETS/PFC)|8/8/8|8/8/8|
 
**OperationalTrafficClasses**: 

|TC|TSA|Bande passante|Priorités|
|----|-----|--------|-------|
|
|0| Strict|&nbsp;|0-7|

**OperationalFlowControl**: priorité 3 activé **OperationalClassifications**:

|Protocole|Port/Type|Priorité|
|--------|---------|--------|
|
|Par défaut|&nbsp;|0|
|NetDirect| 445|3|


### <a name="allocate-50-of-the-bandwidth-reservation-to-smb-direct-rdma"></a>Allouer 50% de la réservation de bande passante à \(RDMA\) Direct SMB

Vous pouvez utiliser les commandes suivantes pour affecter la moitié de la réservation de bande passante à SMB Direct.

    New-NetQosTrafficClass "SMB" -priority 3 -bandwidthpercentage 50 -algorithm ETS

Voici les résultats de l’exemple de cette commande.

|Nom|Algorithme |Bandwidth(%)| Priorité |PolicySet |IfIndex |IfAlias |
|----|---------| ------------ |--------| ---------|------- |------- |
|
|SMB | ETS     | 50 |3 |Global |&nbsp;|&nbsp;|                                      
 
Vous pouvez utiliser la commande suivante pour afficher des informations sur la réservation de bande passante.

    Get-NetQosTrafficClass | ft -AutoSize

Voici les résultats de l’exemple de cette commande.
 
|Nom|Algorithme |Bandwidth(%)| Priorité |PolicySet |IfIndex |IfAlias |
|----|---------| ------------ |--------| ---------|------- |------- |
|
|[Par défaut]| ETS|50 |0-2,4-7|  Global|&nbsp;|&nbsp;| 
SMB |ETS|50 |3 |Global|&nbsp;|&nbsp;| 

### <a name="create-traffic-classes-for-tenant-ip-traffic-optional"></a>Créer des Classes de trafic pour le trafic de client IP \(optional\)

Vous pouvez utiliser cette étape pour créer deux classes de trafic supplémentaire pour le trafic de client IP. 

Lorsque vous exécutez les exemples de commandes suivants, vous pouvez omettre les valeurs «IP1» et «IP2» si vous préférez.

    New-NetQosTrafficClass "IP1" -Priority 1 -bandwidthpercentage 10 -algorithm ETS

Voici les résultats de l’exemple de cette commande.

|Nom|Algorithme |Bandwidth(%)| Priorité |PolicySet |IfIndex |IfAlias |
|----|---------| ------------ |--------| ---------|------- |------- |
|
|IP1 |ETS |10 |1 |Global|&nbsp;|&nbsp;|


    New-NetQosTrafficClass "IP2" -Priority 2 -bandwidthpercentage 10 -algorithm ETS

Voici les résultats de l’exemple de cette commande.

|Nom|Algorithme |Bandwidth(%)| Priorité |PolicySet |IfIndex |IfAlias |
|----|---------| ------------ |--------| ---------|------- |------- |
|
|IP2 |ETS |10 |2 |Global|&nbsp;|&nbsp;|

Vous pouvez utiliser la commande suivante pour afficher les classes de trafic de qualité de service.

    Get-NetQosTrafficClass | ft -AutoSize

Voici les résultats de l’exemple de cette commande.

|Nom|Algorithme |Bandwidth(%)| Priorité |PolicySet |IfIndex |IfAlias |
|----|---------| ------------ |--------| ---------|------- |------- |
|
|[Par défaut] |ETS |30 |0,4-7 |Global|&nbsp;|&nbsp;|
|SMB |ETS |50 |3 |Global|&nbsp;|&nbsp;|
|IP1 |ETS |10 |1 |Global|&nbsp;|&nbsp;|
|IP2 |ETS |10 |2 |Global|&nbsp;|&nbsp;|

## <a name="configure-debugger-optional"></a>Configurer le débogueur \(Optional\)

Vous pouvez utiliser cette étape pour configurer le débogueur.

Par défaut, le débogueur attaché bloque NetQos. Vous pouvez utiliser les commandes suivantes pour remplacer le débogueur et afficher les résultats.

    Set-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" AllowFlowControlUnderDebugger -type DWORD -Value 1 –Force
    
    Get-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" | ft AllowFlowControlUnderDebugger

Voici les résultats de l’exemple de cette commande.

    AllowFlowControlUnderDebugger
    -----------------------------
    1

## <a name="test-rdma-mode-1"></a>Test RDMA \(Mode 1\)

Vous pouvez utiliser cette étape pour vous assurer que l’infrastructure est correctement configuré avant la création d’un commutateur virtuel et la transition vers \(Mode 2\) RDMA.

L’illustration suivante représente l’état actuel des ordinateurs hôtes Hyper-V.

![Test RDMA](../../media/Converged-NIC/4-datacenter-test-rdma.jpg)

Pour vérifier la configuration RDMA, vous pouvez exécuter la commande suivante.

    Get-NetAdapterRdma | ft -AutoSize

Voici les résultats de l’exemple de cette commande.

|Nom |InterfaceDescription |Activé|
|----|--------------------|-------|
|
|TEST-40G-1| Carte Mellanox ConnectX-4 VPI #2 |True|
|TEST-40G-2| Carte Mellanox ConnectX-4 VPI |True|


### <a name="determine-the-ifindex-value-of-your-target-adapter"></a>Déterminer la valeur ifIndex de votre carte cible

Vous pouvez utiliser la commande suivante pour détecter la valeur ifIndex de la carte cible.

    Get-NetIPConfiguration -InterfaceAlias "TEST*" | ft InterfaceAlias,InterfaceIndex,IPv4Address

Voici les résultats de l’exemple de cette commande.

|InterfaceAlias |InterfaceIndex |IPv4Address|
|-------------- |-------------- |-----------|
|TEST-40G-1 |14 |{192.168.1.3}|
|TEST-40G-2 | 13 |{192.168.2.3}|

### <a name="download-diskspdexe-and-a-powershell-script"></a>Téléchargement de DiskSpd.exe et un Script PowerShell

Pour continuer, vous devez d’abord télécharger les éléments suivants.

- Télécharger l’utilitaire de DiskSpd.exe et extraire l’utilitaire dans C:\TEST\[http://tinyurl.com/z68h3rc](http://tinyurl.com/z68h3rc)

- Télécharger le script powershell Test-RDMA à C:\TEST\[https://github.com/Microsoft/SDN/blob/master/Diagnostics/Test-Rdma.ps1](https://github.com/Microsoft/SDN/blob/master/Diagnostics/Test-Rdma.ps1)

### <a name="run-the-powershell-script"></a>Exécutez le script PowerShell

Lorsque vous exécutez le script Windows PowerShell de .ps1 Test-Rdma, vous pouvez transmettre la valeur ifIndex le script, ainsi que l’adresse IP de la carte à distance sur le même réseau local virtuel.

Vous pouvez utiliser la commande suivante pour exécuter le script avec une ifIndex 14 sur la carte réseau 192.168.1.5.
    
    C:\TEST\Test-RDMA.PS1 -IfIndex 14 -IsRoCE $true -RemoteIpAddress 192.168.1.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
    
    VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\\diskspd.exe
    VERBOSE: The adapter TEST-40G-1 is a physical adapter
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
    

>[!NOTE]
>Si le trafic RDMA échoue, le cas RoCE plus précisément, consultez votre configuration ToR Switch pour les paramètres PFC/ETS appropriées qui doivent correspondre aux paramètres de l’ordinateur hôte. Reportez-vous à la section de la qualité de service dans ce document pour les valeurs de référence.

### <a name="determine-the-ifindex-value-of-your-target-adapter"></a>Déterminer la valeur ifIndex de votre carte cible

Vous pouvez utiliser la commande suivante pour détecter la valeur ifIndex de la carte cible.

    Get-NetIPConfiguration -InterfaceAlias "TEST*" | ft InterfaceAlias,InterfaceIndex,IPv4Address

Voici les résultats de l’exemple de cette commande.

|InterfaceAlias |InterfaceIndex |IPv4Address|
|-------------- |-------------- |-----------|
|TEST-40G-1 |14 |{192.168.1.3}|
|TEST-40G-2 | 13 |{192.168.2.3}|

Vous pouvez utiliser la commande suivante pour exécuter le script avec une ifIndex 13 sur la carte réseau 192.168.2.5.

    
    C:\TEST\Test-RDMA.PS1 -IfIndex 13 -IsRoCE $true -RemoteIpAddress 192.168.2.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
    VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\\diskspd.exe
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
    

## <a name="create-a-hyper-v-virtual-switch"></a>Créer un commutateur virtuel Hyper-V

Vous pouvez utiliser cette section pour créer un \(vSwitch\) commutateur virtuel Hyper-V sur vos ordinateurs hôtes Hyper-V.

L’illustration suivante représente l’hôte 1 Hyper-V avec un commutateur virtuel.

![Créer un commutateur virtuel Hyper-V](../../media/Converged-NIC/5-datacenter-create-vswitch.jpg)

### <a name="create-a-vswitch-in-switch-embedded-teaming-set-mode"></a>Créer un commutateur virtuel en mode \(SET\) Switch Embedded Teaming

Vous pouvez utiliser la commande suivante pour créer une équipe d’indépendants du commutateur jeu nommée **VMSTEST** sur l’hôte 1 Hyper-V. Les deux cartes réseau sur cet ordinateur, TEST-40G-1 et TEST-40G-2, sont ajoutés à l’équipe de jeu avec cette commande.
    
    New-VMSwitch –Name "VMSTEST" –NetAdapterName "TEST-40G-1","TEST-40G-2" -EnableEmbeddedTeaming $true -AllowManagementOS $true
    
Voici les résultats de l’exemple de cette commande.

|Nom |SwitchType |NetAdapterInterfaceDescription|
|---- |---------- |------------------------------|
|VMSTEST |Externe |Interface associées|

Vous pouvez utiliser cette commande pour afficher l’association de cartes physiques dans le jeu.

    Get-VMSwitchTeam -Name "VMSTEST" | fl

Voici les résultats de l’exemple de cette commande.

**Nom**: VMSTEST **Id**: ad9bb542-dda2-4450-a00e-f96d44bdfbec **NetAdapterInterfaceDescription**: {carte Ethernet Pro Mellanox ConnectX-3, carte Ethernet Pro Mellanox ConnectX-3 #2} **TeamingMode**: SwitchIndependent **LoadBalancingAlgorithm**: dynamique

#### <a name="display-two-views-of-the-host-vnic"></a>Afficher les deux vues de la carte réseau virtuelle hôte

Vous pouvez utiliser les deux commandes suivantes pour les deux vues différentes de la carte réseau virtuelle de l’ordinateur hôte.

Vous pouvez utiliser cette commande pour afficher les propriétés de la carte réseau virtuelle de l’ordinateur hôte.

    Get-NetAdapter

Voici les résultats de l’exemple de cette commande.

| Nom | InterfaceDescription | ifIndex | État | Adresse MAC | Vitesse de la connexion | |------|---|---|---|---| | | vEthernet (VMSTEST) | Hyper-V Carte Ethernet virtuelle #2 | 28 | Des | Gbits/s E4-1D-2D-07-40-71|80 |

Vous pouvez utiliser cette commande pour afficher les propriétés supplémentaires de la carte réseau virtuelle de l’ordinateur hôte.

    Get-VMNetworkAdapter -ManagementOS

Voici les résultats de l’exemple de cette commande.

|Nom |IsManagementOs |VMName |SwitchName |Adresse MAC |État |AdressesIP|
|----|--------------|------|----------|----------|------|-----------|
|
|VMSTEST|True |VMSTEST |E41D2D074071| {Ok}|&nbsp;|

#### <a name="test-the-network-connection-to-the-remote-vlan-101-adapter"></a>Tester la connexion réseau à la carte réseau local virtuel 101 à distance

Vous pouvez utiliser cette commande pour tester la connexion à la carte réseau local virtuel 101 à distance.

`Test-NetConnection 192.168.1.5` 

Voici les résultats de l’exemple de cette commande.

    WARNING: Ping to 192.168.1.5 failed -- Status: DestinationHostUnreachable
    
    ComputerName   : 192.168.1.5
    RemoteAddress  : 192.168.1.5
    InterfaceAlias : vEthernet (CORP-External-Switch)
    SourceAddress  : 10.199.48.170
    PingSucceeded  : False
    PingReplyDetails (RTT) : 0 ms
    
### <a name="reconfigure-vlans"></a>Reconfigurer les réseaux locaux virtuels

Vous pouvez utiliser cette étape pour supprimer le paramètre d’accès VLAN de la carte réseau physique et pour définir le VLANID à l’aide du commutateur virtuel.

Vous devez supprimer le paramètre de réseau local virtuel de l’accès pour empêcher les deux balisage automatique le trafic sortant avec l’ID de VLAN incorrecte et du filtrage des paquets entrants le trafic qui ne correspond pas à l’ID de réseau local virtuel de l’accès.

Vous pouvez utiliser les commandes suivantes pour supprimer le paramètre d’accès VLAN.
    
    Set-NetAdapterAdvancedProperty -Name "Test-40G-1" -RegistryKeyword VlanID -RegistryValue "0"
    Set-NetAdapterAdvancedProperty -Name "Test-40G-2" -RegistryKeyword VlanID -RegistryValue "0"
    

Vous pouvez utiliser les commandes suivantes pour définir le VLANID à l’aide des commandes Windows PowerShell vSwitch spécifiques et pour afficher les résultats de cette action.

    
    Set-VMNetworkAdapterVlan -VMNetworkAdapterName "VMSTEST" -VlanId "101" -Access -ManagementOS
    Get-VMNetworkAdapterVlan -ManagementOS -VMNetworkAdapterName "VMSTEST"
    

Voici les résultats de l’exemple de cette commande.

VMName VMNetworkAdapterName Mode VlanList
------ -------------------- ----   --------
       VMSTEST              Access 101     

Vous pouvez utiliser la commande suivante pour tester la connexion réseau et afficher les résultats.

    Test-NetConnection 192.168.1.5
    
    ComputerName   : 192.168.1.5
    RemoteAddress  : 192.168.1.5
    InterfaceAlias : vEthernet (VMSTEST)
    SourceAddress  : 192.168.1.3
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms
    

Si vos résultats ne sont pas similaires aux résultats exemple et ping échoue avec le message «Avertissement: Échec de la commande Ping sur 192.168.1.5--état: DestinationHostUnreachable, «confirmer que le «vEthernet (VMSTEST)» est l’adresse IP appropriée en exécutant la commande suivante.

    
    Get-NetIPAddress -InterfaceAlias "vEthernet (VMSTEST)"
    

Si l’adresse IP n’est pas définie, vous pouvez utiliser la commande suivante pour résoudre le problème et afficher les résultats de vos actions.

    
    New-NetIPAddress -InterfaceAlias "vEthernet (VMSTEST)" -IPAddress 192.168.1.3 -PrefixLength 24
    
    IPAddress : 192.168.1.3
    InterfaceIndex: 37
    InterfaceAlias: vEthernet (VMSTEST)
    AddressFamily : IPv4
    Type  : Unicast
    PrefixLength  : 24
    PrefixOrigin  : Manual
    SuffixOrigin  : Manual
    AddressState  : Tentative
    ValidLifetime : Infinite ([TimeSpan]::MaxValue)
    PreferredLifetime : Infinite ([TimeSpan]::MaxValue)
    SkipAsSource  : False
    PolicyStore   : ActiveStore
    
#### <a name="rename-the-management-nic"></a>Renommer la carte réseau de gestion

Vous pouvez utiliser cette section pour renommer la carte réseau de gestion et ensuite utiliser des instances distinctes de carte réseau virtuelle hôte pour RDMA.

Vous pouvez exécuter les commandes suivantes pour renommer la carte réseau de gestion et d’afficher certaines propriétés de la carte réseau.

    Rename-VMNetworkAdapter -ManagementOS -Name “VMSTEST” -NewName “MGT”
    Get-VMNetworkAdapter -ManagementOS

Voici les résultats de l’exemple de cette commande.

|Nom |IsManagementOs |VMName |SwitchName |Adresse MAC |État |AdressesIP
|----|--------------|------|----------|----------|------|-----------|
|
|Commutateur de CORP-externe |True |&nbsp;|Commutateur de CORP-externe |001B785768AA |{Ok}|&nbsp;|
|GESTION AVANCÉE |True |&nbsp;|VMSTEST |E41D2D074071 |{Ok}|&nbsp;|

Vous pouvez exécuter la commande suivante pour afficher des propriétés supplémentaires de la carte réseau.

    Get-NetAdapter

Voici les résultats de l’exemple de cette commande.

|Nom |InterfaceDescription |ifIndex |État |Adresse MAC |Vitesse de la connexion|
|----|--------------------|------|----------|---------|------|
|
|vEthernet (gestion) |Carte Ethernet virtuelle Hyper-V #2 |28 |À distance | E4-1D-2D-07-40-71 |80Gbits/s|

## <a name="test-hyper-v-virtual-switch-rdma"></a>Tester le commutateur virtuel Hyper-V RDMA

L’illustration suivante représente l’état actuel de vos ordinateurs hôtes Hyper-V, y compris le commutateur virtuel sur l’hôte 1 Hyper-V.

![Testez le commutateur virtuel Hyper-V](../../media/Converged-NIC/6-datacenter-test-vswitch-rdma.jpg)

### <a name="set-priority-tagging-on-the-host-vnic-to-complement-the-previous-vlan-settings"></a>Définir une priorité marquage sur la carte réseau virtuelle hôte pour compléter les paramètres de réseau local virtuel précédents

Vous pouvez utiliser les exemples de commandes suivantes pour définir la priorité de marquage sur la carte réseau virtuelle hôte et afficher les résultats de vos actions.
    
    Set-VMNetworkAdapter -ManagementOS -Name "MGT" -IeeePriorityTag on
    Get-VMNetworkAdapter -ManagementOS -Name "MGT" | fl Name,IeeePriorityTag
    

    Name: MGT
    IeeePriorityTag : On
    
### <a name="create-two-host-vnics-for-rdma"></a>Créez deux cartes réseau virtuelles hôtes pour RDMA

Vous pouvez utiliser les exemples de commandes suivantes pour créer deux cartes réseau virtuelles hôtes pour RDMA et de les connecter au vSwitch VMSTEST.
    
    Add-VMNetworkAdapter –SwitchName "VMSTEST" –Name SMB1 –ManagementOS
    Add-VMNetworkAdapter –SwitchName "VMSTEST" –Name SMB2 –ManagementOS
    
Vous pouvez utiliser la commande suivante pour afficher les propriétés de la carte réseau de gestion.
    
    Get-VMNetworkAdapter -ManagementOS
    
Voici les résultats de l’exemple de cette commande.

|Nom |IsManagementOs |VMName |SwitchName |Adresse MAC |État |AdressesIP|
|----|--------------|------|----------|----------|------|-----------|
|
|Commutateur de CORP-externe |True |Commutateur de CORP-externe |001B785768AA|{Ok} |&nbsp;| 
|Gestion avancée |True |VMSTEST |E41D2D074071 |{Ok} |&nbsp;|
|SMB1 |True |VMSTEST |00155D30AA00 |{Ok} |&nbsp;|
|SMB2 |True |VMSTEST |00155D30AA01 |{Ok} |&nbsp;|

### <a name="assign-an-ip-address-to-the-smb-host-vnics"></a>Attribuer une adresse IP à la carte réseau virtuelle hôte SMB

Vous pouvez utiliser cette section pour attribuer addressrd IP à le vEthernet de cartes réseau virtuelles hôtes SMB \(SMB1\) et vEthernet \(SMB2\).

Le TEST-40G-1 et TEST-40G-2des cartes physiques ont toujours un réseau local virtuel de l’accès de 101 et 102 configuré. Pour cette raison, les cartes marquer le trafic - et test réussit.

Auparavant, vous configuré les deux ID de VLAN pNIC à zéro, puis définissez le commutateur virtuel VMSTEST à 101 de réseau local virtuel. Après cela, vous ne pouviez toujours exécuter un test ping de la carte réseau local virtuel 101 à distance à l’aide de la carte réseau virtuelle de gestion avancée, mais il n’existe actuellement aucun membre 102 de réseau local virtuel.

Vous pouvez désormais utiliser la commande suivante pour supprimer le paramètre de réseau local virtuel de l’accès à partir de la carte réseau physique afin d’empêcher les deux balisage automatique le trafic sortant avec l’ID de VLAN incorrecte et afin d’éviter le filtrage de trafic d’entrée qui ne correspond pas à l’ID de réseau local virtuel de l’accès.

Vous pouvez utiliser la commande suivante pour atteindre ces objectifs par ajout d’une nouvelle adresse IP à l’interface vEthernet (SMB1) et afficher les résultats.

    
    New-NetIPAddress -InterfaceAlias "vEthernet (SMB1)" -IPAddress 192.168.2.111 -PrefixLength 24
    
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
    

Vous pouvez utiliser la commande suivante pour tester la carte réseau local virtuel 102 à distance et afficher les résultats.
    
    Test-NetConnection 192.168.2.5 
    
    ComputerName   : 192.168.2.5
    RemoteAddress  : 192.168.2.5
    InterfaceAlias : vEthernet (SMB1)
    SourceAddress  : 192.168.2.111
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms
    
Vous pouvez utiliser la commande suivante pour ajouter une nouvelle adresse IP de l’interface vEthernet \(SMB2\) et afficher les résultats.
    
    New-NetIPAddress -InterfaceAlias "vEthernet (SMB2)" -IPAddress 192.168.2.222 -PrefixLength 24 
    
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
    
Vous n’avez pas besoin tester la connexion à nouveau, car la communication est établie.

### <a name="place-the-rdma-host-vnics-on-vlan-102"></a>Placer la carte réseau virtuelle hôte RDMA sur réseau local virtuel 102

Vous pouvez utiliser cette section pour attribuer la carte réseau virtuelle hôte RDMA à 102 de réseau local virtuel.

Étant donné que la carte réseau virtuelle hôte «Gestion» se trouve sur le réseau local virtuel 101, vous pouvez utiliser les exemples de commandes suivants pour placer la carte réseau virtuelle hôte RDMA sur la 102 de réseau local virtuel existant et afficher les résultats de vos actions.

    
    Set-VMNetworkAdapterVlan -VMNetworkAdapterName "SMB1" -VlanId "102" -Access -ManagementOS
    Set-VMNetworkAdapterVlan -VMNetworkAdapterName "SMB2" -VlanId "102" -Access -ManagementOS
    
    Get-VMNetworkAdapterVlan -ManagementOS
    
    VMName VMNetworkAdapterName Mode VlanList
    ------ -------------------- ---- --------
       SMB1 Access   102 
       Mgt  Access   101 
       SMB2 Access   102 
       CORP-External-Switch Untagged
    
### <a name="inspect-the-mapping-of-smb1-and-smb2-to-the-underlying-physical-nics"></a>Examinez le mappage de SMB1 et SMB2 aux cartes réseau physique sous-jacent

Vous pouvez utiliser cette section pour inspecter le mappage des SMB1 et SMB2 pour les cartes réseau physiques sous-jacents sous le commutateur virtuel défini une équipe.  L’association de cartes réseau virtuelles hôtes pour les cartes réseau physiques est aléatoire et soumis à rééquilibrage lors de la création et la destruction.

Dans ce cas, vous pouvez utiliser un mécanisme indirect pour vérifier l’association en cours.

Les adresses MAC de SMB1 et SMB2 sont associés au membre de l’équipe de cartes réseau TEST-40G-2. Cela n’est pas idéale car Test-40G-1 ne dispose pas d’une carte réseau virtuelle hôte SMB associé et n’autorise pas l’utilisation du trafic RDMA sur le lien jusqu'à ce qu’une carte réseau virtuelle hôte SMB est mappé à celui-ci.

Vous pouvez utiliser les exemples de commandes suivantes pour afficher ces informations.

    
    Get-NetAdapterVPort (Preferred)
    
    Get-NetAdapterVmqQueue
    
    Name   QueueID MacAddressVlanID Processor VmFriendlyName
    ----   ------- ---------------- --------- --------------
    TEST-40G-1 1   E4-1D-2D-07-40-71 1010:17
    TEST-40G-2 1   00-15-5D-30-AA-00 1020:17
    TEST-40G-2 2   00-15-5D-30-AA-01 1020:17
    
    Get-VMNetworkAdapter -ManagementOS
    
    Name IsManagementOs VMName SwitchName   MacAddress   Status IPAddresses
    ---- -------------- ------ ----------   ----------   ------ -----------
    CORP-External-Switch True  CORP-External-Switch 001B785768AA {Ok}  
    Mgt  True  VMSTEST  E41D2D074071 {Ok}  
    SMB1 True  VMSTEST  00155D30AA00 {Ok}  
    SMB2 True  VMSTEST  00155D30AA01 {Ok}  
    

Les deux commandes ci-dessous ne doivent renvoyer aucune information, car vous n’avez pas encore effectué de mappage.
    
    Get-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST -VMNetworkAdapterName SMB1
    Get-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST -VMNetworkAdapterName SMB2
    
Vous pouvez utiliser les exemples de commandes suivants pour mapper SMB1 et SMB2 pour séparer les membres de l’équipe carte réseau physiques et pour afficher les résultats de vos actions.

>[!IMPORTANT]
>Assurez-vous que vous effectuez cette étape avant de continuer, ou votre implémentation échouera.
    
    Set-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST -VMNetworkAdapterName "SMB1" -PhysicalNetAdapterName "Test-40G-1"
    Set-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST -VMNetworkAdapterName "SMB2" -PhysicalNetAdapterName "Test-40G-2"
    
    Get-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST
    
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
    
### <a name="confirm-the-mac-associations"></a>Vérifiez les associations de MAC

Vous pouvez utiliser les exemples de commandes suivantes pour vérifier les associations de MAC que vous avez créé précédemment.
    
    Get-NetAdapterVmqQueue
    
    Name   QueueID MacAddressVlanID Processor VmFriendlyName
    ----   ------- ---------------- --------- --------------
    TEST-40G-1 1   E4-1D-2D-07-40-71 1010:17
    TEST-40G-1 2   00-15-5D-30-AA-00 1020:17
    TEST-40G-2 1   00-15-5D-30-AA-01 1020:17
    

Étant donné que les deux cartes réseau virtuelles hôtes se trouvent sur le même sous-réseau et ont le même \(102\) ID de réseau local virtuel, vous pouvez valider la communication à partir du système distant et afficher les résultats de vos actions.

>[!IMPORTANT]
>Exécutez les commandes suivantes sur l’ordinateur distant.

    
    Test-NetConnection 192.168.2.111
    
    
    ComputerName   : 192.168.2.111
    RemoteAddress  : 192.168.2.111
    InterfaceAlias : Test-40G-2
    SourceAddress  : 192.168.2.5
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms
    
    Test-NetConnection 192.168.2.222
    
    ComputerName   : 192.168.2.222
    RemoteAddress  : 192.168.2.222
    InterfaceAlias : Test-40G-2
    SourceAddress  : 192.168.2.5
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms 
    
        
    Set-VMNetworkAdapter -ManagementOS -Name "SMB1" -IeeePriorityTag on
    Set-VMNetworkAdapter -ManagementOS -Name "SMB2" -IeeePriorityTag on
    Get-VMNetworkAdapter -ManagementOS -Name "SMB*" | fl Name,SwitchName,IeeePriorityTag,Status
    
    Name: SMB1
    SwitchName  : VMSTEST
    IeeePriorityTag : On
    Status  : {Ok}
    
    Name: SMB2
    SwitchName  : VMSTEST
    IeeePriorityTag : On
    Status  : {Ok}
    

    Get-NetAdapterRdma -Name "vEthernet*" | sort Name | ft -AutoSize
    
    Name  InterfaceDescription Enabled
    ----  -------------------- -------
    vEthernet (SMB2)  Hyper-V Virtual Ethernet Adapter #4  False  
    vEthernet (SMB1)  Hyper-V Virtual Ethernet Adapter #3  False  
    vEthernet (MGT)   Hyper-V Virtual Ethernet Adapter #2  False   
    
    
    Enable-NetAdapterRdma -Name "vEthernet (SMB1)"
    Enable-NetAdapterRdma -Name "vEthernet (SMB2)"
    Get-NetAdapterRdma -Name "vEthernet*" | sort Name | fl *
    
    
    Name  InterfaceDescription Enabled
    ----  -------------------- -------
    vEthernet (SMB2)  Hyper-V Virtual Ethernet Adapter #4  True   
    vEthernet (SMB1)  Hyper-V Virtual Ethernet Adapter #3  True  
    vEthernet (MGT)   Hyper-V Virtual Ethernet Adapter #2  False
      

### <a name="validate-rdma-functionality-from-the-remote-system"></a>Valider la fonctionnalité RDMA à partir du système distant

Vous pouvez utiliser cette section pour valider la fonctionnalité RDMA à partir du système à distance sur le système local, qui dispose d’un commutateur virtuel, ce test les deux cartes sont membres de l’équipe de jeu vSwitch.

Étant donné que les deux cartes réseau virtuelles hôtes \ (SMB1 et SMB2\) sont attribués à un réseau local virtuel 102, vous pouvez sélectionner la carte réseau local virtuel 102 sur le système distant.  

Dans ce processus, le Test-40G-2cartes réseau est RDMA à SMB1 (192.168.2.111) et SMB2 (192.168.2.222).

Facultatif: Vous devrez peut-être désactiver le pare-feu sur ce système.  Pour plus d’informations, consultez votre stratégie d’infrastructure.

    
    Set-NetFirewallProfile -All -Enabled False
    
    Get-NetAdapterAdvancedProperty -Name "Test-40G-2"
    
    Name  DisplayNameDisplayValue   RegistryKeyword RegistryValue  
    ----  -----------------------   --------------- -------------  
    .
    .
    Test-40G-2VLAN ID102VlanID  {102} 
    
    Get-NetAdapter
    
    Name  InterfaceDescriptionifIndex Status   MacAddress LinkSpeed
    ----  --------------------------- ------   ---------- ---------
    Test-40G-2Mellanox ConnectX-3 Pro Ethernet A...#3   3 Up   E4-1D-2D-07-43-D140 Gbps
    
    
    Get-NetAdapterRdma
    
    Name  InterfaceDescription Enabled
    ----  -------------------- -------
    Test-40G-2Mellanox ConnectX-3 Pro Ethernet Adap... True   
    
    
    C:\TEST\Test-RDMA.PS1 -IfIndex 3 -IsRoCE $true -RemoteIpAddress 192.168.2.111 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
    VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\\diskspd.exe
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
    
    
    C:\TEST\Test-RDMA.PS1 -IfIndex 3 -IsRoCE $true -RemoteIpAddress 192.168.2.222 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
    VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\\diskspd.exe
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
    
### <a name="test-for-rdma-traffic-from-the-local-to-the-remote-computer"></a>Test pour le trafic RDMA à partir de l’ordinateur local à l’ordinateur distant

Dans cette section, vous pouvez vérifier que le trafic RDMA est envoyé à partir de l’ordinateur local à l’ordinateur distant.

Vous pouvez utiliser les exemples de commandes suivants pour tester et afficher le flux de trafic.

    
    Get-NetAdapter | ft –AutoSize
    
    Name  InterfaceDescriptionifIndex Status   MacAddress LinkSpeed
    ----  --------------------------- ------   ---------- ---------
    vEthernet (SMB2)  Hyper-V Virtual Ethernet Adapter #4  45 Up   00-15-5D-30-AA-0380 Gbps
    vEthernet (SMB1)  Hyper-V Virtual Ethernet Adapter #3  41 Up   00-15-5D-30-AA-0280 Gbps
    
    
    C:\TEST\Test-RDMA.PS1 -IfIndex 41 -IsRoCE $true -RemoteIpAddress 192.168.2.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
    VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\\diskspd.exe
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
    
    
    C:\TEST\Test-RDMA.PS1 -IfIndex 45 -IsRoCE $true -RemoteIpAddress 192.168.2.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
    VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\\diskspd.exe
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
    
La dernière ligne dans cette sortie, «test de trafic RDMA réussite: trafic RDMA a été envoyé à 192.168.2.5, «montre que vous avez correctement configuré NIC convergée sur votre carte.

## <a name="all-topics-in-this-guide"></a>Toutes les rubriques de ce guide

Ce guide contient les rubriques suivantes.

- [Configuration des cartes réseau convergé avec une seule carte réseau](cnic-single.md)
- [Configuration des cartes réseau associées de cartes réseau convergé](cnic-datacenter.md)
- [Configuration du commutateur physique pour les cartes réseau convergé](cnic-app-switch-config.md)
- [Résolution des problèmes convergent des Configurations de cartes réseau](cnic-app-troubleshoot.md)
