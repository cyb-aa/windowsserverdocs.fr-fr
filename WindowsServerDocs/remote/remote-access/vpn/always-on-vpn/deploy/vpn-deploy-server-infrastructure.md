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
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6067172"
---
# Étape2. Configurer l’infrastructure de serveur

>S’applique à: Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows 10

& #171;  [ **Précédente:** étape 1. Planifier le déploiement de VPN toujours actif](always-on-vpn-deploy-planning.md)<br>
& #187;  [ **Suivant:** étape 3. Configurer le serveur d’accès à distance pour toujours sur VPN](vpn-deploy-ras.md)


Dans cette étape, vous installez et configurez les composants côté serveur nécessaires pour prendre en charge de la connexion VPN. Les composants côté serveur incluent la configuration d’infrastructure à clé publique pour distribuer les certificats utilisés par les utilisateurs, le serveur VPN et le serveur NPS.  Vous configurez également RRAS pour prendre en charge les connexions de protocole IKEv2 et le serveur NPS pour exécuter une autorisation pour des connexions VPN.

## Configurer l’inscription automatique de certificat dans la stratégie de groupe
Dans cette procédure, vous configurez la stratégie de groupe sur le contrôleur de domaine afin que les membres du domaine demandent automatiquement des certificats d’utilisateur et d’ordinateur. Cela permet aux utilisateurs VPN demander et récupérer des certificats d’utilisateur qui s’authentifient automatiquement des connexions VPN. De même, cette stratégie permet serveurs NPS pour demander le serveur de certificats d’authentification automatiquement. 

Vous inscrire manuellement des certificats sur les serveurs VPN.

