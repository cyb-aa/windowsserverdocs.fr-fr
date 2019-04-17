---
ms.assetid: 4deff06a-d0ef-4e5a-9701-5911ba667201
title: Outil de restauration rapide ADFS AD
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 02/20/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: cd1cc8dab07288ba73c507bc551f089bb79502bc
ms.sourcegitcommit: 36d7b1dfd7da8e9f303d007a628e76149de000f2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/21/2018
---
# <a name="ad-fs-rapid-restore-tool"></a>Outil de restauration rapide ADFS AD

>S’applique à: Windows Server2016, Windows Server2012R2

## <a name="overview"></a>Vue d’ensemble
Aujourd'hui, AD FS soient hautement disponibles en définissant une batterie AD FS. Certaines organisations seraient comme un moyen d’avoir un seul serveur de déploiement d’AD FS, éliminant ainsi le besoin de plusieurs serveurs AD FS et équilibrage infrastructure, tout en gardant certaines assurance de service peut être restaurée rapidement s’il existe un problème.
Le nouvel outil Restauration rapide de AD FS fournit un moyen de restaurer des données AD FS sans avoir besoin d’une sauvegarde complète et une restauration du système d’exploitation ou de l’état du système. Vous pouvez utiliser le nouvel outil pour exporter la configuration AD FS vers Azure ou vers un emplacement local.  Ensuite, vous pouvez appliquer les données exportées à une nouvelle installation d’AD FS, nouvelle création ou de la duplication de l’environnement AD FS. 

## <a name="scenarios"></a>Scénarios
L’outil Restauration rapide de AD FS peut être utilisé dans les scénarios suivants:

1. Restaurer rapidement des fonctionnalités AD FS après un problème
    - Utilisez l’outil pour créer une installation de mise en veille à froid d’AD FS qui peut être déployée rapidement à la place du serveur AD FS en ligne
2. Déployer des environnements de test et de production identiques
    - Utilisez l’outil pour créer rapidement une copie exacte de la production AD FS dans un environnement de test, ou pour déployer rapidement une configuration de test validé en production

## <a name="what-is-backed-up"></a>Ce qui est sauvegardé
L’outil de sauvegarde de la configuration AD FS suivante
    
- AD FS configuration base de données (SQL ou WID)
- Fichier de configuration (situé dans le dossier AD FS)
- Généré automatiquement de signature de jeton et le déchiffrement des certificats et clés privées (à partir du conteneur de la gestion distribuée de clés Active Directory)
- Certificat SSL et tout inscrits en externe (signature de jetons, déchiffrement de jeton et de communication du service) des certificats et clés privées correspondantes (Remarque: l’utilisateur qui exécute le script doit disposer des autorisations pour y accéder et les clés privées doivent être exportables)
- Une liste des fournisseurs d’authentification personnalisés, les magasins d’attributs et fournisseur de revendications local approbations qui sont installés.

