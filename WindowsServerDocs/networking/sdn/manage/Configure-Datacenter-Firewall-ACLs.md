---
title: Configurer des listes de contrôle d’accès (ACL) du pare-feu de centre de données
description: Vous pouvez appliquer des listes ACL spécifiques aux interfaces réseau.  Si les ACL sont également définies sur le sous-réseau virtuel auquel l’interface réseau est connectée, les ACL sont appliquées, mais l’interface réseau ACL sont prioritaires sur l’ACL de sous-réseau virtuel.
manager: dougkim
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 25f18927-a63e-44f3-b02a-81ed51933187
ms.author: pashort
author: shortpatti
ms.date: 08/23/2018
ms.openlocfilehash: 77a7706e39da265eedd65342a0ccf2174ab050ea
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853400"
---
# <a name="configure-datacenter-firewall-access-control-lists-acls"></a>Configurer le pare-feu de centre de données listes de contrôle d’accès (ACL)

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Une fois que vous avez créé une liste ACL et attribué à un sous-réseau virtuel, vous souhaiterez peut-être substituer cette valeur par défaut ACL sur le sous-réseau virtuel avec une ACL spécifique pour une interface réseau individuelle.  Dans ce cas, vous appliquez les ACL spécifiques directement à des interfaces réseau attachées à des réseaux locaux virtuels, plutôt que le réseau virtuel. Si vous avez des ACL n’est définie sur le sous-réseau virtuel connecté à l’interface réseau, les deux ACL sont appliquées et hiérarchise l’ACL ci-dessus les ACL de sous-réseau virtuel d’interface réseau.

>[!IMPORTANT]
>Si vous n’avez pas créé une liste ACL et attribué à un réseau virtuel, consultez [utiliser Access Control Lists (ACL) pour le flux du trafic réseau centre de données gérer](Use-Access-Control-Lists--ACLs--to-Manage-Datacenter-Network-Traffic-Flow.md) pour créer une liste ACL et l’affecter à un sous-réseau virtuel.  

Dans cette rubrique, nous vous montrons comment ajouter une ACL à une interface réseau. Nous vous montrons également comment supprimer une liste ACL à partir d’une interface réseau à l’aide de Windows PowerShell et l’API REST du contrôleur de réseau.

- [Exemple : Ajouter une ACL à une interface réseau](#example-add-an-acl-to-a-network-interface)
- [Exemple : Supprimer une liste ACL à partir d’une interface réseau à l’aide de Windows Powershell et l’API REST du contrôleur de réseau](#example-remove-an-acl-from-a-network-interface-by-using-windows-powershell-and-the-network-controller-rest-api)


## <a name="example-add-an-acl-to-a-network-interface"></a>Exemple : Ajouter une ACL à une interface réseau
Dans cet exemple, nous montre comment ajouter une ACL à un réseau virtuel. 

>[!TIP]
>Il est également possible d’ajouter une liste ACL en même temps que vous créez l’interface réseau.

1. Obtient ou crée l’interface réseau à laquelle vous allez ajouter la liste ACL.
 
   ```PowerShell
   $nic = get-networkcontrollernetworkinterface -ConnectionUri $uri -ResourceId "MyVM_Ethernet1"
   ```
 
2. Obtient ou crée la liste ACL que vous allez ajouter à l’interface réseau.
 
   ```PowerShell
   $acl = get-networkcontrolleraccesscontrollist -ConnectionUri $uri -resourceid "AllowAllACL"
   ```
 
3. Affecter l’ACL à la propriété de l’objet AccessControlList de l’interface réseau
 
   ```PowerShell
    $nic.properties.ipconfigurations[0].properties.AccessControlList = $acl
   ```
 
4. Ajouter l’interface réseau dans le contrôleur de réseau
 
   ```
   new-networkcontrollernetworkinterface -ConnectionUri $uri -Properties $nic.properties -ResourceId $nic.resourceid
   ```
 
## <a name="example-remove-an-acl-from-a-network-interface-by-using-windows-powershell-and-the-network-controller-rest-api"></a>Exemple : Supprimer une liste ACL à partir d’une interface réseau à l’aide de Windows Powershell et l’API REST du contrôleur de réseau
Dans cet exemple, nous vous montrons comment supprimer une liste ACL. Suppression d’une liste ACL, l’ensemble de règles par défaut s’applique à l’interface réseau. L’ensemble de règles par défaut autorise tout le trafic sortant mais bloque tout le trafic entrant.

>[!NOTE]
>Si vous souhaitez autoriser tout le trafic entrant, vous devez suivre le précédent [exemple](#example-add-an-acl-to-a-network-interface) pour ajouter une ACL qui autorise le trafic entrant et sortant de toutes les.


1. Obtenir l’interface réseau à partir duquel vous allez supprimer l’ACL.<br>
   ```PowerShell
   $nic = get-networkcontrollernetworkinterface -ConnectionUri $uri -ResourceId "MyVM_Ethernet1"
   ```
 
2. Assigner $NULL à la propriété de l’objet AccessControlList de la configuration IP.<br>
   ```PowerShell
   $nic.properties.ipconfigurations[0].properties.AccessControlList = $null
   ```
 
3. Ajoutez l’objet d’interface réseau dans le contrôleur de réseau.<br>
   ```PowerShell
   new-networkcontrollernetworkinterface -ConnectionUri $uri -Properties $nic.properties -ResourceId $nic.resourceid
   ```
