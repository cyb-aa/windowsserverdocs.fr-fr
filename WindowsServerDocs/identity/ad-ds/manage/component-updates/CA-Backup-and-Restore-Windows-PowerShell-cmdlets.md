---
ms.assetid: 7e195f5b-b194-40f3-a26d-5cf4ade5fc4d
title: "Applets de commande sauvegarde de l’autorité de certification et de restauration Windows PowerShell"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: aba86ed080cc0b4043805531f0a2138b1b1d3cf8
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="ca-backup-and-restore-windows-powershell-cmdlets"></a>Applets de commande sauvegarde de l’autorité de certification et de restauration Windows PowerShell

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012
>
**Auteur**: Justin Turner, ingénieur Support résolution Senior avec le groupe de Windows  
  
> [!NOTE]  
> Ce contenu est écrit par un ingénieur du support client Microsoft et est destiné aux administrateurs expérimentés et architectes système qui recherchent des explications techniques plus approfondies des fonctionnalités et des solutions Windows Server2012R2 que proposent généralement les rubriques sur TechNet. Toutefois, il n’a pas subi les mêmes passes, afin de la partie du langage peut sembler que moins finalisée que ce qui se trouvent généralement sur TechNet.  
  
## <a name="overview"></a>Vue d’ensemble  
Le module ADCSAdministration Windows PowerShell a été introduit dans la fenêtre Server2012.  Deux nouvelles applets de commande ont été ajoutées à ce module dans la fenêtre Server2012R2 pour prendre en charge la sauvegarde et la restauration d’une autorité de certification.  
  
-   Backup-CARoleService  
  
-   Restore-CARoleService  
  
## <a name="backup-caroleservice"></a>Backup-CARoleService  
**Table de Table SEQ \\\ * ARABIC 17: sauvegarder et restaurer des applets de commande Windows PowerShell**  
  
**Applet de commande ADCSAdministration: Backup-CARoleService**  
  
|Arguments - **gras** arguments sont requis|Description|  
|------------------------------------------------|---------------|  
|**-Chemin d’accès**|-String - emplacement dans lequel la sauvegarde<br />-C’est la seule sans nom de paramètre<br />-paramètre positionnel<br /><br />**Exemple:**<br /><br />Backup-CARoleService.-chemin d’accès c:\adcsbackup1<br /><br />Backup-CARoleService c:\adcsbackup2|  
|-KeyOnly|-Le certificat d’autorité de certification sans la base de données de sauvegarde<br /><br />**Exemple:**<br /><br />Backup-CARoleService c:\adcsbackup3 - KeyOnly|  
|-Mot de passe|-Spécifie le mot de passe pour protéger les clés privées et les certificats d’autorité de certification<br />-Doit être une chaîne sécurisée<br />-N’est pas valide avec le paramètre - DatabaseOnly<br /><br />Exemple:<br /><br />Backup-CARoleService c:\adcsbackup4-mot de passe (Read-Host-invite «mot de passe:» - AsSecureString)<br /><br />Backup-CARoleService c:\adcsbackup5-mot de passe (ConvertTo-SecureString «Pa55w0rd!» -AsPlainText - Force)|  
|-DatabaseOnly|-Sauvegarde la base de données sans le certificat d’autorité de certification<br /><br />Backup-CARoleService c:\adcsbackup6 - DatabaseOnly|  
|-Force|1. Vous permet de remplacer la sauvegarde qui existe déjà à l’emplacement spécifié dans le paramètre - Path<br /><br />Backup-CARoleService c:\adcsbackup1-Force|  
|-Incrémentielle|-Effectuer une sauvegarde incrémentielle<br /><br />Backup-CARoleService c:\adcsbackup7-incrémentielle|  
|-KeepLog|1. Indique à la commande pour conserver les fichiers journaux. Si le commutateur n’est pas spécifié, les fichiers journaux sont tronqués par défaut, sauf dans le scénario incrémentiels<br /><br />Backup-CARoleService c:\adcsbackup7 - KeepLog|  
  
### <a name="-password-secure-string"></a>-Mot de passe <Secure String>  
Si le - mot de passe est utilisé, le mot de passe fourni doit être une chaîne sécurisée.  Utilisez le **Read-Host** applet de commande pour lancer une invite interactive pour la saisie du mot de passe sécurisé, ou utiliser le **ConvertTo-SecureString** applet de commande pour spécifier le mot de passe en ligne.  
  
Passez en revue les exemples suivants  
  
**Spécifie une chaîne sécurisée pour le paramètre de mot de passe à l’aide de Read-Host**  
  
```powershell  
Backup-CARoleService c:\adcsbackup4 -Password (Read-Host -prompt "Password:" -AsSecureString)  
```  
  
**Spécifie une chaîne sécurisée pour le paramètre de mot de passe à l’aide de ConvertTo-SecureString**  
  
```powershell  
Backup-CARoleService c:\adcsbackup5 -Password (ConvertTo-SecureString "Pa55w0rd!" -AsPlainText -Force)  
```  
  
## <a name="restore-caroleservice"></a>Restore-CARoleService  
**Applet de commande ADCSAdministration: Restore-CARoleService**  
  