## <a name="how-to-use-the-tool"></a>Comment utiliser l’outil
Tout d’abord, [télécharger](https://go.microsoft.com/fwlink/?LinkId=825646) et installer le fichier MSI sur votre serveur AD FS.  

>[!NOTE]
>L’outil de restauration rapide ADFS n’est pas conforme aux normes FIPS.

À partir d’une invite de commandes PowerShell, exécutez la commande suivante:

```powershell
import-module 'C:\Program Files (x86)\ADFS Rapid Recreation Tool\ADFSRapidRecreationTool.dll'
```

>[!NOTE] 
>Si vous utilisez la base de données intégrée de Windows (WID), cet outil doit être exécuté sur le serveur AD FS principal.  Vous pouvez utiliser le `Get-SyncProperties` applet de commande PowerShell pour déterminer si le serveur que vous êtes sur le serveur principal.

### <a name="system-requirements"></a>Configuration système requise

- Cet outil fonctionne pour les services AD FS dans Windows Server 2012 R2 et versions ultérieures. 
- Le .NET framework requis est au moins 4.0. 
- La restauration doit être effectuée sur un serveur AD FS de la même version que la sauvegarde et qui utilise le même compte Active Directory comme compte de service AD FS.

## <a name="create-a-backup"></a>Créer une sauvegarde
Pour créer une sauvegarde, utilisez l’applet de commande AD FS de la sauvegarde. Cette applet de commande sauvegarde de la configuration AD FS, base de données, les certificats SSL, etc.. 

L’utilisateur doit être au moins un administrateur local pour exécuter cette applet de commande. Pour sauvegarder le conteneur de gestion distribuée de clés Active Directory (obligatoire configuration par défaut AD FS), l’utilisateur doit être administrateur de domaine ainsi ou doit passer les informations d’identification du compte de service AD FS.

La sauvegarde est nommée selon le modèle «adfsBackup_ID_Date-temps». Il contient le numéro de version, la date et l’heure auxquelles la sauvegarde a été effectuée.
L’applet de commande accepte les paramètres suivants:
    
Jeux de paramètres

![Outil de restauration rapide ADFS AD](media/AD-FS-Rapid-Restore-Tool/parameter1.png)

### <a name="detailed-description"></a>Description détaillée

- **BackupDKM** -sauvegarde le conteneur de gestion distribuée de clés Active Directory qui contient les clés d’AD FS dans la configuration par défaut (jeton généré automatiquement la signature et le déchiffrement des certificats). Il utilise un outil de AD «ldifde» pour exporter le conteneur Active Directory et tous ses sous-arbres.

- -**StorageType &lt;chaîne&gt; ** -le type de stockage de l’utilisateur souhaite utiliser. «FileSystem» indique que l’utilisateur souhaite enregistrer dans un dossier local ou dans le réseau «Azure» indique l’utilisateur souhaite enregistrer dans le conteneur de stockage Azure lorsque l’utilisateur exécute la sauvegarde, ils sélectionner l’emplacement de sauvegarde, le système de fichiers ou dans le cloud. Pour Azure être utilisé, les informations d’identification de stockage Azure doit être passées à l’applet de commande. Les informations d’identification de stockage contient le nom de compte et la clé. En outre, un nom de conteneur doit également être transmis dans. Si le conteneur n’existe pas, il est créé lors de la sauvegarde. Pour le système de fichiers à utiliser un chemin d’accès de stockage doit être donnée. Dans ce répertoire, un nouveau répertoire est créé pour chaque sauvegarde. Chaque répertoire créé contiendra les fichiers sauvegardés. 

- **Cryptage &lt;chaîne&gt; ** -le mot de passe qui va être utilisée pour chiffrer tous les fichiers sauvegardés avant de les stocker

- **AzureConnectionCredentials &lt;pscredential&gt; ** -le nom de compte et la clé pour le compte de stockage Azure

- **AzureStorageContainer &lt;chaîne&gt; ** -le conteneur de stockage où la sauvegarde est stockée dans Azure

- **StoragePath &lt;chaîne&gt; ** -l’emplacement les sauvegardes seront stockées dans

- **ServiceAccountCredential &lt;pscredential&gt; ** -Spécifie le compte de service utilisé pour le Service AD FS en cours d’exécution. Ce paramètre est uniquement nécessaire si l’utilisateur souhaite la gestion distribuée de clés de sauvegarde et n’est pas administrateur de domaine.

- **BackupComment &lt;string []&gt; ** -une chaîne d’information sur la sauvegarde qui sera affichée lors de la restauration, similaire au concept de nom de point de contrôle de Hyper-V. La valeur par défaut est une chaîne vide

 
## <a name="backup-examples"></a>Exemples de sauvegarde
Voici des exemples de sauvegarde pour l’utilisation de l’outil de restauration rapide ADFS.

### <a name="backup-the-ad-fs-configuration-with-the-dkm-to-the-file-system-while-running-as-the-domain-admin"></a>Sauvegarde de la configuration ADFS, avec la gestion distribuée de clés, pour le système de fichiers en cours d’exécution en tant que l’administrateur de domaine

```powershell
Backup-ADFS -StorageType "FileSystem" -StoragePath "C:\Users\administrator\testExport\" -EncryptionPassword "password" -BackupComment "Clean Install of ADFS (FS)" -BackupDKM
```
 
### <a name="backup-the-ad-fs-configuration-with-the-dkm-to-the-file-system-with-the-service-account-credential-running-as-local-admin"></a>Configuration de FS la sauvegarde ActiveDirectory, avec la gestion distribuée de clés, au système de fichiers avec les informations d’identification compte service, en cours d’exécution en tant qu’administrateur local

```powershell
Backup-ADFS -StorageType "FileSystem" -StoragePath "C:\Users\administrator\testExport\" -EncryptionPassword "password" -BackupComment "Clean Install of ADFS (FS)" -BackupDKM -ServiceAccountCredential $cred
```

### <a name="backup-the-ad-fs-configuration-without-the-dkm-to-the-azure-storage-container"></a>Sauvegarde de la configuration ADFS sans la gestion distribuée de clés pour le conteneur de stockage Azure.

```powershell
Backup-ADFS -StorageType "Azure" -AzureConnectionCredentials $cred -AzureStorageContainer "adfsbackups"  -EncryptionPassword "password" -BackupComment "Clean Install of ADFS"
```

### <a name="backup-the-ad-fs-configuration-without-the-dkm-to-the-file-system"></a>La configuration ADFS sans la gestion distribuée de clés pour le système de fichiers de sauvegarde

```powershell   
Backup-ADFS -StorageType "FileSystem" -StoragePath "C:\Users\administrator\testExport\" -EncryptionPassword "password" -BackupComment "Clean Install of ADFS (FS)"
```

## <a name="restore-from-backup"></a>Restaurer à partir de la sauvegarde
Pour appliquer une configuration créée à l’aide d’ADFS de sauvegarde vers une nouvelle installation d’AD FS, utilisez l’applet de commande Restore-ADFS.

Cette applet de commande crée une nouvelle batterie AD FS à l’aide de l’applet de commande `Install-AdfsFarm` et restaure la configuration AD FS, base de données, les certificats, etc.. Si le rôle AD FS n’a pas été installé sur le serveur, l’applet de commande installer.  L’applet de commande vérifie l’emplacement de restauration pour les sauvegardes existantes et invite l’utilisateur à choisir une sauvegarde appropriée en fonction de la date/heure qui qu'il a été effectuée et tout commentaire de sauvegarde que l’utilisateur peut avoir attaché à la sauvegarde. S’il existe plusieurs configurations AD FS avec des noms de service de fédération, l’utilisateur est invité à choisir tout d’abord la configuration AD FS appropriée.
L’utilisateur doit être administrateur local et de domaine pour exécuter cette applet de commande.


>[!NOTE] 
>Avant d’utiliser l’outil de récupération rapide AD FS, assurez-vous que le serveur est joint au domaine avant la restauration de la sauvegarde. 

L’applet de commande accepte les paramètres suivants: 

![Outil de restauration rapide ADFS AD](media/AD-FS-Rapid-Restore-Tool/parameter2.png)

### <a name="detailed-description"></a>Description détaillée

- **StorageType &lt;chaîne&gt; ** -le type de stockage de l’utilisateur souhaite utiliser.
 «FileSystem» indique que l’utilisateur souhaite enregistrer dans un dossier local ou dans le réseau «Azure» indique que l’utilisateur souhaite stocker dans le conteneur de stockage Azure

- **DecryptionPassword &lt;chaîne&gt; ** -le mot de passe qui a été utilisée pour chiffrer tous les fichiers sauvegardés 

- **AzureConnectionCredentials &lt;pscredential&gt; ** -le nom de compte et la clé pour le compte de stockage Azure

- **AzureStorageContainer &lt;chaîne&gt; ** -le conteneur de stockage où la sauvegarde est stockée dans Azure

- **StoragePath &lt;chaîne&gt; ** -l’emplacement les sauvegardes seront stockées dans

- **ADFSName &lt; chaîne &gt; ** -le nom de la fédération qui a été sauvegardé et va être restauré. Si ce n’est pas fourni et nom du service FS qu’une seule puis qui servira. S’il existe plus d’un service de fédération sauvegardé à l’emplacement, puis l’utilisateur est invité à choisir l’un des Services de fédération de sauvegarde.

- **ServiceAccountCredential &lt; pscredential &gt; ** -Spécifie le compte de service qui sera utilisé pour le nouveau Service AD FS en cours de restauration 

- **GroupServiceAccountIdentifier &lt;chaîne&gt; ** -le compte GMSA que l’utilisateur veut à utiliser pour le nouveau Service AD FS en cours de restauration. Par défaut, si aucun n’est fourni puis sauvegardé de nom de compte est utilisé si elle a été GMSA autre l’utilisateur est invité à placer dans un compte de service

- **DBConnectionString &lt;chaîne&gt; ** - si l’utilisateur souhaite utiliser une autre base de données pour la restauration, puis ils doivent passer la chaîne de connexion SQL ou de type WID pour WID.

- **Force &lt;bool&gt; ** -ignorer les invites de saisie l’outil peut-être une fois que la sauvegarde est choisie.

- **RestoreDKM &lt;bool&gt; ** : restaurez le conteneur de gestion distribuée de clés pour la publicité, doit être défini si une nouvelle publicité et la gestion distribuée de clés a été sauvegardé initialement.

## <a name="restore-examples"></a>Restaurer des exemples

### <a name="restore-the-ad-fs-configuration-without-the-dkm-from-the-azure-storage-container"></a>Restaurer la configuration ADFS sans la gestion distribuée de clés à partir du conteneur de stockage Azure

```powershell
Restore-ADFS -StorageType "Azure" -AzureConnectionCredential $cred -DecryptionPassword "password" -AzureStorageContainer "adfsbackups"
```

### <a name="restore-the-ad-fs-configuration-without-the-dkm-from-the-file-system"></a>Restaurer la configuration ADFS sans la gestion distribuée de clés du système de fichiers
 
```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\uSERS\administrator\testExport\" -DecryptionPassword "password"
```

### <a name="restore-the-ad-fs-configuration-with-the-dkm-to-the-file-system"></a>Restaurer la configuration ADFS avec la gestion distribuée de clés pour le système de fichiers 
 
```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\uSERS\administrator\testExport\" -DecryptionPassword "password" -RestoreDKM
```

### <a name="restore-the-ad-fs-configuration-to-wid"></a>Restaurer la Configuration des services ADFS à WID

```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\uSERS\administrator\testExport\" -DecryptionPassword "password" -DBConnectionString "WID"
``` 

### <a name="restore-the-ad-fs-configuration-to-sql"></a>Restaurer la Configuration des services ADFS à SQL

```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\uSERS\administrator\testExport\" -DecryptionPassword "password" -DBConnectionString "Data Source=TESTMACHINE\SQLEXPRESS; Integrated Security=True"
```

### <a name="restores-the-ad-fs-configuration-with-the-specified-gmsa"></a>Restaure la Configuration des services ADFS avec le compte GMSA spécifié

```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\uSERS\administrator\testExport\" -DecryptionPassword "password" -GroupServiceAccountIdentifier "mangupd1\adfsgmsa$"
```
### <a name="restore-the-ad-fs-configuration-with-the-specified-service-account-creds"></a>Restaurer la Configuration des services ADFS avec les informations d’identification du compte de service spécifié

```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\uSERS\administrator\testExport\" -DecryptionPassword "password" -ServiceAccountCredential $cred
```

## <a name="encryption-information"></a>Informations de chiffrement
Toutes les données de sauvegarde est chiffré avant qu’il fait d’appuyer sur le cloud ou de les stocker dans le système de fichiers.  

Chaque document qui est créé dans le cadre de la sauvegarde est chiffré à l’aide de AES-256. Le mot de passe transmis dans l’outil est utilisé comme un mot de passe pour générer un nouveau mot de passe à l’aide de la classe Rfc2898DeriveBytes. 

RngCryptoServiceProvider est utilisé pour générer le salt utilisé par AES et la classe Rfc2898DeriveBytes. 

## <a name="log-files"></a>Fichiers journaux
Chaque fois qu’une sauvegarde ou restauration s’effectue un fichier journal est créé. Ils sont accessibles à l’emplacement suivant:

- **%LocalAppData%\ADFSRapidRecreationTool**

>[!NOTE]
> Lorsque les magasins d’attributs effectuer une restauration que peut créer un fichier PostRestore_Instructions contenant une vue d’ensemble des fournisseurs d’authentification supplémentaires, et approbations de fournisseur de revendications local doivent être installés manuellement avant de démarrer le service AD FS.
