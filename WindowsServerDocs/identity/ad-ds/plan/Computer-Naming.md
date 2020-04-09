---
ms.assetid: f7002265-60fa-40b8-9dd7-4bf131d9320a
title: Noms d’ordinateurs
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 8355b5b54e2ff17a4230c658b63745158f573fce
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80822812"
---
# <a name="computer-naming"></a>Noms d’ordinateurs

>S’applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Lorsqu’un ordinateur exécutant le système d’exploitation Windows 2000, Windows XP, Windows Server 2003, Windows Server 2008 ou Windows Vista joint un domaine, par défaut, l’ordinateur s’attribue un nom. Le nom qu’il attribue lui-même comprend le nom d’hôte de l’ordinateur (autrement dit, le nom de l’ordinateur dans les propriétés système) et le nom DNS (Domain Name System) du domaine Active Directory auquel l’ordinateur est joint (autrement dit, le suffixe DNS principal dans les propriétés système). La concaténation du nom d’hôte et du nom DNS du domaine est appelée nom de domaine complet (FQDN, Fully Qualified Domain Name). Par exemple, si un ordinateur avec le nom d’hôte Serveur1 rejoint le domaine corp.contoso.com, le nom de domaine complet de l’ordinateur est server1.corp.contoso.com.  
  
Si un ordinateur possède déjà un autre nom de domaine DNS qui a été entré de manière statique dans une zone DNS ou s’il est enregistré par un service serveur DHCP/DHCP (Dynamic Host Configuration Protocol) intégré, le nom de domaine complet de l’ordinateur est différent du nom inscrit précédemment. L’ordinateur peut être référencé par un nom.  
  
Pour plus d’informations sur les conventions d’affectation de noms dans Active Directory Domain Services (AD DS), consultez conventions d’affectation de noms dans Active Directory pour les ordinateurs, les domaines, les sites et les unités d’organisation ([https://go.microsoft.com/fwlink/?LinkID=106629](https://go.microsoft.com/fwlink/?LinkID=106629)).  
  


