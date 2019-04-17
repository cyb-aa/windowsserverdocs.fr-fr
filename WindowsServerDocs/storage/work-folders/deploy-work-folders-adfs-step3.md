---
title: "Déployer Dossiers de travail avec ADFS et le proxy d’application Web: Étape3, Configurer Dossiers de travail"
ms.prod: windows-server-threshold
ms.technology: storage-work-folders
ms.topic: article
manager: klaasl
ms.author: jeffpatt
author: JeffPatt24
ms.date: 4/5/2017
ms.assetid: 5a43b104-4d02-4d73-a385-da1cfb67e341
ms.openlocfilehash: 81f30a7a4d50423a68719343fec3032cc6a1602e
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/17/2017
---
# <a name="deploy-work-folders-with-ad-fs-and-web-application-proxy-step-3-set-up-work-folders"></a>Déployer Dossiers de travail avec ADFS et le proxy d’application Web: étape3, configurer les dossiers de travail

>S’applique à: WindowsServer (canal semi-annuel), WindowsServer2016

Cette rubrique décrit la troisièmeétape de déploiement Dossiers de travail avec les services de fédération ActiveDirectory (ADFS) et le proxy d’application Web. Les autres étapes de ce processus sont décrites dans les rubriques suivantes:  
  
-   [Déployer Dossiers de travail avec ADFS et le proxy d’application Web: vue d’ensemble](deploy-work-folders-adfs-overview.md)  
  
-   [Déployer Dossiers de travail avec ADFS et le proxy d’application Web: étape1, configurer ADFS](deploy-work-folders-adfs-step1.md)  
  
-   [Déployer Dossiers de travail avec ADFS et le proxy d’application Web: Étape2, Tâches post-configuration ADFS](deploy-work-folders-adfs-step2.md)  
  
-   [Déployer Dossiers de travail avec ADFS et le proxy d’application Web: Étape4, Configurer le proxy d’application Web](deploy-work-folders-adfs-step4.md)  
  
-   [Déployer Dossiers de travail avec ADFS et le proxy d’application Web: étape5, configurer les clients](deploy-work-folders-adfs-step5.md)  
  
