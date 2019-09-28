---
title: Authentification composée et revendications de Active Directory Domain Services dans Services ADFS
description: Le document suivant décrit l’authentification composée et les revendications de AD DS dans AD FS.
author: billmath
ms.author: billmath
manager: femila
ms.date: 09/07/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 78db6f8b6961cecea55b8d371e9abf952cafdab3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358669"
---
# <a name="compound-authentication-and-ad-ds-claims-in-ad-fs"></a>Authentification composée et revendications AD DS dans AD FS
Windows Server 2012 améliore l’authentification Kerberos en introduisant l’authentification composée.  L’authentification composée permet à une demande du service d’accord de ticket (TGS) Kerberos d’inclure deux identités : 

- identité de l’utilisateur
- identité de l’appareil de l’utilisateur.  

Windows effectue l’authentification composée en étendant le [blindage Kerberos (flexible Authentication Secure Tunneling) ou le blindage Kerberos](https://technet.microsoft.com/library/hh831747.aspx). 

AD FS 2012 et versions ultérieures autorisent la consommation de AD DS des revendications d’utilisateur ou de périphérique émises qui résident dans un ticket d’authentification Kerberos. Dans les versions précédentes de AD FS, le moteur de revendications pouvait uniquement lire les identificateurs de sécurité (SID) d’utilisateur et de groupe à partir de Kerberos, mais n’a pas pu lire les informations de revendications contenues dans un ticket Kerberos.

Vous pouvez activer le contrôle d’accès plus riche pour les applications fédérées à l’aide de revendications d’utilisateur et d’appareil émises par Active Directory Domain Services (AD DS), avec Services ADFS (AD FS).

## <a name="requirements"></a>Configuration requise
1.  Les ordinateurs qui accèdent à des applications fédérées doivent s’authentifier auprès de AD FS à l’aide de l' **authentification intégrée de Windows**. 
    - L’authentification intégrée de Windows n’est disponible que lors de la connexion aux serveurs de AD FS backend.
    - Les ordinateurs doivent être en mesure d’atteindre les serveurs de AD FS backend pour le nom de service FS (Federation Service)
    - Les serveurs AD FS doivent proposer l’authentification intégrée de Windows comme méthode d’authentification principale dans ses paramètres intranet.

2.  La stratégie **prise en charge par le client Kerberos pour l’authentification composée de revendications et le blindage Kerberos** doit être appliquée à tous les ordinateurs qui accèdent à des applications fédérées qui sont protégées par l’authentification composée. Cela s’applique en cas de scénarios à forêt unique ou inter-forêts.

3.  Le domaine hébergeant les serveurs AD FS doit avoir la **prise en charge du KDC pour l’authentification composée des revendications et** le paramètre de stratégie de blindage Kerberos appliqué aux contrôleurs de domaine.

## <a name="steps-for-configuring-ad-fs-in-windows-server-2012-r2"></a>Étapes de configuration de AD FS dans Windows Server 2012 R2
Utilisez les étapes suivantes pour configurer l’authentification composée et les revendications 

### <a name="step-1--enable-kdc-support-for-claims-compound-authentication-and-kerberos-armoring-on-the-default-domain-controller-policy"></a>Étape 1 :  Activer la prise en charge du KDC pour les revendications, l’authentification composée et le blindage Kerberos sur la stratégie de contrôleur de domaine par défaut
1.  Dans Gestionnaire de serveur, sélectionnez Outils, **gestion des stratégie de groupe**.
2.  Accédez à la **stratégie de contrôleur de domaine par défaut**, cliquez avec le bouton droit et sélectionnez **modifier**.
![Gestion des stratégie de groupe](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc1.png)
3.  Sur la **éditeur de gestion des stratégies de groupe**, sous **Configuration ordinateur**, développez **stratégies**, **modèles d’administration**, **système**, puis sélectionnez **KDC**.
4.  Dans le volet droit, double-cliquez sur **prise en charge du KDC pour les revendications, l’authentification composée et le blindage Kerberos**.
![Gestion des stratégie de groupe](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc2.png)
5.  Dans la fenêtre nouvelle boîte de dialogue, définissez prise en charge du KDC pour les revendications sur **activé**.
6.  Sous options, sélectionnez **pris en charge** dans le menu déroulant, puis cliquez sur **appliquer** , puis sur **OK**.
![Gestion des stratégie de groupe](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc3.png)

### <a name="step-2-enable-kerberos-client-support-for-claims-compound-authentication-and-kerberos-armoring-on-computers-accessing-federated-applications"></a>Étape 2 : Activer la prise en charge du client Kerberos pour les revendications, l’authentification composée et le blindage Kerberos sur les ordinateurs qui accèdent à des applications fédérées

1.  Sur un stratégie de groupe appliqué aux ordinateurs qui accèdent à des applications fédérées, dans la **éditeur de gestion des stratégies de groupe**, sous **Configuration ordinateur**, développez **stratégies**, développez **modèles d’administration**, puis **système.** , puis sélectionnez **Kerberos**.
2.  Dans le volet droit de la fenêtre Éditeur de gestion des stratégies de groupe, double-cliquez sur **prise en charge du client Kerberos pour les revendications, l’authentification composée et le blindage Kerberos.**
3.  Dans la fenêtre nouvelle boîte de dialogue, attribuez à prise en charge du client Kerberos la valeur **activé** , puis cliquez sur **appliquer** , puis sur **OK**.
![Gestion des stratégie de groupe](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc4.png)
4.  Fermez l’éditeur de gestion des stratégies de groupe.

### <a name="step-3-ensure-the-ad-fs-servers-have-been-updated"></a>Étape 3 : Assurez-vous que les serveurs AD FS ont été mis à jour.
Vous devez vous assurer que les mises à jour suivantes sont installées sur vos serveurs de AD FS.

|Mettre à jour/Mise à jour|Description|
|----- | ----- |
|[KB2919355](https://www.microsoft.com/download/details.aspx?id=42335)|Mise à jour de sécurité cumulative (comprend KB2919355, KB2932046, KB2934018, KB2937592, KB2938439)|
|[KB2959977](https://www.microsoft.com/download/details.aspx?id=42530)|Mise à jour pour le serveur 2012 R2|
|[Correctif logiciel 3052122](https://support.microsoft.com/help/3052122/update-adds-support-for-compound-id-claims-in-ad-fs-tokens-in-windows)|Cette mise à jour ajoute la prise en charge des revendications d’ID composés dans Services ADFS.|

### <a name="step-4-configure-the-primary-authentication-provider"></a>Étape 4 : Configurer le fournisseur d’authentification principal

1. Définissez le fournisseur d’authentification principal sur **authentification Windows** pour AD FS paramètres intranet.
2. Dans AD FS gestion, sous **stratégies d’authentification**, sélectionnez **authentification principale** et sous **paramètres globaux** , cliquez sur **modifier**.
3. Dans **modifier la stratégie d’authentification globale** sous **Intranet** , sélectionnez **authentification Windows**.
4. Cliquez sur **appliquer** , puis sur **OK**.

![Gestion des stratégies de groupe](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc5.png)

5. À l’aide de PowerShell, vous pouvez utiliser l’applet de commande **Set-AdfsGlobalAuthenticationPolicy** .

``` powershell
Set-AdfsGlobalAuthenticationPolicy -PrimaryIntranetAuthenticationProvider 'WindowsAuthentication'
```
>[!NOTE]
>Dans une batterie de serveurs WID, la commande PowerShell doit être exécutée sur le serveur de AD FS principal.
>Dans une batterie de serveurs SQL, la commande PowerShell peut être exécutée sur n’importe quel serveur de AD FS membre de la batterie de serveurs.

### <a name="step-5--add-the-claim-description-to-ad-fs"></a>Étape 5 :  Ajoutez la description de la revendication à AD FS
1. Ajoutez la description de revendication suivante à la batterie de serveurs. Cette description de revendication n’est pas présente par défaut dans ADFS 2012 R2 et doit être ajoutée manuellement.
2. Dans AD FS gestion, sous **service**, cliquez avec le bouton droit sur Description de la **revendication** et sélectionnez **Ajouter une description de revendication** .
3. Entrez les informations suivantes dans la description de la revendication
   - Nom complet : « Groupe de périphériques Windows » 
   - Description de la revendication<https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsdevicegroup>: ' ' '
4. Activez les deux cases.
5. Cliquez sur **OK**.

![Description de la revendication](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc6.png)

6. À l’aide de PowerShell, vous pouvez utiliser l’applet **de commande Add-AdfsClaimDescription** .
   ``` powershell
   Add-AdfsClaimDescription -Name 'Windows device group' -ClaimType 'https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsdevicegroup' `
   -ShortName 'windowsdevicegroup' -IsAccepted $true -IsOffered $true -IsRequired $false -Notes 'The windows group SID of the device' 
   ```


>[!NOTE]
>Dans une batterie de serveurs WID, la commande PowerShell doit être exécutée sur le serveur de AD FS principal.
>Dans une batterie de serveurs SQL, la commande PowerShell peut être exécutée sur n’importe quel serveur de AD FS membre de la batterie de serveurs.

### <a name="step-6--enable-the-compound-authentication-bit-on-the-msds-supportedencryptiontypes-attribute"></a>Étape 6 :  Activer le bit d’authentification composée sur l’attribut msDS-SupportedEncryptionTypes

1.  Activez le bit d’authentification composée sur l’attribut msDS-SupportedEncryptionTypes sur le compte que vous avez désigné pour exécuter le service AD FS à l’aide de l’applet de commande PowerShell **Set-ADServiceAccount** .  

>[!NOTE]
>Si vous modifiez le compte de service, vous devez activer manuellement l’authentification composée en exécutant les applets de commande Windows PowerShell **Set-ADUser-compoundIdentitySupported : $true** .

``` powershell
Set-ADServiceAccount -Identity “ADFS Service Account” -CompoundIdentitySupported:$true 
```
2. Redémarrez le service ADFS.

>[!NOTE]
>Une fois que « CompoundIdentitySupported » a la valeur true, l’installation du même gMSA sur les nouveaux serveurs (2012 R2/2016) échoue avec l' **erreur suivante : Install-ADServiceAccount : Impossible d’installer le compte de service. Message d'erreur : 'Le contexte fourni ne correspond pas à la cible. '** .
>
>**Solution** : Définissez temporairement CompoundIdentitySupported sur $false. Cette étape amène ADFS à arrêter l’émission de revendications WindowsDeviceGroup. Set-ADServiceAccount-Identity’ADFS service account'-CompoundIdentitySupported : $false installer gMSA sur le nouveau serveur, puis réactiver CompoundIdentitySupported sur $True.
La désactivation de CompoundIdentitySupported, puis la réactivation de ne nécessite pas le redémarrage du service ADFS.

### <a name="step-7-update-the-ad-fs-claims-provider-trust-for-active-directory"></a>Étape 7 : Mettez à jour l’approbation de fournisseur de revendications AD FS pour Active Directory

1. Mettez à jour l’approbation de fournisseur de revendications AD FS pour Active Directory pour inclure la règle de revendication « directe » suivante pour la revendication « WindowsDeviceGroup ».
2.  Dans **AD FS gestion**, cliquez sur **approbations de fournisseur de revendications** et, dans le volet droit, Copy sur **Active Directory** et sélectionnez Modifier les **règles de revendication**.
3.  Sur la **touche modifier les règles de revendication pour active Director** , cliquez sur **Ajouter une règle**.
4.  Dans l' **Assistant Ajout** d’une règle de revendication de transformation, sélectionnez **transférer ou filtrer une revendication entrante** , puis cliquez sur **suivant**.
5.  Ajoutez un nom complet et sélectionnez **groupe de périphériques Windows** dans la liste déroulante **type de revendication entrante** .
6.  Cliquez sur **Terminer**.  Cliquez sur **appliquer** , puis sur **OK**. 
![Description de la revendication](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc7.png)

### <a name="step-8-on-the-relying-party-where-the-windowsdevicegroup-claims-are-expected-add-a-similar-pass-through-or-transform-claim-rule"></a>Étape 8 : Sur la partie de confiance où les revendications « WindowsDeviceGroup » sont attendues, ajoutez une règle de revendication « directe » ou « transformation » similaire.
2. Dans **AD FS gestion**, cliquez sur **approbations de partie de confiance** , puis dans le volet droit, Copy-cliquez sur votre RP et sélectionnez Modifier les règles de **revendication**.
3. Dans les **règles de transformation d’émission** , cliquez sur **Ajouter une règle**.
4. Dans l' **Assistant Ajout** d’une règle de revendication de transformation, sélectionnez **transférer ou filtrer une revendication entrante** , puis cliquez sur **suivant**.
5. Ajoutez un nom complet et sélectionnez **groupe de périphériques Windows** dans la liste déroulante **type de revendication entrante** .
6. Cliquez sur **Terminer**.  Cliquez sur **appliquer** , puis sur **OK**.
   ![Description de la revendication](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc8.png)


## <a name="steps-for-configuring-ad-fs-in-windows-server-2016"></a>Étapes de configuration de AD FS dans Windows Server 2016
Vous trouverez ci-dessous les étapes de la configuration de l’authentification composée sur AD FS pour Windows Server 2016.

### <a name="step-1--enable-kdc-support-for-claims-compound-authentication-and-kerberos-armoring-on-the-default-domain-controller-policy"></a>Étape 1 :  Activer la prise en charge du KDC pour les revendications, l’authentification composée et le blindage Kerberos sur la stratégie de contrôleur de domaine par défaut
1.  Dans Gestionnaire de serveur, sélectionnez Outils, **gestion des stratégie de groupe**.
2.  Accédez à la **stratégie de contrôleur de domaine par défaut**, cliquez avec le bouton droit et sélectionnez **modifier**.
3.  Sur la **éditeur de gestion des stratégies de groupe**, sous **Configuration ordinateur**, développez **stratégies**, **modèles d’administration**, **système**, puis sélectionnez **KDC**.
4.  Dans le volet droit, double-cliquez sur **prise en charge du KDC pour les revendications, l’authentification composée et le blindage Kerberos**.
5.  Dans la fenêtre nouvelle boîte de dialogue, définissez prise en charge du KDC pour les revendications sur **activé**.
6.  Sous options, sélectionnez **pris en charge** dans le menu déroulant, puis cliquez sur **appliquer** , puis sur **OK**.


### <a name="step-2-enable-kerberos-client-support-for-claims-compound-authentication-and-kerberos-armoring-on-computers-accessing-federated-applications"></a>Étape 2 : Activer la prise en charge du client Kerberos pour les revendications, l’authentification composée et le blindage Kerberos sur les ordinateurs qui accèdent à des applications fédérées

1.  Sur un stratégie de groupe appliqué aux ordinateurs qui accèdent à des applications fédérées, dans la **éditeur de gestion des stratégies de groupe**, sous **Configuration ordinateur**, développez **stratégies**, développez **modèles d’administration**, puis **système.** , puis sélectionnez **Kerberos**.
2.  Dans le volet droit de la fenêtre Éditeur de gestion des stratégies de groupe, double-cliquez sur **prise en charge du client Kerberos pour les revendications, l’authentification composée et le blindage Kerberos.**
3.  Dans la fenêtre nouvelle boîte de dialogue, attribuez à prise en charge du client Kerberos la valeur **activé** , puis cliquez sur **appliquer** , puis sur **OK**.
4.  Fermez l’éditeur de gestion des stratégies de groupe.

### <a name="step-3-configure-the-primary-authentication-provider"></a>Étape 3 : Configurer le fournisseur d’authentification principal

1. Définissez le fournisseur d’authentification principal sur **authentification Windows** pour AD FS paramètres intranet.
2. Dans AD FS gestion, sous **stratégies d’authentification**, sélectionnez **authentification principale** et sous **paramètres globaux** , cliquez sur **modifier**.
3. Dans **modifier la stratégie d’authentification globale** sous **Intranet** , sélectionnez **authentification Windows**.
4. Cliquez sur **appliquer** , puis sur **OK**.
5. À l’aide de PowerShell, vous pouvez utiliser l’applet de commande **Set-AdfsGlobalAuthenticationPolicy** .

``` powershell
Set-AdfsGlobalAuthenticationPolicy -PrimaryIntranetAuthenticationProvider 'WindowsAuthentication'
```
>[!NOTE]
>Dans une batterie de serveurs WID, la commande PowerShell doit être exécutée sur le serveur de AD FS principal.
>Dans une batterie de serveurs SQL, la commande PowerShell peut être exécutée sur n’importe quel serveur de AD FS membre de la batterie de serveurs.

### <a name="step-4--enable-the-compound-authentication-bit-on-the-msds-supportedencryptiontypes-attribute"></a>Étape 4 :  Activer le bit d’authentification composée sur l’attribut msDS-SupportedEncryptionTypes

1.  Activez le bit d’authentification composée sur l’attribut msDS-SupportedEncryptionTypes sur le compte que vous avez désigné pour exécuter le service AD FS à l’aide de l’applet de commande PowerShell **Set-ADServiceAccount** .  

>[!NOTE]
>Si vous modifiez le compte de service, vous devez activer manuellement l’authentification composée en exécutant les applets de commande Windows PowerShell **Set-ADUser-compoundIdentitySupported : $true** .

``` powershell
Set-ADServiceAccount -Identity “ADFS Service Account” -CompoundIdentitySupported:$true 
```
2. Redémarrez le service ADFS.

>[!NOTE]
>Une fois que « CompoundIdentitySupported » a la valeur true, l’installation du même gMSA sur les nouveaux serveurs (2012 R2/2016) échoue avec l' **erreur suivante : Install-ADServiceAccount : Impossible d’installer le compte de service. Message d'erreur : 'Le contexte fourni ne correspond pas à la cible. '** .
>
>**Solution** : Définissez temporairement CompoundIdentitySupported sur $false. Cette étape amène ADFS à arrêter l’émission de revendications WindowsDeviceGroup. Set-ADServiceAccount-Identity’ADFS service account'-CompoundIdentitySupported : $false installer gMSA sur le nouveau serveur, puis réactiver CompoundIdentitySupported sur $True.
La désactivation de CompoundIdentitySupported, puis la réactivation de ne nécessite pas le redémarrage du service ADFS.

### <a name="step-5-update-the-ad-fs-claims-provider-trust-for-active-directory"></a>Étape 5 : Mettez à jour l’approbation de fournisseur de revendications AD FS pour Active Directory

1. Mettez à jour l’approbation de fournisseur de revendications AD FS pour Active Directory pour inclure la règle de revendication « directe » suivante pour la revendication « WindowsDeviceGroup ».
2.  Dans **AD FS gestion**, cliquez sur **approbations de fournisseur de revendications** et, dans le volet droit, Copy sur **Active Directory** et sélectionnez Modifier les **règles de revendication**.
3.  Sur la **touche modifier les règles de revendication pour active Director** , cliquez sur **Ajouter une règle**.
4.  Dans l' **Assistant Ajout** d’une règle de revendication de transformation, sélectionnez **transférer ou filtrer une revendication entrante** , puis cliquez sur **suivant**.
5.  Ajoutez un nom complet et sélectionnez **groupe de périphériques Windows** dans la liste déroulante **type de revendication entrante** .
6.  Cliquez sur **Terminer**.  Cliquez sur **appliquer** , puis sur **OK**. 


### <a name="step-6-on-the-relying-party-where-the-windowsdevicegroup-claims-are-expected-add-a-similar-pass-through-or-transform-claim-rule"></a>Étape 6 : Sur la partie de confiance où les revendications « WindowsDeviceGroup » sont attendues, ajoutez une règle de revendication « directe » ou « transformation » similaire.
2. Dans **AD FS gestion**, cliquez sur **approbations de partie de confiance** , puis dans le volet droit, Copy-cliquez sur votre RP et sélectionnez Modifier les règles de **revendication**.
3. Dans les **règles de transformation d’émission** , cliquez sur **Ajouter une règle**.
4. Dans l' **Assistant Ajout** d’une règle de revendication de transformation, sélectionnez **transférer ou filtrer une revendication entrante** , puis cliquez sur **suivant**.
5. Ajoutez un nom complet et sélectionnez **groupe de périphériques Windows** dans la liste déroulante **type de revendication entrante** .
6. Cliquez sur **Terminer**.  Cliquez sur **appliquer** , puis sur **OK**.

## <a name="validation"></a>Validation
Pour valider la publication des revendications « WindowsDeviceGroup », créez une application de prise en charge des revendications de test à l’aide de .net 4,6. Avec WIF SDK 4,0.
Configurez l’application en tant que partie de confiance dans ADFS et mettez-la à jour avec la règle de revendication comme indiqué dans les étapes ci-dessus.
Lors de l’authentification auprès de l’application à l’aide du fournisseur d’authentification intégrée Windows ADFS, les revendications suivantes sont converties.
![Métrage](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc9.png)

Les revendications pour l’ordinateur/périphérique peuvent maintenant être consommées pour des contrôles d’accès plus riches.

Par exemple, le **AdditionalAuthenticationRules** suivant indique à AD FS d’appeler MFA si : l’utilisateur d’authentification n’est pas membre du groupe de sécurité « -1-5-21-2134745077-1211275016-3050530490-1117 » et l’ordinateur (où est l’utilisateur L’authentification à partir de) n’est pas membre du groupe de sécurité « S-1-5-21-2134745077-1211275016-3050530490-1115 (WindowsDeviceGroup) »

Toutefois, si l’une des conditions ci-dessus est remplie, n’appelez pas MFA.

```
'NOT EXISTS([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsdevicegroup", Value =~ "S-1-5-21-2134745077-1211275016-3050530490-1115"])
&& NOT EXISTS([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "S-1-5-21-2134745077-1211275016-3050530490-1117"])
=> issue(Type = "https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", Value = "https://schemas.microsoft.com/claims/multipleauthn");'
```
