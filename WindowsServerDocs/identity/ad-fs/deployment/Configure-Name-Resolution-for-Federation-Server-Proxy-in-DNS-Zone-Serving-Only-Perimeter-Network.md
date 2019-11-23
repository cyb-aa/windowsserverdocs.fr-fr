---
ms.assetid: b7109e46-b66e-4c5c-8b87-a6611d68415a
title: Configurer la résolution de noms pour un serveur proxy de fédération dans une zone DNS desservant uniquement le réseau de périmètre
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: de4627f2e03e6432f4e678cd9ca932819cb483d5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408438"
---
# <a name="configure-name-resolution-for-a-federation-server-proxy-in-a-dns-zone-that-serves-only-the-perimeter-network"></a>Configurer la résolution de noms pour un serveur proxy de fédération dans une zone DNS desservant uniquement le réseau de périmètre


Pour que la résolution de noms puisse fonctionner correctement pour un serveur de Fédération dans un Services ADFS \(AD FS\) scénario dans lequel une ou plusieurs zones DNS \(DNS ne servent que le réseau de périmètre, les tâches suivantes doivent être effectuées :\)  
  
-   Le fichier hosts sur le serveur proxy de Fédération doit être mis à jour pour ajouter l’adresse IP d’un serveur de Fédération.  
  
-   Le DNS dans le réseau de périmètre doit être configuré pour résoudre toutes les demandes de client pour le AD FS nom d’hôte sur le serveur proxy de Fédération. Pour ce faire, vous ajoutez un ordinateur hôte \(un\) enregistrement de ressource au DNS de périmètre pour le serveur proxy de Fédération.  
  
> [!NOTE]  
> Ces procédures partent du principe qu’un ordinateur hôte \(un\) enregistrement de ressource pour le serveur de Fédération a déjà été créé dans le DNS du réseau d’entreprise. Si cet enregistrement n’existe pas encore, créez-le, puis exécutez ces procédures. Pour plus d’informations sur la création de l’ordinateur hôte \(un enregistrement de ressource\) pour le serveur de Fédération, consultez [Ajouter un ordinateur &#40;hôte a&#41; à un serveur DNS d’entreprise pour un serveur de Fédération](Add-a-Host--A--Resource-Record-to-Corporate-DNS-for-a-Federation-Server.md).  
  
## <a name="add-the-ip-address-of-a-federation-server-to-the-hosts-file"></a>Ajouter l’adresse IP d’un serveur de Fédération au fichier hosts  
Pour qu’un serveur proxy de Fédération puisse fonctionner comme prévu dans le réseau de périmètre d’un partenaire de compte, vous devez ajouter une entrée au fichier hosts sur ce serveur proxy de Fédération qui pointe vers le nom d’hôte DNS d’un serveur de Fédération \(par exemple, fs.fabrikam.com\) et \(adresse IP, par exemple, 192.168.1.4\) dans le réseau d’entreprise du partenaire de compte. L’ajout de cette entrée au fichier hosts empêche le serveur proxy de Fédération de se contacter pour résoudre un appel initié par le client\-à un serveur de Fédération du partenaire de compte.  
  
Pour effectuer cette procédure, vous devez au minimum être membre du groupe **Administrateurs**ou d'un groupe équivalent sur l'ordinateur local.  Examinez les informations relatives à l’utilisation des comptes et des appartenances au groupe appropriés dans la rubrique [Groupes locaux et de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
#### <a name="to-add-the-ip-address-of-a-federation-server-to-the-hosts-file"></a>Pour ajouter l’adresse IP d’un serveur de Fédération au fichier hosts  
  
1.  Accédez au dossier% SystemRoot%\\Winnt\\system32\\drivers Directory, puis localisez le fichier **hosts** .  
  
2.  Démarrez le Bloc-notes, puis ouvrez le fichier **hosts**.  
  
3.  Ajoutez l’adresse IP et le nom d’hôte d’un serveur de Fédération du partenaire de compte au fichier **hosts** , comme illustré dans l’exemple suivant :  
  
    **192.168.1.4fs.fabrikam.com**  
  
4.  Enregistrez et fermez le fichier.  
  
## <a name="add-a-host-a-resource-record-to-perimeter-dns-for-a-federation-server-proxy"></a>Ajouter un ordinateur hôte \(un\) enregistrement de ressource au DNS de périmètre pour un serveur proxy de Fédération  
Pour que les clients sur Internet puissent accéder correctement à un serveur de Fédération par le biais d’un serveur proxy de Fédération nouvellement déployé, vous devez d’abord créer un ordinateur hôte \(un\) enregistrement de ressource dans le DNS de périmètre. Cet enregistrement de ressource résout le nom d’hôte du serveur de Fédération de compte \(par exemple, fs.fabrikam.com\) à l’adresse IP du serveur proxy de Fédération de compte \(par exemple, 131.107.27.68\) dans le réseau de périmètre.  
  
> [!NOTE]  
> Il est supposé que vous utilisez un serveur DNS, exécutant Windows 2000 Server, Windows Server 2003 ou Windows Server 2008 avec le service serveur DNS, pour contrôler la zone DNS du périmètre.  
  
Pour mener à bien cette procédure, il faut appartenir au groupe **Administrateurs** ou à un groupe équivalent.  Examinez les informations relatives à l’utilisation des comptes et des appartenances au groupe appropriés dans la rubrique [Groupes locaux et de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
#### <a name="to-add-a-host-a-resource-record-to-perimeter-dns-for-a-federation-server-proxy"></a>Pour ajouter un ordinateur hôte \(un\) enregistrement de ressource au DNS de périmètre pour un serveur proxy de Fédération  
  
1.  Sur un serveur DNS pour le réseau de périmètre, ouvrez le\-du composant logiciel enfichable DNS dans. Cliquez sur **Démarrer**, pointez sur **Outils d’administration**, puis cliquez sur **DNS**.  
  
2.  Dans l’arborescence de la console, cliquez avec le bouton droit\-sur la zone de recherche directe applicable, puis cliquez sur **nouvel hôte \(A ou AAAA\)** .  
  
3.  Dans **nom**, tapez uniquement le nom d’ordinateur du serveur de Fédération. Par exemple, pour le nom de domaine complet \(FQDN\) fs.fabrikam.com, tapez **FS**.  
  
4.  Dans **adresse IP**, tapez l’adresse IP du nouveau serveur proxy de Fédération, par exemple, **131.107.27.68**.  
  
5.  Cliquez sur **Ajouter un hôte**.  
  
## <a name="additional-references"></a>Références supplémentaires  
[Liste de vérification : configuration d’un serveur proxy de Fédération](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  
[Exigences relatives à la résolution de noms pour les serveurs proxy de fédération](https://technet.microsoft.com/library/dd807055.aspx)  
  