> [!NOTE]
>   Les instructions présentées dans cette section ont été conçues pour un environnement WindowsServer2016. Si vous utilisez Windows Server2012R2, suivez les [instructions pour Windows Server2012R2](https://technet.microsoft.com/library/dn747208(v=ws.11).aspx).

Pour configurer Dossiers de travail, procédez comme suit.  
  
## <a name="pre-installment-work"></a>Tâche de préinstallation  
Pour pouvoir installer Dossiers de travail, vous devez disposer d’un serveur joint au domaine et exécutant WindowsServer2016. La configuration réseau du serveur doit être valide.  
  
Dans l’exemple de test, joignez l’ordinateur qui exécute Dossiers de travail au domaine Contoso et configurez l’interface réseau comme décrit dans les sections suivantes. 

### <a name="set-the-server-ip-address"></a>Configurer l’adresse IP du serveur  
Changez l’adresse IP de votre serveur en adresse IP statique. Pour l’exemple de test, utilisez la classeIPA, qui est 192.168.0.170/Masque de sous-réseau: 255.255.0.0/Passerelle par défaut: 192.168.0.1/ServeurDNS préféré: 192.168.0.150 (l’adresseIP de votre contrôleur de domaine). 
  
### <a name="create-the-cname-record-for-work-folders"></a>Créer l’enregistrement CNAME pour Dossiers de travail  
Pour créer l’enregistrement CNAME pour Dossiers de travail, procédez comme suit:  
  
1.  Sur votre contrôleur de domaine, ouvrez le **GestionnaireDNS**.  
  
2.  Développez le dossier Zones de recherche directe, cliquez avec le bouton droit sur votre domaine, puis cliquez sur **Nouvel Alias (CNAME)**.  
  
3.  Dans la fenêtre **Nouvel enregistrement de ressource**, dans le champ **Nom de l’alias**, entrez l’alias de Dossiers de travail. Dans l’exemple de test, il s’agit de **workfolders**.  
  
4.  Dans le champ **Nom de domaine complet**, la valeur doit être **workfolders.contoso.com**.  
  
5.  Dans le champ **Nom de domaine complet de l’hôte cible**, entrez le nom de domaine complet du serveur Dossiers de travail. Dans l’exemple de test, il s’agit de **2016-WF.contoso.com**.  
  
6.  Cliquez sur **OK**.  
  
Pour effectuer les mêmes étapes via WindowsPowerShell, utilisez la commande suivante. La commande doit être exécutée sur le contrôleur de domaine.  
  
```powershell  
Add-DnsServerResourceRecord  -ZoneName "contoso.com" -Name workfolders -CName  -HostNameAlias 2016-wf.contoso.com   
```  
  
### <a name="install-the-ad-fs-certificate"></a>Installer le certificat ADFS  
Installez le certificat ADFS qui a été créé pendant l’installation d’ADFS dans le magasin de certificats de l’ordinateur local, en procédant comme suit:  
  
1.  Cliquez sur **Démarrer**, puis sur **Exécuter**.  
  
2.  Tapez **MMC**.  
  
3.  Dans le menu **Fichier**, cliquez sur **Ajouter/Supprimer un composant logiciel enfichable**.  
  
4.  Dans la zone **Composants logiciels enfichables disponibles**, cliquez sur **Certificats**, puis sur **Ajouter**. L’assistant Composant logiciel enfichable Certificats démarre.  
  
5.  Sélectionnez **Un compte d’ordinateur**, puis cliquez sur **Suivant**.  
  
6.  Sélectionnez **Ordinateur local (l’ordinateur sur lequel cette console s’exécute)**, puis cliquez sur **Terminer**.  
  
7.  Cliquez sur **OK**.  
  
8.  Développez le dossier **Racine de la console\Certificats\(Ordinateur local)\Personnel\Certificats**.  
  
9. Cliquez avec le bouton droit sur **Certificats**, cliquez sur **Toutes les tâches**, puis cliquez sur **Importer**.  
  
10. Accédez au dossier contenant le certificat ADFS et suivez les instructions de l’Assistant pour importer le fichier et le placer dans le magasin de certificats.

11. Développez le dossier **Racine de la console\Certificats\(Ordinateur local)\Autorités de certification racines de confiance\Certificats**.  
  
12. Cliquez avec le bouton droit sur **Certificats**, cliquez sur **Toutes les tâches**, puis cliquez sur **Importer**.  
  
13. Accédez au dossier contenant le certificat ADFS, puis suivez les instructions de l’assistant pour importer le fichier et le placer dans le magasin des autorités de certification racines de confiance.  
  
### <a name="create-the-work-folders-self-signed-certificate"></a>Créer le certificat auto-signé Dossiers de travail  
Pour créer le certificat auto-signé Dossiers de travail, procédez comme suit:  
  
1.  Télécharger les scripts fournis dans billet de blog [Deploying Work Folders with AD FS and Web Application Proxy](https://blogs.technet.microsoft.com/filecab/2014/03/03/deploying-work-folders-with-ad-fs-and-web-application-proxy-wap), puis copiez le fichier makecert.ps1 sur l’ordinateur de Dossiers de travail.  
  
2.  Ouvrez une fenêtre Windows PowerShell avec des privilèges d’administrateur.  
  
3.  Définissez la stratégie d’exécution sur unrestricted:  
  
    ```powershell  
    PS C:\temp\scripts> Set-ExecutionPolicy -ExecutionPolicy Unrestricted   
    ```  
  
4.  Passez au répertoire où vous avez copié le script.  
  
5.  Exécutez le script makecert:  
  
    ```powershell  
    PS C:\temp\scripts> .\makecert.ps1  
    ```  
  
6.  Lorsque vous êtes invité à modifier l’objet du certificat, entrez la nouvelle valeur pour l’objet. Dans cet exemple, la valeur est **workfolders.contoso.com**.  
  
7.  Lorsque vous êtes invité à entrer les noms SAN (Subject alternative name), appuyez sur Y, puis entrez les noms SAN, un à la fois.  
  
    Dans cet exemple, tapez **workfolders.contoso.com**, puis appuyez sur Entrée. Tapez ensuite **2016-WF.contoso.com**, puis appuyez sur Entrer.  
  
    Lorsque tous les noms SAN ont été entrés, appuyez sur Entrée dans une ligne vide.  
  
8.  Lorsque vous êtes invité à installer les certificats dans le magasin d’autorités de certification racines de confiance, appuyez sur Y.  
  
Le certificat Dossiers de travail doit être un certificatSAN avec les valeurs suivantes:  
  
-   **workfolders**.**domaine**  
  
-   **nom de l’ordinateur**.**domaine**  
  
Dans l’exemple de test, les valeurs sont:  
  
-   **workfolders.contoso.com**  
  
-   **2016-WF.contoso.com**  
  
## <a name="install-work-folders"></a>Installer Dossiers de travail  
Pour installer le rôle Dossiers de travail, procédez comme suit:  
  
1.  Ouvrez le **Gestionnaire de serveur**, sélectionnez **Ajouter des rôles et des fonctionnalités**, puis cliquez sur **Suivant**.  
  
2.  Dans la page **Type d’installation**, sélectionnez **Installation basée sur un rôle ou une fonctionnalité**, puis cliquez sur **Suivant**.  
  
3.  Dans la page **Sélection du serveur**, sélectionnez le serveur actuel et cliquez sur **Suivant**.  
  
4.  Dans la page **Rôles de serveur**, développez **Services de fichiers et de stockage**, développez **Services de fichiers et iSCSI**, puis sélectionnez **Dossiers de travail**.  
  
5.  Dans la boîte de dialogue **Assistant Ajout de rôles et de fonctionnalités**, cliquez sur **Ajouter des fonctionnalités**, puis sur **Suivant**.  
  
6.  Dans la page **Fonctionnalités** cliquez sur **Suivant**.  
  
7.  Dans la page **Confirmation**, cliquez sur **Installer**.  
  
## <a name="configure-work-folders"></a>Configurer Dossiers de travail  
Pour configurer Dossiers de travail, procédez comme suit:  
  
1.  Ouvrez le **Gestionnaire de serveur**.  
  
2.  Sélectionnez **Services de fichiers et de stockage**, puis **Dossiers de travail**.  
  
3.  Dans la page **Dossiers de travail**, démarrez l’**Assistant Nouveau partage de synchronisation**, puis cliquez sur **Suivant**.  
  
4.  Dans la page **Serveur et chemin d’accès**, sélectionnez le serveur où le partage de synchronisation sera créé, entrez un chemin d’accès local où seront stockées les données de Dossiers de travail, puis cliquez sur **Suivant**.  
  
    Si le chemin d’accès n’existe pas, il vous sera demandé de le créer. Cliquez sur **OK**.  
  
5.  Dans la page **Structure des dossiers utilisateur**, sélectionnez **Alias utilisateur**, puis cliquez sur **Suivant**.  
  
6.  Dans la page **Nom du partage de synchronisation**, entrez le nom du partage de synchronisation. Dans l’exemple de test, il s’agit de **WorkFolders**. Cliquez sur **Suivant**.  
  
7.  Dans la page **Accès à la synchronisation**, ajoutez les utilisateurs ou les groupes qui auront accès au nouveau partage de synchronisation. Dans l’exemple de test, accordez l’accès à tous les utilisateurs du domaine. Cliquez sur **Suivant**.  
  
8.  Dans la page **Stratégies de sécurité PC**, sélectionnez **Chiffrer les dossiers de travail** et **Verrouiller automatiquement l’écran et exiger un mot de passe**. Cliquez sur **Suivant**.  
  
9. Dans la page **Confirmation**, cliquez sur **Créer** pour terminer le processus de configuration.  
  
## <a name="work-folders-post-configuration-work"></a>Dossiers de travail tâches post-configuration  
Pour terminer la configuration de Dossiers de travail, effectuez les étapes supplémentaires suivantes:  
  
-   Lier le certificat Dossiers de travail au portSSL  
  
-   Configurer Dossiers de travail pour utiliser l’authentification ADFS  
  
-   Exporter le certificat Dossiers de travail (si vous utilisez un certificat auto-signé)  
  
### <a name="bind-the-certificate"></a>Lier le certificat  
Dossiers de travail communique uniquement via SSL et doit posséder un certificat auto-signé lié au port, créé au préalable (ou que votre autorité de certification a émis).  
  
Deuxméthodes sont à votre disposition pour lier le certificat au port via WindowsPowerShell: les applets de commande IIS et netsh.  
  
#### <a name="bind-the-certificate-by-using-netsh"></a>Lier le certificat à l’aide de netsh  
Pour utiliser l’utilitaire de script de ligne de commande netsh dans WindowsPowerShell, vous devez diriger la commande vers netsh. L’exemple de script suivant recherche le certificat avec l’objet **workfolders.contoso.com** et les lie au port443 à l’aide de netsh:  
  
```powershell  
$subject = "workfolders.contoso.com"   
Try  
{  
#In case there are multiple certificates with the same subject, get the latest version   
$cert = Get-ChildItem CERT:\LocalMachine\My |where {$_.Subject -match $subject} | sort $_.NotAfter -Descending | select -first 1    
$thumbprint = $cert.Thumbprint  
$Command = "http add sslcert ipport=0.0.0.0:443 certhash=$thumbprint appid={CE66697B-3AA0-49D1-BDBD-A25C8359FD5D} certstorename=MY"  
$Command | netsh  
}  
Catch  
{  
"     Error: unable to locate certificate for $($subject)"  
Exit  
}   
```  
  
#### <a name="bind-the-certificate-by-using-iis-cmdlets"></a>Lier le certificat à l’aide des applets de commandeIIS  
Vous pouvez également lier le certificat au port à l’aide des applets de commande de gestion IIS, qui sont disponibles si vous avez installé les outils de gestion IIS et les scripts.  
  
> [!NOTE]  
> L’installation des outils de gestionIIS n’active pas la version complète des ServicesInternet (IIS). sur l’ordinateur de Dossiers de travail; il active uniquement les applets de commande de gestion. Cette configuration offre un certain nombre d’avantages. Par exemple, si vous recherchez des applets de commande pour fournir la fonctionnalité que vous obtenez de netsh. Lorsque le certificat est lié au port via l’applet de commande New-WebBinding, la liaison n’est en aucun cas pas dépendante d’IIS. Après avoir effectué la liaison, vous pouvez même supprimer la console d’administration Web, et le certificat sera toujours lié au port. Vous pouvez vérifier la liaison via netsh en tapant **netsh http show sslcert**.  
  
L’exemple suivant utilise l’applet de commande New-WebBinding pour trouver le certificat avec l’objet **workfolders.contoso.com** et le lier au port443:  
  
```powershell  
$subject = "workfolders.contoso.com"  
Try  
{  
#In case there are multiple certificates with the same subject, get the latest version   
$cert =Get-ChildItem CERT:\LocalMachine\My |where {$_.Subject -match $subject } | sort $_.NotAfter -Descending | select -first 1   
$thumbprint = $cert.Thumbprint  
New-WebBinding -Name "Default Web Site" -IP * -Port 443 -Protocol https  
#The default IIS website name must be used for the binding. Because Work Folders uses Hostable Web Core and its own configuration file, its website name, 'ECSsite', will not work with the cmdlet. The workaround is to use the default IIS website name, even though IIS is not enabled, because the NewWebBinding cmdlet looks for a site in the default IIS configuration file.   
Push-Location IIS:\SslBindings  
Get-Item cert:\LocalMachine\MY\$thumbprint | new-item *!443  
Pop-Location  
}  
Catch  
{  
"     Error: unable to locate certificate for $($subject)"  
Exit  
}   
```  
  
### <a name="set-up-ad-fs-authentication"></a>Configurer l’authentification ADFS  
Pour configurer Dossiers de travail de manière à utiliser les services ADFS pour l’authentification, procédez comme suit:  
  
1.  Ouvrez le **Gestionnaire de serveur**.  
  
2.  Cliquez sur **Serveurs**, puis sélectionnez votre serveur Dossiers de travail dans la liste.  
  
3.  Cliquez avec le bouton droit sur le nom du serveur, puis cliquez sur **Paramètres de Dossiers de travail**.  
  
4.  Dans la fenêtre **Paramètres de Dossier de travail**, sélectionnez **Services de fédération Active Directory (AD FS)** puis tapez l’URL du service de fédération. Cliquez sur **Appliquer**.  
  
    Dans l’exemple de test, l’URL est **https://blueadfs.contoso.com**.  
  
L’applet de commande permettant d’effectuer la même tâche via WindowsPowerShell est la suivante:  
  
```powershell  
Set-SyncServerSetting -ADFSUrl "https://blueadfs.contoso.com"   
```  
  
Si vous configurez des services ADFS avec des certificats auto-signés, vous pouvez recevoir un message d’erreur indiquant que l’URL du service de fédération est incorrecte, inaccessible, ou qu’une approbation de partie de confiance n’a pas été configurée.  
  
Cette erreur peut également survenir si le certificatADFS n’est pas installé sur le serveur Dossiers de travail ou si l’enregistrement CNAME pour ADFS n’était pas configuré correctement. Vous devez corriger ces problèmes avant de poursuivre.  
  
### <a name="export-the-work-folders-certificate"></a>Exporter le certificat Dossiers de travail  
Le certificat Dossiers de travail auto-signé doit être exporté afin que vous puissiez l’installer ultérieurement sur les ordinateurs suivants dans l’environnement de test:  
  
-   Le serveur qui est utilisé pour le proxy d’application Web  
  
-   Le client Windows joint au domaine  
  
-   Le client Windows qui n’est pas joint au domaine  
  
Pour exporter le certificat, suivez les mêmes étapes que vous avez utilisées pour exporter le certificatADFS précédemment, comme décrit dans [Déployer Dossiers de travail avec ADFS et le proxy d’application Web: Étape2, Tâches post-configuration ADFS](deploy-work-folders-adfs-step2.md), Exporter le certificatADFS.  
  
Étape suivante: [Déployer Dossiers de travail avec ADFS et le proxy d’application Web: Étape4, Configurer le proxy d’application Web](deploy-work-folders-adfs-step4.md)  
  
## <a name="see-also"></a>Voir aussi  
[Vue d’ensemble des Dossiers de travail](Work-Folders-Overview.md)  
  

