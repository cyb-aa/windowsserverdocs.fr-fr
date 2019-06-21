---
title: Étape 2 configurer le serveur d’accès à distance
description: Cette rubrique fait partie du guide les Clients DirectAccess de gérer à distance dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c0257b98-5633-4264-9df6-b6ffae80592c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f60373a24663c73c537e747d5993e60fa2a38972
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282817"
---
# <a name="step-2-configure-the-remote-access-server"></a>Étape 2 configurer le serveur d’accès à distance

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique décrit comment configurer les paramètres client et serveur sont requis pour la gestion à distance des clients DirectAccess. Avant de commencer les étapes de déploiement, assurez-vous que vous avez effectué les étapes de planification sont décrites dans [étape 2 planifier le déploiement de l’accès à distance](../plan/Step-2-Plan-the-Remote-Access-Deployment.md).  
  
|Tâche|Description|  
|----|--------|  
|Installer le rôle Accès à distance|Installez le rôle Accès à distance.|  
|Configurer le type de déploiement|Configurez le type de déploiement comme DirectAccess et VPN, DirectAccess uniquement ou VPN uniquement.|  
|Configurer les clients DirectAccess|Configurez le serveur d'accès à distance avec les groupes de sécurité contenant les clients DirectAccess.|  
|Configurer le serveur d'accès à distance|Configurez les paramètres de serveur d’accès à distance.|  
|Configurer les serveurs d'infrastructure|Configurez les serveurs d'infrastructure utilisés dans l'organisation.|  
|Configurer les serveurs d'applications|Configurer les serveurs d’applications pour exiger l’authentification et le chiffrement.|  
|Résumé de la configuration et autres objets de stratégie de groupe|Consultez le résumé de la configuration de l'accès à distance et modifiez les objets de stratégie de groupe, si vous le souhaitez.|  
  
> [!NOTE]  
> Cette rubrique inclut des exemples d'applets de commande Windows PowerShell que vous pouvez utiliser pour automatiser certaines des procédures décrites. Pour plus d’informations, consultez [Utilisation des applets de commande](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="BKMK_Role"></a>Installer le rôle accès à distance  
Vous devez installer le rôle accès à distance sur un serveur de votre organisation qui agira en tant que le serveur d’accès à distance.  
  
#### <a name="to-install-the-remote-access-role"></a>Pour installer le rôle Accès à distance  
  
### <a name="to-install-the-remote-access-role-on-directaccess-servers"></a>Pour installer le rôle accès à distance sur les serveurs DirectAccess  
  
1.  Sur le serveur DirectAccess, dans la console Gestionnaire de serveur, dans le **tableau de bord**, cliquez sur **ajouter des rôles et fonctionnalités**.  
  
2.  Cliquez sur **Suivant** trois fois pour accéder à l’écran de sélection du rôle de serveur.  
  
3.  Sur le **sélectionner des rôles de serveur** boîte de dialogue, sélectionnez **accès à distance**, puis cliquez sur **suivant**.  
  
4.  Cliquez sur **suivant** trois fois.  
  
5.  Sur le **sélectionnez services de rôle** boîte de dialogue, sélectionnez **DirectAccess et VPN (RAS)** puis cliquez sur **ajouter des fonctionnalités**.  
  
6.  Sélectionnez **routage**, sélectionnez **Proxy d’Application Web**, cliquez sur **ajouter des fonctionnalités**, puis cliquez sur **suivant**.  
  
7. Cliquez sur **Suivant**, puis cliquez sur **Installer**.  
  
8.  Dans la boîte de dialogue **Progression de l’installation** , vérifiez que l’installation s’est correctement déroulée et cliquez sur **Fermer**.  
  
![Windows PowerShell](../../../../media/Step-2-Configure-the-Remote-Access-Server/PowerShellLogoSmall.gif)***<em>commandes Windows PowerShell équivalentes</em>***  
  
