---
ms.assetid: f7002265-60fa-40b8-9dd7-4bf131d9320a
title: "Nom d’ordinateur"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: d0dbe2da76a1cf3d1a4dd74183b5dd7106ef17b9
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="computer-naming"></a>Nom d’ordinateur

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Lorsqu’un ordinateur exécutant le système d’exploitation Windows2000, WindowsXP, Windows Server2003, Windows Server2008 ou WindowsVista rejoint un domaine, par défaut, l’ordinateur s’attribue un nom. Le nom, il s’attribue comprend le nom d’hôte de l’ordinateur (autrement dit, le nom d’ordinateur dans les propriétés système) et le nom du système DNS (Domain Name) du domaine ActiveDirectory qui joint l’ordinateur (c'est-à-dire, suffixe DNS principal dans les propriétés système). La concaténation du nom d’hôte et le nom DNS du domaine porte le nom de domaine complet (FQDN). Par exemple, si un ordinateur avec un nom d’hôte Server1 rejoint le domaine corp.contoso.com, le nom de domaine complet de l’ordinateur est server1.corp.contoso.com.  
  
Si un ordinateur possède déjà un nom de domaine DNS différent qui a été statiquement entré dans une zone DNS ou inscrit par un service de serveur DNS/dynamiques DHCP Host Configuration Protocol () intégré, le nom de domaine complet de l’ordinateur est différent de celui qui a été inscrit auparavant. L’ordinateur peut être référencé par nom.  
  
Pour plus d’informations sur les conventions d’affectation de noms dans les Services de domaine ActiveDirectory (ADDS), voir les conventions d’affectation de noms dans ActiveDirectory pour les ordinateurs, domaines, sites et unités d’organisation ([https://go.microsoft.com/fwlink/?LinkID=106629](https://go.microsoft.com/fwlink/?LinkID=106629)).  
  


