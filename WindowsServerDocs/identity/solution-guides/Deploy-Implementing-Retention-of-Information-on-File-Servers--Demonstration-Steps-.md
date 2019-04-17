---
ms.assetid: ee008835-7d3b-4977-adcb-7084c40e5918
title: "Déployer l’implémentation de rétention des informations sur les serveurs de fichiers (étapes de démonstration)"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: e0f79dd72190888340144bc5c109ee31fa301937
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="deploy-implementing-retention-of-information-on-file-servers-demonstration-steps"></a>Déployer l’implémentation de rétention des informations sur les serveurs de fichiers (étapes de démonstration)

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Vous pouvez définir des périodes de rétention pour les dossiers et placer des fichiers légale à l’aide d’Infrastructure de Classification des fichiers et Gestionnaire de ressources du serveur de fichiers.  
  
**Dans ce document**  
  
-   [Conditions préalables](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_Prereqs)  
  
-   [Étape1: Créer des définitions de propriété de ressource](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_Step1)  
  
-   [Étape2: Configurer les notifications](Deploy-Implementing-Retention-of-Information-on-File-Servers--Demonstration-Steps-.md#BKMK_Step2)  
  
-   [Étape3: Créer une tâche de gestion de fichiers](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_Step3)  
  
-   [Étape4: Classifier un fichier manuellement](Deploy-Implementing-Retention-of-Information-on-File-Servers--Demonstration-Steps-.md#BKMK_Step4)  
  
> [!NOTE]  
> Cette rubrique inclut des applets de commande exemple Windows PowerShell que vous pouvez utiliser pour automatiser certaines des procédures décrites. Pour plus d’informations, voir [applets de commande à l’aide de](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="prerequisites"></a>Conditions préalables  
Les étapes décrites dans cette rubrique supposent que vous avez un serveur SMTP configuré pour les notifications d’expiration de fichier.  
  
## <a name="BKMK_Step1"></a>Étape1: Créer des définitions de propriété de ressource  
Dans cette étape, nous activer les propriétés de ressource de période de rétention et de détectabilité pour que l’Infrastructure de Classification des fichiers peut utiliser ces propriétés de ressource pour baliser les fichiers analysés dans un dossier réseau partagé.  
  
[Effectuez cette étape à l’aide de Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep1)  
  
#### <a name="to-create-resource-property-definitions"></a>Pour créer des définitions de propriétés de ressource  
  
1.  Sur le contrôleur de domaine, connectez-vous au serveur en tant que membre du groupe de sécurité Admins du domaine.  
  
2.  Ouvrir le centre d’administration ActiveDirectory. Dans le Gestionnaire de serveur, cliquez sur **outils**, puis cliquez sur **centre d’administration Active Directory**.  
  
3.  Développez **contrôle d’accès dynamique**, puis cliquez sur **propriétés de ressource**.  
  
4.  Avec le bouton droit **période de rétention**, puis cliquez sur **activer**.  
  
5.  Avec le bouton droit **détectabilité**, puis cliquez sur **activer**.  
  
![guides de solutions](media/Deploy-Implementing-Retention-of-Information-on-File-Servers--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell équivalentes commandes ***  
  
L’applet de commande Windows PowerShell ou les applets de commande suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.  
  
```  
Set-ADResourceProperty -Enabled:$true -Identity:'CN=RetentionPeriod_MS,CN=Resource Properties,CN=Claims Configuration,CN=Services,CN=Configuration,DC=contoso,DC=com'  
Set-ADResourceProperty -Enabled:$true -Identity:'CN=Discoverability_MS,CN=Resource Properties,CN=Claims Configuration,CN=Services,CN=Configuration,DC=contoso,DC=com'  
```  
  
## <a name="BKMK_Step2"></a>Étape2: Configurer les notifications  
Dans cette étape, nous utilisons la console du Gestionnaire de ressources de serveur de fichiers pour configurer le serveur SMTP et l’adresse de messagerie par défaut que les rapports sont envoyés à partir de l’adresse de messagerie d’administrateur par défaut.  
  
[Effectuez cette étape à l’aide de Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep2)  
  
#### <a name="to-configure-notifications"></a>Pour configurer les notifications  
  
1.  Connectez-vous au serveur de fichiers en tant que membre du groupe de sécurité Administrateurs.  
  
2.  À partir de l’invite de commandes Windows PowerShell, tapez **Update-FsrmClassificationPropertyDefinition**, puis appuyez sur ENTRÉE. Cette commande permet de synchroniser les définitions de propriétés qui sont créées sur le contrôleur de domaine pour le serveur de fichiers.  
  
3.  Ouvrez le Gestionnaire de ressources du serveur de fichiers. Dans le Gestionnaire de serveur, cliquez sur **outils**, puis cliquez sur **File Server Resource Manager**.  
  
4.  Avec le bouton droit **Gestionnaire de ressources du serveur de fichiers (local)**, puis cliquez sur **configurer les Options**.  
  
5.  Sur le **Notifications par courrier électronique** onglet, configurez les éléments suivants:  
  
    -   Dans le **nom du serveur SMTP ou l’adresse IP**, tapez le nom du serveur SMTP sur votre réseau.  
  
    -   Dans le **Administrateurs destinataires par défaut** zone, tapez l’adresse de messagerie de l’administrateur qui doit recevoir la notification.  
  
    -   Dans le **par défaut «De» de l’adresse de messagerie**, tapez l’adresse de messagerie qui doit être utilisé pour envoyer les notifications.  
  
6.  Cliquez sur **OK**.  
  
![guides de solutions](media/Deploy-Implementing-Retention-of-Information-on-File-Servers--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell équivalentes commandes ***  
  
L’applet de commande Windows PowerShell ou les applets de commande suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.  
  
```  
Set-FsrmSetting -SmtpServer IP address of SMTP server -FromEmailAddress "FromEmailAddress" -AdminEmailAddress "AdministratorEmailAddress"  
```  
  
## <a name="BKMK_Step3"></a>Étape3: Créer une tâche de gestion de fichiers  
Dans cette étape, nous utilisons la console du Gestionnaire de ressources de serveur de fichiers pour créer une tâche de gestion de fichiers qui exécutera le dernier jour du mois et faire expirer des fichiers avec les critères suivants:  
  
-   Le fichier n’est pas classifié comme étant légale.  
  
-   Le fichier est classifié comme ayant une période de rétention à long terme.  
  
-   Le fichier n’a pas été modifié au cours des 10dernières années.  
  
[Effectuez cette étape à l’aide de Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep3)  
  
#### <a name="to-create-a-file-management-task"></a>Pour créer une tâche de gestion de fichiers  
  
1.  Connectez-vous au serveur de fichiers en tant que membre du groupe de sécurité Administrateurs.  
  
2.  Ouvrez le Gestionnaire de ressources du serveur de fichiers. Dans le Gestionnaire de serveur, cliquez sur **outils**, puis cliquez sur **File Server Resource Manager**.  
  
3.  Avec le bouton droit **tâches de gestion de fichiers**, puis cliquez sur **créer une tâche de gestion de fichiers**.  
  
4.  Sur le **général** onglet le **nom de la tâche**, tapez un nom pour la tâche de gestion de fichiers, par exemple, la tâche de rétention.  
  
5.  Sur le **étendue**, cliquez sur **ajouter**et choisir les dossiers qui doivent être inclus dans cette règle, tels que D:\Finance Documents.  
  
6.  Sur le **Action** onglet le **Type**, cliquez sur **expiration de fichier**. Dans le **répertoire d’Expiration**, tapez un chemin d’accès à un dossier sur le serveur de fichiers local où les fichiers ayant expiré va être déplacées. Ce dossier doit posséder une liste de contrôle d’accès qui accorde l’accès uniquement les administrateurs de serveur de fichiers.  
  
7.  Sur le **Notification**, cliquez sur **ajouter**.  
  
    -   Sélectionnez le **envoyer un courrier électronique aux administrateurs suivants** case à cocher.  
  
    -   Sélectionnez le **envoyer un courrier électronique aux utilisateurs avec les fichiers affectés** case à cocher, puis cliquez sur **OK**.  
  
8.  Sur le **Condition**, cliquez sur **ajouter**et ajoutez les propriétés suivantes:  
  
    -   Dans le **propriété**, cliquez sur **détectabilité**. Dans le **opérateur**, cliquez sur **pas égal**. Dans le **valeur**, cliquez sur **longuement**.  
  
    -   Dans le **propriété**, cliquez sur **période de rétention**. Dans le **opérateur**, cliquez sur **égale**. Dans le **valeur**, cliquez sur **à long terme**.  
  
9. Sur le **Condition** onglet, sélectionnez le **nombre de jours depuis la dernière modification du fichier** case à cocher, puis définissez la valeur sur **3650**.  
  
10. Sur le **calendrier**, cliquez sur le **mensuel** option, puis sélectionnez le **dernière** case à cocher.  
  
11. Cliquez sur **OK**.  
  
![guides de solutions](media/Deploy-Implementing-Retention-of-Information-on-File-Servers--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell équivalentes commandes ***  
  
L’applet de commande Windows PowerShell ou les applets de commande suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.  
  
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
  
## <a name="BKMK_Step4"></a>Étape4: Classifier un fichier manuellement  
Dans cette étape, nous allons classifier manuellement un fichier pour être légale. Le dossier parent de ce fichier sera classifié avec une période de rétention à long terme.  
  
#### <a name="to-manually-classify-a-file"></a>Pour classifier manuellement un fichier  
  
1.  Connectez-vous au serveur de fichiers en tant que membre du groupe de sécurité Administrateurs.  
  
2.  Accédez au dossier qui a été configuré dans l’étendue de la tâche de gestion de fichiers créée à l’étape3.  
  
3.  Cliquez avec le bouton droit, puis cliquez sur **propriétés**.  
  
4.  Sur le **Classification**, cliquez sur **période de rétention**, cliquez sur **à long terme**, puis cliquez sur **OK**.  
  
5.  Cliquez sur un fichier dans ce dossier, puis cliquez sur **propriétés**.  
  
6.  Sur le **Classification**, cliquez sur **détectabilité**, cliquez sur **longuement**, cliquez sur **appliquer**, puis cliquez sur **OK**.  
  
7.  Sur le serveur de fichiers, exécutez la tâche de gestion de fichiers à l’aide de la console du Gestionnaire de ressources de serveur de fichiers. Une fois la tâche de gestion de fichiers terminée, vérifiez le dossier et vérifiez que le fichier n’a pas été déplacé vers le répertoire d’expiration.  
  
8.  Cliquez sur le même fichier dans ce dossier, puis cliquez sur **propriétés**.  
  
9. Sur le **Classification**, cliquez sur **détectabilité**, cliquez sur **non Applicable**, cliquez sur **appliquer**, puis cliquez sur **OK**.  
  
10. Sur le serveur de fichiers, réexécutez la tâche de gestion de fichiers à l’aide de la console du Gestionnaire de ressources de serveur de fichiers. Une fois la tâche de gestion de fichiers terminée, vérifiez le dossier et assurez-vous que le fichier a été déplacé vers le répertoire d’expiration.  
  
## <a name="BKMK_Links"></a>Voir aussi  
  
-   [Scénario: Implémenter la rétention des informations sur les serveurs de fichiers](Scenario--Implement-Retention-of-Information-on-File-Servers.md)  
  
-   [Planifier la rétention des informations sur les serveurs de fichiers](assetId:///edf13190-7077-455a-ac01-f534064a9e0c)  
  
-   [Contrôle d’accès dynamique: Vue d’ensemble du scénario](Dynamic-Access-Control--Scenario-Overview.md)  
  

