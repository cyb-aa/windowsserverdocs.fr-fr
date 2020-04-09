---
ms.assetid: 61ed00fd-51c7-4728-91fa-8501de9d8f28
title: Publication d’applications avec SharePoint, Exchange et la passerelle des services Bureau à distance
author: billmath
manager: mtillman
ms.author: billmath
ms.date: 04/30/2018
ms.topic: article
ms.prod: windows-server
ms.technology: web-app-proxy
ms.openlocfilehash: 18851463b82afc1dc34615e6faaa14622c80224a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80818682"
---
# <a name="publishing-applications-with-sharepoint-exchange-and-rdg"></a>Publication d’applications avec SharePoint, Exchange et la passerelle des services Bureau à distance

> S’applique à Windows Server 2016

**Ce contenu est pertinent pour la version locale du proxy d’application Web. Pour activer l’accès sécurisé aux applications locales sur le Cloud, consultez le [Azure ad le contenu du proxy d’application](https://azure.microsoft.com/documentation/articles/active-directory-application-proxy-get-started/).**

Cette rubrique décrit les tâches nécessaires à la publication de SharePoint Server, d’Exchange Server ou de la passerelle Bureau à distance (RDP) via le proxy d’application Web.

> [!NOTE]
> Ces informations sont fournies telles quelles.  Services Bureau à distance prend en charge et recommande l’utilisation [d’Azure App proxy pour fournir un accès à distance sécurisé aux applications locales](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-get-started).

## <a name="publish-sharepoint-server"></a><a name="BKMK_6.1"></a>Publier le serveur SharePoint
Vous pouvez publier un site SharePoint via le proxy d’application Web lorsque le site SharePoint est configuré pour l’authentification basée sur les revendications ou l’authentification Windows intégrée. Si vous souhaitez utiliser Services ADFS (AD FS) pour la pré-authentification, vous devez configurer une partie de confiance à l’aide de l’un des assistants.

-   Si le site SharePoint utilise l’authentification basée sur les revendications, vous devez utiliser l’Assistant Ajouter une approbation de partie de confiance afin de configurer l’approbation pour l’application.

-   Si le site SharePoint utilise l’authentification Windows intégrée, vous devez utiliser l’Assistant Ajouter une approbation de partie de confiance non basée sur les revendications afin de configurer l’approbation pour l’application. Vous pouvez utiliser IWA avec une application web basée sur les revendications du moment que vous configurez le contrôleur de domaine Kerberos (KDC).

    Pour permettre aux utilisateurs de s’authentifier à l’aide de l’authentification Windows intégrée, le serveur proxy d’application Web doit être joint à un domaine.

    Vous devez configurer l’application pour prendre en charge la délégation contrainte Kerberos. Vous pouvez effectuer cette opération sur le contrôleur de domaine de n’importe quelle application. Vous pouvez également configurer l’application directement sur le serveur principal si elle s’exécute sur Windows Server 2012 R2 ou Windows Server 2012. Pour plus d’informations, voir [Nouveautés de l’authentification Kerberos](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831747(v=ws.11)). Vous devez également vous assurer que les serveurs proxy d’application Web sont configurés pour la délégation aux noms de principal de service des serveurs principaux. Pour obtenir une procédure pas à pas de configuration du proxy d’application Web pour publier une application à l’aide de l’authentification Windows intégrée, consultez [configurer un site pour utiliser l’authentification Windows intégrée](https://docs.microsoft.com/previous-versions/orphan-topics/ws.11/dn308246(v=ws.11)).

Si votre site SharePoint est configuré à l’aide des mappages d’accès de substitution (AAM) ou des collections de sites de noms d’hôtes, vous pouvez utiliser différentes URL du serveur externe et principal pour publier votre application. Toutefois, si vous ne configurez pas votre site SharePoint à l’aide de AAM ou des collections de sites hôtes, vous devez utiliser les mêmes URL de serveur externe et principal.

## <a name="publish-exchange-server"></a><a name="BKMK_6.2"></a>Publier Exchange Server
Le tableau suivant décrit les services Exchange que vous pouvez publier via le proxy d’application Web et la pré-authentification prise en charge pour ces services :


|    Service Exchange    |                                                                            Pré-authentification                                                                            |                                                                                                                                       Remarques                                                                                                                                        |
|------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    Outlook Web App     | -AD FS à l’aide de l’authentification non basée sur les revendications<br />-Direct<br />-AD FS à l’aide de l’authentification basée sur les revendications pour Exchange 2013 service Pak 1 (SP1) sur site |                                                                  Pour plus d’informations, voir : [Utiliser l’authentification basée sur les revendications AD FS avec Outlook Web App et le CAE](https://go.microsoft.com/fwlink/?LinkId=393723)                                                                  |
| Panneau de configuration Exchange |                                                                               Pass-through                                                                               |                                                                                                                                                                                                                                                                                    |
|    Outlook Anywhere    |                                                                               Pass-through                                                                               | Vous devez publier les trois URL pour que Outlook Anywhere fonctionne correctement :<p>-L’URL de découverte automatique.<br />-Le nom d’hôte externe du serveur Exchange ; autrement dit, l’URL configurée pour la connexion des clients à.<br />-Le nom de domaine complet interne du serveur Exchange. |
|  Exchange ActiveSync   |                                                     Pass-through<br/> AD FS à l’aide du protocole d’autorisation de base HTTP                                                      |                                                                                                                                                                                                                                                                                    |

Pour publier Outlook Web App à l’aide de l’authentification Windows intégrée, vous devez utiliser l’Assistant Ajouter une approbation de partie de confiance non basée sur les revendications afin de configurer l’approbation pour l’application.

Pour permettre aux utilisateurs de s’authentifier à l’aide de la délégation Kerberos restreinte, le serveur proxy d’application Web doit être joint à un domaine.

Vous devez configurer l’application pour prendre en charge l’authentification Kerberos. En outre, vous devez enregistrer un nom de principal du service (SPN) sur le compte sous lequel le service Web s’exécute. Vous pouvez le faire sur le contrôleur de domaine ou sur les serveurs principaux. Dans un environnement Exchange à charge équilibrée, cela nécessiterait l’utilisation d’un autre compte de service, consultez [configuration de l’authentification Kerberos pour les serveurs d’accès client avec équilibrage de charge](https://docs.microsoft.com/exchange/configuring-kerberos-authentication-for-load-balanced-client-access-servers-exchange-2013-help) .

Vous pouvez également configurer l’application directement sur le serveur principal si elle s’exécute sur Windows Server 2012 R2 ou Windows Server 2012. Pour plus d’informations, voir [Nouveautés de l’authentification Kerberos](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831747(v=ws.11)). Vous devez également vous assurer que les serveurs proxy d’application Web sont configurés pour la délégation aux noms de principal de service des serveurs principaux.

## <a name="publishing-remote-desktop-gateway-through-web-application-proxy"></a>Publication Bureau à distance passerelle via le proxy d’application Web
Si vous souhaitez restreindre l’accès à votre passerelle d’accès à distance et ajouter une pré-authentification pour l’accès à distance, vous pouvez la déployer via le proxy d’application Web. C’est un excellent moyen de s’assurer que vous disposez d’une pré-authentification riche pour RDG, y compris MFA. La publication sans pré-authentification est également une option qui fournit un point d’entrée unique dans vos systèmes.

#### <a name="how-to-publish-an-application-in-rdg-using-web-application-proxy-pass-through-authentication"></a>Comment publier une application dans RDG à l’aide de l’authentification directe de proxy d’application Web

1. L’installation sera différente selon que vos rôles de Accès web Bureau à distance (/RDWeb) et de passerelle Bureau à distance (RPC) se trouvent sur le même serveur ou sur des serveurs différents.

2. Si les rôles Bureau à distance Accès web et passerelle Bureau à distance sont hébergés sur le même serveur RDG, vous pouvez simplement publier le nom de domaine complet racine dans le proxy d’application Web, par exemple, https://rdg.contoso.com/.

   Vous pouvez également publier les deux répertoires virtuels individuellement, par exemple<https://rdg.contoso.com/rdweb/> et https://rdg.contoso.com/rpc/.

3. Si les Accès web RD et la passerelle des services Bureau à distance sont hébergés sur des serveurs RDG distincts, vous devez publier les deux répertoires virtuels individuellement. Vous pouvez utiliser le même nom de domaine complet externe, par exemple https://rdweb.contoso.com/rdweb/ et https://gateway.contoso.com/rpc/.

4. Si le nom de domaine complet interne et externe est différent, vous ne devez pas désactiver la conversion d’en-tête de demande sur la règle de publication RDWeb. Pour ce faire, exécutez le script PowerShell suivant sur le serveur proxy d’application Web, mais il doit être activé par défaut.

   ```PowerShell
   Get-WebApplicationProxyApplication applicationname | Set-WebApplicationProxyApplication -DisableTranslateUrlInRequestHeaders:$false
   ```

   > [!NOTE]
   > Si vous avez besoin de prendre en charge des clients riches tels que les connexions aux programmes RemoteApp et aux services Bureau à RDG ou les connexions Bureau à distance iOS, celles-ci ne prennent pas en charge l’authentification préalable. vous devez donc publier des à l’aide de l’authentification directe.

#### <a name="how-to-publish-an-application-in-rdg-using-web-application-proxy-with-pre-authentication"></a>Comment publier une application dans RDG à l’aide du proxy d’application Web avec pré-authentification

1.  La pré-authentification du proxy d’application Web avec RDG fonctionne en passant le cookie de pré-authentification obtenu par Internet Explorer passé au client Connexion Bureau à distance (Mstsc. exe). Il est ensuite utilisé par le client Connexion Bureau à distance (Mstsc. exe). Elle est ensuite utilisée par Connexion Bureau à distance client comme preuve d’authentification.

    La procédure suivante indique au serveur de collection d’inclure les propriétés RDP personnalisées nécessaires dans les fichiers RDP d’application distante qui sont envoyés aux clients. Ils indiquent au client que la pré-authentification est requise et de transmettre les cookies pour l’adresse du serveur de pré-authentification à Connexion Bureau à distance client (Mstsc. exe). Conjointement à la désactivation de la fonctionnalité HttpOnly sur l’application de proxy d’application Web, permet au client Connexion Bureau à distance (Mstsc. exe) d’utiliser le cookie de proxy d’application Web obtenu par le biais du navigateur.

    L’authentification auprès du serveur de Accès web Bureau à distance continue d’utiliser l’ouverture de session du formulaire Accès web des services Bureau à distance. Cela fournit le moins d’invites d’authentification utilisateur, car le formulaire de connexion Bureau à distance Accès web crée une banque d’informations d’identification côté client qui peut ensuite être utilisée par Connexion Bureau à distance client (Mstsc. exe) pour tout lancement d’application distant suivant.

2.  Tout d’abord, créez une approbation de partie de confiance manuelle dans AD FS comme si vous publiiez une application prenant en charge les revendications. Cela signifie que vous devez créer une approbation de partie de confiance factice qui est là pour appliquer la pré-authentification, afin d’obtenir une pré-authentification sans délégation contrainte Kerberos sur le serveur publié. Une fois qu’un utilisateur s’est authentifié, tout le reste est transmis.

    > [!WARNING]
    > Il peut sembler que l’utilisation de la délégation est préférable, mais elle ne résout pas entièrement les exigences de l’authentification unique mstsc et il y a des problèmes lors de la délégation au répertoire/RPC, car le client s’attend à gérer l’authentification de la passerelle des services Bureau à distance.

3.  Pour créer une approbation de partie de confiance manuelle, suivez les étapes de la console de gestion AD FS :

    1.  Utiliser l’Assistant **Ajout d’approbation de partie de confiance**

    2.  Sélectionnez **entrer manuellement les données relatives à la partie de confiance**.

    3.  Acceptez tous les paramètres par défaut.

    4.  Pour l’identificateur d’approbation de la partie de confiance, entrez le nom de domaine complet externe que vous allez utiliser pour l’accès RDG, par exemple https://rdg.contoso.com/.

        Il s’agit de l’approbation de partie de confiance que vous allez utiliser lors de la publication de l’application dans le proxy d’application Web.

4.  Publiez la racine du site (par exemple, https://rdg.contoso.com/) dans le proxy d’application Web. Définissez l’authentification préalable sur AD FS et utilisez l’approbation de la partie de confiance que vous avez créée ci-dessus. Cela permettra à/RDWeb et/RPC d’utiliser le même cookie d’authentification du proxy d’application Web.

    Il est possible de publier/RDWeb et/RPC en tant qu’applications distinctes et même d’utiliser différents serveurs publiés. Vous devez simplement vous assurer que vous publiez à la fois à l’aide de la même approbation de partie de confiance que le jeton de proxy d’application Web est émis pour l’approbation de partie de confiance et qu’elle est donc valide sur les applications publiées avec la même approbation de partie de confiance.

5.  Si le nom de domaine complet interne et externe est différent, vous ne devez pas désactiver la conversion d’en-tête de demande sur la règle de publication RDWeb. Pour ce faire, exécutez le script PowerShell suivant sur le serveur proxy d’application Web, mais il doit être activé par défaut :

    ```PowerShell
    Get-WebApplicationProxyApplication applicationname | Set-WebApplicationProxyApplication -DisableTranslateUrlInRequestHeaders:$false
    ```

6.  Désactivez la propriété de cookie HttpOnly dans le proxy d’application Web sur l’application publiée RDG. Pour autoriser le contrôle ActiveX RDG à accéder au cookie d’authentification du proxy d’application Web, vous devez désactiver la propriété HttpOnly sur le cookie du proxy d’application Web.

    Pour cela, vous devez installer le [correctif cumulatif de novembre 2014 pour Windows RT 8,1, Windows 8.1 et Windows Server 2012 R2 (KB3000850)](https://support.microsoft.com/kb/3000850).

    Après l’installation du correctif, exécutez le script PowerShell suivant sur le serveur proxy d’application Web en spécifiant le nom d’application approprié :

    ```PowerShell
    Get-WebApplicationProxyApplication applicationname | Set-WebApplicationProxyApplication -DisableHttpOnlyCookieProtection:$true
    ```

    La désactivation de HttpOnly permet au contrôle ActiveX RDG d’accéder au cookie d’authentification du proxy d’application Web.

7.  Configurez la collection RDG appropriée sur le serveur de collecte pour permettre au client Connexion Bureau à distance (Mstsc. exe) de savoir que la pré-authentification est requise dans le fichier RDP.

    -   Dans Windows Server 2012 et Windows Server 2012 R2, vous pouvez le faire en exécutant l’applet de commande PowerShell suivante sur le serveur de collection RDG :

        ```PowerShell
        Set-RDSessionCollectionConfiguration -CollectionName "<yourcollectionname>" -CustomRdpProperty "pre-authentication server address:s: <https://externalfqdn/rdweb/>`nrequire pre-authentication:i:1"
        ```

        Veillez à supprimer les < et > crochets lorsque vous remplacez par vos propres valeurs, par exemple :

        ```PowerShell
        Set-RDSessionCollectionConfiguration -CollectionName "MyAppCollection" -CustomRdpProperty "pre-authentication server address:s: https://rdg.contoso.com/rdweb/`nrequire pre-authentication:i:1"
        ```

    -   Dans Windows Server 2008 R2 :

        1.  Connectez-vous à la Terminal Server à l’aide d’un compte disposant de privilèges d’administrateur.

        2.  Accédez à **démarrer** >**outils d’administration** > **services Terminal** > **Gestionnaire RemoteApp TS.**

        3.  Dans le volet **vue d’ensemble** des Gestionnaire RemoteApp TS, en regard de paramètres RDP, cliquez sur **modifier**.

        4.  Sous l’onglet **Paramètres RDP personnalisés** , tapez les paramètres RDP suivants dans la zone Paramètres RDP personnalisés :

            `pre-authentication server address: s: https://externalfqdn/rdweb/`

            `require pre-authentication:i:1`

        5.  Lorsque vous avez terminé, cliquez sur **appliquer**.

            Cela indique au serveur de collection d’inclure les propriétés RDP personnalisées dans les fichiers RDP envoyés aux clients. Celles-ci indiquent au client que la pré-authentification est requise et de transmettre les cookies pour l’adresse du serveur de pré-authentification au client Connexion Bureau à distance (Mstsc. exe). Cela, conjointement à la désactivation de HttpOnly sur l’application de proxy d’application Web, permet au client Connexion Bureau à distance (Mstsc. exe) d’utiliser le cookie d’authentification du proxy d’application Web obtenu par le biais du navigateur.

            Pour plus d’informations sur le protocole RDP, consultez [configuration du scénario de mot de passe à usage unique de la passerelle TS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731249(v=ws.10)).

## <a name="see-also"></a><a name="BKMK_Links"></a>Voir aussi

- [Planification de la publication d’applications à l’aide du proxy d’application Web](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn383650(v=ws.11))

- [Résolution des problèmes au niveau du proxy d’application web](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn770156(v=ws.11))

- [Guide pas à pas du proxy d’application Web](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn280944(v=ws.11))