|Arguments - **gras** arguments sont requis|Description|  
|------------------------------------------------|---------------|  
|**-Chemin d’accès**|-String - emplacement pour restaurer la sauvegarde à partir de<br />-C’est la seule sans nom de paramètre<br />-paramètre positionnel<br /><br />**Exemple:**<br /><br />Restore-CARoleService. - chemin d’accès c:\adcsbackup1 - Force<br /><br />Restore-CARoleService c:\adcsbackup2-Force|  
|-KeyOnly|-Restaurer le certificat d’autorité de certification sans la base de données<br />-Doit être spécifié si la sauvegarde a été effectuée avec l’option - KeyOnly<br /><br />**Exemple:**<br /><br />Restore-CARoleService c:\adcsbackup3 - KeyOnly-Force|  
|-Mot de passe|-Spécifie le mot de passe des certificats d’autorité de certification et des clés privées<br />-Doit être une chaîne sécurisée<br /><br />**Exemple:**<br /><br />Restore-CARoleService c:\adcsbackup4-mot de passe (en lecture-host - invite «mot de passe:» - AsSecureString)-Force<br /><br />Restore-CARoleService c:\adcsbackup5-mot de passe (ConvertTo-SecureString «Pa55w0rd!» -AsPlainText - Force)-Force|  
|-DatabaseOnly|-Restaurer la base de données sans le certificat d’autorité de certification<br /><br />Restore-CARoleService c:\adcsbackup6 - DatabaseOnly|  
|-Force|-Vous permet de remplacer les clés préexistants<br />-Paramètre est facultatif, mais lors de la restauration sur place, il est probablement requis<br /><br />Restore-CARoleService c:\adcsbackup1-Force|  
  
### <a name="issues"></a>Problèmes  
Un mot de passe non protégé est sauvegarde en cas d’échec de la fonction ConvertTo-SecureString lors de l’utilisation de la Backup-CARoleService avec le mot de passe paramètre-.  
  
![Sauvegarde de l’autorité de certification et la restauration](media/CA-Backup-and-Restore-Windows-PowerShell-cmdlets/GTR_ADDS_BackupCARole.gif)  
  
**Table de Table SEQ \\\ * ARABIC 18: les erreurs courantes**  
  
|Action|Erreur|Commentaire|  
|----------|---------|-----------|  
|**Restore-CARoleService C:\ADCSBackup**|Restore-CARoleService: le processus ne peut pas accéder au fichier, car il est utilisé par un autre processus. (Exception de HRESULT:<br /><br />0 x 80070020)|Arrêter le service Services de certificats ActiveDirectory avant d’exécuter l’applet de commande Restore-CARoleService|  
|**Restore-CARoleService C:\ADCSBackup**|Restore-CARoleService: le répertoire n’est pas vide. (Exception de HRESULT: 0x80070091)|Utilisez le paramètre - Force pour remplacer les clés préexistants|  
|**Backup-CARoleService C:\ADCSBackup-mot de passe (Read-Host-invite «mot de passe:» - AsSecureString) - DatabaseOnly**|Backup-CARoleService: jeu de paramètres ne peuvent pas être résolu à l’aide de paramètres nommés spécifié.|-Mot de passe que le paramètre est utilisé uniquement pour le mot de passe protéger les clés privées et est par conséquent non valide lorsque vous ne sauvegardez pas|  
|**Restore-CARoleService C:\ADCSBack15-mot de passe (Read-Host-invite «mot de passe:» - AsSecureString) - DatabaseOnly**|Restore-CARoleService: jeu de paramètres ne peuvent pas être résolu à l’aide de paramètres nommés spécifié.|-Mot de passe que le paramètre est utilisé uniquement pour le mot de passe protéger les clés privées et est par conséquent non valide lorsque vous ne restaurez pas|  
|**Restore-CARoleService C:\ADCSBack14-mot de passe (Read-Host-invite «mot de passe:» - AsSecureString)**|Restore-CARoleService: le système ne peut pas trouver le fichier spécifié. (Exception de HRESULT: 0 x 80070002)|Le chemin d’accès spécifié ne contient pas d’une sauvegarde de base de données valide.  Par exemple, le chemin d’accès n’est pas valide ou la sauvegarde a été effectuée avec l’option - KeysOnly?|  
  
## <a name="additional-resources"></a>Ressources supplémentaires  
[Guide de Migration de Services de certificats ActiveDirectory](https://technet.microsoft.com/library/ee126170(v=ws.10).aspx)  
  
[Sauvegarde d’une base de données de l’autorité de certification et la clé privée](https://technet.microsoft.com/library/ee126140(v=ws.10).aspx#BKMK_BackUpDB)  
  
[La restauration de la base de données de l’autorité de certification et la configuration sur le serveur de destination](https://technet.microsoft.com/library/ee126140(v=ws.10).aspx#BKMK_RestoreCA)  
  
## <a name="try-this-backup-the-ca-in-your-lab-using-windows-powershell"></a>Procédez comme suit: Sauvegarde de l’autorité de certification dans votre laboratoire à l’aide de Windows PowerShell  
  
1.  Utilisez les commandes dans cette leçon pour sauvegarder la base de données de l’autorité de certification et la clé privée sécurisé avec un mot de passe.  
  
2.  Retarder la restauration de l’autorité de certification pour l’instant.  
  


