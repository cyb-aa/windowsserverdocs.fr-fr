---
ms.assetid: c28c60ff-693d-49ee-a75b-58f24866217b
title: Exigences relatives à la résolution de noms pour les serveurs proxy de fédération
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: a94e4de181cd8794d479bbd6695a94658aba0f86
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855020"
---
# <a name="name-resolution-requirements-for-federation-server-proxies"></a>Exigences relatives à la résolution de noms pour les serveurs proxy de fédération

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Lorsque les ordinateurs clients sur Internet tentent d’accéder à une application sécurisée par Active Directory Federation Services \(AD FS\), ils doivent tout d’abord s’authentifier auprès du serveur de fédération. Dans la plupart des cas, le serveur de fédération n’est généralement pas directement accessible à partir d’Internet. Par conséquent, les ordinateurs clients Internet doivent être redirigés vers le serveur proxy de fédération à la place. Vous pouvez procéder à une redirection en ajoutant le système de nom de domaine approprié \(DNS\) enregistrements à votre zone DNS ou les zones accessibles sur Internet.  
  
La méthode qui vous permet de rediriger les clients Internet vers le serveur proxy de fédération dépend de la façon dont vous configurez la zone DNS dans votre réseau de périmètre ou d’une zone DNS que vous contrôlez sur Internet. Les serveurs proxy de fédération sont destinés à être utilisés dans un réseau de périmètre. Ils parviennent à rediriger les demandes de client Internet aux serveurs de fédération avec succès uniquement lorsque DNS a été configuré correctement dans tous les Internet\-accessible sur les zones que vous contrôlez. Par conséquent, la configuration de votre Internet\-accessible sur des zones, si vous disposez d’une zone DNS desservant uniquement le réseau de périmètre ou une zone DNS desservant le réseau de périmètre et les clients Internet, est important.  
  
Cette rubrique décrit les étapes que vous pouvez prendre pour configurer la résolution de noms lorsque vous placez un serveur proxy de fédération dans votre réseau de périmètre. Pour déterminer les étapes à suivre, vous devez d’abord déterminer lequel des scénarios DNS suivants correspond le mieux à l’infrastructure DNS du réseau de périmètre de votre organisation. Ensuite, suivez les étapes de ce scénario.  
  
## <a name="dns-zone-serving-only-the-perimeter-network"></a>Zone DNS desservant uniquement le réseau de périmètre  
Dans ce scénario, votre organisation possède une ou deux zones DNS dans le réseau de périmètre et votre organisation ne contrôle aucune zone DNS sur Internet. Résolution de noms réussie pour un serveur proxy de fédération dans la zone DNS qui sert uniquement le scénario de réseau de périmètre dépend des conditions suivantes :  
  
-   Le serveur proxy de fédération doit avoir un paramètre dans le fichier hosts pour résoudre le nom de domaine pleinement qualifié \(FQDN\) de l’URL de point de terminaison de serveur de fédération à une adresse IP d’un serveur de fédération ou d’un cluster de serveurs de fédération.  
  
-   DNS dans le réseau de périmètre du partenaire de compte doit être configuré afin que le nom de domaine complet de l’URL de point de terminaison de serveur de fédération corresponde à l’adresse IP du serveur proxy de fédération.  
  
L’illustration suivante et les étapes correspondantes montrent la réalisation de chacune de ces conditions pour un exemple donné. Dans cette illustration, l’équilibrage de charge réseau Microsoft \(NLB\) technologie fournit un seul nom de domaine complet de cluster et une seule adresse IP du cluster pour une batterie de serveurs de fédération existante.  
  
![exigences relatives aux noms](media/adfs2_deploy_single_fs.gif)  
  
