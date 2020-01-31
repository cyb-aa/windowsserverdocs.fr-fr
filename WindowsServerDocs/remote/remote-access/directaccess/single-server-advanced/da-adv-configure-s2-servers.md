---
title: Étape 2 configurer des serveurs DirectAccess avancés
description: Cette rubrique fait partie du guide déployer un serveur DirectAccess unique avec des paramètres avancés pour Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 35afec8e-39a4-463b-839a-3c300ab01174
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2c5fec6d9dafa350f46dfb5b2f213d628391b87f
ms.sourcegitcommit: 07c9d4ea72528401314e2789e3bc2e688fc96001
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76822782"
---
# <a name="step-2-configure-advanced-directaccess-servers"></a>Étape 2 configurer des serveurs DirectAccess avancés

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique décrit comment configurer les paramètres de client et de serveur requis pour un déploiement avancé de l'accès à distance qui utilise un serveur d'accès à distance unique dans un environnement mixte IPv4 et IPv6. Avant de commencer les étapes de déploiement, assurez-vous que vous avez effectué les étapes de planification décrites dans [planifier un déploiement avancé de DirectAccess](Plan-an-Advanced-DirectAccess-Deployment.md).  
  
|Tâche|Description|  
|----|--------|  
|2.1. Installer le rôle Accès à distance|Installez le rôle Accès à distance.|  
|2.2. Configurer le type de déploiement|Configurez le type de déploiement comme DirectAccess et VPN, DirectAccess uniquement ou VPN uniquement.|  
|[Planifier un déploiement avancé de DirectAccess](Plan-an-Advanced-DirectAccess-Deployment.md)|Configurez le serveur d'accès à distance avec les groupes de sécurité contenant les clients DirectAccess.|  
|2.4. Configurer le serveur d'accès à distance|Configurez les paramètres du serveur d'accès à distance|  
|2.5. Configurer les serveurs d'infrastructure|Configurez les serveurs d'infrastructure utilisés dans l'organisation.|  
|2.6. Configurer les serveurs d'applications|Configurez les serveurs d'applications afin qu'ils exigent une authentification et un chiffrement.|  
|2.7. Résumé de la configuration et autres objets de stratégie de groupe|Consultez le résumé de la configuration de l'accès à distance et modifiez les objets de stratégie de groupe, si vous le souhaitez.|  
|2.8. Comment configurer le serveur d'accès à distance au moyen de Windows PowerShell|Configurez l’accès à distance à l’aide de Windows PowerShell.|  
  
> [!NOTE]  
> Cette rubrique inclut des exemples d'applets de commande Windows PowerShell que vous pouvez utiliser pour automatiser certaines des procédures décrites. Pour plus d’informations, consultez [Utilisation des applets de commande](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="BKMK_Role"></a>2,1. Installer le rôle Accès à distance  
Pour déployer l'accès à distance, vous devez installer le rôle Accès à distance sur un serveur de votre organisation qui agira en tant que serveur d'accès à distance.  
  
#### <a name="to-install-the-remote-access-role"></a>Pour installer le rôle Accès à distance  
  
1.  Sur le serveur d’accès à distance, dans la console Gestionnaire de serveur, dans le **tableau de bord**, cliquez sur **Ajouter des rôles et des fonctionnalités**.  
  
2.  Cliquez sur **Suivant** trois fois pour accéder à l'écran **Sélectionner des rôles de serveurs**.  
  
3.  Dans la page **Sélectionner des rôles de serveurs**, sélectionnez **Accès à distance**, cliquez sur **Ajouter des fonctionnalités**, puis cliquez sur **Suivant**.  
  
4.  Cliquez sur **Suivant** cinq fois.  
  
5.  Dans la page **Confirmer les sélections d’installation** , cliquez sur **Installer**.  
  
6.  Dans la page **Progression de l'installation**, vérifiez que l'installation s'est correctement déroulée et cliquez sur **Fermer**.  
  
![de la progression de l’installation,](../../../media/Step-2-Configuring-DirectAccess-Servers/PowerShellLogoSmall.gif)***<em>commandes équivalentes Windows PowerShell</em>***  
  
L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.  
  
```  
Install-WindowsFeature RemoteAccess -IncludeManagementTools  
```  
  
## <a name="BKMK_Deploy"></a>2,2. Configurer le type de déploiement  
L'accès à distance peut être déployé à l'aide de la console de gestion de l'accès à distance de trois façons différentes :  
  
-   DirectAccess et VPN  
  
-   DirectAccess uniquement  
  
