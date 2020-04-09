---
ms.assetid: 7e195f5b-b194-40f3-a26d-5cf4ade5fc4d
title: Sauvegarder et restaurer les applets de commande Windows PowerShell de l’autorité de certification
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: d1dd406780dc61e1ce52d423ca6148d2a9dd2c3d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80823082"
---
# <a name="ca-backup-and-restore-windows-powershell-cmdlets"></a>Sauvegarder et restaurer les applets de commande Windows PowerShell de l’autorité de certification

> S’applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012
> 
> **Auteur**: Justin Turner, ingénieur du support technique senior avec le groupe Windows  
> 
> [!NOTE]
> Ce contenu est écrit par un ingénieur du support client Microsoft et est destiné aux administrateurs expérimentés et aux architectes système qui recherchent des explications techniques plus approfondies des fonctionnalités et des solutions Windows Server 2012 R2 que n'en proposent généralement les rubriques de TechNet. Toutefois, il n'a pas subi les mêmes passes de correction. De ce fait, une partie du langage peut sembler moins finalisée que le contenu de TechNet.  
  
## <a name="overview"></a>Overview  
Le module Windows PowerShell ADCSAdministration a été introduit dans Windows Server 2012.  Deux nouvelles applets de commande ont été ajoutées à ce module dans Windows Server 2012 R2 pour prendre en charge la sauvegarde et la restauration d’une autorité de certification.  
  
-   Backup-CARoleService  
  
-   Restore-CARoleService  
  
## <a name="backup-caroleservice"></a>Backup-CARoleService  
**Table SEQ Table \\\* arabe 17 : sauvegarde et restauration des applets de commande Windows PowerShell**  
  
**Applet de commande ADCSAdministration : Backup-CARoleService**  
  
|Arguments-les arguments en **gras** sont obligatoires|Description|  
|------------------------------------------------|---------------|  
|**-Chemin d’accès**|-String-location pour enregistrer la sauvegarde<br />-Il s’agit du seul paramètre sans nom<br />paramètre positionnel<p>**Exemple :**<p>Backup-CARoleService.-path c:\adcsbackup1<p>Backup-CARoleService c:\adcsbackup2|  
|-Uniquement|-Sauvegarder le certificat de l’autorité de certification sans la base de données<p>**Exemple :**<p>Backup-CARoleService c:\adcsbackup3-uniquement|  
|-Password|-Spécifie le mot de passe pour protéger les clés privées et les certificats d’autorité de certification<br />-Doit être une chaîne sécurisée<br />-Non valide avec le paramètre-DatabaseOnly<p>Exemple :<p>Backup-CARoleService c:\adcsbackup4-Password (lecture-hôte-invite "mot_de_passe :"-AsSecureString)<p>Backup-CARoleService c:\adcsbackup5-Password (ConvertTo-SecureString "Pa55w0rd !" -AsPlainText-force)|  
|-DatabaseOnly|-Sauvegarder la base de données sans le certificat d’autorité de certification<p>Backup-CARoleService c:\adcsbackup6-DatabaseOnly|  
|-Force|1. vous permet de remplacer la sauvegarde qui existe déjà à l’emplacement spécifié dans le paramètre-Path.<p>Backup-CARoleService c:\adcsbackup1-force|  
|-Incrémentiel|-Effectuer une sauvegarde incrémentielle<p>Backup-CARoleService c:\adcsbackup7-Incremental|  
|-KeepLog|1. indique à la commande de conserver les fichiers journaux. Si le commutateur n’est pas spécifié, les fichiers journaux sont tronqués par défaut, sauf dans le scénario incrémentiel.<p>Backup-CARoleService c:\adcsbackup7-KeepLog|  
  
### <a name="-password-secure-string"></a>-Password <Secure String>  
Si le paramètre-Password est utilisé, le mot de passe fourni doit être une chaîne sécurisée.  Utilisez l’applet de commande **Read-Host** pour lancer une invite interactive pour une entrée de mot de passe sécurisée, ou utilisez l’applet de commande **ConvertTo-SecureString** pour spécifier le mot de passe en ligne.  
  
Passez en revue les exemples suivants  
  
**Spécification d’une chaîne sécurisée pour le paramètre de mot de passe à l’aide de Read-Host**  
  
```powershell  
Backup-CARoleService c:\adcsbackup4 -Password (Read-Host -prompt "Password:" -AsSecureString)  
```  
  
**Spécification d’une chaîne sécurisée pour le paramètre de mot de passe à l’aide de ConvertTo-SecureString**  
  
```powershell  
Backup-CARoleService c:\adcsbackup5 -Password (ConvertTo-SecureString "Pa55w0rd!" -AsPlainText -Force)  
```  
  
## <a name="restore-caroleservice"></a>Restore-CARoleService  
**Applet de commande ADCSAdministration : Restore-CARoleService**  
  
