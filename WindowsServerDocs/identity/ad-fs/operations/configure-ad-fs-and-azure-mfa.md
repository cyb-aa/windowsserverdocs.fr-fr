---
ms.assetid: 24c4b9bb-928a-4118-acf1-5eb06c6b08e5
title: Configurer ADFS2016 et Azure MFA
description: ''
ms.author: billmath
author: billmath
manager: mtillman
ms.date: 01/28/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 33b782ded2ae1bdd8b00c08b81e4e0ee7f885899
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66188826"
---
# <a name="configure-azure-mfa-as-authentication-provider-with-ad-fs"></a>Configurer Azure MFA en tant que fournisseur d’authentification avec AD FS

Si votre organisation est fédérée avec Azure AD, vous pouvez utiliser Azure multi-Factor Authentication pour sécuriser les ressources AD FS, à la fois en local et dans le cloud. Azure MFA vous permet d’éliminer les mots de passe et fournissent un moyen plus sûr d’authentifier.  À compter de Windows Server 2016, vous pouvez maintenant configurer Azure MFA pour l’authentification principale ou utilisez-le comme fournisseur d’authentification supplémentaires. 
  
Contrairement avec AD FS dans Windows Server 2012 R2, l’adaptateur Azure MFA de 2016 AD FS s’intègre directement à Azure AD et ne nécessite pas un serveur Azure MFA local.   L’adaptateur Azure MFA est intégrée à Windows Server 2016, et il n’est pas nécessaire pour une installation supplémentaire.


## <a name="registering-users-for-azure-mfa-with-ad-fs"></a>L’inscription des utilisateurs pour Azure MFA avec AD FS

AD FS ne prend pas en charge inline &#34;preuve des&#34;, ou l’enregistrement des informations de vérification de sécurité Azure MFA comme numéro de téléphone ou application mobile. Cela signifie que les utilisateurs doivent obtenir à l’épreuve d’inscrire en vous rendant sur [ https://account.activedirectory.windowsazure.com/Proofup.aspx ](https://account.activedirectory.windowsazure.com/Proofup.aspx) avant d’utiliser Azure MFA pour s’authentifier auprès des applications AD FS. Lorsqu’un utilisateur qui le n'a pas encore l’épreuve d’inscrire dans Azure AD tente de s’authentifier avec Azure MFA à AD FS, ils obtiendra une erreur AD FS.  En tant qu’un administrateur AD FS, vous pouvez personnaliser cette expérience d’erreur pour guider l’utilisateur à la page de vérification à la place.  Vous pouvez le faire à l’aide de la personnalisation de onload.js pour détecter la chaîne de message d’erreur dans la page AD FS et afficher un nouveau message pour guider les utilisateurs à visiter [ https://aka.ms/mfasetup ](https://aka.ms/mfasetup), puis essayez à nouveau de l’authentification. Pour des instructions détaillées, voir la « AD FS web page Personnaliser pour guider les utilisateurs pour inscrire les méthodes de vérification MFA » ci-dessous dans cet article.

