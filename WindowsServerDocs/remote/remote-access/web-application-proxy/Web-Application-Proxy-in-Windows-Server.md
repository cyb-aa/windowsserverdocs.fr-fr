---
ms.assetid: 0b3587b2-219f-43d8-88b4-1254eaa8b910
title: Proxy d'application web dans Windows Server
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.service: ''
ms.suite: na
ms.technology: web-app-proxy
ms.tgt_pltfrm: na
ms.topic: article
author: kgremban
ms.openlocfilehash: bfa57a18ee74e1e54f6e7c1ed85d4bfbccb8937b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404250"
---
# <a name="web-application-proxy-in-windows-server"></a>Proxy d'application web dans Windows Server

>S’applique à Windows Server&reg; 2016

**Ce contenu est pertinent pour la version locale du proxy d’application Web. Pour activer l’accès sécurisé aux applications locales sur le Cloud, consultez le [Azure ad le contenu du proxy d’application](https://azure.microsoft.com/documentation/articles/active-directory-application-proxy-get-started/).**  
  
Le contenu de cette section décrit les nouveautés et les modifications apportées au proxy d’application Web pour Windows Server 2016. Les nouvelles fonctionnalités et modifications répertoriées ici sont celles qui ont le plus de chances d’avoir l’impact le plus important lorsque vous utilisez la version préliminaire.  
  
## <a name="web-application-proxy-new-features"></a>Nouvelles fonctionnalités du proxy d’application Web  
  
- Pré-authentification pour la publication d’applications de base HTTP  
  
  HTTP Basic est le protocole d’autorisation utilisé par de nombreux protocoles, notamment ActiveSync, pour connecter des clients riches, y compris des smartphones, à votre boîte aux lettres Exchange. Le proxy d’application Web interagit traditionnellement avec AD FS à l’aide de redirections qui ne sont pas prises en charge sur les clients ActiveSync. Cette nouvelle version du proxy d’application Web prend en charge la publication d’une application à l’aide de HTTP Basic en permettant à l’application HTTP de recevoir une approbation de partie de confiance non basée sur les revendications pour l’application à l’service FS (Federation Service).  
  
  Pour plus d’informations sur la publication HTTP de base, consultez [publication d’applications à l’aide de AD FS la pré-authentification](../web-application-proxy/../web-application-proxy/Publishing-Applications-using-AD-FS-Preauthentication.md)  
  
- Publication de domaine générique d’applications  
  
  Pour prendre en charge des scénarios tels que SharePoint 2013, l’URL externe de l’application peut désormais inclure un caractère générique pour vous permettre de publier plusieurs applications à partir d’un domaine spécifique, par exemple, https://*. SP-apps. contoso. com. Cela simplifiera la publication des applications SharePoint.  
  
- Redirection HTTP vers HTTPs  
  
  Afin de s’assurer que vos utilisateurs peuvent accéder à votre application, même s’ils négligent de taper HTTPs dans l’URL, le proxy d’application Web prend désormais en charge la redirection HTTP vers HTTPs.  
  
- Publication HTTP  
  
  Il est désormais possible de publier des applications HTTP à l’aide de la pré-authentification pass-through  
  
- Publication des applications de passerelle Bureau à distance  
  
  Pour plus d’informations sur RDG dans le proxy d’application Web, consultez [publication d’applications avec SharePoint, Exchange et RDG](../web-application-proxy/Publishing-Applications-with-SharePoint,-Exchange-and-RDG.md)  
  
- Nouveau journal de débogage pour une meilleure résolution des problèmes et un journal de service amélioré pour une piste d’audit complète et une meilleure gestion des erreurs  
  
  Pour plus d’informations sur la résolution des problèmes, consultez [résolution des problèmes de proxy d’application Web](https://technet.microsoft.com/library/dn770156.aspx)  
  
- Améliorations de l’interface utilisateur Console Administrateur  
  
- Propagation de l’adresse IP du client aux applications principales  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Nouveautés de Windows Server 2016](https://technet.microsoft.com/library/dn765472.aspx)  
  
-   [Publication d’applications à l’aide de la préauthentification AD FS](../web-application-proxy/Publishing-Applications-using-AD-FS-Preauthentication.md)  
  
-   [Résolution des problèmes au niveau du proxy d’application web](https://technet.microsoft.com/library/dn770156.aspx)  
  


