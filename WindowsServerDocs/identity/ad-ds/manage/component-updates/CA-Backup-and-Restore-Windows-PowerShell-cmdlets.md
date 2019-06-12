---
ms.assetid: 7e195f5b-b194-40f3-a26d-5cf4ade5fc4d
title: Applets de commande sauvegarde de l’autorité de certification et de restauration Windows PowerShell
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 6055d9b694f72a6a874acdcb5135fde61bcf0d76
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66442750"
---
# <a name="ca-backup-and-restore-windows-powershell-cmdlets"></a>Applets de commande sauvegarde de l’autorité de certification et de restauration Windows PowerShell

> S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012
> 
> **Auteur**: Justin Turner, ingénieur Support résolution Senior auprès du groupe Windows  
> 
> [!NOTE]
> Ce contenu est écrit par un ingénieur du support client Microsoft et est destiné aux administrateurs expérimentés et aux architectes système qui recherchent des explications techniques plus approfondies des fonctionnalités et des solutions Windows Server 2012 R2 que n'en proposent généralement les rubriques de TechNet. Toutefois, il n'a pas subi les mêmes passes de correction. De ce fait, une partie du langage peut sembler moins finalisée que le contenu de TechNet.  
  
## <a name="overview"></a>Vue d'ensemble  
Le module ADCSAdministration Windows PowerShell a été introduit dans Windows Server 2012.  Deux nouvelles applets de commande ont été ajoutées à ce module dans Windows Server 2012 R2 pour prendre en charge la sauvegarde et la restauration d’une autorité de certification.  
  
-   Backup-CARoleService  
  
-   Restore-CARoleService  
  
## <a name="backup-caroleservice"></a>Backup-CARoleService  
**Table SEQ Table \\ \* arabe 17 : Sauvegarde et les applets de commande de restauration Windows PowerShell**  
  
**Applet de commande ADCSAdministration : Backup-CARoleService**  
  
|Arguments - **gras** arguments sont requis|Description|  
|------------------------------------------------|---------------|  
|**-Path**|-Chaîne - emplacement d’enregistrement de la sauvegarde<br />-C’est le seul paramètre sans nom<br />-paramètre positionnel<br /><br />**Exemple :**<br /><br />Backup-CARoleService.-chemin d’accès c:\adcsbackup1<br /><br />Backup-CARoleService c:\adcsbackup2|  
|-KeyOnly|-Le certificat d’autorité de certification sans la base de données de sauvegarde<br /><br />**Exemple :**<br /><br />Backup-CARoleService c:\adcsbackup3 -KeyOnly|  
|-Mot de passe|-Spécifie le mot de passe pour protéger les certificats d’autorité de certification et les clés privées<br />-Doit être une chaîne sécurisée<br />-Non valide avec le paramètre - DatabaseOnly<br /><br />Exemple :<br /><br />Backup-CARoleService c:\adcsbackup4 -Password (Read-Host -prompt "Password:" -AsSecureString)<br /><br />Backup-CARoleService c:\adcsbackup5-mot de passe (ConvertTo-SecureString « Pa55w0rd ! » -AsPlainText -Force)|  
|-DatabaseOnly|-Sauvegarde la base de données sans le certificat d’autorité de certification<br /><br />Backup-CARoleService c:\adcsbackup6 -DatabaseOnly|  
|-Force|1.  Vous pouvez remplacer la sauvegarde existe déjà dans l’emplacement spécifié dans le paramètre - Path<br /><br />Backup-CARoleService c:\adcsbackup1 -Force|  
|-Incrémentiel|-Effectuer une sauvegarde incrémentielle<br /><br />Backup-CARoleService c:\adcsbackup7 -Incremental|  
|-KeepLog|1.  Indique à la commande pour conserver les fichiers journaux. Si le commutateur n’est pas spécifié, les fichiers journaux sont tronqués par défaut, sauf dans le scénario incrémentiels<br /><br />Backup-CARoleService c:\adcsbackup7 -KeepLog|  
  
### <a name="-password-secure-string"></a>-Password <Secure String>  
Si le - mot de passe est utilisé, le mot de passe fourni doit être une chaîne sécurisée.  Utiliser le **Read-Host** applet de commande pour lancer une invite interactive pour l’entrée de mot de passe sécurisé, ou utilisez le **ConvertTo-SecureString** applet de commande pour spécifier la mot de passe en ligne.  
  
Passez en revue les exemples suivants  
  
**En spécifiant une chaîne sécurisée pour le paramètre de mot de passe à l’aide de Read-Host**  
  
```powershell  
Backup-CARoleService c:\adcsbackup4 -Password (Read-Host -prompt "Password:" -AsSecureString)  
```  
  
**En spécifiant une chaîne sécurisée pour le paramètre de mot de passe à l’aide de ConvertTo-SecureString**  
  
```powershell  
Backup-CARoleService c:\adcsbackup5 -Password (ConvertTo-SecureString "Pa55w0rd!" -AsPlainText -Force)  
```  
  
## <a name="restore-caroleservice"></a>Restore-CARoleService  
**Applet de commande ADCSAdministration : Restore-CARoleService**  
  
