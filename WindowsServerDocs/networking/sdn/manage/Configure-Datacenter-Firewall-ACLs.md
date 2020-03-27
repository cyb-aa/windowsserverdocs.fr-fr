---
title: Configurer des listes de contrôle d’accès (ACL) du pare-feu de centre de données
description: Vous pouvez appliquer des listes de contrôle d’accès spécifiques à des interfaces réseau.  Si les listes de contrôle d’accès sont également définies sur le sous-réseau virtuel auquel l’interface réseau est connectée, les deux listes de contrôle d’accès sont appliquées, mais les ACL de l’interface réseau sont hiérarchisées au-dessus des ACL du sous-réseau virtuel.
manager: dougkim
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 25f18927-a63e-44f3-b02a-81ed51933187
ms.author: lizross
author: eross-msft
ms.date: 08/23/2018
ms.openlocfilehash: fdf9f7dbe8fb9541cc8f77cfbce014c5210296d5
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80317665"
---
# <a name="configure-datacenter-firewall-access-control-lists-acls"></a>Configurer des listes de Access Control de pare-feu de centre de donnes

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Une fois que vous avez créé une liste de contrôle d’accès et que vous l’avez affectée à un sous-réseau virtuel, vous pouvez remplacer cette liste de contrôle d’accès par défaut sur le sous-réseau virtuel par une liste de contrôle d’accès spécifique pour une interface réseau individuelle.  Dans ce cas, vous appliquez directement des listes de contrôle d’accès spécifiques aux interfaces réseau attachées aux réseaux locaux virtuels, et non au réseau virtuel. Si des listes de contrôle d’accès sont définies sur le sous-réseau virtuel connecté à l’interface réseau, les deux listes de contrôle d’accès sont appliquées et hiérarchisent les ACL de l’interface réseau au-dessus des ACL du sous-réseau virtuel.

>[!IMPORTANT]
>Si vous n’avez pas créé de liste de contrôle d’accès et que vous l’avez attribuée à un réseau virtuel, consultez [utiliser des listes de Access Control pour gérer le flux de trafic réseau du centre de gestion](Use-Access-Control-Lists--ACLs--to-Manage-Datacenter-Network-Traffic-Flow.md) pour créer une liste de contrôle d’accès et l’affecter à un sous-réseau virtuel.  

Dans cette rubrique, nous vous montrons comment ajouter une liste de contrôle d’accès à une interface réseau. Nous vous montrons également comment supprimer une liste de contrôle d’accès d’une interface réseau à l’aide de Windows PowerShell et de l’API REST du contrôleur de réseau.

- [Exemple : ajouter une liste de contrôle d’accès à une interface réseau](#example-add-an-acl-to-a-network-interface)
- [Exemple : supprimer une liste de contrôle d’accès d’une interface réseau à l’aide de Windows PowerShell et de l’API REST du contrôleur de réseau](#example-remove-an-acl-from-a-network-interface-by-using-windows-powershell-and-the-network-controller-rest-api)


## <a name="example-add-an-acl-to-a-network-interface"></a>Exemple : ajouter une liste de contrôle d’accès à une interface réseau
Dans cet exemple, nous expliquons comment ajouter une liste de contrôle d’accès à un réseau virtuel. 

>[!TIP]
>Il est également possible d’ajouter une liste de contrôle d’accès au moment de la création de l’interface réseau.

1. Récupérez ou créez l’interface réseau à laquelle vous allez ajouter la liste de contrôle d’accès.
 
   ```PowerShell
   $nic = get-networkcontrollernetworkinterface -ConnectionUri $uri -ResourceId "MyVM_Ethernet1"
   ```
 
2. Obtient ou crée la liste de contrôle d’accès que vous allez ajouter à l’interface réseau.
 
   ```PowerShell
   $acl = get-networkcontrolleraccesscontrollist -ConnectionUri $uri -resourceid "AllowAllACL"
   ```
 
3. Affecter la liste de contrôle d’accès à la propriété AccessControlList de l’interface réseau
 
   ```PowerShell
    $nic.properties.ipconfigurations[0].properties.AccessControlList = $acl
   ```
 
4. Ajouter l’interface réseau dans le contrôleur de réseau
 
   ```
   new-networkcontrollernetworkinterface -ConnectionUri $uri -Properties $nic.properties -ResourceId $nic.resourceid
   ```
 
## <a name="example-remove-an-acl-from-a-network-interface-by-using-windows-powershell-and-the-network-controller-rest-api"></a>Exemple : supprimer une liste de contrôle d’accès d’une interface réseau à l’aide de Windows PowerShell et de l’API REST du contrôleur de réseau
Dans cet exemple, nous vous montrons comment supprimer une liste de contrôle d’accès. La suppression d’une liste de contrôle d’accès applique l’ensemble de règles par défaut à l’interface réseau. L’ensemble de règles par défaut autorise tout le trafic sortant, mais bloque tout le trafic entrant.

>[!NOTE]
>Si vous souhaitez autoriser tout le trafic entrant, vous devez suivre l' [exemple](#example-add-an-acl-to-a-network-interface) précédent pour ajouter une liste de contrôle d’accès qui autorise tout le trafic entrant et sortant.


1. Récupérez l’interface réseau à partir de laquelle vous allez supprimer la liste de contrôle d’accès.<br>
   ```PowerShell
   $nic = get-networkcontrollernetworkinterface -ConnectionUri $uri -ResourceId "MyVM_Ethernet1"
   ```
 
2. Assignez $NULL à la propriété AccessControlList de configuration IP.<br>
   ```PowerShell
   $nic.properties.ipconfigurations[0].properties.AccessControlList = $null
   ```
 
3. Ajoutez l’objet d’interface réseau dans le contrôleur de réseau.<br>
   ```PowerShell
   new-networkcontrollernetworkinterface -ConnectionUri $uri -Properties $nic.properties -ResourceId $nic.resourceid
   ```
