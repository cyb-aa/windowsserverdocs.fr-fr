---
title: "Déployer Dossiers de travail avec ADFS et le proxy d’application web: Étape1, Configurer ADFS"
ms.prod: windows-server-threshold
ms.technology: storage-work-folders
ms.topic: article
manager: klaasl
ms.author: jeffpatt
author: JeffPatt24
ms.date: 4/5/2017
ms.assetid: 938cdda2-f17e-4964-9218-f5868fd96735
ms.openlocfilehash: 4a8a044ad6a8ec5275f5be4b949a2ab58d16da61
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/17/2017
---
# <a name="deploy-work-folders-with-ad-fs-and-web-application-proxy-step-1-set-up-ad-fs"></a>Déployer Dossiers de travail avec ADFS et le proxy d’application Web: étape1, configurer ADFS

>S’applique à: WindowsServer (canal semi-annuel), WindowsServer2016

Cette rubrique décrit la première étape du déploiement de Dossiers de travail avec les services de fédération ActiveDirectory (ADFS) et le proxy d’application Web. Les autres étapes de ce processus sont décrites dans les rubriques suivantes:  
  
-   [Déployer Dossiers de travail avec ADFS et le proxy d’application Web: Vue d’ensemble](deploy-work-folders-adfs-overview.md)  
  
-   [Déployer Dossiers de travail avec ADFS et le proxy d’application Web: Étape2, Tâches post-configuration ADFS](deploy-work-folders-adfs-step2.md)  
  
-   [Déployer Dossiers de travail avec ADFS et le proxy d’application Web: étape3, configurer les dossiers de travail](deploy-work-folders-adfs-step3.md)  
  
-   [Déployer Dossiers de travail avec ADFS et le proxy d’application Web: étape4, configurer le proxy d’application Web](deploy-work-folders-adfs-step4.md)  
  
-   [Déployer Dossiers de travail avec ADFS et le proxy d’application Web: étape5, configurer les clients](deploy-work-folders-adfs-step5.md)  
  
