---
title: Composée des revendications de l’authentification et les Services de domaine Active Directory dans Active Directory Federation Services
description: Le document suivant explique l’authentification composée et les revendications AD DS dans AD FS.
author: billmath
ms.author: billmath
manager: femila
ms.date: 09/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 270fb6efd63e6355c410ee45d09e6fd16b14222b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867990"
---
# <a name="compound-authentication-and-ad-ds-claims-in-ad-fs"></a>Authentification composée et revendications AD DS dans AD FS
Windows Server 2012 améliore l’authentification Kerberos en introduisant l’authentification composée.  L’authentification composée permet à une demande de Service d’accord de tickets Kerberos (TGS) à inclure deux identités : 

- l’identité de l’utilisateur
- l’identité d’appareil de l’utilisateur.  

Windows effectue l’authentification composée en étendant [Kerberos FAST Flexible Authentication Secure Tunneling (), ou le blindage Kerberos](https://technet.microsoft.com/library/hh831747.aspx). 

AD FS 2012 et versions ultérieures permet la consommation des services AD DS, reçoit des revendications utilisateur ou périphérique qui résident dans un ticket d’authentification Kerberos. Dans les versions précédentes d’AD FS, les revendications moteur peut uniquement lire utilisateur et groupe ID de sécurité (SID) Kerberos, mais n’a pas pu lire les informations contenues dans un ticket Kerberos des revendications.

Vous pouvez activer le contrôle d’accès plus riche pour les applications fédérées à l’aide des Services de domaine Active Directory (AD DS)-émis ensemble, les revendications d’utilisateur et appareil avec Active Directory Federation Services (ADFS).

## <a name="requirements"></a>Configuration requise
1.  Les ordinateurs l’accès aux applications fédérées, doivent s’authentifier à l’aide des services AD FS **l’authentification intégrée de Windows**. 
    - L’authentification intégrée Windows est disponible uniquement lors de la connexion aux serveurs principaux AD FS.
    - Ordinateurs doivent être en mesure d’atteindre les serveurs principaux AD FS pour le nom de Service de fédération
    - Serveurs AD FS doivent proposer l’authentification intégrée Windows comme une méthode d’authentification principale dans ses paramètres Intranet.

2.  La stratégie **prise en charge de client Kerberos pour l’authentification composée de revendications et le blindage Kerberos** doivent être appliquées à tous les ordinateurs l’accès aux applications fédérées qui sont protégées par l’authentification composée. Cela s’applique en cas de forêt unique ou les scénarios de forêt inter-domaines.

3.  Le domaine qui héberge les serveurs AD FS doit avoir le **prise en charge pour les revendications l’authentification composée et le blindage Kerberos** paramètre de stratégie appliqué aux contrôleurs de domaine.

## <a name="steps-for-configuring-ad-fs-in-windows-server-2012-r2"></a>Procédure de configuration AD FS dans Windows Server 2012 R2
Utilisez les étapes suivantes pour la configuration des revendications et l’authentification composée 

### <a name="step-1--enable-kdc-support-for-claims-compound-authentication-and-kerberos-armoring-on-the-default-domain-controller-policy"></a>Étape 1 :  Activer la prise en charge du contrôleur de domaine Kerberos des revendications, l’authentification composée et le blindage Kerberos sur la stratégie de contrôleur de domaine par défaut
1.  Dans le Gestionnaire de serveur, sélectionnez Outils, **Group Policy Management**.
2.  Naviguez vers le bas jusqu'à la **stratégie des contrôleurs de domaine par défaut**, avec le bouton droit et sélectionnez **modifier**.
![Gestion des stratégies de groupe](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc1.png)
3.  Sur le **éditeur de gestion de stratégie de groupe**, sous **Configuration ordinateur**, développez **stratégies**, développez **modèles d’administration** , développez **système**, puis sélectionnez **KDC**.
4.  Dans le volet droit, double-cliquez sur **prise en charge des revendications, l’authentification composée et le blindage Kerberos**.
![Gestion des stratégies de groupe](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc2.png)
5.  Dans la nouvelle fenêtre de boîte de dialogue, définissez la prise en charge de contrôleur de domaine Kerberos des revendications à **activé**.
6.  Sous Options, sélectionnez **pris en charge** dans le menu déroulant puis cliquez sur **appliquer** et **OK**.
![Gestion des stratégies de groupe](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc3.png)

### <a name="step-2-enable-kerberos-client-support-for-claims-compound-authentication-and-kerberos-armoring-on-computers-accessing-federated-applications"></a>Étape 2 : Activer la prise en charge de client Kerberos des revendications, l’authentification composée et le blindage Kerberos sur les ordinateurs l’accès aux applications fédérées

1.  Sur une stratégie de groupe appliqués aux ordinateurs l’accès aux applications fédérées, dans le **éditeur de gestion de stratégie de groupe**, sous **Configuration ordinateur**, développez **stratégies**, développez **modèles d’administration**, développez **système**, puis sélectionnez **Kerberos**.
2.  Dans le volet droit de la fenêtre Éditeur de gestion de stratégie de groupe, double-cliquez sur **prise en charge de client Kerberos des revendications, l’authentification composée et le blindage Kerberos.**
3.  Dans la nouvelle fenêtre de boîte de dialogue, la valeur est prise en charge de Kerberos client **activé** et cliquez sur **appliquer** et **OK**.
![Gestion des stratégies de groupe](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc4.png)
4.  Fermez l’éditeur de gestion des stratégies de groupe.

### <a name="step-3-ensure-the-ad-fs-servers-have-been-updated"></a>Étape 3 : Assurez-vous que les serveurs AD FS ont été mis à jour.
Vous devez vous assurer que les mises à jour suivantes sont installés sur vos serveurs AD FS.

|Mettre à jour/Mise à jour|Description|
|----- | ----- |
|[KB2919355](https://www.microsoft.com/download/details.aspx?id=42335)|Mise à jour cumulative (inclut KB2919355, KB2932046, KB2934018, KB2937592, KB2938439)|
|[KB2959977](https://www.microsoft.com/download/details.aspx?id=42530)|Mise à jour pour Server 2012 R2|
|[Correctif logiciel 3052122](https://support.microsoft.com/help/3052122/update-adds-support-for-compound-id-claims-in-ad-fs-tokens-in-windows)|Cette mise à jour ajoute la prise en charge des revendications d’ID composées dans Active Directory Federation Services.|

### <a name="step-4-configure-the-primary-authentication-provider"></a>Étape 4 : Configurer le fournisseur d’authentification principale

1. La valeur est le fournisseur d’authentification principal **l’authentification Windows** pour les paramètres AD FS Intranet.
2. Dans Gestion AD FS, sous **stratégies d’authentification**, sélectionnez **l’authentification principale** et sous **paramètres globaux** cliquez sur **modifier**.
3. Sur **modifier la stratégie d’authentification globale** sous **Intranet** sélectionnez **l’authentification Windows**.
4. Cliquez sur **appliquer** et **Ok**.

![Gestion des stratégies de groupe](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc5.png)

5. À l’aide de PowerShell, vous pouvez utiliser la **Set-AdfsGlobalAuthenticationPolicy** applet de commande.

``` powershell
Set-AdfsGlobalAuthenticationPolicy -PrimaryIntranetAuthenticationProvider 'WindowsAuthentication'
```
>[!NOTE]
>Dans un schéma WID basé sur batterie de serveurs, la commande doit être exécutée sur le serveur principal AD FS de PowerShell.
>Dans une batterie de serveurs SQL en fonction, la commande PowerShell peut être exécutée sur n’importe quel serveur AD FS qui est un membre de la batterie de serveurs.

### <a name="step-5--add-the-claim-description-to-ad-fs"></a>Étape 5 :  Ajouter la description de revendication à AD FS
1.  Ajouter la Description de revendication suivante à la batterie de serveurs. Cette Description de revendication n’est pas présente par défaut dans AD FS 2012 R2 et doit être ajouté manuellement.
2.  Dans Gestion AD FS, sous **Service**, avec le bouton droit **description de revendication** et sélectionnez **ajouter description de revendication**
3.  Entrez les informations suivantes dans la description de revendication
    - Nom complet : « Groupe d’appareils Windows » 
    - Description de revendication : 'https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsdevicegroup' »
4. Cochez la case dans les deux zones.
5. Cliquez sur **OK**.

![Description de revendication](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc6.png)

6. À l’aide de PowerShell, vous pouvez utiliser la **Add-AdfsClaimDescription** applet de commande.
``` powershell
Add-AdfsClaimDescription -Name 'Windows device group' -ClaimType 'https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsdevicegroup' `
-ShortName 'windowsdevicegroup' -IsAccepted $true -IsOffered $true -IsRequired $false -Notes 'The windows group SID of the device' 
```


>[!NOTE]
>Dans un schéma WID basé sur batterie de serveurs, la commande doit être exécutée sur le serveur principal AD FS de PowerShell.
>Dans une batterie de serveurs SQL en fonction, la commande PowerShell peut être exécutée sur n’importe quel serveur AD FS qui est un membre de la batterie de serveurs.

### <a name="step-6--enable-the-compound-authentication-bit-on-the-msds-supportedencryptiontypes-attribute"></a>Étape 6 :  Activer le bit de l’authentification composée sur l’attribut msDS-SupportedEncryptionTypes

1.  Activer l’authentification composée bits sur l’attribut msDS-SupportedEncryptionTypes du compte que vous avez désigné pour exécuter le service AD FS en utilisant le **Set-ADServiceAccount** applet de commande PowerShell.  

>[!NOTE]
>Si vous modifiez le compte de service, vous devez activer manuellement l’authentification composée en exécutant la **Set-ADUser - compoundIdentitySupported : $true** applets de commande Windows PowerShell.

``` powershell
Set-ADServiceAccount -Identity “ADFS Service Account” -CompoundIdentitySupported:$true 
```
2.  Redémarrez le Service ADFS.

>[!NOTE]
>Une fois définie à true, installation de la même gMSA sur Nouveau échoue de serveurs (2012 R2/2016) avec l’erreur suivante : 'CompoundIdentitySupported' **Install-ADServiceAccount : Impossible d’installer le compte de service. Message d'erreur : « Le contexte fourni ne correspondait pas la cible. »** .
>
>**Solution** : Définissez temporairement CompoundIdentitySupported sur $false. Cette étape entraîne par ADFS arrêter l’émission des revendications de WindowsDeviceGroup. Set-ADServiceAccount-Identity « Compte de Service AD FS » - CompoundIdentitySupported : $false d’installation du service administré de groupe sur le nouveau serveur et CompoundIdentitySupported à $True.
CompoundIdentitySupported de désactivation et réactivation puis ne nécessite pas de redémarrage du service ADFS.

### <a name="step-7-update-the-ad-fs-claims-provider-trust-for-active-directory"></a>Étape 7 : Approbation de fournisseur de revendications de mise à jour les services AD FS pour Active Directory

1. Mettre à jour l’AD FS revendications approbation de fournisseur pour Active Directory inclure la règle de revendication de « Pass-through » suivante pour la revendication de « WindowsDeviceGroup ».
2.  Dans **gestion AD FS**, cliquez sur **approbations de fournisseur de revendications** et dans le volet droit, clic droit **Active Directory** et sélectionnez **modifier les règles de revendication**.
3.  Sur le **modifier les règles de revendication pour Active Directory** cliquez sur **ajouter une règle**.
4.  Sur le **ajouter un Assistant de règle de revendication de transformation** sélectionnez **passer ou filtrer une revendication entrante** et cliquez sur **suivant**.
5.  Ajoutez un nom d’affichage et sélectionnez **groupe d’appareils Windows** à partir de la **type de revendication entrante** liste déroulante.
6.  Cliquez sur **Terminer**.  Cliquez sur **appliquer** et **Ok**. 
![Description de revendication](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc7.png)

### <a name="step-8-on-the-relying-party-where-the-windowsdevicegroup-claims-are-expected-add-a-similar-pass-through-or-transform-claim-rule"></a>Étape 8 : Sur la partie de confiance dans lequel les revendications « WindowsDeviceGroup » sont attendues, ajoutez une règle de revendication « Pass-through » ou « Transformer » similaire.
2.  Dans **gestion AD FS**, cliquez sur **confiance** et dans le volet droit, clic droit votre fournisseur de ressources, puis sélectionnez **modifier les règles de revendication**.
3.  Sur le **règles de transformation d’émission** cliquez sur **ajouter une règle**.
4.  Sur le **ajouter un Assistant de règle de revendication de transformation** sélectionnez **passer ou filtrer une revendication entrante** et cliquez sur **suivant**.
5.  Ajoutez un nom d’affichage et sélectionnez **groupe d’appareils Windows** à partir de la **type de revendication entrante** liste déroulante.
6.  Cliquez sur **Terminer**.  Cliquez sur **appliquer** et **Ok**.
![Description de revendication](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc8.png)


##<a name="steps-for-configuring-ad-fs-in-windows-server-2016"></a>Procédure de configuration AD FS dans Windows Server 2016
Ce qui suit décrit en détail les étapes de configuration de l’authentification composée sur AD FS pour Windows Server 2016.

### <a name="step-1--enable-kdc-support-for-claims-compound-authentication-and-kerberos-armoring-on-the-default-domain-controller-policy"></a>Étape 1 :  Activer la prise en charge du contrôleur de domaine Kerberos des revendications, l’authentification composée et le blindage Kerberos sur la stratégie de contrôleur de domaine par défaut
1.  Dans le Gestionnaire de serveur, sélectionnez Outils, **Group Policy Management**.
2.  Naviguez vers le bas jusqu'à la **stratégie des contrôleurs de domaine par défaut**, avec le bouton droit et sélectionnez **modifier**.
3.  Sur le **éditeur de gestion de stratégie de groupe**, sous **Configuration ordinateur**, développez **stratégies**, développez **modèles d’administration** , développez **système**, puis sélectionnez **KDC**.
4.  Dans le volet droit, double-cliquez sur **prise en charge des revendications, l’authentification composée et le blindage Kerberos**.
5.  Dans la nouvelle fenêtre de boîte de dialogue, définissez la prise en charge de contrôleur de domaine Kerberos des revendications à **activé**.
6.  Sous Options, sélectionnez **pris en charge** dans le menu déroulant puis cliquez sur **appliquer** et **OK**.


### <a name="step-2-enable-kerberos-client-support-for-claims-compound-authentication-and-kerberos-armoring-on-computers-accessing-federated-applications"></a>Étape 2 : Activer la prise en charge de client Kerberos des revendications, l’authentification composée et le blindage Kerberos sur les ordinateurs l’accès aux applications fédérées

1.  Sur une stratégie de groupe appliqués aux ordinateurs l’accès aux applications fédérées, dans le **éditeur de gestion de stratégie de groupe**, sous **Configuration ordinateur**, développez **stratégies**, développez **modèles d’administration**, développez **système**, puis sélectionnez **Kerberos**.
2.  Dans le volet droit de la fenêtre Éditeur de gestion de stratégie de groupe, double-cliquez sur **prise en charge de client Kerberos des revendications, l’authentification composée et le blindage Kerberos.**
3.  Dans la nouvelle fenêtre de boîte de dialogue, la valeur est prise en charge de Kerberos client **activé** et cliquez sur **appliquer** et **OK**.
4.  Fermez l’éditeur de gestion des stratégies de groupe.

### <a name="step-3-configure-the-primary-authentication-provider"></a>Étape 3 : Configurer le fournisseur d’authentification principale

1. La valeur est le fournisseur d’authentification principal **l’authentification Windows** pour les paramètres AD FS Intranet.
2. Dans Gestion AD FS, sous **stratégies d’authentification**, sélectionnez **l’authentification principale** et sous **paramètres globaux** cliquez sur **modifier**.
3. Sur **modifier la stratégie d’authentification globale** sous **Intranet** sélectionnez **l’authentification Windows**.
4. Cliquez sur **appliquer** et **Ok**.
5. À l’aide de PowerShell, vous pouvez utiliser la **Set-AdfsGlobalAuthenticationPolicy** applet de commande.

``` powershell
Set-AdfsGlobalAuthenticationPolicy -PrimaryIntranetAuthenticationProvider 'WindowsAuthentication'
```
>[!NOTE]
>Dans un schéma WID basé sur batterie de serveurs, la commande doit être exécutée sur le serveur principal AD FS de PowerShell.
>Dans une batterie de serveurs SQL en fonction, la commande PowerShell peut être exécutée sur n’importe quel serveur AD FS qui est un membre de la batterie de serveurs.

### <a name="step-4--enable-the-compound-authentication-bit-on-the-msds-supportedencryptiontypes-attribute"></a>Étape 4 :  Activer le bit de l’authentification composée sur l’attribut msDS-SupportedEncryptionTypes

1.  Activer l’authentification composée bits sur l’attribut msDS-SupportedEncryptionTypes du compte que vous avez désigné pour exécuter le service AD FS en utilisant le **Set-ADServiceAccount** applet de commande PowerShell.  

>[!NOTE]
>Si vous modifiez le compte de service, vous devez activer manuellement l’authentification composée en exécutant la **Set-ADUser - compoundIdentitySupported : $true** applets de commande Windows PowerShell.

``` powershell
Set-ADServiceAccount -Identity “ADFS Service Account” -CompoundIdentitySupported:$true 
```
2.  Redémarrez le Service ADFS.

>[!NOTE]
>Une fois définie à true, installation de la même gMSA sur Nouveau échoue de serveurs (2012 R2/2016) avec l’erreur suivante : 'CompoundIdentitySupported' **Install-ADServiceAccount : Impossible d’installer le compte de service. Message d'erreur : « Le contexte fourni ne correspondait pas la cible. »** .
>
>**Solution** : Définissez temporairement CompoundIdentitySupported sur $false. Cette étape entraîne par ADFS arrêter l’émission des revendications de WindowsDeviceGroup. Set-ADServiceAccount-Identity « Compte de Service AD FS » - CompoundIdentitySupported : $false d’installation du service administré de groupe sur le nouveau serveur et CompoundIdentitySupported à $True.
CompoundIdentitySupported de désactivation et réactivation puis ne nécessite pas de redémarrage du service ADFS.

### <a name="step-5-update-the-ad-fs-claims-provider-trust-for-active-directory"></a>Étape 5 : Approbation de fournisseur de revendications de mise à jour les services AD FS pour Active Directory

1. Mettre à jour l’AD FS revendications approbation de fournisseur pour Active Directory inclure la règle de revendication de « Pass-through » suivante pour la revendication de « WindowsDeviceGroup ».
2.  Dans **gestion AD FS**, cliquez sur **approbations de fournisseur de revendications** et dans le volet droit, clic droit **Active Directory** et sélectionnez **modifier les règles de revendication**.
3.  Sur le **modifier les règles de revendication pour Active Directory** cliquez sur **ajouter une règle**.
4.  Sur le **ajouter un Assistant de règle de revendication de transformation** sélectionnez **passer ou filtrer une revendication entrante** et cliquez sur **suivant**.
5.  Ajoutez un nom d’affichage et sélectionnez **groupe d’appareils Windows** à partir de la **type de revendication entrante** liste déroulante.
6.  Cliquez sur **Terminer**.  Cliquez sur **appliquer** et **Ok**. 


### <a name="step-6-on-the-relying-party-where-the-windowsdevicegroup-claims-are-expected-add-a-similar-pass-through-or-transform-claim-rule"></a>Étape 6 : Sur la partie de confiance dans lequel les revendications « WindowsDeviceGroup » sont attendues, ajoutez une règle de revendication « Pass-through » ou « Transformer » similaire.
2.  Dans **gestion AD FS**, cliquez sur **confiance** et dans le volet droit, clic droit votre fournisseur de ressources, puis sélectionnez **modifier les règles de revendication**.
3.  Sur le **règles de transformation d’émission** cliquez sur **ajouter une règle**.
4.  Sur le **ajouter un Assistant de règle de revendication de transformation** sélectionnez **passer ou filtrer une revendication entrante** et cliquez sur **suivant**.
5.  Ajoutez un nom d’affichage et sélectionnez **groupe d’appareils Windows** à partir de la **type de revendication entrante** liste déroulante.
6.  Cliquez sur **Terminer**.  Cliquez sur **appliquer** et **Ok**.

## <a name="validation"></a>Validation
Pour valider la version de revendications de 'WindowsDeviceGroup', créez un test de revendications Application prenant en charge à l’aide de .net 4.6. Avec le kit SDK WIF 4.0.
Configurer l’Application en tant que partie de confiance dans AD FS et mettre à jour avec la règle de revendication comme spécifié dans les étapes ci-dessus.
Lors de l’authentification à l’Application à l’aide du fournisseur de l’authentification intégrée de Windows d’ADFS, les revendications suivantes sont converties.
![Validation](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc9.png)

Les revendications pour l’ordinateur / l’appareil peuvent maintenant être consommées pour les contrôles d’accès plus riches.

Par exemple, ce qui suit **AdditionalAuthenticationRules** indique à AD FS pour appeler l’authentification Multifacteur si – l’utilisateur d’authentification n’est pas membre du groupe de sécurité «-1-5-21-2134745077-1211275016-3050530490-1117 » et l’ordinateur (où est le l’utilisateur est l’authentification à partir de) n’est pas membre du groupe de sécurité « S-1-5-21-2134745077-1211275016-3050530490-1115 (WindowsDeviceGroup) »

Toutefois, si une des conditions ci-dessus sont remplie, n’appellent pas de MFA.

```
'NOT EXISTS([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsdevicegroup", Value =~ "S-1-5-21-2134745077-1211275016-3050530490-1115"])
&& NOT EXISTS([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "S-1-5-21-2134745077-1211275016-3050530490-1117"])
=> issue(Type = "https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", Value = "https://schemas.microsoft.com/claims/multipleauthn");'
```
