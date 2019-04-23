---
title: DNS (Domain Name System)
description: Cette rubrique fournit une vue d’ensemble de DNS dans Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: 1324ba18-4e28-4b9d-bbe7-75707e6d30ab
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3d4ec63e904dd899a3ddc53a59274ad607136edd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870820"
---
# <a name="domain-name-system-dns"></a>DNS (Domain Name System)

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Système DNS (Domain Name) est un de la suite de standards du secteur en matière de protocoles qui utilisent TCP/IP et le Client DNS et le serveur DNS fournissent ensemble des services de résolution de nom ordinateur nom de l’adresse mappage aux utilisateurs et ordinateurs.  
  
> [!NOTE]  
> Outre cette rubrique, le contenu DNS suivant est disponible.  
>   
> -   [Quelles sont les nouveautés dans le Client DNS](What-s-New-in-DNS-Client.md)  
> -   [Nouveautés du serveur DNS](What-s-New-in-DNS-Server.md)  
> -   [Guide de scénario de stratégie DNS](deploy/DNS-Policy-Scenario-Guide.md)  
> -   Vidéo : [Windows Server 2016 : Gestion du service DNS dans IPAM](https://channel9.msdn.com/Blogs/windowsserver/Windows-Server-2016-DNS-management-in-IPAM)  
  
Dans Windows Server 2016, DNS est un rôle de serveur que vous pouvez installer à l’aide des commandes de gestionnaire de serveur ou Windows PowerShell. Si vous installez une nouvelle forêt Active Directory et le même domaine, DNS est automatiquement installé avec Active Directory que le serveur de Catalogue Global pour la forêt et le domaine.  
  
Services de domaine Active Directory (AD DS) utilise DNS comme son mécanisme de localisation de contrôleur de domaine. Lors d’une des opérations Active Directory principal, telles que l’authentification, la mise à jour, ou effectuez une recherche, les ordinateurs utilisent DNS pour rechercher les contrôleurs de domaine Active Directory. En outre, les contrôleurs de domaine utilisent DNS se pour trouver mutuellement.  
  
Le service Client DNS est inclus dans toutes les versions client et serveur du système d’exploitation Windows et est en cours d’exécution par défaut lors de l’installation du système d’exploitation. Lorsque vous configurez une connexion réseau TCP/IP avec l’adresse IP d’un serveur DNS, le Client DNS interroge le serveur DNS pour découvrir les contrôleurs de domaine et pour résoudre les noms d’ordinateurs aux adresses IP. Par exemple, lorsqu’un utilisateur de réseau avec un compte d’utilisateur Active Directory se connecte à un domaine Active Directory, le service Client DNS interroge le serveur DNS pour localiser un contrôleur de domaine pour le domaine Active Directory. Lorsque le serveur DNS répond à la requête et fournit l’adresse IP du contrôleur de domaine pour le client, le client contacte le contrôleur de domaine et le processus d’authentification peut commencer.  
  
Les services de serveur DNS de Windows Server 2016 et le Client DNS utilisent le protocole DNS qui est inclus dans la suite de protocoles TCP/IP. DNS fait partie de la couche d’application du modèle de référence TCP/IP, comme indiqué dans l’illustration suivante.  
  
![DNS dans TCP/IP](../media/Domain-Name-System--DNS-/dns_in_tcpip.jpg)  
  