L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.  
  
```  
Install-WindowsFeature RemoteAccess -IncludeManagementTools  
```  
  
## <a name="BKMK_Deploy"></a>Configurer le type de déploiement  
Il existe trois options que vous pouvez utiliser pour déployer l’accès à distance à partir de la console de gestion de l’accès à distance :  
  
-   DirectAccess et VPN  
  
-   DirectAccess uniquement  
  
-   VPN uniquement  
  
> [!NOTE]  
> Ce guide utilise la seule méthode de DirectAccess de déploiement dans les procédures d’exemple.  
  
#### <a name="to-configure-the-deployment-type"></a>Pour configurer le type de déploiement  
  
1.  Sur le serveur d'accès à distance, ouvrez la console de gestion de l'accès à distance : Sur le **Démarrer** écran, type, type **Console de gestion des accès à distance**, puis appuyez sur ENTRÉE. Si la boîte de dialogue **Contrôle de compte d'utilisateur** s'affiche, vérifiez que l'action affichée est celle que vous voulez, puis cliquez sur **Oui**.  
  
2.  Dans la console de gestion de l'accès à distance, dans le volet du milieu, cliquez sur **Exécuter l'Assistant Configuration de l'accès à distance**.  
  
3.  Dans le **configurer l’accès à distance** boîte de dialogue, sélectionnez DirectAccess et VPN, DirectAccess uniquement ou VPN uniquement.  
  
## <a name="BKMK_Clients"></a>Configurer des clients DirectAccess  
Pour configurer l'utilisation de DirectAccess sur un ordinateur client, ce dernier doit appartenir au groupe de sécurité sélectionné. Une fois que DirectAccess est configuré, les ordinateurs clients dans le groupe de sécurité sont configurés pour recevoir les objets de stratégie de groupe (GPO) DirectAccess pour la gestion à distance.  
  
#### <a name="to-configure-directaccess-clients"></a>Pour configurer les clients DirectAccess  
  
1.  Dans le volet central de la console de gestion de l'accès à distance, dans la zone **Étape 1 : Clients distants**, cliquez sur **Modifier**.  
  
2.  Dans l’Assistant Installation du Client DirectAccess, sur le **scénario de déploiement** , cliquez sur **déployer de DirectAccess pour la gestion à distance uniquement**, puis cliquez sur **suivant**.  
  
3.  Dans la page **Sélectionner des groupes**, cliquez sur **Ajouter**.  
  
4.  Dans le **sélectionner des groupes** boîte de dialogue, sélectionnez les groupes de sécurité qui contiennent les ordinateurs clients DirectAccess, puis cliquez sur **suivant**.  
  
5.  Dans la page **Assistant Connectivité réseau** :  
  
    -   Dans la table, ajoutez les ressources qui seront utilisés pour déterminer la connectivité au réseau interne. Une sonde web par défaut est créée automatiquement si aucune autre ressource n'est configurée. Lorsque vous configurez les emplacements de sonde web pour déterminer la connectivité au réseau d’entreprise, assurez-vous d’avoir au moins une sonde HTTP basé configurée. La configuration uniquement un sondage ping n’est pas suffisant, et elle peut aboutir à une détermination inexacte de l’état de connectivité. Il s’agit, car la commande ping est exempté d’IPsec. Par conséquent, ping ne garantit pas que les tunnels IPsec sont établis correctement.  
  
    -   Ajouter une adresse de messagerie du support technique pour permettre aux utilisateurs d'envoyer des informations s'ils rencontrent des problèmes de connectivité.  
  
    -   Fournissez un nom convivial pour la connexion à DirectAccess.  
  
    -   Cochez la case **Permettre aux clients DirectAccess d'utiliser la résolution de noms locale**, si nécessaire.  
  
        > [!NOTE]  
        > Lors de la résolution de noms locale est activée, les utilisateurs qui exécutent l’Assistant NCA peuvent résoudre des noms à l’aide de serveurs DNS qui sont configurés sur l’ordinateur client DirectAccess.  
  