>[!NOTE]
> Auparavant, les utilisateurs devaient s’authentifier avec MFA pour l’inscription (visite [ https://account.activedirectory.windowsazure.com/Proofup.aspx ](https://account.activedirectory.windowsazure.com/Proofup.aspx), par exemple via le raccourci [ https://aka.ms/mfasetup ](https://aka.ms/mfasetup)).  À présent, un utilisateur AD FS qui n’a pas encore inscrit les informations de vérification MFA permettre accéder à Azure AD&#34;page de vérification s via le raccourci [ https://aka.ms/mfasetup ](https://aka.ms/mfasetup) à l’aide de l’authentification principale uniquement (telles que l’authentification intégrée Windows ou nom d’utilisateur et mot de passe via les services AD FS des pages web).  Si l’utilisateur ne dispose d’aucune méthode de vérification configurée, Azure AD effectuera l’inscription en ligne dans laquelle l’utilisateur voit le message &#34;votre administrateur exige que ce compte pour la vérification de sécurité supplémentaire&#34;, et l’utilisateur peut ensuite Sélectionnez cette option pour &#34;configurer maintenant&#34;.
> Les utilisateurs qui disposent déjà d’au moins une méthode de vérification MFA configurée seront toujours être invités à fournir l’authentification Multifacteur lors de la visite de la page de vérification.

## <a name="recommended-deployment-topologies"></a>Topologies de déploiement recommandées

### <a name="azure-mfa-as-primary-authentication"></a>Azure MFA comme authentification principale

Il existe quelques bonnes raisons d’utiliser Azure MFA comme authentification principale avec AD FS :

 - Pour éviter les mots de passe pour la connexion à Azure AD, Office 365 et autres applications AD FS
 - Pour protéger le mot de passe basée sur l’authentification en exigeant un facteur supplémentaire tel que le code de vérification avant le mot de passe

Si vous souhaitez utiliser Azure MFA comme une méthode d’authentification principale dans AD FS pour bénéficier de ces avantages, vous souhaiterez probablement aussi conserver la possibilité d’utiliser, notamment de l’accès conditionnel Azure AD &#34;true MFA&#34; en invitant d’autres facteurs dans AD FS.

Maintenant faire cela en configurant le paramètre de domaine Azure AD effectue une authentification Multifacteur en local (paramètre &#34;SupportsMfa&#34; sur $True).  Dans cette configuration, AD FS peut être invité par Azure AD pour effectuer une authentification supplémentaire ou &#34;true MFA&#34; pour les scénarios d’accès conditionnel qui en ont besoin.  

Comme décrit ci-dessus, tout utilisateur d’AD FS qui n’a pas encore inscrit (informations de vérification configurées MFA) doit être invité via une page d’erreur AD FS personnalisée à visiter [ https://aka.ms/mfasetup ](https://aka.ms/mfasetup) pour configurer les informations de vérification, puis Essayez à nouveau compte de connexion AD FS.  
Étant donné que Azure MFA en tant que principal est considéré comme un facteur unique, une fois que les utilisateurs de la configuration initiale doivent fournir un facteur supplémentaire pour gérer ou mettre à jour leurs informations de vérification dans Azure AD, ou pour accéder aux autres ressources qui nécessitent l’authentification Multifacteur.

>[!NOTE]
> Avec ADFS 2019, vous devez effectuer une modification pour le type de revendication d’ancrage pour l’approbation de fournisseur de revendications Active Directory et modifiez-la à partir de la windowsaccountname et le nom UPN. Exécutez l’applet de commande PowerShell ci-dessous. Cela n’a aucun impact sur le fonctionnement interne de la batterie de serveurs AD FS. Vous pouvez remarquer que quelques utilisateurs peuvent être ré-demandées par invite pour les informations d’identification une fois cette modification est apportée. Après vous être connecté à nouveau, les utilisateurs finaux ne voient aucune différence. 

```powershell
Set-AdfsClaimsProviderTrust -AnchorClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn" -TargetName "Active Directory"
```

### <a name="azure-mfa-as-additional-authentication-to-office-365"></a>Azure MFA comme authentification supplémentaire à Office 365

Auparavant, si vous voulez obtenir Azure MFA comme méthode d’authentification supplémentaire dans AD FS pour Office 365 ou autres parties de confiance, la meilleure option était de configurer Azure AD pour composée MFA, dans lequel l’authentification principale est effectuée en local dans AD FS et l’authentification Multifacteur est tr iggered par Azure AD. Maintenant, vous pouvez utiliser Azure MFA comme authentification supplémentaire dans AD FS lorsque le domaine SupportsMfa est défini sur $True.  

Comme décrit ci-dessus, tout utilisateur d’AD FS qui n’a pas encore inscrit (informations de vérification configurées MFA) doit être invité via une page d’erreur AD FS personnalisée à visiter [ https://aka.ms/mfasetup ](https://aka.ms/mfasetup) pour configurer les informations de vérification, puis Essayez à nouveau compte de connexion AD FS.  

## <a name="pre-requisites"></a>Conditions préalables

Les conditions préalables suivantes sont requises lors de l’utilisation d’Azure MFA pour l’authentification avec AD FS :  
  
- Un [abonnement Azure avec Azure Active Directory](https://azure.microsoft.com/pricing/free-trial/).  
- [Azure multi-Factor Authentication](https://azure.microsoft.com/documentation/articles/multi-factor-authentication/)  
- Le proxy d’application Web est en mesure de communiquer avec les éléments suivants sur les ports 80 et 443 :

    - https://adnotifications.windowsazure.com
    - https://login.microsoftonline.com


> [!NOTE]
> Azure AD et Azure MFA sont incluses dans Azure AD Premium et Enterprise Mobility Suite (EMS).  Si vous avez un d'entre eux, il est inutile des abonnements individuels.
- Un environnement local de Windows Server 2016 AD FS.  
   - Le serveur doit être en mesure de communiquer avec les URL suivantes sur les ports 80 et 443.
      - https://adnotifications.windowsazure.com
      - https://login.microsoftonline.com
- Votre environnement local est [fédéré avec Azure AD.](https://azure.microsoft.com/documentation/articles/active-directory-aadconnect-get-started-custom/#configuring-federation-with-ad-fs)  
- [Module Azure Active Directory de Windows pour Windows PowerShell](https://docs.microsoft.com/powershell/module/Azuread/?view=azureadps-2.0).  
- Autorisations d’administrateur global sur votre instance d’Azure AD à configurer à l’aide d’Azure AD PowerShell.  
- Administrateur d’entreprise pour configurer la batterie de serveurs AD FS pour Azure MFA.  
  
## <a name="configure-the-ad-fs-servers"></a>Configurer les serveurs AD FS

Afin de terminer la configuration pour Azure MFA pour AD FS, vous devez configurer chaque serveur AD FS à l’aide de la procédure décrite. 

>[!NOTE]
>Assurez-vous que ces étapes sont exécutées sur **tous les** serveurs AD FS dans la batterie de serveurs. Si vous avez plusieurs serveurs AD FS dans votre batterie de serveurs, vous pouvez effectuer la configuration nécessaire à distance à l’aide d’Azure AD PowerShell.  

### <a name="step-1-generate-a-certificate-for-azure-mfa-on-each-ad-fs-server-using-the-new-adfsazuremfatenantcertificate-cmdlet"></a>Étape 1 : Générer un certificat pour Azure MFA sur chaque serveur AD FS à l’aide du `New-AdfsAzureMfaTenantCertificate` applet de commande

La première chose à faire est de générer un certificat pour l’authentification Multifacteur Azure à utiliser.  Cela est possible à l’aide de PowerShell.  Vous trouverez le certificat généré dans le magasin de certificats des ordinateurs locaux, et elle est marquée avec un nom d’objet qui contient l’ID de locataire pour votre annuaire Azure AD.

![AD FS et l’authentification Multifacteur](media/Configure-AD-FS-2016-and-Azure-MFA/ADFS_AzureMFA3.png)  

Notez que TenantID est le nom de votre annuaire dans Azure AD.  Utilisez l’applet de commande PowerShell suivante pour générer le nouveau certificat.  
    `$certbase64 = New-AdfsAzureMfaTenantCertificate -TenantID <tenantID>`  

![AD FS et l’authentification Multifacteur](media/Configure-AD-FS-2016-and-Azure-MFA/ADFS_AzureMFA1.PNG)  
  
### <a name="step-2-add-the-new-credentials-to-the-azure-multi-factor-auth-client-service-principal"></a>Étape 2 : Ajouter les nouvelles informations d’identification au Azure multi-Factor Auth Client Principal du Service

Pour activer les serveurs AD FS communiquer avec Azure multi-Factor Auth Client, vous devez ajouter les informations d’identification au Principal du Service pour Azure multi-Factor Auth Client. Les certificats générés à l’aide de la `New-AdfsAzureMFaTenantCertificate` applet de commande servira de ces informations d’identification. Procédez comme suit pour ajouter les nouvelles informations d’identification au Azure multi-Factor Auth Client Principal du Service à l’aide de PowerShell.  

> [!NOTE]
> Pour effectuer cette étape, vous devez vous connecter à votre instance d’Azure AD avec PowerShell à l’aide `Connect-MsolService`.  Ces étapes supposent que vous avez déjà connectés via PowerShell.  Pour plus d’informations, consultez [ `Connect-MsolService`.](https://msdn.microsoft.com/library/dn194123.aspx)  

**Définir le certificat en tant que les nouvelles informations d’identification par rapport à Azure multi-Factor Auth Client**  

`New-MsolServicePrincipalCredential -AppPrincipalId 981f26a1-7f43-403b-a875-f8b09b8cd720 -Type asymmetric -Usage verify -Value $certBase64`

> [!IMPORTANT]
> Cette commande doit être exécutée sur tous les serveurs AD FS dans votre batterie de serveurs.  Azure AD MFA échoue sur les serveurs que vous n’avez pas le certificat défini en tant que les nouvelles informations d’identification par rapport à Azure multi-Factor Auth Client.

> [!NOTE]  
> 981f26a1-7f43-403b-a875-f8b09b8cd720 est le GUID pour Azure multi-Factor Auth Client.  
  
## <a name="configure-the-ad-fs-farm"></a>Configurer la batterie de serveurs AD FS  
  
Une fois que vous avez terminé la section précédente sur chaque serveur AD FS, vous devez exécuter le `Set-AdfsAzureMfaTenant` applet de commande.  
  
Cette applet de commande doit être exécutée qu’une seule fois pour une batterie de serveurs AD FS.  Utiliser PowerShell pour effectuer cette étape.
  
> [!NOTE]  
> Vous devez redémarrer le service AD FS sur chaque serveur dans la batterie de serveurs avant que ces modifications prennent effet.  
  
    Set-AdfsAzureMfaTenant -TenantId <tenant ID> -ClientId 981f26a1-7f43-403b-a875-f8b09b8cd720  

![AD FS et l’authentification Multifacteur](media/Configure-AD-FS-2016-and-Azure-MFA/ADFS_AzureMFA5.png)  
  
Après cela, vous verrez que Azure MFA est disponible comme une méthode d’authentification principale pour l’intranet et extranet utilisation.    
  
![AD FS et l’authentification Multifacteur](media/Configure-AD-FS-2016-and-Azure-MFA/ADFS_AzureMFA6.png)  

## <a name="renew-and-manage-ad-fs-azure-mfa-certificates"></a>Renouveler et gérer AD FS Azure MFA certificats

Les conseils suivants vous accompagne tout au long de la gestion des certificats Azure MFA sur vos serveurs AD FS.
Par défaut, lorsque vous configurez des services AD FS avec Azure MFA, les certificats générés par le biais de la `New-AdfsAzureMfaTenantCertificate` applet de commande PowerShell sont valides pendant 2 ans.  Pour déterminer comment fermer pour expiration vos certificats ne sont et ensuite pour renouveler et installer de nouveaux certificats, procédez comme suit.

### <a name="assess-ad-fs-azure-mfa-certificate-expiration-date"></a>Évaluer la date d’expiration de certificat AD FS Azure MFA

Sur chaque serveur AD FS, dans l’ordinateur local mon magasin, il y aura un certificat auto-signé avec &#34;UO = Microsoft AD FS Azure MFA&#34; dans l’émetteur et le sujet.  Il s’agit du certificat Azure MFA.  Vérifier la période de validité de ce certificat sur chaque serveur AD FS pour déterminer la date d’expiration.  

### <a name="create-new-ad-fs-azure-mfa-certificate-on-each-ad-fs-server"></a>Créer un nouveau certificat de MFA Azure AD FS sur chaque serveur AD FS

Si la période de validité des certificats est proche de sa fin, démarrez le processus de renouvellement en générant un nouveau certificat Azure MFA sur chaque serveur AD FS. Dans une fenêtre de commande PowerShell, générer un nouveau certificat sur chaque serveur AD FS à l’aide de l’applet de commande suivante :

```
PS C:\> $newcert = New-AdfsAzureMfaTenantCertificate -TenantId <tenant id such as contoso.onmicrosoft.com> -Renew $true
```

À la suite de cette applet de commande, un nouveau certificat valide à partir de 2 jours à l’avenir à 2 jours + 2 ans sera généré.  Opérations AD FS et Azure MFA ne seront pas affectées par cette applet de commande ou le nouveau certificat. (Remarque : le délai de 2 jours est intentionnel et fournit des temps d’exécuter les étapes ci-dessous pour configurer le nouveau certificat dans le client avant le démarrage de AD FS à l’utiliser pour l’authentification Multifacteur Azure.)

### <a name="configure-each-new-ad-fs-azure-mfa-certificate-in-the-azure-ad-tenant"></a>Configurer chaque nouveau certificat AD FS Azure MFA dans le locataire Azure AD

À l’aide du module Azure AD PowerShell, pour chaque nouveau certificat (sur chaque serveur AD FS), mettre à jour les paramètres de votre locataire Azure AD comme suit (Remarque : vous devez vous connecter au locataire à l’aide de `Connect-MsolService` pour exécuter les commandes suivantes).

```
PS C:/> New-MsolServicePrincipalCredential -AppPrincipalId 981f26a1-7f43-403b-a875-f8b09b8cd720 -Type Asymmetric -Usage Verify -Value $newcert
```

`$certbase64` est le nouveau certificat.  Le certificat codé en base64 peut être obtenu, exportez le certificat (sans la clé privée) en tant que DER codé fichier et l’ouverture dans Notepad.exe, puis copie/collage dans la session PowerShell et affectez à la variable `$certbase64`.

### <a name="verify-that-the-new-certificates-will-be-used-for-azure-mfa"></a>Vérifiez que les nouveaux certificats seront utilisés pour l’authentification Multifacteur Azure

Une fois les nouveaux certificats sont valides, AD FS les ramassage et de démarrer à l’aide de chaque certificat respectif pour Azure MFA dans quelques heures par jour.  Une fois que cela se produit, sur chaque serveur vous verrez un événement enregistré dans le journal des événements AD FS administrateur avec les informations suivantes :

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

## <a name="customize-the-ad-fs-web-page-to-guide-users-to-register-mfa-verification-methods"></a>Personnaliser la page web AD FS pour guider les utilisateurs pour inscrire les méthodes de vérification MFA

Utilisez les exemples suivants pour personnaliser vos pages web de AD FS pour les utilisateurs qui n’ont pas encore épreuve haut (configuré les informations de vérification MFA).

### <a name="find-the-error"></a>Rechercher l’erreur

Tout d’abord, il existe deux différents messages d’erreur QU'AD FS renvoie dans le cas dans lequel l’utilisateur ne dispose pas des informations de vérification.
Si vous utilisez Azure MFA comme authentification principale, l’utilisateur non vérifié verra une page d’erreur AD FS contenant les messages suivants :
```
    <div id="errorArea">
        <div id="openingMessage" class="groupMargin bigText">
            An error occurred
        </div>
        <div id="errorMessage" class="groupMargin">
            Authentication attempt failed. Select a different sign in option or close the web browser and sign in again. Contact your administrator for more information.
        </div>
```
Quand Azure AD comme authentification supplémentaire est tentée, l’utilisateur non vérifié s’affiche une page d’erreur AD FS contenant les messages suivants :
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

### <a name="catch-the-error-and-update-the-page-text"></a>Intercepter l’erreur et de mettre à jour le texte de la page

Pour intercepter l’erreur et l’utilisateur apparaîtra conseils personnalisés ajouter simplement le code javascript à la fin du fichier onload.js qui fait partie du thème web AD FS.  Ainsi, procédez comme suit :
 - Recherchez l’ou les chaînes erreur identification
 - fournir du contenu web personnalisé.  

> [!NOTE]
> Pour obtenir des conseils en général sur la façon de personnaliser le fichier onload.js, consultez l’article [Advanced Customization of AD FS Sign-in Pages](advanced-customization-of-ad-fs-sign-in-pages.md).

Voici un exemple simple, il pouvez que vous souhaitez étendre :

1. Ouvrez Windows PowerShell sur votre serveur AD FS principal et créer un thème Web AD FS en exécutant la commande suivante :
    
    ``` PowerShell
        New-AdfsWebTheme –Name ProofUp –SourceName default
    ``` 
2. Ensuite, créez le dossier et exporter le thème de Web AD FS par défaut :

    ``` PowerShell
       New-Item -Path 'c:\Theme' -ItemType Directory;Export-AdfsWebTheme –Name default –DirectoryPath c:\Theme
    ```
3. Ouvrez le fichier C:\Theme\script\onload.js dans un éditeur de texte
4. Ajoutez le code suivant à la fin du fichier onload.js
    
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
    > Vous devez modifier « < YOUR_DOMAIN_NAME_HERE > » ; Pour utiliser votre nom de domaine. Par exemple : `var domain_hint = "contoso.com";`
    
5. Enregistrez le fichier onload.js
6. Importer le fichier onload.js dans votre thème personnalisé en tapant la commande Windows PowerShell suivante :
    
    ``` PowerShell
    Set-AdfsWebTheme -TargetName ProofUp -AdditionalFileResource @{Uri=’/adfs/portal/script/onload.js’;path="c:\theme\script\onload.js"}
    ```
7. Enfin, appliquez le thème de Web AD FS personnalisé en tapant la commande Windows PowerShell suivante :
    
    ``` PowerShell
    Set-AdfsWebConfig -ActiveThemeName "ProofUp"
    ```

## <a name="next-steps"></a>Étapes suivantes

[Gérer les protocoles TLS/SSL et Suites de chiffrement utilisé par AD FS et Azure MFA](manage-ssl-protocols-in-ad-fs.md)
