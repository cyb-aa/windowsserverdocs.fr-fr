---
ms.assetid: 276a7f7d-5faa-4c00-a51c-3fa511fe52f9
title: Configurer un environnement de laboratoire ADFS
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 53d0e24f7fcb9efc64406dc6ed01f5bb1deb2277
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="set-up-an-ad-fs-lab-environment"></a>Configurer un environnement de laboratoire ADFS

>S’applique à: Windows Server2012R2

Cette rubrique décrit les étapes pour configurer un environnement de test qui peut être utilisé pour effectuer les procédures des guides de procédure pas à pas suivants:  
  
-   [Procédure pas à pas: Jonction avec un appareil iOS](Walkthrough--Workplace-Join-with-an-iOS-Device.md)  
  
-   [Procédure pas à pas: Joindre un espace de travail avec un appareil Windows](Walkthrough--Workplace-Join-with-a-Windows-Device.md)  
  
-   [Guide pas à pas: Gérer les risques avec contrôle d’accès conditionnel](Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md)  
  
-   [Guide de procédure pas à pas: Gérer les risques avec une authentification multifacteur supplémentaire pour les Applications sensibles](Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)  
  
> [!NOTE]  
> Nous vous déconseillons d’installer le serveur web et le serveur de fédération sur le même ordinateur.  
  
Pour configurer cet environnement de test, procédez comme suit:  
  
1.  [Étape1: Configurer le contrôleur de domaine (DC1)](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_1)  
  
2.  [Étape2: Configurer le serveur de fédération (ADFS1) avec Device Registration Service](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_4)  
  
3.  [Étape3: Configurer le serveur web (WebServ1) et un exemple d’application basée sur les revendications](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_5)  
  
4.  [Étape4: Configurer l’ordinateur client (Client1)](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_10)  
  
## <a name="BKMK_1"></a>Étape1: Configurer le contrôleur de domaine (DC1)  
Dans le cadre de cet environnement de test, vous pouvez appeler votre domaine ActiveDirectory de racine **contoso.com** et spécifiez **pass@word1**comme mot de passe administrateur.  
  
