---
title: Étape 2 configurer le serveur d’accès à distance
description: Cette rubrique fait partie du guide gérer des clients DirectAccess à distance dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: c0257b98-5633-4264-9df6-b6ffae80592c
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 7fce9eff4376e2d292622fe3a29fc8b391ad3bab
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859182"
---
# <a name="step-2-configure-the-remote-access-server"></a>Étape 2 configurer le serveur d’accès à distance

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique décrit comment configurer les paramètres client et serveur requis pour la gestion à distance des clients DirectAccess. Avant de commencer les étapes de déploiement, assurez-vous que vous avez effectué les étapes de planification décrites dans [étape 2 planifier le déploiement de l’accès à distance](../plan/Step-2-Plan-the-Remote-Access-Deployment.md).  
  
|Tâche|Description|  
|----|--------|  
|Installer le rôle Accès à distance|Installez le rôle Accès à distance.|  
|Configurer le type de déploiement|Configurez le type de déploiement comme DirectAccess et VPN, DirectAccess uniquement ou VPN uniquement.|  
|Configurer les clients DirectAccess|Configurez le serveur d'accès à distance avec les groupes de sécurité contenant les clients DirectAccess.|  
|Configurer le serveur d'accès à distance|Configurez les paramètres du serveur d’accès à distance.|  
|Configurer les serveurs d'infrastructure|Configurez les serveurs d'infrastructure utilisés dans l'organisation.|  
|Configurer les serveurs d'applications|Configurez les serveurs d’applications pour exiger l’authentification et le chiffrement.|  
|Résumé de la configuration et autres objets de stratégie de groupe|Consultez le résumé de la configuration de l'accès à distance et modifiez les objets de stratégie de groupe, si vous le souhaitez.|  
  
> [!NOTE]  
> Cette rubrique comprend des exemples d'applets de commande Windows PowerShell que vous pouvez utiliser pour automatiser certaines des procédures décrites. Pour plus d’informations, consultez [Utilisation des applets de commande](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="install-the-remote-access-role"></a><a name="BKMK_Role"></a>Installer le rôle accès à distance  
Vous devez installer le rôle accès à distance sur un serveur de votre organisation qui agira en tant que serveur d’accès à distance.  
  
#### <a name="to-install-the-remote-access-role"></a>Pour installer le rôle Accès à distance  
  
### <a name="to-install-the-remote-access-role-on-directaccess-servers"></a>Pour installer le rôle accès à distance sur les serveurs DirectAccess  
  
1.  Sur le serveur DirectAccess, dans la console Gestionnaire de serveur, dans le **tableau de bord**, cliquez sur **Ajouter des rôles et des fonctionnalités**.  
  
2.  Cliquez sur **Suivant** trois fois pour accéder à l’écran de sélection du rôle de serveur.  
  
3.  Dans la boîte de dialogue **Sélectionner des rôles de serveurs** , sélectionnez **accès à distance**, puis cliquez sur **suivant**.  
  
4.  Cliquez sur **suivant** trois fois.  
  
5.  Dans la boîte de dialogue **Sélectionner des services de rôle** , sélectionnez **DirectAccess et VPN (RAS)** , puis cliquez sur **Ajouter des fonctionnalités**.  
  
6.  Sélectionnez **routage**, sélectionnez **proxy d’application Web**, cliquez sur **Ajouter des fonctionnalités**, puis cliquez sur **suivant**.  
  
7. Cliquez sur **Suivant**, puis sur **Installer**.  
  
8.  Dans la boîte de dialogue **Progression de l'installation**, vérifiez que l'installation a réussi, puis cliquez sur **Fermer**.  
  
![les commandes Windows PowerShell](../../../../media/Step-2-Configure-the-Remote-Access-Server/PowerShellLogoSmall.gif)***<em>équivalentes</em> Windows PowerShell***  
  
La ou les applets de commande Windows PowerShell suivantes ont la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles apparaissent ici sur plusieurs lignes en raison de contraintes de mise en forme.  
  
