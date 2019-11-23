---
ms.assetid: b035e9f8-517f-432a-8dfb-40bfc215bee5
title: Déployer l’assistance en cas d’accès refusé (étapes de démonstration)
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: afc05f395753e5c5614e92d109d71e05980d5d92
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407171"
---
# <a name="deploy-access-denied-assistance-demonstration-steps"></a>Déployer l’assistance en cas d’accès refusé (étapes de démonstration)

>S’applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cette rubrique explique comment configurer l'assistance en cas d'accès refusé et vérifier qu'elle fonctionne correctement.  
  
**Dans ce document**  
  
-   [Étape 1 : configurer l’assistance en refus d’accès](Deploy-Access-Denied-Assistance--Demonstration-Steps-.md#BKMK_1)  
  
-   [Étape 2 : configurer les paramètres de notification par courrier électronique](Deploy-Access-Denied-Assistance--Demonstration-Steps-.md#BKMK_2)  
  
-   [Étape 3 : vérifier que l’assistance en refus d’accès est configurée correctement](Deploy-Access-Denied-Assistance--Demonstration-Steps-.md#BKMK_3)  
  
> [!NOTE]  
> Cette rubrique inclut des exemples d'applets de commande Windows PowerShell que vous pouvez utiliser pour automatiser certaines des procédures décrites. Pour plus d’informations, consultez [Utilisation des applets de commande](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="BKMK_1"></a>Étape 1 : configurer l’assistance en refus d’accès  
Vous pouvez configurer l'assistance en cas d'accès refusé dans un domaine à l'aide de la stratégie de groupe ou configurer l'assistance individuellement sur chaque serveur de fichiers à l'aide de la console Gestionnaire de ressources du serveur de fichiers. Vous pouvez aussi modifier le message d'accès refusé pour un dossier partagé spécifique sur un serveur de fichiers.  
  
Vous pouvez configurer l'assistance en cas d'accès refusé pour le domaine à l'aide de la stratégie de groupe en procédant comme suit :  
  
[Effectuez cette étape à l’aide de Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep1)  
  
#### <a name="to-configure-access-denied-assistance-by-using-group-policy"></a>Pour configurer l'assistance en cas d'accès refusé à l'aide de la stratégie de groupe  
  
1.  Ouvrez Gestion des stratégies de groupe. Dans le Gestionnaire de serveur, cliquez sur **Outils**, puis sur **Gestion de stratégie de groupe**.  
  
2.  Cliquez avec le bouton droit sur la stratégie de groupe appropriée, puis cliquez sur **Modifier**.  
  
3.  Cliquez sur **Configuration ordinateur**, **Stratégies**, **Modèles d'administration**, **Système**, puis **Assistance en cas d'accès refusé**.  
  
4.  Cliquez avec le bouton droit sur **Personnaliser le message pour les erreurs d'accès refusé**, puis cliquez sur **Modifier**.  
  
5.  Sélectionnez l'option **Activé** .  
  
6.  Configurez les options suivantes :  
  
    1.  Dans la zone **Afficher le message suivant aux utilisateurs qui se voient refuser l'accès à un dossier ou à un fichier** , tapez le message qui doit s'afficher quand l'accès à un fichier ou à un dossier est refusé à l'utilisateur.  
  
        Vous pouvez ajouter des macros au message qui insèrent du texte personnalisé. Voici les macros disponibles :  
  
        -   **[Original File Path]** Chemin d'accès au fichier d'origine auquel l'utilisateur a accédé.  
  
        -   **[Original File Path Folder]** Dossier parent du chemin d'accès au fichier d'origine auquel l'utilisateur a accédé.  
  
        -   **[Admin Email]** Liste de destinataires de messagerie d'administration.  
  
        -   **[Data Owner Email]** Liste de destinataires de messagerie des propriétaires de données.  
  
    2.  Cochez la case **Autoriser les utilisateurs à demander de l'assistance**.  
  
    3.  Conservez les autres paramètres par défaut.  
  
guides de solution ![](media/Deploy-Access-Denied-Assistance--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>commandes Windows PowerShell équivalentes</em>***  
  
L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.  
  
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
  
Vous pouvez aussi configurer l'assistance en cas d'accès refusé individuellement sur chaque serveur de fichiers à l'aide de la console Gestionnaire de ressources du serveur de fichiers.  
  
[Effectuez cette étape à l’aide de Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep1a)  
  
#### <a name="to-configure-access-denied-assistance-by-using-file-server-resource-manager"></a>Pour configurer l'assistance en cas d'accès refusé à l'aide de la console Gestionnaire de ressources du serveur de fichiers  
  
1.  Ouvrez le Gestionnaire de ressources du serveur de fichiers. Dans le Gestionnaire de serveur, cliquez sur **Outils**, puis sur **Gestionnaire de ressources du serveur de fichiers**.  
  
2.  Cliquez avec le bouton droit sur **Gestionnaire de ressources du serveur de fichiers (local)** , puis cliquez sur **Configurer les options**.  
  
3.  Cliquez sur l'onglet **Assistance en cas d'accès refusé**.  
  
4.  Cochez la case **Activer l'assistance en cas d'accès refusé**.  
  
5.  Dans la zone **Afficher le message suivant aux utilisateurs qui se voient refuser l'accès à un dossier ou à un fichier**, tapez le message qui doit s'afficher quand l'accès à un fichier ou à un dossier est refusé à l'utilisateur.  
  
    Vous pouvez ajouter des macros au message qui insèrent du texte personnalisé. Voici les macros disponibles :  
  
    -   **[Original File Path]** Chemin d'accès au fichier d'origine auquel l'utilisateur a accédé.  
  
    -   **[Original File Path Folder]** Dossier parent du chemin d'accès au fichier d'origine auquel l'utilisateur a accédé.  
  
    -   **[Admin Email]** Liste de destinataires de messagerie d'administration.  
  
    -   **[Data Owner Email]** Liste de destinataires de messagerie des propriétaires de données.  
  
6.  Cliquez sur **Configurer les demandes par courrier électronique**, cochez la case **Autoriser les utilisateurs à demander de l'assistance** , puis cliquez sur **OK**.  
  
7.  Cliquez sur **Aperçu** si vous souhaitez voir à quoi ressemblera le message d'erreur.  
  
8.  Cliquez sur **OK**.  
  
guides de solution ![](media/Deploy-Access-Denied-Assistance--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>commandes Windows PowerShell équivalentes</em>***  
  
L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.
  
```  
Set-FSRMAdrSetting -Event "AccessDenied" -DisplayMessage "Type the text that the user will see in the error message dialog box." -Enabled:$true -AllowRequests:$true  
```  
  
Après avoir configuré l'assistance en cas d'accès refusé, vous devez l'activer pour tous les types de fichiers à l'aide de la stratégie de groupe.  
  
[Effectuez cette étape à l’aide de Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep1c)  
  
#### <a name="to-configure-access-denied-assistance-for-all-file-types-by-using-group-policy"></a>Pour configurer l'assistance en cas d'accès refusé pour tous les types de fichiers à l'aide de la stratégie de groupe  
  
1.  Ouvrez Gestion des stratégies de groupe. Dans le Gestionnaire de serveur, cliquez sur **Outils**, puis sur **Gestion de stratégie de groupe**.  
  
2.  Cliquez avec le bouton droit sur la stratégie de groupe appropriée, puis cliquez sur **Modifier**.  
  
3.  Cliquez sur **Configuration ordinateur**, **Stratégies**, **Modèles d'administration**, **Système**, puis **Assistance en cas d'accès refusé**.  
  
4.  Cliquez avec le bouton droit sur **Activer l'assistance en cas d'accès refusé sur le client pour tous les types de fichiers**, puis cliquez sur **Modifier**.  
  
5.  Cliquez sur **Activé**, puis sur **OK**.  
  
guides de solution ![](media/Deploy-Access-Denied-Assistance--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>commandes Windows PowerShell équivalentes</em>***  
  
L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme. 
  
```  
Set-GPRegistryValue -Name "Name of GPO" -key "HKLM\SOFTWARE\Policies\Microsoft\Windows\Explore" -ValueName EnableShellExecuteFileStreamCheck -Type DWORD -value 1  
  
```  
  
Vous pouvez aussi spécifier un message d'accès refusé distinct pour chaque dossier partagé sur un serveur de fichiers à l'aide de la console Gestionnaire de ressources du serveur de fichiers.  
  
[Effectuez cette étape à l’aide de Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep1b)  
  
#### <a name="to-specify-a-separate-access-denied-message-for-a-shared-folder-by-using-file-server-resource-manager"></a>Pour spécifier un message d'accès refusé distinct pour un dossier partagé à l'aide du Gestionnaire de ressources du serveur de fichiers  
  
1.  Ouvrez le Gestionnaire de ressources du serveur de fichiers. Dans le Gestionnaire de serveur, cliquez sur **Outils**, puis sur **Gestionnaire de ressources du serveur de fichiers**.  
  
2.  Développez **Gestionnaire de ressources du serveur de fichiers (local)** , puis cliquez sur **Gestion de la classification**.  
  
3.  Cliquez avec le bouton droit sur **Propriétés de classification**, puis cliquez sur **Définir les propriétés de gestion des dossiers**.  
  
4.  Dans la zone **Propriété**, cliquez sur **Message d'assistance en cas d'accès refusé**, puis sur **Ajouter**.  
  
5.  Cliquez sur **Parcourir** et choisissez le dossier auquel vous souhaitez affecter le message d'accès refusé personnalisé.  
  
6.  Dans la zone **Valeur**, tapez le message qui doit être affiché quand l'utilisateur ne peut pas accéder à une ressource de ce dossier.  
  
    Vous pouvez ajouter des macros au message qui insèrent du texte personnalisé. Voici les macros disponibles :  
  
    -   **[Original File Path]** Chemin d'accès au fichier d'origine auquel l'utilisateur a accédé.  
  
    -   **[Original File Path Folder]** Dossier parent du chemin d'accès au fichier d'origine auquel l'utilisateur a accédé.  
  
    -   **[Admin Email]** Liste de destinataires de messagerie d'administration.  
  
    -   **[Data Owner Email]** Liste de destinataires de messagerie des propriétaires de données.  
  
7.  Cliquez sur **OK**, puis sur **Fermer**.  
  
guides de solution ![](media/Deploy-Access-Denied-Assistance--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>commandes Windows PowerShell équivalentes</em>***  
  
L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme. 
  
```  
Set-FSRMMgmtProperty -Namespace "folder path" -Name "AccessDeniedMessage_MS" -Value "Type the text that the user will see in the error message dialog box."  
```  
  
## <a name="BKMK_2"></a>Étape 2 : configurer les paramètres de notification par courrier électronique  
Vous devez configurer les paramètres de notification par courrier électronique sur chaque serveur de fichiers qui enverra les messages d'assistance en cas d'accès refusé.  
  
[Effectuez cette étape à l’aide de Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep2)  
  
1.  Ouvrez le Gestionnaire de ressources du serveur de fichiers. Dans le Gestionnaire de serveur, cliquez sur **Outils**, puis sur **Gestionnaire de ressources du serveur de fichiers**.  
  
2.  Cliquez avec le bouton droit sur **Gestionnaire de ressources du serveur de fichiers (local)** , puis cliquez sur **Configurer les options**.  
  
3.  Cliquez sur l'onglet **Notifications par courrier électronique** .  
  
4.  Configurez les paramètres suivants :  
  
    -   Dans la zone **Nom ou adresse IP du serveur SMTP** , tapez le nom ou l'adresse IP du serveur SMTP de votre organisation.  
  
    -   Dans les zones de texte **destinataires de l’administrateur par défaut** et **adresse de messagerie par défaut « de »** , tapez l’adresse de messagerie de l’administrateur du serveur de fichiers.  
  
5.  Cliquez sur **Envoyer un message de test** pour vérifier que les notifications par courrier électronique sont configurées correctement.  
  
6.  Cliquez sur **OK**.  
  
guides de solution ![](media/Deploy-Access-Denied-Assistance--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>commandes Windows PowerShell équivalentes</em>***  
  
L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.
  
```  
set-FSRMSetting -SMTPServer "server1" -AdminEmailAddress "fileadmin@contoso.com" -FromEmailAddress "fileadmin@contoso.com"  
```  
  
## <a name="BKMK_3"></a>Étape 3 : vérifier que l’assistance en refus d’accès est configurée correctement  
Vous pouvez vérifier que l’assistance en cas d’accès refusé est configurée correctement en demandant à un utilisateur qui exécute Windows 8 d’essayer d’accéder à un partage ou à un fichier de ce partage auquel il n’a pas accès. À l'apparition du message d'accès refusé, un bouton **Demander de l'aide** doit s'afficher. Après avoir cliqué sur le bouton Demander de l'aide, l'utilisateur peut indiquer le motif de sa demande d'accès et envoyer un message électronique au propriétaire du dossier ou à l'administrateur du serveur de fichiers. Celui-ci peut vérifier que ce message est arrivé et qu'il contient les détails appropriés.  
  
> [!IMPORTANT]  
> Si vous souhaitez vérifier l’assistance en cas d’accès refusé en demandant à un utilisateur qui exécute Windows Server 2012, vous devez installer l’expérience utilisateur avant de vous connecter au partage de fichiers.  
  
## <a name="BKMK_Links"></a>Voir aussi  
  
-   [Scénario : assistance en cas d’accès refusé](Scenario--Access-Denied-Assistance.md)  
  
-   [Planifier l’assistance en cas d’accès refusé](assetId:///b169f0a4-8b97-4da8-ae4a-c8f1986d19e1)  
  
-   [Access Control dynamique : vue d’ensemble du scénario](Dynamic-Access-Control--Scenario-Overview.md)  
  