-   Installer le service de rôle ADDS et les Services de domaine ActiveDirectory (ADDS) pour que votre ordinateur un contrôleur de domaine dans Windows Server2012R2. Cette action met à niveau votre schéma ADDS dans le cadre de la création du contrôleur de domaine. Pour plus d’informations et des instructions détaillées, voir[Library/hh472162.aspx https://technet.microsoft.com/](https://technet.microsoft.com/library/hh472162.aspx).  
  
### <a name="BKMK_2"></a>Créer des comptes de test ActiveDirectory  
Une fois que votre contrôleur de domaine est opérationnel, vous pouvez créer des comptes d’utilisateur de test et le groupe de test dans ce domaine et ajouter le compte d’utilisateur pour le compte de groupe. Vous utilisez ces comptes pour effectuer les procédures des guides de procédure pas à pas qui sont référencés plus haut dans cette rubrique.  
  
Créez les comptes suivants:  
  
-   Utilisateur: **Robert Hatley** avec les informations d’identification suivantes: nom d’utilisateur: **RobertH** et mot de passe:**P@ssword**  
  
-   Groupe: **Finance**  
  
Pour plus d’informations sur la création des comptes d’utilisateur et groupe dans ActiveDirectory (AD), consultez [https://technet.microsoft.com/library/cc783323%28v=ws.10%29.aspx ](https://technet.microsoft.com/library/cc783323%28v=ws.10%29.aspx).  
  
Ajouter le **Robert Hatley** compte pour le **Finance** groupe. Pour plus d’informations sur l’ajout d’un utilisateur à un groupe dans ActiveDirectory, voir [https://technet.microsoft.com/library/cc737130%28v=ws.10%29.aspx ](https://technet.microsoft.com/library/cc737130%28v=ws.10%29.aspx).  
  
### <a name="create-a-gmsa-account"></a>Créer un compte de service administré de groupe  
Le groupe compte de Service gérés (GMSA) est requis lors de l’installation d’ActiveDirectory Federation Services (ADFS) et la configuration.  
  
##### <a name="to-create-a-gmsa-account"></a>Pour créer un compte de service administré de groupe  
  
1.  Ouvrez une fenêtre de commande Windows PowerShell et tapez:  
  
    ```  
    Add-KdsRootKey –EffectiveTime (Get-Date).AddHours(-10)  
    New-ADServiceAccount FsGmsa -DNSHostName adfs1.contoso.com -ServicePrincipalNames http/adfs1.contoso.com  
  
    ```  
  
## <a name="BKMK_4"></a>Étape2: Configurer le serveur de fédération (ADFS1) à l’aide de Device Registration Service  
Pour configurer un autre ordinateur virtuel, installez Windows Server2012R2 et connectez-le au domaine **contoso.com**. Configurer l’ordinateur après l’avoir joint au domaine, puis passez à l’installer et configurer le rôle ADFS.  
  
Pour visionner une vidéo, voir [ActiveDirectory Federation Services série de vidéos procédurales: l’installation d’une batterie de serveurs ADFS ](https://technet.microsoft.com/video/dn469436).  
  
### <a name="install-a-server-ssl-certificate"></a>Installer un certificat SSL de serveur  
Vous devez installer un certificat Secure Socket Layer (SSL) du serveur sur le serveur ADFS1 dans le magasin de l’ordinateur local. Le certificat doit avoir les attributs suivants:  
  
-   Nom du sujet (CN): adfs1.contoso.com  
  
-   Autre nom d’objet (DNS): adfs1.contoso.com  
  
-   Autre nom d’objet (DNS): enterpriseregistration.contoso.com  
  
Pour plus d’informations sur la configuration des certificats SSL, consultez [configurer SSL/TLS sur un site Web dans le domaine avec une autorité de certification d’entreprise ](https://social.technet.microsoft.com/wiki/contents/articles/12485.configure-ssltls-on-a-web-site-in-the-domain-with-an-enterprise-ca.aspx).  
  
[ActiveDirectory Federation Services série de vidéos procédurales: Mise à jour des certificats ](https://technet.microsoft.com/video/adfs-updating-certificates).  
  
### <a name="install-the-ad-fs-server-role"></a>Installer le rôle de serveur ADFS  
  
##### <a name="to-install-the-federation-service-role-service"></a>Pour installer le service de rôle Service de fédération  
  
1.  Ouvrez une session sur le serveur en utilisant le compte d’administrateur de domaine administrator@contoso.com.  
  
2.  Démarrez le Gestionnaire de serveur. Pour démarrer le Gestionnaire de serveur, cliquez sur **le Gestionnaire de serveur** sur les fenêtres **Démarrer** de l’écran, ou cliquez sur **le Gestionnaire de serveur** sur la barre des tâches Windows sur le bureau Windows. Sur le **Quick Start** onglet de la **Bienvenue** vignette sur le **tableau de bord**, cliquez sur **ajouter des rôles et fonctionnalités **. Vous pouvez également cliquer sur **Ajout de rôles et fonctionnalités** sur le **gérer** menu.  
  
3.  Sur le **avant de commencer** , cliquez sur **suivant**.  
  
4.  Sur le **sélectionner le type d’installation**, cliquez sur **installation basée sur un rôle ou une fonctionnalité**, puis cliquez sur **suivant**.  
  
5.  Sur le **serveur de destination sélectionnez**, cliquez sur **sélectionner un serveur du pool de serveurs**, vérifiez que l’ordinateur cible est sélectionné, puis cliquez sur **suivant**.  
  
6.  Sur le **sélectionner des rôles de serveur**, cliquez sur **ActiveDirectory Federation Services**, puis cliquez sur **suivant**.  
  
7.  Sur le **sélectionner des fonctionnalités** , cliquez sur **suivant**.  
  
8.  Sur le **Service de fédération ActiveDirectory (ADFS)**, cliquez sur **suivant **.  
  
9. Après avoir vérifié les informations sur la **confirmer les sélections d’installation** page, sélectionnez le **redémarrer automatiquement le serveur de destination si nécessaire** case à cocher, puis cliquez sur **installer**.  
  
10. Sur le **progression de l’Installation** page, vérifiez que tous les éléments installés correctement, puis cliquez sur **fermer**.  
  
### <a name="configure-the-federation-server"></a>Configurer le serveur de fédération  
L’étape suivante consiste à configurer le serveur de fédération.  
  
##### <a name="to-configure-the-federation-server"></a>Pour configurer le serveur de fédération  
  
1.  Dans le Gestionnaire de serveur **tableau de bord**, cliquez sur le **Notifications** indicateur, puis cliquez sur **configurer le service de fédération sur le serveur**.  
  
    Le **Assistant Configuration de Service de fédération ActiveDirectory** s’ouvre.  
  
2.  Sur le **Bienvenue** page, sélectionnez **créer le premier serveur de fédération dans une batterie de serveurs de fédération**, puis cliquez sur **suivant**.  
  
3.  Sur le **se connecter aux services ADDS**, spécifiez un compte doté de droits d’administrateur de domaine pour le **contoso.com** domaine ActiveDirectory que cet ordinateur est joint, puis cliquez sur **suivant **.  
  
4.  Sur le **spécifier les propriétés de Service** page, procédez comme suit, puis cliquez sur **suivant**:  
  
    -   Importez le certificat SSL que vous avez obtenu précédemment. Ce certificat est le certificat d’authentification de service requis. Accédez à l’emplacement de votre certificat SSL.  
  
    -   Pour fournir un nom pour votre service de fédération, tapez **adfs1.contoso.com**. Cette valeur est la même valeur que vous avez fournies lorsque vous inscrit un certificat SSL dans les Services de certificats ActiveDirectory (ADCS).  
  
    -   Pour attribuer un nom d’affichage pour votre service de fédération, tapez **Contoso Corporation **.  
  
5.  Sur le **spécifier un compte de Service** page, sélectionnez **utiliser un compte d’utilisateur de domaine ou compte de Service administré de groupe**, puis spécifiez le compte de service administré de groupe **fsgmsa** que vous avez créé le contrôleur de domaine.  
  
6.  Sur le **spécifier la base de données de Configuration** page, sélectionnez **créer une base de données sur ce serveur à l’aide de la base de données interne Windows**, puis cliquez sur **suivant **.  
  
7.  Sur le **examiner les Options** page, vérifiez vos sélections de configuration, puis cliquez sur **suivant**.  
  
8.  Sur le **vérifications des conditions préalables**, vérifiez que toutes les vérifications de la configuration requise ont été correctement effectuées, puis cliquez sur **configurer **.  
  
9. Sur le **résultats** page, passez en revue les résultats, vérifiez si la configuration a réussi, puis cliquez sur **étapes ultérieures requises pour terminer le déploiement de votre service de fédération **.  
  
### <a name="configure-device-registration-service"></a>Configurer Device Registration Service  
L’étape suivante consiste à configurer Device Registration Service sur le serveur ADFS1. Pour visionner une vidéo, voir [ActiveDirectory Federation Services série de vidéos procédurales: l’activation de Device Registration Service ](https://technet.microsoft.com/video/adfs-how-to-enabling-the-device-registration-service).  
  
##### <a name="to-configure-device-registration-service-for-windows-server-2012-rtm"></a>Pour configurer Device Registration Service pour Windows Server2012 RTM  
  
1.  > [!IMPORTANT]  
    > **L’étape suivante s’applique à la version de Windows Server2012R2 RTM.**  
  
    Ouvrez une fenêtre de commande Windows PowerShell et tapez:  
  
    ```  
    Initialize-ADDeviceRegistration  
    ```  
  
    Lorsque vous êtes invité pour un compte de service, tapez **contosofsgmsa$**.  
  
    Exécutez maintenant l’applet de commande Windows PowerShell.  
  
    ```  
    Enable-AdfsDeviceRegistration  
    ```  
  
2.  Sur le serveur ADFS1, dans le **gestion ADFS** de la console, accédez à **stratégies d’authentification **. Sélectionnez **modifier l’authentification principale globale **. Activez la case à cocher **activer l’authentification des appareils**, puis cliquez sur **OK **.  
  
### <a name="add-host-a-and-alias-cname-resource-records-to-dns"></a>Ajouter l’hôte (A) et les enregistrements de ressource Alias (CNAME) dans DNS  
Sur DC1, vous devez vous assurer que les enregistrements de nom de domaine (DNS) suivants sont créés pour Device Registration Service.  
  
|Entrée|Type|Adresse|  
|---------|--------|-----------|  
|adfs1|Hôte (A)|Adresse IP du serveur ADFS|  
|enterpriseregistration|Alias (CNAME)|adfs1.contoso.com|  
  
Vous pouvez utiliser la procédure suivante pour ajouter un enregistrement de ressource hôte (A) pour les serveurs de noms DNS d’entreprise pour le serveur de fédération et Device Registration Service.  
  
L’appartenance au groupe Administrateurs ou un équivalent est le minimum requis pour effectuer cette procédure. Examinez les informations relatives à l’aide des comptes appropriés et les appartenances de groupe dans le lien hypertexte "https://go.microsoft.com/fwlink/?LinkId=83477" Local et les groupes de domaine par défaut (https://go.microsoft.com/fwlink/p/?LinkId=83477).  
  
##### <a name="to-add-a-host-a-and-alias-cname-resource-records-to-dns-for-your-federation-server"></a>Pour ajouter un ordinateur hôte (A) et les enregistrements de ressource alias (CNAME) dans DNS pour votre serveur de fédération  
  
1.  Sur DC1, dans le Gestionnaire de serveur, sur le **outils** menu, cliquez sur **DNS** pour ouvrir le composant logiciel enfichable DNS.  
  
2.  Dans l’arborescence de la console, développez DC1, **Zones de recherche directe**, avec le bouton droit **contoso.com**, puis cliquez sur **nouvel hôte (A ou AAAA)**.  
  
3.  Dans **nom,** tapez le nom que vous souhaitez utiliser pour votre batterie ADFS. Pour cette procédure pas à pas, tapez **adfs1**.  
  
4.  Dans **adresse IP**, tapez l’adresse IP du serveur ADFS1. Cliquez sur **ajouter l’hôte**.  
  
5.  Avec le bouton droit **contoso.com**, puis cliquez sur **nouvel Alias (CNAME)**.  
  
6.  Dans le **nouvel enregistrement de ressource** boîte de dialogue, tapez **enterpriseregistration** dans les **nom d’Alias** zone.  
  
7.  Dans le domaine nom complet (FQDN) de la zone d’ordinateur hôte cible, tapez **adfs1.contoso.com**, puis cliquez sur **OK **.  
  
    > [!IMPORTANT]  
    > Dans un déploiement réel, si votre société possède plusieurs suffixes de nom principal (UPN) d’utilisateur, vous devez créer plusieurs un enregistrement CNAME pour chaque suffixe UPN dans DNS.  
  
## <a name="BKMK_5"></a>Étape3: Configurer le serveur web (WebServ1) et un exemple d’application basée sur les revendications  
Configurer un ordinateur virtuel (WebServ1) en installant le système d’exploitation Windows Server2012R2 et connectez-le au domaine **contoso.com**. Après que l’avoir joint au domaine, vous pouvez passer à installer et configurer le rôle de serveur Web.  
  
Pour effectuer les procédures qui ont été mentionnés plus haut dans cette rubrique, vous devez disposer d’un exemple d’application qui est sécurisé par votre serveur de fédération (ADFS1).  
  
Vous pouvez télécharger le Kit de développement logiciel WindowsIdentityFoundation ([https://www.microsoft.com/download/details.aspx?id=4451](https://www.microsoft.com/download/details.aspx?id=4451), qui inclut un exemple d’application basée sur les revendications.  
  
Vous devez effectuer les étapes suivantes pour configurer un serveur web avec cet exemple d’application basée sur les revendications.  
  
> [!NOTE]  
> Ces étapes ont été testées sur un serveur web qui exécute le système d’exploitation Windows Server2012R2.  
  
1.  [Installer le rôle de serveur Web et WindowsIdentityFoundation](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_15)  
  
2.  [Installer le Kit de développement de WindowsIdentityFoundation](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_13)  
  
3.  [Configurer l’application de revendications dans IIS](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_9)  
  
4.  [Créer une approbation de partie de confiance sur votre serveur de fédération](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_11)  
  
### <a name="BKMK_15"></a>Installer le rôle de serveur Web et WindowsIdentityFoundation  
  
1.  > [!NOTE]  
    > Vous devez avoir accès au support d’installation Windows Server2012R2.  
  
    Connectez-vous à WebServ1 en utilisant **administrator@contoso.com**et le mot de passe **pass@word1**.  
  
2.  Gestionnaire de serveur, sur le **Quick Start** onglet de la **Bienvenue** vignette sur le **tableau de bord**, cliquez sur **ajouter des rôles et fonctionnalités **. Vous pouvez également cliquer sur **Ajout de rôles et fonctionnalités** sur le **gérer** menu.  
  
3.  Sur le **avant de commencer** , cliquez sur **suivant**.  
  
4.  Sur le **sélectionner le type d’installation**, cliquez sur **installation basée sur un rôle ou une fonctionnalité**, puis cliquez sur **suivant**.  
  
5.  Sur le **serveur de destination sélectionnez**, cliquez sur **sélectionner un serveur du pool de serveurs**, vérifiez que l’ordinateur cible est sélectionné, puis cliquez sur **suivant**.  
  
6.  Sur le **sélectionner des rôles de serveur**, sélectionnez la case à cocher à côté **serveur Web (IIS)**, cliquez sur **ajouter des fonctionnalités**, puis cliquez sur **suivant **.  
  
7.  Sur le **sélectionner des fonctionnalités**, sélectionnez **WindowsIdentityFoundation3.5**, puis cliquez sur **suivant **.  
  
8.  Sur le **rôle de serveur Web (IIS)**, cliquez sur **suivant**.  
  
9. Sur le **sélectionner les services de rôle** page, sélectionnez et développez **développement d’applications **. Sélectionnez **ASP.NET 3.5**, cliquez sur **ajouter des fonctionnalités**, puis cliquez sur **suivant **.  
  
10. Sur le **confirmer les sélections d’installation**, cliquez sur **spécifier un chemin d’accès source secondaire **. Entrez le chemin d’accès du répertoire Sxs sur lequel se trouve dans le support d’installation de Windows Server2012R2. Par exemple D:SourcesSxs. Cliquez sur **OK**, puis cliquez sur **installer **.  
  
### <a name="BKMK_13"></a>Installer le Kit de développement de WindowsIdentityFoundation  
  
1.  Exécutez WindowsIdentityFoundation-SDK-3.5.msi pour installer WindowsIdentityFoundation3.5 de kit de développement logiciel (https://www.microsoft.com/download/details.aspx?id=4451). Choisissez toutes les options par défaut.  
  
### <a name="BKMK_9"></a>Configurer l’application de revendications dans IIS  
  
1.  Installer un certificat SSL valide dans le magasin de certificats ordinateur. Le certificat doit contenir le nom de votre serveur web, **webserv1.contoso.com **.  
  
2.  Copiez le contenu de c: Program fichiers (x86) WindowsIdentityFoundationSDKv3.5SamplesQuick StartWeb ApplicationPassiveRedirectBasedClaimsAwareWebApp à C:InetpubClaimapp.  
  
3.  Modifier le **Default.aspx.cs** de fichiers afin que le filtrage de revendications a lieu. Cette étape est effectuée pour s’assurer que l’application d’exemple affiche toutes les revendications qui sont émises par le serveur de fédération. Procédez comme suit:  
  
    1.  Ouvrez **Default.aspx.cs** dans un éditeur de texte.  
  
    2.  Recherchez dans le fichier pour la deuxième instance de `ExpectedClaims`.  
  
    3.  Commentez la totalité `IF`instruction ainsi que ses accolades. Indiquez les commentaires en tapant «/ /» (sans les guillemets) au début d’une ligne.  
  
    4.  Votre `FOREACH`instruction doit maintenant ressembler à cet exemple de code.  
  
        ```  
        Foreach (claim claim in claimsIdentity.Claims)  
        {  
           //Before showing the claims validate that this is an expected claim  
           //If it is not in the expected claims list then don’t show it  
           //if (ExpectedClaims.Contains( claim.ClaimType ) )  
           // {  
              writeClaim( claim, table );  
           //}  
        }  
  
        ```  
  
    5.  Enregistrez et fermez **Default.aspx.cs **.  
  
    6.  Ouvrez **web.config** dans un éditeur de texte.  
  
    7.  Supprimez la totalité de `<microsoft.identityModel>`section. Supprimer tout le code depuis `including <microsoft.identityModel>`et des `</microsoft.identityModel>`.  
  
    8.  Enregistrez et fermez **web.config **.  
  
4.  **Configurer le Gestionnaire des services Internet**  
  
    1.  Ouvrez **Internet Information Services (IIS) Manager **.  
  
    2.  Accédez à **Pools d’applications**, avec le bouton droit **DefaultAppPool** pour sélectionner **paramètres avancés **. Définissez **charger le profil utilisateur** à **True**, puis cliquez sur **OK **.  
  
    3.  Avec le bouton droit **DefaultAppPool** pour sélectionner **les paramètres de base **. Modifier le **Version CLR .NET** à **CLR .NET v2.0.50727 Version **.  
  
    4.  Avec le bouton droit **site Web par défaut** pour sélectionner **modifier les liaisons **.  
  
    5.  Ajouter un **HTTPS** au port de liaison **443** avec le certificat SSL que vous avez installés.  
  
    6.  Avec le bouton droit **site Web par défaut** pour sélectionner **ajouter une Application **.  
  
    7.  Affectez à l’alias **claimapp** et le chemin d’accès physique **c:inetpubclaimapp**.  
  
5.  Pour configurer **claimapp** pour travailler avec votre serveur de fédération, procédez comme suit:  
  
    1.  Exécutez FedUtil.exe qui se trouve dans **c: Program fichiers (x86) WindowsIdentityFoundationSDKv3.5**.  
  
    2.  Définissez l’emplacement de configuration d’application sur **C:inetputclaimappweb.config** et définissez l’URI d’application sur l’URL de votre site, **https://webserv1.contoso.com /claimapp/**. Cliquez sur **suivant**.  
  
    3.  Sélectionnez **utiliser un STS existant** et accédez à l’URL des métadonnées de votre serveur ADFS **https://adfs1.contoso.com/federationmetadata/2007-06/federationmetadata.xml **. Cliquez sur **suivant**.  
  
    4.  Sélectionnez **désactiver la validation de chaîne de certificat**, puis cliquez sur **suivant **.  
  
    5.  Sélectionnez **aucun chiffrement**, puis cliquez sur **suivant **. Sur le **revendications proposées**, cliquez sur **suivant **.  
  
    6.  Activez la case à cocher **planifier une tâche à effectuer des mises à jour des métadonnées de WS-Federation quotidiennes **. Cliquez sur **Terminer**.  
  
    7.  Votre exemple d’application est maintenant configuré. Si vous testez l’URL de l’application **https://webserv1.contoso.com/claimapp**, il doit vous rediriger vers votre serveur de fédération. Le serveur de fédération doit afficher une page d’erreur, car vous n’avez pas encore configuré l’approbation. En d’autres termes, vous n’avez pas sécurisé cette application test avec ADFS.  
  
Vous devez à présent sécuriser votre exemple d’application qui s’exécute sur votre serveur web avec ADFS. Pour cela, en ajoutant une relation de confiance sur votre serveur de fédération (ADFS1). Pour visionner une vidéo, voir [ActiveDirectory Federation Services série de vidéos procédurales: ajouter une confiance ](https://technet.microsoft.com/video/adfs-how-to-add-a-relying-party-trust).  
  
### <a name="BKMK_11"></a>Créer une approbation de partie de confiance sur votre serveur de fédération  
  
1.  Sur votre serveur de fédération (ADFS1), dans le **console Gestion ADFS**, accédez à **approbations de partie de confiance**, puis cliquez sur **ajouter une approbation de partie de confiance **.  
  
2.  Sur le **sélectionner une Source de données**, sélectionnez **importer les données sur la partie de confiance publiées en ligne ou sur un réseau local**, entrez l’URL des métadonnées de **claimapp**, puis cliquez sur **suivant **. Exécutez FedUtil.exe créé un fichier .xml de métadonnées. Il se trouve dans   
    **https://webserv1.contoso.com/claimapp/federationmetadata/2007-06/federationmetadata.xml**.  
  
3.  Sur le **spécifier le nom complet**, spécifiez le **nom d’affichage** pour votre confiance **claimapp**, puis cliquez sur **suivant **.  
  
4.  Sur le **configurer l’authentification multifacteur maintenant? **page, sélectionnez **je ne souhaite pas spécifier le paramètre de l’authentification multifacteur pour cette approbation de partie de confiance pour l’instant**, puis cliquez sur **suivant **.  
  
5.  Sur le **choisir les règles d’autorisation d’émission** page, sélectionnez **autoriser tous les utilisateurs à accéder à cette partie de confiance**, puis cliquez sur **suivant **.  
  
6.  Sur le **prêt à ajouter l’approbation**, cliquez sur **suivant **.  
  
7.  Sur le **modifier les règles de revendication** boîte de dialogue, cliquez sur **ajouter une règle **.  
  
8.  Sur le **choisir le Type de règle** page, sélectionnez **envoyer des revendications à l’aide d’une règle personnalisée**, puis cliquez sur **suivant **.  
  
9. Sur le **configurer une règle de revendication** page, dans le **nom de règle de revendication**, tapez **toutes les revendications **. Dans le **règle personnalisée**, tapez la règle de revendication suivante.  
  
    ```  
    c:[ ]  
    => issue(claim = c);  
  
    ```  
  
10. Cliquez sur **Terminer**, puis cliquez sur **OK**.  
  
## <a name="BKMK_10"></a>Étape4: Configurer l’ordinateur client (Client1)  
Configurer un autre ordinateur virtuel et installez Windows8.1. Cet ordinateur virtuel doit être sur le même réseau virtuel que les autres ordinateurs. Cet ordinateur ne doit pas être joint au domaine Contoso.  
  
Le client doit approuver le certificat SSL utilisé pour le serveur de fédération (ADFS1), que vous avez configuré dans [étape2: configurer le serveur de fédération (ADFS1) avec Device Registration Service](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_4). Il doit également être en mesure de valider les informations sur la révocation du certificat.  
  
Vous devez également configurer et utiliser un compte Microsoft pour vous connecter à Client1.  
  
## <a name="see-also"></a>Voir aussi  
[ActiveDirectory Federation Services série de vidéos procédurales: L’installation d’une batterie ADFS](https://technet.microsoft.com/video/dn469436)  
[ActiveDirectory Federation Services série de vidéos procédurales: Mise à jour des certificats](https://technet.microsoft.com/video/adfs-updating-certificates)  
[ActiveDirectory Federation Services série de vidéos procédurales: Ajouter une partie de confiance](https://technet.microsoft.com/video/adfs-how-to-add-a-relying-party-trust)  
[ActiveDirectory Federation Services série de vidéos procédurales: L’activation de Device Registration Service](https://technet.microsoft.com/video/adfs-how-to-enabling-the-device-registration-service)  
[ActiveDirectory Federation Services série de vidéos procédurales: L’installation du Proxy d’Application Web](https://technet.microsoft.com/video/dn469438)  
  
