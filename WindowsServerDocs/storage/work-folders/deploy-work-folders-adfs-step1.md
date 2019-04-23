---
title: 'Déployer Dossiers de travail avec AD FS et le proxy d’application web : Étape 1, Configurer AD FS'
ms.prod: windows-server-threshold
ms.technology: storage-work-folders
ms.topic: article
manager: klaasl
ms.author: jeffpatt
author: JeffPatt24
ms.date: 10/18/2018
ms.assetid: 938cdda2-f17e-4964-9218-f5868fd96735
ms.openlocfilehash: a26b784c18049ee473a191abc7bfa0a5d253d15e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59883030"
---
# <a name="deploy-work-folders-with-ad-fs-and-web-application-proxy-step-1-set-up-ad-fs"></a>Déployer des dossiers de travail avec AD FS et Proxy d’Application Web : Étape 1, la configuration AD FS

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique décrit la première étape du déploiement de Dossiers de travail avec les services de fédération Active Directory (AD FS) et le proxy d’application Web. Vous pouvez trouver les autres étapes de ce processus dans ces rubriques :  
  
-   [Déployer des dossiers de travail avec AD FS et Proxy d’Application Web : Vue d’ensemble](deploy-work-folders-adfs-overview.md)  
  
-   [Déployer des dossiers de travail avec AD FS et Proxy d’Application Web : Étape 2, le travail de post-configuration AD FS](deploy-work-folders-adfs-step2.md)  
  
-   [Déployer des dossiers de travail avec AD FS et Proxy d’Application Web : Étape 3, configurer des dossiers de travail](deploy-work-folders-adfs-step3.md)  
  
-   [Déployer des dossiers de travail avec AD FS et Proxy d’Application Web : Étape 4 : configurer le Proxy d’Application Web](deploy-work-folders-adfs-step4.md)  
  
-   [Déployer des dossiers de travail avec AD FS et Proxy d’Application Web : L’étape 5, configurez les Clients](deploy-work-folders-adfs-step5.md)  
  
