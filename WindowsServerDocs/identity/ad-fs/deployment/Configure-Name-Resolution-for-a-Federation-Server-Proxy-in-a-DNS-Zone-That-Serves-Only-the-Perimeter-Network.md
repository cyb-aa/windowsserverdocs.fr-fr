---
ms.assetid: b7109e46-b66e-4c5c-8b87-a6611d68415a
title: "Configurer la résolution de noms pour un serveur Proxy de fédération dans une Zone DNS qui sert uniquement le réseau de périmètre"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 32b8e3cc133ce95872881115608bb8cfb17b2427
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="configure-name-resolution-for-a-federation-server-proxy-in-a-dns-zone-that-serves-only-the-perimeter-network"></a>Configurer la résolution de noms pour un serveur Proxy de fédération dans une Zone DNS qui sert uniquement le réseau de périmètre

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Afin que la résolution de noms peut fonctionner correctement pour un serveur de fédération dans un scénario de \(ADFS\) ActiveDirectory Federation Services dans lequel un ou plusieurs zones de système de nom de domaine \(DNS\) servent uniquement le réseau de périmètre, les tâches suivantes doivent être effectuées:  
  
-   Le fichier hosts sur le serveur proxy de fédération doit être mis à jour pour ajouter l’adresse IP d’un serveur de fédération.  
  
-   DNS dans le réseau de périmètre doit être configuré pour résoudre le que nom de serveur proxy de fédération d’hôte toutes les demandes de client pour les services ADFS. Pour ce faire, vous ajoutez un enregistrement de ressource hôte \(A\) à DNS de périmètre pour le serveur proxy de fédération.  
  
> [!NOTE]  
> Ces procédures supposent qu’un enregistrement de ressource hôte \(A\) pour le serveur de fédération a déjà été créé dans le DNS du réseau d’entreprise. Si cet enregistrement n’existe pas, créez cet enregistrement, puis effectuer ces procédures. Pour plus d’informations sur la création de l’enregistrement de ressource hôte \(A\) pour le serveur de fédération, consultez [ajouter un ordinateur hôte et #40; a et #41; enregistrement de ressource au DNS d’entreprise pour un serveur de fédération](Add-a-Host--A--Resource-Record-to-Corporate-DNS-for-a-Federation-Server.md).  
  
## <a name="add-the-ip-address-of-a-federation-server-to-the-hosts-file"></a>Ajouter l’adresse IP d’un serveur de fédération dans le fichier hôtes  
Afin qu’un serveur proxy de fédération peut fonctionner comme prévu dans le réseau de périmètre du partenaire de compte, vous devez ajouter une entrée dans le fichier hôtes sur ce serveur proxy de fédération qui pointe vers le nom d’hôte DNS d’un serveur de fédération \ (par exemple, fs.fabrikam.com\) et l’adresse IP \ (par exemple, 192.168.1.4\) dans le réseau d’entreprise du partenaire de compte. Ajout de cette entrée aux fichiers hôtes empêche le serveur proxy de fédération de contacter lui-même pour résoudre un appel initié par le client à un serveur de fédération du partenaire de compte.  
  
L’appartenance au groupe **administrateurs**, ou équivalente, sur l’ordinateur local est la condition minimale requise pour effectuer cette procédure.  Examinez les informations relatives à l’aide des comptes appropriés et les appartenances au groupe [locaux et groupes de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
#### <a name="to-add-the-ip-address-of-a-federation-server-to-the-hosts-file"></a>Pour ajouter l’adresse IP d’un serveur de fédération dans le fichier hôtes  
  
1.  Accédez au dossier %systemroot%\\Winnt\\System32\\Drivers active et recherchez le **hôtes** fichier.  
  
2.  Démarrez le bloc-notes, puis ouvrez le **hôtes** fichier.  
  
3.  Ajouter l’adresse IP et le nom d’hôte d’un serveur de fédération du partenaire de compte pour le **hôtes** de fichiers, comme illustré dans l’exemple suivant:  
  
    **192.168.1.4fs.fabrikam.com**  
  
4.  Enregistrez et fermez le fichier.  
  
## <a name="add-a-host-a-resource-record-to-perimeter-dns-for-a-federation-server-proxy"></a>Ajouter un enregistrement de ressource hôte \(A\) à DNS de périmètre pour un serveur proxy de fédération  
Afin que les clients sur Internet puissent accéder avec succès un serveur de fédération par le biais d’un serveur proxy de fédération qui vient d’être déployé, vous devez d’abord créer un enregistrement de ressource hôte \(A\) dans DNS de périmètre. Cet enregistrement de ressource résout le nom d’hôte du serveur de fédération compte \ (par exemple, fs.fabrikam.com\) à l’adresse IP du serveur proxy de fédération compte \ (par exemple, 131.107.27.68\) dans le réseau de périmètre.  
  
> [!NOTE]  
> Il est supposé que vous utilisez un serveur DNS, exécutant Windows2000 Server, Windows Server2003 ou Windows Server2008 avec le service serveur DNS, pour contrôler la zone DNS de périmètre.  
  
L’appartenance au groupe **administrateurs**, ou équivalente, est la condition minimale requise pour effectuer cette procédure.  Examinez les informations relatives à l’aide des comptes appropriés et les appartenances au groupe [locaux et groupes de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
#### <a name="to-add-a-host-a-resource-record-to-perimeter-dns-for-a-federation-server-proxy"></a>Pour ajouter un enregistrement de ressource hôte \(A\) à DNS de périmètre pour un serveur proxy de fédération  
  
1.  Sur un serveur DNS du réseau de périmètre, ouvrez le DNS de composants. Cliquez sur **Démarrer**, pointez sur **outils d’administration**, puis cliquez sur **DNS**.  
  
2.  Dans la console de l’arborescence, la zone de recherche directe applicable, clic droit, puis cliquez sur **nouvel hôte \(A or AAAA\)**.  
  
3.  Dans **nom**, tapez uniquement le nom d’ordinateur du serveur de fédération. Par exemple, pour le nom de domaine complet nom \(FQDN\) fs.fabrikam.com, type **fs**.  
  
4.  Dans **adresse IP**, tapez l’adresse IP pour le nouveau serveur proxy de fédération, par exemple, **131.107.27.68**.  
  
5.  Cliquez sur **ajouter l’hôte**.  
  
## <a name="additional-references"></a>Références supplémentaires  
[Liste de vérification: Configuration d’un serveur Proxy de fédération](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  
[Exigences de résolution de nom de serveur proxy de fédération](https://technet.microsoft.com/library/dd807055.aspx)  
  