>[!TIP]
>Pour les ordinateurs joints à un non domained, consultez la [configuration de l’autorité de certification pour hors domaine joint ordinateurs](#ca-configuration-for-non-domain-joined-computers) ci-dessous. Dans la mesure où le serveur RRAS n’est pas joint au domaine, l’inscription automatique ne peuvent pas être utilisé pour inscrire le certificat de passerelle VPN.  Par conséquent, utilisez une procédure de demande de certificat en mode hors connexion. 


1.  Sur un contrôleur de domaine, ouvrez Gestion des stratégies de groupe.

2.  Dans le volet de navigation, avec le bouton droit de votre domaine (par exemple, corp.contoso.com), puis cliquez sur**créer un objet GPO dans ce domaine et le lier ici**.

3.  Dans la boîte de dialogue Nouvel objet GPO, tapez **L’inscription automatique de stratégie**, puis cliquez sur **OK**.

4.  Dans le volet de navigation, avec le bouton droit de **La stratégie de l’inscription automatique**, puis cliquez sur **Modifier**.

5.  Dans l’éditeur de gestion de stratégie de groupe, procédez comme suit pour configurer l’inscription automatique de certificat ordinateur:

    1.  Dans le volet de navigation, cliquez sur **Stratégies de clé ordinateur Configuration\\Policies\\Windows Settings\\Security Settings\\Public**.

    2.  Dans le volet d’informations, cliquez sur**Services Client – inscription automatique des certificats**, puis cliquez sur **Propriétés**.

    3.  Sur le Client de Services de certificats – boîte de dialogue des propriétés de l’inscription automatique, dans le **Modèle de Configuration**, cliquez sur **activé**.

    4.  Sélectionnez**renouveler les certificats expirés, mettre à jour les certificats en attente et supprimer les certificats révoqués**et**mettre à jour les certificats qui utilisent les modèles de certificats**.

    5.  Cliquez sur**OK**.

6.  Dans l’éditeur de gestion de stratégie de groupe, procédez comme suit pour configurer l’inscription automatique d’utilisateur:

    1.  Dans le volet de navigation, cliquez sur **Les stratégies de clé utilisateur Configuration\\Policies\\Windows Settings\\Security Settings\\Public**.

    2.  Dans le volet d’informations, cliquez sur**Services Client – inscription automatique des certificats**, puis cliquez sur **Propriétés**.

    3.  Sur le Client de Services de certificats – boîte de dialogue des propriétés de l’inscription automatique, dans le **Modèle de Configuration**, cliquez sur **activé**.

    4.  Sélectionnez**renouveler les certificats expirés, mettre à jour les certificats en attente et supprimer les certificats révoqués**et**mettre à jour les certificats qui utilisent les modèles de certificats**.

    5.  Cliquez sur**OK**.

    6.  Fermez l’éditeur de gestion des stratégies de groupe.

7.  Gestion des stratégies de groupe de fermer.

### Configuration de l’autorité de certification pour les ordinateurs joints à un domaine non

Dans la mesure où le serveur RRAS n’est pas joint au domaine, l’inscription automatique ne peuvent pas être utilisé pour inscrire le certificat de passerelle VPN.  Par conséquent, utilisez une procédure de demande de certificat en mode hors connexion. 

1. Sur le serveur RRAS, générer un fichier nommé **VPNGateway.inf** en fonction de la stratégie de certificat exemple demander fournies dans l’annexe une (section 0) et personnaliser les entrées suivantes: 

   - Dans la section [NewRequest], remplacez vpn.contoso.com utilisé pour le nom du sujet avec le point de terminaison [_client_] VPN choisie nom de domaine complet.

   - Dans la section [Extensions], remplacez vpn.contoso.com utilisé pour l’autre nom du sujet avec le point de terminaison [_client_] VPN choisie nom de domaine complet.

2. Enregistrer ou copier le fichier **VPNGateway.inf** à un emplacement choisi.

3. À partir d’une invite de commandes avec élévation de privilèges, accédez au dossier qui contient le fichier **VPNGateway.inf** et le type:

   ```
   certreq -new VPNGateway.inf VPNGateway.req
   ```

4. Copiez le fichier de sortie **VPNGateway.req** nouvellement créé sur un serveur d’autorité de Certification ou station de travail d’accès privilégié (patte). 

5. Enregistrer ou copier le fichier **VPNGateway.req** à un emplacement choisi sur le serveur de l’autorité de Certification, ou station de travail d’accès privilégié (patte).

6. À partir d’une invite de commandes avec élévation de privilèges, accédez au dossier qui contient le fichier VPNGateway.req créé à l’étape précédente, puis tapez: 

   ```
   certreq -attrib “CertificateTemplate:[Customer]VPNGateway” -submit VPNgateway.req VPNgateway.cer
   ```

7. Si vous y êtes invité par la fenêtre de liste d’autorité de Certification, sélectionnez l’autorité de certification de l’entreprise appropriées pour traiter la demande de certificat.

8. Copiez le fichier de sortie **VPNGateway.cer** nouvellement créé sur le serveur RRAS. 

9. Enregistrer ou copier le fichier **VPNGateway.cer** à un emplacement choisi sur le serveur RRAS.

10. À partir d’une invite de commandes avec élévation de privilèges, accédez au dossier qui contient le fichier VPNGateway.cer créé à l’étape précédente, puis tapez:
   
   ```
   certreq -accept VPNGateway.cer
   ```

11. Exécutez le composant logiciel enfichable MMC certificats comme décrit [ici](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-view-certificates-with-the-mmc-snap-in) la sélection de l’option de **compte d’ordinateur** .

12. Vérifiez l’existence d’un certificat valide pour le serveur RRAS avec les propriétés suivantes:

   - **Destiné à des fins:** Authentification du serveur, sécurité IP IKE intermédiaire 

   - **Modèle de certificat:** [_Client_] Serveur VPN

#### Exemple: VPNGateway.inf de script

Ici, vous pouvez voir un exemple de script d’une stratégie de demande de certificat utilisée pour demander un certificat de passerelle VPN à l’aide d’un processus hors-bande.

>[!TIP]
>Vous trouverez une copie du script VPNGateway.inf dans le Kit IP VPN offre sous le dossier de stratégies de demande de certificat. Uniquement mettre à jour le 'Objet' et '\_continue\_' avec des valeurs spécifiques au client.

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

## Créer les utilisateurs VPN, les serveurs VPN et les groupes de serveurs NPS

Dans cette procédure, vous pouvez ajouter un nouveau groupe Active Directory (AD) qui contient les utilisateurs autorisés à utiliser la connexion VPN pour se connecter au réseau de votre organisation.

Ce groupe a deux objectifs:

-   Il définit les utilisateurs sont autorisés à s’inscrire automatiquement pour les certificats d’utilisateur que nécessite la connexion VPN.

-   Il définit les utilisateurs qui le serveur NPS autorise pour l’accès VPN.

En utilisant un groupe personnalisé, si vous souhaitez jamais révoquer l’accès VPN de l’utilisateur, vous pouvez supprimer cet utilisateur à partir du groupe.

Vous ajoutez également un groupe contenant des serveurs VPN et un autre groupe contenant des serveurs NPS. Ces groupes vous permettent de limiter les demandes de certificat à leurs membres.

>[!NOTE] 
>Nous vous recommandons de VPN serveurs qui se trouvent dans le périmètre de DMA/ne pas être joints au domaine. Toutefois, si vous souhaitez que les serveurs VPN joints au domaine pour faciliter la gestion (stratégies de groupe, sauvegarde/Monitoring agent, aucun utilisateur local à gérer et ainsi de suite), ajoutez ensuite un groupe AD pour le modèle de certificat de serveur VPN.

### Configurer le groupe utilisateurs VPN

1.  Sur un contrôleur de domaine, ouvrez les utilisateurs Active Directory et les ordinateurs.

2.  Avec le bouton droit à un conteneur ou une unité d’organisation, cliquez sur **Nouveau** et cliquez sur **le groupe**.

3.  Dans la zone **nom du groupe**, tapez **Les utilisateurs VPN**, puis cliquez sur **OK**.

4.  **Les utilisateurs VPN**d’avec le bouton droit, puis cliquez sur **Propriétés**.

5.  Sous l’onglet **membres** de la boîte de dialogue Propriétés des utilisateurs VPN, cliquez sur **Ajouter**.

6.  Dans la boîte de dialogue Sélectionnez utilisateurs, ajoutez tous les utilisateurs qui ont besoin de VPN accéder et cliquez sur **OK**.

7.  Fermez utilisateurs et ordinateurs Active Directory.

### Configurez les groupes de serveurs VPN et les serveurs NPS

1.  Sur un contrôleur de domaine, ouvrez les utilisateurs Active Directory et les ordinateurs.

2.  Avec le bouton droit à un conteneur ou une unité d’organisation, cliquez sur **Nouveau** et cliquez sur **le groupe**.

3.  Dans la zone **nom du groupe**, tapez les **Serveurs VPN**, puis cliquez sur **OK**.

4.  Avec le bouton droit de **Serveurs VPN**, puis cliquez sur **Propriétés**.

5.  Sous l’onglet **membres** de la boîte de dialogue des propriétés des serveurs VPN, cliquez sur **Ajouter**.

6.  Cliquez sur les **Types d’objet**, activez la case à cocher **ordinateurs** , puis cliquez sur **OK**.

7.  Dans la zone **Entrez les noms des objets à sélectionner**, tapez les noms de vos serveurs VPN, puis cliquez sur **OK**.

8.  Cliquez sur **OK** pour fermer la boîte de dialogue des propriétés des serveurs VPN.

9.  Répétez les étapes précédentes pour le groupe de serveurs NPS.

10. Fermez utilisateurs et ordinateurs Active Directory.

## Créer le modèle de l’authentification des utilisateurs

Dans cette procédure, vous configurez un modèle de l’authentification client-serveur personnalisé. Ce modèle est nécessaire dans la mesure où vous souhaitez améliorer la sécurité globale du certificat en sélectionnant les niveaux de compatibilité mis à niveau et en choisissant le fournisseur de chiffrement de plateforme de Microsoft. Cette dernière modification permet d’utiliser le module de plateforme sécurisée sur les ordinateurs clients pour sécuriser le certificat. Pour une vue d’ensemble du module TPM, voir [Trusted Platform Module présentation de la technologie](https://docs.microsoft.com/windows/device-security/tpm/trusted-platform-module-overview).

**Procédure:**

1.  Sur l’autorité de certification, ouvrez autorité de Certification.

2.  Dans le volet de navigation, cliquez sur**Les modèles de certificats**, puis cliquez sur**Gérer**.

3.  Dans la console Modèles de certificats, avec le bouton droit**d’utilisateur**, puis cliquez sur**Dupliquer le modèle**.

    >[!WARNING]
    >Ne cliquez pas sur **Appliquer** ou **OK** à tout moment avant l’étape 10.  Si vous cliquez sur ces boutons avant d’entrer dans tous les paramètres, nombreux choix deviennent fixe et n’est plus modifiable. Par exemple, l’onglet **chiffrement** , si le _Fournisseur de stockage de chiffrement hérité_ présente dans le champ de catégorie de fournisseur, il devient désactivés, pour empêcher toute autre modification. La seule solution consiste à supprimer le modèle et les recréer.  

4.  Dans la boîte de dialogue Propriétés du nouveau modèle, sur**Général**onglet, procédez comme suit:

    1.  Dans la zone **nom complet du modèle**, tapez **L’authentification des utilisateurs VPN**.

    2.  Désactivez la case à cocher **Publier le certificat dans Active Directory** .

5.  Sur la**sécurité**onglet, procédez comme suit:

    1.  Cliquez sur **Ajouter**.

    2.  Le sélectionner les utilisateurs, les ordinateurs, les comptes de Service ou boîte de dialogue groupes, entrez **Les utilisateurs VPN**, puis cliquez sur **OK**.

    3.  Dans les **noms d’utilisateur ou de groupe**, cliquez sur **Les utilisateurs VPN**.

    4.  Dans **les autorisations pour les utilisateurs VPN**, activez les cases à cocher **s’inscrire** et **d’inscription automatique** dans la colonne **Autoriser** .

       >[!TIP]
       >Veillez à conserver en lecture la case à cocher. En d’autres termes, vous avez besoin d’autorisations de lecture pour l’inscription. 

    5.  Dans les **noms d’utilisateur ou de groupe**, cliquez sur **Les utilisateurs du domaine**, puis cliquez sur **Supprimer**.

6.  Sous l’onglet **compatibilité** , procédez comme suit:

    1.  Dans l' **Autorité de Certification**, cliquez sur **Windows Server2012R2**.

    2.  Dans la boîte de dialogue **résultants modifications** , cliquez sur **OK**.

    3.  Dans la zone **destinataire du certificat**, cliquez sur **Windows8.1 et Windows Server2012R2**.

    4.  Dans la boîte de dialogue **résultants modifications** , cliquez sur **OK**.

7.  Sous l’onglet **Traitement de la demande** , désactivez la case à cocher **Autoriser la clé privée à être exportée** .

8.  Sous l’onglet de **chiffrement** , procédez comme suit:

    1.  Dans la **Catégorie de fournisseur**, cliquez sur **Fournisseur de stockage de clés**.

    2.  Cliquez sur **requêtes doivent utiliser un des fournisseurs suivants**.

    3.  Activez la case à cocher de **Fournisseur de chiffrement de plateforme de Microsoft** .

9.  Sous l’onglet **Nom du sujet** , si vous n’avez pas une adresse de messagerie répertoriée sur tous les comptes d’utilisateur, désactivez les cases à cocher **inclure le nom de courrier électronique dans le nom du sujet** et le **nom de courrier électronique** .

10. Cliquez sur**OK** pour enregistrer le modèle de certificat d’authentification des utilisateurs VPN.

11. Console des modèles àgarantir fermer.

12. Dans le volet de navigation de theCertification Authoritysnap-in, avec le bouton droit de**Modèles de certificats**, cliquez sur **Nouveau** , puis sur**Modèle de certificat à émettre**.

13. **L’authentification des utilisateurs VPN**, puis cliquez sur**OK**.

14. Fermez le composant logiciel enfichable Autorité des theCertification.

## Créer le modèle d’authentification du serveur VPN

Dans cette procédure, vous pouvez configurer un nouveau modèle d’authentification du serveur de votre serveur VPN. Ajout de que la stratégie d’application de sécurité IP (IPsec) IKE Intermediate permet au serveur de certificats de filtre si plusieurs certificats est disponible dans l’authentification du serveur étendu utilisation de la clé.

>[!IMPORTANT] 
>Dans la mesure où les clients VPN d’accéder à ce serveur à partir de l’Internet public, l’objet et les autres noms sont différents du nom de serveur interne. Par conséquent, vous ne pouvez pas inscrire automatiquement ce certificat sur les serveurs VPN.

**Éléments requis:**<p>
Joint au domaine des serveurs VPN

**Procédure:** 

1.  Sur l’autorité de certification, ouvrez autorité de Certification.

2.  Dans le volet de navigation, cliquez sur **Les modèles de certificats**, puis cliquez sur**Gérer**.

3.  Dans la console Modèles de certificats, cliquez sur le**serveur RAS et IAS**, puis cliquez sur**Dupliquer le modèle**.

4.  Dans la boîte de dialogue Propriétés du nouveau modèle, sur**Général**onglet, dans la zone **nom complet du modèle**, entrez un nom descriptif pour le serveur VPN, par exemple, **L’authentification du serveur VPN** ou **Serveur RADIUS**.

5.  Sur les**Extensions**onglet, procédez comme suit:

    1.  Cliquez sur les**Stratégies d’Application**, puis cliquez sur**Modifier**.

    2.  Dans la boîte de dialogue **Modifier l’Extension des stratégies d’Application** , cliquez sur**Ajouter**.

    3.  Dans la boîte de dialogue **Ajouter une stratégie d’Application** , cliquez sur **la sécurité IP IKE intermédiaire**, puis cliquez sur **OK**.<p>Ajout d’IP permet de sécurité IKE intermédiaire pour le EKU dans les scénarios où il existe plus d’un certificat d’authentification serveur sur le serveur VPN. Lors de la sécurité IP IKE intermédiaire est présente, IPSec utilise uniquement le certificat avec les deux options EKU. Sans cela, l’authentification IKEv2 peut échouer avec l’erreur 13801: les informations d’identification d’authentification IKE ne sont pas acceptables.

    4.  Cliquez sur **OK** pour revenir aux**Propriétés du nouveau modèle**boîte de dialogue.

6.  Sur la**sécurité**onglet, procédez comme suit:

    1.  Cliquez sur **Ajouter**.

    2.  La boîte de dialogue **Sélectionner les utilisateurs, les ordinateurs, les comptes de Service ou les groupes** , entrez les **Serveurs VPN**, puis cliquez sur **OK**.

    3.  Dans les **noms d’utilisateur ou de groupe**, cliquez sur **Les serveurs VPN**.

    4.  Dans **les autorisations pour les serveurs VPN**, activez la case à cocher **s’inscrire** dans la colonne **Autoriser** .

    5.  Dans les **noms d’utilisateur ou de groupe**, cliquez sur les **serveurs RAS et IAS**, puis cliquez sur **Supprimer**.

7.  Sous l’onglet **Nom du sujet** , procédez comme suit:

    1.  Cliquez sur **fournir dans la demande**.

    2.  Dans la boîte de dialogue Avertissement de **Modèles de certificats** , cliquez sur **OK**.

8.  (Facultatif) *Si vous configurez l’accès conditionnel pour une connexion VPN*, cliquez sur l’onglet **Traitement de la demande** , puis cliquez sur **Autoriser à exporter la clé privée** pour le sélectionner.

9.  Cliquez sur**OK** pour enregistrer le modèle de certificat de serveur VPN.

10. Console des modèles àgarantir fermer.

11. Dans le volet de navigation de la Certification Authoritysnap-in, avec le bouton droit de**Modèles de certificats**, cliquez sur **Nouveau** , puis sur**Modèle de certificat à émettre**.

12. Redémarrez les services d’autorité de certification.

13. Sélectionnez le nom que vous avez choisi à l’étape 4 ci-dessus, puis cliquez sur**OK**.

14. Fermez le composant logiciel enfichable Autorité des theCertification.

## Créer le modèle d’authentification du serveur NPS

Le modèle de certificat troisième et dernière créer est le modèle d’authentification du serveur NPS. Le modèle d’authentification du serveur NPS est une copie simple du modèle serveur RAS et IAS sécurisé au groupe de serveurs NPS que vous avez créé précédemment dans cette section. 

Vous allez configurer ce certificat pour l’inscription automatique.

**Procédure:**

1.  Sur l’autorité de certification, ouvrez autorité de Certification.

2.  Dans le volet de navigation, cliquez sur **Les modèles de certificats**, puis cliquez sur**Gérer**.

3.  Dans la console Modèles de certificats, cliquez sur le**serveur RAS et IAS**et sélectionnez **Dupliquer le modèle**.

4.  Dans la boîte de dialogue Propriétés du nouveau modèle, sur**Général**onglet, dans la zone **nom complet du modèle**, tapez **L’authentification du serveur NPS**.

5.  Sur la**sécurité**onglet, procédez comme suit:

    1.  Cliquez sur **Ajouter**.

    2.  La boîte de dialogue **Sélectionner les utilisateurs, les ordinateurs, les comptes de Service ou les groupes** , entrez les **Serveurs NPS**, puis cliquez sur **OK**.

    3.  Dans les **noms d’utilisateur ou de groupe**, cliquez sur **Les serveurs NPS**.

    4.  Dans **les autorisations pour les serveurs NPS**, activez les cases à cocher **s’inscrire** et **d’inscription automatique** dans la colonne **Autoriser** .

    5.  Dans les **noms d’utilisateur ou de groupe**, cliquez sur les **serveurs RAS et IAS**, puis cliquez sur **Supprimer**.

6.  Cliquez sur**OK** pour enregistrer le modèle de certificat de serveur NPS.

7.  Console des modèles àgarantir fermer.

8.  Dans le volet de navigation de la Certification Authoritysnap-in, avec le bouton droit de**Modèles de certificats**, cliquez sur **Nouveau** , puis sur**Modèle de certificat à émettre**.

9.  **L’authentification du serveur NPS**, puis cliquez sur**OK**.

10. Fermez le composant logiciel enfichable Autorité des theCertification.

## Inscrire et valider le certificat de l’utilisateur

Étant donné que vous utilisez la stratégie de groupe pour inscrire automatiquement les certificats utilisateur, vous devez uniquement mettre à jour la stratégie et Windows 10 s’inscrira automatiquement le compte d’utilisateur pour le certificat approprié. Vous pouvez ensuite valider le certificat dans la console certificats.

**Procédure:**

1.  Connectez-vous à un ordinateur client joint au domaine en tant que membre du groupe **d’Utilisateurs VPN** .

2.  Appuyez sur la touche Windows + R, tapez **gpupdate/force**et appuyez sur ENTRÉE.

3.  Dans le menu Démarrer, tapez **certmgr.msc**et appuyez sur ENTRÉE.

4.  Dans le composant logiciel enfichable Certificats, sous **personnel**, cliquez sur **les certificats**. Vos certificats s’affichent dans le volet d’informations.

5.  Cliquez sur le certificat qui a votre nom d’utilisateur de domaine actuel, puis cliquez sur **Ouvrir**.

6.  Sous l’onglet **Général** , vérifiez que la date répertoriée sous **valide à partir du** est date du jour. Si elle n’est pas le cas, vous avez peut-être sélectionné le bon certificat.

7.  Cliquez sur **OK**, puis fermez le composant logiciel enfichable Certificats.

## S’inscrire et de valider les certificats de serveur

À la différence du certificat de l’utilisateur, vous devez inscrire manuellement certificat du serveur VPN. Une fois que vous avez inscrit il, validez-la en utilisant le même processus que vous avez utilisé pour le certificat de l’utilisateur. Comme le certificat de l’utilisateur, le serveur NPS inscrit automatiquement son certificat d’authentification, afin que tout ce que vous devez faire est le valider.

>[!NOTE] 
>Vous devrez peut-être redémarrer les serveurs VPN et NPS pour leur permettre de mettre à jour leur appartenance aux groupes avant d’effectuer ces étapes.

### S’inscrire et valider le certificat du serveur VPN

1.  Dans le menu de démarrage du serveur VPN, tapez **certlm.msc**et appuyez sur ENTRÉE.

2.  Avec le bouton droit **personnel**, cliquez sur **Toutes les tâches** , puis sur **Demander un nouveau certificat** pour démarrer l’Assistant d’inscription de certificat.

3.  Sur la page Avant de commencer, cliquez sur **Suivant**.

4.  Sur la page Sélectionner une stratégie de certificat d’inscription, cliquez sur **suivant**.

5.  Sur la page de demander des certificats, cliquez sur la case à cocher en regard du serveur VPN pour le sélectionner.

6.  Sous la case à cocher du serveur VPN, cliquez sur le **que plus d’informations est nécessaire** pour ouvrir la boîte de dialogue des propriétés du certificat et procédez comme suit:

    1.  Cliquez sur l’onglet **objet** , **Nom commun** sous le **nom du sujet**, dans le **Type**.

    2.  Sous le **nom du sujet**, dans la **valeur**, entrez le nom des clients domaine externe utilisé pour se connecter au réseau VPN, par exemple, vpn.contoso.com, puis cliquez sur **Ajouter**.

    3.  Sous **Alternative Name**, **Type**, cliquez sur **DNS**.

    4.  Sous un **Autre nom**, dans la **valeur**, entrez toutes les noms des clients utilisent pour se connecter au réseau VPN, par exemple, vpn.contoso.com, vpn, 132.64.86.2.

    5.  Après avoir tapé chaque nom, cliquez sur **Ajouter** .

    6.  Lorsque vous avez terminé, cliquez sur **OK**.

7.  Cliquez sur **S’inscrire**.

8.  Cliquez sur **Terminer**.

9.  Dans le composant logiciel enfichable Certificats, sous **personnel**, cliquez sur **les certificats**.<p>Vos certificats répertoriés s’affichent dans le volet d’informations.

10. Cliquez sur le certificat qui a le nom de votre serveur VPN, puis cliquez sur **Ouvrir**.

11. Sous l’onglet **Général** , vérifiez que la date répertoriée sous **valide à partir du** est date du jour. Si elle n’est pas le cas, vous avez peut-être sélectionné le certificat incorrect.

12. Sous l’onglet **Détails** , cliquez sur **Utilisation avancée de la clé**et vérifiez que **la sécurité IP IKE intermédiaire** et **L’authentification du serveur** s’affichent dans la liste.

13. Cliquez sur **OK** pour fermer le certificat.

14. Fermez le composant logiciel enfichable Certificats.

### Valider le certificat de serveur NPS

1.  Redémarrez le serveur NPS.

2.  Dans le menu de démarrage du serveur NPS, tapez **certlm.msc**et appuyez sur ENTRÉE.

3.  Dans le composant logiciel enfichable Certificats, sous **personnel**, cliquez sur **les certificats**.<p>Vos certificats répertoriés s’affichent dans le volet d’informations.

4.  Cliquez sur le certificat qui a le nom de votre serveur NPS, puis cliquez sur **Ouvrir**.

5.  Sous l’onglet **Général** , vérifiez que la date répertoriée sous **valide à partir du** est date du jour. Si elle n’est pas le cas, vous avez peut-être sélectionné le certificat incorrect.

6.  Cliquez sur **OK** pour fermer le certificat.

7.  Fermez le composant logiciel enfichable Certificats.

## Étape suivante
[Étape 3. Configurer le serveur d’accès à distance pour toujours actif (AlwaysOn)](vpn-deploy-ras.md): dans cette étape, vous configurez l’accès distant VPN pour autoriser les connexions VPN IKEv2, refuser les connexions à partir d’autres protocoles VPN et affectez un pool d’adresses IP statiques pour l’émission d’adresses IP à la connexion clients VPN autorisés.







---
