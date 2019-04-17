---
ms.assetid: c28c60ff-693d-49ee-a75b-58f24866217b
title: "Exigences de résolution de nom de serveur proxy de fédération"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: a94e4de181cd8794d479bbd6695a94658aba0f86
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="name-resolution-requirements-for-federation-server-proxies"></a>Exigences de résolution de nom de serveur proxy de fédération

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Lorsque les ordinateurs clients sur Internet tentent d’accéder à une application sécurisée par Active Directory Federation Services \(AD FS\), ils doivent d’abord s’authentifier le serveur de fédération. Dans la plupart des cas, le serveur de fédération n’est généralement pas directement accessible à partir d’Internet. Par conséquent, les ordinateurs clients Internet doivent être redirigés vers le serveur proxy de fédération à la place. Vous pouvez procéder à une redirection en ajoutant les enregistrements de système de nom de domaine \(DNS\) appropriés pour votre zone DNS ou aux zones accessibles sur Internet.  
  
La méthode que vous utilisez pour rediriger les clients Internet vers le serveur proxy de fédération dépend de la façon dont vous configurez la zone DNS dans votre réseau de périmètre ou d’une zone DNS que vous contrôlez sur Internet. Serveurs proxy de fédération est conçues pour une utilisation dans un réseau de périmètre. Ils redirigent les demandes des clients Internet aux serveurs de fédération avec succès uniquement lorsque le DNS a été configuré correctement dans toutes les zones côté Internet que vous contrôlez. Par conséquent, la configuration de vos zones accessibles sur Internet, si vous disposez d’une zone DNS desservant uniquement le réseau de périmètre ou une zone DNS desservant le réseau de périmètre et les clients Internet, est important.  
  
Cette rubrique décrit les étapes que vous pouvez effectuer pour configurer la résolution de noms lorsque vous placez un serveur proxy de fédération dans votre réseau de périmètre. Pour déterminer les étapes à suivre, vous devez d’abord déterminer lequel des scénarios DNS suivants correspond le mieux à l’infrastructure DNS du réseau de périmètre de votre organisation. Ensuite, suivez les étapes de ce scénario.  
  
## <a name="dns-zone-serving-only-the-perimeter-network"></a>Zone DNS desservant uniquement le réseau de périmètre  
Dans ce scénario, votre organisation dispose d’une ou deux zones DNS dans le réseau de périmètre et votre organisation ne contrôle pas les zones DNS sur Internet. Résolution de noms correctement pour un serveur proxy de fédération dans la zone DNS qui sert uniquement le scénario de réseau de périmètre varie selon les conditions suivantes:  
  
-   Le serveur proxy de fédération doit posséder un paramètre dans le fichier hosts pour résoudre le nom de domaine complet nom \(FQDN\) de l’URL du serveur point de terminaison de fédération à une adresse IP d’un serveur de fédération ou d’un cluster de serveurs de fédération.  
  
-   DNS dans le réseau de périmètre du partenaire de compte doit être configuré afin que le nom de domaine complet de l’URL de point de terminaison de serveur de fédération correspond à l’adresse IP du serveur proxy de fédération.  
  
L’illustration suivante et les étapes correspondantes montrent comment chacun de ces conditions est obtenue par un exemple donné. Dans cette illustration, la technologie \(NLB\) l’équilibrage de charge réseau Microsoft fournit un seul, nom de domaine complet de cluster et un unique, l’adresse IP de cluster pour une batterie de serveurs de fédération existante.  
  
![exigences relatives au nom](media/adfs2_deploy_single_fs.gif)  
  
