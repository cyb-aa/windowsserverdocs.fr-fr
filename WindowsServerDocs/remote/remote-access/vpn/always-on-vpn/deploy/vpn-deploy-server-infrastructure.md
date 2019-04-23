---
title: Configurer l’Infrastructure de serveur
description: Dans cette étape, vous installez et configurez les composants côté serveur nécessaires pour prendre en charge de la connexion VPN. Les composants côté serveur incluent la configuration d’infrastructure à clé publique pour distribuer les certificats utilisés par les utilisateurs, le serveur VPN et le serveur NPS.
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: ''
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.date: 08/30/2018
ms.reviewer: deverette
ms.openlocfilehash: 21b494bea1990fb8424537205db483d977331465
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832930"
---
# <a name="step-2-configure-the-server-infrastructure"></a>Étape 2. Configurer l’infrastructure de serveur

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows 10

&#171;  [**Précédent :** Étape 1. Planifier le déploiement de VPN Always On](always-on-vpn-deploy-planning.md)<br>
&#187;  [**Suivant :** Étape 3. Configurer le serveur d’accès à distance pour toujours sur VPN](vpn-deploy-ras.md)


Dans cette étape, vous installez et configurez les composants côté serveur nécessaires pour prendre en charge de la connexion VPN. Les composants côté serveur incluent la configuration d’infrastructure à clé publique pour distribuer les certificats utilisés par les utilisateurs, le serveur VPN et le serveur NPS.  Vous également configurer RRAS pour prendre en charge les connexions IKEv2 et le serveur NPS pour gérer l’autorisation pour les connexions VPN.

## <a name="configure-certificate-autoenrollment-in-group-policy"></a>Configurer l’inscription automatique de certificat dans la stratégie de groupe
Dans cette procédure, vous configurez la stratégie de groupe sur le contrôleur de domaine afin que les membres du domaine demander automatiquement des certificats utilisateur et ordinateur. Ainsi, les utilisateurs VPN demander et récupérer des certificats d’utilisateur qui s’authentifient automatiquement des connexions VPN. De même, cette stratégie permet de serveurs NPS pour demander le serveur de certificats d’authentification automatiquement. 

Vous inscrivez manuellement des certificats sur les serveurs VPN.