```  
Install-WindowsFeature RemoteAccess -IncludeManagementTools  
```  
  
## <a name="configure-the-deployment-type"></a><a name="BKMK_Deploy"></a>Configurer le type de déploiement  
Il existe trois options que vous pouvez utiliser pour déployer l’accès à distance à partir de la console de gestion de l’accès à distance :  
  
-   DirectAccess et VPN  
  
-   DirectAccess uniquement  
  
-   VPN uniquement  
  
> [!NOTE]  
> Ce guide utilise la méthode de déploiement DirectAccess uniquement dans les exemples de procédures.  
  
#### <a name="to-configure-the-deployment-type"></a>Pour configurer le type de déploiement  
  
1.  Sur le serveur d’accès à distance, ouvrez la console de gestion de l’accès à distance : dans l’écran **Démarrer** , tapez, tapez **console de gestion de l’accès à distance**, puis appuyez sur entrée. Si la boîte de dialogue **Contrôle de compte d'utilisateur** apparaît, confirmez que l'action affichée est bien celle que vous souhaitez effectuer, puis cliquez sur **Oui**.  
  
2.  Dans la console de gestion de l'accès à distance, dans le volet du milieu, cliquez sur **Exécuter l'Assistant Configuration de l'accès à distance**.  
  
3.  Dans la boîte de dialogue **configurer l’accès à distance** , sélectionnez DIRECTACCESS et VPN, DirectAccess uniquement ou VPN uniquement.  
  
## <a name="configure-directaccess-clients"></a><a name="BKMK_Clients"></a>Configurer les clients DirectAccess  
Pour configurer l'utilisation de DirectAccess sur un ordinateur client, ce dernier doit appartenir au groupe de sécurité sélectionné. Une fois DirectAccess configuré, les ordinateurs clients du groupe de sécurité sont approvisionnés pour recevoir les objets de stratégie de groupe DirectAccess pour la gestion à distance.  
  
#### <a name="to-configure-directaccess-clients"></a>Pour configurer les clients DirectAccess  
  
1.  Dans le volet central de la console de gestion de l'accès à distance, dans la zone **Étape 1 : Clients distants**, cliquez sur **Modifier**.  
  
2.  Dans l’Assistant Installation du client DirectAccess, sur la page **scénario de déploiement** , cliquez sur **déployer DirectAccess uniquement pour la gestion à distance**, puis cliquez sur **suivant**.  
  
3.  Dans la page **Sélectionner des groupes**, cliquez sur **Ajouter**.  
  
4.  Dans la boîte de dialogue **Sélectionner des groupes** , sélectionnez les groupes de sécurité qui contiennent les ordinateurs clients DirectAccess, puis cliquez sur **suivant**.  
  
5.  Dans la page **Assistant Connectivité réseau** :  
  
    -   Dans le tableau, ajoutez les ressources qui seront utilisées pour déterminer la connectivité au réseau interne. Une sonde web par défaut est créée automatiquement si aucune autre ressource n'est configurée. Quand vous configurez les emplacements de sonde Web pour déterminer la connectivité au réseau d’entreprise, assurez-vous qu’au moins une sonde HTTP est configurée. La configuration d’une seule sonde ping n’est pas suffisante et peut entraîner une détermination inexacte de l’état de la connectivité. Cela est dû au fait que le test ping est exempté d’IPsec. Par conséquent, la commande ping ne garantit pas que les tunnels IPsec sont correctement établis.  
  
    -   Ajouter une adresse de messagerie du support technique pour permettre aux utilisateurs d'envoyer des informations s'ils rencontrent des problèmes de connectivité.  
  
    -   Fournissez un nom convivial pour la connexion à DirectAccess.  
  
    -   Cochez la case **Permettre aux clients DirectAccess d'utiliser la résolution de noms locale**, si nécessaire.  
  
        > [!NOTE]  
        > Lorsque la résolution de noms locale est activée, les utilisateurs qui exécutent l’Assistant NCA peuvent résoudre les noms à l’aide de serveurs DNS configurés sur l’ordinateur client DirectAccess.  
  
