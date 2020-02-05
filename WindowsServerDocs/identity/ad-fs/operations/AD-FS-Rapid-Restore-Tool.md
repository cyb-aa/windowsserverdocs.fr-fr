---
ms.assetid: 4deff06a-d0ef-4e5a-9701-5911ba667201
title: Outil de restauration rapide AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/02/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 8b47cdc4770b1ed6478d1502ed5264164e99352b
ms.sourcegitcommit: a33404f92867089bb9b0defcd50960ff231eef3f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/04/2020
ms.locfileid: "77013044"
---
# <a name="ad-fs-rapid-restore-tool"></a>Outil de restauration rapide AD FS

## <a name="overview"></a>Vue d'ensemble
Aujourd’hui AD FS est rendu hautement disponible en configurant une batterie de serveurs AD FS. Certaines organisations souhaitent avoir un seul serveur AD FS déploiement, éliminant la nécessité de disposer de plusieurs serveurs AD FS et d’une infrastructure d’équilibrage de charge réseau, tout en ayant la garantie que le service peut être restauré rapidement en cas de problème.
Le nouvel outil de restauration rapide AD FS offre un moyen de restaurer des données AD FS sans avoir à effectuer une sauvegarde et une restauration complètes du système d’exploitation ou de l’état du système. Vous pouvez utiliser le nouvel outil pour exporter AD FS configuration vers Azure ou vers un emplacement local.  Vous pouvez ensuite appliquer les données exportées à une nouvelle installation de AD FS, en recréant ou en dupliquant l’environnement AD FS. 

## <a name="scenarios"></a>Scénarios
L’outil de restauration AD FS rapide peut être utilisé dans les scénarios suivants :

1. Restaurer rapidement AD FS fonctionnalité après un problème
    - Utilisez l’outil pour créer une installation de secours à froid de AD FS qui peut être déployée rapidement à la place du serveur de AD FS en ligne
2. Déployer des environnements de test et de production identiques
    - Utilisez l’outil pour créer rapidement une copie précise du AD FS de production dans un environnement de test ou pour déployer rapidement une configuration de test validée en production.

>[!NOTE] 
>Si vous utilisez la réplication de fusion SQL ou les groupes de disponibilité Always on, l’outil de restauration rapide n’est pas pris en charge. Nous vous recommandons d’utiliser des sauvegardes SQL et une sauvegarde du certificat SSL comme alternative.

## <a name="what-is-backed-up"></a>Ce qui est sauvegardé
L’outil sauvegarde la configuration de AD FS suivante
    
- Base de données de configuration de AD FS (SQL ou WID)
- Fichier de configuration (situé dans AD FS dossier)
- Signatures de jetons et déchiffrement de certificats et clés privées générés automatiquement (à partir du conteneur Active Directory DKM)
- Certificat SSL et tous les certificats inscrits en externe (signature de jetons, déchiffrement de jeton et communication de service) et clés privées correspondantes (Remarque : les clés privées doivent être exportables et l’utilisateur qui exécute le script doit avoir les autorisations nécessaires pour y accéder)
- Liste des fournisseurs d’authentification personnalisés, des magasins d’attributs et des approbations de fournisseur de revendications locaux qui sont installés.

