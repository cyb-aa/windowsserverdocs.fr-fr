---
ms.assetid: ee008835-7d3b-4977-adcb-7084c40e5918
title: Deploy Implementing Retention of Information on File Servers (Demonstration Steps)
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 994eadfa205b62c5a512ab130c71fa6c22d1cff6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357544"
---
# <a name="deploy-implementing-retention-of-information-on-file-servers-demonstration-steps"></a>Deploy Implementing Retention of Information on File Servers (Demonstration Steps)

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vous pouvez définir des périodes de rétention pour les dossiers et placer des fichiers en mode de conservation légale à l'aide de l'Infrastructure de classification des fichiers et du Gestionnaire de ressources du serveur de fichiers.  
  
**Dans ce document**  
  
-   [Conditions préalables](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_Prereqs)  
  
-   [Étape 1 : Créer des définitions de propriétés de ressource @ no__t-0  
  
-   [Étape 2 : Configurer les notifications @ no__t-0  
  
-   [Étape 3 : Créer une tâche de gestion de fichiers @ no__t-0  
  
-   [Étape 4 : Classer un fichier manuellement @ no__t-0  
  
> [!NOTE]  
> Cette rubrique inclut des exemples d'applets de commande Windows PowerShell que vous pouvez utiliser pour automatiser certaines des procédures décrites. Pour plus d’informations, consultez [Utilisation des applets de commande](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="prerequisites"></a>Prérequis  
Les étapes décrites dans cette rubrique partent du principe que vous avez un serveur SMTP configuré pour les notifications d'expiration de fichier.  
  
## <a name="BKMK_Step1"></a>Étape 1 : créer des définitions de propriétés de ressource  
Lors de cette étape, nous allons activer les propriétés de ressource de période de rétention et de détectabilité pour que l'Infrastructure de classification des fichiers puisse les utiliser pour baliser les fichiers analysés dans un dossier réseau partagé.  
  
[Effectuez cette étape à l’aide de Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep1)  
  
#### <a name="to-create-resource-property-definitions"></a>Pour créer des définitions de propriétés de ressource  
  
1.  Sur le contrôleur de domaine, connectez-vous au serveur en tant que membre du groupe de sécurité Admins du domaine.  
  
2.  Ouvrez le Centre d'administration Active Directory. Dans le Gestionnaire de serveur, cliquez sur **Outils**, puis sur **Centre d'administration Active Directory**.  
  
3.  Développez **Contrôle d'accès dynamique**, puis cliquez sur **Propriétés de ressource**.  
  
4.  Cliquez avec le bouton droit sur **Période de rétention**, puis cliquez sur **Activer**.  
  
5.  Cliquez avec le bouton droit sur **Détectabilité**, puis cliquez sur **Activer**.  
  
@no__t-guides 0solution-](media/Deploy-Implementing-Retention-of-Information-on-File-Servers--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>commandes Windows PowerShell équivalentes</em>***  
  
L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.  
  
```  
Set-ADResourceProperty -Enabled:$true -Identity:'CN=RetentionPeriod_MS,CN=Resource Properties,CN=Claims Configuration,CN=Services,CN=Configuration,DC=contoso,DC=com'  
Set-ADResourceProperty -Enabled:$true -Identity:'CN=Discoverability_MS,CN=Resource Properties,CN=Claims Configuration,CN=Services,CN=Configuration,DC=contoso,DC=com'  
```  
  
## <a name="BKMK_Step2"></a>Étape 2 : configurer les notifications  
Lors de cette étape, nous allons utiliser la console Gestionnaire de ressources du serveur de fichiers pour configurer le serveur SMTP, l'adresse de messagerie de l'administrateur par défaut et l'adresse de messagerie par défaut depuis laquelle sont envoyés les rapports.  
  
[Effectuez cette étape à l’aide de Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep2)  
  
#### <a name="to-configure-notifications"></a>Pour configurer les notifications  
  
1.  Connectez-vous au serveur de fichiers en tant que membre du groupe de sécurité Administrateurs.  
  
2.  À l'invite de commandes Windows PowerShell, tapez **Update-FsrmClassificationPropertyDefinition**, puis appuyez sur Entrée. Cette commande permet de synchroniser les définitions de propriétés qui sont créées sur le contrôleur de domaine avec le serveur de fichiers.  
  
3.  Ouvrez le Gestionnaire de ressources du serveur de fichiers. Dans le Gestionnaire de serveur, cliquez sur **Outils**, puis sur **Gestionnaire de ressources du serveur de fichiers**.  
  
4.  Cliquez avec le bouton droit sur **Gestionnaire de ressources du serveur de fichiers (local)** , puis cliquez sur **Configurer les options**.  
  
5.  Sous l'onglet **Notifications par courrier électronique** , configurez ce qui suit :  
  
    -   Dans la zone **Nom ou adresse IP du serveur SMTP**, tapez le nom du serveur SMTP de votre réseau.  
  
    -   Dans la zone **Administrateurs destinataires par défaut**, tapez l'adresse de messagerie de l'administrateur qui doit recevoir la notification.  
  
    -   Dans la zone **adresse de messagerie par défaut** , tapez l’adresse de messagerie à utiliser pour envoyer les notifications.  
  
6.  Cliquez sur **OK**.  
  
@no__t-guides 0solution-](media/Deploy-Implementing-Retention-of-Information-on-File-Servers--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>commandes Windows PowerShell équivalentes</em>***  
  
L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.  
  
```  
Set-FsrmSetting -SmtpServer IP address of SMTP server -FromEmailAddress "FromEmailAddress" -AdminEmailAddress "AdministratorEmailAddress"  
```  
  
## <a name="BKMK_Step3"></a>Étape 3 : créer une tâche de gestion de fichiers  
Lors de cette étape, nous allons utiliser la console Gestionnaire de ressources du serveur de fichiers pour créer une tâche de gestion de fichiers qui s'exécutera le dernier jour du mois et provoquera l'expiration des fichiers qui remplissent les critères suivants :  
  
-   le fichier n'est pas classifié comme étant en mode de conservation légale ;  
  
-   le fichier est classifié comme ayant une période de rétention à long terme ;  
  
-   le fichier n'a pas été modifié au cours des 10 dernières années.  
  
[Effectuez cette étape à l’aide de Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep3)  
  
#### <a name="to-create-a-file-management-task"></a>Pour créer une tâche de gestion de fichiers  
  
1.  Connectez-vous au serveur de fichiers en tant que membre du groupe de sécurité Administrateurs.  
  
2.  Ouvrez le Gestionnaire de ressources du serveur de fichiers. Dans le Gestionnaire de serveur, cliquez sur **Outils**, puis sur **Gestionnaire de ressources du serveur de fichiers**.  
  
3.  Cliquez avec le bouton droit sur **Tâches de gestion de fichiers**, puis cliquez sur **Créer une tâche de gestion de fichiers**.  
  
4.  Sous l'onglet **Général**, dans la zone **Nom de la tâche**, tapez un nom pour la tâche de gestion de fichiers, par exemple Tâche de rétention.  
  
5.  Sous l'onglet **Étendue** , cliquez sur **Ajouter**et choisissez les dossiers qui doivent être inclus dans cette règle, tels que D:\Finance Documents.  
  
6.  Sous l'onglet **Action** , dans la zone **Type** , cliquez sur **Expiration de fichier**. Dans la zone **Répertoire d'expiration** , tapez le chemin d'accès à un dossier sur le serveur de fichiers local où seront déplacés les fichiers ayant expiré. Ce dossier doit avoir une liste de contrôle d'accès qui accorde un accès uniquement aux administrateurs du serveur de fichiers.  
  
7.  Sous l'onglet **Notification** , cliquez sur **Ajouter**.  
  
    -   Cochez la case **Envoyer un courrier électronique aux administrateurs suivants**.  
  
    -   Cochez la case **Envoyer un message électronique aux utilisateurs avec les fichiers affectés** , puis cliquez sur **OK**.  
  
8.  Sous l'onglet **Condition**, cliquez sur **Ajouter** et ajoutez les propriétés suivantes :  
  
    -   Dans la liste **Propriété**, cliquez sur **Détectabilité**. Dans la liste **Opérateur** , cliquez sur **Différente**. Dans la liste **Valeur**, cliquez sur **Attente**.  
  
    -   Dans la liste **Propriété** , cliquez sur **Période de rétention**. Dans la liste **Opérateur** , cliquez sur **Égal**. Dans la liste **Valeur**, cliquez sur **Long terme**.  
  
9. Sous l'onglet **Condition** , cochez la case **Nombre de jours depuis la dernière modification au fichier** , puis affectez la valeur **3650**.  
  
10. Sous l'onglet **Planification** , cliquez sur l'option **Tous les mois** , puis cochez la case **Dernier** .  
  
11. Cliquez sur **OK**.  
  
@no__t-guides 0solution-](media/Deploy-Implementing-Retention-of-Information-on-File-Servers--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>commandes Windows PowerShell équivalentes</em>***  
  
L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.  
  
```  
$fmjexpiration = New-FSRMFmjAction -Type 'Expiration' -ExpirationFolder folder  
$fmjNotificationAction = New-FsrmFmjNotificationAction -Type Email -MailTo "[FileOwner],[AdminEmail]"  
$fmjNotification = New-FsrmFMJNotification -Days 10 -Action @($fmjNotificationAction)  
$fmjCondition1 = New-FSRMFmjCondition -Property 'Discoverability_MS' -Condition 'NotEqual' -Value "Hold" 
$fmjCondition2 = New-FSRMFmjCondition -Property 'RetentionPeriod_MS' -Condition 'Equal' -Value "Long-term"  
$fmjCondition3 = New-FSRMFmjCondition -Property 'File.DateLastAccessed' -Condition 'Equal' -Value 3650  
$date = get-date  
$schedule = New-FsrmScheduledTask -Time $date -Monthly @(-1)    
$fmj1=New-FSRMFileManagementJob -Name "Retention Task" -Namespace @('D:\Finance Documents') -Action $fmjexpiration -Schedule $schedule -Notification @($fmjNotification) -Condition @( $fmjCondition1, $fmjCondition2, $fmjCondition3)  
```  
  
## <a name="BKMK_Step4"></a>Étape 4 : classifier un fichier manuellement  
Lors de cette étape, nous allons classifier manuellement un fichier en mode de conservation légale. Le dossier parent de ce fichier sera classifié avec une période de rétention à long terme.  
  
#### <a name="to-manually-classify-a-file"></a>Pour classifier manuellement un fichier  
  
1.  Connectez-vous au serveur de fichiers en tant que membre du groupe de sécurité Administrateurs.  
  
2.  Accédez au dossier qui a été configuré dans l'étendue de la tâche de gestion de fichiers à l'Étape 3.  
  
3.  Cliquez avec le bouton droit sur le dossier et cliquez sur **Propriétés**.  
  
4.  Sous l'onglet **Classification**, cliquez sur **Période de rétention**, sur **Long terme**, puis sur **OK**.  
  
5.  Cliquez avec le bouton droit sur un fichier dans ce dossier, puis cliquez sur **Propriétés**.  
  
6.  Sous l'onglet **Classification** , cliquez sur **Détectabilité**, sur **Attente**, sur **Appliquer**, puis sur **OK**.  
  
7.  Sur le serveur de fichiers, exécutez la tâche de gestion de fichiers à l'aide de la console Gestionnaire de ressources du serveur de fichiers. Une fois la tâche de gestion de fichiers terminée, vérifiez le dossier et assurez-vous que le fichier n'a pas été déplacé vers le répertoire d'expiration.  
  
8.  Cliquez avec le bouton droit sur le même fichier dans ce dossier, puis cliquez sur **Propriétés**.  
  
9. Sous l'onglet **Classification** , cliquez sur **Détectabilité**, sur **Non applicable**, sur **Appliquer**, puis sur **OK**.  
  
10. Sur le serveur de fichiers, réexécutez la tâche de gestion de fichiers à l'aide de la console Gestionnaire de ressources du serveur de fichiers. Une fois la tâche de gestion de fichiers terminée, vérifiez le dossier et assurez-vous que ce fichier a été déplacé vers le répertoire d'expiration.  
  
## <a name="BKMK_Links"></a>Voir aussi  
  
-   [Scénario : implémenter la conservation des informations sur les serveurs de fichiers](Scenario--Implement-Retention-of-Information-on-File-Servers.md)  
  
-   [Planifier la rétention des informations sur les serveurs de fichiers](assetId:///edf13190-7077-455a-ac01-f534064a9e0c)  
  
-   [Contrôle d’accès dynamique : Vue d’ensemble du scénario](Dynamic-Access-Control--Scenario-Overview.md)  
  

