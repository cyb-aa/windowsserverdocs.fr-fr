---
title: DNS (Domain Name System)
description: Cette rubrique fournit une vue d’ensemble du DNS dans Windows Server 2016
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dns
ms.topic: article
ms.assetid: 1324ba18-4e28-4b9d-bbe7-75707e6d30ab
ms.author: lizross
author: eross-msft
ms.openlocfilehash: f23b6c1056ea29f583da055b303fb648539240ba
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80317712"
---
# <a name="domain-name-system-dns"></a>DNS (Domain Name System)

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Le système DNS (Domain Name System) est l’une des gammes de protocoles standard qui composent le protocole TCP/IP, et le client DNS et le serveur DNS fournissent des services de résolution de noms de nom d’ordinateur à adresse IP aux ordinateurs et aux utilisateurs.  
  
> [!NOTE]  
> En plus de cette rubrique, le contenu DNS suivant est disponible.  
>   
> -   [Nouveautés du client DNS](What-s-New-in-DNS-Client.md)  
> -   [Nouveautés du serveur DNS](What-s-New-in-DNS-Server.md)  
> -   [Guide du scénario de stratégie DNS](deploy/DNS-Policy-Scenario-Guide.md)  
> -   Vidéo : [Windows Server 2016 : gestion DNS dans IPAM](https://channel9.msdn.com/Blogs/windowsserver/Windows-Server-2016-DNS-management-in-IPAM)  
  
Dans Windows Server 2016, DNS est un rôle serveur que vous pouvez installer à l’aide de Gestionnaire de serveur ou de commandes Windows PowerShell. Si vous installez une nouvelle forêt et un nouveau domaine Active Directory, DNS est automatiquement installé avec Active Directory en tant que serveur de catalogue global pour la forêt et le domaine.  
  
Active Directory Domain Services (AD DS) utilise DNS en tant que mécanisme d’emplacement du contrôleur de domaine. Lorsque l’une des opérations de Active Directory principal est effectuée, telles que l’authentification, la mise à jour ou la recherche, les ordinateurs utilisent le DNS pour localiser Active Directory contrôleurs de domaine. En outre, les contrôleurs de domaine utilisent DNS pour les localiser les uns les autres.  
  
Le service client DNS est inclus dans toutes les versions client et serveur du système d’exploitation Windows et s’exécute par défaut lors de l’installation du système d’exploitation. Quand vous configurez une connexion réseau TCP/IP avec l’adresse IP d’un serveur DNS, le client DNS interroge le serveur DNS pour détecter les contrôleurs de domaine et résoudre les noms d’ordinateurs en adresses IP. Par exemple, lorsqu’un utilisateur réseau disposant d’un compte d’utilisateur Active Directory se connecte à un domaine Active Directory, le service client DNS interroge le serveur DNS pour trouver un contrôleur de domaine pour le domaine Active Directory. Lorsque le serveur DNS répond à la requête et fournit l’adresse IP du contrôleur de domaine au client, le client contacte le contrôleur de domaine et le processus d’authentification peut commencer.  
  
Le serveur DNS Windows Server 2016 et les services du client DNS utilisent le protocole DNS inclus dans la suite de protocoles TCP/IP. DNS fait partie de la couche application du modèle de référence TCP/IP, comme indiqué dans l’illustration suivante.  
  
![DNS dans TCP/IP](../media/Domain-Name-System--DNS-/dns_in_tcpip.jpg)  
  