-   VPN uniquement  
  
Ce guide utilise un déploiement DirectAccess uniquement dans les procédures d'exemple.  
  
#### <a name="to-configure-the-deployment-type"></a>Pour configurer le type de déploiement  
  
1.  Sur le serveur d’accès à distance, ouvrez la console de gestion de l’accès à distance : dans l’écran d' **Accueil** , tapez**RAMgmtUI. exe**, puis appuyez sur entrée. Si la boîte de dialogue **Contrôle de compte d'utilisateur** s'affiche, vérifiez que l'action affichée est celle que vous voulez, puis cliquez sur **Oui**.  
  
2.  Dans la console de gestion de l'accès à distance, dans le volet du milieu, cliquez sur **Exécuter l'Assistant Configuration de l'accès à distance**.  
  
3.  Dans la boîte de dialogue **Configurer l'accès à distance**, cliquez pour choisir de déployer DirectAccess et VPN, DirectAccess uniquement ou VPN uniquement.  
  
## <a name="BKMK_Clients"></a>2,3. Configurer les clients DirectAccess  
Pour configurer l'utilisation de DirectAccess sur un ordinateur client, ce dernier doit appartenir au groupe de sécurité sélectionné. Une fois DirectAccess configuré, les ordinateurs clients du groupe de sécurité sont configurés pour recevoir l'objet de stratégie de groupe DirectAccess. Vous pouvez également configurer le scénario de déploiement, qui vous autorise à configurer DirectAccess pour l'accès client et l'administration à distance, ou seulement pour l'administration à distance.  
  
#### <a name="to-configure-directaccess-clients"></a>Pour configurer les clients DirectAccess  
  
1.  Dans le volet central de la console de gestion de l'accès à distance, dans la zone **Étape 1 : Clients distants**, cliquez sur **Modifier**.  
  
2.  Dans l'Assistant Installation du client DirectAccess, dans la page **Scénario de déploiement**, cliquez sur le scénario de déploiement que vous voulez utiliser dans votre organisation (**DirectAccess complet** ou **Administration à distance uniquement**), puis cliquez sur **Suivant**.  
  
3.  Dans la page **Sélectionner des groupes**, cliquez sur **Ajouter**.  
  
4.  Dans la boîte de dialogue **Sélectionner des groupes**, sélectionnez les groupes de sécurité qui contiennent vos ordinateurs clients DirectAccess.  
  
    > [!NOTE]  
    > Si le groupe de sécurité se trouve dans une forêt différente de celle du serveur d’accès à distance, une fois que vous avez terminé l’Assistant Configuration de l’accès à distance, cliquez sur **Actualiser les serveurs d’administration** dans le volet **tâches** pour découvrir les contrôleurs de domaine et les serveurs de Configuration Manager de la nouvelle forêt.  
  
5.  Cochez la case **Activer DirectAccess pour les ordinateurs portables uniquement** pour autoriser uniquement les ordinateurs portables à accéder au réseau interne, si nécessaire.  
  
6.  Cochez la case **Utiliser le tunneling forcé** pour acheminer tout le trafic client (vers le réseau interne et vers Internet) via le serveur d'accès à distance, si nécessaire.  
  
7.  Cliquez sur **Suivant**.  
  
8.  Dans la page **Assistant Connectivité réseau** :  
  
    -   Dans la table, ajoutez les ressources qui seront utilisées pour déterminer la connectivité au réseau interne. Une sonde web par défaut est créée automatiquement si aucune autre ressource n'est configurée.  
  
        > [!CAUTION]  
        > Quand vous configurez les emplacements de sonde web pour déterminer la connectivité au réseau d'entreprise, assurez-vous de disposer d'au moins une sonde HTTP configurée. La seule configuration d'une sonde **ping** n'est pas suffisante et pourrait conduire à une détermination inexacte de l'état de la connectivité. Ceci est dû au fait que la sonde **ping** est exemptée d'IPsec et, par conséquent, qu'elle ne garantit pas que les tunnels IPsec sont établis correctement.  
  
    -   Ajouter une adresse de messagerie du support technique pour permettre aux utilisateurs d'envoyer des informations s'ils rencontrent des problèmes de connectivité.  
  
    -   Fournissez un nom convivial pour la connexion à DirectAccess. Ce nom apparaît dans la liste des réseaux quand les utilisateurs cliquent sur l'icône de réseau dans la zone de notification.  
  
    -   Cochez la case **Permettre aux clients DirectAccess d'utiliser la résolution de noms locale**, si nécessaire.  
  
        > [!NOTE]  
        > Quand la résolution de noms locale est activée, les utilisateurs qui exécutent l'Assistant Connectivité réseau peuvent choisir de résoudre les noms en utilisant les serveurs DNS configurés sur l'ordinateur client DirectAccess.  
  
