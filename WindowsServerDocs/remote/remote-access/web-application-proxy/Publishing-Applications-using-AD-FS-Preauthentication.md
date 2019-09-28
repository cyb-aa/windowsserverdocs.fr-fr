---
ms.assetid: 5f733510-c96e-4d3a-85d2-4407de95926e
title: Publication d’applications à l’aide de la préauthentification ADFS
description: ''
author: kgremban
manager: femila
ms.date: 07/13/2016
ms.topic: article
ms.prod: windows-server
ms.technology: web-app-proxy
ms.openlocfilehash: bd5c4c97e01942e7c5ab8ed1aba3fcf92030ac59
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404266"
---
# <a name="publishing-applications-using-ad-fs-preauthentication"></a>Publication d’applications à l’aide de la préauthentification ADFS

>S'applique à : Windows Server 2016

le contenu **This est pertinent pour la version locale du proxy d’application Web. Pour activer l’accès sécurisé aux applications locales sur le Cloud, consultez le [Azure ad le contenu du proxy d’application](https://azure.microsoft.com/documentation/articles/active-directory-application-proxy-get-started/).**  
  
Cette rubrique explique comment publier des applications via le proxy d’application Web à l’aide de la pré-authentification Services ADFS (AD FS).  
  
Pour tous les types d’applications que vous pouvez publier à l’aide de la pré-authentification AD FS, vous devez ajouter une approbation de partie de confiance AD FS à la service FS (Federation Service).  
  
Le processus général de pré-authentification AD FS est le suivant :  
  
> [!NOTE]  
> Ce processus d’authentification n’est pas applicable pour les clients qui utilisent des applications Microsoft Store.  
  
1.  L’appareil client tente d’accéder à une application Web publiée sur une URL de ressource particulière. par exemple https://app1.contoso.com/.  
  
    L’URL de ressource est une adresse publique sur laquelle le proxy d’application Web écoute les requêtes HTTPs entrantes.  
  
    Si la redirection HTTP vers HTTPs est activée, le proxy d’application Web écoute également les requêtes HTTP entrantes.  
  
2.  Le proxy d’application web redirige la requête HTTPs vers le serveur AD FS avec les paramètres encodés d’URL, y compris l’URL de ressource et le appRealm (un identificateur de partie de confiance).  
  
    L’utilisateur s’authentifie à l’aide de la méthode d’authentification requise par le serveur AD FS ; par exemple, le nom d’utilisateur et le mot de passe, l’authentification à deux facteurs avec un mot de passe à usage unique, et ainsi de suite.  
  
3.  Une fois l’utilisateur authentifié, le serveur AD FS émet un jeton de sécurité, le « jeton Edge », qui contient les informations suivantes et redirige la requête HTTPs vers le serveur proxy d’application Web :  
  
    -   L’identificateur de ressource auquel l’utilisateur essaie d’accéder.  
  
    -   Identité de l’utilisateur sous la forme d’un nom d’utilisateur principal (UPN).  
  
    -   L’expiration de l’approbation d’accès accordée (l’accès est accordé à l’utilisateur pour une période de temps limitée, ensuite l’utilisateur doit de nouveau s’authentifier).  
  
    -   La signature des informations dans le jeton edge.  
  
4.  Le proxy d’application Web reçoit la requête HTTPs redirigée à partir du serveur AD FS avec le jeton Edge et valide et utilise le jeton comme suit :  
  
    -   Valide que la signature du jeton de périphérie provient du service de Fédération qui est configuré dans la configuration du proxy d’application Web.  
  
    -   Valide que ce jeton a été émis par l’application correcte.  
  
    -   Valide que le jeton n’a pas expiré.  
  
    -   Utilise l’identité de l’utilisateur quand cela est nécessaire, par exemple pour obtenir un ticket Kerberos si le serveur principal est configuré pour utiliser l’authentification Windows intégrée.  
  
5.  Si le jeton Edge est valide, le proxy d’application Web transfère la requête HTTPs à l’application Web publiée à l’aide de HTTP ou HTTPs.  
  
6.  Le client a désormais accès à l’application Web publiée, cependant, l’application publiée peut être configurée pour exiger de l’utilisateur une authentification supplémentaire. Par exemple, si l’application Web publiée est un site SharePoint et ne nécessite pas d’authentification supplémentaire, l’utilisateur pourra afficher le site SharePoint dans un navigateur.  
  
7.  Le proxy d’application Web enregistre un cookie sur l’appareil client. Le cookie est utilisé par le proxy d’application Web pour identifier que cette session a déjà été pré-authentifiée et qu’aucune autre pré-authentification n’est nécessaire.  
  
> [!IMPORTANT]  
> Lors de la configuration de l’URL externe et de l’URL du serveur principal, vous devez entrer un nom de domaine complet (FQDN), et non une adresse IP.  
  
> [!NOTE]  
> Cette rubrique inclut des exemples d'applets de commande Windows PowerShell que vous pouvez utiliser pour automatiser certaines des procédures décrites. Pour plus d’informations, consultez [Utilisation des applets de commande](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="BKMK_1.1"></a>Publier une application basée sur les revendications pour les clients de navigateur Web  
Pour publier une application qui utilise des revendications pour l’authentification, vous devez ajouter une approbation de partie de confiance pour l’application au service de fédération.  
  
Lors de la publication d’applications basées sur les revendications et de l’accès à l’application à partir d’un navigateur, le flux d’authentification général est le suivant :  
  
1.  Le client tente d’accéder à une application basée sur les revendications à l’aide d’un navigateur Web. par exemple, https://appserver.contoso.com/claimapp/.  
  
2.  Le navigateur Web envoie une requête HTTPs au serveur proxy d’application Web qui redirige la demande vers le serveur de AD FS.  
  
3.  Le serveur AD FS authentifie l’utilisateur et l’appareil et redirige la demande vers le proxy d’application Web. La requête contient désormais le jeton edge. Le serveur AD FS ajoute un cookie d’authentification unique (SSO) à la demande, car l’utilisateur a déjà effectué l’authentification auprès du serveur de AD FS.  
  
4.  Le proxy d’application Web valide le jeton, ajoute son propre cookie et transfère la requête au serveur principal.  
  
5.  Le serveur principal redirige la requête vers le serveur AD FS pour obtenir le jeton de sécurité de l’application.  
  
6.  La demande est redirigée vers le serveur principal par le serveur de AD FS. La requête contient désormais le jeton d’application et le cookie SSO. L’accès à l’application est accordé à l’utilisateur et ce dernier n’a pas besoin d’entrer un nom d’utilisateur ou un mot de passe.  
  
Cette procédure décrit comment publier une application basée sur les revendications, par exemple un site SharePoint, qui sera accessible aux clients de navigateur Web. Avant de commencer, assurez-vous d’avoir effectué les étapes suivantes :  
  
-   Création d’une approbation de partie de confiance pour l’application dans la console de gestion AD FS.  
  
-   Vérifié qu’un certificat sur le serveur proxy d’application Web est approprié pour l’application que vous souhaitez publier.  
  

  
#### <a name="to-publish-a-claims-based-application"></a>Pour publier une application basée sur les revendications  
  
1.  Sur le serveur proxy d’application Web, dans la console Gestion de l’accès à distance, dans le volet de **navigation** , cliquez sur **proxy d’application Web**, puis, dans le volet **tâches** , cliquez sur **publier**.  
  
2.  Dans l’ **Assistant Publication d’une nouvelle application**, dans la page **Bienvenue** , cliquez sur **Suivant**.  
  
3.  Dans la page **pré-authentification** , cliquez sur **services ADFS (AD FS)** , puis cliquez sur **suivant**.  
  
4.  Sur la page **Clients pris en charge**, sélectionnez **Web et MSOFBA**, puis cliquez sur **Suivant**.  
  
5.  Dans la page **Partie de confiance**, dans la liste des parties de confiance, sélectionnez la partie de confiance pour l’application que vous voulez publier, puis cliquez sur **Suivant**.  
  
6.  Dans la page **Paramètres de publication** , procédez comme suit et cliquez sur **Suivant**:  
  
    -   Dans la zone **Nom** , tapez un nom convivial pour l’application.  
  
        Ce nom est utilisé uniquement dans la liste des applications publiées dans la console de gestion de l’accès à distance.  
  
    -   Dans la zone **URL externe**, entrez l’URL externe pour cette application, par exemple, https://sp.contoso.com/app1/.  
  
    -   Dans la liste **Certificat externe**, sélectionnez un certificat dont l’objet couvre l’URL externe.  
  
    -   Dans la zone **URL du serveur principal**, entrez l’URL du serveur principal. Notez que cette valeur est entrée automatiquement quand vous entrez l’URL externe et vous ne devez la modifier que si l’URL du serveur principal est différente ; par exemple, https://sp/app1/.  
  
        > [!NOTE]  
        > Le proxy d’application Web peut traduire les noms d’hôte dans des URL, mais ne peut pas traduire les noms de chemin d’accès. C’est pourquoi, vous pouvez entrer différents noms d’hôte, mais vous devez entrer le même nom de chemin. Par exemple, vous pouvez entrer une URL externe de https://apps.contoso.com/app1/ et une URL de serveur principal de https://app-server/app1/. Toutefois, vous ne pouvez pas entrer une URL externe de https://apps.contoso.com/app1/ et une URL de serveur principal de https://apps.contoso.com/internal-app1/.  
  
7.  Dans la page **Confirmation**, passez les paramètres en revue, puis cliquez sur **Publier**. Vous pouvez copier la commande PowerShell pour l’installation d’applications publiées supplémentaires.  
  
8.  Dans la page **Résultats** , vérifiez que l’application a été correctement publiée, puis cliquez sur **Fermer**.  
  
![](../../media/Publishing-Applications-using-AD-FS-Preauthentication/PowerShellLogoSmall.gif)***<em>commandes Windows PowerShell équivalentes</em>***  
  
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
  
## <a name="BKMK_1.2"></a>Publier une application basée sur authentification Windows intégrée pour les clients de navigateur Web  
Le proxy d’application Web peut être utilisé pour publier des applications qui utilisent l’authentification Windows intégrée. autrement dit, le proxy d’application Web effectue la pré-authentification en fonction des besoins et peut ensuite effectuer l’authentification unique sur l’application publiée qui utilise l’authentification Windows intégrée. Pour publier une application qui utilise l’authentification Windows intégrée, vous devez ajouter une approbation de partie de confiance sans revendication pour l’application au service de fédération.  
  
Pour autoriser le proxy d’application Web à effectuer une authentification unique (SSO) et à effectuer la délégation des informations d’identification à l’aide de la délégation Kerberos Kerberos, le serveur proxy d’application Web doit être joint à un domaine. Voir [Active Directory de plan](https://technet.microsoft.com/library/dn383648.aspx#BKMK_AD).  
  
Pour permettre aux utilisateurs d’accéder aux applications qui utilisent l’authentification Windows intégrée, le serveur proxy d’application Web doit être en mesure de fournir la délégation pour les utilisateurs à l’application publiée. Vous pouvez effectuer cette opération sur le contrôleur de domaine de n’importe quelle application. Vous pouvez également effectuer cette opération sur le serveur principal s’il s’exécute sur Windows Server 2012 R2 ou Windows Server 2012. Pour plus d’informations, voir [Nouveautés de l’authentification Kerberos](https://technet.microsoft.com/library/hh831747.aspx).  
  
Pour obtenir une procédure pas à pas de configuration du proxy d’application Web pour publier une application à l’aide de l’authentification Windows intégrée, consultez [configurer un site pour utiliser l’authentification Windows intégrée](https://technet.microsoft.com/library/dn280943.aspx#BKMK_3).  
  
Lors de l’utilisation de l’authentification Windows intégrée sur les serveurs principaux, l’authentification entre le proxy d’application Web et l’application publiée n’est pas basée sur les revendications, au lieu de cela, elle utilise la délégation Kerberos confrontée pour authentifier les utilisateurs finaux sur l’application. Le flux général est décrit ci-dessous :  
  
1.  Le client tente d’accéder à une application qui n’est pas basée sur les revendications à l’aide d’un navigateur Web. par exemple, https://appserver.contoso.com/nonclaimapp/.  
  
2.  Le navigateur Web envoie une requête HTTPs au serveur proxy d’application Web qui redirige la demande vers le serveur de AD FS.  
  
3.  Le serveur AD FS authentifie l’utilisateur et redirige la demande vers le proxy d’application Web. La requête contient désormais le jeton edge.  
  
4.  Le proxy d’application Web valide le jeton.  
  
5.  Si le jeton est valide, le proxy d’application Web obtient un ticket Kerberos du contrôleur de domaine pour le compte de l’utilisateur.  
  
6.  Le proxy d’application Web ajoute le ticket Kerberos à la requête dans le cadre du jeton SPNEGO (simple and Protected GSS-API Negotiation Mechanism) et transfère la requête au serveur principal. La requête contient le ticket Kerberos donc l’utilisateur a accès à l’application et n’a pas besoin de s’authentifier de nouveau.  
  
Cette procédure décrit comment publier une application qui utilise l’authentification Windows intégrée, par exemple Outlook Web App, qui sera accessible aux clients de navigateur Web. Avant de commencer, assurez-vous d’avoir effectué les étapes suivantes :  
  
-   Création d’une approbation de partie de confiance ne prenant pas en charge les revendications pour l’application dans la console de gestion AD FS.  
  
-   Configuré le serveur principal pour prendre en charge la délégation contrainte Kerberos sur le contrôleur de domaine ou en utilisant l’applet de commande Set-ADUser avec le paramètre -PrincipalsAllowedToDelegateToAccount. Notez que si le serveur principal s’exécute sur Windows Server 2012 R2 ou Windows Server 2012, vous pouvez également exécuter cette commande PowerShell sur le serveur principal.  
  
-   Vérifiez que les serveurs proxy d’application Web sont configurés pour la délégation aux noms de principal de service des serveurs principaux.  
  
-   Vérifié qu’un certificat sur le serveur proxy d’application Web est approprié pour l’application que vous souhaitez publier.  
  
 
  
#### <a name="to-publish-a-non-claims-based-application"></a>Pour publier une application qui n’est pas basée sur les revendications  
  
1.  Sur le serveur proxy d’application Web, dans la console Gestion de l’accès à distance, dans le volet de **navigation** , cliquez sur **proxy d’application Web**, puis, dans le volet **tâches** , cliquez sur **publier**.  
  
2.  Dans l’ **Assistant Publication d’une nouvelle application**, dans la page **Bienvenue** , cliquez sur **Suivant**.  
  
3.  Dans la page **pré-authentification** , cliquez sur **services ADFS (AD FS)** , puis cliquez sur **suivant**.  
  
4.  Sur la page **Clients pris en charge**, sélectionnez **Web et MSOFBA**, puis cliquez sur **Suivant**.  
  
5.  Dans la page **Partie de confiance**, dans la liste des parties de confiance, sélectionnez la partie de confiance pour l’application que vous voulez publier, puis cliquez sur **Suivant**.  
  
6.  Dans la page **Paramètres de publication** , procédez comme suit et cliquez sur **Suivant**:  
  
    -   Dans la zone **Nom** , tapez un nom convivial pour l’application.  
  
        Ce nom est utilisé uniquement dans la liste des applications publiées dans la console de gestion de l’accès à distance.  
  
    -   Dans la zone **URL externe**, entrez l’URL externe pour cette application, par exemple, https://owa.contoso.com/.  
  
    -   Dans la liste **Certificat externe**, sélectionnez un certificat dont l’objet couvre l’URL externe.  
  
    -   Dans la zone **URL du serveur principal**, entrez l’URL du serveur principal. Notez que cette valeur est entrée automatiquement quand vous entrez l’URL externe et vous ne devez la modifier que si l’URL du serveur principal est différente ; par exemple, https://owa/.  
  
        > [!NOTE]  
        > Le proxy d’application Web peut traduire les noms d’hôte dans des URL, mais ne peut pas traduire les noms de chemin d’accès. C’est pourquoi, vous pouvez entrer différents noms d’hôte, mais vous devez entrer le même nom de chemin. Par exemple, vous pouvez entrer une URL externe de https://apps.contoso.com/app1/ et une URL de serveur principal de https://app-server/app1/. Toutefois, vous ne pouvez pas entrer une URL externe de https://apps.contoso.com/app1/ et une URL de serveur principal de https://apps.contoso.com/internal-app1/.  
  
    -   Dans la zone **SPN du serveur principal**, entrez le nom de principal du service pour le serveur principal, par exemple, HTTP/owa.contoso.com.  
  
7.  Dans la page **Confirmation**, passez les paramètres en revue, puis cliquez sur **Publier**. Vous pouvez copier la commande PowerShell pour l’installation d’applications publiées supplémentaires.  
  
8.  Dans la page **Résultats** , vérifiez que l’application a été correctement publiée, puis cliquez sur **Fermer**.  
  
![](../../media/Publishing-Applications-using-AD-FS-Preauthentication/PowerShellLogoSmall.gif)***<em>commandes Windows PowerShell équivalentes</em>***  
  
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
  
## <a name="BKMK_1.3"></a>Publier une application qui utilise MS-OFBA  
Le proxy d’application Web prend en charge l’accès à partir de Microsoft Office clients tels que Microsoft Word qui accèdent aux documents et aux données sur les serveurs principaux. La seule différence entre ces applications et un navigateur standard est que la redirection vers le STS n’est pas effectuée via une redirection HTTP normale, mais avec des en-têtes MS-OFBA spéciaux comme indiqué dans : [https://msdn.microsoft.com/library/dd773463(v=office.12).aspx](https://msdn.microsoft.com/library/dd773463(v=office.12).aspx). L’application principale peut être basée sur les revendications ou IWA.   
Pour publier une application pour les clients qui utilisent MS-OFBA, vous devez ajouter une approbation de partie de confiance pour l’application au service FS (Federation Service). En fonction de l’application, vous pouvez utiliser l’authentification basée sur les revendications ou l’authentification Windows intégrée. C’est pourquoi, vous devez ajouter l’approbation de partie de confiance appropriée à l’application.  
  
Pour autoriser le proxy d’application Web à effectuer une authentification unique (SSO) et à effectuer la délégation des informations d’identification à l’aide de la délégation Kerberos Kerberos, le serveur proxy d’application Web doit être joint à un domaine. Voir [Active Directory de plan](https://technet.microsoft.com/library/dn383648.aspx#BKMK_AD).  
  
Aucune étape de planification supplémentaire n'est nécessaire si l'application utilise l'authentification basée sur les revendications. Si l’application utilisait l’authentification Windows intégrée, consultez [publier une application intégrée basée sur authentification Windows pour les clients de navigateur Web](../web-application-proxy/../web-application-proxy/Publishing-Applications-using-AD-FS-Preauthentication.md#BKMK_1.2).  
  
Le processus d’authentification pour les clients qui utilisent le protocole MS-OFBA à l’aide de l’authentification basée sur les revendications est décrit ci-dessous. L’authentification pour ce scénario peut utiliser le jeton d’application dans l’URL ou dans le corps.  
  
1.  L’utilisateur travaille dans un programme Office, et à partir de la liste **Documents récents**, ouvre un fichier sur un site SharePoint.  
  
2.  Le programme Office affiche une fenêtre avec un contrôle de navigateur qui nécessite la saisie d’informations d’identification.  
  
    > [!NOTE]  
    > Dans certains cas, la fenêtre peut ne pas s’afficher car le client est déjà authentifié.  
  
3.  Le proxy d’application web redirige la requête vers le serveur AD FS, qui effectue l’authentification.  
  
4.  Le serveur AD FS redirige la demande vers le proxy d’application Web. La requête contient désormais le jeton edge.  
  
5.  Le serveur AD FS ajoute un cookie d’authentification unique (SSO) à la demande, car l’utilisateur a déjà effectué l’authentification auprès du serveur de AD FS.  
  
6.  Le proxy d’application Web valide le jeton et transfère la requête au serveur principal.  
  
7.  Le serveur principal redirige la requête vers le serveur AD FS pour obtenir le jeton de sécurité de l’application.  
  
8.  La requête est redirigée vers le serveur principal. La requête contient désormais le jeton d’application et le cookie SSO. L’accès au site SharePoint est accordé à l’utilisateur et ce dernier n’a pas besoin d’entrer un nom d’utilisateur ou un mot de passe pour afficher le fichier.  
  
Les étapes de publication d’une application qui utilise MS-OFBA sont identiques à celles d’une application basée sur les revendications ou non basée sur les revendications. Pour les applications basées sur les revendications, consultez [publier une application basée sur les revendications pour les clients de navigateur Web](../web-application-proxy/../web-application-proxy/Publishing-Applications-using-AD-FS-Preauthentication.md#BKMK_1.1), pour les applications non basées sur les revendications, consultez [publier une application intégrée basée sur authentification Windows pour les clients de navigateur Web](../web-application-proxy/../web-application-proxy/Publishing-Applications-using-AD-FS-Preauthentication.md#BKMK_1.2). Le proxy d’application Web détecte automatiquement le client et authentifie l’utilisateur en fonction des besoins.  
  
## <a name="publish-an-application-that-uses-http-basic"></a>Publier une application qui utilise HTTP Basic  

HTTP Basic est le protocole d’autorisation utilisé par de nombreux protocoles pour connecter des clients enrichis, y compris des smartphones, à votre boîte aux lettres Exchange. Pour plus d’informations sur HTTP Basic, consultez [RFC 2617](https://www.ietf.org/rfc/rfc2617.txt). Le proxy d’application Web interagit traditionnellement avec AD FS à l’aide de redirections. la plupart des clients enrichis ne prennent pas en charge les cookies ou la gestion des États. De cette façon, le proxy d’application Web permet à l’application HTTP de recevoir une approbation de partie de confiance non basée sur les revendications pour l’application à l’service FS (Federation Service). Voir [Active Directory de plan](https://technet.microsoft.com/library/dn383648.aspx#BKMK_AD).  
  
Le workflow d’authentification pour les clients qui utilisent le protocole HTTP de base est décrit ci-dessous et dans ce diagramme :  
  
![](../../media/Publishing-Applications-using-AD-FS-Preauthentication/WebApplicationProxy_httpBasicflow.png)  
  
1.  L’utilisateur tente d’accéder à une application Web publiée un client de téléphone.  
  
2.  L’application envoie une requête HTTPs à l’URL publiée par le proxy d’application Web.  
  
3.  Si la demande ne contient pas d’informations d’identification, le proxy d’application Web renvoie une réponse HTTP 401 à l’application contenant l’URL du serveur d’AD FS d’authentification.  
  
4.  L’utilisateur envoie à nouveau la requête HTTPs à l’application avec l’autorisation définie sur de base et le nom d’utilisateur et le mot de passe chiffré de base 64 de l’utilisateur dans l’en-tête de demande www-Authenticate.  
  
5.  Étant donné que l’appareil ne peut pas être redirigé vers AD FS, le proxy d’application Web envoie une demande d’authentification à AD FS avec les informations d’identification dont le nom d’utilisateur et le mot de passe sont inclus. Le jeton est acquis pour le compte de l’appareil.  
  
6.  Afin de réduire le nombre de demandes envoyées au AD FS, le proxy d’application Web valide les demandes client ultérieures à l’aide de jetons mis en cache tant que le jeton est valide. Le proxy d’application Web nettoie régulièrement le cache. Vous pouvez afficher la taille du cache à l’aide du compteur de performances.  
  
7.  Si le jeton est valide, le proxy d’application Web transfère la demande au serveur principal et l’utilisateur se voit accorder l’accès à l’application Web publiée.  
  
La procédure suivante explique comment publier des applications de base HTTP.  
  
#### <a name="to-publish-an-http-basic-application"></a>Pour publier une application de base HTTP  
  
1.  Sur le serveur proxy d’application Web, dans la console Gestion de l’accès à distance, dans le volet de **navigation** , cliquez sur **proxy d’application Web**, puis, dans le volet **tâches** , cliquez sur **publier**.  
  
2.  Dans l’ **Assistant Publication d’une nouvelle application**, dans la page **Bienvenue** , cliquez sur **Suivant**.  
  
3.  Dans la page **pré-authentification** , cliquez sur **services ADFS (AD FS)** , puis cliquez sur **suivant**.  
  
4.  Sur la page **clients pris en charge** , sélectionnez **http de base** , puis cliquez sur **suivant**.  
  
    Si vous souhaitez activer l’accès à Exchange uniquement à partir d’appareils joints à l’espace de travail, cochez la case **activer l’accès uniquement pour les appareils joints à l’espace de travail** . Pour plus d’informations [, consultez joindre à l’espace de travail depuis n’importe quel appareil pour l’authentification unique et l’authentification de second facteur transparente entre les applications de l’entreprise](https://technet.microsoft.com/library/dn280945.aspx).  
  
5.  Dans la page **Partie de confiance**, dans la liste des parties de confiance, sélectionnez la partie de confiance pour l’application que vous voulez publier, puis cliquez sur **Suivant**. Notez que cette liste contient uniquement des parties de confiance sur-revendications.  
  
6.  Dans la page **Paramètres de publication** , procédez comme suit et cliquez sur **Suivant**:  
  
    -   Dans la zone **Nom** , tapez un nom convivial pour l’application.  
  
        Ce nom est utilisé uniquement dans la liste des applications publiées dans la console de gestion de l’accès à distance.  
  
    -   Dans la zone **URL externe** , entrez l’URL externe pour cette application. par exemple, mail.contoso.com  
  
    -   Dans la liste **Certificat externe**, sélectionnez un certificat dont l’objet couvre l’URL externe.  
  
    -   Dans la zone **URL du serveur principal**, entrez l’URL du serveur principal. Notez que cette valeur est entrée automatiquement quand vous entrez l’URL externe et vous ne devez la modifier que si l’URL du serveur principal est différente ; par exemple, mail.contoso.com.  
  
7.  Dans la page **Confirmation**, passez les paramètres en revue, puis cliquez sur **Publier**. Vous pouvez copier la commande PowerShell pour l’installation d’applications publiées supplémentaires.  
  
8.  Dans la page **Résultats** , vérifiez que l’application a été correctement publiée, puis cliquez sur **Fermer**.  
  
![](../../media/Publishing-Applications-using-AD-FS-Preauthentication/PowerShellLogoSmall.gif)***<em>commandes Windows PowerShell équivalentes</em>***  
  
L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.  
  
Ce script Windows PowerShell permet la pré-authentification pour tous les appareils, pas seulement pour les appareils joints à l’espace de travail.  
  
```  
Add-WebApplicationProxyApplication  
     -BackendServerUrl 'https://mail.contoso.com'   
     -ExternalCertificateThumbprint '697F4FF0B9947BB8203A96ED05A3021830638E50'   
     -ExternalUrl 'https://mail.contoso.com'   
     -Name 'Exchange'   
     -ExternalPreAuthentication ADFSforRichClients  
     -ADFSRelyingPartyName 'EAS_Relying_Party'  
```  
  
L’exemple ci-dessous préauthentifie uniquement les appareils joints à l’espace de travail :  
  
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
  
## <a name="BKMK_1.4"></a>Publier une application qui utilise OAuth2 comme une application Microsoft Store  
Pour publier une application pour les applications Microsoft Store, vous devez ajouter une approbation de partie de confiance pour l’application au service FS (Federation Service).  
  
Pour autoriser le proxy d’application Web à effectuer une authentification unique (SSO) et à effectuer la délégation des informations d’identification à l’aide de la délégation Kerberos Kerberos, le serveur proxy d’application Web doit être joint à un domaine. Voir [Active Directory de plan](https://technet.microsoft.com/library/dn383648.aspx#BKMK_AD).  
  
> [!NOTE]  
> Le proxy d’application Web prend en charge la publication uniquement pour les applications Microsoft Store qui utilisent le protocole OAuth 2,0.  
  
Dans la console de gestion AD FS, vous devez vous assurer que le point de terminaison OAuth est activé pour le proxy. Pour vérifier si le point de terminaison OAuth est activé pour le proxy, ouvrez la console de gestion AD FS, développez **Service**, cliquez sur **Points de terminaison**, dans la liste **Points de terminaison** , recherchez le point de terminaison OAuth et vérifiez que la valeur dans la colonne **Proxy activé** est **Oui**.  
  
Le workflow d’authentification pour les clients qui utilisent des applications Microsoft Store est décrit ci-dessous :  
  
> [!NOTE]  
> Le proxy d’application web redirige vers le serveur de AD FS pour l’authentification. Étant donné que les applications Microsoft Store ne prennent pas en charge les redirections, si vous utilisez des applications Microsoft Store, vous devez définir l’URL du serveur AD FS à l’aide de l’applet de commande Set-WebApplicationProxyConfiguration et du paramètre OAuthAuthenticationURL.  
>   
> Les applications Microsoft Store peuvent être publiées uniquement à l’aide de Windows PowerShell.  
  
1.  Le client tente d’accéder à une application Web publiée à l’aide d’une application Microsoft Store.  
  
2.  L’application envoie une requête HTTPs à l’URL publiée par le proxy d’application Web.  
  
3.  Le proxy d’application Web renvoie une réponse HTTP 401 à l’application contenant l’URL du serveur d’AD FS d’authentification. Ce processus est appelé « détection ».  
  
    > [!NOTE]  
    > Si l’application connaît l’URL du serveur d’authentification AD FS et qu’elle possède déjà un jeton combo contenant le jeton OAuth et le jeton Edge, les étapes 2 et 3 sont ignorées dans ce processus d’authentification.  
  
4.  L’application envoie une requête HTTPs au serveur AD FS.  
  
5.  L’application utilise le Service Broker d’authentification Web pour générer une boîte de dialogue dans laquelle l’utilisateur entre des informations d’identification pour s’authentifier auprès du serveur de AD FS. Pour plus d’informations sur le service Broker d’authentification web, voir [Service Broker d’authentification web](https://msdn.microsoft.com/library/windows/apps/hh750287.aspx).  
  
6.  Une fois l’authentification réussie, le serveur AD FS crée un jeton combo qui contient le jeton OAuth et le jeton Edge et envoie le jeton à l’application.  
  
7.  L’application envoie une requête HTTPs contenant le jeton combo à l’URL publiée par le proxy d’application Web.  
  
8.  Le proxy d’application Web divise le jeton combo en deux parties et valide le jeton Edge.  
  
9. Si le jeton Edge est valide, le proxy d’application Web transfère la demande au serveur principal avec uniquement le jeton OAuth. L’utilisateur a accès à l’application Web publiée.  
  
Cette procédure décrit comment publier une application pour OAuth2. Ce type d’application peut être publié uniquement à l’aide de Windows PowerShell. Avant de commencer, assurez-vous d’avoir effectué les étapes suivantes :  
  
-   Création d’une approbation de partie de confiance pour l’application dans la console de gestion AD FS.  
  
-   Vérifiez que le point de terminaison OAuth est activé pour le proxy dans la console de gestion AD FS et prenez note du chemin d’URL.  
  
-   Vérifié qu’un certificat sur le serveur proxy d’application Web est approprié pour l’application que vous souhaitez publier.  
  
#### <a name="to-publish-an-oauth2-app"></a>Pour publier une application OAuth2  
  
1.  Sur le serveur proxy d’application Web, dans la console Gestion de l’accès à distance, dans le volet de **navigation** , cliquez sur **proxy d’application Web**, puis, dans le volet **tâches** , cliquez sur **publier**.  
  
2.  Dans l’ **Assistant Publication d’une nouvelle application**, dans la page **Bienvenue** , cliquez sur **Suivant**.  
  
3.  Dans la page **pré-authentification** , cliquez sur **services ADFS (AD FS)** , puis cliquez sur **suivant**.  
  
4.  Sur la page **clients pris en charge** , sélectionnez **OAuth2** , puis cliquez sur **suivant**.  
  
5.  Dans la page **Partie de confiance**, dans la liste des parties de confiance, sélectionnez la partie de confiance pour l’application que vous voulez publier, puis cliquez sur **Suivant**.  
  
6.  Dans la page **Paramètres de publication** , procédez comme suit et cliquez sur **Suivant**:  
  
    -   Dans la zone **Nom** , tapez un nom convivial pour l’application.  
  
        Ce nom est utilisé uniquement dans la liste des applications publiées dans la console de gestion de l’accès à distance.  
  
    -   Dans la zone **URL externe**, entrez l’URL externe pour cette application, par exemple, https://server1.contoso.com/app1/.  
  
    -   Dans la liste **Certificat externe**, sélectionnez un certificat dont l’objet couvre l’URL externe.  
  
        Pour vous assurer que vos utilisateurs peuvent accéder à votre application, même s’ils négligent de taper HTTPs dans l’URL, activez la case à cocher **activer la redirection HTTP vers https** .  
  
    -   Dans la zone **URL du serveur principal**, entrez l’URL du serveur principal. Notez que cette valeur est entrée automatiquement quand vous entrez l’URL externe et vous ne devez la modifier que si l’URL du serveur principal est différente ; par exemple, https://sp/app1/.  
  
        > [!NOTE]  
        > Le proxy d’application Web peut traduire les noms d’hôte dans des URL, mais ne peut pas traduire les noms de chemin d’accès. C’est pourquoi, vous pouvez entrer différents noms d’hôte, mais vous devez entrer le même nom de chemin. Par exemple, vous pouvez entrer une URL externe de https://apps.contoso.com/app1/ et une URL de serveur principal de https://app-server/app1/. Toutefois, vous ne pouvez pas entrer une URL externe de https://apps.contoso.com/app1/ et une URL de serveur principal de https://apps.contoso.com/internal-app1/.  
  
7.  Dans la page **Confirmation**, passez les paramètres en revue, puis cliquez sur **Publier**. Vous pouvez copier la commande PowerShell pour l’installation d’applications publiées supplémentaires.  
  
8.  Dans la page **Résultats** , vérifiez que l’application a été correctement publiée, puis cliquez sur **Fermer**.  
  
Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.  
  
Pour définir l’URL d’authentification OAuth pour une adresse de serveur de Fédération fs.contoso.com et un chemin d’URL/ADFS/oauth2/ :  
  
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
  
-   [Résolution des problèmes au niveau du proxy d’application web](https://technet.microsoft.com/library/dn770156.aspx)  
  
-   [Publier des applications via le proxy d’application Web](https://technet.microsoft.com/library/dn383659.aspx)  
  
-   [Planification de la publication d’applications à l’aide du proxy d’application Web](https://technet.microsoft.com/library/dn383650.aspx)  
  
-   [Guide pas à pas du proxy d’application Web](https://technet.microsoft.com/library/dn280944.aspx)  
  
-   [Applets de commande du proxy d’application Web dans Windows PowerShell](https://technet.microsoft.com/library/dn283404.aspx)  
  
-   [Add-WebApplicationProxyApplication](https://technet.microsoft.com/library/dn283409.aspx)  
  
-   [Set-WebApplicationProxyConfiguration](https://technet.microsoft.com/library/dn283406.aspx)  
  