>[!TIP]
>Pour les ordinateurs non domained jointes, consultez [configuration de l’autorité de certification pour n’appartenant pas au domaine des ordinateurs joints au](#ca-configuration-for-non-domain-joined-computers) ci-dessous. Étant donné que le serveur RRAS n’est pas joint à un domaine, l’inscription automatique ne peut pas être utilisé pour inscrire le certificat de passerelle VPN.  Par conséquent, utilisez une procédure de demande de certificat hors ligne. 


1.  Sur un contrôleur de domaine, ouvrez Gestion des stratégies de groupe.

2.  Dans le volet de navigation, cliquez sur votre domaine (par exemple, corp.contoso.com), puis cliquez sur **créer un objet GPO dans ce domaine et le lier ici**.

3.  Dans la boîte de dialogue Nouvel objet GPO, tapez **stratégie d’inscription automatique**, puis cliquez sur **OK**.

4.  Dans le volet de navigation, cliquez sur **stratégie d’inscription automatique**, puis cliquez sur **modifier**.

5.  Dans l’éditeur de gestion de stratégie de groupe, procédez comme suit pour configurer l’inscription automatique d’ordinateur :

    1.  Dans le volet de navigation, cliquez sur **Configuration ordinateur\\stratégies\\Windows paramètres\\paramètres de sécurité\\stratégies de clé publique**.

    2.  Dans le volet détails, cliquez sur **Client de Services de certificats – inscription automatique**, puis cliquez sur **propriétés**.

    3.  Sur le Client des Services de certificats – inscription automatique boîte de dialogue Propriétés, dans **modèle de Configuration**, cliquez sur **activé**.

    4.  Sélectionnez **renouveler les certificats expirés, mettre à jour des certificats en attente et supprimer les certificats révoqués** et **mettre à jour les certificats qui utilisent les modèles de certificat**.

    5.  Cliquez sur **OK**.

6.  Dans l’éditeur de gestion de stratégie de groupe, procédez comme suit pour configurer l’inscription automatique d’utilisateur :

    1.  Dans le volet de navigation, cliquez sur **Configuration utilisateur\\stratégies\\Windows paramètres\\paramètres de sécurité\\stratégies de clé publique**.

    2.  Dans le volet détails, cliquez sur **Client de Services de certificats – inscription automatique**, puis cliquez sur **propriétés**.

    3.  Sur le Client des Services de certificats – inscription automatique boîte de dialogue Propriétés, dans **modèle de Configuration**, cliquez sur **activé**.

    4.  Sélectionnez **renouveler les certificats expirés, mettre à jour des certificats en attente et supprimer les certificats révoqués** et **mettre à jour les certificats qui utilisent les modèles de certificat**.

    5.  Cliquez sur **OK**.

    6.  Fermez l’éditeur de gestion des stratégies de groupe.

7.  Fermez la gestion des stratégies de groupe.

### <a name="ca-configuration-for-non-domain-joined-computers"></a>Configuration de l’autorité de certification pour les ordinateurs n’appartenant pas au domaine joints

Étant donné que le serveur RRAS n’est pas joint à un domaine, l’inscription automatique ne peut pas être utilisé pour inscrire le certificat de passerelle VPN.  Par conséquent, utilisez une procédure de demande de certificat hors ligne. 

1. Sur le serveur RRAS, générer un fichier appelé **VPNGateway.inf** en fonction de la demande de stratégie de certificat exemple fournie dans l’annexe une (section 0) et personnaliser les entrées suivantes : 

   - Dans la section [NewRequest], remplacez vpn.contoso.com utilisé pour le nom de sujet avec le choix [_client_] nom de domaine complet du point de terminaison VPN.

   - Dans la section [Extensions], remplacez vpn.contoso.com utilisé pour l’autre nom du sujet avec le choix [_client_] nom de domaine complet du point de terminaison VPN.

2. Enregistrer ou copier le **VPNGateway.inf** fichier à un emplacement choisi.

3. À partir d’une invite de commandes avec élévation de privilèges, accédez au dossier qui contient le **VPNGateway.inf** fichier et tapez :

   ```
   certreq -new VPNGateway.inf VPNGateway.req
   ```

4. Copie nouvellement créée **VPNGateway.req** fichier de sortie à un serveur d’autorité de Certification ou la station de travail à accès privilégié (PAW). 

5. Enregistrer ou copier le **VPNGateway.req** fichier à un emplacement spécifique sur le serveur de l’autorité de Certification, ou station de travail à accès privilégié (PAW).

6. À partir d’une invite de commandes avec élévation de privilèges, accédez au dossier qui contient le fichier VPNGateway.req créé à l’étape précédente et type : 

   ```
   certreq -attrib “CertificateTemplate:[Customer]VPNGateway” -submit VPNgateway.req VPNgateway.cer
   ```

7. Si vous y êtes invité par la fenêtre liste des autorités de Certification, sélectionnez l’AC d’entreprise approprié pour traiter la demande de certificat.

8. Copie nouvellement créée **VPNGateway.cer** fichier de sortie dans le serveur RRAS. 

9. Enregistrer ou copier le **VPNGateway.cer** fichier à un emplacement spécifique sur le serveur RRAS.

10. À partir d’une invite de commandes avec élévation de privilèges, accédez au dossier qui contient le fichier VPNGateway.cer créé à l’étape précédente et type :
   
   ```
   certreq -accept VPNGateway.cer
   ```

11. Exécutez le composant logiciel enfichable MMC Certificats, comme décrit [ici](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-view-certificates-with-the-mmc-snap-in) en sélectionnant le **compte d’ordinateur** option.

12. Vérifiez l’existence d’un certificat valide pour le serveur RRAS avec les propriétés suivantes :

   - **Rôles prévus :** Authentification de serveur, la sécurité IP IKE intermédiaire 

   - **Modèle de certificat :** [_client_] serveur VPN

#### <a name="example-vpngatewayinf-script"></a>Exemple : Script de VPNGateway.inf

Ici, vous pouvez voir un exemple de script d’une stratégie de demande de certificat utilisée pour demander un certificat de la passerelle VPN à l’aide d’un processus hors-bande.

>[!TIP]
>Vous pouvez obtenir une copie du script VPNGateway.inf dans le Kit IP VPN offre sous le dossier stratégies de demande de certificat. Met à jour uniquement le « objet » et «\_continuer\_' avec des valeurs spécifiques du client.

```
[Version] 

Signature="$Windows NT$"

[NewRequest]
Subject = "CN=vpn.contoso.com"
Exportable = FALSE   
KeyLength = 2048     
KeySpec = 1          
KeyUsage = 0xA0      
MachineKeySet = True
ProviderName = "Microsoft RSA SChannel Cryptographic Provider"
RequestType = PKCS10 

[Extensions]
2.5.29.17 = "{text}"
_continue_ = "dns=vpn.contoso.com&"

```

## <a name="create-the-vpn-users-vpn-servers-and-nps-servers-groups"></a>Créer les utilisateurs VPN, les serveurs VPN, ainsi que les groupes de serveurs NPS

Dans cette procédure, vous pouvez ajouter un nouveau groupe d’Active Directory (AD) qui contient les utilisateurs autorisés à utiliser des VPN pour se connecter au réseau de votre organisation.

Ce groupe a deux objectifs :

-   Il définit quels utilisateurs sont autorisés à s’inscrire automatiquement pour les certificats d’utilisateur que requiert la connexion VPN.

-   Il définit les utilisateurs qui autorise par le serveur NPS pour l’accès VPN.

En utilisant un groupe personnalisé, si vous voulez révoquer l’accès d’un utilisateur VPN, vous pouvez supprimer cet utilisateur à partir du groupe.

Vous ajoutez également un groupe contenant des serveurs VPN et un autre groupe contenant des serveurs NPS. Ces groupes permettent de limiter les demandes de certificat à leurs membres.

>[!NOTE] 
>Nous vous recommandons de VPN serveurs qui se trouvent dans le DMA/périmètre ne pas être joints au domaine. Toutefois, si vous préférez que les serveurs VPN joints au domaine pour une meilleure gérabilité (stratégies de groupe, l’agent de sauvegarde/surveillance, aucun utilisateur local à gérer et ainsi de suite), ajoutez ensuite un groupe AD pour le modèle de certificat de serveur VPN.

### <a name="configure-the-vpn-users-group"></a>Configurer le groupe d’utilisateurs VPN

1.  Sur un contrôleur de domaine, ouvrez Active Directory Users and Computers.

2.  Avec le bouton droit à un conteneur ou une unité d’organisation, cliquez sur **New** et cliquez sur **groupe**.

3.  Dans **nom_groupe**, type **utilisateurs VPN**, puis cliquez sur **OK**.

4.  Avec le bouton droit **utilisateurs VPN**, puis cliquez sur **propriétés**.

5.  Sur le **membres** onglet de la boîte de dialogue Propriétés des utilisateurs VPN, cliquez sur **ajouter**.

6.  Sur la boîte de dialogue Sélectionner des utilisateurs, ajoutez tous les utilisateurs qui ont besoin d’un accès VPN et cliquez sur **OK**.

7.  Fermez Utilisateurs et ordinateurs Active Directory.

### <a name="configure-the-vpn-servers-and-nps-servers-groups"></a>Configurer les groupes de serveurs VPN et les serveurs NPS

1.  Sur un contrôleur de domaine, ouvrez Active Directory Users and Computers.

2.  Avec le bouton droit à un conteneur ou une unité d’organisation, cliquez sur **New** et cliquez sur **groupe**.

3.  Dans **nom_groupe**, type **serveurs VPN**, puis cliquez sur **OK**.

4.  Avec le bouton droit **serveurs VPN**, puis cliquez sur **propriétés**.

5.  Sur le **membres** onglet de la boîte de dialogue Propriétés des serveurs VPN, cliquez sur **ajouter**.

6.  Cliquez sur **Types d’objets**, sélectionnez le **ordinateurs** case à cocher, puis cliquez sur **OK**.

7.  Dans **Entrez les noms des objets à sélectionner**, tapez les noms de vos serveurs VPN, cliquez sur **OK**.

8.  Cliquez sur **OK** pour fermer la boîte de dialogue Propriétés des serveurs VPN.

9.  Répétez les étapes précédentes pour le groupe de serveurs NPS.

10. Fermez Utilisateurs et ordinateurs Active Directory.

## <a name="create-the-user-authentication-template"></a>Créer le modèle d’authentification de l’utilisateur

Dans cette procédure, vous configurez un modèle d’authentification personnalisé client-serveur. Ce modèle est requis parce que vous souhaitez améliorer la sécurité globale du certificat en sélectionnant les niveaux de compatibilité de mise à niveau et en choisissant le fournisseur de chiffrement de plateforme Microsoft. Cette dernière modification vous permet d’utiliser le module de plateforme sécurisée sur les ordinateurs clients pour sécuriser le certificat. Pour une vue d’ensemble du module TPM, consultez [Trusted Platform Module Technology Overview](https://docs.microsoft.com/windows/device-security/tpm/trusted-platform-module-overview).

**Procédure :**

1.  Sur l’autorité de certification, ouvrez l’autorité de Certification.

2.  Dans le volet de navigation, cliquez sur **modèles de certificats**, puis cliquez sur **gérer**.

3.  Dans la console Modèles de certificat, cliquez sur **utilisateur**, puis cliquez sur **dupliquer le modèle**.

    >[!WARNING]
    >Ne cliquez pas sur **appliquer** ou **OK** à tout moment avant l’étape 10.  Si vous cliquez sur ces boutons avant de passer tous les paramètres, nombreux choix deviennent fixe et n’est plus modifiable. Par exemple, sur le **cryptographie** si l’onglet, _fournisseur de stockage de chiffrement hérité_ présente dans le champ de catégorie de fournisseur, il est désactivé, empêchant toute autre modification. La seule alternative consiste à supprimer le modèle et le recréer.  

4.  Dans la boîte de dialogue Propriétés du nouveau modèle, sur le **général** onglet, procédez comme suit :

    1.  Dans **nom complet du modèle**, type **l’authentification des utilisateurs VPN**.

    2.  Effacer la **publier le certificat dans Active Directory** case à cocher.

5.  Sur le **sécurité** onglet, procédez comme suit :

    1.  Cliquez sur **Ajouter**.

    2.  Sur Sélectionner des utilisateurs, ordinateurs, comptes de Service ou boîte de dialogue de groupes, tapez **utilisateurs VPN**, puis cliquez sur **OK**.

    3.  Dans **noms d’utilisateur ou groupe**, cliquez sur **utilisateurs VPN**.

    4.  Dans **autorisations pour les utilisateurs VPN**, sélectionnez le **inscription** et **inscription automatique** cases à cocher dans la **autoriser** colonne.

       >[!TIP]
       >Veillez à conserver en lecture la case à cocher. En d’autres termes, vous devez disposer des autorisations en lecture pour l’inscription. 

    5.  Dans **noms d’utilisateur ou groupe**, cliquez sur **utilisateurs du domaine**, puis cliquez sur **supprimer**.

6.  Sur le **compatibilité** onglet, procédez comme suit :

    1.  Dans **autorité de Certification**, cliquez sur **Windows Server 2012 R2**.

    2.  Sur le **modifications résultantes** boîte de dialogue, cliquez sur **OK**.

    3.  Dans **destinataire du certificat**, cliquez sur **Windows 8.1 / Windows Server 2012 R2**.

    4.  Sur le **modifications résultantes** boîte de dialogue, cliquez sur **OK**.

7.  Sur le **traitement de la demande** onglet, désactivez le **autoriser à exporter la clé privée** case à cocher.

8.  Sur le **cryptographie** onglet, procédez comme suit :

    1.  Dans **catégorie de fournisseur**, cliquez sur **Key Storage Provider**.

    2.  Cliquez sur **requêtes doivent utiliser un des fournisseurs suivants**.

    3.  Sélectionnez le **fournisseur de chiffrement de plateforme Microsoft** case à cocher.

9.  Sur le **nom de l’objet** si l’onglet, vous n’avez pas une adresse de messagerie répertoriée sur tous les comptes d’utilisateur, désactivez le **inclure le nom de messagerie dans le nom de l’objet** et **nom de messagerie électronique** cases à cocher.

10. Cliquez sur **OK** pour enregistrer le modèle de certificat de l’authentification des utilisateurs VPN.

11. Fermez la console Modèles de certificats.

12. Dans le volet de navigation du composant logiciel enfichable Autorité de Certification, cliquez sur **modèles de certificats**, cliquez sur **New** puis cliquez sur **modèle de certificat à délivrer**.

13. Cliquez sur **l’authentification des utilisateurs VPN**, puis cliquez sur **OK**.

14. Fermez le composant logiciel enfichable Autorité de Certification.

## <a name="create-the-vpn-server-authentication-template"></a>Créer le modèle d’authentification du serveur VPN

Dans cette procédure, vous pouvez configurer un nouveau modèle d’authentification du serveur pour votre serveur VPN. Utilisation de clé étendue d’ajout de que la stratégie d’application IPsec (IP Security) IKE Intermediate permet au serveur de certificats de filtre si plusieurs certificats est disponible avec l’authentification du serveur.

>[!IMPORTANT] 
>Étant donné que les clients VPN d’accéder à ce serveur à partir de l’Internet public, l’objet et les autres noms sont différents du nom de serveur interne. Par conséquent, vous ne pouvez pas inscrire automatiquement ce certificat sur les serveurs VPN.

**Conditions préalables :**<p>
Joint au domaine des serveurs VPN

**Procédure :** 

1.  Sur l’autorité de certification, ouvrez l’autorité de Certification.

2.  Dans le volet de navigation, cliquez sur **modèles de certificats**, puis cliquez sur **gérer**.

3.  Dans la console Modèles de certificat, cliquez sur **serveur RAS et IAS**, puis cliquez sur **dupliquer le modèle**.

4.  Dans la boîte de dialogue Propriétés du nouveau modèle, sur le **général** sous l’onglet **nom complet du modèle**, entrez un nom descriptif pour le serveur VPN, par exemple, **serveur VPN Authentification** ou **serveur RADIUS**.

5.  Sur le **Extensions** onglet, procédez comme suit :

    1.  Cliquez sur **stratégies d’Application**, puis cliquez sur **modifier**.

    2.  Dans le **modifier l’Extension des stratégies d’Application** boîte de dialogue, cliquez sur **ajouter**.

    3.  Sur le **ajouter une stratégie Application** boîte de dialogue, cliquez sur **sécurité IP IKE intermédiaire**, puis cliquez sur **OK**.<p>Ajout d’adresse IP IKE intermédiaire pour l’EKU de sécurité vous aide à dans les scénarios où il existe plus d’un certificat d’authentification serveur sur le serveur VPN. Lors de la sécurité IP IKE intermédiaire est présente, IPSec utilise uniquement le certificat avec les deux options EKU. Sans cela, l’authentification IKEv2 peut échouer avec l’erreur 13801 : Informations d’identification de l’authentification IKE sont inacceptables.

    4.  Cliquez sur **OK** pour revenir à la **propriétés du nouveau modèle** boîte de dialogue.

6.  Sur le **sécurité** onglet, procédez comme suit :

    1.  Cliquez sur **Ajouter**.

    2.  Sur le **sélectionner des utilisateurs, les ordinateurs, les comptes de Service ou les groupes** boîte de dialogue, tapez **serveurs VPN**, puis cliquez sur **OK**.

    3.  Dans **noms d’utilisateur ou groupe**, cliquez sur **serveurs VPN**.

    4.  Dans **autorisations pour les serveurs VPN**, sélectionnez le **inscription** case à cocher dans la **autoriser** colonne.

    5.  Dans **noms d’utilisateur ou groupe**, cliquez sur **serveurs RAS et IAS**, puis cliquez sur **supprimer**.

7.  Sur le **nom de l’objet** onglet, procédez comme suit :

    1.  Cliquez sur **fourni dans la demande**.

    2.  Sur le **modèles de certificats** avertissement de boîte de dialogue, cliquez sur **OK**.

8.  (Facultatif) *Si vous configurez l’accès conditionnel pour la connectivité VPN*, cliquez sur le **traitement de la demande** onglet, puis cliquez sur **autoriser à exporter la clé privée** pour le sélectionner.

9.  Cliquez sur **OK** pour enregistrer le modèle de certificat de serveur VPN.

10. Fermez la console Modèles de certificats.

11. Dans le volet de navigation du composant logiciel enfichable Autorité de Certification, cliquez sur **modèles de certificats**, cliquez sur **New** puis cliquez sur **modèle de certificat à délivrer**.

12. Redémarrez les services d’autorité de certification.

13. Sélectionnez le nom que vous avez choisi à l’étape 4 ci-dessus, puis cliquez sur **OK**.

14. Fermez le composant logiciel enfichable Autorité de Certification.

## <a name="create-the-nps-server-authentication-template"></a>Créer le modèle d’authentification du serveur NPS

Le modèle de certificat troisième et dernière à créer est le modèle de l’authentification du serveur NPS. Le modèle de l’authentification du serveur NPS est une simple copie du modèle de serveur RAS et IAS sécurisé pour le groupe de serveur NPS que vous avez créé précédemment dans cette section. 

Vous allez configurer ce certificat pour l’inscription automatique.

**Procédure :**

1.  Sur l’autorité de certification, ouvrez l’autorité de Certification.

2.  Dans le volet de navigation, cliquez sur **modèles de certificats**, puis cliquez sur **gérer**.

3.  Dans la console Modèles de certificat, cliquez sur **serveur RAS et IAS**, puis sélectionnez **dupliquer le modèle**.

4.  Dans la boîte de dialogue Propriétés du nouveau modèle, sur le **général** sous l’onglet **nom complet du modèle**, type **l’authentification du serveur NPS**.

5.  Sur le **sécurité** onglet, procédez comme suit :

    1.  Cliquez sur **Ajouter**.

    2.  Sur le **sélectionner des utilisateurs, les ordinateurs, les comptes de Service ou les groupes** boîte de dialogue, tapez **serveurs NPS**, puis cliquez sur **OK**.

    3.  Dans **noms d’utilisateur ou groupe**, cliquez sur **serveurs NPS**.

    4.  Dans **autorisations pour les serveurs NPS**, sélectionnez le **inscription** et **inscription automatique** cases à cocher dans la **autoriser** colonne.

    5.  Dans **noms d’utilisateur ou groupe**, cliquez sur **serveurs RAS et IAS**, puis cliquez sur **supprimer**.

6.  Cliquez sur **OK** pour enregistrer le modèle de certificat de serveur NPS.

7.  Fermez la console Modèles de certificats.

8.  Dans le volet de navigation du composant logiciel enfichable Autorité de Certification, cliquez sur **modèles de certificats**, cliquez sur **New** puis cliquez sur **modèle de certificat à délivrer**.

9.  Cliquez sur **l’authentification du serveur NPS**, puis cliquez sur **OK**.

10. Fermez le composant logiciel enfichable Autorité de Certification.

## <a name="enroll-and-validate-the-user-certificate"></a>Inscrire et de valider le certificat utilisateur

Étant donné que vous utilisez stratégie de groupe pour inscrire automatiquement les certificats utilisateur, vous devez uniquement mettre à jour la stratégie et Windows 10 sont inscrits automatiquement le compte d’utilisateur pour le certificat correct. Vous pouvez ensuite valider le certificat dans la console de certificats.

**Procédure :**

1.  Connectez-vous à un ordinateur client joint au domaine en tant que membre de la **utilisateurs VPN** groupe.

2.  Appuyez sur la touche de Windows + R, type **gpupdate /force**, puis appuyez sur ENTRÉE.

3.  Dans le menu Démarrer, tapez **certmgr.msc**, puis appuyez sur ENTRÉE.

4.  Dans le composant logiciel enfichable Certificats, sous **personnel**, cliquez sur **certificats**. Vos certificats apparaissent dans le volet de détails.

5.  Cliquez sur le certificat qui a votre nom d’utilisateur de domaine actuel, puis cliquez sur **Open**.

6.  Sur le **général** onglet, vérifiez que la date est répertorié sous **valide à partir de** est la date d’aujourd'hui. Si elle n’est pas le cas, vous avez peut-être sélectionné le bon certificat.

7.  Cliquez sur **OK**et fermez le composant logiciel enfichable Certificats.

## <a name="enroll-and-validate-the-server-certificates"></a>Inscrire et de valider les certificats de serveur

À la différence du certificat utilisateur, vous devez manuellement inscrire le certificat du serveur VPN. Une fois que vous l’avez inscrit, validez-le en utilisant le même processus que vous avez utilisé pour le certificat de l’utilisateur. Comme le certificat utilisateur, le serveur NPS s’inscrit automatiquement son certificat d’authentification, par conséquent, il vous suffit est de le valider.

>[!NOTE] 
>Vous devrez peut-être redémarrer les serveurs VPN et NPS pour leur permettre de mettre à jour leur appartenance aux groupes avant que vous pouvez effectuer ces étapes.

### <a name="enroll-and-validate-the-vpn-server-certificate"></a>Inscrire et de valider le certificat de serveur VPN

1.  Dans le menu de démarrage du serveur VPN, tapez **certlm.msc**, puis appuyez sur ENTRÉE.

2.  Avec le bouton droit **personnel**, cliquez sur **toutes les tâches** puis cliquez sur **demander un nouveau certificat** pour démarrer l’Assistant Inscription de certificats.

3.  Dans la page avant de commencer, cliquez sur **suivant**.

4.  Dans la page Sélectionner la stratégie d’inscription certificat, cliquez sur **suivant**.

5.  Dans la page demander des certificats, cliquez sur la case à cocher en regard du serveur VPN pour le sélectionner.

6.  Sous la case à cocher du serveur VPN, cliquez sur **plus d’informations est requis** pour ouvrir la boîte de dialogue Propriétés du certificat et effectuez les étapes suivantes :

    1.  Cliquez sur le **sujet** , cliquez sur **nom commun** sous **nom de l’objet**, dans **Type**.

    2.  Sous **nom de l’objet**, dans **valeur**, entrez le nom des clients de domaine externe utilisé pour se connecter au VPN, par exemple, vpn.contoso.com, puis cliquez sur **ajouter**.

    3.  Sous **autre nom**, dans **Type**, cliquez sur **DNS**.

    4.  Sous **autre nom**, dans **valeur**, entrez tous les noms de serveur que les clients utilisent pour se connecter au VPN, par exemple, vpn.contoso.com, vpn, 132.64.86.2.

    5.  Cliquez sur **ajouter** après avoir entré chaque nom.

    6.  Lorsque vous avez terminé, cliquez sur **OK**.

7.  Cliquez sur **S’inscrire**.

8.  Cliquez sur **Terminer**.

9.  Dans le composant logiciel enfichable Certificats, sous **personnel**, cliquez sur **certificats**.<p>Vos certificats répertoriés apparaissent dans le volet de détails.

10. Cliquez sur le certificat qui est votre serveur VPN nommez, puis cliquez sur **Open**.

11. Sur le **général** onglet, vérifiez que la date est répertorié sous **valide à partir de** est la date d’aujourd'hui. Si elle n’est pas le cas, vous avez peut-être sélectionné le certificat incorrect.

12. Sur le **détails** , cliquez sur **Enhanced Key Usage**et vérifiez que **sécurité IP IKE intermédiaire** et **l’authentification du serveur** afficher dans la liste.

13. Cliquez sur **OK** pour fermer le certificat.

14. Fermez le composant logiciel enfichable Certificats.

### <a name="validate-the-nps-server-certificate"></a>Valider le certificat de serveur NPS

1.  Redémarrez le serveur NPS.

2.  Dans le menu de démarrage du serveur NPS, tapez **certlm.msc**, puis appuyez sur ENTRÉE.

3.  Dans le composant logiciel enfichable Certificats, sous **personnel**, cliquez sur **certificats**.<p>Vos certificats répertoriés apparaissent dans le volet de détails.

4.  Cliquez sur le certificat qui est votre serveur NPS nommez, puis cliquez sur **Open**.

5.  Sur le **général** onglet, vérifiez que la date est répertorié sous **valide à partir de** est la date d’aujourd'hui. Si elle n’est pas le cas, vous avez peut-être sélectionné le certificat incorrect.

6.  Cliquez sur **OK** pour fermer le certificat.

7.  Fermez le composant logiciel enfichable Certificats.

## <a name="next-step"></a>Étape suivante
[Étape 3. Configurer le serveur d’accès à distance pour toujours sur VPN](vpn-deploy-ras.md): Dans cette étape, vous configurez les VPN d’accès distant pour autoriser les connexions VPN IKEv2, refuser les connexions à partir d’autres protocoles VPN et affecter un pool d’adresses IP statiques pour l’émission d’adresses IP à la connexion des clients VPN autorisés.







---