9. Cliquez sur **Terminer**.  
  
## <a name="BKMK_Server"></a>2,4. Configurer le serveur d'accès à distance  
Pour déployer l'accès à distance, vous devez configurer le serveur d'accès à distance avec les cartes réseau appropriées, une URL publique pour le serveur d'accès à distance auquel les ordinateurs clients peuvent se connecter (adresse ConnectTo), un certificat IP-HTTPS dont le sujet correspond à l'adresse ConnectTo, les paramètres IPv6 et l'authentification des ordinateurs clients.  
  
#### <a name="to-configure-the-remote-access-server"></a>Pour configurer le serveur d'accès à distance  
  
1.  Dans le volet central de la console de gestion de l'accès à distance, dans la zone **Étape 2 : Serveur d'accès à distance**, cliquez sur **Configurer**.  
  
2.  Dans l'Assistant Installation du serveur d'accès à distance, dans la page **Topologie du réseau**, cliquez sur la topologie de déploiement qui sera utilisée dans votre organisation. Dans **Tapez le nom public ou l'adresse IPv4 utilisé par les clients pour se connecter au serveur d'accès à distance**, entrez le nom public du déploiement (ce nom correspond au nom de sujet du certificat IP-HTTPS, par exemple : edge1.contoso.com), puis cliquez sur **Suivant**.  
  
3.  Dans la page **Cartes réseau**, l'Assistant détecte automatiquement les cartes réseau pour les réseaux présents dans votre déploiement. Si l'Assistant ne détecte pas les cartes réseau appropriées, sélectionnez manuellement les cartes correctes. L'Assistant détecte aussi automatiquement le certificat IP-HTTPS, basé sur le nom public du déploiement défini dans l'étape précédente de l'Assistant. Si l'Assistant ne détecte pas le certificat IP-HTTPS approprié, cliquez sur **Parcourir** pour sélectionner manuellement le certificat correct, puis cliquez sur **Suivant**.  
  
4.  Dans la page **Configuration du préfixe** (apparaît uniquement si IPv6 est déployé dans le réseau interne), l'Assistant détecte automatiquement les paramètres IPv6 utilisés dans le réseau interne. Si votre déploiement nécessite des préfixes supplémentaires, configurez les préfixes IPv6 pour le réseau interne, un préfixe IPv6 à attribuer aux ordinateurs clients DirectAccess et un préfixe IPv6 à attribuer aux ordinateurs clients du réseau VPN.  
  
    > [!NOTE]  
    > Vous pouvez spécifier plusieurs préfixes IPv6 internes en utilisant une liste séparée par des points-virgules, telle que 2001:db8:1::/48;2001:db8:2::/48.  
  
5.  Dans la page **Authentification** :  
  
    -   Dans **Authentification utilisateur**, cliquez sur **Informations d'identification Active Directory**. Pour configurer un déploiement en utilisant l'authentification à deux facteurs, cliquez sur **Authentification à 2 facteurs**. Pour plus d'informations, voir [Déployer l'accès à distance avec l'authentification par mot de passe à usage unique](https://technet.microsoft.com/library/hh831379.aspx).  
  
    -   Pour les déploiements multisites et à authentification à 2 facteurs, vous devez utiliser l'authentification par certificat d'ordinateur. Cochez la case **Utiliser les certificats d'ordinateur** pour utiliser l'authentification par certificat d'ordinateur, et sélectionnez le certificat racine IPsec.  
  
    -   Pour autoriser les ordinateurs clients Windows 7 à se connecter via DirectAccess, activez la case à cocher **autoriser les ordinateurs clients Windows 7 à se connecter via DirectAccess** .  
  
        > [!NOTE]  
        > Vous devez également utiliser l'authentification par certificat d'ordinateur dans ce type de déploiement.  
  
6.  Cliquez sur **Terminer**.  
  
## <a name="BKMK_Infra"></a>2,5. Configurer les serveurs d'infrastructure  
Pour configurer les serveurs d'infrastructure dans un déploiement de l'accès à distance, vous devez configurer le serveur Emplacement réseau, les paramètres DNS (y compris la liste de recherche de suffixes DNS) et les serveurs d'administration qui ne sont pas détectés automatiquement par l'accès à distance.  
  