> [!NOTE]
>   Les instructions présentées dans cette section ont été conçues pour un environnement WindowsServer2016. Si vous utilisez Windows Server2012R2, suivez les [instructions pour Windows Server2012R2](https://technet.microsoft.com/library/dn747208(v=ws.11).aspx).

Pour configurer ADFS pour l’utiliser avec Dossiers de travail, utilisez les procédures suivantes.  
  
## <a name="pre-installment-work"></a>Tâche de préinstallation  
Si vous envisagez de convertir l’environnement de test que vous configurez avec ces instructions pour le mettre en production, vous pouvez effectuer deux opérations avant de commencer:  
  
-   Configurer un compte d’administrateur de domaine ActiveDirectory à utiliser pour exécuter le service ADFS.
  
-   Obtenir un certificat SAN (Subject Alternative Name) SSL (Secure Sockets Layer) pour l’authentification du serveur. Dans l’exemple de test, vous utiliserez un certificat auto-signé, mais pour la production, vous devez utiliser un certificat approuvé publiquement.  
  
L’obtention de ces éléments peut prendre un certain temps, en fonction des stratégies de votre société. Il peut donc s’avérer utile de lancer le processus de demande de ces éléments avant de commencer à créer l’environnement de test.  
  
Il existe de nombreuses autorités de certification commerciale (CA) auprès desquelles vous pouvez acheter le certificat. Vous trouverez une liste des autorités de certification approuvées par Microsoft dans [l’article 931125 de la Base de connaissances](http://support.microsoft.com/kb/931125). Une autre solution consiste à obtenir un certificat auprès de l’autorité de certification de votre société.  
  
Pour l’environnement de test, vous utiliserez un certificat auto-signé qui est créé par l’un des scripts fournis.  
  
> [!NOTE]  
> ADFS ne prend pas en charge les certificats de chiffrement de nouvelle génération (CNG), ce qui signifie que vous ne pouvez pas créer le certificat auto-signé à l’aide de l’applet de commande WindowsPowerShell New-SelfSignedCertificate. Vous pouvez cependant utiliser le script makecert.ps1 inclus dans le billet de blog [Deploying Work Folders with AD FS and Web Application Proxy](https://blogs.technet.microsoft.com/filecab/2014/03/03/deploying-work-folders-with-ad-fs-and-web-application-proxy-wap). Ce script crée un certificat auto\-signé qui fonctionne avec ADFS et vous invite à fournir les noms SAN qui seront nécessaires pour créer le certificat.  
  
Ensuite, exécutez les tâches de préinstallation supplémentaires décrites dans les sections suivantes.  
  
### <a name="create-an-ad-fs-self-signed-certificate"></a>Créer un certificat auto\-signé ADFS  
Pour créer un certificat auto-signé ADFS, procédez comme suit:  
  
1.  Téléchargez les scripts fournis dans le billet de blog [Deploying Work Folders with AD FS and Web Application Proxy](https://blogs.technet.microsoft.com/filecab/2014/03/03/deploying-work-folders-with-ad-fs-and-web-application-proxy-wap), puis copiez le fichier makecert.ps1 sur la machine ADFS.  
  
2.  Ouvrez une fenêtre Windows PowerShell avec des privilèges d’administrateur.  
  
3.  Définissez la stratégie d’exécution sur unrestricted:  
  
    ```powershell  
    PS C:\temp\scripts> .\makecert.ps1 C:\temp\scripts> Set-ExecutionPolicy –ExecutionPolicy Unrestricted   
    ```  
  
4.  Accédez au répertoire où vous avez copié le script.  
  
5.  Exécutez le script makecert:  
  
    ```powershell  
    PS C:\temp\scripts> .\makecert.ps1  
    ```  
  
6.  Lorsque vous êtes invité à modifier l’objet du certificat, entrez la nouvelle valeur de l’objet. Dans cet exemple, la valeur est **blueadfs.contoso.com**.  
  
7.  Lorsque vous êtes invité à entrer les noms SAN, appuyez sur Y, puis entrez les noms SAN, un par un.  
  
    Dans cet exemple, tapez **blueadfs.contoso.com** et appuyez sur Entrée, puis tapez **2016-adfs.contoso.com** et appuyez sur Entrée, puis tapez **enterpriseregistration.contoso.com** et appuyez sur Entrée.  
  
    Lorsque tous les noms SAN ont été entrés, appuyez sur Entrée au niveau d’une ligne vide.  
  
8.  Lorsque vous êtes invité à installer les certificats dans le magasin d’autorités de certification racines de confiance, appuyez sur Y.  
  
Le certificat ADFS doit être un certificat SAN avec les valeurs suivantes:  

-   nom du service ADFS.domaine

-   enterpriseregistration.domaine

-   nom du serveur ADFS.domaine

Dans l’exemple de test, les valeurs sont:  
  
-   **blueadfs.contoso.com**
  
-   **enterpriseregistration.contoso.com**
  
-   **2016-adfs.contoso.com**
  
Le SAN enterpriseregistration est nécessaire pour Workplace Join.  
  
### <a name="set-the-server-ip-address"></a>Configurer l’adresse IP du serveur  
Remplacez l’adresse IP de votre serveur par une adresse IP statique. Pour l’exemple de test, utilisez la classeIPA, qui est 192.168.0.160/Masque de sous-réseau: 255.255.0.0/Passerelle par défaut: 192.168.0.1/ServeurDNS préféré: 192.168.0.150 (l’adresseIP de votre contrôleur de domaine\).  
  
## <a name="install-the-ad-fs-role-service"></a>Installer le service de rôle ADFS  
Pour installer les services ADFS, procédez comme suit:  
  
1.  Ouvrez une session sur la machine physique ou virtuelle sur laquelle vous envisagez d’installer les services ADFS, ouvrez le **Gestionnaire de serveur** et démarrez l’Assistant Ajout de rôles et de fonctionnalités.  
  
2.  Dans la page **Rôles de serveur**, sélectionnez le rôle **Services de fédération Active Directory (AD FS)**, puis cliquez sur **Suivant**.  
  
3.  Dans la page **Services de fédération ActiveDirectory (ADFS)**, vous verrez un message indiquant que le rôle Proxy d’application Web ne peut pas être installé sur le même ordinateur que les services ADFS. Cliquez sur **Suivant**.  
  
4.  Cliquez sur **Installer** dans la page de confirmation.  
  
Pour réaliser la même installation d’ADFS via WindowsPowerShell, utilisez les commandes suivantes:  
  
```powershell  
Add-WindowsFeature RSAT-AD-Tools  
Add-WindowsFeature AD FS-Federation –IncludeManagementTools  
```  
  
## <a name="configure-ad-fs"></a>Configurer ADFS  
Configurez ensuite les services ADFS en utilisant le Gestionnaire de serveur ou WindowsPowerShell.  
  
### <a name="configure-ad-fs-by-using-server-manager"></a>Configurer ADFS à l’aide du Gestionnaire de serveur  
Pour configurer ADFS à l’aide du Gestionnaire de serveur, procédez comme suit:  
  
1.  Ouvrez le Gestionnaire de serveur.  
  
2.  Cliquez sur l’indicateur **Notifications** en haut de la fenêtre Gestionnaire de serveur, puis cliquez sur **Configurez le service FS (Federation Service) sur ce serveur**.  
  
3.  L’Assistant Configuration des services ADFS (Active Directory Federation Services) s’ouvre. Dans la page **Connexion à ADDS**, entrez le compte d’administrateur de domaine que vous souhaitez utiliser en tant que compte ADFS, puis cliquez sur **Suivant**.  
  
4.  Dans la page **Spécifier les propriétés de service**, entrez le nom de l’objet du certificat SSL (Secure Sockets Layer) à utiliser pour la communication ADFS. Dans l’exemple de test, il s’agit de **blueadfs.contoso.com**.  
  
5.  Entrez le nom du Service de fédération. Dans l’exemple de test, il s’agit de **blueadfs.contoso.com**. Cliquez sur **Suivant**.  
  
    > [!NOTE]  
    > Le nom du Service de fédération ne doit pas utiliser le nom d’un serveur existant dans l’environnement. Si vous utilisez le nom d’un serveur existant, l’installation d’ADFS échoue et vous devrez la relancer.  
  
6.  Dans la page **Spécifier un compte de service**, entrez le nom que vous souhaitez utiliser pour le compte de service géré. Dans l’exemple de test, sélectionnez **Créer un compte de service géré de groupe** et dans **Nom du compte**, entrez **ADFSService**. Cliquez sur **Suivant**.  
  
7.  Dans la page **Spécifier une base de données de configuration**, sélectionnez **Créez une base de données sur ce serveur à l’aide de la base de données interne Windows** et cliquez sur **Suivant**.  
  
8.  La page **Examiner les options** vous donne une vue d’ensemble des options que vous avez sélectionnées. Cliquez sur **Suivant**.  
  
9. La page **Vérifications des conditions préalables** indique si toutes les vérifications de la configuration requise ont donné satisfaction. S’il n’existe aucun problème, cliquez sur **Configurer**.  
  
    > [!NOTE]  
    > Si vous avez utilisé le nom du serveur ADFS ou de n’importe quel autre ordinateur existant pour le nom du service de fédération, un message d’erreur s’affiche. Vous devez recommencer l’installation et choisir un nom autre que celui d’un ordinateur existant.  
  
10. Une fois la configuration terminée, la page **Résultats** confirme que les services ADFS ont été correctement configurés.  
  
### <a name="configure-ad-fs-by-using-powershell"></a>Configurer ADFS à l’aide de PowerShell  
Pour effectuer la même configuration d’ADFS via Windows PowerShell, utilisez les commandes suivantes.  
  
Pour installer les services ADFS:  
  
```powershell  
Add-WindowsFeature RSAT-AD-Tools  
Add-WindowsFeature ADFS-Federation -IncludeManagementTools   
```  
  
Pour créer le compte de service géré:  
  
```powershell  
New-ADServiceAccount "ADFSService"-Server 2016-DC.contoso.com -Path "CN=Managed Service Accounts,DC=Contoso,DC=COM" -DNSHostName 2016-ADFS.contoso.com -ServicePrincipalNames HTTP/2016-ADFS,HTTP/2016-ADFS.contoso.com  
```  
  
Après avoir configuré les services ADFS, vous devez configurer une batterie ADFS en utilisant le compte de service géré que vous avez créé à l’étape précédente et le certificat que vous avez créé au cours des étapes préalables à la configuration.  
  
Pour configurer une batterie ADFS:  
  
```powershell  
$cert = Get-ChildItem CERT:\LocalMachine\My |where {$_.Subject -match blueadfs.contoso.com} | sort $_.NotAfter -Descending | select -first 1    
$thumbprint = $cert.Thumbprint  
Install-ADFSFarm -CertificateThumbprint $thumbprint -FederationServiceDisplayName "Contoso Corporation" –FederationServiceName blueadfs.contoso.com -GroupServiceAccountIdentifier contoso\ADFSService$ -OverwriteConfiguration -ErrorAction Stop  
```  
  
Étape suivante: [Déployer Dossiers de travail avec ADFS et le proxy d’application Web: Étape2, Tâches post-configuration ADFS](deploy-work-folders-adfs-step2.md)  
  
## <a name="see-also"></a>Voir aussi  
[Vue d’ensemble des Dossiers de travail](Work-Folders-Overview.md)  
  

