---
ms.assetid: c28c60ff-693d-49ee-a75b-58f24866217b
title: Exigences relatives à la résolution de noms pour les serveurs proxy de fédération
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 51176101b471ec940e2b43a95e1a1a8d37b394f3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408065"
---
# <a name="name-resolution-requirements-for-federation-server-proxies"></a>Exigences relatives à la résolution de noms pour les serveurs proxy de fédération

Lorsque des ordinateurs clients sur Internet tentent d’accéder à une application sécurisée par Services ADFS \(AD FS @ no__t-1, ils doivent d’abord s’authentifier auprès du serveur de Fédération. Dans la plupart des cas, le serveur de Fédération n’est généralement pas directement accessible à partir d’Internet. Par conséquent, les ordinateurs clients Internet doivent être redirigés vers le serveur proxy de Fédération à la place. Vous pouvez effectuer une redirection réussie en ajoutant les enregistrements Domain Name System \(DNS @ no__t-1 appropriés à votre zone DNS ou aux zones DNS qui font face à Internet.  
  
La méthode que vous utilisez pour rediriger les clients Internet vers le serveur proxy de Fédération dépend de la façon dont vous configurez la zone DNS dans votre réseau de périmètre ou de la façon dont vous configurez une zone DNS que vous contrôlez sur Internet. Les serveurs proxy de fédération sont destinés à être utilisés dans un réseau de périmètre. Ils redirigent les demandes des clients Internet vers les serveurs de Fédération avec succès uniquement lorsque DNS a été configuré correctement dans toutes les zones Internet @ no__t-0facing que vous contrôlez. Par conséquent, la configuration de vos zones Internet @ no__t-0facing, que vous disposiez d’une zone DNS desservant uniquement le réseau de périmètre ou d’une zone DNS desservant le réseau de périmètre et les clients Internet, est importante.  
  
Cette rubrique décrit les étapes que vous pouvez suivre pour configurer la résolution de noms lorsque vous placez un serveur proxy de Fédération dans votre réseau de périmètre. Pour déterminer les étapes à suivre, vous devez d’abord déterminer lequel des scénarios DNS suivants correspond le mieux à l’infrastructure DNS du réseau de périmètre de votre organisation. Ensuite, suivez les étapes de ce scénario.  
  
## <a name="dns-zone-serving-only-the-perimeter-network"></a>Zone DNS desservant uniquement le réseau de périmètre  
Dans ce scénario, votre organisation possède une ou deux zones DNS dans le réseau de périmètre et votre organisation ne contrôle aucune zone DNS sur Internet. La résolution de noms réussie pour un proxy de serveur de Fédération dans la zone DNS qui sert uniquement au scénario de réseau de périmètre dépend des conditions suivantes :  
  
-   Le serveur proxy de Fédération doit avoir un paramètre dans le fichier hosts pour résoudre le nom de domaine complet \(FQDN @ no__t-1 de l’URL du point de terminaison du serveur de Fédération en adresse IP d’un serveur de Fédération ou d’un cluster de serveurs de Fédération.  
  
-   Le DNS du réseau de périmètre du partenaire de compte doit être configuré de sorte que le nom de domaine complet de l’URL de point de terminaison du serveur de Fédération corresponde à l’adresse IP du serveur proxy de Fédération.  
  
L’illustration suivante et les étapes correspondantes montrent la réalisation de chacune de ces conditions pour un exemple donné. Dans cette illustration, la technologie d’équilibrage de charge réseau Microsoft \(NLB @ no__t-1 fournit un nom de domaine complet de cluster unique et une adresse IP de cluster unique pour une batterie de serveurs de fédération existante.  
  
![exigences relatives aux noms](media/adfs2_deploy_single_fs.gif)  
  
Pour plus d’informations sur la configuration d’une adresse IP de cluster ou d’un nom de domaine complet de cluster à l’aide de NLB, consultez [spécification des paramètres de cluster](https://go.microsoft.com/fwlink/?LinkId=75282).  
  
### <a name="1-configure-the-hosts-file-on-the-federation-server-proxy"></a>1. Configurer le fichier hôtes sur le serveur proxy de fédération  
Dans la mesure où le système DNS du réseau de périmètre est configuré pour résoudre toutes les demandes de fs.fabrikam.com sur le serveur proxy de Fédération de compte, le serveur proxy de Fédération du partenaire de compte a une entrée dans son fichier hosts local pour résoudre fs.fabrikam.com à l’adresse IP du nom réel du serveur de Fédération de compte @no__t 0or-nom DNS de cluster pour la batterie de serveurs de Fédération @ no__t-1 qui est connectée au réseau d’entreprise. Cela permet au serveur proxy de Fédération de compte de résoudre le nom d’hôte fs.fabrikam.com en serveur de Fédération de comptes plutôt qu’à lui-même, comme cela se produit s’il essayait de Rechercher fs.fabrikam.com à l’aide du DNS de périmètre, afin que la Fédération le serveur proxy peut communiquer avec le serveur de Fédération.  
  
### <a name="2-configure-perimeter-dns"></a>2. Configurer le DNS de périmètre  
Étant donné qu’il n’existe qu’un seul nom d’hôte AD FS auquel les ordinateurs clients sont dirigés, qu’ils se trouvent sur un intranet ou sur Internet, les ordinateurs clients sur Internet qui utilisent le serveur DNS de périmètre doivent résoudre le nom de domaine complet du serveur de Fédération de compte @no__t -0fs. fabrikam. Afin de pouvoir transférer les clients vers le serveur proxy de Fédération de compte lorsqu’ils tentent de résoudre fs.fabrikam.com, le DNS de périmètre contient une zone DNS corp.fabrikam.com limitée avec un seul hôte \(A @ no__t-1 enregistrement de ressource pour FS @no__t -2FS. fabrikam. com @ no__t-3 et l’adresse IP du serveur proxy de Fédération de compte sur le réseau de périmètre.  
  
Pour plus d’informations sur la façon de modifier le fichier hosts du serveur proxy de Fédération et de configurer le DNS dans le réseau de périmètre, consultez [configurer la résolution de noms pour un serveur proxy de Fédération dans une zone DNS qui dessert uniquement le réseau de périmètre](../../ad-fs/deployment/Configure-Name-Resolution-for-a-Federation-Server-Proxy-in-a-DNS-Zone-That-Serves-Only-the-Perimeter-Network.md).  
  
## <a name="dns-zone-serving-both-the-perimeter-network-and-internet-clients"></a>Zone DNS desservant le réseau de périmètre et les clients Internet  
Dans ce scénario, votre entreprise contrôle la zone DNS du réseau de périmètre et au moins une zone DNS sur Internet. La résolution de noms réussie pour un serveur proxy de Fédération dans ce scénario dépend des conditions suivantes :  
  
-   DNS dans la zone Internet du partenaire de compte doit être configuré de sorte que le nom de domaine complet du nom d’hôte du serveur de Fédération soit résolu en adresse IP du serveur proxy de Fédération dans le réseau de périmètre.  
  
-   Le DNS dans le réseau de périmètre du partenaire de compte doit être configuré de sorte que le nom de domaine complet du nom d’hôte du serveur de Fédération corresponde à l’adresse IP du serveur de Fédération du réseau d’entreprise.  
  
L’illustration suivante et les étapes correspondantes montrent la réalisation de chacune de ces conditions pour un exemple donné.  
  
![exigences relatives aux noms](media/adfs2_deploy_fsp_3DNS.gif)  
  
### <a name="1-configure-perimeter-dns"></a>1. Configurer le DNS de périmètre  
Pour ce scénario, il est supposé que vous allez configurer la zone DNS Internet que vous contrôlez pour résoudre les requêtes effectuées pour une URL de point de terminaison spécifique \(that est, FS. fabrikam. com @ no__t-1 au serveur proxy de Fédération dans le réseau de périmètre. vous devez également configurer la zone du DNS de périmètre pour transférer ces demandes vers le serveur de Fédération du réseau d’entreprise.  
  
Afin que les clients puissent être transférés vers le serveur de Fédération de compte lorsqu’ils tentent de résoudre fs.fabrikam.com, le DNS de périmètre est configuré avec un seul hôte \(A @ no__t-1 ressource record pour FS @no__t -2FS. fabrikam. com @ no__t-3 et l’adresse IP du serveur de Fédération de compte sur le réseau d’entreprise. Cela permet au serveur proxy de Fédération de compte de résoudre le nom d’hôte fs.fabrikam.com en serveur de Fédération de comptes plutôt qu’à lui-même, comme cela se produit s’il essayait de Rechercher fs.fabrikam.com à l’aide de DNS Internet, afin que le serveur de Fédération le proxy peut communiquer avec le serveur de Fédération.  
  
### <a name="2-configure-internet-dns"></a>2. Configurer le DNS Internet  
Pour réussir la résolution de noms dans ce scénario, toutes les demandes des ordinateurs clients sur Internet pour fs.fabrikam.com doivent être résolues par la zone DNS Internet que vous contrôlez. Par conséquent, vous devez configurer votre zone DNS Internet pour transférer les demandes des clients pour fs.fabrikam.com à l’adresse IP du serveur proxy de Fédération de compte dans le réseau de périmètre.  
  
Pour plus d’informations sur la façon de modifier le réseau de périmètre et les zones DNS Internet, consultez [configurer la résolution de noms pour un serveur proxy de Fédération dans une zone DNS qui dessert à la fois le réseau de périmètre et les clients Internet](../../ad-fs/deployment/Configure-Name-Resolution-for-a-Federation-Server-Proxy-in-a-DNS-Zone-That-Serves-Both-the-Perimeter-Network-and-Internet-Clients.md).  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