#### <a name="to-configure-the-infrastructure-servers"></a>Pour configurer les serveurs d'infrastructure  
  
1.  Dans le volet central de la console de gestion de l'accès à distance, dans la zone **Étape 3 : Serveurs d'infrastructure**, cliquez sur **Configurer**.  
  
2.  Dans l'Assistant Installation du serveur d'infrastructure, dans la page **Serveur Emplacement réseau**, cliquez sur l'option correspondant à l'emplacement du serveur Emplacement réseau dans votre déploiement. Si le serveur Emplacement réseau figure sur un serveur web distant, entrez l'URL et cliquez sur **Valider** avant de continuer. Si le serveur Emplacement réseau se trouve sur le serveur d'accès à distance, cliquez sur **Parcourir** pour localiser le certificat approprié, puis cliquez sur **Suivant**.  
  
3.  Dans la page **DNS**, dans la table, entrez tous les suffixes de noms supplémentaires qui seront appliqués en tant qu'exemptions de table de stratégie de résolution de noms. Sélectionnez une option de résolution de noms locale, puis cliquez sur **Suivant**.  
  
4.  Dans la page **Liste de recherche de suffixes DNS**, le serveur d'accès à distance détecte automatiquement tous les suffixes de domaine dans le déploiement. Utilisez les boutons **Ajouter** et **Supprimer** pour ajouter et supprimer des suffixes de domaine dans la liste des suffixes de domaine à utiliser. Pour ajouter un nouveau suffixe de domaine, dans **Nouveau suffixe**, entrez le suffixe, puis cliquez sur **Ajouter**. Cliquez sur **Suivant**.  
  
5.  Dans la page **Gestion**, ajoutez tous les serveurs d'administration qui ne sont pas détectés automatiquement, puis cliquez sur **Suivant**. L’accès à distance ajoute automatiquement des contrôleurs de domaine et des serveurs de Configuration Manager.  
  
    > [!NOTE]  
    > Bien que les serveurs soient ajoutés automatiquement, ils n’apparaissent pas dans la liste. Une fois la configuration appliquée pour la première fois, les serveurs de Configuration Manager s’affichent dans la liste.  
  
6.  Cliquez sur **Terminer**.  
  
## <a name="BKMK_App"></a>2,6. Configurer les serveurs d'applications  
Dans un déploiement de l'accès à distance, la configuration de serveurs d'applications est une tâche facultative. L'accès à distance vous permet d'exiger une authentification pour les serveurs d'applications sélectionnés, laquelle est déterminée par leur appartenance à un groupe de sécurité de serveurs d'applications. Par défaut, le trafic destiné aux serveurs d'applications qui exigent une authentification est également chiffré. Toutefois, vous pouvez choisir de ne pas chiffrer le trafic destiné aux serveurs d'applications et d'utiliser l'authentification uniquement.  
  
> [!NOTE]  
> L’authentification sans chiffrement est prise en charge uniquement sur les serveurs d’applications exécutant Windows Server 2012 R2, Windows Server 2012 ou Windows Server 2008 R2.  
  
#### <a name="to-configure-application-servers"></a>Pour configurer les serveurs d'applications  
  
1.  Dans le volet central de la console de gestion de l'accès à distance, dans la zone **Étape 4 : Serveurs d'applications**, cliquez sur **Configurer**.  
  
2.  Dans l'Assistant Installation des serveurs d'applications DirectAccess, pour exiger l'authentification auprès des serveurs d'applications sélectionnés, cliquez sur **Étendre l'authentification aux serveurs d'applications sélectionnés**. Cliquez sur **Ajouter** pour sélectionner le groupe de sécurité des serveurs d'applications.  
  
3.  Pour limiter l'accès aux seuls serveurs figurant dans le groupe de sécurité des serveurs d'applications, cochez la case **Autoriser l'accès uniquement aux serveurs inclus dans les groupes de sécurité**.  
  
4.  Pour utiliser l’authentification sans chiffrement, sélectionnez l’option **ne pas chiffrer le trafic. Case à cocher Utiliser uniquement l’authentification** .  
  
5.  Cliquez sur **Terminer**.  
  
## <a name="BKMK_GPO"></a>2,7. Résumé de la configuration et autres objets de stratégie de groupe  
Une fois la configuration de l'accès à distance terminée, la **Vérification de l'accès DirectAccess** est affichée. Vous pouvez passer en revue tous les paramètres que vous avez sélectionnés auparavant, y compris :  
  
