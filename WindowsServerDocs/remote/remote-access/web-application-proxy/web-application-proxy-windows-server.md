---
ms.assetid: d8adcb68-18e0-41bf-a817-d57344bf2e7d
title: Proxy d’application web dans Windows Server 2016
description: ''
author: kgremban
manager: femila
ms.date: 07/13/2016
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: web-app-proxy
ms.openlocfilehash: 2f24e1b8605503d338b15f385017bbeacce682fb
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872210"
---
# <a name="web-application-proxy-in-windows-server-2016"></a>Proxy d’application web dans Windows Server 2016

>S'applique à : Windows Server 2016

**Ce contenu est pertinent pour la version locale de Proxy d’Application Web. Pour activer un accès sécurisé aux applications locales sur le cloud, consultez le [contenu Azure Proxy d’Application AD](https://azure.microsoft.com/documentation/articles/active-directory-application-proxy-get-started/).**  
  
Le contenu de cette section décrit les nouveautés et les modifications dans le Proxy d’Application Web pour Windows Server 2016. Les nouvelles fonctionnalités et les modifications répertoriées ici sont celles susceptibles d’avoir l’impact le plus important lorsque vous travaillez avec la version préliminaire.  
  
## <a name="web-application-proxy-new-features-in-windows-server-2016"></a>Web Application Proxy nouvelles fonctionnalités dans Windows Server 2016
  
-   Pré-authentification pour la publication d’applications HTTP de base  
  
    HTTP de base est le protocole d’autorisation utilisé par de nombreux protocoles, y compris d’ActiveSync, pour connecter des clients riches, y compris les smartphones, avec votre boîte aux lettres Exchange. Traditionnellement, le Proxy d’Application Web interagit avec AD FS à l’aide de redirections qui n’est pas pris en charge sur les clients ActiveSync. Cette nouvelle version Web du Proxy d’Application prend en charge pour publier une application à l’aide de HTTP de base en activant l’application HTTP recevoir un revendications de confiance pour l’application pour le Service de fédération.  
  
    Pour plus d’informations sur la publication de base HTTP, consultez [publication d’Applications à l’aide de la pré-authentification AD FS](Publishing-Applications-using-AD-FS-Preauthentication.md#publish-an-application-that-uses-http-basic)  
  
-   Publication de domaine avec caractère générique d’applications  
  
    Pour prendre en charge des scénarios tels que SharePoint 2013, l’URL externe pour l’application peut maintenant inclure un caractère générique pour vous permettre de publier plusieurs applications depuis un domaine spécifique, par exemple, https://*.sp-apps.contoso.com. Cela simplifiera la publication des applications SharePoint.  
  
-   Redirection HTTP vers HTTPS  
  
    Pour vous assurer de vos utilisateurs peuvent accéder à votre application, même si elles négligent de taper l’URL HTTPS, le Proxy d’Application Web prend désormais en charge HTTP à la redirection de HTTPS.  
  
-   Publication HTTP  
  
    Il est désormais possible de publier des applications HTTP à l’aide de la pré-authentification pass-through  
  
-   Publication d’applications de passerelle Bureau à distance  
  
    Pour plus d’informations sur la passerelle Bureau à distance dans le Proxy d’Application Web, consultez [publication d’Applications avec SharePoint, Exchange et passerelle Bureau à distance](../web-application-proxy/Publishing-Applications-with-SharePoint,-Exchange-and-RDG.md)  
  
-   Nouveau journal de débogage pour la résolution des problèmes mieux et journal de service amélioré pour la piste d’audit complète et la gestion des erreurs améliorée  
  
    Pour plus d’informations sur la résolution des problèmes, consultez [dépannage de Proxy d’Application Web](https://technet.microsoft.com/library/dn770156.aspx)  
  
-   Améliorations de l’interface utilisateur de la Console Administrateur  
  
-   Propagation de l’adresse IP du client pour les applications principales  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Quelles sont les nouveautés dans Windows Server 2016](https://technet.microsoft.com/library/dn765472.aspx)  
  
-   [Publication d’Applications à l’aide de la pré-authentification AD FS](../web-application-proxy/Publishing-Applications-using-AD-FS-Preauthentication.md)  
  
-   [Résolution des problèmes de Proxy d’Application Web](https://technet.microsoft.com/library/dn770156.aspx)  
  