6.  Cliquez sur **Terminer**.  
  
## <a name="BKMK_Server"></a>Configurer le serveur d’accès à distance  
Pour déployer l’accès à distance, vous devez configurer le serveur qui agira en tant que le serveur d’accès à distance avec les éléments suivants :  
  
1.  Cartes réseau appropriées  
  
2.  Une URL publique pour le serveur d’accès à distance client auquel les ordinateurs peuvent se connecter (l’adresse ConnectTo)  
  
3.  Un certificat IP-HTTPS avec un objet qui correspond à l’adresse ConnectTo  
  
4.  Paramètres IPv6  
  
5.  Authentification d’ordinateur client  
  
#### <a name="to-configure-the-remote-access-server"></a>Pour configurer le serveur d'accès à distance  
  
1.  Dans le volet central de la console de gestion de l'accès à distance, dans la zone **Étape 2 : Serveur d'accès à distance**, cliquez sur **Configurer**.  
  
2.  Dans l'Assistant Installation du serveur d'accès à distance, dans la page **Topologie du réseau**, cliquez sur la topologie de déploiement qui sera utilisée dans votre organisation. Dans **Tapez le nom public ou l'adresse IPv4 utilisé par les clients pour se connecter au serveur d'accès à distance**, entrez le nom public du déploiement (ce nom correspond au nom de sujet du certificat IP-HTTPS, par exemple : edge1.contoso.com), puis cliquez sur **Suivant**.  
  
3.  Sur le **cartes réseau** page, l’Assistant détecte automatiquement :  
  
    -   Cartes réseau pour les réseaux dans votre déploiement. Si l'Assistant ne détecte pas les cartes réseau appropriées, sélectionnez manuellement les cartes correctes.  
  
    -   Certificat IP-HTTPS. Cela est basé sur le nom public pour le déploiement que vous définissez lors de l’étape précédente de l’Assistant. Si l’Assistant ne détecte pas le certificat IP-HTTPS approprié, cliquez sur **Parcourir** afin de sélectionner manuellement le certificat approprié.  
  
4.  Cliquez sur **Suivant**.  
  
5.  Sur le **Configuration du préfixe** page (cette page est visible uniquement si IPv6 est détecté dans le réseau interne), l’Assistant détecte automatiquement les paramètres IPv6 qui sont utilisés sur le réseau interne. Si votre déploiement nécessite des préfixes supplémentaires, configurez les préfixes IPv6 pour le réseau interne, un préfixe IPv6 à attribuer aux ordinateurs clients DirectAccess et un préfixe IPv6 à attribuer aux ordinateurs clients du réseau VPN.  
  
6.  Dans la page **Authentification** :  
  
    -   Pour les déploiements multisites et à authentification à 2 facteurs, vous devez utiliser l'authentification par certificat d'ordinateur. Sélectionnez le **utilisent des certificats d’ordinateur** case à cocher pour utiliser l’authentification de certificat d’ordinateur et sélectionnez le certificat racine IPsec.  
  
    -   Pour activer les ordinateurs clients qui exécutent Windows 7 se connecter via DirectAccess, sélectionnez le **autoriser Windows client les ordinateurs 7 à se connecter via DirectAccess** case à cocher. Vous devez également utiliser l'authentification par certificat d'ordinateur dans ce type de déploiement.  
  
7.  Cliquez sur **Terminer**.  
  
## <a name="BKMK_Infra"></a>Configurer les serveurs d’infrastructure  
Pour configurer les serveurs d’infrastructure dans un déploiement de l’accès à distance, vous devez configurer les éléments suivants :  
  
-   Serveur Emplacement réseau  
  
-   Liste de recherche de suffixe de paramètres DNS, y compris le DNS  
  
