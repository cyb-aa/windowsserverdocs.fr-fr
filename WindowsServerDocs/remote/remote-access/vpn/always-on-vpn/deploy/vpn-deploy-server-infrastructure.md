---
title: Configurer l’infrastructure de serveur
description: Dans cette étape, vous installez et configurez les composants côté serveur nécessaires pour prendre en charge le VPN. Les composants côté serveur incluent la configuration de l’infrastructure à clé publique pour distribuer les certificats utilisés par les utilisateurs, le serveur VPN et le serveur NPS.
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: ''
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.date: 08/30/2018
ms.reviewer: deverette
ms.openlocfilehash: b954419904f97102cef14fbd4a7a68496e8730af
ms.sourcegitcommit: 0467b8e69de66e3184a42440dd55cccca584ba95
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69546515"
---
# <a name="step-2-configure-the-server-infrastructure"></a>Étape 2. Configurer l’infrastructure de serveur

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Premier** Étape 1. Planifier le déploiement de VPN Toujours actif (AlwaysOn)](always-on-vpn-deploy-planning.md)
- [**Situé** Étape 3. Configurer le serveur d’accès à distance pour VPN Toujours actif (AlwaysOn)](vpn-deploy-ras.md)

Au cours de cette étape, vous allez installer et configurer les composants côté serveur nécessaires à la prise en charge du VPN. Les composants côté serveur incluent la configuration de l’infrastructure à clé publique pour distribuer les certificats utilisés par les utilisateurs, le serveur VPN et le serveur NPS.  Vous configurez également RRAS pour prendre en charge les connexions IKEv2 et le serveur NPS pour effectuer l’autorisation pour les connexions VPN.

## <a name="configure-certificate-autoenrollment-in-group-policy"></a>Configurer l’inscription automatique des certificats dans stratégie de groupe
Dans cette procédure, vous configurez stratégie de groupe sur le contrôleur de domaine afin que les membres du domaine demandent automatiquement des certificats d’utilisateur et d’ordinateur. Cela permet aux utilisateurs VPN de demander et de récupérer des certificats utilisateur qui authentifient automatiquement les connexions VPN. De même, cette stratégie permet aux serveurs NPS de demander automatiquement des certificats d’authentification serveur. 

Vous inscrivez manuellement les certificats sur les serveurs VPN.

