---
ms.assetid: f7002265-60fa-40b8-9dd7-4bf131d9320a
title: Noms d’ordinateurs
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ae2483571d67b4cdb32c2a547b924b1315573da0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842160"
---
# <a name="computer-naming"></a>Noms d’ordinateurs

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Lorsqu’un ordinateur exécutant le système d’exploitation Windows 2000, Windows XP, Windows Server 2003, Windows Server 2008 ou Windows Vista rejoint un domaine, par défaut, l’ordinateur s’attribue un nom. Le nom, il s’attribue comprend le nom d’hôte de l’ordinateur (autrement dit, le nom d’ordinateur dans les propriétés système) et le nom du système DNS (Domain Name) du domaine Active Directory auquel l’ordinateur joint (autrement dit, suffixe DNS principal dans les propriétés système). La concaténation du nom d’hôte et le nom DNS du domaine est connue sous le nom de domaine complet (FQDN). Par exemple, si un ordinateur hôte nom de Serveur1 jointures le domaine corp.contoso.com, le nom de domaine complet de l’ordinateur est Serveur1.corp.contoso.com.  
  
Si un ordinateur a déjà un autre nom de domaine DNS qui a été statiquement entrer dans une zone DNS ou inscrit par un service DNS/dynamique hôte (serveur DHCP Configuration Protocol) intégré, le nom de domaine complet de l’ordinateur est différent du nom qui a été enregistré précédemment. L’ordinateur peut être référencé par le nom.  
  
Pour plus d’informations sur les conventions d’affectation de noms dans les Services de domaine Active Directory (AD DS), consultez conventions d’affectation de noms dans Active Directory pour les ordinateurs, domaines, sites et unités d’organisation ([https://go.microsoft.com/fwlink/?LinkID=106629](https://go.microsoft.com/fwlink/?LinkID=106629)).  
  