> [!NOTE]
>   Les instructions présentées dans cette section ont été conçues pour un environnement Windows Server 2016. Si vous utilisez Windows Server 2012 R2, suivez les [instructions pour Windows Server 2012 R2](https://technet.microsoft.com/library/dn747208(v=ws.11).aspx).

Pour configurer AD FS pour l’utiliser avec Dossiers de travail, utilisez les procédures suivantes.  
  
## <a name="pre-installment-work"></a>Tâche de préinstallation  
Si vous envisagez de convertir l’environnement de test que vous configurez avec ces instructions pour le mettre en production, vous pouvez effectuer deux opérations avant de commencer :  
  
-   Configurer un compte d’administrateur de domaine Active Directory à utiliser pour exécuter le service AD FS.
  
-   Obtenir un certificat SAN (Subject Alternative Name) SSL (Secure Sockets Layer) pour l’authentification du serveur. Dans l’exemple de test, vous utiliserez un certificat auto-signé, mais pour la production, vous devez utiliser un certificat approuvé publiquement.  
  
L’obtention de ces éléments peut prendre un certain temps, en fonction des stratégies de votre société. Il peut donc s’avérer utile de lancer le processus de demande de ces éléments avant de commencer à créer l’environnement de test.  
  
Il existe de nombreuses autorités de certification commerciale (CA) auprès desquelles vous pouvez acheter le certificat. Vous trouverez une liste des autorités de certification approuvées par Microsoft dans [l’article 931125 de la Base de connaissances](https://support.microsoft.com/kb/931125). Une autre solution consiste à obtenir un certificat auprès de l’autorité de certification de votre société.  
  
Pour l’environnement de test, vous utiliserez un certificat auto-signé qui est créé par l’un des scripts fournis.  
  
> [!NOTE]  
> AD FS ne prend pas en charge les certificats de chiffrement de nouvelle génération (CNG), ce qui signifie que vous ne pouvez pas créer le certificat auto-signé à l’aide de l’applet de commande Windows PowerShell New-SelfSignedCertificate. Vous pouvez cependant utiliser le script makecert.ps1 inclus dans le billet de blog [Deploying Work Folders with AD FS and Web Application Proxy](https://blogs.technet.microsoft.com/filecab/2014/03/03/deploying-work-folders-with-ad-fs-and-web-application-proxy-wap). Ce script crée un certificat auto\-signé qui fonctionne avec AD FS et vous invite à fournir les noms SAN qui seront nécessaires pour créer le certificat.  
  
Ensuite, exécutez les tâches de préinstallation supplémentaires décrites dans les sections suivantes.  
  
### <a name="create-an-ad-fs-self-signed-certificate"></a>Créer un certificat auto\-signé AD FS  
Pour créer un certificat auto-signé AD FS, procédez comme suit :  
  
1.  Téléchargez les scripts fournis dans le billet de blog [Deploying Work Folders with AD FS and Web Application Proxy](https://blogs.technet.microsoft.com/filecab/2014/03/03/deploying-work-folders-with-ad-fs-and-web-application-proxy-wap), puis copiez le fichier makecert.ps1 sur la machine AD FS.  
  
2.  Ouvrez une fenêtre Windows PowerShell avec des privilèges d’administrateur.  
  
3.  Définissez la stratégie d’exécution sur unrestricted :  
  
    ```powershell  
    Set-ExecutionPolicy –ExecutionPolicy Unrestricted   
    ```  
  
4.  Passez au répertoire où vous avez copié le script.  
  
5.  Exécutez le script makecert :  
  
    ```powershell  
    .\makecert.ps1  
    ```  
  
6.  Lorsque vous êtes invité à modifier l’objet du certificat, entrez la nouvelle valeur pour l’objet. Dans cet exemple, la valeur est **blueadfs.contoso.com**.  
  
7.  Lorsque vous êtes invité à entrer les noms SAN, appuyez sur Y, puis entrez les noms SAN, un par un.  
  
    Dans cet exemple, tapez **blueadfs.contoso.com** et appuyez sur Entrée, puis tapez **2016-adfs.contoso.com** et appuyez sur Entrée, puis tapez **enterpriseregistration.contoso.com** et appuyez sur Entrée.  
  
    Lorsque tous les noms SAN ont été entrés, appuyez sur Entrée dans une ligne vide.  
  
8.  Lorsque vous êtes invité à installer les certificats dans le magasin d’autorités de certification racines de confiance, appuyez sur Y.  
  
Le certificat AD FS doit être un certificat SAN avec les valeurs suivantes :  

-   Service AD FS nom.domaine

-   enterpriseregistration.domain

-   nom du serveur AD FS.domaine

Dans l’exemple de test, les valeurs sont :  
  
-   **blueadfs.contoso.com**
  
-   **enterpriseregistration.contoso.com**
  
-   **2016-adfs.contoso.com**
  
Le SAN enterpriseregistration est nécessaire pour Workplace Join.  
  
### <a name="set-the-server-ip-address"></a>Configurer l’adresse IP du serveur  
Changez l’adresse IP de votre serveur en adresse IP statique. Pour l’exemple de test, utilisez la classe IP A, qui est 192.168.0.160 / masque de sous-réseau : 255.255.0.0 / passerelle par défaut : 192.168.0.1 / préféré DNS : 192.168.0.150 (l’adresse IP de votre contrôleur de domaine\).  
  
## <a name="install-the-ad-fs-role-service"></a>Installer le service de rôle AD FS  
Pour installer les services AD FS, procédez comme suit :  
  
1.  Ouvrez une session sur la machine physique ou virtuelle sur laquelle vous envisagez d’installer les services AD FS, ouvrez le **Gestionnaire de serveur** et démarrez l’Assistant Ajout de rôles et de fonctionnalités.  
  
2.  Dans la page **Rôles de serveur**, sélectionnez le rôle **Services de fédération Active Directory (AD FS)**, puis cliquez sur **Suivant**.  
  
3.  Dans la page **Services de fédération Active Directory (AD FS)**, vous verrez un message indiquant que le rôle Proxy d’application Web ne peut pas être installé sur le même ordinateur que les services AD FS. Cliquez sur **Suivant**.  
  
4.  Cliquez sur **Installer** dans la page de confirmation.  
  
Pour réaliser la même installation d’AD FS via Windows PowerShell, utilisez les commandes suivantes :  
  
```powershell  
Add-WindowsFeature RSAT-AD-Tools  
Add-WindowsFeature ADFS-Federation –IncludeManagementTools  
```  
  
## <a name="configure-ad-fs"></a>Configurer AD FS  
Configurez ensuite les services AD FS en utilisant le Gestionnaire de serveur ou Windows PowerShell.  
  
### <a name="configure-ad-fs-by-using-server-manager"></a>Configurer AD FS à l’aide du Gestionnaire de serveur  
Pour configurer AD FS à l’aide du Gestionnaire de serveur, procédez comme suit :  
  
1.  Ouvrez le Gestionnaire de serveur.  
  
2.  Cliquez sur l’indicateur **Notifications** en haut de la fenêtre Gestionnaire de serveur, puis cliquez sur **Configurez le service FS (Federation Service) sur ce serveur**.  
  
3.  L’Assistant Configuration des services AD FS (Active Directory Federation Services) s’ouvre. Dans la page **Connexion à AD DS**, entrez le compte d’administrateur de domaine que vous souhaitez utiliser en tant que compte AD FS, puis cliquez sur **Suivant**.  
  
4.  Dans la page **Spécifier les propriétés de service**, entrez le nom de l’objet du certificat SSL (Secure Sockets Layer) à utiliser pour la communication AD FS. Dans l’exemple de test, il s’agit de **blueadfs.contoso.com**.  
  
5.  Entrez le nom du Service de fédération. Dans l’exemple de test, il s’agit de **blueadfs.contoso.com**. Cliquez sur **Suivant**.  
  
    > [!NOTE]  
    > Le nom du Service de fédération ne doit pas utiliser le nom d’un serveur existant dans l’environnement. Si vous utilisez le nom d’un serveur existant, l’installation d’AD FS échoue et vous devrez la relancer.  
  
6.  Dans la page **Spécifier un compte de service**, entrez le nom que vous souhaitez utiliser pour le compte de service géré. Dans l’exemple de test, sélectionnez **Créer un compte de service géré de groupe** et dans **Nom du compte**, entrez **ADFSService**. Cliquez sur **Suivant**.  
  
7.  Dans la page **Spécifier une base de données de configuration**, sélectionnez **Créez une base de données sur ce serveur à l’aide de la base de données interne Windows** et cliquez sur **Suivant**.  
  
8.  La page **Examiner les options** vous donne une vue d’ensemble des options que vous avez sélectionnées. Cliquez sur **Suivant**.  
  
9. La page **Vérifications des conditions préalables** indique si toutes les vérifications de la configuration requise ont donné satisfaction. S’il n’existe aucun problème, cliquez sur **Configurer**.  
  
    > [!NOTE]  
    > Si vous avez utilisé le nom du serveur AD FS ou de n’importe quel autre ordinateur existant pour le nom du service de fédération, un message d’erreur s’affiche. Vous devez recommencer l’installation et choisir un nom autre que celui d’un ordinateur existant.  
  
10. Une fois la configuration terminée, la page **Résultats** confirme que les services AD FS ont été correctement configurés.  
  
### <a name="configure-ad-fs-by-using-powershell"></a>Configurer AD FS à l’aide de PowerShell  
Pour effectuer la même configuration d’AD FS via Windows PowerShell, utilisez les commandes suivantes.  
  
Pour installer les services AD FS :  
  
```powershell  
Add-WindowsFeature RSAT-AD-Tools  
Add-WindowsFeature ADFS-Federation -IncludeManagementTools   
```  
  
Pour créer le compte de service géré :  
  
```powershell  
New-ADServiceAccount "ADFSService"-Server 2016-DC.contoso.com -Path "CN=Managed Service Accounts,DC=Contoso,DC=COM" -DNSHostName 2016-ADFS.contoso.com -ServicePrincipalNames HTTP/2016-ADFS,HTTP/2016-ADFS.contoso.com  
```  
  
Après avoir configuré les services AD FS, vous devez configurer une batterie AD FS en utilisant le compte de service géré que vous avez créé à l’étape précédente et le certificat que vous avez créé au cours des étapes préalables à la configuration.  
  
Pour configurer une batterie AD FS :  
  
```powershell  
$cert = Get-ChildItem CERT:\LocalMachine\My |where {$_.Subject -match blueadfs.contoso.com} | sort $_.NotAfter -Descending | select -first 1    
$thumbprint = $cert.Thumbprint  
Install-ADFSFarm -CertificateThumbprint $thumbprint -FederationServiceDisplayName "Contoso Corporation" –FederationServiceName blueadfs.contoso.com -GroupServiceAccountIdentifier contoso\ADFSService$ -OverwriteConfiguration -ErrorAction Stop  
```  
  
Étape suivante : [Déployer des dossiers de travail avec AD FS et Proxy d’Application Web : Étape 2, le travail de post-configuration AD FS](deploy-work-folders-adfs-step2.md)  
  
## <a name="see-also"></a>Voir aussi  
[Vue d’ensemble des dossiers de travail](Work-Folders-Overview.md)  
  

