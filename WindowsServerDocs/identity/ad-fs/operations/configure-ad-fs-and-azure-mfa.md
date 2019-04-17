---
ms.assetid: 24c4b9bb-928a-4118-acf1-5eb06c6b08e5
title: Configurer ADFS2016 et Azure MFA
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 11/01/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: be8e88ae36f344f1265fb76e66c19e0ac8aeb533
ms.sourcegitcommit: ffdfbb76c7f775e20d089d3f662d4f9a7d642f1e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/05/2018
---
# <a name="configure-ad-fs-2016-and-azure-mfa"></a>Configurer ADFS2016 et Azure MFA

>S’applique à: Windows Server2016

Si votre organisation est fédérée avec Azure AD, vous pouvez utiliser Azure multi-Factor Authentication pour sécuriser les ressources ADFS, à la fois local et dans le cloud. Azure MFA vous permet d’éliminer les mots de passe et fournissent un moyen plus fiable pour s’authentifier.  À partir de Windows Server2016, vous pouvez maintenant configurer Azure MFA pour l’authentification principale. 
  
À la différence avec ADFS dans Windows Server2012R2, l’adaptateur de l’authentification Multifacteur Azure ADFS2016 s’intègre directement avec Azure AD et ne nécessite pas un serveur local Azure MFA.   La carte d’Azure MFA est intégrée à Windows Server2016, et il n’est pas nécessaire pour une installation supplémentaire.

## <a name="registering-users-for-azure-mfa-with-ad-fs-2016"></a>Inscription des utilisateurs pour l’authentification Multifacteur Azure avec ADFS2016
ADFS ne prend pas en charge inline "preuve de", ou l’enregistrement des informations sur la vérification de sécurité Azure MFA comme numéro de téléphone ou l’application mobile. Cela signifie que les utilisateurs doivent obtenir l’épreuve en visitant https://account.activedirectory.windowsazure.com/Proofup.aspx avant d’utiliser l’authentification Multifacteur Azure s’authentifier auprès d’applications ADFS. Lorsqu’un utilisateur qui n’a pas encore épreuve dans Azure ActiveDirectory tente de s’authentifier auprès d’Azure MFA à ADFS, ils obtiendront une erreur ADFS.  En tant qu’un administrateur ADFS, vous pouvez personnaliser cette expérience d’erreur pour inviter l’utilisateur à depuis la page à la place.  Vous pouvez le faire à l’aide de la personnalisation onload.js pour détecter la chaîne de message d’erreur dans la page ADFS et afficher un nouveau message pour guider les utilisateurs pour visiter https://aka.ms/mfasetup, puis essayez à nouveau d’authentification. Pour obtenir des instructions détaillées, voir le "les services ADFS web page Personnaliser pour guider les utilisateurs pour inscrire les méthodes de vérification de l’authentification Multifacteur" ci-dessous dans cet article.