1.  **Paramètres des objets Stratégie de groupe** : Le nom d’objet de stratégie de groupe du serveur DirectAccess et le nom d’objet de stratégie de groupe du client sont répertoriés. De plus, vous pouvez cliquer sur le lien **Modifier** en regard du titre **Paramètres de l'objet de stratégie de groupe** pour modifier les paramètres des objets de stratégie de groupe.  
  
2.  **Clients distants** : La configuration du client DirectAccess est affichée, notamment le groupe de sécurité, l'état de tunneling forcé, les vérificateurs de connectivité et le nom de la connexion à DirectAccess.  
  
3.  **Serveur d’accès à distance** : La configuration de DirectAccess est affichée avec le nom public/l’adresse, la configuration des cartes réseau, les informations de certificat et les informations de mot de passe à usage unique, le cas échéant.  
  
4.  **Serveurs d’infrastructure** : Cette liste inclut l'URL du serveur d’emplacement réseau, les suffixes DNS utilisés par les clients DirectAccess et des informations sur les serveurs d'administration.  
  
5.  **Serveurs d’application** : L'état de l’administration à distance DirectAccess est affiché, en plus de l’état de l’authentification de bout en bout auprès des serveurs d’applications spécifiques.  
  
## <a name="BKMK_PS"></a>2,8. Comment configurer le serveur d'accès à distance au moyen de Windows PowerShell  
![les commandes Windows PowerShell](../../../media/Step-2-Configuring-DirectAccess-Servers/PowerShellLogoSmall.gif)**équivalentes** Windows PowerShell  
  
L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.  
  
Pour effectuer une installation complète dans une topologie de périmètre de l’accès à distance pour DirectAccess uniquement dans un domaine avec la racine **Corp.contoso.com** et en utilisant les paramètres suivants : serveur de stratégie de groupe : **paramètres de serveur DirectAccess**, objet de stratégie de groupe du client : paramètres du client DirectAccess, carte réseau interne : **corpnet**, carte réseau externe : **Internet**, adresse ConnectTto : **Edge1.contoso.com**et serveur emplacement réseau : **nls.Corp.contoso.com**:  
  
```  
Install-RemoteAccess -Force -PassThru -ServerGpoName 'corp.contoso.com\DirectAccess Server Settings' -ClientGpoName 'corp.contoso.com\DirectAccess Client Settings' -DAInstallType 'FullInstall' -InternetInterface 'Internet' -InternalInterface 'Corpnet' -ConnectToAddress 'edge1.contoso.com' -NlsUrl 'https://nls.corp.contoso.com/'  
```  
  
Pour configurer le serveur d'accès à distance afin qu'il utilise l'authentification par certificat d'ordinateur, avec un certificat racine IPsec émis par l'autorité de certification nommée CORP-APP1-CA :  
  
```  
$certs = Get-ChildItem Cert:\LocalMachine\Root  
$IPsecRootCert = $certs | Where-Object {$_.Subject -Match "corp-APP1-CA"}  
Set-DAServer -IPsecRootCertificate $IPsecRootCert  
```  
  
Pour ajouter le groupe de sécurité contenant les clients DirectAccess nommés **DirectAccessClients** et pour supprimer le groupe de sécurité par défaut Ordinateurs du domaine :  
  
```  
Add-DAClient -SecurityGroupNameList @('corp.contoso.com\DirectAccessClients')  
Remove-DAClient -SecurityGroupNameList @('corp.contoso.com\Domain Computers')  
```  
  
Pour activer l’accès à distance pour tous les ordinateurs (et non uniquement les ordinateurs portables et les ordinateurs portables) et pour activer l’accès à distance pour les clients Windows 7 :  
  
```  
Set-DAClient -OnlyRemoteComputers 'Disabled' -Downlevel 'Enabled'  
```  
  
Pour configurer l'expérience client DirectAccess, y compris le nom convivial de connexion et l'URL de sonde web :  
  
```  
Set-DAClientExperienceConfiguration -FriendlyName 'Contoso DirectAccess Connection' -PreferLocalNamesAllowed $False -PolicyStore 'corp.contoso.com\DirectAccess Client Settings' -CorporateResources @('HTTP:https://directaccess-WebProbeHost.corp.contoso.com')  
```  
  
## <a name="BKMK_Links"></a>Étape précédente  
  
-   [Étape 1 : configurer l’infrastructure DirectAccess avancée](da-adv-configure-s1-infrastructure.md)  
  
## <a name="next-step"></a>Étape suivante  
  
-   [Étape 3 : vérifier le déploiement](Step-3-Verify-the-Deployment.md)  
  


