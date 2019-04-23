---
ms.assetid: c8597cc8-bdcb-4e59-a09e-128ef5ebeaf8
title: Audit des processus de ligne de commande
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 66ae6992775319cf614b0cb4c21f864150746687
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859550"
---
# <a name="command-line-process-auditing"></a>Audit des processus de ligne de commande

>S'applique à : Windows Server 2016, Windows Server 2012 R2

**Auteur**: Justin Turner, ingénieur Support résolution Senior auprès du groupe Windows  
  
> [!NOTE]  
> Ce contenu est écrit par un ingénieur du support client Microsoft et est destiné aux administrateurs expérimentés et aux architectes système qui recherchent des explications techniques plus approfondies des fonctionnalités et des solutions Windows Server 2012 R2 que n'en proposent généralement les rubriques de TechNet. Toutefois, il n'a pas subi les mêmes passes de correction. De ce fait, une partie du langage peut sembler moins finalisée que le contenu de TechNet.  
  
## <a name="overview"></a>Vue d'ensemble  
  
-   L’événement d’audit de création préexistant processus ID 4688 inclut désormais les informations d’audit pour les processus de ligne de commande.  
  
-   Il enregistre également hachage SHA1/2 de l’exécutable dans le journal des événements Applocker  
  
    -   Application et des services\microsoft\windows\applocker  
  
-   Vous activez via un GPO, mais il est désactivé par défaut  
  
    -   « Inclure la ligne de commande dans les événements de création de processus »  
  
![l’audit de ligne de commande](media/Command-line-process-auditing/GTR_ADDS_Event4688.gif)  
  
**Figure SEQ Figure \\ \* événement 16 arabe 4688**  
  
Passez en revue le mises à jour ID d’événement 4688 dans REF _Ref366427278 \h Figure 16.  Avant cette version aucune mise à jour des informations de **ligne de commande de processus** consignées.  En raison de cette journalisation supplémentaire, nous pouvons maintenant voir que non seulement le processus wscript.exe n’a été démarré, mais qu’il était également utilisé pour exécuter un script VB.  
  
## <a name="configuration"></a>Configuration  
Pour voir les effets de cette mise à jour, vous devez activer les deux paramètres de stratégie.  
  
### <a name="you-must-have-audit-process-creation-auditing-enabled-to-see-event-id-4688"></a>Vous devez disposer de la création du processus d’Audit l’audit est activé pour voir les ID d’événement 4688.  
Pour activer la stratégie de création de processus d’Audit, modifiez la stratégie de groupe suivant :  
  
**Emplacement de la stratégie :** Configuration ordinateur > stratégies > Paramètres de Windows > Paramètres de sécurité > Configuration avancée d’Audit > détaillées de suivi  
  
**Nom de la stratégie :** Auditer la création du processus  
  
**Pris en charge :** Windows 7 et versions ultérieures  
  
**Description/support :**  
  
Ce paramètre de stratégie de sécurité détermine si le système d’exploitation génère des événements d’audit lorsqu’un processus est créé (démarrage) et le nom du programme ou de l’utilisateur qui l’a créée.  
  
Ces événements d’audit peut vous aider à comprendre comment un ordinateur est utilisé et pour effectuer le suivi de l’activité des utilisateurs.  
  
Volume d’événements : Faible à moyen, en fonction de l’utilisation du système  
  
**Par défaut :** Non configuré  
  
### <a name="in-order-to-see-the-additions-to-event-id-4688-you-must-enable-the-new-policy-setting-include-command-line-in-process-creation-events"></a>Pour afficher les ajouts apportés à l’ID d’événement 4688, vous devez activer le nouveau paramètre de stratégie : Inclure la ligne de commande dans les événements de création de processus  
**Table SEQ Table \\ \* paramètre de stratégie de processus de ligne commande de 19 arabe**  
  
