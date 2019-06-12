---
ms.assetid: b7109e46-b66e-4c5c-8b87-a6611d68415a
title: Configurer la résolution de noms pour un serveur proxy de fédération dans une zone DNS desservant uniquement le réseau de périmètre
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 7d046c720c5c6250b6efa03e068aa66e2a6bbe3d
ms.sourcegitcommit: 9a4ab3a0d00b06ff16173aed616624c857589459
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/11/2019
ms.locfileid: "66828523"
---
# <a name="configure-name-resolution-for-a-federation-server-proxy-in-a-dns-zone-that-serves-only-the-perimeter-network"></a>Configurer la résolution de noms pour un serveur proxy de fédération dans une zone DNS desservant uniquement le réseau de périmètre


Pour que la résolution de noms fonctionne correctement pour un serveur de fédération dans un Active Directory Federation Services \(AD FS\) scénario dans quel système de nom de domaine un ou plusieurs \(DNS\) zones servent uniquement du périmètre réseau, ce qui suit les tâches doivent être effectuées :  
  
-   Le fichier hosts sur le serveur proxy de fédération doit être mis à jour pour ajouter l’adresse IP d’un serveur de fédération.  
  
-   DNS dans le réseau de périmètre doit être configuré pour résoudre le que nom pour le serveur proxy de fédération d’hôte de toutes les demandes de client pour les services AD FS. Pour ce faire, vous ajoutez un hôte \(A\) enregistrement de ressource au DNS de périmètre pour le serveur proxy de fédération.  
  
> [!NOTE]  
> Ces procédures supposent qu’un hôte \(A\) enregistrement de ressource pour le serveur de fédération a déjà été créé dans l’entreprise network DNS. Si cet enregistrement n’existe pas encore, créez cet enregistrement et puis exécuter ces procédures. Pour plus d’informations sur la création de l’hôte \(A\) enregistrement de ressource pour le serveur de fédération, consultez [ajouter un hôte &#40;A&#41; enregistrement de ressource au DNS d’entreprise pour un serveur de fédération](Add-a-Host--A--Resource-Record-to-Corporate-DNS-for-a-Federation-Server.md).  
  
## <a name="add-the-ip-address-of-a-federation-server-to-the-hosts-file"></a>Ajouter l’adresse IP d’un serveur de fédération au fichier hosts  
Afin qu’un serveur proxy de fédération puisse fonctionner comme prévu dans le réseau de périmètre d’un partenaire de compte, vous devez ajouter une entrée au fichier hosts sur ce serveur proxy de fédération qui pointe vers le nom d’hôte DNS d’un serveur de fédération \(, par exemple, fs.fabrikam.com \) et l’adresse IP \(, par exemple, 192.168.1.4\) du réseau d’entreprise du partenaire de compte. Ajout de cette entrée au fichier des hôtes empêche le serveur proxy de fédération de contacter lui-même pour résoudre un client\-initiée par appel à un serveur de fédération du partenaire de compte.  
  
Pour effectuer cette procédure, vous devez au minimum être membre du groupe **Administrateurs**ou d'un groupe équivalent sur l'ordinateur local.  Examinez les informations relatives à l’utilisation des comptes et des appartenances au groupe appropriés dans la rubrique [Groupes locaux et de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
#### <a name="to-add-the-ip-address-of-a-federation-server-to-the-hosts-file"></a>Pour ajouter l’adresse IP d’un serveur de fédération au fichier hosts  
  
1.  Accédez à la %SystemRoot%\\Winnt\\System32\\dossier pilotes et recherchez le **hôtes** fichier.  
  
2.  Démarrez le Bloc-notes, puis ouvrez le fichier **hosts**.  
  
3.  Ajouter l’adresse IP et le nom d’hôte d’un serveur de fédération du partenaire de compte pour le **hôtes** de fichiers, comme indiqué dans l’exemple suivant :  
  
    **192.168.1.4fs.fabrikam.com**  
  
4.  Enregistrez et fermez le fichier.  
  
## <a name="add-a-host-a-resource-record-to-perimeter-dns-for-a-federation-server-proxy"></a>Ajouter un hôte \(A\) enregistrement de ressource au DNS de périmètre pour un serveur proxy de fédération  
Afin que les clients sur Internet peuvent accéder avec succès à un serveur de fédération via un serveur proxy de fédération qui vient d’être déployé, vous devez d’abord créer un hôte \(A\) enregistrement de ressource dans le DNS de périmètre. Cet enregistrement de ressource résout le nom d’hôte du serveur de fédération de compte \(, par exemple, fs.fabrikam.com\) à l’adresse IP du serveur proxy de fédération de compte \(, par exemple, 131.107.27.68\) dans le réseau de périmètre.  
  
> [!NOTE]  
> Il est supposé que vous utilisez un serveur DNS, exécutant Windows 2000 Server, Windows Server 2003 ou Windows Server 2008 avec le service serveur DNS, pour contrôler la zone DNS de périmètre.  
  
Pour mener à bien cette procédure, il faut appartenir au groupe **Administrateurs** ou à un groupe équivalent.  Examinez les informations relatives à l’utilisation des comptes et des appartenances au groupe appropriés dans la rubrique [Groupes locaux et de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
#### <a name="to-add-a-host-a-resource-record-to-perimeter-dns-for-a-federation-server-proxy"></a>Pour ajouter un hôte \(A\) enregistrement de ressource au DNS de périmètre pour un serveur proxy de fédération  
  
1.  Sur un serveur DNS pour le réseau de périmètre, ouvrez le composant logiciel enfichable DNS\-dans. Cliquez sur **Démarrer**, pointez sur **outils d’administration**, puis cliquez sur **DNS**.  
  
2.  Dans l’arborescence de la console, avec le bouton droit\-cliquez sur la zone de recherche directe applicable, puis cliquez sur **nouvel hôte \(A ou AAAA\)** .  
  
3.  Dans **nom**, tapez uniquement le nom d’ordinateur du serveur de fédération. Par exemple, pour le nom de domaine pleinement qualifié \(FQDN\) fs.fabrikam.com, tapez **fs**.  
  
4.  Dans **adresse IP**, tapez l’adresse IP pour le nouveau serveur proxy de fédération, par exemple, **131.107.27.68**.  
  
5.  Cliquez sur **Ajouter un hôte**.  
  
## <a name="additional-references"></a>Références supplémentaires  
[Liste de vérification : configuration d’un serveur de fédération proxy](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  
[Exigences relatives à la résolution de noms pour les serveurs proxy de fédération](https://technet.microsoft.com/library/dd807055.aspx)  
  

