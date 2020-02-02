---
ms.assetid: 24c4b9bb-928a-4118-acf1-5eb06c6b08e5
title: Configurer AD FS 2016 et Azure MFA
description: ''
ms.author: billmath
author: billmath
manager: mtillman
ms.date: 01/28/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: a4f9d8fa71671c4ad4651008729d4cee53c8ee2f
ms.sourcegitcommit: 74107a32efe1e53b36c938166600739a79dd0f51
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2020
ms.locfileid: "76918260"
---
# <a name="configure-azure-mfa-as-authentication-provider-with-ad-fs"></a>Configurer Azure MFA en tant que fournisseur d’authentification avec AD FS

Si votre organisation est fédérée avec Azure AD, vous pouvez utiliser Azure Multi-Factor Authentication pour sécuriser les ressources AD FS, localement et dans le Cloud. Azure MFA vous permet d’éliminer les mots de passe et de fournir une méthode plus sécurisée pour l’authentification.  À compter de Windows Server 2016, vous pouvez désormais configurer Azure MFA pour l’authentification principale ou l’utiliser en tant que fournisseur d’authentification supplémentaire. 
  
Contrairement à AD FS dans Windows Server 2012 R2, l’adaptateur Azure MFA AD FS 2016 s’intègre directement à Azure AD et ne nécessite pas de serveur Azure MFA local.   L’adaptateur Azure MFA est intégré à Windows Server 2016 et il n’est pas nécessaire d’effectuer une installation supplémentaire.


## <a name="registering-users-for-azure-mfa-with-ad-fs"></a>Inscription d’utilisateurs pour Azure MFA avec AD FS

AD FS ne prend pas en charge &#34;la vérification&#34;en ligne ou l’inscription des informations de vérification de la sécurité Azure MFA, telles que le numéro de téléphone ou l’application mobile. Cela signifie que les utilisateurs doivent être authentifiés en visitant [https://account.activedirectory.windowsazure.com/Proofup.aspx](https://account.activedirectory.windowsazure.com/Proofup.aspx) avant d’utiliser l’authentification multifacteur Azure pour s’authentifier auprès des applications AD FS. Lorsqu’un utilisateur qui n’a pas encore effectué une vérification dans Azure AD tente de s’authentifier auprès d’Azure MFA à AD FS, il obtiendra une erreur de AD FS.  En tant qu’administrateur AD FS, vous pouvez personnaliser cette expérience d’erreur pour guider l’utilisateur vers la page vérification à la place.  Pour ce faire, vous pouvez utiliser la personnalisation OnLoad. js pour détecter la chaîne de message d’erreur dans la page de AD FS et afficher un nouveau message pour guider les utilisateurs à visiter [https://aka.ms/mfasetup](https://aka.ms/mfasetup), puis tenter à nouveau l’authentification. Pour obtenir des instructions détaillées, consultez la page « Personnaliser la AD FS Web pour guider les utilisateurs pour inscrire les méthodes de vérification MFA » ci-dessous dans cet article.

