---
ms.assetid: 61ed00fd-51c7-4728-91fa-8501de9d8f28
title: Publication d’applications avec SharePoint, Exchange et la passerelle des services Bureau à distance
description: ''
author: billmath
manager: mtillman
ms.author: billmath
ms.date: 04/30/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: web-app-proxy
ms.openlocfilehash: 21ea6ecf717392027c1d00f818e230e2fe56a11d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826820"
---
# <a name="publishing-applications-with-sharepoint-exchange-and-rdg"></a>Publication d’applications avec SharePoint, Exchange et la passerelle des services Bureau à distance

>S'applique à : Windows Server 2016

**Ce contenu est pertinent pour la version locale de Proxy d’Application Web. Pour activer un accès sécurisé aux applications locales sur le cloud, consultez le [contenu Azure Proxy d’Application AD](https://azure.microsoft.com/documentation/articles/active-directory-application-proxy-get-started/).**  
  
Cette rubrique décrit les tâches nécessaires à la publication SharePoint Server, Exchange Server ou passerelle RDP (Remote Desktop) via le Proxy d’Application Web.  

>[!NOTE]
>Ces informations sont fournies en tant que-est.  Services Bureau à distance prend en charge et recommande l’utilisation de [Proxy d’application Azure pour fournir un accès à distance sécurisé aux applications locales](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-get-started).
  
## <a name="BKMK_6.1"></a>Publication SharePoint Server  
Vous pouvez publier un site SharePoint via le Proxy d’Application Web lorsque le site SharePoint est configuré pour l’authentification basée sur les revendications ou l’authentification Windows intégrée. Si vous souhaitez utiliser Active Directory Federation Services (ADFS) pour l’authentification préalable, vous devez configurer une partie de confiance à l’aide d’un Assistant.  
  
-   Si le site SharePoint utilise l’authentification basée sur les revendications, vous devez utiliser l’Assistant Ajouter une approbation de partie de confiance afin de configurer l’approbation pour l’application.  
  
-   Si le site SharePoint utilise l’authentification Windows intégrée, vous devez utiliser l’Assistant Ajouter une approbation de partie de confiance non basée sur les revendications afin de configurer l’approbation pour l’application. Vous pouvez utiliser IWA avec une application web basée sur les revendications du moment que vous configurez le contrôleur de domaine Kerberos (KDC).  
  
    Pour permettre aux utilisateurs de s’authentifier à l’aide de l’authentification Windows intégrée, le serveur de Proxy d’Application Web doit être joint à un domaine.  
  
    Vous devez configurer l’application pour prendre en charge la délégation contrainte Kerberos. Vous pouvez effectuer cette opération sur le contrôleur de domaine de n’importe quelle application. Vous pouvez également configurer l’application directement sur le serveur principal s’il s’exécute sur Windows Server 2012 R2 ou Windows Server 2012. Pour plus d’informations, voir [Nouveautés de l’authentification Kerberos](https://technet.microsoft.com/library/hh831747.aspx). Vous devez également vous assurer que les serveurs Proxy d’Application Web sont configurés pour la délégation pour les noms de principal du service des serveurs principaux. Pour une procédure pas à pas montrant comment configurer le Proxy d’Application Web pour publier une application à l’aide de l’authentification Windows intégrée, consultez [configurer un site pour utiliser l’authentification Windows intégrée](assetId:///b0788958-627f-450f-877c-209b1bd0db52).  
  
Si votre site SharePoint est configuré à l’aide des mappages d’accès de substitution (AAM) ou des collections de sites de noms d’hôtes, vous pouvez utiliser différentes URL du serveur externe et principal pour publier votre application. Toutefois, si vous ne configurez pas votre site SharePoint à l’aide de AAM ou des collections de sites hôtes, vous devez utiliser les mêmes URL de serveur externe et principal.  
  
## <a name="BKMK_6.2"></a>Publier Exchange Server  
Le tableau suivant décrit les services Exchange que vous pouvez publier via le Proxy d’Application Web et de la pré-authentification prise en charge pour ces services :  
  
|Service Exchange|Pré-authentification|Notes|  
|--------------------|-----------------------|---------|  
|Outlook Web App|-AD FS à l’aide de l’authentification non basée sur des revendications<br />-Directe<br />-AD FS à l’aide de l’authentification basée sur les revendications pour local Exchange 2013 Service Pack 1 (SP1)|Pour plus d’informations, voir : [À l’aide de l’authentification basée sur les revendications AD FS avec Outlook Web App et EAC](https://go.microsoft.com/fwlink/?LinkId=393723)|  
|Panneau de configuration Exchange|Pass-through||  
|Outlook Anywhere|Pass-through|Vous devez publier les trois URL pour que Outlook Anywhere fonctionne correctement :<br /><br />-URL de découverte automatique.<br />-Le nom d’hôte externe du serveur Exchange ; Autrement dit, l’URL qui est configuré pour les clients pour se connecter à.<br />-Le FQDN interne du serveur Exchange.|  
|Exchange ActiveSync|Pass-through<br/> AD FS à l’aide du protocole d’autorisation de base HTTP|| Pour plus d’informations, consultez : https://docs.microsoft.com/windows-server/remote/remote-access/web-application-proxy/publishing-applications-using-ad-fs-preauthentication 
  
Pour publier Outlook Web App à l’aide de l’authentification Windows intégrée, vous devez utiliser l’Assistant Ajouter une approbation de partie de confiance non basée sur les revendications afin de configurer l’approbation pour l’application.  
  
Pour permettre aux utilisateurs de s’authentifier à l’aide de Kerberos la délégation contrainte que du serveur de Proxy d’Application Web doit être joint à un domaine.  
  
Vous devez configurer l’application pour prendre en charge l’authentification Kerberos. En outre vous devez inscrire un nom de principal du service (SPN) pour le compte que le service web s’exécute sous. Pour cela sur le contrôleur de domaine ou sur les serveurs principaux. Dans une charge équilibrée environnement Exchange cela nécessiterait à l’aide de l’autre compte de Service, consultez [l’authentification Kerberos de configuration pour les serveurs d’accès Client avec équilibrage de charge](https://technet.microsoft.com/library/ff808312(v=exchg.150).aspx)  
  
Vous pouvez également configurer l’application directement sur le serveur principal s’il s’exécute sur Windows Server 2012 R2 ou Windows Server 2012. Pour plus d’informations, voir [Nouveautés de l’authentification Kerberos](https://technet.microsoft.com/library/hh831747.aspx). Vous devez également vous assurer que les serveurs Proxy d’Application Web sont configurés pour la délégation pour les noms de principal du service des serveurs principaux.  
  
## <a name="publishing-remote-desktop-gateway-through-web-application-proxy"></a>Passerelle des services Bureau à distance via le Proxy d’Application Web de publication  
Si vous souhaitez restreindre l’accès à votre passerelle d’accès à distance et ajouter l’authentification préalable pour l’accès à distance, vous pouvez déployer via le Proxy d’Application Web. Il s’agit d’un excellent moyen de s’assurer que vous avez pré-authentification riche pour la passerelle Bureau à distance, notamment l’authentification Multifacteur. La publication sans l’authentification préalable est également une option et fournit un point unique d’entrée dans vos systèmes.  
  
#### <a name="how-to-publish-an-application-in-rdg-using-web-application-proxy-pass-through-authentication"></a>Comment publier une application de passerelle Bureau à distance à l’aide de l’authentification directe de Proxy d’Application Web  
  
1.  Processus d’installation sera différent selon que votre accès Web de bureau à distance (/ rdweb) et les rôles de passerelle Bureau à distance (rpc) sont sur le même serveur ou sur des serveurs différents.  
  
2.  Si les rôles de l’accès Web Bureau à distance et passerelle Bureau à distance sont hébergés sur le même serveur de passerelle Bureau à distance, vous pouvez simplement publier la racine du nom de domaine complet dans le Proxy d’Application Web, tels que https://rdg.contoso.com/.  
  
    Vous pouvez également publier les deux répertoires virtuels individuellement, par exemple https://rdg.contoso.com/rdweb/ et https://rdg.contoso.com/rpc/.  
  
3.  Si l’accès Web Bureau à distance et la passerelle Bureau à distance sont hébergés sur des serveurs de passerelle Bureau à distance distincts, vous devez publier les deux répertoires virtuels individuellement. Vous pouvez utiliser, par exemple l’identiques ou différent FQDN externe https://rdweb.contoso.com/rdweb/ et https://gateway.contoso.com/rpc/.  
  
4.  Traduction d’en-tête de demande sur la règle de publication RDWeb ne pas désactiver si l’externe et interne FQDN sont différents. Cela est possible en exécutant le script PowerShell suivant sur le serveur de Proxy d’Application Web, mais elle doit être activée par défaut.
  
    ```  
    Get-WebApplicationProxyApplication applicationname | Set-WebApplicationProxyApplication -DisableTranslateUrlInRequestHeaders:$false  
    ```  
  
    > [!NOTE]  
    > Si vous avez besoin prendre en charge les clients riches tels que RemoteApp et bureau connexions ou iOS connexions Bureau à distance, ils ne gèrent pas l’authentification préalable afin d’avoir à publier la passerelle Bureau à distance à l’aide de l’authentification directe.  
  
#### <a name="how-to-publish-an-application-in-rdg-using-web-application-proxy-with-pre-authentication"></a>Comment publier une application de passerelle Bureau à distance à l’aide du Proxy d’Application Web avec l’authentification préalable  
  
1.  Authentification préalable de Proxy d’Application Web avec la passerelle Bureau à distance fonctionne en passant le cookie d’authentification préalable obtenu par Internet Explorer est passé par le client Connexion Bureau à distance (mstsc.exe). Il est ensuite utilisé par le client Connexion Bureau à distance (mstsc.exe). Il est ensuite utilisé par le client Connexion Bureau à distance en tant que preuve d’authentification.  
  
    La procédure suivante indique au serveur de Collection pour inclure les propriétés RDP personnalisées nécessaires dans les fichiers RDP d’application à distance qui sont envoyés aux clients. Ceux-ci indiquent le client que l’authentification préalable est requis et à passer des cookies pour la pré-authentification serveur adresse au client Connexion Bureau à distance (mstsc.exe). Conjointement avec la désactivation de la fonctionnalité de HttpOnly sur l’application de Proxy d’Application Web, cela permet au client de connexion Bureau à distance (mstsc.exe) d’utiliser le cookie de Proxy d’Application Web obtenu via le navigateur.  
  
    Authentification auprès du serveur d’accès Web de bureau à distance utilise toujours l’ouverture de session de formulaire de l’accès Web Bureau à distance. Cela fournit le plus petit nombre de l’authentification utilisateur vous invite à entrer en tant que le formulaire d’ouverture de session de l’accès Web Bureau à distance crée un magasin d’informations d’identification côté client qui peut ensuite être utilisé par client Connexion Bureau à distance (mstsc.exe) pour tout lancement des applications à distance suivante.  
  
2.  Commencez par créer un manuel de confiance dans AD FS comme si vous devaient publier une application prenant en charge les revendications. Cela signifie que vous devez créer un fantôme de confiance qui est utilisé pour appliquer l’authentification préalable, afin que vous obtenez une authentification préalable sans la délégation contrainte Kerberos sur le serveur publié. Une fois qu’un utilisateur est authentifié, tout le reste est transmis.  
  
    > [!WARNING]  
    > Il peut sembler qu’est préférable d’utiliser la délégation, mais elle ne résout pas entièrement les exigences de l’authentification unique mstsc et il existe des problèmes lors de la délégation pour le répertoire /rpc, car le client s’attend à gérer lui-même l’authentification de passerelle Bureau à distance.  
  
3.  Pour créer un manuel de confiance, suivez les étapes décrites dans la Console de gestion AD FS :  
  
    1.  Utilisez le **ajouter Relying Party Trust** Assistant  
  
    2.  Sélectionnez **entrer manuellement les données relatives à la confiance**.  
  
    3.  Acceptez tous les paramètres par défaut.  
  
    4.  Pour l’identificateur de confiance, entrez le FQDN externe que vous utiliserez pour l’accès de la passerelle Bureau à distance, par exemple https://rdg.contoso.com/.  
  
        Il s’agit de la partie de confiance que vous utiliserez lors de la publication de l’application dans le Proxy d’Application Web.  
  
4.  Publier la racine du site (par exemple, https://rdg.contoso.com/ ) dans le Proxy d’Application Web. La valeur de la pré-authentification AD FS et utilisez la confiance que vous avez créé ci-dessus. Cela permettra/RDWeb et /rpc à utiliser le même cookie d’authentification de Proxy d’Application Web.  
  
    Il est possible de publier des/RDWeb et /rpc en tant qu’applications distinctes et même d’utiliser différents serveurs publiés. Vous devez simplement vous assurer que vous publiez à la fois à l’aide de la même confiance que le jeton de Proxy d’Application Web est émis pour la confiance et est donc valide entre les applications publiées avec la même confiance.  
  
5.  Traduction d’en-tête de demande sur la règle de publication RDWeb ne pas désactiver si l’externe et interne FQDN sont différents. Cela est possible en exécutant le script PowerShell suivant sur le serveur de Proxy d’Application Web, mais elle doit être activée par défaut :
  
    ```  
    Get-WebApplicationProxyApplication applicationname | Set-WebApplicationProxyApplication -DisableTranslateUrlInRequestHeaders:$false 
    ```  
  
6.  Désactiver la propriété de cookie HttpOnly dans Proxy d’Application Web sur la passerelle Bureau à distance une application publiée. Pour autoriser l’accès de contrôle ActiveX de passerelle Bureau à distance pour le cookie d’authentification de Proxy d’Application Web, vous devez désactiver la propriété HttpOnly sur le cookie de Proxy d’Application Web.  
  
    Cela requiert ce qui suit doit être installé [correctif de Proxy d’Application Web](https://support.microsoft.com/en-gb/kb/3000850) ou [ https://support.microsoft.com/en-gb/kb/3000850 ](https://support.microsoft.com/en-gb/kb/3000850).  
  
    Après avoir installé le correctif logiciel exécuter le script PowerShell suivant sur le serveur de Proxy d’Application Web en spécifiant le nom de l’application concernée :  
  
    ```  
    Get-WebApplicationProxyApplication applicationname | Set-WebApplicationProxyApplication -DisableHttpOnlyCookieProtection:$true  
    ```  
  
    La désactivation HttpOnly permet l’accès de contrôle ActiveX de passerelle Bureau à distance pour le cookie d’authentification de Proxy d’Application Web.  
  
7.  Configurer la collection pertinente de la passerelle Bureau à distance sur le serveur de collecte pour permettre la connexion Bureau à distance (mstsc.exe) de client savoir que l’authentification préalable est requis dans le fichier rdp.  
  
    -   Dans Windows Server 2012 et Windows Server 2012 R2 pouvez le faire en exécutant l’applet de commande PowerShell suivante sur le serveur de collecte de passerelle Bureau à distance :  
  
        ```
        Set-RDSessionCollectionConfiguration -CollectionName "<yourcollectionname>" -CustomRdpProperty "pre-authentication server address:s: <https://externalfqdn/rdweb/>`nrequire pre-authentication:i:1"
        ```
  
        Veillez à supprimer les crochets lorsque vous remplacez par vos propres valeurs, par exemple < et > :
        
        ```
        Set-RDSessionCollectionConfiguration -CollectionName "MyAppCollection" -CustomRdpProperty "pre-authentication server address:s: https://rdg.contoso.com/rdweb/`nrequire pre-authentication:i:1"
        ```
  
    -   Dans Windows Server 2008 R2 :  
  
        1.  Ouvrez une session sur le serveur Terminal Server avec un compte disposant des privilèges d’administrateur.  
  
        2.  Accédez à **Démarrer** >**outils d’administration** > **Services Terminal Server** > **Gestionnaire RemoteApp TS.**  
  
        3.  Dans le **vue d’ensemble** volet du Gestionnaire RemoteApp TS, en regard des paramètres RDP, cliquez sur **modification**.  
  
        4.  Sur le **paramètres RDP personnalisés** , tapez les paramètres RDP suivants dans la zone de paramètres RDP personnalisés :  
  
            `pre-authentication server address: s: https://externalfqdn/rdweb/`  
  
            `require pre-authentication:i:1`  
  
        5.  Lorsque vous avez terminé, cliquez sur **appliquer**.  
  
            Cela indique au serveur de Collection pour inclure les propriétés RDP personnalisées dans les fichiers RDP qui sont envoyés aux clients. Ceux-ci indiquent le client que l’authentification préalable est requis et à passer des cookies pour l’authentification préalable les adresses des serveurs pour le client Connexion Bureau à distance (mstsc.exe). Conjointement avec la désactivation HttpOnly sur l’application de Proxy d’Application Web, ainsi, le client Connexion Bureau à distance (mstsc.exe) d’utiliser le cookie d’authentification de Proxy d’Application Web obtenu via le navigateur.  
  
            Pour plus d’informations sur le protocole RDP, consultez [configuration du scénario d’OTP de passerelle TS](https://technet.microsoft.com/library/cc731249(v=ws.10).aspx).  
  
## <a name="BKMK_Links"></a>Voir aussi  
  
-   [Envisagez de publier des Applications à l’aide du Proxy d’Application Web](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn383650(v=ws.11))  
  
-   [Résolution des problèmes de Proxy d’Application Web](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn770156(v=ws.11))  
  
-   [Guide de procédure pas à pas de Web Application Proxy](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn280944(v=ws.11))  
