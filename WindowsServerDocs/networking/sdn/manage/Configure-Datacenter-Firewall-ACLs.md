---
title: Configurer des listes de contrôle d’accès (ACL) des pare-feu de centre de données
description: Cette rubrique fait partie du guide logiciel défini de mise en réseau sur la façon de gérer les charges de travail clientes et des réseaux virtuels dans Windows Server2016.
manager: brianlic
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
ms.openlocfilehash: d5f7c4baad24a720e073857cb6c835167e5b419b
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="configure-datacenter-firewall-access-control-lists-acls"></a>Configurer des listes de contrôle d’accès (ACL) des pare-feu de centre de données

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez appliquer les listes ACL spécifiques aux interfaces réseau.  Si les ACL sont également définis sur le sous-réseau virtuel auquel l’interface réseau est connectée, les ACL sont appliqués, mais l’interface réseau ACL sont prioritaires au-dessus du sous-réseau virtuel ACL.

Cette rubrique contient les sections suivantes.

- [Exemple: Ajouter une ACL à une interface réseau](#bkmk_addacl)
- [Exemple: Supprimer une liste ACL à partir d’une interface réseau à l’aide de Windows Powershell et l’API REST de contrôleur de réseau](#bkmk_removeacl)

##<a name="bkmk_addacl"></a>Exemple: Ajouter une ACL à une interface réseau

Dans la rubrique [utilisation Access Control Lists (ACL) pour gérer les centres de données réseau le flux de trafic](Use-Access-Control-Lists--ACLs--to-Manage-Datacenter-Network-Traffic-Flow.md) vous avez appris comment créer une liste ACL et affectez-le à un sous-réseau virtuel.  Dans certains cas, toutefois, vous souhaiterez peut-être remplacer cette valeur par défaut ACL sur le sous-réseau virtaul avec une ACL spécifique pour une interface réseau individuelles.  Vous devez également appliquer les ACL directement aux interfaces réseau qui sont connectés aux réseaux locaux virtuels au lieu de réseaux virtuels.

Cet exemple montre comment ajouter une ACL à un réseau virtuel. 

>[!NOTE]
>Il est également possible d’ajouter une ACL en même temps que vous créez l’interface réseau.

###<a name="step-1-get-or-create-the-network-interface-to-which-you-will-add-the-acl"></a>Étape1: Obtenir ou créer l’interface de réseau auquel vous allez ajouter la liste ACL

    $nic = get-networkcontrollernetworkinterface -ConnectionUri $uri -ResourceId "MyVM_Ethernet1"

###<a name="step-2-get-or-create-the-acl-you-will-add-to-the-network-interface"></a>Étape2: Obtenir ou de créer la liste ACL que vous ajouterez à l’interface réseau
Vous pouvez utiliser la commande suivante pour obtenir ou de créer la liste ACL. 

    $acl = get-networkcontrolleraccesscontrollist -ConnectionUri $uri -resourceid "AllowAllACL"

###<a name="step-3-assign-the-acl-to-the-accesscontrollist-property-of-the-network-interface"></a>Étape3: Affecter la liste ACL à la propriété AccessControlList de l’interface réseau
Vous pouvez utiliser la commande suivante pour attribuer la liste ACL à la propriété AccessControlList.

    $nic.properties.ipconfigurations[0].properties.AccessControlList = $acl

###<a name="step-4-add-the-network-interface-in-network-controller"></a>Étape4: Ajouter l’interface réseau dans le contrôleur de réseau
Vous pouvez utiliser la commande suivante pour ajouter l’interface réseau dans le contrôleur de réseau.

    new-networkcontrollernetworkinterface -ConnectionUri $uri -Properties $nic.properties -ResourceId $nic.resourceid


##<a name="bkmk_removeacl"></a>Exemple: Supprimer une liste ACL à partir d’une interface réseau à l’aide de Windows Powershell et l’API REST de contrôleur de réseau
Vous pouvez utiliser cet exemple si vous souhaitez supprimer une liste ACL. Lorsque vous supprimez une liste ACL, l’ensemble de règles par défaut sont appliquées à l’interface réseau.

L’ensemble de règles par défaut autorise tout le trafic sortant, mais il bloque tout le trafic entrant.

>[!NOTE]
>Si vous souhaitez autoriser tout le trafic entrant, vous devez suivre l’exemple précédent pour ajouter une ACL qui permet de trafic entrant et sortant tout.

###<a name="step-1-get-the-network-interface-from-which-you-will-remove-the-acl"></a>Étape1: Obtenir l’interface réseau à partir duquel vous allez supprimer la liste ACL
Vous pouvez utiliser la commande suivante pour récupérer l’interface réseau.

    $nic = get-networkcontrollernetworkinterface -ConnectionUri $uri -ResourceId "MyVM_Ethernet1"

###<a name="step-2-assign-null-to-the-accesscontrollist-property-of-the-ipconfiguration"></a>Étape2: Affecter $NULL à la propriété AccessControlList de l’IPFichier
Vous pouvez utiliser la commande suivante pour attribuer $NULL à la propriété AccessControlList.

    $nic.properties.ipconfigurations[0].properties.AccessControlList = $null

###<a name="step-3-add-the-network-interface-object-in-network-controller"></a>Étape3: Ajouter l’objet d’interface réseau dans le contrôleur de réseau
Vous pouvez utiliser la commande suivante pour ajouter l’objet d’interface réseau dans le contrôleur de réseau,

    new-networkcontrollernetworkinterface -ConnectionUri $uri -Properties $nic.properties -ResourceId $nic.resourceid