|Arguments - **gras** arguments sont requis|Description|  
|------------------------------------------------|---------------|  
|**-Path**|-Chaîne - emplacement de restauration de sauvegarde à partir de<br />-C’est le seul paramètre sans nom<br />-paramètre positionnel<br /><br />**Exemple :**<br /><br />Restore-CARoleService.-chemin d’accès c:\adcsbackup1-Force<br /><br />Restore-CARoleService c:\adcsbackup2 -Force|  
|-KeyOnly|-Restaurer le certificat d’autorité de certification sans la base de données<br />-Doit être spécifié si la sauvegarde a été effectuée avec l’option - KeyOnly<br /><br />**Exemple :**<br /><br />Restore-CARoleService c:\adcsbackup3 -KeyOnly -Force|  
|-Mot de passe|-Spécifie le mot de passe de l’autorité de certification et les clés privées<br />-Doit être une chaîne sécurisée<br /><br />**Exemple :**<br /><br />Restore-CARoleService c:\adcsbackup4 -Password (read-host -prompt "Password:" -AsSecureString) -Force<br /><br />Restore-CARoleService c:\adcsbackup5-mot de passe (ConvertTo-SecureString « Pa55w0rd ! » -AsPlainText - Force)-Force|  
|-DatabaseOnly|-Restaurer la base de données sans le certificat d’autorité de certification<br /><br />Restore-CARoleService c:\adcsbackup6 -DatabaseOnly|  
|-Force|-Vous permet de remplacer les clés préexistants<br />-Est un paramètre facultatif, mais lors de la restauration sur place, il est probablement nécessaire<br /><br />Restore-CARoleService c:\adcsbackup1 -Force|  
  
### <a name="issues"></a>Problèmes  
Un mot de passe non protégé sauvegarde si la fonction de ConvertTo-SecureString échoue lors de l’utilisation de la sauvegarde-CARoleService avec-paramètre de mot de passe.  
  
![Restauration et sauvegarde d’autorité de certification](media/CA-Backup-and-Restore-Windows-PowerShell-cmdlets/GTR_ADDS_BackupCARole.gif)  
  
**Table SEQ Table \\ \* arabe 18 : Erreurs courantes**  
  
|Action|Erreur|Commentaire|  
|----------|---------|-----------|  
|**Restore-CARoleService C:\ADCSBackup**|Restore-CARoleService : Le processus ne peut pas accéder au fichier, car il est utilisé par un autre processus. (Exception de HRESULT :<br /><br />0x80070020)|Arrêtez le service Services de certificats Active Directory avant d’exécuter l’applet de commande Restore-CARoleService|  
|**Restore-CARoleService C:\ADCSBackup**|Restore-CARoleService : Le répertoire n’est pas vide. (Exception de HRESULT : 0x80070091)|Utilisez le paramètre - Force pour remplacer des clés préexistantes|  
|**Backup-CARoleService C:\ADCSBackup -Password (Read-Host -Prompt "Password:" -AsSecureString) -DatabaseOnly**|Backup-CARoleService : Jeu de paramètres ne peuvent pas être résolu en utilisant le texte spécifié des paramètres nommés.|-Mot de passe le paramètre est utilisé uniquement pour le mot de passe protéger les clés privées et est donc non valide lorsque vous ne sauvegardez pas les|  
|**Restore-CARoleService C:\ADCSBack15 -Password (Read-Host -Prompt "Password:" -AsSecureString) -DatabaseOnly**|Restore-CARoleService : Jeu de paramètres ne peuvent pas être résolu en utilisant le texte spécifié des paramètres nommés.|-Mot de passe le paramètre est utilisé uniquement pour le mot de passe protéger les clés privées et est donc non valide lorsque vous ne restaurez pas les|  
|**Restore-CARoleService C:\ADCSBack14 -Password (Read-Host -Prompt "Password:" -AsSecureString)**|Restore-CARoleService : Le fichier spécifié est introuvable. (Exception de HRESULT : 0x80070002)|Le chemin d’accès spécifié ne contient pas d’une sauvegarde de base de données valide.  Par exemple, le chemin d’accès n’est pas valide ou la sauvegarde a été effectuée avec l’option - KeysOnly ?|  
  
## <a name="additional-resources"></a>Ressources complémentaires  
[Guide de Migration de Services de certificats Active Directory](https://technet.microsoft.com/library/ee126170(v=ws.10).aspx)  
  
[Sauvegarde de la base de données et de la clé privée d'une autorité de certification](https://technet.microsoft.com/library/ee126140(v=ws.10).aspx#BKMK_BackUpDB)  
  
[Restauration de la base de données et de la configuration de l'autorité de certification sur le serveur de destination](https://technet.microsoft.com/library/ee126140(v=ws.10).aspx#BKMK_RestoreCA)  
  
## <a name="try-this-backup-the-ca-in-your-lab-using-windows-powershell"></a>Essayez ceci : Sauvegarde de l’autorité de certification dans votre laboratoire à l’aide de Windows PowerShell  
  
1.  Utilisez les commandes dans cette leçon pour sauvegarder la base de données de l’autorité de certification et sécurisé avec un mot de passe de clé privée.  
  
2.  Différez la restauration de l’autorité de certification pour l’instant.  
  


