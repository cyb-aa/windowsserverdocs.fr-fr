---
ms.assetid: 1a6740e6-5b6d-41f8-9ec4-32cdbee3e1bb
title: Configurer la résolution de noms pour un serveur proxy de fédération dans une zone DNS desservant le réseau de périmètre et les clients Internet
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: cef87e725db7068ac4ed93524e09a25de95ec276
ms.sourcegitcommit: 9a4ab3a0d00b06ff16173aed616624c857589459
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/11/2019
ms.locfileid: "66828513"
---
# <a name="configure-name-resolution-for-a-federation-server-proxy-in-a-dns-zone-that-serves-both-the-perimeter-network-and-internet-clients"></a>Configurer la résolution de noms pour un serveur proxy de fédération dans une zone DNS desservant le réseau de périmètre et les clients Internet


Pour que la résolution de noms fonctionne correctement pour un serveur proxy de fédération dans un Active Directory Federation Services \(AD FS\) scénario dans quel système de nom de domaine un ou plusieurs \(DNS\) zones servent à la fois le réseau de périmètre et les clients Internet, les tâches suivantes doivent être effectuées :  
  
-   DNS dans la zone Internet que vous contrôlez doit être configuré pour résoudre le que nom pour le serveur proxy de fédération d’hôte de toutes les demandes de client Internet pour les services AD FS. Pour ce faire, vous ajoutez un hôte \(A\) enregistrement de ressource à la zone DNS Internet pour le serveur proxy de fédération.  
  
-   DNS dans le réseau de périmètre doit être configuré pour résoudre le que nom au serveur de fédération d’hôte de toutes les demandes client entrantes pour les services AD FS. Pour ce faire, vous ajoutez un hôte \(A\) enregistrement de ressource à la zone DNS de périmètre pour le serveur proxy de fédération.  
  
> [!NOTE]  
> Il est supposé qu’un hôte \(A\) enregistrement de ressource pour le serveur de fédération a déjà été créé dans l’entreprise network DNS. Si cet enregistrement n’existe pas encore, créez cet enregistrement, puis effectuez ces procédures. Pour plus d’informations sur la création d’un ordinateur hôte \(A\) enregistrement de ressource pour le serveur de fédération, consultez [ajouter un hôte &#40;A&#41; enregistrement de ressource au DNS d’entreprise pour un serveur de fédération](Add-a-Host--A--Resource-Record-to-Corporate-DNS-for-a-Federation-Server.md).  
  
## <a name="add-a-host-a-resource-record-to-the-internet-dns-zone-for-a-federation-server-proxy"></a>Ajouter un hôte \(A\) enregistrement de ressource à la zone DNS Internet pour un serveur proxy de fédération  
Pour que les ordinateurs clients sur Internet puissent accéder avec succès d’un serveur de fédération via un serveur proxy de fédération qui vient d’être déployé, vous devez d’abord créer un hôte \(A\) enregistrement de ressource dans la zone DNS Internet que vous contrôlez. Cet enregistrement de ressource résout le nom d’hôte du serveur de fédération de compte \(, par exemple, fs.fabrikam.com\) à l’adresse IP du serveur proxy de fédération de compte \(, par exemple, 131.107.27.68\) dans le réseau de périmètre.  
  
> [!NOTE]  
> Il est supposé que vous utilisez un serveur DNS exécutant Windows 2000 Server, Windows Server 2003 ou Windows Server 2008 avec le service serveur DNS pour contrôler la zone DNS Internet.  
  
Pour mener à bien cette procédure, il faut appartenir au groupe **Administrateurs** ou à un groupe équivalent.  Examinez les informations relatives à l’utilisation des comptes et des appartenances au groupe appropriés dans la rubrique [Groupes locaux et de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
#### <a name="to-add-a-host-a-resource-record-to-the-internet-dns-zone-for-a-federation-server-proxy"></a>Pour ajouter un hôte \(A\) enregistrement de ressource à la zone DNS Internet pour un serveur proxy de fédération  
  
1.  Sur un serveur DNS pour la zone DNS Internet, ouvrez le composant logiciel enfichable DNS\-dans.  
  
2.  Dans l’arborescence de la console, avec le bouton droit\-cliquez sur la zone de recherche directe applicable, puis cliquez sur **nouvel hôte \(A ou AAAA\)** .  
  
3.  Dans **nom**, tapez uniquement le nom d’ordinateur du serveur de fédération. Par exemple, pour le nom de domaine pleinement qualifié \(FQDN\) fs.fabrikam.com, tapez **fs**.  
  
4.  Dans **adresse IP**, tapez l’adresse IP pour le nouveau serveur proxy de fédération, par exemple, 131.107.27.68.  
  
5.  Cliquez sur **Ajouter un hôte**.  
  
## <a name="add-a-host-a-resource-record-to-the-perimeter-dns-zone-for-a-federation-server-proxy"></a>Ajouter un hôte \(A\) enregistrement de ressource à la zone DNS de périmètre pour un serveur proxy de fédération  
Afin que les demandes de client Internet peuvent être traités correctement par le serveur proxy de fédération et atteint le serveur de fédération après qu’ils sont résolus par la zone DNS Internet, vous devez créer un hôte \(A\) enregistrement de ressource dans le périmètre Zone DNS. Cet enregistrement de ressource résout le nom d’hôte du serveur de fédération de compte \(, par exemple, fs. Fabrikam.com\) à l’adresse IP du serveur de fédération de compte \(, par exemple, 192.168.1.4\) dans le réseau d’entreprise.  
  
> [!NOTE]  
> Il est supposé que vous utilisez un serveur DNS exécutant Windows 2000 Server, Windows Server 2003, Windows Server 2008 ou Windows Server® 2012 avec le service serveur DNS pour contrôler la zone DNS de périmètre.  
  
Pour mener à bien cette procédure, il faut appartenir au groupe **Administrateurs** ou à un groupe équivalent.  Examinez les informations relatives à l’utilisation des comptes et des appartenances au groupe appropriés dans la rubrique [Groupes locaux et de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
#### <a name="to-add-a-host-a-resource-record-to-the-perimeter-dns-zone-for-a-federation-server-proxy"></a>Pour ajouter un hôte \(A\) enregistrement de ressource à la zone DNS de périmètre pour un serveur proxy de fédération  
  
1.  Sur un serveur DNS pour le réseau de périmètre, ouvrez le **DNS aligner\-dans**.  
  
2.  Dans l’arborescence de la console, avec le bouton droit\-cliquez sur la zone de recherche directe applicable, puis cliquez sur **nouvel hôte \(A ou AAAA\)** .  
  
3.  Dans **nom**, tapez uniquement le nom d’ordinateur du serveur de fédération. Par exemple, pour le FQDN fs.fabrikam.com, tapez **fs**.  
  
4.  Dans le **adresse IP** zone de texte, tapez l’adresse IP d’adresses du serveur de fédération du réseau d’entreprise, par exemple, 192.168.1.4.  
  
5.  Cliquez sur **Ajouter un hôte**.  
  
## <a name="additional-references"></a>Références supplémentaires  
[Liste de vérification : configuration d’un serveur de fédération proxy](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  
[Exigences relatives à la résolution de noms pour les serveurs proxy de fédération](https://technet.microsoft.com/library/dd807055.aspx)  
  