-   Tous les serveurs de gestion qui ne sont pas détectés automatiquement par accès à distance  
  
#### <a name="to-configure-the-infrastructure-servers"></a>Pour configurer les serveurs d'infrastructure  
  
1.  Dans le volet central de la console de gestion de l'accès à distance, dans la zone **Étape 3 : Serveurs d'infrastructure**, cliquez sur **Configurer**.  
  
2.  Dans l'Assistant Installation du serveur d'infrastructure, dans la page **Serveur Emplacement réseau**, cliquez sur l'option correspondant à l'emplacement du serveur Emplacement réseau dans votre déploiement.  
  
    -   Si le serveur emplacement réseau se trouve sur un serveur web distant, entrez l’URL, puis cliquez sur **Validate** avant de continuer.  
  
    -   Si le serveur Emplacement réseau se trouve sur le serveur d'accès à distance, cliquez sur **Parcourir** pour localiser le certificat approprié, puis cliquez sur **Suivant**.  
  
3.  Sur le **DNS** page, dans la table, entrez les suffixes de noms supplémentaires qui seront appliquées en tant qu’exemptions de la Table de stratégie de résolution de noms (NRPT). Sélectionnez une option de résolution de noms locale, puis cliquez sur **Suivant**.  
  
4.  Sur le **liste de recherche de suffixes DNS** page, le serveur d’accès à distance détecte automatiquement les suffixes de domaine dans le déploiement. Utiliser le **ajouter** et **supprimer** boutons pour créer la liste des suffixes de domaine que vous souhaitez utiliser. Pour ajouter un nouveau suffixe de domaine, dans **Nouveau suffixe**, entrez le suffixe, puis cliquez sur **Ajouter**. Cliquez sur **Suivant**.  
  
5.  Sur le **gestion** page, ajouter des serveurs d’administration qui ne sont pas détectés automatiquement, puis cliquez sur **suivant**. L'accès à distance ajoute automatiquement des contrôleurs de domaine et des serveurs System Center Configuration Manager.  
  
6.  Cliquez sur **Terminer**.  
  
## <a name="BKMK_App"></a>Configurer les serveurs d’applications  
Dans un déploiement complet de l’accès à distance, la configuration des serveurs d’applications est une tâche facultative. Dans ce scénario pour la gestion à distance des clients DirectAccess, les serveurs d’applications ne sont pas utilisées, et cette étape est grisée pour indiquer qu’il n’est pas active. Cliquez sur **Terminer** pour appliquer la configuration.  
  
## <a name="BKMK_GPO"></a>Configuration résumé et autres objets stratégie de groupe  
Une fois la configuration de l'accès à distance terminée, la **Vérification de l'accès DirectAccess** est affichée. Vous pouvez passer en revue tous les paramètres que vous avez sélectionnés auparavant, y compris :  
  
-   **Paramètres de stratégie de groupe**  
  
    Le nom du serveur de stratégie de groupe DirectAccess et le nom de l’objet stratégie de groupe Client sont répertoriés. Vous pouvez cliquer sur le **modification** situé en regard du **paramètres GPO** titre pour modifier les paramètres de stratégie de groupe.  
  
-   **Clients distants**  
  
    La configuration du client DirectAccess est affichée, y compris le groupe de sécurité, les vérificateurs de connectivité et nom de connexion DirectAccess.  
  
-   **Serveur d’accès à distance**  
  
    La configuration de DirectAccess est affichée, y compris le nom public, l’adresse, configuration de la carte réseau et les informations de certificat.  
  
-   **Serveurs d’infrastructure**  
  
    cette liste inclut l'URL du serveur Emplacement réseau, les suffixes DNS utilisés par les clients DirectAccess et des informations sur les serveurs d'administration.  
  
## <a name="BKMK_Links"></a>Voir aussi  
  
-   [Étape 3 : Vérifier le déploiement](Step-3-Verify-the-Deployment_2.md)  
  
  