6.  Cliquez sur **Terminer**.  
  
## <a name="configure-the-remote-access-server"></a><a name="BKMK_Server"></a>Configurer le serveur d’accès à distance  
Pour déployer l’accès à distance, vous devez configurer le serveur qui agira en tant que serveur d’accès à distance avec les éléments suivants :  
  
1.  Corriger les cartes réseau  
  
2.  Une URL publique pour le serveur d’accès à distance auquel les ordinateurs clients peuvent se connecter (adresse ConnectTo)  
  
3.  Un certificat IP-HTTPs avec un objet qui correspond à l’adresse ConnectTo  
  
4.  Paramètres IPv6  
  
5.  Authentification de l’ordinateur client  
  
#### <a name="to-configure-the-remote-access-server"></a>Pour configurer le serveur d'accès à distance  
  
1.  Dans le volet central de la console de gestion de l'accès à distance, dans la zone **Étape 2 : Serveur d'accès à distance**, cliquez sur **Configurer**.  
  
2.  Dans l'Assistant Installation du serveur d'accès à distance, dans la page **Topologie du réseau**, cliquez sur la topologie de déploiement qui sera utilisée dans votre organisation. Dans **Tapez le nom public ou l'adresse IPv4 utilisé par les clients pour se connecter au serveur d'accès à distance**, entrez le nom public du déploiement (ce nom correspond au nom de sujet du certificat IP-HTTPS, par exemple : edge1.contoso.com), puis cliquez sur **Suivant**.  
  
3.  Dans la page **cartes réseau** , l’Assistant détecte automatiquement les éléments suivants :  
  
    -   Cartes réseau pour les réseaux de votre déploiement. Si l'Assistant ne détecte pas les cartes réseau appropriées, sélectionnez manuellement les cartes correctes.  
  
    -   Certificat IP-HTTPs. Cela est basé sur le nom public du déploiement que vous avez défini lors de l’étape précédente de l’Assistant. Si l’Assistant ne détecte pas le certificat IP-HTTPs correct, cliquez sur **Parcourir** pour sélectionner manuellement le certificat approprié.  
  
4.  Cliquez sur **Suivant**.  
  
5.  Dans la page **configuration du préfixe** (cette page est visible uniquement si IPv6 est détecté sur le réseau interne), l’Assistant détecte automatiquement les paramètres IPv6 qui sont utilisés sur le réseau interne. Si votre déploiement nécessite des préfixes supplémentaires, configurez les préfixes IPv6 pour le réseau interne, un préfixe IPv6 à attribuer aux ordinateurs clients DirectAccess et un préfixe IPv6 à attribuer aux ordinateurs clients du réseau VPN.  
  
6.  Dans la page **Authentification** :  
  
    -   Pour les déploiements multisites et à authentification à 2 facteurs, vous devez utiliser l'authentification par certificat d'ordinateur. Activez la case à cocher **utiliser des certificats d’ordinateur** pour utiliser l’authentification par certificat d’ordinateur et sélectionnez le certificat racine IPSec.  
  
    -   Pour permettre aux ordinateurs clients exécutant Windows 7 de se connecter via DirectAccess, activez la case à cocher **autoriser les ordinateurs clients Windows 7 à se connecter via DirectAccess** . Vous devez également utiliser l'authentification par certificat d'ordinateur dans ce type de déploiement.  
  
7.  Cliquez sur **Terminer**.  
  
## <a name="configure-the-infrastructure-servers"></a><a name="BKMK_Infra"></a>Configurer les serveurs d’infrastructure  
Pour configurer les serveurs d’infrastructure dans un déploiement de l’accès à distance, vous devez configurer les éléments suivants :  
  
-   Serveur Emplacement réseau  
  
-   Paramètres DNS, y compris la liste de recherche de suffixes DNS  
  
-   Tous les serveurs d’administration qui ne sont pas détectés automatiquement par l’accès à distance  
  
