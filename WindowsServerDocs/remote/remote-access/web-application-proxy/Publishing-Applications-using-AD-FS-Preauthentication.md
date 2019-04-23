---
ms.assetid: 5f733510-c96e-4d3a-85d2-4407de95926e
title: Publication d’applications à l’aide de la préauthentification ADFS
description: ''
author: kgremban
manager: femila
ms.date: 07/13/2016
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: web-app-proxy
ms.openlocfilehash: c7dab1dbf97d2dcbda1fe0375e61300f2a1cc373
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862240"
---
# <a name="publishing-applications-using-ad-fs-preauthentication"></a>Publication d’applications à l’aide de la préauthentification ADFS

>S'applique à : Windows Server 2016

**Ce contenu est pertinent pour la version locale de Proxy d’Application Web. Pour activer un accès sécurisé aux applications locales sur le cloud, consultez le [contenu Azure Proxy d’Application AD](https://azure.microsoft.com/documentation/articles/active-directory-application-proxy-get-started/).**  
  
Cette rubrique décrit comment publier des applications via le Proxy d’Application Web à l’aide d’Active Directory Federation Services (ADFS) la pré-authentification.  
  
Pour tous les types d’application que vous pouvez publier à l’aide de la pré-authentification AD FS, vous devez ajouter un AD FS de confiance pour le Service de fédération.  
  
Le flux de pré-authentification AD FS est comme suit :  
  
> [!NOTE]  
> Ce flux d’authentification n’est pas applicable pour les clients qui utilisent des applications du Microsoft Store.  
  
1.  L’appareil client essaie d’accéder à une application web publiée sur une URL de ressource spécifique ; par exemple https://app1.contoso.com/.  
  
    L’URL de ressource est une adresse publique sur lequel le Proxy d’Application Web écoute les requêtes HTTPS entrantes.  
  
    Si la redirection HTTP vers HTTPS est activé, le Proxy d’Application Web écoute également les requêtes HTTP entrantes.  
  
2.  Proxy d’Application Web redirige la requête HTTPS au serveur AD FS avec les paramètres encodés en URL, y compris l’URL de ressource et l’appRealm (un identificateur de partie de confiance tiers).  
  
    L’utilisateur s’authentifie à l’aide de la méthode d’authentification requise par le serveur AD FS. par exemple, nom d’utilisateur et mot de passe, authentification à deux facteurs avec un mot de passe à usage unique et ainsi de suite.  
  
3.  Une fois que l’utilisateur est authentifié, le serveur AD FS émet une sécurité de jeton, le « jeton edge », qui contient les informations suivantes et redirige la demande de point de terminaison HTTPS sur le serveur de Proxy d’Application Web :  
  
    -   L’identificateur de ressource auquel l’utilisateur essaie d’accéder.  
  
    -   Identité de l’utilisateur en tant qu’un nom d’utilisateur principal (UPN).  
  
    -   L’expiration de l’approbation d’accès accordée (l’accès est accordé à l’utilisateur pour une période de temps limitée, ensuite l’utilisateur doit de nouveau s’authentifier).  
  
    -   La signature des informations dans le jeton edge.  
  
4.  Proxy d’Application Web reçoit la requête HTTPS redirigée à partir du serveur AD FS avec le jeton edge et valide et utilise le jeton comme suit :  
  
    -   Valide le fait que la signature du jeton edge est à partir du service de fédération est configuré dans la configuration de Proxy d’Application Web.  
  
    -   Valide que ce jeton a été émis par l’application correcte.  
  
    -   Valide que le jeton n’a pas expiré.  
  
    -   Utilise l’identité de l’utilisateur quand cela est nécessaire, par exemple pour obtenir un ticket Kerberos si le serveur principal est configuré pour utiliser l’authentification Windows intégrée.  
  
5.  Si le jeton edge est valide, le Proxy d’Application Web transfère la requête HTTPS à l’application web publiée à l’aide de HTTP ou HTTPS.  
  
6.  Le client a désormais accès à l’application Web publiée, cependant, l’application publiée peut être configurée pour exiger de l’utilisateur une authentification supplémentaire. Par exemple, si l’application Web publiée est un site SharePoint et ne nécessite pas d’authentification supplémentaire, l’utilisateur pourra afficher le site SharePoint dans un navigateur.  
  
7.  Proxy d’Application Web enregistre un cookie sur l’appareil client. Le cookie est utilisé par le Proxy d’Application Web pour identifier que cette session a déjà été pré-authentifiée et qu’aucune autre pré-authentification n’est nécessaire.  
  
> [!IMPORTANT]  
> Lors de la configuration de l’URL externe et de l’URL du serveur principal, vous devez entrer un nom de domaine complet (FQDN), et non une adresse IP.  
  
> [!NOTE]  
> Cette rubrique inclut des exemples d'applets de commande Windows PowerShell que vous pouvez utiliser pour automatiser certaines des procédures décrites. Pour plus d’informations, consultez [Utilisation des applets de commande](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="BKMK_1.1"></a>Publier une Application basée sur les revendications pour les Clients de navigateur Web  
Pour publier une application qui utilise des revendications pour l’authentification, vous devez ajouter une approbation de partie de confiance pour l’application au service de fédération.  
  
Lors de la publication d’applications basées sur les revendications et de l’accès à l’application à partir d’un navigateur, le flux d’authentification général est le suivant :  
  
1.  Le client tente d’accéder à une application basée sur les revendications à l’aide d’un navigateur web ; par exemple, https://appserver.contoso.com/claimapp/.  
  
2.  Le navigateur web envoie une requête HTTPS au serveur Proxy d’Application Web qui redirige la demande vers le serveur AD FS.  
  
3.  Le serveur AD FS authentifie l’utilisateur et l’appareil et redirige la demande vers le Proxy d’Application Web. La requête contient désormais le jeton edge. Le serveur AD FS ajoute un cookie d’authentification unique (SSO) à la demande, car l’utilisateur a déjà effectué l’authentification sur le serveur AD FS.  
  
4.  Proxy d’Application Web valide le jeton, ajoute son propre cookie et transfère la requête au serveur principal.  
  
5.  Le serveur principal redirige la demande vers le serveur AD FS pour obtenir un jeton la sécurité de l’application.  
  
6.  La demande est redirigée vers le serveur principal par le serveur AD FS. La requête contient désormais le jeton d’application et le cookie SSO. L’accès à l’application est accordé à l’utilisateur et ce dernier n’a pas besoin d’entrer un nom d’utilisateur ou un mot de passe.  
  
Cette procédure décrit comment publier une application basée sur les revendications, par exemple un site SharePoint, qui sera accessible aux clients de navigateur Web. Avant de commencer, assurez-vous d’avoir effectué les étapes suivantes :  
  
-   Créé une approbation de partie de confiance pour l’application dans la console de gestion AD FS.  
  
-   Vérifié qu’un certificat sur le serveur de Proxy d’Application Web est approprié pour l’application que vous souhaitez publier.  
  

  
#### <a name="to-publish-a-claims-based-application"></a>Pour publier une application basée sur les revendications  
  
1.  Sur le serveur de Proxy d’Application Web, dans la console de gestion de l’accès à distance, dans le **Navigation** volet, cliquez sur **Proxy d’Application Web**, puis, dans le **tâches** volet, cliquez sur **Publier**.  
  
2.  Dans l’ **Assistant Publication d’une nouvelle application**, dans la page **Bienvenue** , cliquez sur **Suivant**.  
  
3.  Sur le **la pré-authentification** , cliquez sur **Active Directory Federation Services (ADFS)**, puis cliquez sur **suivant**.  
  
4.  Sur la page **Clients pris en charge**, sélectionnez **Web et MSOFBA**, puis cliquez sur **Suivant**.  
  
5.  Dans la page **Partie de confiance**, dans la liste des parties de confiance, sélectionnez la partie de confiance pour l’application que vous voulez publier, puis cliquez sur **Suivant**.  
  
6.  Dans la page **Paramètres de publication** , procédez comme suit et cliquez sur **Suivant**:  
  
    -   Dans la zone **Nom** , tapez un nom convivial pour l’application.  
  
        Ce nom est utilisé uniquement dans la liste des applications publiées dans la console de gestion de l’accès à distance.  
  
    -   Dans la zone **URL externe**, entrez l’URL externe pour cette application, par exemple, https://sp.contoso.com/app1/.  
  
    -   Dans la liste **Certificat externe**, sélectionnez un certificat dont l’objet couvre l’URL externe.  
  
    -   Dans la zone **URL du serveur principal**, entrez l’URL du serveur principal. Notez que cette valeur est entrée automatiquement lorsque vous entrez l’URL externe et vous devez le modifier uniquement si l’URL du serveur principal est différente ; par exemple, https://sp/app1/.  
  
        > [!NOTE]  
        > Proxy d’Application Web peut traduire les noms d’hôte en URL, mais ne peut pas traduire les noms de chemin d’accès. C’est pourquoi, vous pouvez entrer différents noms d’hôte, mais vous devez entrer le même nom de chemin. Par exemple, vous pouvez entrer une URL externe https://apps.contoso.com/app1/ et une URL de serveur principal https://app-server/app1/. Toutefois, vous ne pouvez pas entrer une URL externe https://apps.contoso.com/app1/ et une URL de serveur principal https://apps.contoso.com/internal-app1/.  
  
7.  Dans la page **Confirmation**, passez les paramètres en revue, puis cliquez sur **Publier**. Vous pouvez copier la commande PowerShell pour l’installation d’applications publiées supplémentaires.  
  
8.  Dans la page **Résultats** , vérifiez que l’application a été correctement publiée, puis cliquez sur **Fermer**.  
  
![](../../media/Publishing-Applications-using-AD-FS-Preauthentication/PowerShellLogoSmall.gif)Windows PowerShell équivalente commandes ***  
  
L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.  
  
```  
Add-WebApplicationProxyApplication  
    -BackendServerURL 'https://sp.contoso.com/app1/'  
    -ExternalCertificateThumbprint '1a2b3c4d5e6f1a2b3c4d5e6f1a2b3c4d5e6f1a2b'  
    -ExternalURL 'https://sp.contoso.com/app1/'  
    -Name 'SP'  
    -ExternalPreAuthentication ADFS  
    -ADFSRelyingPartyName 'SP_Relying_Party'  
```  
  
## <a name="BKMK_1.2"></a>Publier une Application de base authentifié Windows intégrée pour les Clients de navigateur Web  
Proxy d’Application Web peut être utilisé pour publier des applications qui utilisent l’authentification Windows intégrée ; Autrement dit, le Proxy d’Application Web effectue la pré-authentification requise et peut effectuer l’authentification unique sur l’application publiée qui utilise l’authentification Windows intégrée. Pour publier une application qui utilise l’authentification Windows intégrée, vous devez ajouter une approbation de partie de confiance sans revendication pour l’application au service de fédération.  
  
Pour autoriser le Proxy d’Application Web pour effectuer l’authentification unique (SSO) et pour effectuer la délégation des informations d’identification à l’aide de Kerberos la délégation contrainte, le Proxy d’Application Web serveur doit être joint à un domaine. Consultez [planifier Active Directory](https://technet.microsoft.com/library/dn383648.aspx#BKMK_AD).  
  
Pour permettre aux utilisateurs d’accéder aux applications qui utilisent l’authentification Windows intégrée, le serveur de Proxy d’Application Web doit être en mesure de fournir la délégation pour les utilisateurs sur l’application publiée. Vous pouvez effectuer cette opération sur le contrôleur de domaine de n’importe quelle application. Vous pouvez également procéder sur le serveur principal s’il s’exécute sur Windows Server 2012 R2 ou Windows Server 2012. Pour plus d’informations, voir [Nouveautés de l’authentification Kerberos](https://technet.microsoft.com/library/hh831747.aspx).  
  
Pour une procédure pas à pas montrant comment configurer le Proxy d’Application Web pour publier une application à l’aide de l’authentification Windows intégrée, consultez [configurer un site pour utiliser l’authentification Windows intégrée](https://technet.microsoft.com/library/dn280943.aspx#BKMK_3).  
  
Lorsque vous utilisez l’authentification Windows intégrée sur les serveurs principaux, l’authentification entre le Proxy d’Application Web et l’application publiée n’est pas basée sur les revendications, au lieu de cela, il utilise la délégation contrainte Kerberos pour authentifier les utilisateurs finaux à l’application. Le flux général est décrit ci-dessous :  
  
1.  Le client tente d’accéder à une application non basée sur des revendications à l’aide d’un navigateur web ; par exemple, https://appserver.contoso.com/nonclaimapp/.  
  
2.  Le navigateur web envoie une requête HTTPS au serveur Proxy d’Application Web qui redirige la demande vers le serveur AD FS.  
  
3.  Le serveur AD FS authentifie l’utilisateur et redirige la demande vers le Proxy d’Application Web. La requête contient désormais le jeton edge.  
  
4.  Proxy d’Application Web valide le jeton.  
  
5.  Si le jeton est valid, le Proxy d’Application Web obtient un ticket Kerberos à partir du contrôleur de domaine pour le compte de l’utilisateur.  
  
6.  Proxy d’Application Web ajoute le ticket Kerberos à la demande dans le cadre du jeton SPNEGO Simple and Protected GSS-API négociation mécanisme () et transfère la requête au serveur principal. La requête contient le ticket Kerberos donc l’utilisateur a accès à l’application et n’a pas besoin de s’authentifier de nouveau.  
  
Cette procédure décrit comment publier une application qui utilise l’authentification Windows intégrée, par exemple Outlook Web App, qui sera accessible aux clients de navigateur Web. Avant de commencer, assurez-vous d’avoir effectué les étapes suivantes :  
  
-   Créé une non-claims-aware confiance pour l’application dans la console de gestion AD FS.  
  
-   Configuré le serveur principal pour prendre en charge la délégation contrainte Kerberos sur le contrôleur de domaine ou en utilisant l’applet de commande Set-ADUser avec le paramètre -PrincipalsAllowedToDelegateToAccount. Notez que si le serveur principal s’exécute sur Windows Server 2012 R2 ou Windows Server 2012, vous pouvez également exécuter cette commande PowerShell sur le serveur principal.  
  
-   Fait en sorte que les serveurs Proxy d’Application Web sont configurés pour la délégation pour les noms de principal du service des serveurs principaux.  
  
-   Vérifié qu’un certificat sur le serveur de Proxy d’Application Web est approprié pour l’application que vous souhaitez publier.  
  
 
  
#### <a name="to-publish-a-non-claims-based-application"></a>Pour publier une application qui n’est pas basée sur les revendications  
  
1.  Sur le serveur de Proxy d’Application Web, dans la console de gestion de l’accès à distance, dans le **Navigation** volet, cliquez sur **Proxy d’Application Web**, puis, dans le **tâches** volet, cliquez sur **Publier**.  
  
2.  Dans l’ **Assistant Publication d’une nouvelle application**, dans la page **Bienvenue** , cliquez sur **Suivant**.  
  
3.  Sur le **la pré-authentification** , cliquez sur **Active Directory Federation Services (ADFS)**, puis cliquez sur **suivant**.  
  
4.  Sur la page **Clients pris en charge**, sélectionnez **Web et MSOFBA**, puis cliquez sur **Suivant**.  
  
5.  Dans la page **Partie de confiance**, dans la liste des parties de confiance, sélectionnez la partie de confiance pour l’application que vous voulez publier, puis cliquez sur **Suivant**.  
  
6.  Dans la page **Paramètres de publication** , procédez comme suit et cliquez sur **Suivant**:  
  
    -   Dans la zone **Nom** , tapez un nom convivial pour l’application.  
  
        Ce nom est utilisé uniquement dans la liste des applications publiées dans la console de gestion de l’accès à distance.  
  
    -   Dans la zone **URL externe**, entrez l’URL externe pour cette application, par exemple, https://owa.contoso.com/.  
  
    -   Dans la liste **Certificat externe**, sélectionnez un certificat dont l’objet couvre l’URL externe.  
  
    -   Dans la zone **URL du serveur principal**, entrez l’URL du serveur principal. Notez que cette valeur est entrée automatiquement lorsque vous entrez l’URL externe et vous devez le modifier uniquement si l’URL du serveur principal est différente ; par exemple, https://owa/.  
  
        > [!NOTE]  
        > Proxy d’Application Web peut traduire les noms d’hôte en URL, mais ne peut pas traduire les noms de chemin d’accès. C’est pourquoi, vous pouvez entrer différents noms d’hôte, mais vous devez entrer le même nom de chemin. Par exemple, vous pouvez entrer une URL externe https://apps.contoso.com/app1/ et une URL de serveur principal https://app-server/app1/. Toutefois, vous ne pouvez pas entrer une URL externe https://apps.contoso.com/app1/ et une URL de serveur principal https://apps.contoso.com/internal-app1/.  
  
    -   Dans la zone **SPN du serveur principal**, entrez le nom de principal du service pour le serveur principal, par exemple, HTTP/owa.contoso.com.  
  
7.  Dans la page **Confirmation**, passez les paramètres en revue, puis cliquez sur **Publier**. Vous pouvez copier la commande PowerShell pour l’installation d’applications publiées supplémentaires.  
  
8.  Dans la page **Résultats** , vérifiez que l’application a été correctement publiée, puis cliquez sur **Fermer**.  
  
![](../../media/Publishing-Applications-using-AD-FS-Preauthentication/PowerShellLogoSmall.gif)Windows PowerShell équivalente commandes ***  
  
L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.  
  
```  
Add-WebApplicationProxyApplication  
    -BackendServerAuthenticationSpn 'HTTP/owa.contoso.com'  
    -BackendServerURL 'https://owa.contoso.com/'  
    -ExternalCertificateThumbprint '1a2b3c4d5e6f1a2b3c4d5e6f1a2b3c4d5e6f1a2b'  
    -ExternalURL 'https://owa.contoso.com/'  
    -Name 'OWA'  
    -ExternalPreAuthentication ADFS  
    -ADFSRelyingPartyName 'Non-Claims_Relying_Party'  
```  
  
## <a name="BKMK_1.3"></a>Publier une Application qui utilise MS-OFBA  
Proxy d’Application Web prend en charge l’accès à partir de clients Microsoft Office telle que Microsoft Word qui accéder aux documents et les données sur les serveurs principaux. La seule différence entre ces applications et un navigateur standard est que la redirection vers STS s’effectue pas via une redirection HTTP mais avec des en-têtes de MS-OFBA spéciaux comme indiqué dans : [ https://msdn.microsoft.com/library/dd773463(v=office.12).aspx ](https://msdn.microsoft.com/library/dd773463(v=office.12).aspx). L’application principale peut être basée sur les revendications ou IWA.   
Pour publier une application pour les clients qui utilisent MS-OFBA, vous devez ajouter une partie de confiance pour l’application pour le Service de fédération. En fonction de l’application, vous pouvez utiliser l’authentification basée sur les revendications ou l’authentification Windows intégrée. C’est pourquoi, vous devez ajouter l’approbation de partie de confiance appropriée à l’application.  
  
Pour autoriser le Proxy d’Application Web pour effectuer l’authentification unique (SSO) et pour effectuer la délégation des informations d’identification à l’aide de Kerberos la délégation contrainte, le Proxy d’Application Web serveur doit être joint à un domaine. Consultez [planifier Active Directory](https://technet.microsoft.com/library/dn383648.aspx#BKMK_AD).  
  
Aucune étape de planification supplémentaire n'est nécessaire si l'application utilise l'authentification basée sur les revendications. Si l’application utilise l’authentification Windows intégrée, consultez [publier une Application de base authentifié Windows intégrée pour les Clients de navigateur Web](../web-application-proxy/../web-application-proxy/Publishing-Applications-using-AD-FS-Preauthentication.md#BKMK_1.2).  
  
Le flux d’authentification pour les clients qui utilisent le protocole MS-OFBA à l’aide de l’authentification basée sur les revendications est décrit ci-dessous. L’authentification pour ce scénario peut utiliser le jeton d’application dans l’URL ou dans le corps.  
  
1.  L’utilisateur travaille dans un programme Office, et à partir de la liste **Documents récents**, ouvre un fichier sur un site SharePoint.  
  
2.  Le programme Office affiche une fenêtre avec un contrôle de navigateur qui nécessite la saisie d’informations d’identification.  
  
    > [!NOTE]  
    > Dans certains cas, la fenêtre peut ne pas s’afficher car le client est déjà authentifié.  
  
3.  Proxy d’Application Web redirige la demande vers le serveur AD FS, qui effectue l’authentification.  
  
4.  Le serveur AD FS redirige la demande au Proxy d’Application Web. La requête contient désormais le jeton edge.  
  
5.  Le serveur AD FS ajoute un cookie d’authentification unique (SSO) à la demande, car l’utilisateur a déjà effectué l’authentification sur le serveur AD FS.  
  
6.  Proxy d’Application Web valide le jeton et transfère la requête au serveur principal.  
  
7.  Le serveur principal redirige la demande vers le serveur AD FS pour obtenir un jeton la sécurité de l’application.  
  
8.  La requête est redirigée vers le serveur principal. La requête contient désormais le jeton d’application et le cookie SSO. L’accès au site SharePoint est accordé à l’utilisateur et ce dernier n’a pas besoin d’entrer un nom d’utilisateur ou un mot de passe pour afficher le fichier.  
  
Les étapes pour publier une application qui utilise MS-OFBA sont identiques aux étapes pour une application basée sur les revendications ou d’une application non basée sur des revendications. Pour les applications basées sur les revendications, consultez [publier une Application basée sur les revendications pour les Clients de navigateur Web](../web-application-proxy/../web-application-proxy/Publishing-Applications-using-AD-FS-Preauthentication.md#BKMK_1.1), pour les applications non basées sur des revendications, consultez [publier une Application basée sur authentifié Windows intégré pour le Web Les Clients de navigateur](../web-application-proxy/../web-application-proxy/Publishing-Applications-using-AD-FS-Preauthentication.md#BKMK_1.2). Proxy d’Application Web détecte automatiquement le client et authentifiera l’utilisateur en fonction des besoins.  
  
## <a name="publish-an-application-that-uses-http-basic"></a>Publier une Application qui utilise HTTP de base  

HTTP de base est le protocole d’autorisation utilisé par de nombreux protocoles, pour connecter des clients riches, y compris les smartphones, avec votre boîte aux lettres Exchange. Pour plus d’informations sur HTTP de base, consultez [RFC 2617](https://www.ietf.org/rfc/rfc2617.txt). Proxy d’Application Web traditionnellement interagit avec AD FS à l’aide de redirections ; la plupart des clients riches ne prennent pas en charge les cookies ou gestion de l’état. De cette façon le Proxy d’Application Web permet à l’application HTTP recevoir un revendications de confiance pour l’application pour le Service de fédération. Consultez [planifier Active Directory](https://technet.microsoft.com/library/dn383648.aspx#BKMK_AD).  
  
Le flux d’authentification pour les clients qui utilisent HTTP de base est décrite ci-dessous et dans ce diagramme :  
  
![](../../media/Publishing-Applications-using-AD-FS-Preauthentication/WebApplicationProxy_httpBasicflow.png)  
  
1.  L’utilisateur tente d’accéder à une application web publiée un client de téléphone.  
  
2.  L’application envoie une requête HTTPS à l’URL publiée par le Proxy d’Application Web.  
  
3.  Si la demande ne contient pas les informations d’identification, le Proxy d’Application Web retourne une réponse HTTP 401 à l’application contenant l’URL de l’authentification serveur AD FS.  
  
4.  L’utilisateur envoie la requête HTTPS à l’application à nouveau avec l’autorisation définie sur Basic et nom d’utilisateur et de la Base 64 chiffrée mot de passe de l’utilisateur dans le www-en-tête de demande d’authentification.  
  
5.  Étant donné que l’appareil ne peut pas être redirigé vers AD FS, le Proxy d’Application Web envoie une demande d’authentification pour AD FS avec les informations d’identification qu’il a notamment le nom d’utilisateur et mot de passe. Le jeton est acquis pour le compte de l’appareil.  
  
6.  Afin de réduire le nombre de demandes envoyées à AD FS, le Proxy d’Application Web, valide les demandes suivantes du client à l’aide de jetons mis en cache pour tant que le jeton est valide. Proxy d’Application Web nettoie régulièrement le cache. Vous pouvez afficher la taille du cache à l’aide du compteur de performances.  
  
7.  Si le jeton est valid, le Proxy d’Application Web transfère la requête au serveur principal et l’utilisateur a accès à l’application web publiée.  
  
La procédure suivante explique comment publier des applications de base HTTP.  
  
#### <a name="to-publish-an-http-basic-application"></a>Pour publier une application de base HTTP  
  
1.  Sur le serveur de Proxy d’Application Web, dans la console de gestion de l’accès à distance, dans le **Navigation** volet, cliquez sur **Proxy d’Application Web**, puis, dans le **tâches** volet, cliquez sur **Publier**.  
  
2.  Dans l’ **Assistant Publication d’une nouvelle application**, dans la page **Bienvenue** , cliquez sur **Suivant**.  
  
3.  Sur le **la pré-authentification** , cliquez sur **Active Directory Federation Services (ADFS)**, puis cliquez sur **suivant**.  
  
4.  Sur le **Clients pris en charge** page, sélectionnez **HTTP Basic** puis cliquez sur **suivant**.  
  
    Si vous souhaitez autoriser l’accès à l’échange uniquement à partir d’appareils joints à un espace de travail, sélectionnez le **appareils joints à activer l’accès uniquement pour l’espace de travail** boîte. Pour plus d’informations, consultez [rejoindre un espace de travail à partir de n’importe quel appareil pour l’authentification unique et transparente deuxième facteur Authentication Across Company Applications](https://technet.microsoft.com/library/dn280945.aspx).  
  
5.  Dans la page **Partie de confiance**, dans la liste des parties de confiance, sélectionnez la partie de confiance pour l’application que vous voulez publier, puis cliquez sur **Suivant**. Notez que cette liste contient uniquement les parties de confiance sur les revendications  
  
6.  Dans la page **Paramètres de publication** , procédez comme suit et cliquez sur **Suivant**:  
  
    -   Dans la zone **Nom** , tapez un nom convivial pour l’application.  
  
        Ce nom est utilisé uniquement dans la liste des applications publiées dans la console de gestion de l’accès à distance.  
  
    -   Dans le **URL externe** , entrez l’URL externe pour cette application, par exemple, mail.contoso.com  
  
    -   Dans la liste **Certificat externe**, sélectionnez un certificat dont l’objet couvre l’URL externe.  
  
    -   Dans la zone **URL du serveur principal**, entrez l’URL du serveur principal. Notez que cette valeur est entrée automatiquement lorsque vous entrez l’URL externe et vous devez le modifier uniquement si l’URL du serveur principal est différente ; par exemple, mail.contoso.com.  
  
7.  Dans la page **Confirmation**, passez les paramètres en revue, puis cliquez sur **Publier**. Vous pouvez copier la commande PowerShell pour l’installation d’applications publiées supplémentaires.  
  
8.  Dans la page **Résultats** , vérifiez que l’application a été correctement publiée, puis cliquez sur **Fermer**.  
  
![](../../media/Publishing-Applications-using-AD-FS-Preauthentication/PowerShellLogoSmall.gif)Windows PowerShell équivalente commandes ***  
  
L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.  
  
Ce script Windows PowerShell permet la pré-authentification pour tous les appareils, pas seulement les appareils joints à un espace de travail.  
  
```  
Add-WebApplicationProxyApplication  
     -BackendServerUrl 'https://mail.contoso.com'   
     -ExternalCertificateThumbprint '697F4FF0B9947BB8203A96ED05A3021830638E50'   
     -ExternalUrl 'https://mail.contoso.com'   
     -Name 'Exchange'   
     -ExternalPreAuthentication ADFSforRichClients  
     -ADFSRelyingPartyName 'EAS_Relying_Party'  
```  
  
Les éléments suivants préauthentifie uniquement les appareils joints à l’espace de travail :  
  
```  
Add-WebApplicationProxyApplication  
     -BackendServerUrl 'https://mail.contoso.com'   
     -ExternalCertificateThumbprint '697F4FF0B9947BB8203A96ED05A3021830638E50'   
     -EnableHTTPRedirect:$true   
     -ExternalUrl 'https://mail.contoso.com'   
     -Name 'Exchange'   
     -ExternalPreAuthentication ADFSforRichClients  
     -ADFSRelyingPartyName 'EAS_Relying_Party'  
```  
  
## <a name="BKMK_1.4"></a>Publier une Application qui utilise OAuth2, par exemple une application de Microsoft Store  
Pour publier une application pour les applications Microsoft Store, vous devez ajouter une partie de confiance pour l’application pour le Service de fédération.  
  
Pour autoriser le Proxy d’Application Web pour effectuer l’authentification unique (SSO) et pour effectuer la délégation des informations d’identification à l’aide de Kerberos la délégation contrainte, le Proxy d’Application Web serveur doit être joint à un domaine. Consultez [planifier Active Directory](https://technet.microsoft.com/library/dn383648.aspx#BKMK_AD).  
  
> [!NOTE]  
> Proxy d’Application Web prend en charge la publication uniquement pour les applications Microsoft Store qui utilisent le protocole OAuth 2.0.  
  
Dans la console de gestion AD FS, il se peut que vous devez vous assurer que le point de terminaison OAuth est activé pour le proxy. Pour vérifier si le point de terminaison OAuth est activé pour le proxy, ouvrez la console de gestion AD FS, développez **Service**, cliquez sur **Points de terminaison**, dans la liste **Points de terminaison** , recherchez le point de terminaison OAuth et vérifiez que la valeur dans la colonne **Proxy activé** est **Oui**.  
  
Le flux d’authentification pour les clients qui utilisent des applications du Microsoft Store est décrit ci-dessous :  
  
> [!NOTE]  
> Le Proxy d’Application Web redirige vers le serveur AD FS pour l’authentification. Étant donné que les applications Microsoft Store ne gèrent pas les redirections, si vous utilisez les applications Microsoft Store, il est nécessaire de définir l’URL du serveur AD FS à l’aide de l’applet de commande Set-WebApplicationProxyConfiguration et du paramètre OAuthAuthenticationURL.  
>   
> Les applications Microsoft Store peuvent être publiées uniquement à l’aide de Windows PowerShell.  
  
1.  Le client tente d’accéder à une application web publiée à l’aide d’une application Microsoft Store.  
  
2.  L’application envoie une requête HTTPS à l’URL publiée par le Proxy d’Application Web.  
  
3.  Proxy d’Application Web retourne une réponse HTTP 401 à l’application contenant l’URL de l’authentification serveur AD FS. Ce processus est appelé « découverte ».  
  
    > [!NOTE]  
    > Si l’application connaît l’URL du serveur AD FS authentification et dispose déjà d’un jeton combo contenant le jeton OAuth et le jeton edge, les étapes 2 et 3 sont ignorées dans ce flux d’authentification.  
  
4.  L’application envoie une requête HTTPS au serveur AD FS.  
  
5.  L’application utilise le service broker d’authentification web pour générer une boîte de dialogue dans laquelle l’utilisateur entre des informations d’identification pour s’authentifier sur le serveur AD FS. Pour plus d’informations sur le service Broker d’authentification web, voir [Service Broker d’authentification web](https://msdn.microsoft.com/library/windows/apps/hh750287.aspx).  
  
6.  Après une authentification réussie, le serveur AD FS crée un jeton combo qui contient le jeton OAuth et le jeton edge et envoie le jeton à l’application.  
  
7.  L’application envoie une requête HTTPS contenant le jeton combo à l’URL publiée par le Proxy d’Application Web.  
  
8.  Proxy d’Application Web divise le jeton combo en deux parties et valide le jeton edge.  
  
9. Si le jeton edge est valide, le Proxy d’Application Web transfère la requête au serveur principal avec uniquement le jeton OAuth. L’utilisateur a accès à l’application Web publiée.  
  
Cette procédure décrit comment publier une application pour OAuth2. Ce type d’application peut être publié uniquement à l’aide de Windows PowerShell. Avant de commencer, assurez-vous d’avoir effectué les étapes suivantes :  
  
-   Créé une approbation de partie de confiance pour l’application dans la console de gestion AD FS.  
  
-   Fait en sorte que le point de terminaison OAuth est activé dans la console de gestion AD FS pour le proxy et prenez note du chemin d’accès d’URL.  
  
-   Vérifié qu’un certificat sur le serveur de Proxy d’Application Web est approprié pour l’application que vous souhaitez publier.  
  
#### <a name="to-publish-an-oauth2-app"></a>Pour publier une application OAuth2  
  
1.  Sur le serveur de Proxy d’Application Web, dans la console de gestion de l’accès à distance, dans le **Navigation** volet, cliquez sur **Proxy d’Application Web**, puis, dans le **tâches** volet, cliquez sur **Publier**.  
  
2.  Dans l’ **Assistant Publication d’une nouvelle application**, dans la page **Bienvenue** , cliquez sur **Suivant**.  
  
3.  Sur le **la pré-authentification** , cliquez sur **Active Directory Federation Services (ADFS)**, puis cliquez sur **suivant**.  
  
4.  Sur le **Clients pris en charge** page, sélectionnez **OAuth2** puis cliquez sur **suivant**.  
  
5.  Dans la page **Partie de confiance**, dans la liste des parties de confiance, sélectionnez la partie de confiance pour l’application que vous voulez publier, puis cliquez sur **Suivant**.  
  
6.  Dans la page **Paramètres de publication** , procédez comme suit et cliquez sur **Suivant**:  
  
    -   Dans la zone **Nom** , tapez un nom convivial pour l’application.  
  
        Ce nom est utilisé uniquement dans la liste des applications publiées dans la console de gestion de l’accès à distance.  
  
    -   Dans la zone **URL externe**, entrez l’URL externe pour cette application, par exemple, https://server1.contoso.com/app1/.  
  
    -   Dans la liste **Certificat externe**, sélectionnez un certificat dont l’objet couvre l’URL externe.  
  
        Pour vous assurer de vos utilisateurs peuvent accéder à votre application, même si elles négligent de taper l’URL HTTPS, sélectionnez le **activer la Redirection HTTP vers HTTPS** boîte.  
  
    -   Dans la zone **URL du serveur principal**, entrez l’URL du serveur principal. Notez que cette valeur est entrée automatiquement lorsque vous entrez l’URL externe et vous devez le modifier uniquement si l’URL du serveur principal est différente ; par exemple, https://sp/app1/.  
  
        > [!NOTE]  
        > Proxy d’Application Web peut traduire les noms d’hôte en URL, mais ne peut pas traduire les noms de chemin d’accès. C’est pourquoi, vous pouvez entrer différents noms d’hôte, mais vous devez entrer le même nom de chemin. Par exemple, vous pouvez entrer une URL externe https://apps.contoso.com/app1/ et une URL de serveur principal https://app-server/app1/. Toutefois, vous ne pouvez pas entrer une URL externe https://apps.contoso.com/app1/ et une URL de serveur principal https://apps.contoso.com/internal-app1/.  
  
7.  Dans la page **Confirmation**, passez les paramètres en revue, puis cliquez sur **Publier**. Vous pouvez copier la commande PowerShell pour l’installation d’applications publiées supplémentaires.  
  
8.  Dans la page **Résultats** , vérifiez que l’application a été correctement publiée, puis cliquez sur **Fermer**.  
  
Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.  
  
Pour définir l’URL d’authentification OAuth pour un serveur de fédération, adresse de fs.contoso.com et un chemin d’URL de/ADFS/oauth2 / :  
  
```  
Set-WebApplicationProxyConfiguration -OAuthAuthenticationURL 'https://fs.contoso.com/adfs/oauth2/'  
```  
  
Pour publier l’application :  
  
```  
Add-WebApplicationProxyApplication  
    -BackendServerURL 'https://storeapp.contoso.com/'  
    -ExternalCertificateThumbprint '1a2b3c4d5e6f1a2b3c4d5e6f1a2b3c4d5e6f1a2b'  
    -ExternalURL 'https://storeapp.contoso.com/'  
    -Name 'Microsoft Store app Server'  
    -ExternalPreAuthentication ADFS  
    -ADFSRelyingPartyName 'Store_app_Relying_Party'  
    -UseOAuthAuthentication  
```  
  
## <a name="BKMK_Links"></a>Voir aussi  
  
-   [Résolution des problèmes de Proxy d’Application Web](https://technet.microsoft.com/library/dn770156.aspx)  
  
-   [Publier des Applications via le Proxy d’Application Web](https://technet.microsoft.com/library/dn383659.aspx)  
  
-   [Envisagez de publier des Applications à l’aide du Proxy d’Application Web](https://technet.microsoft.com/library/dn383650.aspx)  
  
-   [Guide de procédure pas à pas de Web Application Proxy](https://technet.microsoft.com/library/dn280944.aspx)  
  
-   [Cmdlets de Proxy d’Application Web dans Windows PowerShell](https://technet.microsoft.com/library/dn283404.aspx)  
  
-   [Add-WebApplicationProxyApplication](https://technet.microsoft.com/library/dn283409.aspx)  
  
-   [Set-WebApplicationProxyConfiguration](https://technet.microsoft.com/library/dn283406.aspx)  
  