Pour plus d’informations sur la configuration d’une adresse IP du cluster ou un nom de domaine complet du cluster à l’aide de NLB, consultez [en spécifiant les paramètres de Cluster](https://go.microsoft.com/fwlink/?LinkId=75282).  
  
### <a name="1-configure-the-hosts-file-on-the-federation-server-proxy"></a>1. Configurer le fichier hôtes sur le serveur proxy de fédération  
Étant donné que DNS dans le réseau de périmètre est configuré pour résoudre toutes les demandes de fs.fabrikam.com pour le serveur proxy de fédération de compte, le serveur proxy de fédération compte partenaire a une entrée dans son fichier hosts local pour résoudre fs.fabrikam.com à l’adresse IP de la serveur de fédération de compte réel \(ou le nom DNS de cluster pour la batterie de serveurs de fédération\) qui est connecté au réseau d’entreprise. Cela rend possible pour le serveur proxy de fédération du compte résoudre le nom d’hôte fs.fabrikam.com auprès du serveur de fédération de compte plutôt qu’en lui-même, comme se produirait si elle a tenté de rechercher fs.fabrikam.com à l’aide de DNS de périmètre, afin que la fédération serveur proxy peut communiquer avec le serveur de fédération.  
  
### <a name="2-configure-perimeter-dns"></a>2. Configurer le DNS de périmètre  
Car il est uniquement un seul AD FS nom d’hôte que les ordinateurs clients sont dirigés vers, s’ils sont sur un intranet ou sur Internet, les ordinateurs clients sur Internet qui utilisent le serveur DNS de périmètre doivent résoudre le nom de domaine complet pour le serveur de fédération de compte \(fs.fabrikam.com\) à l’adresse IP de compte serveur proxy de fédération sur le réseau de périmètre. Afin qu’il peut transférer les clients vers le serveur proxy de fédération de compte lorsqu’ils tentent de résoudre fs.fabrikam.com, le DNS de périmètre contient une zone DNS corp.fabrikam.com limitée avec un seul hôte \(A\) enregistrement de ressource pour fs \(fs.fabrikam.com\) et l’adresse IP de compte serveur proxy de fédération sur le réseau de périmètre.  
  
Pour plus d’informations sur comment modifier le fichier hosts de serveur proxy de fédération et de configurer des serveurs DNS dans le réseau de périmètre, consultez [configurer la résolution de noms pour un serveur Proxy de fédération dans une Zone DNS qui sert uniquement le réseau de périmètre](../../ad-fs/deployment/Configure-Name-Resolution-for-a-Federation-Server-Proxy-in-a-DNS-Zone-That-Serves-Only-the-Perimeter-Network.md).  
  
## <a name="dns-zone-serving-both-the-perimeter-network-and-internet-clients"></a>Zone DNS desservant le réseau de périmètre et les clients Internet  
Dans ce scénario, votre entreprise contrôle la zone DNS du réseau de périmètre et au moins une zone DNS sur Internet. Résolution de noms réussie pour un serveur proxy de fédération dans ce scénario dépend des conditions suivantes :  
  
-   DNS dans la zone Internet du partenaire de compte doit être configuré afin que le nom de domaine complet du nom d’hôte du serveur fédération corresponde à l’adresse IP du serveur proxy de fédération dans le réseau de périmètre.  
  
-   DNS dans le réseau de périmètre du partenaire de compte doit être configuré afin que le nom de domaine complet du nom d’hôte du serveur fédération corresponde à l’adresse IP du serveur de fédération dans le réseau d’entreprise.  
  
L’illustration suivante et les étapes correspondantes montrent la réalisation de chacune de ces conditions pour un exemple donné.  
  
![exigences relatives aux noms](media/adfs2_deploy_fsp_3DNS.gif)  
  
### <a name="1-configure-perimeter-dns"></a>1. Configurer le DNS de périmètre  
Pour ce scénario, car il est supposé que vous allez configurer la zone DNS Internet que vous contrôlez pour résoudre les requêtes effectuées pour une URL de point de terminaison spécifique \(fs.fabrikam.com, autrement dit,\) pour le serveur proxy de fédération dans le réseau de périmètre, vous devez également configurer la zone dans le périmètre du DNS pour transférer ces demandes au serveur de fédération dans le réseau d’entreprise.  
  
Afin que les clients peuvent être transférés vers le serveur de fédération de compte lorsqu’ils tentent de résoudre fs.fabrikam.com, le DNS de périmètre est configuré avec un seul hôte \(A\) enregistrement de ressource pour fs \(fs.fabrikam.com\) et l’adresse IP du serveur de fédération de compte sur le réseau d’entreprise. Cela rend possible pour le serveur proxy de fédération du compte résoudre le nom d’hôte fs.fabrikam.com auprès du serveur de fédération de compte plutôt qu’en lui-même, comme se produirait si elle a tenté de rechercher fs.fabrikam.com à l’aide de DNS Internet, afin que le serveur de fédération proxy peut communiquer avec le serveur de fédération.  
  
### <a name="2-configure-internet-dns"></a>2. Configurer le DNS Internet  
Pour réussir la résolution de noms dans ce scénario, toutes les demandes des ordinateurs clients sur Internet pour fs.fabrikam.com doivent être résolues par la zone DNS Internet que vous contrôlez. Par conséquent, vous devez configurer votre zone DNS Internet pour transférer les demandes des clients pour fs.fabrikam.com à l’adresse IP de compte serveur proxy de fédération dans le réseau de périmètre.  
  
Pour plus d’informations sur la façon de modifier le réseau de périmètre et les zones DNS Internet, consultez [configurer la résolution de noms pour un serveur Proxy de fédération dans un DNS Zone que sert à la fois le réseau de périmètre et les Clients Internet](../../ad-fs/deployment/Configure-Name-Resolution-for-a-Federation-Server-Proxy-in-a-DNS-Zone-That-Serves-Both-the-Perimeter-Network-and-Internet-Clients.md).  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