>[!NOTE]
> Auparavant, les utilisateurs devaient s’authentifier avec l’authentification Multifacteur pour l’inscription (visitant https://account.activedirectory.windowsazure.com/Proofup.aspx, par exemple via le raccourci aka.ms/mfasetup).  Désormais, un utilisateur ADFS qui n’a pas encore inscrit les informations de vérification de l’authentification Multifacteur permettre accéder à Azure AD de depuis la page via le raccourci aka.ms/mfasetup à l’aide de l’authentification principale uniquement (comme les pages web à l’authentification intégrée de Windows ou nom d’utilisateur et mot de passe via les services ADFS) .  Si l’utilisateur ne dispose d’aucune méthode de vérification configurée, Azure AD effectuer l’inscription en ligne dans laquelle l’utilisateur voit le message "votre administrateur a requis que vous configurez ce compte pour la vérification de sécurité supplémentaires", et l’utilisateur permet de sélectionner "Configurer maintenant".
> Les utilisateurs qui ont déjà au moins une méthode de vérification de l’authentification Multifacteur configurée seront toujours invités à fournir l’authentification Multifacteur lors de la visite depuis la page.

### <a name="recommended-deployment-topologies"></a>Topologies de déploiement recommandées

#### <a name="azure-mfa-as-primary-authentication"></a>Authentification Multifacteur Azure en tant que l’authentification principale
Il existe quelques bonnes raisons d’utiliser l’authentification Multifacteur Azure comme authentification principale avec ADFS:
 - Pour éviter les mots de passe pour la connexion à Azure AD, Office365 et autres applications d’ADFS
 - Pour protéger le mot de passe en fonction de connexion en exigeant un facteur supplémentaire comme code de vérification avant le mot de passe

Si vous souhaitez utiliser l’authentification Multifacteur Azure comme une méthode d’authentification principale dans ADFS pour bénéficier de ces avantages, vous souhaiterez probablement aussi conserver la possibilité d’utiliser Azure AD conditionnel accès notamment "true l’authentification Multifacteur" en invitant à d’autres facteurs dans ADFS.

Vous pouvez maintenant ce faire en configurant le paramètre de domaine Azure AD pour effectuer l’authentification Multifacteur sur local (le paramètre "SupportsMfa" sur $True).  Dans cette configuration, ADFS peut être invité par Azure AD pour effectuer une authentification supplémentaire ou "l’authentification Multifacteur true" pour les scénarios d’accès conditionnel qui le requièrent.  

Comme décrit ci-dessus, tout utilisateur ADFS qui n’a pas encore inscrit (l’authentification Multifacteur vérification informations configurées) doivent être invitées via une page d’erreur ADFS personnalisée à visiter aka.ms/mfasetup pour configurer les informations de vérification, puis essayez à nouveau d’ouverture de session ADFS.  
Étant donné que l’authentification Multifacteur Azure en tant que principal est considéré comme un seul facteur, une fois la configuration initiale, les utilisateurs devront fournir un facteur supplémentaire pour gérer ou mettre à jour leurs informations de vérification dans Azure AD, ou d’accéder aux autres ressources qui nécessitent l’authentification Multifacteur.


#### <a name="azure-mfa-as-additional-authentication-to-office-365"></a>Azure MFA comme authentification supplémentaire à Office 365
Auparavant, si vous souhaitiez ont Azure MFA comme méthode d’authentification supplémentaire dans ADFS pour Office365 ou autres parties de confiance, la meilleure option était de configurer Azure AD pour composée l’authentification Multifacteur, dans lequel l’authentification principale est effectuée sur localement dans les services ADFS et l’authentification Multifacteur est tr iggered par Azure AD. Maintenant, vous pouvez utiliser l’authentification Multifacteur Azure comme authentification supplémentaire dans ADFS lorsque le paramètre SupportsMfa du domaine est défini sur $True.  

Comme décrit ci-dessus, tout utilisateur ADFS qui n’a pas encore inscrit (l’authentification Multifacteur vérification informations configurées) doivent être invitées via une page d’erreur ADFS personnalisée à visiter aka.ms/mfasetup pour configurer les informations de vérification, puis essayez à nouveau d’ouverture de session ADFS.  


## <a name="pre-requisites"></a>Conditions préalables  
Les conditions préalables suivantes sont requises lors de l’utilisation d’Azure MFA pour l’authentification avec ADFS:  
  
- Un [abonnement Azure avec Azure ActiveDirectory](https://azure.microsoft.com/pricing/free-trial/).  
- [Authentification multifacteur Azure](https://azure.microsoft.com/documentation/articles/multi-factor-authentication/)  

>[!NOTE]   
> Azure AD et l’authentification Multifacteur Azure sont inclus dans Azure AD Premium et Enterprise Mobility Suite (EMS).  Si vous avez un de ces inutile des abonnements individuels.   
- Un environnement sur site ADFS de Windows Server2016.  
- Votre environnement local est [fédérés avec Azure AD.](https://azure.microsoft.com/documentation/articles/active-directory-aadconnect-get-started-custom/#configuring-federation-with-ad-fs)  
- [WindowsAzureActiveDirectoryModule pour Windows PowerShell](https://go.microsoft.com/fwlink/p/?linkid=236297).  
- Autorisations d’administrateur global sur votre instance d’Azure AD pour configurer à l’aide d’Azure ActiveDirectory PowerShell.  
- Administrateur d’entreprise pour configurer la batterie ADFS pour l’authentification Multifacteur Azure.  
  
  
## <a name="configure-the-ad-fs-servers"></a>Configuration des serveurs ADFS  
Afin de terminer la configuration de l’authentification Multifacteur Azure pour ADFS, vous devez configurer chaque serveur ADFS en suivant les étapes décrites. 

>[!NOTE]
>Assurez-vous que ces étapes doivent être effectuées sur **tous les** serveurs ADFS dans la batterie de serveurs. Si vous disposez de plusieurs serveurs ADFS dans votre batterie de serveurs, vous pouvez effectuer la configuration nécessaire à distance à l’aide d’Azure ActiveDirectory Powershell.  
  
  
  
### <a name="step-1-generate-a-certificate-for-azure-mfa-on-each-ad-fs-server-using-the-new-adfsazuremfatenantcertificate-cmdlet"></a>Étape1: Générer un certificat pour l’authentification Multifacteur Azure sur chaque serveur ADFS à l’aide du `New-AdfsAzureMfaTenantCertificate`applet de commande.   
La première chose que vous devez faire est de générer un certificat pour Azure MFA à utiliser.  Cela est possible à l’aide de PowerShell.  Vous trouverez le certificat généré dans le magasin de certificats ordinateur local, et il est marqué avec un nom d’objet contenant le TenantID pour votre annuaire Azure AD.    
  
![ADFS et l’authentification Multifacteur](media/Configure-AD-FS-2016-and-Azure-MFA/ADFS_AzureMFA3.png)  
  
Notez que TenantID est le nom de votre répertoire dans Azure AD.  Utilisez l’applet de commande PowerShell suivante pour générer le nouveau certificat.  
    `$certbase64 = New-AdfsAzureMfaTenantCertificate -TenantID <tenantID>`  
      
![ADFS et l’authentification Multifacteur](media/Configure-AD-FS-2016-and-Azure-MFA/ADFS_AzureMFA1.PNG)  
  
### <a name="step-2-add-the-new-credentials-to-azure-multi-factor-auth-client-spn"></a>Étape2: Ajouter les nouvelles informations d’identification pour Azure multi-Factor Auth Client SPN   
Afin d’activer les serveurs ADFS communiquer avec le Client d’authentification multifacteur Azure, vous devez ajouter les informations d’identification pour le SPN pour le Client d’authentification multifacteur Azure. Les certificats générés à l’aide de la `New-AdfsAzureMFaTenantCertificate`applet de commande servira de ces informations d’identification. Procédez comme suit pour ajouter les nouvelles informations d’identification pour le SPN de Client Azure multi-Factor Authentication à l’aide de PowerShell.  

>[!NOTE]   
>Afin d’effectuer cette étape, vous devez vous connecter à votre instance d’Azure AD avec PowerShell à l’aide de Connect-MsolService.  Ces étapes supposent que vous avez déjà connectés via PowerShell.  Pour plus d’informations, voir [Connect MsolService.](https://msdn.microsoft.com/library/dn194123.aspx)  
     
  
2. **Définir le certificat en tant que les nouvelles informations d’identification par rapport à Azure multi-Factor Auth Client**  
    
    `New-MsolServicePrincipalCredential -AppPrincipalId 981f26a1-7f43-403b-a875-f8b09b8cd720 -Type asymmetric -Usage verify -Value $certBase64 `

>[!IMPORTANT]
>  Cette commande doit être exécutée sur tous les serveurs de votre batterie ADFS.  Azure MFA AD échoue sur les serveurs qui ne disposez pas le certificat en tant que les nouvelles informations d’identification par rapport à Azure multi-Factor Auth Client. 

>[!NOTE]  
> 981f26a1-7f43-403b-a875-f8b09b8cd720 est le guid pour Azure multi-Factor Auth Client.  
  
## <a name="configure-the-ad-fs-farm"></a>Configurer la batterie de serveurs ADFS  
  
Une fois que vous avez terminé la section précédente sur chaque serveur ADFS, vous devez exécuter le `Set-AdfsAzureMfaTenant`applet de commande.  
  
Cette applet de commande doit être exécutée une seule fois pour une batterie ADFS.  Utilisez PowerShell pour effectuer cette étape.    
  
>[!NOTE]  
>Vous devez redémarrer le service ADFS sur chaque serveur dans la batterie de serveurs que ces modifications prennent effet.  
  
    Set-AdfsAzureMfaTenant -TenantId <tenant ID> -ClientId 981f26a1-7f43-403b-a875-f8b09b8cd720  
      
![ADFS et l’authentification Multifacteur](media/Configure-AD-FS-2016-and-Azure-MFA/ADFS_AzureMFA5.png)  
  
Après cela, vous verrez que l’authentification Multifacteur Azure est disponible sous la forme d’une méthode d’authentification principale pour l’intranet et d’utilisation extranet.    
  
![ADFS et l’authentification Multifacteur](media/Configure-AD-FS-2016-and-Azure-MFA/ADFS_AzureMFA6.png)  

## <a name="renew-and-manage-ad-fs-azure-mfa-certificates"></a>Renouveler et gérer ADFS Azure MFA certificats
Le guide suivant présente comment gérer les certificats d’authentification Multifacteur Azure sur vos serveurs ADFS.
Par défaut, lorsque vous configurez des services ADFS avec une authentification Multifacteur Azure, les certificats générés via l’applet de commande PowerShell New-AdfsAzureMfaTenantCertificate sont valides pendant 2ans.  Pour déterminer comment fermer pour expiration vos certificats se trouvent et puis pour renouveler et installer de nouveaux certificats, utilisez la procédure suivante.

### <a name="assess-ad-fs-azure-mfa-certificate-expiration-date"></a>Évaluer la date d’expiration de certificat ADFS Azure MFA
Sur chaque serveur ADFS, dans l’ordinateur local mon magasin, il y aura un certificat auto-signé avec "unité d’organisation = MicrosoftADFS Azure MFA" dans l’émetteur et l’objet.  Il s’agit du certificat de l’authentification Multifacteur Azure.  Vérifiez la période de validité de ce certificat sur chaque serveur ADFS pour déterminer la date d’expiration.  

### <a name="create-new-ad-fs-azure-mfa-certificate-on-each-ad-fs-server"></a>Créer un nouveau certificat de l’authentification Multifacteur Azure ADFS sur chaque serveur ADFS
Si la période de validité des certificats est proche de sa fin, démarrez le processus de renouvellement en générant un nouveau certificat d’authentification Multifacteur Azure sur chaque serveur ADFS. Dans une fenêtre de commande powershell, générer un nouveau certificat sur chaque serveur ADFS à l’aide de l’applet de commande suivante:

```
PS C:\> $newcert = New-AdfsAzureMfaTenantCertificate -TenantId <tenant id such as contoso.onmicrosoft.com> -Renew $true
```

À la suite de cette applet de commande, un nouveau certificat valide à partir de 2jours à l’avenir à 2jours + 2ans sera généré.  Opérations ADFS et Azure MFA ne seront pas affectées par cette applet de commande ou le nouveau certificat. (Remarque: le délai de 2jours est intentionnel et offre de temps pour exécuter les étapes ci-dessous pour configurer le nouveau certificat dans le client avant le démarrage d’ADFS à l’utiliser pour Azure MFA.)


### <a name="configure-each-new-ad-fs-azure-mfa-certificate-in-the-azure-ad-tenant"></a>Configurez chaque nouveau certificat ADFS Azure MFA dans le client Azure AD
L’aide du module Azure AD PowerShell, chaque nouveau certificat (sur chaque serveur ADFS), de mettre à jour vos paramètres de client Azure AD comme suit (Remarque: vous devez d’abord connecter pour le client à l’aide de Connect-MsolService pour exécuter les commandes suivantes).

```
PS C:/> New-MsolServicePrincipalCredential -AppPrincipalId 981f26a1-7f43-403b-a875-f8b09b8cd720 -Type Asymmetric -Usage Verify -Value $certbase64
```
    Where $certbase64 is the new certificate.  The base64 encoded certificate can be obtained by exporting the certificate (without the private key) as a DER encoded file and opening in Notepad.exe, then copy/pasting to the PSH session and assigning to the variable $certbase64

### <a name="verify-that-the-new-certificates-will-be-used-for-azure-mfa"></a>Vérifiez que les nouveaux certificats seront utilisés pour l’authentification Multifacteur Azure
Une fois les nouveaux certificats deviennent valides, ADFS les mains et démarrer à l’aide de chaque certificat respectif pour Azure MFA dans quelques heures par jour.  Une fois que cela se produit, sur chaque serveur vous verrez un événement enregistré dans le journal des événements ADFS Admin avec les informations suivantes: nom du journal: ADFS/Admin Source: ADFS Date: 2/27/2018 ID d’événement 7 h 33:31: catégorie de tâche 547: aucun niveau: mots clés d’informations: ADFS utilisateur: ordinateur DOMAIN\adfssvc: ADFS.domain.contoso.com Description: le certificat client pour l’authentification Multifacteur Azure a été renouvelé.  

TenantId: contoso.onmicrosoft.com. Ancienne empreinte numérique: 7CC103D60967318A11D8C51C289EF85214D9FC63. Ancienne date d’expiration: 15/9/2019 9 h 43:17. Nouvelle empreinte numérique: 8110D7415744C9D4D5A4A6309499F7B48B5F3CCF. Nouvelle date d’expiration: 2/27/2020 2 h 16:07.

## <a name="customize-the-ad-fs-web-page-to-guide-users-to-register-mfa-verification-methods"></a>Personnaliser la page web ADFS pour guider les utilisateurs pour inscrire les méthodes de vérification de l’authentification Multifacteur

Utilisez les exemples suivants pour personnaliser vos pages web d’ADFS pour les utilisateurs qui n’ont pas encore épreuve des (configuré les informations de vérification de l’authentification Multifacteur).

### <a name="find-the-error"></a>Recherchez l’erreur
Tout d’abord, il existe deux différents messages d’erreur QU'ADFS retourne dans le cas dans lesquels l’utilisateur ne dispose pas des informations de vérification.
Si vous utilisez l’authentification Multifacteur Azure en tant que l’authentification principale, l’utilisateur non vérifié voit une page d’erreur ADFS contenant les messages suivants:
```
    <div id="errorArea"> 
        <div id="openingMessage" class="groupMargin bigText">
            An error occurred 
        </div> 
        <div id="errorMessage" class="groupMargin">
            Authentication attempt failed. Select a different sign in option or close the web browser and sign in again. Contact your administrator for more information. 
        </div>
```
Lors de la tentative de Azure AD comme authentification supplémentaire, l’utilisateur non vérifié voit une page d’erreur ADFS contenant les messages suivants:
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

### <a name="catch-the-error-and-update-the-page-text"></a>Intercepter l’erreur et le texte de la page de mise à jour
Intercepter l’erreur et d’afficher à l’utilisateur personnalisé des conseils convient à ajouter javascript à la fin du fichier onload.js qui fait partie de l’ADFS web thème pour (1) Rechercher les chaînes d’erreur identification et (2) fournir personnalisée contenu Web.  (Pour obtenir des conseils en général sur la façon de personnaliser le fichier onload.js, consultez l’article [ici](https://docs.microsoft.com/en-us/windows-server/identity/ad-fs/operations/advanced-customization-of-ad-fs-sign-in-pages).)

