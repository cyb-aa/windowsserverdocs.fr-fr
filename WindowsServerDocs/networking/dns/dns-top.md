---
title: Nom de domaine (DNS)
description: Cette rubrique fournit une vue d’ensemble de DNS dans Windows Server2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: 1324ba18-4e28-4b9d-bbe7-75707e6d30ab
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 23851d2d8015fc6ae9e0653e8a0843f8c4295162
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="domain-name-system-dns"></a>Nom de domaine (DNS)

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Système DNS (Domain Name) est un de la suite de normes du secteur des protocoles qui composent TCP/IP et le Client DNS et le serveur DNS fournissent ensemble ordinateur nom to-IP services de résolution de noms de mappage adresse aux utilisateurs et ordinateurs.  
  
> [!NOTE]  
> Outre cette rubrique, le contenu DNS suivant est disponible.  
>   
> -   [Quelles sont les nouveautés dans le Client DNS](What-s-New-in-DNS-Client.md)  
> -   [Nouveautés du serveur DNS](What-s-New-in-DNS-Server.md)  
> -   [Guide de scénario de stratégie DNS](deploy/DNS-Policy-Scenario-Guide.md)  
> -   Vidéo: [Windows Server2016: gestion du service DNS dans IPAM](https://channel9.msdn.com/Blogs/windowsserver/Windows-Server-2016-DNS-management-in-IPAM)  
  
Dans Windows Server2016, DNS est un rôle de serveur que vous pouvez installer à l’aide des commandes de gestionnaire de serveur ou Windows PowerShell. Si vous installez un nouveau domaine et une forêt ActiveDirectory, DNS est automatiquement installé avec ActiveDirectory que le serveur de Catalogue Global pour la forêt et le domaine.  
  
Services de domaine ActiveDirectory (ADDS) utilise DNS comme mécanisme d’emplacement du contrôleur de domaine. Lorsqu’une des opérations ActiveDirectory principales est effectuée, telles que l’authentification, mise à jour ou de recherche, les ordinateurs utilisent DNS pour localiser les contrôleurs de domaine ActiveDirectory. En outre, les contrôleurs de domaine utilisent DNS pour localiser mutuellement.  
  
Le service Client DNS est inclus dans toutes les versions client et serveur du système d’exploitation Windows et est en cours d’exécution par défaut lors de l’installation du système d’exploitation. Lorsque vous configurez une connexion réseau TCP/IP avec l’adresse IP d’un serveur DNS, le Client DNS interroge le serveur DNS pour découvrir les contrôleurs de domaine et à résoudre les noms d’ordinateur en adresses IP. Par exemple, lorsqu’un utilisateur du réseau avec un compte d’utilisateur ActiveDirectory se connecte à un domaine ActiveDirectory, le service Client DNS interroge le serveur DNS pour localiser un contrôleur de domaine pour le domaine ActiveDirectory. Lorsque le serveur DNS répond à la requête et fournit l’adresse IP du contrôleur de domaine pour le client, le client contacte le contrôleur de domaine et le processus d’authentification peut commencer.  
  
Les services serveur DNS de Windows Server2016 et le Client DNS utilisent le protocole DNS qui est inclus dans la suite de protocoles TCP/IP. DNS fait partie de la couche d’application du modèle de référence TCP/IP, comme illustré dans l’illustration suivante.  
  
![DNS dans TCP/IP](../media/Domain-Name-System--DNS-/dns_in_tcpip.jpg)  
  