|Configuration de la stratégie|Détails|  
|------------------------|-----------|  
|**Path**|Création du processus d’administration Templates\System\Audit|  
|**Paramètre**|**Inclure la ligne de commande dans les événements de création de processus**|  
|**Paramètre par défaut**|Non configuré (non activé)|  
|**Pris en charge :**|?|  
|**Description**|Ce paramètre de stratégie détermine quelles informations sont enregistrées dans les événements d’audit de sécurité lorsqu’un nouveau processus a été créé.<br /><br />Ce paramètre s’applique uniquement lorsque la stratégie de création de processus d’Audit est activée. Si vous activez ce paramètre les informations de ligne de commande pour chaque processus sera connecté en texte brut dans le journal d’événements de sécurité dans le cadre de l’événement de création du processus d’Audit 4688 de stratégie, « un nouveau processus a été créé, » sur les stations de travail et les serveurs sur lequel ce stratégie paramètre est appliqué.<br /><br />Si vous désactivez ou ne configurez pas ce paramètre de stratégie, les informations de ligne de commande du processus ne seront pas incluses dans les événements de la création du processus d’Audit.<br /><br />Default : Non configuré<br /><br />Remarque: Lorsque ce paramètre de stratégie est activé, tout utilisateur ayant accès à lire que les événements de sécurité seront en mesure de lire les arguments de ligne de commande pour toute correctement créé les processus. Arguments de ligne de commande peuvent contenir des informations sensibles et confidentielles telles que les mots de passe ou des données utilisateur.|  
  
![l’audit de ligne de commande](media/Command-line-process-auditing/GTR_ADDS_IncludeCLISetting.gif)  
  
Lorsque vous utilisez les paramètres Configuration avancée de la stratégie d’audit, vous devez confirmer que ces paramètres ne sont pas remplacés par les paramètres de stratégie d’audit de base.  Événement 4719 est consigné lorsque les paramètres sont remplacés.  
  
![l’audit de ligne de commande](media/Command-line-process-auditing/GTR_ADDS_Event4719.gif)  
  
La procédure suivante montre comment éviter les conflits en bloquant l’application des paramètres de stratégie d’audit de base.  
  
### <a name="to-ensure-that-advanced-audit-policy-configuration-settings-are-not-overwritten"></a>Pour vous assurer que les paramètres de Configuration avancée de stratégie d’Audit ne sont pas remplacés.  
![l’audit de ligne de commande](media/Command-line-process-auditing/GTR_ADDS_AdvAuditPolicy.gif)  
  
1.  Ouvrez la console Gestion de stratégie de groupe  
  
2.  Avec le bouton droit de la stratégie de domaine par défaut, puis cliquez sur Modifier.  
  
3.  Double-cliquez sur la Configuration de l’ordinateur, double-cliquez sur des stratégies, puis double-cliquez sur paramètres de Windows.  
  
4.  Double-cliquez sur paramètres de sécurité, stratégies locales, puis cliquez sur Options de sécurité.  
  
5.  Double-cliquez sur d’Audit : Forcer les paramètres de sous-catégorie de stratégie d’audit (Windows Vista ou version ultérieure) pour remplacer les paramètres de catégorie de stratégie d’audit, puis cliquez sur Définir ce paramètre de stratégie.  
  
6.  Cliquez sur activé, puis cliquez sur OK.  
  
## <a name="additional-resources"></a>Ressources complémentaires  
[Création du processus d’audit](https://technet.microsoft.com/library/dd941613(v=WS.10).aspx)  
  
[Advanced Guide pas à pas des stratégie d’Audit de sécurité](https://technet.microsoft.com/library/dd408940(v=WS.10).aspx)  
  
[AppLocker : Forum aux Questions](https://technet.microsoft.com/library/ee619725(v=ws.10).aspx)  
  
## <a name="try-this-explore-command-line-process-auditing"></a>Essayez ceci : Explorez l’audit des processus de ligne de commande  
  
1.  Activer **la création du processus d’Audit** événements et vérifiez la configuration de stratégie d’Audit avancée n’est pas remplacée  
  
2.  Créer un script qui génère des événements d’intérêt et exécutez le script.  Observez les événements.  Le script utilisé pour générer l’événement dans la leçon ressemblait à ceci :  
  
    ```  
    mkdir c:\systemfiles\temp\commandandcontrol\zone\fifthward  
    copy \\192.168.1.254\c$\hidden c:\systemfiles\temp\hidden\commandandcontrol\zone\fifthward  
    start C:\systemfiles\temp\hidden\commandandcontrol\zone\fifthward\ntuserrights.vbs  
    del c:\systemfiles\temp\*.* /Q  
    ```  
  
3.  Activer l’audit des processus de ligne de commande  
  
4.  Exécuter le même script comme avant et observer les événements  
  


