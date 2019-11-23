---
ms.assetid: c8597cc8-bdcb-4e59-a09e-128ef5ebeaf8
title: Audit des processus de ligne de commande
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 5d5ab971327ab7ec16bf2748571882458cc38f72
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71368989"
---
# <a name="command-line-process-auditing"></a>Audit des processus de ligne de commande

>S’applique à : Windows Server 2016, Windows Server 2012 R2

**Auteur**: Justin Turner, ingénieur du support technique senior avec le groupe Windows  
  
> [!NOTE]  
> Ce contenu est écrit par un ingénieur du support client Microsoft et est destiné aux administrateurs expérimentés et aux architectes système qui recherchent des explications techniques plus approfondies des fonctionnalités et des solutions Windows Server 2012 R2 que n'en proposent généralement les rubriques de TechNet. Toutefois, il n'a pas subi les mêmes passes de correction. De ce fait, une partie du langage peut sembler moins finalisée que le contenu de TechNet.  
  
## <a name="overview"></a>Vue d’ensemble  
  
-   L’ID d’événement d’audit de création de processus pré-existant 4688 inclut désormais des informations d’audit pour les processus de ligne de commande.  
  
-   Il journalise également le hachage SHA1/2 du fichier exécutable dans le journal des événements AppLocker  
  
    -   Logs\Microsoft\Windows\AppLocker de l’application et des services  
  
-   Vous activez via un objet de stratégie de groupe, mais il est désactivé par défaut.  
  
    -   « Inclure la ligne de commande dans les événements de création de processus »  
  
![audit de la ligne de commande](media/Command-line-process-auditing/GTR_ADDS_Event4688.gif)  
  
**Figure SEQ figure \\\* l’événement arabe 16 4688**  
  
Passez en revue l’ID d’événement 4688 mis à jour dans REF _Ref366427278 \h figure 16.  Avant cette mise à jour, aucune information pour **traiter la ligne de commande** n’est journalisée.  En raison de cette journalisation supplémentaire, nous pouvons maintenant voir que non seulement le processus WScript. exe a démarré, mais qu’il a également été utilisé pour exécuter un script VB.  
  
## <a name="configuration"></a>Configuration  
Pour voir les effets de cette mise à jour, vous devez activer deux paramètres de stratégie.  
  
### <a name="you-must-have-audit-process-creation-auditing-enabled-to-see-event-id-4688"></a>Vous devez avoir activé l’audit de création de processus d’audit pour voir l’ID d’événement 4688.  
Pour activer la stratégie d’audit de création de processus, modifiez la stratégie de groupe suivante :  
  
**Emplacement de la stratégie :** Configuration de l’ordinateur > stratégies > Paramètres Windows > paramètres de sécurité > Configuration avancée de l’audit > suivi détaillé  
  
**Nom de la stratégie :** Auditer la création de processus  
  
**Pris en charge sur :** Windows 7 et versions ultérieures  
  
**Description/aide :**  
  
Ce paramètre de stratégie de sécurité détermine si le système d’exploitation génère des événements d’audit lors de la création d’un processus (démarre) et le nom du programme ou de l’utilisateur qui l’a créé.  
  
Ces événements d’audit peuvent vous aider à comprendre comment un ordinateur est utilisé et à effectuer le suivi de l’activité des utilisateurs.  
  
Volume d’événements : faible à moyen, en fonction de l’utilisation du système  
  
**Par défaut :** Non configuré  
  
### <a name="in-order-to-see-the-additions-to-event-id-4688-you-must-enable-the-new-policy-setting-include-command-line-in-process-creation-events"></a>Pour afficher les ajouts à l’ID d’événement 4688, vous devez activer le nouveau paramètre de stratégie : inclure la ligne de commande dans les événements de création de processus  
**Table SEQ Table \\\* paramètre de stratégie de processus de ligne de commande arabe 19**  
  