Pour plus d’informations sur la configuration d’une adresse IP de cluster ou un nom de domaine complet du cluster à l’aide de NLB, consultez [spécifiant les paramètres de Cluster](https://go.microsoft.com/fwlink/?LinkId=75282).  
  
### <a name="1-configure-the-hosts-file-on-the-federation-server-proxy"></a>1. configurer le fichier hosts sur le serveur proxy de fédération  
Étant donné que le DNS du réseau de périmètre est configuré pour résoudre toutes les demandes de fs.fabrikam.com pour le serveur proxy de fédération de compte, compte partenaire serveur proxy de fédération a une entrée dans son fichier hôtes local de résoudre fs.fabrikam.com à l’adresse IP du serveur de fédération de comptes réel \ (ou le nom DNS de cluster pour le farm\ de serveur de fédération) qui est connecté au réseau d’entreprise. Cela rend possible pour le serveur proxy de fédération du compte résoudre le nom d’hôte fs.fabrikam.com pour le serveur de fédération de comptes plutôt qu’en lui-même, comme se produirait si elle avait tenté de rechercher FS.fabrikam.com à l’aide de DNS de périmètre, afin que le serveur proxy de fédération peut communiquer avec le serveur de fédération.  
  
### <a name="2-configure-perimeter-dns"></a>2. configurer le DNS de périmètre  
Étant donné seulement un seul AD FS nom d’hôte que les ordinateurs clients sont dirigés vers: si elles sont sur un intranet ou sur Internet, les ordinateurs clients sur Internet qui utilisent le serveur DNS de périmètre doivent résoudre le nom de domaine complet pour le compte federation server \(fs.fabrikam.com\) à l’adresse IP du compte serveur proxy de fédération sur le réseau de périmètre. Afin qu’il peut transférer les clients une session sur le serveur proxy de fédération de comptes lorsqu’ils tentent de résoudre fs.fabrikam.com, le DNS de périmètre contient une zone DNS corp.fabrikam.com limitée avec un enregistrement de ressource hôte unique \(A\) pour \(fs.fabrikam.com\) fs et l’adresse IP du compte serveur proxy de fédération sur le réseau de périmètre.  
  
Pour plus d’informations sur comment modifier le fichier hôtes du serveur proxy de fédération et configurer le DNS dans le réseau de périmètre, voir [configurer la résolution de noms pour un serveur Proxy de fédération dans une Zone DNS qui sert uniquement le réseau de périmètre](../../ad-fs/deployment/Configure-Name-Resolution-for-a-Federation-Server-Proxy-in-a-DNS-Zone-That-Serves-Only-the-Perimeter-Network.md).  
  
## <a name="dns-zone-serving-both-the-perimeter-network-and-internet-clients"></a>Zone DNS desservant le réseau de périmètre et les clients Internet  
Dans ce scénario, votre organisation contrôle la zone DNS du réseau de périmètre et au moins une zone DNS sur Internet. Résolution de noms correctement pour un serveur proxy de fédération dans ce scénario dépend des conditions suivantes:  
  
-   DNS dans la zone Internet du partenaire de compte doit être configuré afin que le nom de domaine complet du nom d’hôte du serveur fédération correspond à l’adresse IP du serveur proxy de fédération dans le réseau de périmètre.  
  
-   DNS dans le réseau de périmètre du partenaire de compte doit être configuré afin que le nom de domaine complet du nom d’hôte du serveur fédération correspond à l’adresse IP du serveur de fédération du réseau d’entreprise.  
  
L’illustration suivante et les étapes correspondantes montrent comment chacun de ces conditions est obtenue par un exemple donné.  
  
![exigences relatives au nom](media/adfs2_deploy_fsp_3DNS.gif)  
  
### <a name="1-configure-perimeter-dns"></a>1. configurer le DNS de périmètre  
Pour ce scénario, car il est supposé que vous allez configurer la zone DNS Internet que vous contrôlez pour résoudre les requêtes effectuées pour une URL de point de terminaison spécifique \ (autrement dit, fs.fabrikam.com\) pour le serveur proxy de fédération dans le réseau de périmètre, vous devez également configurer la zone dans le périmètre DNS pour transférer ces demandes vers le serveur de fédération du réseau d’entreprise.  
  
Afin que les clients peuvent être transférés vers le serveur de fédération de comptes lorsqu’ils tentent de résoudre fs.fabrikam.com, DNS de périmètre est configuré avec un enregistrement de ressource hôte unique \(A\) pour \(fs.fabrikam.com\) fs et l’adresse IP du serveur de fédération de comptes sur le réseau d’entreprise. Cela rend possible pour le serveur proxy de fédération du compte résoudre le nom d’hôte fs.fabrikam.com pour le serveur de fédération de comptes plutôt qu’en lui-même, comme s’il avait tenté de rechercher FS.fabrikam.com à l’aide de DNS Internet se produit: afin que le serveur proxy de fédération peut communiquer avec le serveur de fédération.  
  
### <a name="2-configure-internet-dns"></a>2. configurer le DNS Internet  
La résolution de noms réussisse dans ce scénario, toutes les demandes des ordinateurs clients sur Internet pour fs.fabrikam.com doivent être résolues par la zone DNS Internet que vous contrôlez. Par conséquent, vous devez configurer votre zone DNS Internet pour transférer les demandes des clients pour fs.fabrikam.com à l’adresse IP du compte serveur proxy de fédération dans le réseau de périmètre.  
  
Pour plus d’informations sur la façon de modifier le réseau de périmètre et les zones DNS Internet, consultez [configurer la résolution de noms pour un serveur Proxy de fédération dans un DNS Zone que sert à la fois le réseau de périmètre et les Clients Internet](../../ad-fs/deployment/Configure-Name-Resolution-for-a-Federation-Server-Proxy-in-a-DNS-Zone-That-Serves-Both-the-Perimeter-Network-and-Internet-Clients.md).  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception ADFS dans Windows Server2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
