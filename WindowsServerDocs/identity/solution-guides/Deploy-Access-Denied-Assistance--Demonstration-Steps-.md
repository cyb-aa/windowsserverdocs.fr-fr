---
ms.assetid: b035e9f8-517f-432a-8dfb-40bfc215bee5
title: "Déployer l’Assistance accès refusé (procédure descriptive)"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 5201441ba884fe4658b917919e60c7d20530341b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="deploy-access-denied-assistance-demonstration-steps"></a>Déployer l’Assistance accès refusé (procédure descriptive)

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Cette rubrique explique comment configurer l’assistance en cas d’accès refusé et vérifier qu’elle fonctionne correctement.  
  
**Dans ce document**  
  
-   [Étape1: Configurer l’assistance en cas d’accès refusé](Deploy-Access-Denied-Assistance--Demonstration-Steps-.md#BKMK_1)  
  
-   [Étape2: Configurer les paramètres de notification par courrier électronique](Deploy-Access-Denied-Assistance--Demonstration-Steps-.md#BKMK_2)  
  
-   [Étape3: Vérifier qu’assistance en cas d’accès refusé est configurée correctement](Deploy-Access-Denied-Assistance--Demonstration-Steps-.md#BKMK_3)  
  
> [!NOTE]  
> Cette rubrique inclut des applets de commande exemple Windows PowerShell que vous pouvez utiliser pour automatiser certaines des procédures décrites. Pour plus d’informations, voir [applets de commande à l’aide de](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="BKMK_1"></a>Étape1: Configurer l’assistance en cas d’accès refusé  
Vous pouvez configurer l’assistance de refus d’accès au sein d’un domaine à l’aide de stratégie de groupe, ou vous pouvez configurer l’assistance individuellement sur chaque serveur de fichiers à l’aide de la console du Gestionnaire de ressources de serveur de fichiers. Vous pouvez également modifier le message d’accès refusé pour un dossier partagé spécifique sur un serveur de fichiers.  
  
Vous pouvez configurer l’accès refusé une assistance pour le domaine à l’aide de stratégie de groupe comme suit:  
  
[Effectuez cette étape à l’aide de Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep1)  
  
#### <a name="to-configure-access-denied-assistance-by-using-group-policy"></a>Pour configurer l’assistance en cas d’accès refusé à l’aide de stratégie de groupe  
  
1.  Gestion des stratégies de groupe Ouvrir. Dans le Gestionnaire de serveur, cliquez sur **outils**, puis cliquez sur **gestion des stratégies de groupe**.  
  
2.  Avec le bouton droit de la stratégie de groupe approprié, puis cliquez sur **modifier**.  
  
3.  Cliquez sur **Configuration ordinateur**, cliquez sur **stratégies**, cliquez sur **modèles d’administration**, cliquez sur **système**, puis cliquez sur **denied Assistance**.  
  
4.  Avec le bouton droit **personnaliser le message pour les erreurs d’accès refusé**, puis cliquez sur **modifier**.  
  
5.  Sélectionnez le **activé** option.  
  
6.  Configurez les options suivantes:  
  
    1.  Dans le **affiche le message suivant aux utilisateurs l’accès sont refusés**, tapez un message que les utilisateurs verront lorsqu’ils l’accès sont refusés à un fichier ou dossier.  
  
        Vous pouvez ajouter des macros au message qui insère du texte personnalisé. Les macros disponibles:  
  
        -   **[Original File Path] **Le chemin d’accès de fichier d’origine est accessible par l’utilisateur.  
  
        -   **[Original File Path Folder] **Du dossier parent du chemin de fichier d’origine qui a été accessibles par l’utilisateur.  
  
        -   **[Admin Email] **La liste de destinataires de courrier électronique administrateur.  
  
        -   **[Data Owner Email] **La liste de destinataires de courrier électronique propriétaire données.  
  
    2.  Sélectionnez le **permettre aux utilisateurs de demander une assistance** case à cocher.  
  
    3.  Conservez les paramètres par défaut restants.  
  
![guides de solutions](media/Deploy-Access-Denied-Assistance--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell équivalentes commandes ***  
  
L’applet de commande Windows PowerShell ou les applets de commande suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.  
  
```  
Set-GPRegistryValue -Name "Name of GPO" -key "HKLM\Software\Policies\Microsoft\Windows\ADR\AccessDenied" -ValueName AllowEmailRequests -Type DWORD -value 1  
Set-GPRegistryValue -Name "Name of GPO" -key "HKLM\Software\Policies\Microsoft\Windows\ADR\AccessDenied" -ValueName GenerateLog -Type DWORD -value 1  
Set-GPRegistryValue -Name "Name of GPO" -key "HKLM\Software\Policies\Microsoft\Windows\ADR\AccessDenied" -ValueName IncludeDeviceClaims -Type DWORD -value 1  
Set-GPRegistryValue -Name "Name of GPO" -key "HKLM\Software\Policies\Microsoft\Windows\ADR\AccessDenied" -ValueName IncludeUserClaims -Type DWORD -value 1  
Set-GPRegistryValue -Name "Name of GPO" -key "HKLM\Software\Policies\Microsoft\Windows\ADR\AccessDenied" -ValueName PutAdminOnTo -Type DWORD -value 1  
Set-GPRegistryValue -Name "Name of GPO" -key "HKLM\Software\Policies\Microsoft\Windows\ADR\AccessDenied" -ValueName PutDataOwnerOnTo -Type DWORD -value 1  
Set-GPRegistryValue -Name "Name of GPO" -key "HKLM\Software\Policies\Microsoft\Windows\ADR\AccessDenied" -ValueName ErrorMessage -Type MultiString -value "Type the text that the user will see in the error message dialog box."  
Set-GPRegistryValue -Name "Name of GPO" -key "HKLM\Software\Policies\Microsoft\Windows\ADR\AccessDenied" -ValueName Enabled -Type DWORD -value 1 
  
```  
  
Sinon, vous pouvez configurer assistance en cas d’accès refusé individuellement sur chaque serveur de fichiers à l’aide de la console du Gestionnaire de ressources de serveur de fichiers.  
  
[Effectuez cette étape à l’aide de Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep1a)  
  
#### <a name="to-configure-access-denied-assistance-by-using-file-server-resource-manager"></a>Pour configurer l’assistance en cas d’accès refusé à l’aide du Gestionnaire de ressources du serveur de fichiers  
  
1.  Ouvrez le Gestionnaire de ressources du serveur de fichiers. Dans le Gestionnaire de serveur, cliquez sur **outils**, puis cliquez sur **File Server Resource Manager**.  
  
2.  Avec le bouton droit **Gestionnaire de ressources du serveur de fichiers (Local)**, puis cliquez sur **configurer les Options**.  
  
3.  Cliquez sur le **denied Assistance** onglet.  
  
4.  Sélectionnez le **activer l’assistance en cas d’accès refusé** case à cocher.  
  
5.  Dans le **affiche le message suivant aux utilisateurs autorisés à accéder à un fichier ou dossier**, tapez un message que les utilisateurs verront lorsqu’ils l’accès sont refusés à un fichier ou dossier.  
  
    Vous pouvez ajouter des macros au message qui insère du texte personnalisé. Les macros disponibles:  
  
    -   **[Original File Path] **Le chemin d’accès de fichier d’origine est accessible par l’utilisateur.  
  
    -   **[Original File Path Folder] **Du dossier parent du chemin de fichier d’origine qui a été accessibles par l’utilisateur.  
  
    -   **[Admin Email] **La liste de destinataires de courrier électronique administrateur.  
  
    -   **[Data Owner Email] **La liste de destinataires de courrier électronique propriétaire données.  
  
6.  Cliquez sur **demandes par courrier électronique de configurer**, sélectionnez le **permettre aux utilisateurs de demander une assistance** case à cocher, puis cliquez sur **OK**.  
  
7.  Cliquez sur **Preview** si vous souhaitez afficher un aperçu du message d’erreur à l’utilisateur.  
  
8.  Cliquez sur **OK**.  
  
![guides de solutions](media/Deploy-Access-Denied-Assistance--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell équivalentes commandes ***  
  
L’applet de commande Windows PowerShell ou les applets de commande suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.
  
```  
Set-FSRMAdrSetting -Event "AccessDenied" -DisplayMessage "Type the text that the user will see in the error message dialog box." -Enabled:$true -AllowRequests:$true  
```  
  
Après avoir configuré l’assistance en cas d’accès refusé, vous devez l’activer pour tous les types de fichiers à l’aide de stratégie de groupe.  
  
[Effectuez cette étape à l’aide de Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep1c)  
  
#### <a name="to-configure-access-denied-assistance-for-all-file-types-by-using-group-policy"></a>Pour configurer l’assistance en cas d’accès refusé pour tous les types de fichiers à l’aide de stratégie de groupe  
  
1.  Gestion des stratégies de groupe Ouvrir. Dans le Gestionnaire de serveur, cliquez sur **outils**, puis cliquez sur **gestion des stratégies de groupe**.  
  
2.  Avec le bouton droit de la stratégie de groupe approprié, puis cliquez sur **modifier**.  
  
3.  Cliquez sur **Configuration ordinateur**, cliquez sur **stratégies**, cliquez sur **modèles d’administration**, cliquez sur **système**, puis cliquez sur **denied Assistance**.  
  
4.  Avec le bouton droit **Activer accès refusé l’assistance de client pour tous les types de fichiers**, puis cliquez sur **modifier**.  
  
5.  Cliquez sur **activé**, puis cliquez sur **OK**.  
  
![guides de solutions](media/Deploy-Access-Denied-Assistance--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell équivalentes commandes ***  
  
L’applet de commande Windows PowerShell ou les applets de commande suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme. 
  
```  
Set-GPRegistryValue -Name "Name of GPO" -key "HKLM\SOFTWARE\Policies\Microsoft\Windows\Explore" -ValueName EnableShellExecuteFileStreamCheck -Type DWORD -value 1  
  
```  
  
Vous pouvez également spécifier un message d’accès refusé distinct pour chaque dossier partagé sur un serveur de fichiers à l’aide de la console du Gestionnaire de ressources de serveur de fichiers.  
  
[Effectuez cette étape à l’aide de Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep1b)  
  
#### <a name="to-specify-a-separate-access-denied-message-for-a-shared-folder-by-using-file-server-resource-manager"></a>Pour spécifier un message d’accès refusé distinct pour un dossier partagé à l’aide du Gestionnaire de ressources du serveur de fichiers  
  
1.  Ouvrez le Gestionnaire de ressources du serveur de fichiers. Dans le Gestionnaire de serveur, cliquez sur **outils**, puis cliquez sur **File Server Resource Manager**.  
  
2.  Développez **Gestionnaire de ressources du serveur de fichiers (Local)**, puis cliquez sur **gestion de la Classification**.  
  
3.  Avec le bouton droit **propriétés de Classification**, puis cliquez sur **définir les propriétés de gestion des dossiers**.  
  
4.  Dans le **propriété**, cliquez sur **Message d’assistance en cas d’accès refusé**, puis cliquez sur **ajouter**.  
  
5.  Cliquez sur **Parcourir**, puis choisissez le dossier qui doit avoir le message d’accès refusé personnalisé.  
  
6.  Dans le **valeur**, tapez le message qui doit être affiché aux utilisateurs lorsqu’ils ne peuvent pas accéder à une ressource de ce dossier.  
  
    Vous pouvez ajouter des macros au message qui insère du texte personnalisé. Les macros disponibles:  
  
    -   **[Original File Path] **Le chemin d’accès de fichier d’origine est accessible par l’utilisateur.  
  
    -   **[Original File Path Folder] **Du dossier parent du chemin de fichier d’origine qui a été accessibles par l’utilisateur.  
  
    -   **[Admin Email] **La liste de destinataires de courrier électronique administrateur.  
  
    -   **[Data Owner Email] **La liste de destinataires de courrier électronique propriétaire données.  
  
7.  Cliquez sur **OK**, puis cliquez sur **fermer**.  
  
![guides de solutions](media/Deploy-Access-Denied-Assistance--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell équivalentes commandes ***  
  
L’applet de commande Windows PowerShell ou les applets de commande suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme. 
  
```  
Set-FSRMMgmtProperty -Namespace "folder path" -Name "AccessDeniedMessage_MS" -Value "Type the text that the user will see in the error message dialog box."  
```  
  
## <a name="BKMK_2"></a>Étape2: Configurer les paramètres de notification par courrier électronique  
Vous devez configurer les paramètres de notification par courrier électronique sur chaque serveur de fichiers qui enverra les messages d’assistance accès refusé.  
  
[Effectuez cette étape à l’aide de Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep2)  
  
1.  Ouvrez le Gestionnaire de ressources du serveur de fichiers. Dans le Gestionnaire de serveur, cliquez sur **outils**, puis cliquez sur **File Server Resource Manager**.  
  
2.  Avec le bouton droit **Gestionnaire de ressources du serveur de fichiers (Local)**, puis cliquez sur **configurer les Options**.  
  
3.  Cliquez sur le **Notifications par courrier électronique** onglet.  
  
4.  Configurez les paramètres suivants:  
  
    -   Dans le **nom du serveur SMTP ou l’adresse IP**, tapez le nom de l’adresse IP du serveur SMTP de votre organisation.  
  
    -   Dans le **Administrateurs destinataires par défaut** et **par défaut adresse de messagerie de** boîtes de dialogue, tapez l’adresse de messagerie de l’administrateur de serveur de fichiers.  
  
5.  Cliquez sur **envoyer un courrier électronique Test** pour vous assurer que les notifications par courrier électronique sont configurées correctement.  
  
6.  Cliquez sur **OK**.  
  
![guides de solutions](media/Deploy-Access-Denied-Assistance--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell équivalentes commandes ***  
  
L’applet de commande Windows PowerShell ou les applets de commande suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.
  
```  
set-FSRMSetting -SMTPServer "server1" -AdminEmailAddress "fileadmin@contoso.com" -FromEmailAddress "fileadmin@contoso.com"  
```  
  
## <a name="BKMK_3"></a>Étape3: Vérifier qu’assistance en cas d’accès refusé est configurée correctement  
Vous pouvez vérifier que l’assistance en cas d’accès refusé est correctement configuré par un utilisateur qui est en cours d’exécution essayez d’accéder à un partage ou un fichier dans ce partage qu’ils n’ont pas accès à Windows8. Lorsque le message d’accès refusé s’affiche, l’utilisateur doit s’afficher un **demander une Assistance** bouton. Après avoir cliqué sur le bouton demander de l’aide, l’utilisateur peut indiquer la raison de l’accès, puis envoyer un courrier électronique au propriétaire du dossier ou de l’administrateur de serveur de fichiers. Le propriétaire du dossier ou un administrateur de serveur de fichiers peut vérifier que le message est arrivé et qu’il contient les détails appropriés.  
  
> [!IMPORTANT]  
> Si vous souhaitez vérifier l’assistance en cas d’accès refusé par un utilisateur qui exécute Windows Server2012, vous devez installer la fonctionnalité expérience utilisateur avant de se connecter au partage de fichiers.  
  
## <a name="BKMK_Links"></a>Voir aussi  
  
-   [Scénario: Access-Denied Assistance](Scenario--Access-Denied-Assistance.md)  
  
-   [Planifier l’assistance en cas d’accès refusé](assetId:///b169f0a4-8b97-4da8-ae4a-c8f1986d19e1)  
  
-   [Contrôle d’accès dynamique: Vue d’ensemble du scénario](Dynamic-Access-Control--Scenario-Overview.md)  
  

