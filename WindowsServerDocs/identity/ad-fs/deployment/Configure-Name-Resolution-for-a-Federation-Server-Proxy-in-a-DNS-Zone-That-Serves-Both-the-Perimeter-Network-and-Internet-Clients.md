---
ms.assetid: 1a6740e6-5b6d-41f8-9ec4-32cdbee3e1bb
title: "Configurer la résolution de noms pour un serveur Proxy de fédération dans un serveur DNS de la Zone qui sert à la fois le périmètre réseau et les Clients Internet"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 3b154459163b2142ff1d3aba424a86305d093de4
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="configure-name-resolution-for-a-federation-server-proxy-in-a-dns-zone-that-serves-both-the-perimeter-network-and-internet-clients"></a>Configurer la résolution de noms pour un serveur Proxy de fédération dans un serveur DNS de la Zone qui sert à la fois le périmètre réseau et les Clients Internet

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Afin que la résolution de noms peut fonctionner correctement pour un serveur proxy de fédération dans un scénario de \(ADFS\) ActiveDirectory Federation Services dans lequel un ou plusieurs zones de système de nom de domaine \(DNS\) servent le réseau de périmètre et les clients Internet, les tâches suivantes doivent être effectuées:  
  
-   DNS dans la zone Internet que vous contrôlez doit être configuré pour résoudre le que nom de serveur proxy de fédération d’hôte toutes les demandes de client Internet pour les services ADFS. Pour ce faire, vous ajoutez un enregistrement de ressource hôte \(A\) à la zone DNS Internet pour le serveur proxy de fédération.  
  
-   DNS dans le réseau de périmètre doit être configuré pour résoudre le que nom pour le serveur de fédération d’hôte toutes les demandes clientes entrantes pour les services ADFS. Pour ce faire, vous ajoutez un enregistrement de ressource hôte \(A\) à la zone DNS de périmètre pour le serveur proxy de fédération.  
  
> [!NOTE]  
> Il est supposé qu’un enregistrement de ressource hôte \(A\) pour le serveur de fédération a déjà été créé dans le DNS du réseau d’entreprise. Si cet enregistrement n’existe pas, créez cet enregistrement, puis effectuez ces procédures. Pour plus d’informations sur la création d’un enregistrement de ressource hôte \(A\) pour le serveur de fédération, consultez [ajouter un ordinateur hôte et #40; a et #41; enregistrement de ressource au DNS d’entreprise pour un serveur de fédération](Add-a-Host--A--Resource-Record-to-Corporate-DNS-for-a-Federation-Server.md).  
  
## <a name="add-a-host-a-resource-record-to-the-internet-dns-zone-for-a-federation-server-proxy"></a>Ajouter un enregistrement de ressource hôte \(A\) à la zone DNS Internet pour un serveur proxy de fédération  
Afin que les ordinateurs clients sur Internet puissent accéder avec succès un serveur de fédération par le biais d’un serveur proxy de fédération qui vient d’être déployé, vous devez d’abord créer un enregistrement de ressource hôte \(A\) dans la zone DNS Internet que vous contrôlez. Cet enregistrement de ressource résout le nom d’hôte du serveur de fédération compte \ (par exemple, fs.fabrikam.com\) à l’adresse IP du serveur proxy de fédération compte \ (par exemple, 131.107.27.68\) dans le réseau de périmètre.  
  
> [!NOTE]  
> Il est supposé que vous utilisez un serveur DNS exécutant Windows2000 Server, Windows Server2003 ou Windows Server2008 avec le service serveur DNS pour contrôler la zone DNS Internet.  
  
L’appartenance au groupe **administrateurs**, ou équivalente, est la condition minimale requise pour effectuer cette procédure.  Examinez les informations relatives à l’aide des comptes appropriés et les appartenances au groupe [locaux et groupes de domaine par défaut](http://go.microsoft.com/fwlink/?LinkId=83477).   
  
#### <a name="to-add-a-host-a-resource-record-to-the-internet-dns-zone-for-a-federation-server-proxy"></a>Pour ajouter un enregistrement de ressource hôte \(A\) à la zone DNS Internet pour un serveur proxy de fédération  
  
1.  Sur un serveur DNS pour la zone DNS Internet, ouvrez le DNS de composants.  
  
2.  Dans la console de l’arborescence, la zone de recherche directe applicable, clic droit, puis cliquez sur **nouvel hôte \(A or AAAA\)**.  
  
3.  Dans **nom**, tapez uniquement le nom d’ordinateur du serveur de fédération. Par exemple, pour le nom de domaine complet nom \(FQDN\) fs.fabrikam.com, type **fs**.  
  
4.  Dans **adresse IP**, tapez l’adresse IP pour le nouveau serveur proxy de fédération, par exemple, 131.107.27.68.  
  
5.  Cliquez sur **ajouter l’hôte**.  
  
## <a name="add-a-host-a-resource-record-to-the-perimeter-dns-zone-for-a-federation-server-proxy"></a>Ajouter un enregistrement de ressource hôte \(A\) à la zone DNS de périmètre pour un serveur proxy de fédération  
Afin que les demandes des clients Internet peuvent être traitées par le serveur proxy de fédération et une fois qu’ils sont résolus par la zone DNS Internet d’atteindre le serveur de fédération, vous devez créer un enregistrement de ressource hôte \(A\) dans la zone DNS de périmètre. Cet enregistrement de ressource résout le nom d’hôte du serveur de fédération compte \ (par exemple, fs. fabrikam.com\) à l’adresse IP du serveur de fédération compte \ (par exemple, 192.168.1.4\) dans le réseau d’entreprise.  
  
> [!NOTE]  
> Il est supposé que vous utilisez un serveur DNS exécutant Windows2000 Server, Windows Server2003, Windows Server2008 ou Windows Server® 2012 avec le service serveur DNS pour contrôler la zone DNS de périmètre.  
  
L’appartenance au groupe **administrateurs**, ou équivalente, est la condition minimale requise pour effectuer cette procédure.  Examinez les informations relatives à l’aide des comptes appropriés et les appartenances au groupe [locaux et groupes de domaine par défaut](http://go.microsoft.com/fwlink/?LinkId=83477).   
  
#### <a name="to-add-a-host-a-resource-record-to-the-perimeter-dns-zone-for-a-federation-server-proxy"></a>Pour ajouter un enregistrement de ressource hôte \(A\) à la zone DNS de périmètre pour un serveur proxy de fédération  
  
1.  Sur un serveur DNS du réseau de périmètre, ouvrez le **DNS enfichable**.  
  
2.  Dans la console de l’arborescence, la zone de recherche directe applicable, clic droit, puis cliquez sur **nouvel hôte \(A or AAAA\)**.  
  
3.  Dans **nom**, tapez uniquement le nom d’ordinateur du serveur de fédération. Par exemple, pour le nom de domaine complet fs.fabrikam.com, tapez **fs**.  
  
4.  Dans le **adresse IP** zone de texte, tapez l’adresse IP d’adresse du serveur de fédération dans le réseau d’entreprise, par exemple, 192.168.1.4.  
  
5.  Cliquez sur **ajouter l’hôte**.  
  
## <a name="additional-references"></a>Références supplémentaires  
[Liste de vérification: Configuration d’un serveur Proxy de fédération](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  
[Exigences de résolution de nom de serveur proxy de fédération](https://technet.microsoft.com/library/dd807055.aspx)  
  

