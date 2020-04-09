---
ms.assetid: 1a6740e6-5b6d-41f8-9ec4-32cdbee3e1bb
title: Configurer la résolution de noms pour un serveur proxy de fédération dans une zone DNS desservant le réseau de périmètre et les clients Internet
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 487ba9d90043ada0d401d7e5a9d02872e1872b7e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854932"
---
# <a name="configure-name-resolution-for-a-federation-server-proxy-in-a-dns-zone-that-serves-both-the-perimeter-network-and-internet-clients"></a>Configurer la résolution de noms pour un serveur proxy de fédération dans une zone DNS desservant le réseau de périmètre et les clients Internet


Pour que la résolution de noms puisse fonctionner correctement pour un serveur proxy de Fédération dans un Services ADFS \(AD FS\) scénario dans lequel une ou plusieurs zones DNS \(DNS\)s servent à la fois au réseau de périmètre et aux clients Internet, les tâches suivantes doivent être effectuées :  
  
-   DNS dans la zone Internet que vous contrôlez doit être configuré pour résoudre toutes les demandes de client Internet pour le nom d’hôte AD FS sur le serveur proxy de Fédération. Pour ce faire, vous ajoutez un ordinateur hôte \(un\) enregistrement de ressource à la zone DNS Internet pour le serveur proxy de Fédération.  
  
-   Le DNS dans le réseau de périmètre doit être configuré pour résoudre toutes les demandes entrantes des clients pour le AD FS nom d’hôte sur le serveur de Fédération. Pour ce faire, vous ajoutez un ordinateur hôte \(un\) enregistrement de ressource à la zone DNS de périmètre pour le serveur proxy de Fédération.  
  
> [!NOTE]  
> Il est supposé qu’un ordinateur hôte \(un enregistrement de ressource\) pour le serveur de Fédération a déjà été créé dans le DNS du réseau d’entreprise. Si cet enregistrement n’existe pas encore, créez-le, puis exécutez ces procédures. Pour plus d’informations sur la création d’un ordinateur hôte \(un\) enregistrement de ressource pour le serveur de Fédération, consultez [Ajouter un ordinateur &#40;hôte a&#41; à un serveur DNS d’entreprise pour un serveur de Fédération](Add-a-Host--A--Resource-Record-to-Corporate-DNS-for-a-Federation-Server.md).  
  
## <a name="add-a-host-a-resource-record-to-the-internet-dns-zone-for-a-federation-server-proxy"></a>Ajouter un ordinateur hôte \(un enregistrement de ressource\) à la zone DNS Internet pour un serveur proxy de Fédération  
Pour que les ordinateurs clients sur Internet puissent accéder correctement à un serveur de Fédération par le biais d’un serveur proxy de Fédération nouvellement déployé, vous devez d’abord créer un ordinateur hôte \(un\) enregistrement de ressource dans la zone DNS Internet que vous contrôlez. Cet enregistrement de ressource résout le nom d’hôte du serveur de Fédération de compte \(par exemple, fs.fabrikam.com\) à l’adresse IP du serveur proxy de Fédération de compte \(par exemple, 131.107.27.68\) dans le réseau de périmètre.  
  
> [!NOTE]  
> Il est supposé que vous utilisez un serveur DNS exécutant Windows 2000 Server, Windows Server 2003 ou Windows Server 2008 avec le service serveur DNS pour contrôler la zone DNS Internet.  
  
Pour mener à bien cette procédure, il faut appartenir au groupe **Administrateurs** ou à un groupe équivalent.  Examinez les informations relatives à l’utilisation des comptes et des appartenances au groupe appropriés dans la rubrique [Groupes locaux et de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
#### <a name="to-add-a-host-a-resource-record-to-the-internet-dns-zone-for-a-federation-server-proxy"></a>Pour ajouter un ordinateur hôte \(un enregistrement de ressource\) à la zone DNS Internet pour un serveur proxy de Fédération  
  
1.  Sur un serveur DNS pour la zone DNS Internet, ouvrez le\-du composant logiciel enfichable DNS dans.  
  
2.  Dans l’arborescence de la console, cliquez avec le bouton droit\-sur la zone de recherche directe applicable, puis cliquez sur **nouvel hôte \(A ou AAAA\)** .  
  
3.  Dans **nom**, tapez uniquement le nom d’ordinateur du serveur de Fédération. Par exemple, pour le nom de domaine complet \(FQDN\) fs.fabrikam.com, tapez **FS**.  
  
4.  Dans **adresse IP**, tapez l’adresse IP du nouveau serveur proxy de Fédération, par exemple, 131.107.27.68.  
  
5.  Cliquez sur **Ajouter un hôte**.  
  
## <a name="add-a-host-a-resource-record-to-the-perimeter-dns-zone-for-a-federation-server-proxy"></a>Ajouter un ordinateur hôte \(un enregistrement de ressource\) à la zone DNS de périmètre pour un serveur proxy de Fédération  
Pour que les demandes des clients Internet puissent être traitées correctement par le serveur proxy de Fédération et atteindre le serveur de Fédération une fois qu’elles ont été résolues par la zone DNS Internet, vous devez créer un ordinateur hôte \(un\) enregistrement de ressource dans la zone DNS du périmètre. Cet enregistrement de ressource résout le nom d’hôte du serveur de Fédération de compte \(par exemple, FS. fabrikam.com\) à l’adresse IP du serveur de Fédération de compte \(par exemple, 192.168.1.4\) dans le réseau d’entreprise.  
  
> [!NOTE]  
> Il est supposé que vous utilisez un serveur DNS exécutant Windows 2000 Server, Windows Server 2003, Windows Server 2008 ou Windows Server&reg; 2012 avec le service serveur DNS pour contrôler la zone DNS du périmètre.  
  
Pour mener à bien cette procédure, il faut appartenir au groupe **Administrateurs** ou à un groupe équivalent.  Examinez les informations relatives à l’utilisation des comptes et des appartenances au groupe appropriés dans la rubrique [Groupes locaux et de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
#### <a name="to-add-a-host-a-resource-record-to-the-perimeter-dns-zone-for-a-federation-server-proxy"></a>Pour ajouter un ordinateur hôte \(un enregistrement de ressource\) à la zone DNS de périmètre pour un serveur proxy de Fédération  
  
1.  Sur un serveur DNS pour le réseau de périmètre, ouvrez le **\-du composant logiciel enfichable DNS dans**.  
  
2.  Dans l’arborescence de la console, cliquez avec le bouton droit\-sur la zone de recherche directe applicable, puis cliquez sur **nouvel hôte \(A ou AAAA\)** .  
  
3.  Dans **nom**, tapez uniquement le nom d’ordinateur du serveur de Fédération. Par exemple, pour le nom de domaine complet fs.fabrikam.com, tapez **fs**.  
  
4.  Dans la zone de texte **adresse IP** , tapez l’adresse IP du serveur de Fédération dans le réseau d’entreprise, par exemple 192.168.1.4.  
  
5.  Cliquez sur **Ajouter un hôte**.  
  
## <a name="additional-references"></a>Références supplémentaires  
[Liste de vérification : configuration d’un serveur proxy de Fédération](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  
[Exigences relatives à la résolution de noms pour les serveurs proxy de fédération](https://technet.microsoft.com/library/dd807055.aspx)  
  

