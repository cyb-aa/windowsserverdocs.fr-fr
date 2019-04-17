---
title: "Composée des revendications de l’authentification et les Services de domaine ActiveDirectory dans ActiveDirectory Federation Services"
description: "Le document suivant explique l’authentification composée et les revendications ADDS dans ADFS."
author: billmath
ms.author: billmath
manager: femila
ms.date: 09/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f5f80cbcbdb2deca9b098014627365b8ea819b5f
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="compound-authentication-and-ad-ds-claims-in-ad-fs"></a>L’authentification composée et les revendications ADDS dans ADFS
Windows Server2012 améliore l’authentification Kerberos en introduisant l’authentification composée.  L’authentification composée permet une demande de service d’accord de tickets Kerberos (TGS) à inclure deux identités: 

- l’identité de l’utilisateur
- l’identité de l’appareil de l’utilisateur.  

Windows effectue l’authentification composée en étendant [Kerberos Authentication Secure Tunneling FAST (Flexible) ou le blindage Kerberos](https://technet.microsoft.com/library/hh831747.aspx). 

ADFS2012 et les versions ultérieures permet la consommation d’ADDS, reçoit des revendications d’utilisateur ou un périphérique qui se trouvent dans un ticket d’authentification Kerberos. Dans les versions antérieures d’ADFS, les revendications moteur peut uniquement lire utilisateur et groupe identificateurs de sécurité (SID) Kerberos mais n’a pas pu lire les informations contenues dans un ticket Kerberos des revendications.

Vous pouvez activer le contrôle d’accès plus riche pour les applications fédérées à l’aide des Services de domaine ActiveDirectory (ADDS)-émis ensemble, des revendications d’utilisateur et périphérique avec ActiveDirectory Federation Services (ADFS).

## <a name="requirements"></a>Configuration requise
1.  Les ordinateurs de l’accès aux applications fédérées, doivent s’authentifier à l’aide des services ADFS **l’authentification intégrée Windows**. 
    - L’authentification intégrée Windows est disponible uniquement lors de la connexion aux serveurs principaux ADFS.
    - Ordinateurs doivent être en mesure d’atteindre les serveurs principaux ADFS pour le nom de Service de fédération
    - ADFS serveurs doit proposer l’authentification intégrée de Windows comme une méthode d’authentification principale dans ses paramètres Intranet.

2.  La stratégie **prise en charge du client Kerberos pour l’authentification composée de revendications et le blindage Kerberos** doivent être appliquées à tous les ordinateurs, l’accès aux applications fédérées qui sont protégées par l’authentification composée. Cela s’applique en cas de forêt unique, ou entre les scénarios de la forêt.

3.  Le domaine hébergeant les serveurs de ADFS doit posséder le **prise en charge pour les revendications de l’authentification composée et le blindage Kerberos** paramètre de stratégie appliqué aux contrôleurs de domaine.

## <a name="steps-for-configuring-ad-fs-in-windows-server-2012-r2"></a>Étapes de configuration ADFS dans Windows Server2012R2
Utilisez les étapes suivantes pour la configuration des revendications et l’authentification composée 

### <a name="step-1--enable-kdc-support-for-claims-compound-authentication-and-kerberos-armoring-on-the-default-domain-controller-policy"></a>Étape1: Activer la prise en charge des revendications, l’authentification composée et le blindage Kerberos sur la stratégie de contrôleur de domaine par défaut
1.  Dans le Gestionnaire de serveur, sélectionnez Outils, **gestion des stratégies de groupe**.
2.  Naviguez vers le bas jusqu'à la **stratégie de contrôleur de domaine par défaut**, avec le bouton droit et sélectionnez **modifier**.
![Gestion des stratégies de groupe](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc1.png)
3.  Sur le **éditeur de gestion de stratégie de groupe**, sous **Configuration ordinateur**, développez **stratégies**, développez **modèles d’administration**, développez **système**, sélectionnez **KDC**.
4.  Dans le volet droit, double-cliquez sur **prise en charge des revendications, l’authentification composée et le blindage Kerberos**.
![Gestion des stratégies de groupe](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc2.png)
5.  Dans la boîte de dialogue Nouveau, définir la prise en charge des revendications à **activé**.
6.  Sous Options, sélectionnez **pris en charge** dans le menu déroulant puis cliquez sur **appliquer** et **OK**.
![Gestion des stratégies de groupe](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc3.png)

### <a name="step-2-enable-kerberos-client-support-for-claims-compound-authentication-and-kerberos-armoring-on-computers-accessing-federated-applications"></a>Étape2: Activer la prise en charge du client Kerberos des revendications, l’authentification composée et le blindage Kerberos sur les ordinateurs de l’accès aux applications fédérées

1.  Sur une stratégie de groupe appliqués aux ordinateurs d’accéder aux applications fédérées, dans le **éditeur de gestion de stratégie de groupe**, sous **Configuration ordinateur**, développez **stratégies**, développez **administration Modèles**, développez **système**, puis sélectionnez **Kerberos**.
2.  Dans le volet droit de la fenêtre Éditeur de gestion de stratégie de groupe, double-cliquez sur **prise en charge du client Kerberos des revendications, l’authentification composée et le blindage Kerberos.**
3.  Dans la boîte de dialogue Nouveau, la valeur prise en charge du client Kerberos **activé** et cliquez sur **appliquer** et **OK**.
![Gestion des stratégies de groupe](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc4.png)
4.  Fermez l’éditeur de gestion de stratégie de groupe.

### <a name="step-3-ensure-the-ad-fs-servers-have-been-updated"></a>Étape3: Vérifiez que les serveurs ADFS ont été mis à jour.
Vous devez vous assurer que les mises à jour suivantes sont installées sur vos serveurs ADFS.

|Mise à jour|Description|
|----- | ----- |
|[KB2919355](https://www.microsoft.com/download/details.aspx?id=42335)|Mise à jour de sécurité cumulative (inclut KB2919355, KB2932046, KB2934018, KB2937592, KB2938439)|
|[KB2959977](https://www.microsoft.com/download/details.aspx?id=42530)|Mise à jour pour Server2012R2|
|[Correctif logiciel 3052122](https://support.microsoft.com/help/3052122/update-adds-support-for-compound-id-claims-in-ad-fs-tokens-in-windows)|Cette mise à jour ajoute la prise en charge des revendications d’ID composées dans ActiveDirectory Federation Services.|

### <a name="step-4-configure-the-primary-authentication-provider"></a>Étape4: Configurer le fournisseur d’authentification principal

1. Définir le fournisseur d’authentification principal **l’authentification Windows** pour les paramètres ADFS Intranet.
2. Dans Gestion ADFS, sous **stratégies d’authentification**, sélectionnez **l’authentification principale** sous **paramètres globaux** cliquez sur **modifier**.
3. Sur **modifier la stratégie d’authentification globale** sous **Intranet** sélectionnez **l’authentification Windows**.
4. Cliquez sur **appliquer** et **Ok**.

![Gestion des stratégies de groupe](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc5.png)

5. À l’aide de PowerShell, vous pouvez utiliser le **Set-AdfsGlobalAuthenticationPolicy** applet de commande.

``` powershell
Set-AdfsGlobalAuthenticationPolicy -PrimaryIntranetAuthenticationProvider 'WindowsAuthentication'
```
>[!NOTE]
>Dans WID basée sur la batterie de serveurs, la commande doit être exécutée sur le serveur principal ADFS de PowerShell.
>Dans une batterie de serveurs SQL en fonction, la commande PowerShell peut être exécutée sur n’importe quel serveur ADFS qui est un membre de la batterie de serveurs.

### <a name="step-5--add-the-claim-description-to-ad-fs"></a>Étape5: Ajouter la description de revendication à ADFS
1.  Ajouter la Description de revendication suivantes à la batterie de serveurs. Cette Description de revendication n’est pas présente par défaut dans ADFS2012R2 et doit être ajouté manuellement.
2.  Dans Gestion ADFS, sous **Service**, avec le bouton droit **description de revendication** et sélectionnez **ajouter description de revendication**
3.  Entrez les informations suivantes dans la description de revendication
    - Nom complet: «Windows groupe de périphériques» 
    - Description de revendication: 'https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsdevicegroup' '
4. Placez une coche dans les deux zones.
5. Cliquez sur **OK**.

![Description de revendication](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc6.png)

6. À l’aide de PowerShell, vous pouvez utiliser le **Add-AdfsClaimDescription** applet de commande.
``` powershell
Add-AdfsClaimDescription -Name 'Windows device group' -ClaimType 'https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsdevicegroup' `
-ShortName 'windowsdevicegroup' -IsAccepted $true -IsOffered $true -IsRequired $false -Notes 'The windows group SID of the device' 
```


>[!NOTE]
>Dans WID basée sur la batterie de serveurs, la commande doit être exécutée sur le serveur principal ADFS de PowerShell.
>Dans une batterie de serveurs SQL en fonction, la commande PowerShell peut être exécutée sur n’importe quel serveur ADFS qui est un membre de la batterie de serveurs.

### <a name="step-6--enable-the-compound-authentication-bit-on-the-msds-supportedencryptiontypes-attribute"></a>Étape6: Activer le bit de l’authentification composée sur l’attribut msDS-SupportedEncryptionTypes

1.  Activer l’authentification composée sur l’attribut msDS-SupportedEncryptionTypes sur le compte désigné pour exécuter le service ADFS à l’aide de bits le **Set-ADServiceAccount** applet de commande PowerShell.  

>[!NOTE]
>Si vous modifiez le compte de service, puis vous devez activer manuellement l’authentification composée en exécutant la **Set-ADUser - compoundIdentitySupported: $true** applets de commande Windows PowerShell.

``` powershell
Set-ADServiceAccount -Identity “ADFS Service Account” -CompoundIdentitySupported:$true 
```
2.  Redémarrez le Service ADFS.

>[!NOTE]
>Une fois que «CompoundIdentitySupported» est définie sur true, installation de la fonctionnalité gMSA même sur le nouveau échoue (2012R2/2016) de serveurs avec l’erreur suivante: **Install-ADServiceAccount: ne peut pas installer de compte de service. Message d’erreur: «le contexte fourni ne correspond pas la cible.»**.
>
>**Solution**: temporairement la valeur $false CompoundIdentitySupported. Cette étape permet d’ADFS arrêter l’émission des revendications WindowsDeviceGroup. Set-ADServiceAccount-identité «Compte de Service ADFS» - CompoundIdentitySupported: $false installer la fonctionnalité gMSA sur le nouveau serveur, puis activez CompoundIdentitySupported à $True.
La désactivation de CompoundIdentitySupported et puis réactivation ne requiert pas de redémarrage du service ADFS.

### <a name="step-7-update-the-ad-fs-claims-provider-trust-for-active-directory"></a>Étape7: Mise à jour du fournisseur de revendications ADFS approbation pour ActiveDirectory

1. Mettre à jour le ADFS revendications approbation de fournisseur pour ActiveDirectory inclure la règle de revendication «Pass-through» suivante de revendication «WindowsDeviceGroup».
2.  Dans **gestion ADFS**, cliquez sur **approbations de fournisseur de revendications** et dans le volet droit, cliquez roite **ActiveDirectory** et sélectionnez **modifier les règles de revendication**.
3.  Sur le **modifier les règles de revendication pour le directeur Active** cliquez sur **ajouter une règle**.
4.  Sur le **ajouter un Assistant de règle de revendication transformer** sélectionnez **passer ou filtrer une revendication entrante** et cliquez sur **suivant**.
5.  Ajoutez un nom d’affichage et sélectionnez **groupe de périphériques Windows** à partir de la **type de revendication entrante** liste déroulante.
6.  Cliquez sur **Terminer**.  Cliquez sur **appliquer** et **Ok**. 
![Description de revendication](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc7.png)

### <a name="step-8-on-the-relying-party-where-the-windowsdevicegroup-claims-are-expected-add-a-similar-pass-through-or-transform-claim-rule"></a>Étape8: Sur la partie de confiance où les revendications «WindowsDeviceGroup» sont prévues, ajoutez une règle de revendication «Pass-through» ou «Transformer» similaire.
2.  Dans **gestion ADFS**, cliquez sur **approbations de partie de confiance** et dans le volet droit, cliquez roite votre identité, puis sélectionnez **modifier les règles de revendication**.
3.  Sur le **règles de transformation d’émission** cliquez sur **ajouter une règle**.
4.  Sur le **ajouter un Assistant de règle de revendication transformer** sélectionnez **passer ou filtrer une revendication entrante** et cliquez sur **suivant**.
5.  Ajoutez un nom d’affichage et sélectionnez **groupe de périphériques Windows** à partir de la **type de revendication entrante** liste déroulante.
6.  Cliquez sur **Terminer**.  Cliquez sur **appliquer** et **Ok**.
![Description de revendication](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc8.png)


##<a name="steps-for-configuring-ad-fs-in-windows-server-2016"></a>Étapes de configuration ADFS dans Windows Server2016
La liste suivante décrit en détail les étapes de configuration de l’authentification composée sur ADFS pour Windows Server2016.

### <a name="step-1--enable-kdc-support-for-claims-compound-authentication-and-kerberos-armoring-on-the-default-domain-controller-policy"></a>Étape1: Activer la prise en charge des revendications, l’authentification composée et le blindage Kerberos sur la stratégie de contrôleur de domaine par défaut
1.  Dans le Gestionnaire de serveur, sélectionnez Outils, **gestion des stratégies de groupe**.
2.  Naviguez vers le bas jusqu'à la **stratégie de contrôleur de domaine par défaut**, avec le bouton droit et sélectionnez **modifier**.
3.  Sur le **éditeur de gestion de stratégie de groupe**, sous **Configuration ordinateur**, développez **stratégies**, développez **modèles d’administration**, développez **système**, sélectionnez **KDC**.
4.  Dans le volet droit, double-cliquez sur **prise en charge des revendications, l’authentification composée et le blindage Kerberos**.
5.  Dans la boîte de dialogue Nouveau, définir la prise en charge des revendications à **activé**.
6.  Sous Options, sélectionnez **pris en charge** dans le menu déroulant puis cliquez sur **appliquer** et **OK**.


### <a name="step-2-enable-kerberos-client-support-for-claims-compound-authentication-and-kerberos-armoring-on-computers-accessing-federated-applications"></a>Étape2: Activer la prise en charge du client Kerberos des revendications, l’authentification composée et le blindage Kerberos sur les ordinateurs de l’accès aux applications fédérées

1.  Sur une stratégie de groupe appliqués aux ordinateurs d’accéder aux applications fédérées, dans le **éditeur de gestion de stratégie de groupe**, sous **Configuration ordinateur**, développez **stratégies**, développez **administration Modèles**, développez **système**, puis sélectionnez **Kerberos**.
2.  Dans le volet droit de la fenêtre Éditeur de gestion de stratégie de groupe, double-cliquez sur **prise en charge du client Kerberos des revendications, l’authentification composée et le blindage Kerberos.**
3.  Dans la boîte de dialogue Nouveau, la valeur prise en charge du client Kerberos **activé** et cliquez sur **appliquer** et **OK**.
4.  Fermez l’éditeur de gestion de stratégie de groupe.

### <a name="step-3-configure-the-primary-authentication-provider"></a>Étape3: Configurer le fournisseur d’authentification principal

1. Définir le fournisseur d’authentification principal **l’authentification Windows** pour les paramètres ADFS Intranet.
2. Dans Gestion ADFS, sous **stratégies d’authentification**, sélectionnez **l’authentification principale** sous **paramètres globaux** cliquez sur **modifier**.
3. Sur **modifier la stratégie d’authentification globale** sous **Intranet** sélectionnez **l’authentification Windows**.
4. Cliquez sur **appliquer** et **Ok**.
5. À l’aide de PowerShell, vous pouvez utiliser le **Set-AdfsGlobalAuthenticationPolicy** applet de commande.

``` powershell
Set-AdfsGlobalAuthenticationPolicy -PrimaryIntranetAuthenticationProvider 'WindowsAuthentication'
```
>[!NOTE]
>Dans WID basée sur la batterie de serveurs, la commande doit être exécutée sur le serveur principal ADFS de PowerShell.
>Dans une batterie de serveurs SQL en fonction, la commande PowerShell peut être exécutée sur n’importe quel serveur ADFS qui est un membre de la batterie de serveurs.

### <a name="step-4--enable-the-compound-authentication-bit-on-the-msds-supportedencryptiontypes-attribute"></a>Étape4: Activer le bit de l’authentification composée sur l’attribut msDS-SupportedEncryptionTypes

1.  Activer l’authentification composée sur l’attribut msDS-SupportedEncryptionTypes sur le compte désigné pour exécuter le service ADFS à l’aide de bits le **Set-ADServiceAccount** applet de commande PowerShell.  

>[!NOTE]
>Si vous modifiez le compte de service, puis vous devez activer manuellement l’authentification composée en exécutant la **Set-ADUser - compoundIdentitySupported: $true** applets de commande Windows PowerShell.

``` powershell
Set-ADServiceAccount -Identity “ADFS Service Account” -CompoundIdentitySupported:$true 
```
2.  Redémarrez le Service ADFS.

>[!NOTE]
>Une fois que «CompoundIdentitySupported» est définie sur true, installation de la fonctionnalité gMSA même sur le nouveau échoue (2012R2/2016) de serveurs avec l’erreur suivante: **Install-ADServiceAccount: ne peut pas installer de compte de service. Message d’erreur: «le contexte fourni ne correspond pas la cible.»**.
>
>**Solution**: temporairement la valeur $false CompoundIdentitySupported. Cette étape permet d’ADFS arrêter l’émission des revendications WindowsDeviceGroup. Set-ADServiceAccount-identité «Compte de Service ADFS» - CompoundIdentitySupported: $false installer la fonctionnalité gMSA sur le nouveau serveur, puis activez CompoundIdentitySupported à $True.
La désactivation de CompoundIdentitySupported et puis réactivation ne requiert pas de redémarrage du service ADFS.

### <a name="step-5-update-the-ad-fs-claims-provider-trust-for-active-directory"></a>Étape5: Mise à jour du fournisseur de revendications ADFS approbation pour ActiveDirectory

1. Mettre à jour le ADFS revendications approbation de fournisseur pour ActiveDirectory inclure la règle de revendication «Pass-through» suivante de revendication «WindowsDeviceGroup».
2.  Dans **gestion ADFS**, cliquez sur **approbations de fournisseur de revendications** et dans le volet droit, cliquez roite **ActiveDirectory** et sélectionnez **modifier les règles de revendication**.
3.  Sur le **modifier les règles de revendication pour le directeur Active** cliquez sur **ajouter une règle**.
4.  Sur le **ajouter un Assistant de règle de revendication transformer** sélectionnez **passer ou filtrer une revendication entrante** et cliquez sur **suivant**.
5.  Ajoutez un nom d’affichage et sélectionnez **groupe de périphériques Windows** à partir de la **type de revendication entrante** liste déroulante.
6.  Cliquez sur **Terminer**.  Cliquez sur **appliquer** et **Ok**. 


### <a name="step-6-on-the-relying-party-where-the-windowsdevicegroup-claims-are-expected-add-a-similar-pass-through-or-transform-claim-rule"></a>Étape6: Sur la partie de confiance où les revendications «WindowsDeviceGroup» sont prévues, ajoutez une règle de revendication «Pass-through» ou «Transformer» similaire.
2.  Dans **gestion ADFS**, cliquez sur **approbations de partie de confiance** et dans le volet droit, cliquez roite votre identité, puis sélectionnez **modifier les règles de revendication**.
3.  Sur le **règles de transformation d’émission** cliquez sur **ajouter une règle**.
4.  Sur le **ajouter un Assistant de règle de revendication transformer** sélectionnez **passer ou filtrer une revendication entrante** et cliquez sur **suivant**.
5.  Ajoutez un nom d’affichage et sélectionnez **groupe de périphériques Windows** à partir de la **type de revendication entrante** liste déroulante.
6.  Cliquez sur **Terminer**.  Cliquez sur **appliquer** et **Ok**.

## <a name="validation"></a>Validation
Pour valider la version de revendications «WindowsDeviceGroup», créez un test de revendications Application prenant en charge à l’aide de .net 4.6. Avec le Kit de développement WIF 4.0.
Configurer l’Application en tant qu’une partie de confiance dans ADFS et sa mise à jour avec la règle de revendication comme spécifié dans les étapes ci-dessus.
Lors de l’authentification à l’Application à l’aide du fournisseur d’authentification intégrée de Windows d’ADFS, les revendications suivantes sont associées.
![Validation](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc9.png)

Les revendications pour l’ordinateur/appareil peuvent maintenant être consommées pour des contrôles d’accès plus riches.

Par exemple, les éléments suivants **AdditionalAuthenticationRules** indique à ADFS pour appeler l’authentification Multifacteur si l’utilisateur d’authentification n’est pas membre du groupe de sécurité «-1-5-21-2134745077-1211275016-3050530490-1117» et l’ordinateur (où l’utilisateur est L’authentification à partir de) n’est pas membre du groupe de sécurité «S-1-5-21-2134745077-1211275016-3050530490-1115 (WindowsDeviceGroup)»

Toutefois, si une des conditions ci-dessus sont remplie, n’appellent pas l’authentification Multifacteur.

```
'NOT EXISTS([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsdevicegroup", Value =~ "S-1-5-21-2134745077-1211275016-3050530490-1115"])
&& NOT EXISTS([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "S-1-5-21-2134745077-1211275016-3050530490-1117"])
=> issue(Type = "https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", Value = "https://schemas.microsoft.com/claims/multipleauthn");'
```