|Arguments-les arguments en **gras** sont obligatoires|Description|  
|------------------------------------------------|---------------|  
|**-Chemin d’accès**|-String-emplacement à partir duquel restaurer la sauvegarde<br />-Il s’agit du seul paramètre sans nom<br />paramètre positionnel<p>**Exemple :**<p>Restore-CARoleService.-path c:\adcsbackup1-force<p>Restore-CARoleService c:\adcsbackup2-force|  
|-Uniquement|-Restaurer le certificat de l’autorité de certification sans la base de données<br />-Doit être spécifié si la sauvegarde a été effectuée avec l’option-keyonly<p>**Exemple :**<p>Restore-CARoleService c:\adcsbackup3-keyonly-force|  
|-Password|-Spécifie le mot de passe des certificats et des clés privées de l’autorité de certification<br />-Doit être une chaîne sécurisée<p>**Exemple :**<p>Restore-CARoleService c:\adcsbackup4-Password (lecture-hôte-invite "mot_de_passe :"-AsSecureString)-force<p>Restore-CARoleService c:\adcsbackup5-Password (ConvertTo-SecureString "Pa55w0rd !" -AsPlainText-force)-force|  
|-DatabaseOnly|-Restaurer la base de données sans le certificat d’autorité de certification<p>Restore-CARoleService c:\adcsbackup6-DatabaseOnly|  
|-Force|-Permet de remplacer les clés préexistantes<br />-Est un paramètre facultatif, mais lors de la restauration sur place, il est probablement nécessaire<p>Restore-CARoleService c:\adcsbackup1-force|  
  
### <a name="issues"></a>Problèmes  
Une sauvegarde non protégée par mot de passe est effectuée si la fonction ConvertTo-SecureString échoue lors de l’utilisation de Backup-CARoleService avec le paramètre-Password.  
  
![Sauvegarde et restauration de l’autorité de certification](media/CA-Backup-and-Restore-Windows-PowerShell-cmdlets/GTR_ADDS_BackupCARole.gif)  
  
**Table SEQ Table \\\* arabe 18 : erreurs courantes**  
  
|Action|Error|Commentaire|  
|----------|---------|-----------|  
|**Restore-CARoleService C:\ADCSBackup**|Restore-CARoleService : le processus ne peut pas accéder au fichier, car il est utilisé par un autre processus. (Exception de HRESULT :<p>0x80070020|Arrêtez le service Active Directory Certificate Services avant d’exécuter l’applet de commande Restore-CARoleService|  
|**Restore-CARoleService C:\ADCSBackup**|Restore-CARoleService : le répertoire n’est pas vide. (Exception de HRESULT : 0x80070091)|Utilisez le paramètre-force pour remplacer les clés préexistantes|  
|**Backup-CARoleService C:\ADCSBackup-Password (lecture-hôte-invite "mot_de_passe :"-AsSecureString)-DatabaseOnly**|Backup-CARoleService : le jeu de paramètres ne peut pas être résolu à l’aide des paramètres nommés spécifiés.|Le paramètre-Password est utilisé uniquement pour protéger les clés privées par mot de passe et n’est donc pas valide lorsque vous ne le sauvegardez pas.|  
|**Restore-CARoleService C:\ADCSBack15-Password (Read-Host-prompt "password :"-AsSecureString)-DatabaseOnly**|Restore-CARoleService : le jeu de paramètres ne peut pas être résolu à l’aide des paramètres nommés spécifiés.|Le paramètre-Password est utilisé uniquement pour protéger les clés privées par mot de passe et n’est donc pas valide lorsque vous ne les restaurez pas|  
|**Restore-CARoleService C:\ADCSBack14-Password (Read-Host-prompt "password :"-AsSecureString)**|Restore-CARoleService : le système ne trouve pas le fichier spécifié. (Exception de HRESULT : 0x80070002)|Le chemin d’accès spécifié ne contient pas de sauvegarde de base de données valide.  Le chemin d’accès n’est peut-être pas valide ou la sauvegarde a été effectuée avec l’option-KeysOnly ?|  
  
## <a name="additional-resources"></a>Ressources supplémentaires  
[Guide de migration des services de certificats Active Directory](https://technet.microsoft.com/library/ee126170(v=ws.10).aspx)  
  
[Sauvegarde de la base de données et de la clé privée d'une autorité de certification](https://technet.microsoft.com/library/ee126140(v=ws.10).aspx#BKMK_BackUpDB)  
  
[Restauration de la base de données et de la configuration de l'autorité de certification sur le serveur de destination](https://technet.microsoft.com/library/ee126140(v=ws.10).aspx#BKMK_RestoreCA)  
  
## <a name="try-this-backup-the-ca-in-your-lab-using-windows-powershell"></a>Essayez ceci : sauvegardez l’autorité de certification dans votre laboratoire à l’aide de Windows PowerShell  
  
1.  Utilisez les commandes de cette leçon pour sauvegarder la base de données de l’autorité de certification et la clé privée sécurisée à l’aide d’un mot de passe.  
  
2.  Maintenez la restauration de l’autorité de certification à l’heure actuelle.  
  