>[!TIP]
>Pour les ordinateurs joints non, consultez [configuration de l’autorité de certification pour les ordinateurs non joints à un domaine](#ca-configuration-for-non-domain-joined-computers). Étant donné que le serveur RRAS n’est pas joint à un domaine, l’inscription automatique ne peut pas être utilisée pour inscrire le certificat de la passerelle VPN.  Par conséquent, utilisez une procédure de demande de certificat hors connexion.

1. Sur un contrôleur de domaine, ouvrez gestion des stratégie de groupe.

2. Dans le volet de navigation, cliquez avec le bouton droit sur votre domaine (par exemple, corp.contoso.com), puis sélectionnez **créer un objet de stratégie de groupe dans ce domaine, et le lier ici**.

3. Dans la boîte de dialogue nouvel objet GPO, entrez **stratégie d’inscription automatique**, puis sélectionnez **OK**.

4. Dans le volet de navigation, cliquez avec le bouton droit sur **stratégie d’inscription automatique**, puis sélectionnez **modifier**.

5. Dans le Éditeur de gestion des stratégies de groupe, procédez comme suit pour configurer l’inscription automatique des certificats d’ordinateur:

    1. Dans le volet de navigation, accédez **à configuration** > ordinateur >  >  > stratégies paramètres Windows paramètres de sécurité**stratégies de clé publique**.

    2. Dans le volet d’informations, cliquez avec le bouton droit sur **client des services de certificats – inscription automatique**, puis sélectionnez **Propriétés**.

    3. Dans la boîte de dialogue client des services de certificats – propriétés d’inscription automatique, dans **modèle de configuration**, sélectionnez **activé**.

    4. Sélectionnez **Renouveler les certificats expirés, mettre à jour les certificats en attente et supprimer les certificats révoqués** et **Mettre à jour les certificats qui utilisent les modèles de certificats**.

    5. Sélectionnez **OK**.

6. Dans le Éditeur de gestion des stratégies de groupe, procédez comme suit pour configurer l’inscription automatique des certificats utilisateur:

    1. Dans le volet de navigation, accédez **à configuration** > utilisateur >  >  > stratégies paramètres Windows paramètres de sécurité**stratégies de clé publique**.

    2. Dans le volet d'informations, cliquez avec le bouton droit sur **Client des services de certificats - Inscription automatique**, puis sélectionnez **Propriétés**.

    3. Dans la boîte de dialogue client des services de certificats – propriétés d’inscription automatique, dans **modèle de configuration**, sélectionnez **activé**.

    4. Sélectionnez **Renouveler les certificats expirés, mettre à jour les certificats en attente et supprimer les certificats révoqués** et **Mettre à jour les certificats qui utilisent les modèles de certificats**.

    5. Sélectionnez **OK**.

    6. Fermez l’éditeur de gestion des stratégies de groupe.

7. Fermez la gestion des stratégies de groupe.

### <a name="ca-configuration-for-non-domain-joined-computers"></a>Configuration de l’autorité de certification pour les ordinateurs non joints à un domaine

Étant donné que le serveur RRAS n’est pas joint à un domaine, l’inscription automatique ne peut pas être utilisée pour inscrire le certificat de la passerelle VPN.  Par conséquent, utilisez une procédure de demande de certificat hors connexion.

1. Sur le serveur RRAS, générez un fichier appelé **VPNGateway. inf** basé sur l’exemple de demande de stratégie de certificat fourni dans l’annexe a (section 0) et personnalisez les entrées suivantes:

   - Dans la section [NewRequest], remplacez vpn.contoso.com utilisé pour le nom du sujet par le nomde domaine complet du point de terminaison VPN choisi [Customer].

   - Dans la section [extensions], remplacez vpn.contoso.com utilisé pour l’autre nom de l’objet par lenom de domaine complet du point de terminaison VPN choisi [Customer].

2. Enregistrez ou copiez le fichier **VPNGateway. inf** à un emplacement choisi.

3. À partir d’une invite de commandes avec élévation de privilèges, accédez au dossier qui contient le fichier **VPNGateway. inf** et tapez:

   ```
   certreq -new VPNGateway.inf VPNGateway.req
   ```

4. Copiez le fichier de sortie **VPNGateway. req** nouvellement créé sur un serveur d’autorité de certification ou une station de travail à accès privilégié (patte).

5. Enregistrez ou copiez le fichier **VPNGateway. req** à un emplacement choisi sur le serveur de l’autorité de certification, ou sur la station de travail à accès privilégié (patte).

6. À partir d’une invite de commandes avec élévation de privilèges, accédez au dossier qui contient le fichier VPNGateway. req créé à l’étape précédente et tapez:

   ```
   certreq -attrib “CertificateTemplate:[Customer]VPNGateway” -submit VPNgateway.req VPNgateway.cer
   ```

7. Si vous y êtes invité par la fenêtre Liste de l’autorité de certification, sélectionnez l’autorité de certification d’entreprise appropriée pour traiter la demande de certificat.

8. Copiez le fichier de sortie **VPNGateway. cer** nouvellement créé sur le serveur RRAS. 

9. Enregistrez ou copiez le fichier **VPNGateway. cer** à un emplacement choisi sur le serveur RRAS.

10. À partir d’une invite de commandes avec élévation de privilèges, accédez au dossier qui contient le fichier VPNGateway. cer créé à l’étape précédente et tapez:

    ```
    certreq -accept VPNGateway.cer
    ```

11. Exécutez le composant logiciel enfichable MMC certificats, comme décrit [ici](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-view-certificates-with-the-mmc-snap-in) , en sélectionnant l’option **compte d’ordinateur** .

12. Assurez-vous qu’il existe un certificat valide pour le serveur RRAS avec les propriétés suivantes:

    - **Rôles prévus:** Authentification du serveur, sécurité IP IKE intermédiaire 

    - **Modèle de certificat:** [_Client_] Serveur VPN

#### <a name="example-vpngatewayinf-script"></a>Exemple : Script VPNGateway. inf

Ici, vous pouvez voir un exemple de script d’une stratégie de demande de certificat utilisée pour demander un certificat de passerelle VPN à l’aide d’un processus hors bande.

>[!TIP]
>Vous pouvez trouver une copie du script VPNGateway. inf dans le kit d’IP de l’offre VPN dans le dossier stratégies de demande de certificat. Mettez à jour uniquement les champs «subject\_»\_et «continue» avec des valeurs spécifiques au client.

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

## <a name="create-the-vpn-users-vpn-servers-and-nps-servers-groups"></a>Créer les groupes utilisateurs VPN, serveurs VPN et serveurs NPS

Dans cette procédure, vous pouvez ajouter un nouveau groupe de Active Directory (AD) qui contient les utilisateurs autorisés à utiliser le VPN pour se connecter au réseau de votre organisation.

Ce groupe remplit deux fonctions:

- Il définit les utilisateurs autorisés à s’inscrire automatiquement pour les certificats utilisateur requis par le VPN.

- Il définit les utilisateurs que le serveur NPS autorise pour l’accès VPN.

En utilisant un groupe personnalisé, si vous souhaitez révoquer l’accès VPN d’un utilisateur, vous pouvez supprimer cet utilisateur du groupe.

Vous ajoutez également un groupe contenant des serveurs VPN et un autre groupe contenant des serveurs NPS. Vous utilisez ces groupes pour restreindre les demandes de certificat à leurs membres.

>[!NOTE]
>Nous recommandons que les serveurs VPN qui résident dans le périphérique DMA/périmètre ne soient pas joints à un domaine. Toutefois, si vous préférez que les serveurs VPN soient joints à un domaine pour une meilleure facilité de gestion (stratégies de groupe, agent de sauvegarde/surveillance, aucun utilisateur local à gérer, etc.), ajoutez un groupe AD au modèle de certificat de serveur VPN.

### <a name="configure-the-vpn-users-group"></a>Configurer le groupe d’utilisateurs VPN

1. Sur un contrôleur de domaine, ouvrez Active Directory utilisateurs et ordinateurs.

2. Cliquez avec le bouton droit sur un conteneur ou une unité d’organisation, sélectionnez **nouveau**, puis sélectionnez **groupe**.

3. Dans **nom du groupe**, entrez **VPN Users**, puis sélectionnez **OK**.

4. Cliquez avec le bouton droit sur **utilisateurs VPN** et sélectionnez **Propriétés**.

5. Sous l’onglet **membres** de la boîte de dialogue Propriétés des utilisateurs VPN, sélectionnez **Ajouter**.

6. Dans la boîte de dialogue Sélectionner des utilisateurs, ajoutez tous les utilisateurs qui ont besoin d’un accès VPN, puis sélectionnez **OK**.

7. Fermez Utilisateurs et ordinateurs Active Directory.

### <a name="configure-the-vpn-servers-and-nps-servers-groups"></a>Configurer les groupes de serveurs NPS et de serveurs VPN

1. Sur un contrôleur de domaine, ouvrez Active Directory utilisateurs et ordinateurs.

2. Cliquez avec le bouton droit sur un conteneur ou une unité d’organisation, sélectionnez **nouveau**, puis sélectionnez **groupe**.

3. Dans **nom du groupe**, entrez **serveurs VPN**, puis sélectionnez **OK**.

4. Cliquez avec le bouton droit sur **serveurs VPN** et sélectionnez **Propriétés**.

5. Sous l’onglet **membres** de la boîte de dialogue Propriétés des serveurs VPN, sélectionnez **Ajouter**.

6. Sélectionnez **types d’objets**, activez la case à cocher **ordinateurs** , puis sélectionnez **OK**.

7. Dans **Entrez les noms des objets à sélectionner**, entrez les noms de vos serveurs VPN, puis sélectionnez **OK**.

8. Sélectionnez **OK** pour fermer la boîte de dialogue Propriétés des serveurs VPN.

9. Répétez les étapes précédentes pour le groupe de serveurs NPS.

10. Fermez Utilisateurs et ordinateurs Active Directory.

## <a name="create-the-user-authentication-template"></a>Créer le modèle d’authentification utilisateur

Dans cette procédure, vous configurez un modèle d’authentification client-serveur personnalisé. Ce modèle est nécessaire, car vous souhaitez améliorer la sécurité globale du certificat en sélectionnant les niveaux de compatibilité mis à niveau et en choisissant le fournisseur de chiffrement de la plateforme Microsoft. Cette dernière modification vous permet d’utiliser le module de plateforme sécurisée sur les ordinateurs clients pour sécuriser le certificat. Pour obtenir une vue d’ensemble du module de plateforme sécurisée, consultez [module de plateforme sécurisée (TPM) vue d’ensemble](https://docs.microsoft.com/windows/device-security/tpm/trusted-platform-module-overview)de la technologie.

>[!IMPORTANT] 
>Le fournisseur de chiffrement de plateforme Microsoft «requiert une puce TPM, dans le cas où vous exécutez une machine virtuelle et vous recevez l’erreur suivante: «Impossible de trouver un CSP valide sur l’ordinateur local» lors de la tentative d’inscription manuelle du certificat, vous devez vérifier «fournisseur de stockage de clés logicielles Microsoft» et le faire en deuxième position après «fournisseur de chiffrement de la plateforme Microsoft» sous l’onglet chiffrement du certificat. sous.

**Procédures**

1. Sur l’autorité de certification, ouvrez autorité de certification.

2. Dans le volet de navigation, cliquez avec le bouton droit sur **modèles de certificats** et sélectionnez **gérer**.

3. Dans la console modèles de certificats, cliquez avec le bouton droit sur **utilisateur** et sélectionnez Dupliquer le **modèle**.

   >[!WARNING]
   >Ne sélectionnez pas **appliquer** ou **OK** à tout moment avant l’étape 10.  Si vous sélectionnez ces boutons avant d’entrer tous les paramètres, de nombreux choix sont résolus et ne sont plus modifiables. Par exemple, sous l' onglet chiffrement, si le _fournisseur de stockage de chiffrement hérité_ s’affiche dans le champ catégorie de fournisseur, il est désactivé, ce qui empêche toute autre modification. La seule alternative consiste à supprimer le modèle et à le recréer.  

4. Dans la boîte de dialogue Propriétés du nouveau modèle, sous l’onglet **général** , procédez comme suit:

   1. Dans **nom complet du modèle**, tapez **authentification de l’utilisateur VPN**.

   2. Désactivez la case à cocher **publier le certificat dans Active Directory** .

5. Dans l’onglet **sécurité** , procédez comme suit:

   1. Sélectionnez **Ajouter**.

   2. Dans la boîte de dialogue Sélectionner les utilisateurs, les ordinateurs, les comptes de service ou les groupes, entrez **VPN Users**, puis sélectionnez **OK**.

   3. Dans **noms de groupes ou d’utilisateurs**, sélectionnez **utilisateurs VPN**.

   4. Dans **autorisations pour les utilisateurs VPN**, activez les cases à cocher **inscrire** et **inscription** automatique dans la colonne **autoriser** .

      >[!TIP]
      >Veillez à conserver la case à cocher lecture activée. En d’autres termes, vous avez besoin des autorisations de lecture pour l’inscription. 

   5. Dans **groupes ou noms d’utilisateurs**, sélectionnez **utilisateurs du domaine**, puis sélectionnez **supprimer**.

6. Dans l’onglet **compatibilité** , procédez comme suit:

   1. Dans **autorité de certification**, sélectionnez **Windows Server 2012 R2**.

   2. Dans la boîte de dialogue **modifications résultantes** , sélectionnez **OK**.

   3. Dans **destinataire du certificat**, sélectionnez **Windows 8.1/Windows Server 2012 R2**.

   4. Dans la boîte de dialogue **modifications résultantes** , sélectionnez **OK**.

7. Sous l’onglet traitement de la **demande** , désactivez la case à cocher **autoriser l’exportation de la clé privée** .

8. Sous l' onglet chiffrement, procédez comme suit:

   1. Dans **catégorie de fournisseur**, sélectionnez **fournisseur de stockage de clés**.

   2. **Les demandes SELECT doivent utiliser l’un des fournisseurs suivants**.

   3. Activez la case à cocher fournisseur de chiffrement de **plateforme Microsoft** .

9. Dans l’onglet **nom** de l’objet, si vous n’avez pas d’adresse de messagerie pour tous les comptes d’utilisateur, désactivez les cases à cocher **inclure le nom de messagerie dans le nom d’objet** et le **nom de messagerie** .

10. Sélectionnez **OK** pour enregistrer le modèle de certificat d’authentification utilisateur VPN.

11. Fermez la console Modèles de certificat.

12. Dans le volet de navigation du composant logiciel enfichable Autorité de certification, cliquez avec le bouton droit sur **modèles de certificats**, sélectionnez **nouveau** , puis **modèle de certificat à**délivrer.

13. Sélectionnez **authentification de l’utilisateur VPN**, puis cliquez sur **OK**.

14. Fermez le composant logiciel enfichable Autorité de certification.

## <a name="create-the-vpn-server-authentication-template"></a>Créer le modèle d’authentification du serveur VPN

Dans cette procédure, vous pouvez configurer un nouveau modèle d’authentification serveur pour votre serveur VPN. L’ajout de la stratégie d’application intermédiaire IKE de sécurité IP (IPsec) permet au serveur de filtrer les certificats si plus d’un certificat est disponible avec l’utilisation de la clé étendue d’authentification du serveur.

>[!IMPORTANT]
>Étant donné que les clients VPN accèdent à ce serveur à partir de l’Internet public, l’objet et les autres noms sont différents du nom du serveur interne. Par conséquent, vous ne pouvez pas inscrire automatiquement ce certificat sur les serveurs VPN.

**Conditions préalables**

Serveurs VPN joints à un domaine

**Procédures**

1. Sur l’autorité de certification, ouvrez autorité de certification.

2. Dans le volet de navigation, cliquez avec le bouton droit sur **modèles de certificats** et sélectionnez **gérer**.

3. Dans la console modèles de certificats, cliquez avec le bouton droit sur **serveur RAS et IAS** , puis sélectionnez Dupliquer le **modèle**.

4. Dans la boîte de dialogue Propriétés du nouveau modèle, sous l’onglet **général** , dans **nom complet du modèle**, entrez un nom descriptif pour le serveur VPN, par exemple, authentification du **serveur VPN** ou **serveur RADIUS**.

5. Sous l’onglet **Extensions** , procédez comme suit:

    1. Sélectionnez **stratégies d’application**, puis **modifier**.

    2. Dans la boîte de dialogue **modifier l’extension des stratégies d’application** , sélectionnez **Ajouter**.

    3. Dans la boîte de dialogue **Ajouter une stratégie d’application** , sélectionnez **sécurité IP IKE intermédiaire**, puis cliquez sur **OK**.
   
        L’ajout de la sécurité IP de niveau intermédiaire à l’utilisation améliorée de la sécurité aide dans les scénarios où plusieurs certificats d’authentification de serveur existent sur le serveur VPN. Lorsque la sécurité IP IKE intermédiaire est présente, IPSec utilise uniquement le certificat avec les deux options EKU. Sans cela, l’authentification IKEv2 peut échouer avec l’erreur 13801: Les informations d’authentification IKE ne sont pas acceptables.

    4. Sélectionnez **OK** pour revenir à la boîte **de dialogue Propriétés du nouveau modèle** .

6. Dans l’onglet **sécurité** , procédez comme suit:

    1. Sélectionnez **Ajouter**.

    2. Dans la boîte de dialogue **Sélectionner les utilisateurs, les ordinateurs, les comptes de service ou les groupes** , entrez **serveurs VPN**, puis sélectionnez **OK**.

    3. Dans **noms de groupes ou d’utilisateurs**, sélectionnez **serveurs VPN**.

    4. Dans **autorisations pour les serveurs VPN**, activez la case à cocher **inscrire** dans la colonne **autoriser** .

    5. Dans **noms de groupes ou d’utilisateurs**, sélectionnez **serveurs RAS et IAS**, puis sélectionnez **supprimer**.

7. Dans l’onglet nom de l' **objet** , procédez comme suit:

    1. Sélectionnez **fournir dans la demande**.

    2. Dans la boîte de dialogue d’avertissement **modèles de certificat** , sélectionnez **OK**.

8. Facultatif Si vous configurez l’accès conditionnel pour la connectivité VPN, sélectionnez l’onglet traitement de la **demande** , puis sélectionnez **autoriser l’exportation de la clé privée**.

9. Sélectionnez **OK** pour enregistrer le modèle de certificat de serveur VPN.

10. Fermez la console Modèles de certificat.

11. Dans le volet de navigation du composant logiciel enfichable Autorité de certification, cliquez avec le bouton droit sur **modèles de certificats**, cliquez sur **nouveau** , puis sur **modèle de certificat à**délivrer.

12. Redémarrez les services de l’autorité de certification. (*)

13. Dans le volet de navigation du composant logiciel enfichable Autorité de certification, cliquez avec le bouton droit sur **modèles de certificats**, sélectionnez **nouveau** , puis **modèle de certificat à**délivrer.

14. Sélectionnez le nom que vous avez choisi à l’étape 4 ci-dessus, puis cliquez sur **OK**.

15. Fermez le composant logiciel enfichable Autorité de certification.

* **Vous pouvez arrêter/démarrer le service de l’autorité de certification en exécutant la commande suivante dans CMD:**

```
Net Stop "certsvc"
Net Start "certsvc"
```

## <a name="create-the-nps-server-authentication-template"></a>Créer le modèle d’authentification de serveur NPS

Le troisième et le dernier modèle de certificat à créer est le modèle d’authentification du serveur NPS. Le modèle d’authentification de serveur NPS est une copie simple du modèle de serveur RAS et IAS sécurisée pour le groupe de serveurs NPS que vous avez créé précédemment dans cette section.

Vous allez configurer ce certificat pour l’inscription automatique.

**Procédures**

1. Sur l’autorité de certification, ouvrez autorité de certification.

2. Dans le volet de navigation, cliquez avec le bouton droit sur **modèles de certificats** et sélectionnez **gérer**.

3. Dans la console modèles de certificats, cliquez avec le bouton droit sur **serveur RAS et IAS**, puis sélectionnez Dupliquer le **modèle**.

4. Dans la boîte de dialogue Propriétés du nouveau modèle, sous l’onglet **général** , dans **nom complet du modèle**, tapez authentification du **serveur NPS**.

5. Dans l’onglet **sécurité** , procédez comme suit:

    1. Sélectionnez **Ajouter**.

    2. Dans la boîte de dialogue **Sélectionner les utilisateurs, les ordinateurs, les comptes de service ou les groupes** , entrez **serveurs NPS**, puis sélectionnez **OK**.

    3. Dans **noms de groupes ou d’utilisateurs**, sélectionnez **serveurs NPS**.

    4. Dans **autorisations pour les serveurs NPS**, activez les cases à cocher **inscrire** et **inscription** automatique dans la colonne **autoriser** .

    5. Dans **noms de groupes ou d’utilisateurs**, sélectionnez **serveurs RAS et IAS**, puis sélectionnez **supprimer**.

6. Sélectionnez **OK** pour enregistrer le modèle de certificat de serveur NPS.

7. Fermez la console Modèles de certificat.

8. Dans le volet de navigation du composant logiciel enfichable Autorité de certification, cliquez avec le bouton droit sur **modèles de certificats**, sélectionnez **nouveau** , puis **modèle de certificat à**délivrer.

9. Sélectionnez **authentification du serveur NPS**, puis cliquez sur **OK**.

10. Fermez le composant logiciel enfichable Autorité de certification.

## <a name="enroll-and-validate-the-user-certificate"></a>Inscrire et valider le certificat de l’utilisateur

Étant donné que vous utilisez stratégie de groupe pour inscrire automatiquement des certificats utilisateur, vous devez uniquement mettre à jour la stratégie et Windows 10 inscrit automatiquement le compte d’utilisateur pour le certificat correct. Vous pouvez ensuite valider le certificat dans la console certificats.

**Procédures**

1. Connectez-vous à un ordinateur client joint à un domaine en tant que membre du groupe **utilisateurs VPN** .

2. Appuyez sur la touche Windows + R, tapez **gpupdate/force**, puis appuyez sur entrée.

3. Dans le menu Démarrer, tapez **Certmgr. msc**, puis appuyez sur entrée.

4. Dans le composant logiciel enfichable Certificats, sous **personnel**, sélectionnez **certificats**. Vos certificats s’affichent dans le volet d’informations.

5. Cliquez avec le bouton droit sur le certificat qui contient le nom d’utilisateur de votre domaine actuel, puis sélectionnez **ouvrir**.

6. Sous l’onglet **général** , confirmez que la date indiquée sous **valide à partir de** est la date du jour. Si ce n’est pas le cas, vous avez peut-être sélectionné un certificat incorrect.

7. Sélectionnez **OK**, puis fermez le composant logiciel enfichable Certificats.

## <a name="enroll-and-validate-the-server-certificates"></a>Inscrire et valider les certificats de serveur

Contrairement au certificat de l’utilisateur, vous devez inscrire manuellement le certificat du serveur VPN. Une fois que vous l’avez inscrit, validez-le à l’aide du même processus que celui utilisé pour le certificat de l’utilisateur. À l’instar du certificat utilisateur, le serveur NPS inscrit automatiquement son certificat d’authentification. il vous suffit donc de le valider.

>[!NOTE]
>Vous devrez peut-être redémarrer les serveurs VPN et NPS pour leur permettre de mettre à jour leurs appartenances aux groupes avant de pouvoir effectuer ces étapes.

### <a name="enroll-and-validate-the-vpn-server-certificate"></a>Inscrire et valider le certificat de serveur VPN

1. Dans le menu Démarrer du serveur VPN, tapez **certlm. msc**, puis appuyez sur entrée.

2. Cliquez avec le bouton droit sur **personnel**, sélectionnez **toutes les tâches** , puis sélectionnez **demander un nouveau certificat** pour démarrer l’Assistant inscription de certificats.

3. Dans la page avant de commencer, sélectionnez **suivant**.

4. Dans la page Sélectionner la stratégie d’inscription de certificat, sélectionnez **suivant**.

5. Sur la page demander des certificats, activez la case à cocher en regard du serveur VPN pour le sélectionner.

6. Dans la case à cocher serveur VPN, sélectionnez **des informations supplémentaires sont requises** pour ouvrir la boîte de dialogue Propriétés du certificat et procédez comme suit:

    1. Sélectionnez l’onglet **objet** , sélectionnez **nom commun** sous **nom du sujet**, dans **type**.

    2. Sous **nom du sujet**, dans **valeur**, entrez le nom des clients de domaine externe utilisés pour se connecter au VPN, par exemple, VPN.contoso.com, puis sélectionnez **Ajouter**.

    3. Sous **autre nom**, dans **type**, sélectionnez **DNS**.

    4. Sous **autre nom**, dans **valeur**, entrez tous les noms de serveurs que les clients utilisent pour se connecter au VPN, par exemple, VPN.contoso.com, VPN, 132.64.86.2.

    5. Sélectionnez **Ajouter** après avoir entré chaque nom.

    6. Lorsque vous avez terminé, sélectionnez **OK** .

7. Sélectionnez **inscrire**.

8. Sélectionnez **Terminer**.

9. Dans le composant logiciel enfichable Certificats, sous **personnel**, sélectionnez **certificats**.
    
    Vos certificats répertoriés s’affichent dans le volet d’informations.

10. Cliquez avec le bouton droit sur le certificat qui a le nom de votre serveur VPN, puis sélectionnez **ouvrir**.

11. Sous l’onglet **général** , confirmez que la date indiquée sous **valide à partir de** est la date du jour. Si ce n’est pas le cas, vous avez peut-être sélectionné le certificat incorrect.

12. Dans l’onglet **Détails** , sélectionnez **utilisation améliorée**de la clé, puis vérifiez que l’authentification intermédiaire et **serveur** **IKE de sécurité IP** s’affiche dans la liste.

13. Sélectionnez **OK** pour fermer le certificat.

14. Fermez le composant logiciel enfichable Certificats.

### <a name="validate-the-nps-server-certificate"></a>Valider le certificat de serveur NPS

1. Redémarrez le serveur NPS.

2. Dans le menu Démarrer du serveur NPS, tapez **certlm. msc**, puis appuyez sur entrée.

3. Dans le composant logiciel enfichable Certificats, sous **personnel**, sélectionnez **certificats**.

    Vos certificats répertoriés s’affichent dans le volet d’informations.

4. Cliquez avec le bouton droit sur le certificat qui a le nom de votre serveur NPS, puis sélectionnez **ouvrir**.

5. Sous l’onglet **général** , confirmez que la date indiquée sous **valide à partir de** est la date du jour. Si ce n’est pas le cas, vous avez peut-être sélectionné le certificat incorrect.

6. Sélectionnez **OK** pour fermer le certificat.

7. Fermez le composant logiciel enfichable Certificats.

## <a name="next-steps"></a>Étapes suivantes

[Étape 3. Configurez le serveur d’accès à](vpn-deploy-ras.md)distance pour Always on VPN: Dans cette étape, vous configurez le VPN d’accès à distance pour autoriser les connexions VPN IKEv2, refuser les connexions à partir d’autres protocoles VPN et affecter un pool d’adresses IP statiques pour l’émission d’adresses IP pour la connexion des clients VPN autorisés.
