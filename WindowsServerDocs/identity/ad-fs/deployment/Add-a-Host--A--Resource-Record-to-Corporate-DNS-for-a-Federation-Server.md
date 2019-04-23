---
ms.assetid: 026747c7-4c34-41c7-b7ea-27f9a7f64a35
title: Ajouter un enregistrement de ressource hôte (A) au DNS d’entreprise d’un serveur de fédération
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 425cfe794095f1515eb3fae2f1a5e5db90ba3d00
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856180"
---
# <a name="add-a-host-a-resource-record-to-corporate-dns-for-a-federation-server"></a>Ajouter un enregistrement de ressource hôte (A) au DNS d’entreprise d’un serveur de fédération

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


Pour les clients sur l’entreprise réseau puisse accéder à un serveur de fédération à l’aide de l’authentification Windows intégrée, un hôte \(A\) enregistrement de ressource doit d’abord être créé dans le système de nom de domaine d’entreprise \(DNS\) qui résout le nom d’hôte du serveur de fédération de compte \(, par exemple, fs.fabrikam.com\) à l’adresse IP du serveur de fédération ou du cluster de serveur de fédération. Vous pouvez utiliser la procédure suivante pour ajouter un hôte \(A\) enregistrement de ressource au DNS d’entreprise pour un serveur de fédération.  
  
Pour mener à bien cette procédure, il faut appartenir au groupe **Administrateurs** ou à un groupe équivalent.  Examinez les informations relatives à l’utilisation des comptes et des appartenances au groupe appropriés dans la rubrique [Groupes locaux et de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-add-a-host-a-resource-record-to-corporate-dns-for-a-federation-server"></a>Pour ajouter un hôte \(A\) enregistrement de ressource au DNS d’entreprise pour un serveur de fédération  
  
1.  Sur un serveur DNS pour le réseau d’entreprise, ouvrez le composant logiciel enfichable DNS\-dans.  
  
2.  Dans l’arborescence de la console, avec le bouton droit\-cliquez sur la zone de recherche directe applicable, puis cliquez sur **nouvel hôte \(A ou AAAA\)**.  
  
3.  Dans **nom**, tapez uniquement le nom d’ordinateur du serveur de fédération ou du cluster de serveur de fédération ; par exemple, pour le nom de domaine pleinement qualifié \(FQDN\) fs.fabrikam.com, tapez **fs**.  
  
4.  Dans **adresse IP**, tapez l’adresse IP pour le serveur de fédération ou d’un cluster de serveurs de fédération, par exemple, 192.168.1.4.  
  
5.  Cliquez sur **Ajouter un hôte**.  
  
## <a name="additional-references"></a>Références supplémentaires  
[Liste de vérification : Configuration d’un serveur de fédération](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Exigences de résolution de nom pour les serveurs de fédération](https://technet.microsoft.com/library/dd807055.aspx)  
  