|Configuration de la stratégie|Détails|  
|------------------------|-----------|  
|**Chemin d’accès**|Création de processus Templates\System\Audit d’administration|  
|**Paramètre**|**Inclure la ligne de commande dans les événements de création de processus**|  
|**Paramètre par défaut**|Non configuré (non activé)|  
|**Pris en charge sur :**|?|  
|**Description**|Ce paramètre de stratégie détermine les informations qui sont consignées dans les événements d’audit de sécurité lors de la création d’un nouveau processus.<br /><br />Ce paramètre s’applique uniquement lorsque la stratégie d’audit de création de processus est activée. Si vous activez cette stratégie, les informations de ligne de commande pour chaque processus seront journalisées en texte brut dans le journal des événements de sécurité dans le cadre de l’événement de création de processus d’audit 4688 « un nouveau processus a été créé » sur les stations de travail et les serveurs sur lesquels cette stratégie le paramètre est appliqué.<br /><br />Si vous désactivez ou ne configurez pas ce paramètre de stratégie, les informations de ligne de commande du processus ne seront pas incluses dans les événements de création de processus d’audit.<br /><br />Par défaut : non configuré<br /><br />Remarque : lorsque ce paramètre de stratégie est activé, tout utilisateur disposant d’un accès pour lire les événements de sécurité peut lire les arguments de ligne de commande pour tout processus créé avec succès. Les arguments de ligne de commande peuvent contenir des informations sensibles ou privées, telles que des mots de passe ou des données utilisateur.|  
  
![audit de la ligne de commande](media/Command-line-process-auditing/GTR_ADDS_IncludeCLISetting.gif)  
  
Lorsque vous utilisez les paramètres Configuration avancée de la stratégie d’audit, vous devez confirmer que ces paramètres ne sont pas remplacés par les paramètres de stratégie d’audit de base.  L’événement 4719 est consigné lorsque les paramètres sont remplacés.  
  
![audit de la ligne de commande](media/Command-line-process-auditing/GTR_ADDS_Event4719.gif)  
  
La procédure suivante montre comment empêcher les conflits en bloquant l’application des paramètres de stratégie d’audit de base.  
  
### <a name="to-ensure-that-advanced-audit-policy-configuration-settings-are-not-overwritten"></a>Pour vous assurer que les paramètres de configuration de stratégie d’audit avancés ne sont pas remplacés  
![audit de la ligne de commande](media/Command-line-process-auditing/GTR_ADDS_AdvAuditPolicy.gif)  
  
1.  Ouvrir la console de gestion stratégie de groupe  
  
2.  Cliquez avec le bouton droit sur stratégie de domaine par défaut, puis cliquez sur modifier.  
  
3.  Double-cliquez sur Configuration ordinateur, sur stratégies, puis sur paramètres Windows.  
  
4.  Double-cliquez sur paramètres de sécurité, double-cliquez sur stratégies locales, puis cliquez sur options de sécurité.  
  
5.  Double-cliquez sur audit : forcer les paramètres de sous-catégorie de stratégie d’audit (Windows Vista ou version ultérieure) pour remplacer les paramètres de catégorie de stratégie d’audit, puis cliquez sur définir ce paramètre de stratégie.  
  
6.  Cliquez sur activé, puis sur OK.  
  
## <a name="additional-resources"></a>Ressources complémentaires  
[Auditer la création de processus](https://technet.microsoft.com/library/dd941613(v=WS.10).aspx)  
  
[Guide pas à pas de la stratégie d’audit de sécurité avancée](https://technet.microsoft.com/library/dd408940(v=WS.10).aspx)  
  
[AppLocker : Forum aux questions](https://technet.microsoft.com/library/ee619725(v=ws.10).aspx)  
  
## <a name="try-this-explore-command-line-process-auditing"></a>Essayez ceci : explorer l’audit des processus de ligne de commande  
  
1.  Activez les événements de **création de processus d’audit** et assurez-vous que la configuration de la stratégie d’audit avancée n’est pas remplacée  
  
2.  Créez un script qui générera des événements intéressants et exécutera le script.  Observez les événements.  Le script utilisé pour générer l’événement dans la leçon ressemble à ceci :  
  
    ```  
    mkdir c:\systemfiles\temp\commandandcontrol\zone\fifthward  
    copy \\192.168.1.254\c$\hidden c:\systemfiles\temp\hidden\commandandcontrol\zone\fifthward  
    start C:\systemfiles\temp\hidden\commandandcontrol\zone\fifthward\ntuserrights.vbs  
    del c:\systemfiles\temp\*.* /Q  
    ```  
  
3.  Activer l’audit des processus de ligne de commande  
  
4.  Exécuter le même script qu’avant et observer les événements  
  