#### <a name="to-configure-the-infrastructure-servers"></a>Pour configurer les serveurs d'infrastructure  
  
1.  Dans le volet central de la console de gestion de l'accès à distance, dans la zone **Étape 3 : Serveurs d'infrastructure**, cliquez sur **Configurer**.  
  
2.  Dans l'Assistant Installation du serveur d'infrastructure, dans la page **Serveur Emplacement réseau**, cliquez sur l'option correspondant à l'emplacement du serveur Emplacement réseau dans votre déploiement.  
  
    -   Si le serveur emplacement réseau se trouve sur un serveur Web distant, entrez l’URL, puis cliquez sur **valider** avant de continuer.  
  
    -   Si le serveur Emplacement réseau se trouve sur le serveur d'accès à distance, cliquez sur **Parcourir** pour localiser le certificat approprié, puis cliquez sur **Suivant**.  
  
3.  Dans la page **DNS** , dans la table, entrez des suffixes de noms supplémentaires qui seront appliqués comme exemptions de la table de stratégie de résolution de noms (NRPT). Sélectionnez une option de résolution de noms locale, puis cliquez sur **Suivant**.  
  
4.  Dans la page **liste de recherche de suffixes DNS** , le serveur d’accès à distance détecte automatiquement les suffixes de domaine dans le déploiement. Utilisez les boutons **Ajouter** et **supprimer** pour créer la liste des suffixes de domaine que vous souhaitez utiliser. Pour ajouter un nouveau suffixe de domaine, dans **Nouveau suffixe**, entrez le suffixe, puis cliquez sur **Ajouter**. Cliquez sur **Suivant**.  
  
5.  Sur la page de **gestion** , ajoutez des serveurs d’administration qui ne sont pas détectés automatiquement, puis cliquez sur **suivant**. L’accès à distance ajoute automatiquement des contrôleurs de domaine et des serveurs de Configuration Manager.  
  
6.  Cliquez sur **Terminer**.  
  
## <a name="configure-application-servers"></a><a name="BKMK_App"></a>Configurer les serveurs d’applications  
Dans un déploiement d’accès à distance complet, la configuration des serveurs d’applications est une tâche facultative. Dans ce scénario de gestion à distance des clients DirectAccess, les serveurs d’applications ne sont pas utilisés et cette étape est grisée pour indiquer qu’elle n’est pas active. Cliquez sur **Terminer** pour appliquer la configuration.  
  
## <a name="configuration-summary-and-alternate-gpos"></a><a name="BKMK_GPO"></a>Résumé de la configuration et autres objets de stratégie de groupe  
Une fois la configuration de l'accès à distance terminée, la **Vérification de l'accès DirectAccess** est affichée. Vous pouvez passer en revue tous les paramètres que vous avez sélectionnés auparavant, y compris :  
  
-   **Paramètres d’objet de stratégie de groupe**  
  
    Le nom de l’objet de stratégie de groupe du serveur DirectAccess et le nom de l’objet de stratégie de groupe client Vous pouvez cliquer sur le lien **modifier** en regard du titre paramètres de l' **objet de stratégie de groupe** pour modifier les paramètres de l’objet de stratégie de groupe.  
  
-   **Clients distants**  
  
    La configuration du client DirectAccess s’affiche, notamment le groupe de sécurité, les vérificateurs de connectivité et le nom de la connexion DirectAccess.  
  
-   **Serveur d’accès à distance**  
  
    La configuration DirectAccess s’affiche, y compris le nom et l’adresse publics, la configuration de la carte réseau et les informations de certificat.  
  
-   **Serveurs d’infrastructure**  
  
    cette liste inclut l'URL du serveur Emplacement réseau, les suffixes DNS utilisés par les clients DirectAccess et des informations sur les serveurs d'administration.  
  
## <a name="see-also"></a><a name="BKMK_Links"></a>Voir aussi  
  
-   [Étape 3 : vérifier le déploiement](Step-3-Verify-the-Deployment_2.md)  
  
  


