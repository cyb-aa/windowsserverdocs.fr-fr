---
ms.assetid: 4deff06a-d0ef-4e5a-9701-5911ba667201
title: Outil de restauration rapide ADFS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/02/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 525ba403473a9de522d9ab30662adc868b17b88d
ms.sourcegitcommit: c02756b7f5c92bf5018e17192f6fffb4754b0f06
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67533508"
---
# <a name="ad-fs-rapid-restore-tool"></a>Outil de restauration rapide ADFS

## <a name="overview"></a>Vue d'ensemble
AD FS est rendue hautement disponible dès aujourd'hui en configurant une batterie de serveurs AD FS. Certaines organisations aimerions un moyen d’avoir un seul serveur de déploiement AD FS, éliminant le besoin de plusieurs serveurs AD FS et équilibrage de l’infrastructure, tout en conservant certaines assurance de service peut être restauré rapidement s’il existe un problème.
Le nouvel outil de la restauration rapide AD FS fournit un moyen de restaurer des données AD FS sans nécessiter une sauvegarde complète et une restauration du système d’exploitation ou de l’état du système. Vous pouvez utiliser le nouvel outil pour exporter la configuration AD FS vers Azure ou vers un emplacement sur site.  Ensuite, vous pouvez appliquer les données exportées vers une nouvelle installation d’AD FS, recréer ou duplication de l’environnement AD FS. 

## <a name="scenarios"></a>Scénarios
L’outil de la restauration rapide AD FS peut être utilisé dans les scénarios suivants :

1. Restaurer rapidement la fonctionnalité AD FS après un problème
    - Utilisez l’outil pour créer une installation de secours à froid d’AD FS qui peuvent être déployées rapidement à la place le serveur AD FS en ligne
2. Déployer des environnements de test et de production identiques
    - Utiliser l’outil pour créer rapidement une copie exacte de la production AD FS dans un environnement de test ou à déployer rapidement une configuration de test validé en production

## <a name="what-is-backed-up"></a>Éléments sauvegardés
L’outil de sauvegarde de la configuration AD FS suivante
    
- Base de données AD FS configuration (SQL ou WID)
- Fichier de configuration (situé dans le dossier d’AD FS)
- Généré automatiquement le jeton de signature et de déchiffrement des certificats et des clés privées (à partir du conteneur Active Directory DKM)
- Certificat SSL et tous inscrits en externe certificats (signature de jetons, déchiffrement de jeton et la communication de service) et les clés privées correspondantes (Remarque : les clés privées doivent être exportables et l’utilisateur qui exécute le script doit avoir les autorisations nécessaires pour y accéder)
- Une liste des fournisseurs d’authentification personnalisés, de magasins d’attributs et de fournisseur de revendications local approuve qui sont installés.