>[!NOTE]
> Auparavant, les utilisateurs devaient s’authentifier auprès de MFA pour l’inscription (en visitant [https://account.activedirectory.windowsazure.com/Proofup.aspx](https://account.activedirectory.windowsazure.com/Proofup.aspx), par exemple via le raccourci [https://aka.ms/mfasetup](https://aka.ms/mfasetup)).  À présent, un utilisateur AD FS qui n’a pas encore enregistré les informations de vérification&#34;de l’authentification MFA peut accéder à Azure ad page vérification via le raccourci [https://aka.ms/mfasetup](https://aka.ms/mfasetup) utilisant uniquement l’authentification principale (telle que l’authentification intégrée de Windows ou le nom d’utilisateur et le mot de passe via les pages Web AD FS).  Si l’utilisateur n’a pas de méthode de vérification configurée, Azure AD effectue l’inscription en ligne dans laquelle l' &#34;utilisateur voit le message que votre administrateur a requis pour configurer ce compte pour&#34;une vérification de sécurité supplémentaire, et l' &#34;utilisateur peut alors choisir&#34;de le configurer maintenant.
> Les utilisateurs qui disposent déjà d’au moins une méthode de vérification MFA sont invités à fournir une authentification MFA lors de la visite de la page vérification.

## <a name="recommended-deployment-topologies"></a>Topologies de déploiement recommandées

### <a name="azure-mfa-as-primary-authentication"></a>Azure MFA en tant qu’authentification principale

Il existe quelques bonnes raisons d’utiliser Azure MFA comme authentification principale avec AD FS :

 - Pour éviter les mots de passe pour la connexion à Azure AD, Office 365 et d’autres applications AD FS
 - Pour protéger la connexion basée sur un mot de passe en exigeant un facteur supplémentaire tel que le code de vérification avant le mot de passe

Si vous souhaitez utiliser Azure MFA en tant que méthode d’authentification principale dans AD FS pour tirer parti de ces avantages, vous souhaiterez peut-être également garder la possibilité d' &#34;utiliser Azure ad&#34; accès conditionnel, y compris la véritable MFA, en demandant des facteurs supplémentaires dans AD FS.

Vous pouvez maintenant faire cela en configurant le paramètre de domaine Azure AD pour qu’il effectue une authentification &#34;MFA&#34; locale (en définissant SupportsMfa sur $true).  Dans cette configuration, AD FS pouvez être invité par Azure AD à effectuer une authentification supplémentaire ou &#34;une authentification&#34; MFA pour les scénarios d’accès conditionnel qui en ont besoin.  

Comme décrit ci-dessus, toute AD FS utilisateur qui n’a pas encore été inscrit (les informations de vérification de l’authentification MFA) doit être invité par le biais d’une page d’erreur AD FS personnalisée pour accéder [https://aka.ms/mfasetup](https://aka.ms/mfasetup) pour configurer les informations de vérification, puis retenter AD FS connexion.  
Azure MFA comme principal est considéré comme un facteur unique, après que la configuration initiale doit fournir un facteur supplémentaire pour gérer ou mettre à jour les informations de vérification dans Azure AD, ou pour accéder à d’autres ressources qui requièrent l’authentification MFA.

>[!NOTE]
> Avec ADFS 2019, vous devez apporter une modification au type de revendication d’ancrage pour le Active Directory approbation de fournisseur de revendications et modifier ce type de attributs WindowsAccountName en UPN. Exécutez l’applet de commande PowerShell fournie ci-dessous. Cela n’a aucun impact sur le fonctionnement interne de la batterie de AD FS. Vous remarquerez peut-être que quelques utilisateurs peuvent être invités à saisir des informations d’identification une fois cette modification effectuée. Une fois la connexion établie, les utilisateurs finaux ne voient aucune différence. 

```powershell
Set-AdfsClaimsProviderTrust -AnchorClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn" -TargetName "Active Directory"
```

### <a name="azure-mfa-as-additional-authentication-to-office-365"></a>Azure MFA en tant qu’authentification supplémentaire à Office 365

Avant, si vous souhaitiez avoir Azure MFA comme méthode d’authentification supplémentaire dans AD FS pour Office 365 ou d’autres parties utilisatrices, la meilleure option consistait à configurer Azure AD pour effectuer une MFA composée, dans laquelle l’authentification principale est effectuée localement dans AD FS et MFA est tr iggered par Azure AD. À présent, vous pouvez utiliser Azure MFA comme authentification supplémentaire dans AD FS lorsque le paramètre SupportsMfa du domaine est défini sur $True.  

Comme décrit ci-dessus, toute AD FS utilisateur qui n’a pas encore été inscrit (les informations de vérification de l’authentification MFA) doit être invité par le biais d’une page d’erreur AD FS personnalisée pour accéder [https://aka.ms/mfasetup](https://aka.ms/mfasetup) pour configurer les informations de vérification, puis retenter AD FS connexion.  

## <a name="pre-requisites"></a>Conditions préalables

Les conditions préalables suivantes sont requises lors de l’utilisation d’Azure MFA pour l’authentification avec AD FS :  
  
- Un [abonnement Azure avec Azure Active Directory](https://azure.microsoft.com/pricing/free-trial/).  
- [Multi-Factor Authentication Azure](https://azure.microsoft.com/documentation/articles/multi-factor-authentication/) 


> [!NOTE]
> Azure AD et Azure MFA sont inclus dans Azure AD Premium et Enterprise Mobility suite (EMS).  Si vous avez l’un de ces deux, vous n’avez pas besoin d’abonnements individuels.

- Un environnement Windows Server 2016 AD FS local.  
   - Le serveur doit être en mesure de communiquer avec les URL suivantes sur le port 443.
      - https://adnotifications.windowsazure.com
      - https://login.microsoftonline.com
- Votre environnement local est [fédéré avec Azure ad.](https://azure.microsoft.com/documentation/articles/active-directory-aadconnect-get-started-custom/#configuring-federation-with-ad-fs)  
- [Module windows Azure Active Directory pour Windows PowerShell](https://docs.microsoft.com/powershell/module/Azuread/?view=azureadps-2.0).  
- Les autorisations d’administrateur général sur votre instance de Azure AD pour la configurer à l’aide de Azure AD PowerShell.  
- Informations d’identification de l’administrateur d’entreprise pour configurer la batterie de AD FS pour Azure MFA.

## <a name="configure-the-ad-fs-servers"></a>Configurer les serveurs AD FS

Pour terminer la configuration de l’authentification multifacteur Azure pour AD FS, vous devez configurer chaque serveur de AD FS à l’aide de la procédure décrite. 

>[!NOTE]
>Assurez-vous que ces étapes sont effectuées sur **tous les** serveurs AD FS de la batterie. Si vous avez plusieurs serveurs AD FS dans votre batterie, vous pouvez effectuer la configuration nécessaire à distance à l’aide de Azure AD PowerShell.  

### <a name="step-1-generate-a-certificate-for-azure-mfa-on-each-ad-fs-server-using-the-new-adfsazuremfatenantcertificate-cmdlet"></a>Étape 1 : générer un certificat pour Azure MFA sur chaque serveur AD FS à l’aide de l’applet de commande `New-AdfsAzureMfaTenantCertificate`

La première chose à faire est de générer un certificat pour l’utilisation d’Azure MFA.  Cette opération peut être effectuée à l’aide de PowerShell.  Le certificat généré se trouve dans le magasin de certificats ordinateurs locaux et est marqué avec un nom d’objet contenant le TenantID pour votre répertoire Azure AD.

![AD FS et MFA](media/Configure-AD-FS-2016-and-Azure-MFA/ADFS_AzureMFA3.png)  

Notez que TenantID est le nom de votre répertoire dans Azure AD.  Utilisez l’applet de commande PowerShell suivante pour générer le nouveau certificat.  
    `$certbase64 = New-AdfsAzureMfaTenantCertificate -TenantID <tenantID>`  

![AD FS et MFA](media/Configure-AD-FS-2016-and-Azure-MFA/ADFS_AzureMFA1.PNG)  
  
### <a name="step-2-add-the-new-credentials-to-the-azure-multi-factor-auth-client-service-principal"></a>Étape 2 : ajouter les nouvelles informations d’identification au principal du service Azure Multi-Factor auth client

Afin d’autoriser les serveurs AD FS à communiquer avec le client Azure Multi-Factor auth, vous devez ajouter les informations d’identification au principal du service pour le client Azure Multi-Factor auth. Les certificats générés à l’aide de l’applet de commande `New-AdfsAzureMFaTenantCertificate` serviront de ces informations d’identification. Effectuez les opérations suivantes à l’aide de PowerShell pour ajouter les nouvelles informations d’identification au principal du service Azure Multi-Factor auth.  

> [!NOTE]
> Pour effectuer cette étape, vous devez vous connecter à votre instance de Azure AD avec PowerShell à l’aide de `Connect-MsolService`.  Ces étapes supposent que vous êtes déjà connecté via PowerShell.  Pour plus d’informations [, consultez`Connect-MsolService`.](https://msdn.microsoft.com/library/dn194123.aspx)  

**Définir le certificat comme étant les nouvelles informations d’identification sur le client Azure Multi-Factor auth**  

`New-MsolServicePrincipalCredential -AppPrincipalId 981f26a1-7f43-403b-a875-f8b09b8cd720 -Type asymmetric -Usage verify -Value $certBase64`

> [!IMPORTANT]
> Cette commande doit être exécutée sur tous les serveurs AD FS de votre batterie de serveurs.  Azure AD MFA échoue sur les serveurs sur lesquels le certificat n’est pas défini comme les nouvelles informations d’identification sur le client Azure Multi-Factor auth.

> [!NOTE]  
> 981f26a1-7f43-403b-A875-f8b09b8cd720 est le GUID du client Azure Multi-Factor auth.  
  
## <a name="configure-the-ad-fs-farm"></a>Configurer la batterie de serveurs AD FS  
  
Une fois que vous avez terminé la section précédente sur chaque serveur de AD FS, définissez les informations du locataire Azure à l’aide de l’applet de commande [Set-AdfsAzureMfaTenant](https://docs.microsoft.com/powershell/module/adfs/export-adfsauthenticationproviderconfigurationdata) . Cette applet de commande ne doit être exécutée qu’une seule fois pour une batterie de AD FS.

Ouvrez une invite PowerShell et entrez votre propre *tenantId* avec l’applet de commande [Set-AdfsAzureMfaTenant](https://docs.microsoft.com/powershell/module/adfs/export-adfsauthenticationproviderconfigurationdata) . Pour les clients qui utilisent Microsoft Azure Government Cloud, ajoutez le paramètre `-Environment USGov` :

> [!NOTE]
> Pour que ces modifications prennent effet, vous devez redémarrer le service AD FS sur chaque serveur de la batterie de serveurs. Pour un impact minimal, prenez chaque serveur AD FS de la rotation de l’équilibrage de la charge réseau un à la fois et attendez que toutes les connexions soient drainées.

```powershell
Set-AdfsAzureMfaTenant -TenantId <tenant ID> -ClientId 981f26a1-7f43-403b-a875-f8b09b8cd720
```

![AD FS et MFA](media/Configure-AD-FS-2016-and-Azure-MFA/ADFS_AzureMFA5.png)

Windows Server sans la dernière Service Pack ne prend pas en charge le paramètre `-Environment` pour l’applet de commande [Set-AdfsAzureMfaTenant](https://docs.microsoft.com/powershell/module/adfs/export-adfsauthenticationproviderconfigurationdata) . Si vous utilisez Azure Government Cloud et que les étapes précédentes n’ont pas pu configurer votre locataire Azure en raison du paramètre de `-Environment` manquant, procédez comme suit pour créer manuellement les entrées de registre. Ignorez ces étapes si l’applet de commande précédente a correctement inscrit les informations de votre locataire ou si vous n’êtes pas dans le Cloud Azure Government :

1. Ouvrez l' **éditeur du Registre** sur le serveur de AD FS.
1. Accédez à `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\ADFS`. Créez les valeurs de clé de Registre suivantes :

    | Clé de Registre       | Value |
    |--------------------|-----------------------------------|
    | URL sasurl             | https://adnotifications.windowsazure.us/StrongAuthenticationService.svc/Connector |
    | StsUrl             | https://login.microsoftonline.us |
    | URI        | https://adnotifications.windowsazure.us/StrongAuthenticationService.svc/Connector |

1. Redémarrez le service AD FS sur chaque serveur de la batterie pour que ces modifications prennent effet. Pour un impact minimal, prenez chaque serveur AD FS de la rotation de l’équilibrage de la charge réseau un à la fois et attendez que toutes les connexions soient drainées.

Après cela, vous verrez qu’Azure MFA est disponible en tant que méthode d’authentification principale pour l’utilisation de l’intranet et de l’extranet.

![AD FS et MFA](media/Configure-AD-FS-2016-and-Azure-MFA/ADFS_AzureMFA6.png)  

## <a name="renew-and-manage-ad-fs-azure-mfa-certificates"></a>Renouveler et gérer des AD FS les certificats Azure MFA

Les instructions suivantes vous guident tout au long de la gestion des certificats Azure MFA sur vos serveurs de AD FS.
Par défaut, lorsque vous configurez AD FS avec Azure MFA, les certificats générés par le biais de l’applet de commande `New-AdfsAzureMfaTenantCertificate` PowerShell sont valides pendant 2 ans.  Pour déterminer la durée d’expiration de vos certificats, puis renouveler et installer de nouveaux certificats, utilisez la procédure suivante.

### <a name="assess-ad-fs-azure-mfa-certificate-expiration-date"></a>Évaluer AD FS date d’expiration du certificat Azure MFA

Sur chaque serveur AD FS, dans le magasin de l’ordinateur local, il y aura un certificat auto- &#34;signé avec UO = Microsoft AD FS&#34; Azure MFA dans l’émetteur et le sujet.  Il s’agit du certificat Azure MFA.  Vérifiez la période de validité de ce certificat sur chaque serveur de AD FS pour déterminer la date d’expiration.  

### <a name="create-new-ad-fs-azure-mfa-certificate-on-each-ad-fs-server"></a>Créer un nouveau AD FS certificat Azure MFA sur chaque serveur de AD FS

Si la période de validité de vos certificats est proche de sa fin, démarrez le processus de renouvellement en générant un nouveau certificat Azure MFA sur chaque serveur de AD FS. Dans une fenêtre de commande PowerShell, générez un nouveau certificat sur chaque serveur AD FS à l’aide de l’applet de commande suivante :

```
PS C:\> $newcert = New-AdfsAzureMfaTenantCertificate -TenantId <tenant id such as contoso.onmicrosoft.com> -Renew $true
```

À la suite de cette applet de commande, un nouveau certificat valide à partir de 2 jours dans le futur à 2 jours + 2 ans sera généré.  Les opérations AD FS et Azure MFA ne seront pas affectées par cette applet de commande ou le nouveau certificat. (Remarque : le délai de 2 jours est intentionnel et fournit le temps nécessaire pour exécuter les étapes ci-dessous pour configurer le nouveau certificat dans le locataire avant que AD FS ne commence à l’utiliser pour Azure MFA.)

### <a name="configure-each-new-ad-fs-azure-mfa-certificate-in-the-azure-ad-tenant"></a>Configurez chaque nouveau AD FS certificat Azure MFA dans le locataire Azure AD

À l’aide du module Azure AD PowerShell, pour chaque nouveau certificat (sur chaque serveur AD FS), mettez à jour les paramètres de votre client Azure AD comme suit (Remarque : vous devez d’abord vous connecter au locataire à l’aide de `Connect-MsolService` pour exécuter les commandes suivantes).

```
PS C:/> New-MsolServicePrincipalCredential -AppPrincipalId 981f26a1-7f43-403b-a875-f8b09b8cd720 -Type Asymmetric -Usage Verify -Value $newcert
```

`$certbase64` est le nouveau certificat.  Le certificat encodé en base64 peut être obtenu en exportant le certificat (sans la clé privée) sous la forme d’un fichier encodé DER et en l’ouvrant dans Notepad. exe, puis en le copiant dans la session PowerShell et en l’affectant à la variable `$certbase64`.

### <a name="verify-that-the-new-certificates-will-be-used-for-azure-mfa"></a>Vérifier que le ou les nouveaux certificats seront utilisés pour l’authentification multifacteur Azure

Une fois que le nouveau certificat est valide, AD FS le choisit et commence à utiliser chaque certificat respectif pour Azure MFA dans un délai de quelques heures à un jour.  Une fois cela se produit, sur chaque serveur, vous verrez un événement consigné dans le journal des événements d’administration de AD FS avec les informations suivantes :

```
Log Name:      AD FS/Admin
Source:        AD FS
Date:          2/27/2018 7:33:31 PM
Event ID:      547
Task Category: None
Level:         Information
Keywords:      AD FS
User:          DOMAIN\adfssvc
Computer:      ADFS.domain.contoso.com
Description:
The tenant certificate for Azure MFA has been renewed.  

TenantId: contoso.onmicrosoft.com.
Old thumbprint: 7CC103D60967318A11D8C51C289EF85214D9FC63.
Old expiration date: 9/15/2019 9:43:17 PM.
New thumbprint: 8110D7415744C9D4D5A4A6309499F7B48B5F3CCF.
New expiration date: 2/27/2020 2:16:07 AM.
```

## <a name="customize-the-ad-fs-web-page-to-guide-users-to-register-mfa-verification-methods"></a>Personnaliser la page Web AD FS pour guider les utilisateurs afin qu’ils inscrivent les méthodes de vérification de l’authentification MFA

Utilisez les exemples suivants pour personnaliser votre AD FS pages Web pour les utilisateurs qui n’ont pas encore effectué la vérification (informations de vérification de l’authentification MFA configurées).

### <a name="find-the-error"></a>Rechercher l’erreur

Tout d’abord, plusieurs messages d’erreur AD FS retournent dans le cas où l’utilisateur ne dispose pas d’informations de vérification.
Si vous utilisez Azure MFA comme authentification principale, l’utilisateur non-vérifié verra une page d’erreur AD FS contenant les messages suivants :
```
    <div id="errorArea">
        <div id="openingMessage" class="groupMargin bigText">
            An error occurred
        </div>
        <div id="errorMessage" class="groupMargin">
            Authentication attempt failed. Select a different sign in option or close the web browser and sign in again. Contact your administrator for more information.
        </div>
```
Lorsque Azure AD une tentative d’authentification supplémentaire est effectuée, l’utilisateur non vérifié verra une page d’erreur AD FS contenant les messages suivants :
```
<div id='mfaGreetingDescription' class='groupMargin'>For security reasons, we require additional information to verify your account (mahesh@jenfield.net)</div>
    <div id="errorArea">
        <div id="openingMessage" class="groupMargin bigText">
            An error occurred
        </div>
        <div id="errorMessage" class="groupMargin">
            The selected authentication method is not available for &#39;username@contoso.com&#39;. Choose another authentication method or contact your system administrator for details.
        </div>
```

### <a name="catch-the-error-and-update-the-page-text"></a>Interceptez l’erreur et mettez à jour le texte de la page

Pour intercepter l’erreur et montrer l’aide personnalisée de l’utilisateur, ajoutez simplement le JavaScript à la fin du fichier OnLoad. js qui fait partie du thème Web AD FS.  Cela vous permet d’effectuer les opérations suivantes :
 - Rechercher la ou les chaînes d’erreur d’identification
 - fournissez un contenu Web personnalisé.  

> [!NOTE]
> Pour obtenir des instructions générales sur la personnalisation du fichier OnLoad. js, consultez l’article [personnalisation avancée des pages de connexion AD FS](advanced-customization-of-ad-fs-sign-in-pages.md).

Voici un exemple simple, que vous pouvez étendre :

1. Ouvrez Windows PowerShell sur votre serveur AD FS principal et créez un nouveau thème Web AD FS en exécutant la commande suivante :
    
    ``` PowerShell
        New-AdfsWebTheme –Name ProofUp –SourceName default
    ``` 
2. Ensuite, créez le dossier et exportez le thème Web par défaut AD FS :

    ``` PowerShell
       New-Item -Path 'c:\Theme' -ItemType Directory;Export-AdfsWebTheme –Name default –DirectoryPath c:\Theme
    ```
3. Ouvrir le fichier C:\Theme\script\onload.js dans un éditeur de texte
4. Ajoutez le code suivant à la fin du fichier OnLoad. js
    
    ``` JavaScript
    //Custom Code
    //Customize MFA exception
    //Begin

    var domain_hint = "<YOUR_DOMAIN_NAME_HERE>";
    var mfaSecondFactorErr = "The selected authentication method is not available for";
    var mfaProofupMessage = "You will be automatically redirected in 5 seconds to set up your account for additional security verification. Once you have completed the setup, please return to the application you are attempting to access.<br><br>If you are not redirected automatically, please click <a href='{0}'>here</a>."
    var authArea = document.getElementById("authArea");
    if (authArea) {
        var errorMessage = document.getElementById("errorMessage");
        if (errorMessage) {
            if (errorMessage.innerHTML.indexOf(mfaSecondFactorErr) >= 0) {

                //Hide the error message
                var openingMessage = document.getElementById("openingMessage");
                if (openingMessage) {
                    openingMessage.style.display = 'none'
                }
                var errorDetailsLink = document.getElementById("errorDetailsLink");
                if (errorDetailsLink) {
                    errorDetailsLink.style.display = 'none'
                }

                //Provide a message and redirect to Azure AD MFA Registration Url
                var mfaRegisterUrl = "https://account.activedirectory.windowsazure.com/proofup.aspx?proofup=1&whr=" + domain_hint;
                errorMessage.innerHTML = "<br>" + mfaProofupMessage.replace("{0}", mfaRegisterUrl);
                window.setTimeout(function () { window.location.href = mfaRegisterUrl; }, 5000);
            }
        }
    }

    //End Customize MFA Exception
    //End Custom Code
    ```
    > [!IMPORTANT]
    > Vous devez changer « < YOUR_DOMAIN_NAME_HERE > ». pour utiliser votre nom de domaine. Par exemple : `var domain_hint = "contoso.com";`
    
5. Enregistrer le fichier OnLoad. js
6. Importez le fichier OnLoad. js dans votre thème personnalisé en tapant la commande Windows PowerShell suivante :
    
    ``` PowerShell
    Set-AdfsWebTheme -TargetName ProofUp -AdditionalFileResource @{Uri='/adfs/portal/script/onload.js';path="c:\theme\script\onload.js"}
    ```
7. Enfin, appliquez le thème Web AD FS personnalisé en tapant la commande Windows PowerShell suivante :
    
    ``` PowerShell
    Set-AdfsWebConfig -ActiveThemeName "ProofUp"
    ```

## <a name="next-steps"></a>Étapes suivantes

[Gérer les protocoles TLS/SSL et les suites de chiffrement utilisés par AD FS et Azure MFA](manage-ssl-protocols-in-ad-fs.md)