## <a name="how-to-use-the-tool"></a>Comment utiliser l’outil
Tout d’abord, [Téléchargez](https://go.microsoft.com/fwlink/?LinkId=825646) et installez le MSI sur votre serveur AD FS.  

Exécutez la commande suivante à partir d’une invite de commandes PowerShell :

```powershell
import-module 'C:\Program Files (x86)\ADFS Rapid Recreation Tool\ADFSRapidRecreationTool.dll'
```

>[!NOTE] 
>Si vous utilisez la base de données intégrée de Windows (WID), cet outil doit être exécuté sur le serveur de AD FS principal.  Vous pouvez utiliser l’applet de commande `Get-AdfsSyncProperties` PowerShell pour déterminer si le serveur sur lequel vous êtes ou non est le serveur principal.

### <a name="system-requirements"></a>Configuration requise

- Cet outil fonctionne pour AD FS dans Windows Server 2012 R2 et versions ultérieures. 
- Le .NET Framework requis est au moins 4,0. 
- La restauration doit être effectuée sur un serveur AD FS de la même version que la sauvegarde et qui utilise le même compte Active Directory que le compte de service AD FS.

## <a name="create-a-backup"></a>Créer une sauvegarde
Pour créer une sauvegarde, utilisez l’applet de commande Backup-ADFS. Cette applet de commande sauvegarde la configuration AD FS, la base de données, les certificats SSL, etc. 

L’utilisateur doit être au moins un administrateur local pour exécuter cette applet de commande. Pour sauvegarder le conteneur Active Directory DKM (requis dans la configuration de AD FS par défaut), l’utilisateur doit être un administrateur de domaine, doit transmettre les informations d’identification du compte de service AD FS ou avoir accès au conteneur DKM.  Si vous utilisez un compte gMSA, l’utilisateur doit être administrateur de domaine ou disposer d’autorisations sur le conteneur. vous ne pouvez pas fournir les informations d’identification gMSA. 

La sauvegarde sera nommée selon le modèle « temps adfsBackup_ID_Date ». Elle contient le numéro de version, la date et l’heure de réalisation de la sauvegarde.
L’applet de commande prend les paramètres suivants :
    
Jeux de paramètres

![Outil de restauration rapide AD FS](media/AD-FS-Rapid-Restore-Tool/parameter1.png)

### <a name="detailed-description"></a>Description détaillée

- **BackupDKM** : sauvegarde le conteneur DKM Active Directory qui contient les clés de AD FS dans la configuration par défaut (génération automatique de certificats de signature et de déchiffrement de jetons). Cela utilise un outil AD « LDIFDE » pour exporter le conteneur AD et toutes ses sous-arborescences.

- -**StorageType &lt;chaîne&gt;** : type de stockage que l’utilisateur souhaite utiliser. « FileSystem » indique que l’utilisateur veut le stocker dans un dossier local ou dans le réseau « Azure » indique que l’utilisateur souhaite le stocker dans le conteneur de stockage Azure lorsque l’utilisateur effectue la sauvegarde, il sélectionne l’emplacement de sauvegarde, soit le système de fichiers, soit le pluie. Pour utiliser Azure, les informations d’identification de stockage Azure doivent être transmises à l’applet de commande. Les informations d’identification de stockage contiennent le nom et la clé du compte. En outre, un nom de conteneur doit également être transmis. Si le conteneur n’existe pas, il est créé au cours de la sauvegarde. Pour le système de fichiers à utiliser, un chemin d’accès de stockage doit être fourni. Dans ce répertoire, un nouveau répertoire est créé pour chaque sauvegarde. Chaque répertoire créé contiendra les fichiers sauvegardés. 

- **EncryptionPassword &lt;chaîne&gt;** : mot de passe qui sera utilisé pour chiffrer tous les fichiers sauvegardés avant de les stocker.

- **AzureConnectionCredentials &lt;pscredential&gt;** -le nom et la clé du compte de stockage Azure

- **AzureStorageContainer &lt;string&gt;** : le conteneur de stockage dans lequel la sauvegarde sera stockée dans Azure

- **StoragePath &lt;chaîne&gt;** : l’emplacement dans lequel les sauvegardes seront stockées.

- **ServiceAccountCredential &lt;pscredential&gt;** : spécifie le compte de service utilisé pour le service AD FS qui s’exécute actuellement. Ce paramètre n’est nécessaire que si l’utilisateur souhaite sauvegarder le DKM et n’est pas administrateur de domaine ou n’a pas accès au contenu du conteneur. 

- **BackupComment &lt;String []&gt;** -une chaîne d’information sur la sauvegarde qui sera affichée lors de la restauration, de la même façon que le concept de nom de point de contrôle Hyper-V. La valeur par défaut est une chaîne vide

 
## <a name="backup-examples"></a>Exemples de sauvegarde
Voici des exemples de sauvegarde pour l’utilisation de l’outil de restauration AD FS rapide.

### <a name="backup-the-ad-fs-configuration-with-the-dkm-to-the-file-system-and-has-access-to-the-dkm-container-contents-either-domain-admin-or-delegated"></a>Sauvegardez la configuration AD FS, avec DKM, dans le système de fichiers et ayant accès au contenu du conteneur DKM (administrateur de domaine ou délégué).

```powershell
Backup-ADFS -StorageType "FileSystem" -StoragePath "C:\Users\administrator\testExport\" -EncryptionPassword "password" -BackupComment "Clean Install of ADFS (FS)" -BackupDKM
```
 
### <a name="backup-the-ad-fs-configuration-with-the-dkm-to-the-file-system-with-the-service-account-credential-running-as-local-admin"></a>Sauvegardez la configuration AD FS, avec DKM, dans le système de fichiers avec les informations d’identification du compte de service, en étant exécuté en tant qu’administrateur local.

```powershell
Backup-ADFS -StorageType "FileSystem" -StoragePath "C:\Users\administrator\testExport\" -EncryptionPassword "password" -BackupComment "Clean Install of ADFS (FS)" -BackupDKM -ServiceAccountCredential $cred
```

### <a name="backup-the-ad-fs-configuration-without-the-dkm-to-the-azure-storage-container"></a>Sauvegardez la configuration AD FS sans DKM dans le conteneur de stockage Azure.

```powershell
Backup-ADFS -StorageType "Azure" -AzureConnectionCredentials $cred -AzureStorageContainer "adfsbackups"  -EncryptionPassword "password" -BackupComment "Clean Install of ADFS"
```

### <a name="backup-the-ad-fs-configuration-without-the-dkm-to-the-file-system"></a>Sauvegarder la configuration de AD FS sans DKM dans le système de fichiers

```powershell   
Backup-ADFS -StorageType "FileSystem" -StoragePath "C:\Users\administrator\testExport\" -EncryptionPassword "password" -BackupComment "Clean Install of ADFS (FS)"
```

## <a name="restore-from-backup"></a>Restaurer à partir d’une sauvegarde
Pour appliquer une configuration créée à l’aide de Backup-ADFS à une nouvelle installation de AD FS, utilisez l’applet de commande Restore-ADFS.

Cette applet de commande crée une batterie de AD FS à l’aide de l’applet de commande `Install-AdfsFarm` et restaure la configuration de la AD FS, la base de données, les certificats, etc.  Si le rôle de AD FS n’a pas été installé sur le serveur, l’applet de commande l’installe.  L’applet de commande vérifie l’emplacement de restauration des sauvegardes existantes et invite l’utilisateur à choisir une sauvegarde appropriée en fonction de la date/heure à laquelle elle a été prise et des commentaires de sauvegarde que l’utilisateur pourrait avoir attachés à la sauvegarde. S’il existe plusieurs configurations de AD FS avec des noms de service de Fédération différents, l’utilisateur est invité à choisir d’abord la configuration de AD FS appropriée.
L’utilisateur doit être à la fois local et administrateur de domaine pour exécuter cette applet de commande.


>[!NOTE] 
>Avant d’utiliser l’outil de récupération AD FS rapide, assurez-vous que le serveur est joint au domaine avant de restaurer la sauvegarde. 

L’applet de commande prend les paramètres suivants : 

![Outil de restauration rapide AD FS](media/AD-FS-Rapid-Restore-Tool/parameter2.png)

### <a name="detailed-description"></a>Description détaillée

- **StorageType &lt;chaîne&gt;** : type de stockage que l’utilisateur souhaite utiliser.
 « FileSystem » indique que l’utilisateur veut le stocker dans un dossier local ou dans le réseau « Azure » indique que l’utilisateur veut le stocker dans le conteneur de stockage Azure

- **DecryptionPassword &lt;chaîne&gt;** : mot de passe utilisé pour chiffrer tous les fichiers sauvegardés 

- **AzureConnectionCredentials &lt;pscredential&gt;** -le nom et la clé du compte de stockage Azure

- **AzureStorageContainer &lt;string&gt;** : le conteneur de stockage dans lequel la sauvegarde sera stockée dans Azure

- **StoragePath &lt;chaîne&gt;** : l’emplacement dans lequel les sauvegardes seront stockées.

- **ADFSName &lt; string &gt;** : nom de la Fédération qui a été sauvegardée et qui va être restaurée. Si ce n’est pas le cas et qu’il n’existe qu’un seul nom de service de Fédération, celui-ci sera utilisé. Si plusieurs services de Fédération sont sauvegardés à l’emplacement, l’utilisateur est invité à choisir l’un des services de Fédération sauvegardés.

- **ServiceAccountCredential &lt; pscredential &gt;** : spécifie le compte de service qui sera utilisé pour le nouveau service AD FS en cours de restauration. 

- **GroupServiceAccountIdentifier &lt;string&gt;** : GMSA que l’utilisateur souhaite utiliser pour le nouveau service AD FS en cours de restauration. Par défaut, si aucun n’est fourni, le nom du compte sauvegardé est utilisé s’il a été GMSA, sinon l’utilisateur est invité à insérer un compte de service

- **DBConnectionString &lt;string&gt;** : si l’utilisateur souhaite utiliser une autre base de donnes pour la restauration, il doit passer la chaîne de connexion SQL ou le type dans WID pour WID.

- **Force &lt;bool&gt;** -ignore les invites que l’outil peut avoir une fois que la sauvegarde est sélectionnée

- **RestoreDKM &lt;bool&gt;** -restaurer le conteneur DKM sur AD, doit être défini si vous accédez à une nouvelle publicité et que le DKM a été sauvegardé initialement.

## <a name="restore-examples"></a>Exemples de restauration

### <a name="restore-the-ad-fs-configuration-without-the-dkm-from-the-azure-storage-container"></a>Restaurer la configuration de AD FS sans DKM à partir du conteneur de stockage Azure

```powershell
Restore-ADFS -StorageType "Azure" -AzureConnectionCredential $cred -DecryptionPassword "password" -AzureStorageContainer "adfsbackups"
```

### <a name="restore-the-ad-fs-configuration-without-the-dkm-from-the-file-system"></a>Restaurer la configuration de AD FS sans DKM à partir du système de fichiers
 
```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\Users\administrator\testExport\" -DecryptionPassword "password"
```

### <a name="restore-the-ad-fs-configuration-with-the-dkm-to-the-file-system"></a>Restaurer la configuration de AD FS avec DKM dans le système de fichiers 
 
```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\Users\administrator\testExport\" -DecryptionPassword "password" -RestoreDKM
```

### <a name="restore-the-ad-fs-configuration-to-wid"></a>Restaurer la configuration de AD FS dans WID

```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\Users\administrator\testExport\" -DecryptionPassword "password" -DBConnectionString "WID"
``` 

### <a name="restore-the-ad-fs-configuration-to-sql"></a>Restaurer la configuration de AD FS sur SQL

```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\Users\administrator\testExport\" -DecryptionPassword "password" -DBConnectionString "Data Source=TESTMACHINE\SQLEXPRESS; Integrated Security=True"
```

### <a name="restores-the-ad-fs-configuration-with-the-specified-gmsa"></a>Restaure la configuration de AD FS avec le GMSA spécifié.

```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\Users\administrator\testExport\" -DecryptionPassword "password" -GroupServiceAccountIdentifier "mangupd1\adfsgmsa$"
```
### <a name="restore-the-ad-fs-configuration-with-the-specified-service-account-creds"></a>Restaurer la configuration de AD FS avec les références de compte de service spécifiées

```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\Users\administrator\testExport\" -DecryptionPassword "password" -ServiceAccountCredential $cred
```

## <a name="encryption-information"></a>Informations de chiffrement
Toutes les données de sauvegarde sont chiffrées avant d’être poussées dans le Cloud ou stockées dans le système de fichiers.  

Chaque document créé dans le cadre de la sauvegarde est chiffré à l’aide de la norme AES-256. Le mot de passe passé dans l’outil est utilisé comme phrase secrète pour générer un nouveau mot de passe à l’aide de la classe Rfc2898DeriveBytes. 

RngCryptoServiceProvider est utilisé pour générer le Salt utilisé par AES et la classe Rfc2898DeriveBytes. 

## <a name="log-files"></a>Fichiers journaux
À chaque fois qu’une sauvegarde ou une restauration est effectuée, un fichier journal est créé. Celles-ci se trouvent à l’emplacement suivant :

- **%localappdata%\ADFSRapidRecreationTool**

>[!NOTE]
> Lorsque vous effectuez une restauration, un fichier de PostRestore_Instructions peut être créé, contenant une vue d’ensemble des fournisseurs d’authentification supplémentaires, des magasins d’attributs et des approbations de fournisseur de revendications locales à installer manuellement avant de démarrer le service AD FS.

## <a name="version-release-history"></a>Historique de publication des versions

### <a name="version-10820"></a>Version 1.0.82.0
Publication : juillet 2019

**Problèmes résolus :**
- Résolution de bogue pour les noms de compte de service AD FS qui contiennent des caractères d’échappement LDAP


### <a name="version-10810"></a>Version : 1.0.81.0
Publication : avril 2019

**Problèmes résolus :**


- Correctifs de bogues pour la sauvegarde et la restauration des certificats
- Informations de traçage supplémentaires dans le fichier journal


### <a name="version-10750"></a>Version : 1.0.75.0
Publication : août 2018

**Problèmes résolus :**
* Mettez à jour Backup-ADFS lors de l’utilisation du commutateur-BackupDKM.  L’outil déterminera si le contexte actuel a accès au conteneur DKM.  Dans ce cas, il ne nécessite pas de privilèges d’administrateur de domaine ou d’informations d’identification de compte de service.  Cela permet d’effectuer des sauvegardes automatisées sans fournir de manière explicite les informations d’identification ou s’exécuter en tant que compte d’administrateur de domaine.

### <a name="version-10730"></a>Version : 1.0.73.0
Publication : août 2018

**Problèmes résolus :**
* Mettre à jour les algorithmes de chiffrement pour que l’application soit conforme aux normes FIPS
    
    >[!NOTE]
    > Les anciennes sauvegardes ne fonctionneront pas avec la nouvelle version en raison des modifications apportées aux algorithmes de chiffrement conformément à la conformité FIPS
    
* Ajouter la prise en charge des clusters SQL qui utilisent la réplication de fusion

### <a name="version-10720"></a>Version : 1.0.72.0
Publication : juillet 2018

**Problèmes résolus :**

   - Correctif de bogue : correction du. MSI installer pour prendre en charge les mises à niveau sur place 

### <a name="10180"></a>1.0.18.0
Publication : juillet 2018

**Problèmes résolus :**

   - Résolution de bogue : gérer les mots de passe de compte de service qui contiennent des caractères spéciaux (par ex., « & »)
   - Résolution de bogue : la restauration échoue car Microsoft. IdentityServer. ServiceHost. exe. config est utilisé par un autre processus


### <a name="1000"></a>1.0.0.0
Publication : 2016 octobre

Version initiale de AD FS outil de restauration rapide