## <a name="how-to-use-the-tool"></a>Comment utiliser l’outil
Tout d’abord, [télécharger](https://go.microsoft.com/fwlink/?LinkId=825646) et installez le MSI à votre serveur AD FS.  

À partir d’une invite de PowerShell, exécutez la commande suivante :

```powershell
import-module 'C:\Program Files (x86)\ADFS Rapid Recreation Tool\ADFSRapidRecreationTool.dll'
```

>[!NOTE] 
>Si vous utilisez la base de données intégrée de Windows (WID), cet outil doit être exécuté sur le serveur AD FS principal.  Vous pouvez utiliser le `Get-AdfsSyncProperties` applet de commande PowerShell pour déterminer si le serveur que vous êtes sur le serveur principal.

### <a name="system-requirements"></a>Configuration requise

- Cet outil fonctionne pour AD FS dans Windows Server 2012 R2 et versions ultérieures. 
- Le framework .NET requis est au moins 4.0. 
- La restauration doit être effectuée sur un serveur AD FS de la même version que la sauvegarde et qui utilise le même compte Active Directory comme compte de service AD FS.

## <a name="create-a-backup"></a>Créer une sauvegarde
Pour créer une sauvegarde, utilisez l’applet de commande AD FS de la sauvegarde. Cette applet de commande sauvegarde de la configuration AD FS de base de données, les certificats SSL, etc. 

L’utilisateur doit être au moins un administrateur local pour exécuter cette applet de commande. Pour sauvegarder le conteneur Active Directory DKM (requis dans la configuration par défaut AD FS), l’utilisateur doit être administrateur de domaine, doit passer les informations d’identification du compte de service AD FS ou a accès au conteneur DKM.  Si vous utilisez un compte de service administré de groupe, l’utilisateur doit être administrateur de domaine ou disposer des autorisations au conteneur ; Vous ne pouvez pas fournir les informations d’identification de service administré de groupe. 

La sauvegarde est nommée selon le modèle « adfsBackup_ID_Date-temps ». Il contient le numéro de version, la date et l’heure où la sauvegarde a été effectuée.
L’applet de commande accepte les paramètres suivants :
    
Jeux de paramètres

![Outil de restauration rapide ADFS](media/AD-FS-Rapid-Restore-Tool/parameter1.png)

### <a name="detailed-description"></a>Description détaillée

- **BackupDKM** -sauvegarde le conteneur Active Directory DKM qui contient les clés d’AD FS dans la configuration par défaut (jeton généré automatiquement la signature et le déchiffrement des certificats). Il utilise un outil de AD 'ldifde' pour exporter le conteneur AD et tous ses sous-arborescences.

- -**StorageType &lt;chaîne&gt;**  -le type de stockage, l’utilisateur souhaite utiliser. « FileSystem » indique que l’utilisateur veut stocker dans un dossier local ou dans le réseau « Azure » indique l’utilisateur veut stocker dans le conteneur de stockage Azure lorsque l’utilisateur effectue la sauvegarde, ils sélectionnent l’emplacement de sauvegarde, le système de fichiers ou dans le cloud. Pour utiliser Azure, les informations d’identification de stockage Azure doit être passées à l’applet de commande. Les informations d’identification de stockage contient le nom du compte et la clé. En outre, un nom de conteneur doit également être transmis. Si le conteneur n’existe pas, il est créé lors de la sauvegarde. Pour le système de fichiers à utiliser un chemin d’accès de stockage doit être donné. Dans ce répertoire, un nouveau répertoire sera être créé pour chaque sauvegarde. Chaque répertoire créée contiendra les fichiers sauvegardés. 

- **EncryptionPassword &lt;chaîne&gt;**  -le mot de passe qui doit être utilisé pour chiffrer tous les fichiers sauvegardés avant de les stocker

- **AzureConnectionCredentials &lt;pscredential&gt;**  -le nom du compte et la clé du compte de stockage Azure

- **AzureStorageContainer &lt;chaîne&gt;**  -le conteneur de stockage où la sauvegarde sera stockée dans Azure

- **StoragePath &lt;chaîne&gt;**  -l’emplacement les sauvegardes seront stockées dans

- **ServiceAccountCredential &lt;pscredential&gt;**  -Spécifie le compte de service utilisé pour le Service AD FS en cours d’exécution. Ce paramètre est uniquement nécessaire si l’utilisateur souhaite sauvegarder la gestion distribuée de clés et n’est pas administrateur de domaine ou n’a pas accès au contenu du conteneur. 

- **BackupComment &lt;string []&gt;**  -une chaîne d’information sur la sauvegarde qui sera affichée lors de la restauration, similaire au concept de dénomination de point de contrôle Hyper-V. La valeur par défaut est une chaîne vide

 
## <a name="backup-examples"></a>Exemples de sauvegarde
Voici des exemples de sauvegarde pour l’utilisation de l’outil de restauration rapide AD FS.

### <a name="backup-the-ad-fs-configuration-with-the-dkm-to-the-file-system-and-has-access-to-the-dkm-container-contents-either-domain-admin-or-delegated"></a>La configuration AD FS, avec la gestion distribuée de clés, pour le système de fichiers de sauvegarde et a accès à du contenu du conteneur DKM (soit administrateur de domaine ou délégué)

```powershell
Backup-ADFS -StorageType "FileSystem" -StoragePath "C:\Users\administrator\testExport\" -EncryptionPassword "password" -BackupComment "Clean Install of ADFS (FS)" -BackupDKM
```
 
### <a name="backup-the-ad-fs-configuration-with-the-dkm-to-the-file-system-with-the-service-account-credential-running-as-local-admin"></a>Configuration FS de la sauvegarde Active Directory, avec la gestion distribuée de clés, dans le système de fichiers avec les informations d’identification compte service, en cours d’exécution en tant qu’administrateur local

```powershell
Backup-ADFS -StorageType "FileSystem" -StoragePath "C:\Users\administrator\testExport\" -EncryptionPassword "password" -BackupComment "Clean Install of ADFS (FS)" -BackupDKM -ServiceAccountCredential $cred
```

### <a name="backup-the-ad-fs-configuration-without-the-dkm-to-the-azure-storage-container"></a>Sauvegarder la configuration AD FS sans la gestion distribuée de clés dans le conteneur de stockage Azure.

```powershell
Backup-ADFS -StorageType "Azure" -AzureConnectionCredentials $cred -AzureStorageContainer "adfsbackups"  -EncryptionPassword "password" -BackupComment "Clean Install of ADFS"
```

### <a name="backup-the-ad-fs-configuration-without-the-dkm-to-the-file-system"></a>La configuration AD FS sans la gestion distribuée de clés pour le système de fichiers de sauvegarde

```powershell   
Backup-ADFS -StorageType "FileSystem" -StoragePath "C:\Users\administrator\testExport\" -EncryptionPassword "password" -BackupComment "Clean Install of ADFS (FS)"
```

## <a name="restore-from-backup"></a>Restaurer à partir de la sauvegarde
Pour appliquer une configuration créée à l’aide d’ADFS de sauvegarde vers une nouvelle installation d’AD FS, utilisez l’applet de commande Restore-ADFS.

Cette applet de commande crée une nouvelle batterie de serveurs AD FS à l’aide de l’applet de commande `Install-AdfsFarm` et restaure la configuration AD FS, base de données, les certificats, etc.  Si le rôle AD FS n’a pas été installé sur le serveur, l’applet de commande installez-le.  L’applet de commande vérifie l’emplacement de restauration pour les sauvegardes existantes et invite l’utilisateur à choisir une sauvegarde appropriée en fonction de la date/heure que celle-ci a été effectuée et tout commentaire sur la sauvegarde que l’utilisateur peut avoir associé à la sauvegarde. S’il existe plusieurs configurations AD FS avec des noms de service de fédération, l’utilisateur est invité à choisir tout d’abord la configuration AD FS appropriée.
L’utilisateur doit être administrateur local et de domaine pour exécuter cette applet de commande.


>[!NOTE] 
>Avant d’utiliser l’outil de récupération rapide AD FS, assurez-vous que le serveur est joint au domaine avant de les restaurer la sauvegarde. 

L’applet de commande accepte les paramètres suivants : 

![Outil de restauration rapide ADFS](media/AD-FS-Rapid-Restore-Tool/parameter2.png)

### <a name="detailed-description"></a>Description détaillée

- **StorageType &lt;chaîne&gt;**  -le type de stockage, l’utilisateur souhaite utiliser.
 « FileSystem » indique que l’utilisateur veut stocker dans un dossier local ou dans le réseau « Azure » indique que l’utilisateur souhaite stockez-le dans le conteneur de stockage Azure

- **DecryptionPassword &lt;chaîne&gt;**  -le mot de passe qui a été utilisé pour chiffrer tous les fichiers sauvegardés 

- **AzureConnectionCredentials &lt;pscredential&gt;**  -le nom du compte et la clé du compte de stockage Azure

- **AzureStorageContainer &lt;chaîne&gt;**  -le conteneur de stockage où la sauvegarde sera stockée dans Azure

- **StoragePath &lt;chaîne&gt;**  -l’emplacement les sauvegardes seront stockées dans

- **ADFSName &lt; chaîne &gt;**  -le nom de la fédération qui a été sauvegardé et va être restauré. Si cela n’est pas fourni et il est nom de service de fédération qu’un seul, ceci sera utilisé. S’il existe plusieurs services de fédération sauvegardé à l’emplacement, puis l’utilisateur est invité à choisir un des Services de fédération de sauvegardées.

- **ServiceAccountCredential &lt; pscredential &gt;**  -Spécifie le compte de service qui sera utilisé pour le nouveau Service AD FS en cours de restauration 

- **GroupServiceAccountIdentifier &lt;chaîne&gt;**  -le compte GMSA que l’utilisateur souhaite utiliser pour le nouveau Service AD FS en cours de restauration. Par défaut, si aucun n’est fourni puis sauvegardé de nom de compte est utilisé s’il s’agissait de service administré de groupe, sinon l’utilisateur est invité à placer dans un compte de service

- **DBConnectionString &lt;chaîne&gt;**  : si l’utilisateur souhaite utiliser une autre base de données pour la restauration, ils doivent passer la chaîne de connexion SQL ou un type WID pour WID.

- **Force &lt;bool&gt;**  -ignorer les invites de l’outil peut avoir une fois que la sauvegarde est choisie.

- **RestoreDKM &lt;bool&gt;**  : restaurez le conteneur de gestion distribuée de clés pour la publicité, doit être définie si accédant à une publicité et la gestion distribuée de clés a été sauvegardée initialement.

## <a name="restore-examples"></a>Restore, exemples

### <a name="restore-the-ad-fs-configuration-without-the-dkm-from-the-azure-storage-container"></a>Restaurer la configuration AD FS sans la gestion distribuée de clés à partir du conteneur de stockage Azure

```powershell
Restore-ADFS -StorageType "Azure" -AzureConnectionCredential $cred -DecryptionPassword "password" -AzureStorageContainer "adfsbackups"
```

### <a name="restore-the-ad-fs-configuration-without-the-dkm-from-the-file-system"></a>Restaurer la configuration AD FS sans la gestion distribuée de clés du système de fichiers
 
```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\uSERS\administrator\testExport\" -DecryptionPassword "password"
```

### <a name="restore-the-ad-fs-configuration-with-the-dkm-to-the-file-system"></a>Restaurer la configuration AD FS avec la gestion distribuée de clés sur le système de fichiers 
 
```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\uSERS\administrator\testExport\" -DecryptionPassword "password" -RestoreDKM
```

### <a name="restore-the-ad-fs-configuration-to-wid"></a>Restaurer la Configuration AD FS à WID

```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\uSERS\administrator\testExport\" -DecryptionPassword "password" -DBConnectionString "WID"
``` 

### <a name="restore-the-ad-fs-configuration-to-sql"></a>Restaurer la Configuration AD FS to SQL

```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\uSERS\administrator\testExport\" -DecryptionPassword "password" -DBConnectionString "Data Source=TESTMACHINE\SQLEXPRESS; Integrated Security=True"
```

### <a name="restores-the-ad-fs-configuration-with-the-specified-gmsa"></a>Restaure la Configuration AD FS avec le compte GMSA spécifié

```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\uSERS\administrator\testExport\" -DecryptionPassword "password" -GroupServiceAccountIdentifier "mangupd1\adfsgmsa$"
```
### <a name="restore-the-ad-fs-configuration-with-the-specified-service-account-creds"></a>Restaurer la Configuration AD FS avec les informations d’identification du compte de service spécifié

```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\uSERS\administrator\testExport\" -DecryptionPassword "password" -ServiceAccountCredential $cred
```

## <a name="encryption-information"></a>Informations de chiffrement
Toutes les données de sauvegarde sont chiffrées avant de placer dans le cloud ou de les stocker dans le système de fichiers.  

Chaque document est créé dans le cadre de la sauvegarde est chiffré à l’aide d’AES-256. Le mot de passe transmis dans l’outil est utilisé comme une phrase secrète pour générer un nouveau mot de passe à l’aide de la classe Rfc2898DeriveBytes. 

RngCryptoServiceProvider est utilisé pour générer le salt utilisé par AES et la classe Rfc2898DeriveBytes. 

## <a name="log-files"></a>Fichiers journaux
Chaque fois qu’une sauvegarde ou une restauration est exécutée, un fichier journal est créé. Vous la trouverez à l’emplacement suivant :

- **%localappdata%\ADFSRapidRecreationTool**

>[!NOTE]
> Lorsque effectuez une restauration que peut créer un fichier PostRestore_Instructions contenant une vue d’ensemble des fournisseurs d’authentification supplémentaires, attribut stocke et approbations de fournisseur de revendications local doit être installé manuellement avant de démarrer le service AD FS.

## <a name="version-release-history"></a>Historique des versions

### <a name="version-10820"></a>Version 1.0.82.0
Mise en production : Juillet 2019

**Résolution des problèmes :**
- Les noms de compte qui contiennent des caractères d’échappement LDAP de service de résolution de bogue pour AD FS


### <a name="version-10810"></a>Version : 1.0.81.0
Mise en production : Avril 2019

**Résolution des problèmes :**


- Correctifs de bogues pour la sauvegarde de certificat et de restauration
- Informations de suivi supplémentaires dans le fichier journal


### <a name="version-10750"></a>Version : 1.0.75.0
Mise en production : Août 2018

**Résolution des problèmes :**
* Lorsque vous utilisez le commutateur - BackupDKM, mettez à jour AD FS de la sauvegarde.  L’outil déterminera si le contexte actuel a accès au conteneur DKM.  Dans ce cas, elle ne nécessite pas de privilèges d’administrateur de domaine ou informations d’identification du compte de service.  Cela permet des sauvegardes automatisées se produire sans explicitement en fournissant des informations d’identification ou en cours d’exécution en tant qu’un compte d’administrateur de domaine.

### <a name="version-10730"></a>Version : 1.0.73.0
Mise en production : Août 2018

**Résolution des problèmes :**
* Mettre à jour les algorithmes de chiffrement afin que l’application est conforme aux normes FIPS
    
    >[!NOTE]
    > Les anciennes sauvegardes ne fonctionnent pas avec la nouvelle version en raison de modifications dans les algorithmes de chiffrement en fonction de la conformité aux normes FIPS
    
* Ajouter la prise en charge pour les clusters SQL qui utilisent la réplication de fusion

### <a name="version-10720"></a>Version : 1.0.72.0
Mise en production : Juillet 2018

**Résolution des problèmes :**

   - Correctif de bogue : Fixe le. Programme d’installation MSI pour prendre en charge les mises à niveau sur place 

### <a name="10180"></a>1.0.18.0
Mise en production : Juillet 2018

**Résolution des problèmes :**

   - Correctif de bogue : gérer les mots de passe qui contiennent des caractères spéciaux (Internet Explorer, « & »)
   - Correctif de bogue : restauration échoue car Microsoft.IdentityServer.Servicehost.exe.config est utilisé par un autre processus


### <a name="1000"></a>1.0.0.0
Date de publication : Octobre 2016

Version initiale de l’outil de restauration rapide AD FS
