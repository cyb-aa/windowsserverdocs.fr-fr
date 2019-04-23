---
ms.assetid: 249ba1be-b0d3-4a77-99af-3699074a2b6e
title: Résolution des problèmes relatifs aux contrôleurs de domaine virtualisés
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ca58d36599be76e4d196d91f298b9841884e7a36
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863660"
---
# <a name="virtualized-domain-controller-troubleshooting"></a>Résolution des problèmes relatifs aux contrôleurs de domaine virtualisés

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cette rubrique fournit une méthodologie détaillée pour résoudre les problèmes liés aux fonctionnalités d'un contrôleur de domaine virtualisé.  
  
-   [Résolution des problèmes de virtualiser le clonage de contrôleur de domaine](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_TshootVDCCloning)  
  
-   [Résolution des problèmes de restauration sécurisée des contrôleurs de domaine virtualisés](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_TshootVDCSafeRestore)  
  
## <a name="BKMK_Intro"></a>Introduction  
Le meilleur moyen d'améliorer vos compétences en matière de résolution des problèmes est de créer un laboratoire de test et d'examiner rigoureusement les scénarios de travail normaux. Si vous rencontrez des erreurs, elles sont plus évidentes et plus faciles à comprendre, car vous avez une bonne compréhension du fonctionnement de la promotion d'un contrôleur de domaine. Cela vous permet également de développer vos compétences en matière d'analyses et d'analyses réseau. Cela vaut pour toutes les technologies de systèmes distribués, et pas seulement le déploiement de contrôleurs de domaine virtualisés.  
  
Les éléments essentiels pour la résolution avancée des problèmes de configuration d'un contrôleur de domaine sont les suivants :  
  
1.  Analyse linéaire combinée à la focalisation et la perception des détails  
  
2.  Présentation de l'analyse de capture réseau  
  
3.  Présentation des journaux intégrés  
  
Le premier point et le deuxième point dépassent le cadre de cette rubrique. Toutefois, le troisième point peut être expliqué en détail. La résolution des problèmes de contrôleur de domaine virtualisé nécessite une méthode logique et linéaire. La solution consiste à aborder la question à l'aide des données fournies et à ne recourir aux outils et analyses complexes qu'après avoir épuisé les informations de sortie et de journalisation reçues.  
  
## <a name="BKMK_TshootVDCCloning"></a>Résolution des problèmes de virtualiser le clonage de contrôleur de domaine  
Cette section traite des sujets suivants :  
  
-   [Outils de dépannage](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_Tools)  
  
-   [Options de journalisation](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_LoggingOptions)  
  
-   [Méthodologie générale pour la résolution des problèmes de domaine le clonage de contrôleur](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_GeneralMethodology)  
  
-   [Server Core et le journal des événements](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_ServerCoreEvents)  
  
-   [Résolution des problèmes spécifiques](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_SpecificProblems)  
  
La stratégie de résolution des problèmes de clonage d'un contrôleur de domaine virtualisé respecte le format général suivant :  
  
![résolution des problèmes de contrôleur de domaine virtuel](media/Virtualized-Domain-Controller-Troubleshooting/ADDS_VDC_TroublehsootingFlowchart.png)  
  
### <a name="BKMK_Tools"></a>Outils de dépannage  
  
#### <a name="BKMK_LoggingOptions"></a>Options de journalisation  
Les journaux intégrés sont l'outil le plus important pour la résolution des problèmes de clonage de contrôleur de domaine. Par défaut, tous ces journaux sont activés et configurés pour fournir un mode détaillé maximal.  
  
|||  
|-|-|  
|**Opération**|**Log**|  
|**Le clonage**|-Observateur d’événements\journaux Windows\Système<br />-Événements viewer\Applications et des services\Service<br />-   %systemroot%\debug\dcpromo.log|  
|**Promotion**|-   %systemroot%\debug\dcpromo.log<br />-Événements viewer\Applications et des services\Service<br />-Observateur d’événements\journaux Windows\Système<br />-Événements viewer\Applications et services services\Service de réplication<br />-Événements viewer\Applications et services des services\réplication DFS|  
  
#### <a name="tools-and-commands-for-troubleshooting-domain-controller-configuration"></a>Outils et commandes pour la résolution des problèmes de configuration des contrôleurs de domaine  
Pour résoudre les problèmes non décrits par les journaux, utilisez les outils suivants comme point de départ :  
  
-   Dcdiag.exe  
  
-   Repadmin.exe  
  
-   Moniteur réseau 3.4  
  
### <a name="BKMK_GeneralMethodology"></a>Méthodologie générale pour la résolution des problèmes de domaine le clonage de contrôleur  
  
1.  Est-ce que l'ordinateur virtuel démarre en mode de réparation des services d'annuaire (DSRM) ? Cela indique qu'une résolution des problèmes est nécessaire. Pour vous connecter en mode DSRM, utilisez le compte **.\Administrateur** et spécifiez le mot de passe DSRM.  
  
    1.  Examinez le fichier Dcpromo.log.  
  
        1.  Est-ce que les étapes de clonage initiales ont réussi alors que la promotion du contrôleur de domaine a échoué ?  
  
        2.  Est-ce que les erreurs indiquent des problèmes avec le contrôleur de domaine local ou avec l'environnement AD DS, par exemple le retour d'erreurs par l'émulateur de contrôleur de domaine principal (PDC, Primary Domain Controller) ?  
  
    2.  Examinez les journaux des événements Système et Services d'annuaire, ainsi que les fichiers dccloneconfig.xml et CustomDCCloneAllowList.xml  
  
        1.  Est-ce qu'une application incompatible doit se trouver dans la liste verte de CustomDCCloneAllowList.xml ?  
  
        2.  Est-ce que l'adresse IP ou le nom d'ordinateur est dupliqué ou non valide dans le fichier dccloneconfig.xml ?  
  
        3.  Est-ce que le site Active Directory est non valide dans le fichier dccloneconfig.xml ?  
  
        4.  Est-ce que l'adresse IP n'est pas définie dans le fichier dccloningconfig.xml et est-ce qu'il n'y a aucun serveur DHCP disponible ?  
  
        5.  Est-ce que l'émulateur PDC est en ligne et accessible via le protocole RPC ?  
  
        6.  Est-ce que le contrôleur de domaine est membre du groupe Contrôleurs de domaine clonables ? Est-ce que l'autorisation **Autoriser un contexte de nom à créer un clone de lui-même** est définie à la racine du domaine du groupe ?  
  
        7.  Est-ce que le fichier Dccloneconfig.xml contient des erreurs de syntaxe qui empêchent le bon déroulement de l'analyse ?  
  
        8.  Est-ce que l'hyperviseur est pris en charge ?  
  
        9. Est-ce que la promotion du contrôleur de domaine a échoué après le démarrage du clonage ?  
  
        10. Est-ce que le nombre maximal de noms de contrôleurs de domaine générés automatiquement (9 999) a été dépassé ?  
  
        11. Est-ce que l'adresse MAC est dupliquée ?  
  
2.  Est-ce que le nom d'hôte du clone est identique à celui du contrôleur de domaine source ?  
  
    1.  Existe-t-il un fichier Dccloneconfig.xml dans l'un des emplacements autorisés ?  
  
3.  Est-ce qu'après le démarrage de l'ordinateur virtuel en mode normal et la réussite du clonage, le contrôleur de domaine fonctionne de manière incorrecte ?  
  
    1.  Vérifiez d'abord si le nom d'hôte a été changé sur le clone. Si le nom d'hôte est différent, le clonage s'est effectué au moins partiellement.  
  
    2.  Est-ce que le contrôleur de domaine possède une adresse IP dupliquée du contrôleur de domaine source à partir du fichier dccloneconfig.xml, alors que le contrôleur de domaine source était hors connexion durant le clonage ?  
  
    3.  Si le contrôleur de domaine effectue une publication, traitez cette question comme n'importe quel problème post-promotion non lié au clonage.  
  
    4.  Si le contrôleur de domaine n'effectue aucune publication, recherchez les erreurs post-promotion dans les journaux des événements Service d'annuaire, Système, Application, Réplication de fichiers et Réplication DFS.  
  
#### <a name="disabling-dsrm-boot"></a>Désactivation du démarrage en mode DSRM  
Après un démarrage en mode DSRM à la suite d'une erreur, diagnostiquez la cause de l'échec. Par ailleurs, si le fichier dcpromo.log n'indique pas qu'il est impossible de retenter le clonage, résolvez le problème et réinitialisez l'indicateur DSRM. Un clone raté ne retourne pas automatiquement en mode normal au prochain redémarrage. Vous devez supprimer l'indicateur de démarrage DSRM pour pouvoir retenter le clonage. Toutes ces étapes nécessitent une exécution des commandes en tant qu'administrateur avec des privilèges élevés.  
  
##### <a name="removing-dsrm-with-msconfigexe"></a>Suppression du mode DSRM avec Msconfig.exe  
Pour désactiver le démarrage en mode DSRM via une interface graphique utilisateur, utilisez l'outil de configuration système :  
  
1.  Exécutez msconfig.exe  
  
2.  Sous l'onglet **Démarrer**, sous **Options de démarrage**, désélectionnez **Démarrage sécurisé** (si l'option est déjà activée en même temps que l'option **Réparer Active Directory**).  
  
3.  Cliquez sur OK et redémarrez le système quand vous y êtes invité.  
  
##### <a name="removing-dsrm-with-bcdeditexe"></a>Suppression du mode DSRM avec Bcdedit.exe  
Pour désactiver le mode DSRM à partir de la ligne de commande, utilisez l'Éditeur du magasin des données de configuration de démarrage :  
  
1.  Ouvrez une invite de commandes et exécutez ce qui suit :  
  
    ```  
    Bcdedit.exe /deletevalue safeboot  
    ```  
  
2.  Redémarrez l'ordinateur avec :  
  
    ```  
    Shutdown.exe /t /0 /r  
    ```  
  
> [!NOTE]  
> Bcdedit.exe fonctionne aussi dans une console Windows PowerShell. Les commandes sont les suivantes :  
>   
> Bcdedit.exe /deletevalue safeboot  
>   
> Restart-computer  
  
### <a name="BKMK_ServerCoreEvents"></a>Server Core et le journal des événements  
Les journaux des événements contiennent de nombreuses informations utiles sur les opérations de clonage de contrôleurs de domaine virtualisés. Par défaut, une installation de Windows Server 2012 est une installation Server Core. Cela signifie qu'il n'existe pas d'interface graphique et donc aucun moyen d'exécuter le composant logiciel enfichable local Observateur d'événements.  
  
Pour consulter les journaux des événements sur un serveur exécutant une installation Server Core :  
  
-   Exécutez l'outil Wevtutil.exe localement.  
  
-   Exécutez l'applet de commande PowerShell Get-WinEvent localement.  
  
-   Si vous avez activé les règles de pare-feu avancé de Windows pour les groupes « Gestion à distance du journal des événements » (ou ports équivalents) autoriser les communications entrantes, vous pouvez gérer le journal des événements à distance via Eventvwr.exe, wevtutil.exe ou Get-Winevent. Vous pouvez le faire sur une installation Server Core à l'aide de NETSH.exe, la stratégie de groupe ou la nouvelle applet de commande Set-NetFirewallRule de Windows PowerShell 3.0.  
  
> [!WARNING]  
> N'essayez pas de rajouter l'interpréteur de commandes graphique à l'ordinateur quand il est en mode DSRM. La pile de traitements Windows (CBS) ne peut pas fonctionner correctement en mode sans échec ou DSRM. Les tentatives d'ajout de fonctionnalités ou de rôles en mode DSRM n'aboutissent pas et laissent l'ordinateur dans un état instable jusqu'à ce qu'il soit démarré normalement. Comme un clone de contrôleur de domaine virtualisé en mode DSRM ne peut pas démarrer normalement, et qu'il ne doit pas démarrer normalement dans la plupart des cas, il est impossible d'ajouter l'interpréteur de commandes graphique en toute sécurité. Cela n'est pas pris en charge et peut rendre le serveur inutilisable.  
  
### <a name="BKMK_SpecificProblems"></a>Résolution des problèmes spécifiques  
  
#### <a name="events"></a>Événements  
Tous les événements de clonage de contrôleurs de domaine virtualisés sont écrits dans le journal des événements Services d'annuaire de l'ordinateur virtuel du contrôleur de domaine clone. Les journaux des événements Application, Service de réplication de fichiers et Réplication DFS peuvent également contenir des informations utiles pour résoudre les problèmes d'échec de clonage. Les échecs qui se produisent durant l'appel RPC de l'émulateur PDC peuvent être signalés dans le journal des événements sur l'émulateur PDC.  
  
Vous trouverez ci-dessous les événements de clonage Windows Server 2012 dans le journal des événements Services d'annuaire, avec des remarques et des suggestions pour la résolution des erreurs.  
  
##### <a name="directory-services-event-log"></a>Journal des événements Services d'annuaire  
  
|||  
|-|-|  
|**ID d’événement**|**2160**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Niveau de gravité**|Informationnel|  
|**Message**|Local *<COMPUTERNAME>* a trouvé un fichier de configuration de clonage de contrôleur de domaine virtuel.<br /><br />Le fichier de configuration de duplication de contrôleur de domaine virtuel se trouve à : %1.<br /><br />L'existence du fichier de configuration de clonage de contrôleur de domaine virtuel indique que le contrôleur de domaine virtuel local est un clone d'un autre contrôleur de domaine virtuel. Le *<COMPUTERNAME>* commenceront à se cloner elle-même.|  
|**Notes de publication et de résolution**|Il s'agit d'un événement de succès. Il ne représente un problème que s'il est inattendu. Recherchez le fichier dcclconeconfig.xml dans le répertoire de travail de DSA, %systemroot%\ntds, ainsi qu'à la racine de tous les disques locaux ou amovibles.|  
  
|||  
|-|-|  
|**ID d’événement**|**2161**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Niveau de gravité**|Informationnel|  
|**Message**|Local *<COMPUTERNAME>* n’a pas trouvé le fichier de configuration de clonage de contrôleur de domaine virtuel. L'ordinateur local n'est pas un contrôleur de domaine cloné.|  
|**Notes de publication et de résolution**|Il s'agit d'un événement de succès. Il ne représente un problème que s'il est inattendu. Recherchez le fichier dcclconeconfig.xml dans le répertoire de travail de DSA, %systemroot%\ntds, ainsi qu'à la racine de tous les disques locaux ou amovibles.|  
  
|||  
|-|-|  
|**ID d’événement**|**2162**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Niveau de gravité**|Erreur|  
|**Message**|Échec du clonage du contrôleur de domaine virtuel.<br /><br />Vérifiez les événements consignés dans les journaux des événements Système et dans %systemroot%\debug\dcpromo.log pour plus d'informations sur les erreurs qui correspondent à la tentative de clonage du contrôleur de domaine virtuel.<br /><br />Code d’erreur : %1|  
|**Notes de publication et de résolution**|Suivez les instructions du message, cette erreur est un fourre-tout.|  
  
|||  
|-|-|  
|**ID d’événement**|**2163**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Niveau de gravité**|Informationnel|  
|**Message**|Le service DsRoleSvc a démarré pour cloner le contrôleur de domaine virtuel local.|  
|**Notes de publication et de résolution**|Il s'agit d'un événement de succès. Il ne représente un problème que s'il est inattendu. Recherchez le fichier dcclconeconfig.xml dans le répertoire de travail de DSA, %systemroot%\ntds, ainsi qu'à la racine de tous les disques locaux ou amovibles.|  
  
|||  
|-|-|  
|**ID d’événement**|**2164**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Niveau de gravité**|Erreur|  
|**Message**|*<COMPUTERNAME>* Impossible de démarrer le service DsRoleSvc pour cloner le contrôleur de domaine virtuel local.|  
|**Notes de publication et de résolution**|Examinez les paramètres de service du service Serveur de rôles DS (DsRoleSvc) et assurez-vous que son type de démarrage est manuel. Vérifiez qu'aucun programme tiers n'empêche le démarrage de ce service.|  
  
|||  
|-|-|  
|**ID d’événement**|**2165**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Niveau de gravité**|Erreur|  
|**Message**|*<COMPUTERNAME>* Échec du démarrage d’un thread durant le clonage du contrôleur de domaine virtuel local.<br /><br />Code d'erreur : %1<br /><br />Message d'erreur : %2<br /><br />Nom du thread : %3|  
|**Notes de publication et de résolution**|Contactez les services de support technique Microsoft|  
  
|||  
|-|-|  
|**ID d’événement**|**2166**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Niveau de gravité**|Erreur|  
|**Message**|*<COMPUTERNAME>* a besoin du service RPCSS pour lancer le redémarrage en mode DSRM. L'attente de l'initialisation du service RPCSS à l'état de fonctionnement a échoué.<br /><br />Code d'erreur : %1|  
|**Notes de publication et de résolution**|Examinez le journal des événements Système et les paramètres du service Serveur RPC (Rpcss)|  
  
|||  
|-|-|  
|**ID d’événement**|**2167**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Niveau de gravité**|Erreur|  
|**Message**|*<COMPUTERNAME>* n’a pas pu initialiser les informations de contrôleur de domaine virtuel. Pour plus d'informations, voir l'entrée précédente du journal des événements.<br /><br />Données supplémentaires<br /><br />Code d'échec : %1|  
|**Notes de publication et de résolution**|Suivez les instructions du message, cette erreur est un fourre-tout.|  
  
|||  
|-|-|  
|**ID d’événement**|**2168**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Niveau de gravité**|Informationnel|  
|**Message**|Microsoft-Windows-ActiveDirectory_DomainService<br /><br />Le contrôleur de domaine s'exécute sur un hyperviseur pris en charge. ID de génération d'ordinateur virtuel détecté.<br /><br />Valeur actuelle de l‘ID de génération d’ordinateur virtuel : %1|  
|**Notes de publication et de résolution**|Il s'agit d'un événement de succès. Il ne représente un problème que s'il est inattendu.|  
  
|||  
|-|-|  
|**ID d’événement**|**2169**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Niveau de gravité**|Informationnel|  
|**Message**|Aucun ID de génération d'ordinateur virtuel n'a été détecté. Le contrôleur de domaine est hébergé sur une machine physique, une version de niveau inférieur d'Hyper-V ou un hyperviseur qui ne prend pas en charge l'ID de génération d'ordinateur virtuel.<br /><br />Données supplémentaires<br /><br />Code d'échec retourné durant la vérification de l'ID de génération d'ordinateur virtuel : %1|  
|**Notes de publication et de résolution**|Il s'agit d'un événement de succès si vous n'aviez pas l'intention d'effectuer de clonage. Sinon, examinez le journal des événements Système, ainsi que la documentation de support technique de l'hyperviseur.|  
  
|||  
|-|-|  
|**ID d’événement**|**2170**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Niveau de gravité**|Warning|  
|**Message**|Un changement d'ID de génération a été détecté.<br /><br />ID de génération mis en cache dans les services d'annuaire (ancienne valeur) : %1<br /><br />ID de génération actuellement dans l'ordinateur virtuel (nouvelle valeur) : %2<br /><br />Le changement d'ID de génération se produit après l'application d'une capture instantanée d'ordinateur virtuel, après une opération d'importation d'ordinateur virtuel ou après une opération de migration dynamique. *<COMPUTERNAME>* Crée un nouvel ID d’appel pour récupérer le contrôleur de domaine. Les contrôleurs de domaine virtualisés ne doivent pas être restaurés à l'aide de captures instantanées d'ordinateurs virtuels. La méthode prise en charge pour restaurer le contenu d'une base de données des services de domaine Active Directory consiste à restaurer une sauvegarde de l'état du système effectuée à l'aide d'une application de sauvegarde compatible avec les services de domaine Active Directory.|  
|**Notes de publication et de résolution**|Il s'agit d'un événement de succès si vous aviez l'intention d'effectuer un clonage. Sinon, examinez le journal des événements Système.|  
  
|||  
|-|-|  
|**ID d’événement**|**2171**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Niveau de gravité**|Informationnel|  
|**Message**|Aucun changement d'ID de génération n'a été détecté.<br /><br />ID de génération mis en cache dans les services d'annuaire (ancienne valeur) : %1<br /><br />ID de génération actuellement dans l'ordinateur virtuel (nouvelle valeur) : %2|  
|**Notes de publication et de résolution**|Il s'agit d'un événement de succès si vous n'aviez pas l'intention d'effectuer de clonage. Il doit être visible à chaque redémarrage d'un contrôleur de domaine virtualisé. Sinon, examinez le journal des événements Système.|  
  
|||  
|-|-|  
|**ID d’événement**|**2172**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Niveau de gravité**|Informationnel|  
|**Message**|Lisez l'attribut msDS-GenerationId de l'objet ordinateur du contrôleur de domaine.<br /><br />Valeur de l'attribut msDS-GenerationId : %1|  
|**Notes de publication et de résolution**|Il s'agit d'un événement de succès si vous aviez l'intention d'effectuer un clonage. Sinon, examinez le journal des événements Système.|  
  
|||  
|-|-|  
|**ID d’événement**|**2173**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Niveau de gravité**|Informationnel|  
|**Message**|Échec de lecture de l'attribut msDS-GenerationId de l'objet ordinateur du contrôleur de domaine. La raison peut en être un échec de transaction de base de données, ou l'absence d'ID de génération dans la base de données locale. L'attribut msDS-GenerationId n'existe pas durant le premier redémarrage après dcpromo ou le contrôleur de domaine n'est pas un contrôleur de domaine virtuel.<br /><br />Données supplémentaires<br /><br />Code d'échec : %1|  
|**Notes de publication et de résolution**|Il s'agit d'un événement de succès si vous aviez l'intention d'effectuer un clonage et si l'ordinateur virtuel redémarre pour la première fois après le clonage. Il peut également être ignoré sur les contrôleurs de domaine non virtuels. Sinon, examinez le journal des événements Système.|  
  
|||  
|-|-|  
|**ID d’événement**|**2174**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Niveau de gravité**|Informationnel|  
|**Message**|Le contrôleur de domaine n'est ni un clone du contrôleur de domaine virtuel, ni une capture instantanée de contrôleur de domaine virtuel restauré.|  
|**Notes de publication et de résolution**|Il s'agit d'un événement de succès si vous n'aviez pas l'intention d'effectuer de clonage. Sinon, examinez le journal des événements Système.|  
  
|||  
|-|-|  
|**ID d’événement**|**2175**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Niveau de gravité**|Erreur|  
|**Message**|Le fichier de configuration de clonage du contrôleur de domaine virtuel existe sur une plateforme non prise en charge.|  
|**Notes de publication et de résolution**|Cela se produit quand un fichier dccloneconfig.xml est trouvé mais pas l'ID de génération d'ordinateur virtuel. C'est le cas, par exemple, quand un fichier dccloneconfig.xml est trouvé sur un ordinateur physique ou sur un hyperviseur qui ne prend pas en charge l'ID de génération d'ordinateur virtuel.|  
  
|||  
|-|-|  
|**ID d’événement**|**2176**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Niveau de gravité**|Informationnel|  
|**Message**|Fichier de configuration de clonage du contrôleur de domaine virtuel renommé.<br /><br />Données supplémentaires<br /><br />Ancien nom de fichier : %1<br /><br />Nouveau nom de fichier : %2|  
|**Notes de publication et de résolution**|Changement de nom attendu au démarrage de la sauvegarde d'un ordinateur virtuel source, car l'ID de génération d'ordinateur virtuel n'a pas changé. Cela empêche le contrôleur de domaine source de tenter d'effectuer un clonage.|  
  
|||  
|-|-|  
|**ID d’événement**|**2177**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Niveau de gravité**|Erreur|  
|**Message**|Échec d'attribution d'un nouveau nom au fichier de configuration de clonage du contrôleur de domaine virtuel.<br /><br />Données supplémentaires<br /><br />Nom de fichier : %1<br /><br />Code d'échec : %2 %3|  
|**Notes de publication et de résolution**|Tentative de changement de nom attendue au démarrage de la sauvegarde d'un ordinateur virtuel source, car l'ID de génération d'ordinateur virtuel n'a pas changé. Cela empêche le contrôleur de domaine source de tenter d'effectuer un clonage. Renommez manuellement le fichier et examinez les produits tiers installés qui peuvent empêcher le changement de nom du fichier.|  
  
|||  
|-|-|  
|**ID d’événement**|**2178**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Niveau de gravité**|Informationnel|  
|**Message**|Fichier de configuration de clonage de contrôleur de domaine virtuel détecté, mais ID de génération d'ordinateur virtuel inchangé. Le contrôleur de domaine local est le contrôleur de domaine source du clonage. Renommez le fichier de configuration du clonage.|  
|**Notes de publication et de résolution**|Attendu au démarrage de la sauvegarde d'un ordinateur virtuel source, car l'ID de génération d'ordinateur virtuel n'a pas changé. Cela empêche le contrôleur de domaine source de tenter d'effectuer un clonage.|  
  
|||  
|-|-|  
|**ID d’événement**|**2179**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Niveau de gravité**|Informationnel|  
|**Message**|L'attribut msDS-GenerationId de l'objet ordinateur du contrôleur de domaine a été paramétré comme suit :<br /><br />Attribut GenerationID : %1|  
|**Notes de publication et de résolution**|Il s'agit d'un événement de succès. Il ne représente un problème que s'il est inattendu.|  
  
|||  
|-|-|  
|**ID d’événement**|**2180**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Niveau de gravité**|Warning|  
|**Message**|Échec de définition de l'attribut msDS-GenerationId de l'objet ordinateur du contrôleur de domaine.<br /><br />Données supplémentaires<br /><br />Code d'échec : %1|  
|**Notes de publication et de résolution**|Examinez le journal des événements Système et Dcpromo.log. Recherchez l'erreur spécifique dans Microsoft TechNet, La Base de connaissances Microsoft et les blogs Microsoft pour déterminer sa signification usuelle, puis résolvez le problème en fonction de ces informations.|  
  
|||  
|-|-|  
|**ID d’événement**|**2182**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Niveau de gravité**|Informationnel|  
|**Message**|Événement interne : Il a été demandé au service d'annuaire de cloner un agent de système d'annuaire (DSA, Directory System Agent) à distance :|  
|**Notes de publication et de résolution**|Il s'agit d'un événement de succès. Il ne représente un problème que s'il est inattendu.|  
  
|||  
|-|-|  
|**ID d’événement**|**2183**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Niveau de gravité**|Informationnel|  
|**Message**|Événement interne : *<COMPUTERNAME>* a effectué la demande de clonage de l’Agent de système d’annuaire distant.<br /><br />Nom initial du contrôleur de domaine : %3<br /><br />Nom du contrôleur de domaine pour la demande de clonage : %4<br /><br />Site du contrôleur de domaine pour la demande de clonage : %5<br /><br />Données supplémentaires<br /><br />Valeur d'erreur : %1 %2|  
|**Notes de publication et de résolution**|Il s'agit d'un événement de succès. Il ne représente un problème que s'il est inattendu.|  
  
|||  
|-|-|  
|**ID d’événement**|**2184**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Niveau de gravité**|Erreur|  
|**Message**|*<COMPUTERNAME>* Échec de création d’un compte de contrôleur de domaine pour le contrôleur de domaine cloné.<br /><br />Nom initial du contrôleur de domaine : %1<br /><br />Nombre autorisé de contrôleurs de domaine clonés : %2<br /><br />La limite du nombre de comptes de contrôleur de domaine qui peuvent être générés par clonage *<COMPUTERNAME>* a été dépassé.|  
|**Notes de publication et de résolution**|Un seul nom de contrôleur de domaine source ne peut se générer automatiquement que 9 999 fois, si les contrôleurs de domaine ne sont pas rétrogradés, en fonction de la convention d'affectation de noms. Utilisez l'élément <computername> du code XML pour générer un nouveau nom ou clone unique à partir d'un contrôleur de domaine nommé de manière distincte.|  
  
|||  
|-|-|  
|**ID d’événement**|**2191**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Niveau de gravité**|Informationnel|  
|**Message**|*<COMPUTERNAME>* Définissez la valeur de Registre suivante pour désactiver les mises à jour DNS.<br /><br />Clé de Registre : %1<br /><br />Valeur de Registre : %2<br /><br />Données de valeur de Registre : %3<br /><br />Durant le processus de clonage, l'ordinateur local peut temporairement porter le même nom d'ordinateur que l'ordinateur source du clonage. Les inscriptions d'enregistrement DNS A et AAAA sont désactivées pendant cette période. Ainsi, les clients ne peuvent pas envoyer de demandes à l'ordinateur local qui fait l'objet du clonage. Le processus de clonage réactivera les mises à jour DNS quand le clonage sera terminé.|  
|**Notes de publication et de résolution**|Il s'agit d'un événement de succès. Il ne représente un problème que s'il est inattendu.|  
  
|||  
|-|-|  
|**ID d’événement**|**2192**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Niveau de gravité**|Erreur|  
|**Message**|*<COMPUTERNAME>* Échec de définition de la valeur de Registre suivante pour désactiver les mises à jour DNS.<br /><br />Clé de Registre : %1<br /><br />Valeur de Registre : %2<br /><br />Données de valeur de Registre : %3<br /><br />Code d’erreur : %4<br /><br />Message d’erreur : %5<br /><br />Durant le processus de clonage, l'ordinateur local peut temporairement porter le même nom d'ordinateur que l'ordinateur source du clonage. Les inscriptions d'enregistrement DNS A et AAAA sont désactivées pendant cette période. Ainsi, les clients ne peuvent pas envoyer de demandes à l'ordinateur local qui fait l'objet du clonage.|  
|**Notes de publication et de résolution**|Examinez les journaux des événements Application et Système. Vérifiez si une application tierce ne bloque pas les mises à jour du Registre.|  
  
|||  
|-|-|  
|**ID d’événement**|**2193**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Niveau de gravité**|Informationnel|  
|**Message**|*<COMPUTERNAME>* Définissez la valeur de Registre suivante pour activer les mises à jour DNS.<br /><br />Clé de Registre : %1<br /><br />Valeur de Registre : %2<br /><br />Données de valeur de Registre : %3<br /><br />Durant le processus de clonage, l'ordinateur local peut temporairement porter le même nom d'ordinateur que l'ordinateur source du clonage. Les inscriptions d'enregistrement DNS A et AAAA sont désactivées pendant cette période. Ainsi, les clients ne peuvent pas envoyer de demandes à l'ordinateur local qui fait l'objet du clonage.|  
|**Notes de publication et de résolution**|Il s'agit d'un événement de succès. Il ne représente un problème que s'il est inattendu.|  
  
|||  
|-|-|  
|**ID d’événement**|**2194**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Niveau de gravité**|Erreur|  
|**Message**|*<COMPUTERNAME>* Échec de définition de la valeur de Registre suivante pour activer les mises à jour DNS.<br /><br />Clé de Registre : %1<br /><br />Valeur de Registre : %2<br /><br />Données de valeur de Registre : %3<br /><br />Code d’erreur : %4<br /><br />Message d’erreur : %5<br /><br />Durant le processus de clonage, l'ordinateur local peut temporairement porter le même nom d'ordinateur que l'ordinateur source du clonage. Les inscriptions d'enregistrement DNS A et AAAA sont désactivées pendant cette période. Ainsi, les clients ne peuvent pas envoyer de demandes à l'ordinateur local qui fait l'objet du clonage.|  
|**Notes de publication et de résolution**|Examinez les journaux des événements Application et Système. Vérifiez si une application tierce ne bloque pas les mises à jour du Registre.|  
  
|||  
|-|-|  
|**ID d’événement**|**2195**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Niveau de gravité**|Erreur|  
|**Message**|Échec du mode de démarrage DSRM.<br /><br />Code d'erreur : %1<br /><br />Message d'erreur : %2<br /><br />Quand le clonage du contrôleur de domaine virtuel a échoué ou qu'un fichier de configuration de clonage de contrôleur de domaine virtuel est présent sur un hyperviseur non pris en charge, la machine locale redémarre en mode DSRM pour permettre la résolution des problèmes. Échec du mode de démarrage DSRM.|  
|**Notes de publication et de résolution**|Examinez les journaux des événements Application et Système. Vérifiez si une application tierce ne bloque pas les mises à jour du Registre.|  
  
|||  
|-|-|  
|**ID d’événement**|**2196**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Niveau de gravité**|Erreur|  
|**Message**|Échec d'activation du privilège d'arrêt.<br /><br />Code d'erreur : %1<br /><br />Message d'erreur : %2<br /><br />Quand le clonage du contrôleur de domaine virtuel a échoué ou qu'un fichier de configuration de clonage de contrôleur de domaine virtuel est présent sur un hyperviseur non pris en charge, la machine locale redémarre en mode DSRM pour permettre la résolution des problèmes. Échec d'activation du privilège d'arrêt.|  
|**Notes de publication et de résolution**|Examinez les journaux des événements Application et Système. Vérifiez si une application tierce ne bloque pas l'utilisation des privilèges.|  
  
|||  
|-|-|  
|**ID d’événement**|**2197**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Niveau de gravité**|Erreur|  
|**Message**|Échec de démarrage de l'arrêt du système.<br /><br />Code d'erreur : %1<br /><br />Message d'erreur : %2<br /><br />Quand le clonage du contrôleur de domaine virtuel a échoué ou qu'un fichier de configuration de clonage de contrôleur de domaine virtuel est présent sur un hyperviseur non pris en charge, la machine locale redémarre en mode DSRM pour permettre la résolution des problèmes. Échec de démarrage de l'arrêt du système.|  
|**Notes de publication et de résolution**|Examinez les journaux des événements Application et Système. Vérifiez si une application tierce ne bloque pas l'utilisation des privilèges.|  
  
|||  
|-|-|  
|**ID d’événement**|**2198**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Niveau de gravité**|Erreur|  
|**Message**|*<COMPUTERNAME>* Impossible de créer ou modifier l’objet contrôleur de domaine cloné suivant.<br /><br />Données supplémentaires :<br /><br />Objet :<br /><br />%1<br /><br />Valeur de l’erreur : %2<br /><br />%3|  
|**Notes de publication et de résolution**|Recherchez l'erreur spécifique dans Microsoft TechNet, La Base de connaissances Microsoft et les blogs Microsoft pour déterminer sa signification usuelle, puis résolvez le problème en fonction de ces informations.|  
  
|||  
|-|-|  
|**ID d’événement**|**2199**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Niveau de gravité**|Erreur|  
|**Message**|*<COMPUTERNAME>* Échec de création de l’objet contrôleur de domaine cloné suivant, car l’objet existe déjà.<br /><br />Données supplémentaires :<br /><br />Contrôleur de domaine source :<br /><br />%1<br /><br />Objet :<br /><br />%2|  
|**Notes de publication et de résolution**|Soit la validation du fichier dccloneconfig.xml n'a pas spécifié de contrôleur de domaine existant, soit les copies du fichier dccloneconfig.xml ont été utilisées sur plusieurs clones sans modification du nom. Si la collision est toujours inattendue, déterminez quel est l'administrateur à l'origine de la promotion. Contactez-le pour déterminer si le contrôleur de domaine existant doit être rétrogradé, si les métadonnées du contrôleur de domaine existant doivent être nettoyées ou si le clone doit utiliser un autre nom.|  
  
|||  
|-|-|  
|**ID d’événement**|**2203**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Niveau de gravité**|Erreur|  
|**Message**|Le clonage du dernier contrôleur de domaine virtuel a échoué. Le redémarrage qui vient de s'effectuer est le premier depuis cet échec. Cependant, aucun fichier de configuration de clonage de contrôleur de domaine virtuel et aucun changement d'ID de génération d'ordinateur virtuel n'ont été détectés. Démarrez en mode DSRM.<br /><br />Le clonage du dernier contrôleur de domaine virtuel a échoué : %1<br /><br />Le fichier de configuration de clonage de contrôleur de domaine virtuel existe : %2<br /><br />Un changement d'ID de génération d'ordinateur virtuel a été détecté : %3|  
|**Notes de publication et de résolution**|Attendu si le clonage a échoué précédemment, en raison d'un fichier dccloneconfig.xml manquant ou non valide|  
  
|||  
|-|-|  
|ID d’événement|2210|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|Sévérité|Erreur|  
|Message|<COMPUTERNAME> n'a pas pu créer d'objets pour le contrôleur de domaine clone.<br /><br />Données supplémentaires :<br /><br />ID de clone : %6<br /><br />Nom du contrôleur de domaine clone : %1<br /><br />Boucle de reprise : %2<br /><br />Valeur d’exception : %3<br /><br />Valeur d’erreur : %4<br /><br />DSID : %5|  
|Remarques et résolution|Examinez les journaux des événements Système et Services d'annuaire, ainsi que le fichier dcpromo.log, pour plus d'informations sur les raisons de l'échec du clonage.|  
  
|||  
|-|-|  
|ID d’événement|2211|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|Sévérité|Informationnel|  
|Message|<COMPUTERNAME> a créé des objets pour le contrôleur de domaine clone.<br /><br />Données supplémentaires :<br /><br />ID de clone : %3<br /><br />Nom du contrôleur de domaine clone : %1<br /><br />Boucle de reprise : %2|  
|Remarques et résolution|Il s'agit d'un événement de succès. Il ne représente un problème que s'il est inattendu.|  
  
|||  
|-|-|  
|ID d’événement|2212|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|Sévérité|Informationnel|  
|Message|<COMPUTERNAME> a commencé à créer des objets pour le contrôleur de domaine clone.<br /><br />Données supplémentaires :<br /><br />ID de clone : %1<br /><br />Nom du clone : %2<br /><br />Site clone : %3<br /><br />Contrôleur de domaine en lecture seule clone : %4|  
|Remarques et résolution|Il s'agit d'un événement de succès. Il ne représente un problème que s'il est inattendu.|  
  
|||  
|-|-|  
|ID d’événement|2213|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|Sévérité|Informationnel|  
|Message|<COMPUTERNAME> a créé un objet KrbTgt pour le clonage du contrôleur de domaine en lecture seule.<br /><br />Données supplémentaires :<br /><br />ID de clone : %1<br /><br />GUID du nouvel objet KrbTgt : %2|  
|Remarques et résolution|Il s'agit d'un événement de succès. Il ne représente un problème que s'il est inattendu.|  
  
|||  
|-|-|  
|ID d’événement|2214|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|Sévérité|Informationnel|  
|Message|<COMPUTERNAME> va créer un objet ordinateur pour le contrôleur de domaine clone.<br /><br />Données supplémentaires :<br /><br />ID de clone : %1<br /><br />Contrôleur de domaine initial : %2<br /><br />Contrôleur de domaine clone : %3|  
|Remarques et résolution|Il s'agit d'un événement de succès. Il ne représente un problème que s'il est inattendu.|  
  
|||  
|-|-|  
|ID d’événement|2215|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|Sévérité|Informationnel|  
|Message|<COMPUTERNAME> va ajouter le contrôleur de domaine clone au site suivant.<br /><br />Données supplémentaires :<br /><br />ID de clone : %1<br /><br />Site : %2|  
|Remarques et résolution|Il s'agit d'un événement de succès. Il ne représente un problème que s'il est inattendu.|  
  
|||  
|-|-|  
|ID d’événement|2216|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|Sévérité|Informationnel|  
|Message|<COMPUTERNAME> va créer un conteneur de serveurs pour le contrôleur de domaine clone.<br /><br />Données supplémentaires :<br /><br />ID de clone : %1<br /><br />Conteneur de serveurs : %2|  
|Remarques et résolution|Il s'agit d'un événement de succès. Il ne représente un problème que s'il est inattendu.|  
  
|||  
|-|-|  
|ID d’événement|2217|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|Sévérité|Informationnel|  
|Message|<COMPUTERNAME> va créer un objet serveur pour le contrôleur de domaine clone.<br /><br />Données supplémentaires :<br /><br />ID de clone : %1<br /><br />Objet serveur : %2|  
|Remarques et résolution|Il s'agit d'un événement de succès. Il ne représente un problème que s'il est inattendu.|  
  
|||  
|-|-|  
|ID d’événement|2218|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|Sévérité|Informationnel|  
|Message|<COMPUTERNAME> va créer un objet paramètres NTDS pour le contrôleur de domaine clone.<br /><br />Données supplémentaires :<br /><br />ID de clone : %1<br /><br />Objet : %2|  
|Remarques et résolution|Il s'agit d'un événement de succès. Il ne représente un problème que s'il est inattendu.|  
  
|||  
|-|-|  
|ID d’événement|2219|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|Sévérité|Informationnel|  
|Message|<COMPUTERNAME> va créer des objets connexion pour le contrôleur de domaine en lecture seule clone.<br /><br />Données supplémentaires :<br /><br />ID de clone : %1|  
|Remarques et résolution|Il s'agit d'un événement de succès. Il ne représente un problème que s'il est inattendu.|  
  
|||  
|-|-|  
|ID d’événement|2220|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|Sévérité|Informationnel|  
|Message|<COMPUTERNAME> va créer des objets SYSVOL pour le contrôleur de domaine en lecture seule clone.<br /><br />Données supplémentaires :<br /><br />ID de clone : %1|  
|Remarques et résolution|Il s'agit d'un événement de succès. Il ne représente un problème que s'il est inattendu.|  
  
|||  
|-|-|  
|ID d’événement|2221|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|Sévérité|Erreur|  
|Message|<COMPUTERNAME> n'a pas pu générer de mot de passe aléatoire pour le contrôleur de domaine cloné.<br /><br />Données supplémentaires :<br /><br />ID de clone : %1<br /><br />Nom du contrôleur de domaine clone : %2<br /><br />Erreur : %3 %4|  
|Remarques et résolution|Examinez le journal des événements Système pour plus d'informations sur la raison pour laquelle le mot de passe du compte d'ordinateur n'a pas pu être créé.|  
  
|||  
|-|-|  
|ID d’événement|2222|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|Sévérité|Erreur|  
|Message|<COMPUTERNAME> n'a pas pu définir de mot de passe pour le contrôleur de domaine cloné.<br /><br />Données supplémentaires :<br /><br />ID de clone : %1<br /><br />Nom du contrôleur de domaine clone : %2<br /><br />Erreur : %3 %4|  
|Remarques et résolution|Examinez le journal des événements Système pour plus d'informations sur la raison pour laquelle le mot de passe du compte d'ordinateur n'a pas pu être défini.|  
  
|||  
|-|-|  
|ID d’événement|2223|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|Sévérité|Informationnel|  
|Message|<COMPUTERNAME> a défini un mot de passe de compte d'ordinateur pour le contrôleur de domaine cloné.<br /><br />Données supplémentaires :<br /><br />ID de clone : %1<br /><br />Nom du contrôleur de domaine clone : %2<br /><br />Nombre total de tentatives : %3|  
|Remarques et résolution|Il s'agit d'un événement de succès. Il ne représente un problème que s'il est inattendu.|  
  
|||  
|-|-|  
|ID d’événement|2224|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|Sévérité|Erreur|  
|Message|Échec du clonage du contrôleur de domaine virtuel. Le ou les comptes de service administrés %1 suivants existent sur l'ordinateur cloné :<br /><br />%2<br /><br />Pour que le clonage réussisse, tous les comptes de service administrés doivent être supprimés. Pour ce faire, utilisez l'applet de commande PowerShell Remove-ADComputerServiceAccount.|  
|Remarques et résolution|Attendu durant l'utilisation des comptes de service administrés autonomes (mais pas les comptes de service administrés de groupe). Ne suivez *pas* le conseil associé à l'événement et qui consiste à supprimer le compte. Il s'agit d'une erreur. Use Uninstall-AdServiceAccount - [https://technet.microsoft.com/library/hh852310](https://technet.microsoft.com/library/hh852310).<br /><br />Les comptes de service administrés autonomes (apparus avec Windows Server 2008 R2) ont été remplacés dans Windows Server 2012 par les comptes de service administrés de groupe. Les comptes de service administrés de groupe prennent en charge le clonage.|  
  
|||  
|-|-|  
|ID d’événement|2225|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|Sévérité|Informationnel|  
|Message|Les secrets mis en cache du principal de sécurité suivant ont été supprimés du contrôleur de domaine local :<br /><br />%1<br /><br />Après avoir cloné un contrôleur de domaine en lecture seule, les secrets auparavant mis en cache sur le contrôleur en lecture seule à la source du clonage seront supprimés du contrôleur cloné.|  
|Remarques et résolution|Il s'agit d'un événement de succès. Il ne représente un problème que s'il est inattendu.|  
  
|||  
|-|-|  
|ID d’événement|2226|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|Sévérité|Erreur|  
|Message|Impossible de supprimer les secrets mis en cache du principal de sécurité suivant à partir du contrôleur de domaine local :<br /><br />%1<br /><br />Erreur : %2 (%3)<br /><br />Après avoir cloné un contrôleur de domaine en lecture seule, les secrets mis en cache auparavant sur le contrôleur en lecture seule à la source du clonage doivent être supprimés du clone. Ne pas effectuer cette étape augmente le risque qu'une personne malveillante obtienne ces informations d'identification à partir d'un clone volé ou détourné. Si le principal de sécurité correspond à un compte à privilèges élevés et doit être protégé contre ce type d'attaque, utilisez l'opération rootDSE rODCPurgeAccount pour effacer manuellement ses secrets sur le contrôleur de domaine local.|  
|Remarques et résolution|Pour plus d'informations, examinez les journaux des événements Système et Services d'annuaire.|  
  
|||  
|-|-|  
|ID d’événement|2227|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|Sévérité|Erreur|  
|Message|Une exception s'est déclenchée lors de la tentative de suppression des secrets mis en cache du contrôleur de domaine local.<br /><br />Données supplémentaires :<br /><br />Valeur d’exception : %1<br /><br />Valeur de l’erreur : %2<br /><br />DSID : %3<br /><br />Après avoir cloné un contrôleur de domaine en lecture seule, les secrets mis en cache auparavant sur le contrôleur en lecture seule à la source du clonage doivent être supprimés du clone. Ne pas effectuer cette étape augmente le risque qu'une personne malveillante obtienne ces informations d'identification à partir d'un clone volé ou détourné. Si l'un de ces principaux de sécurité correspond à un compte à privilèges élevés et doit être protégé contre ce type d'attaque, utilisez l'opération rootDSE rODCPurgeAccount pour effacer manuellement ses secrets sur le contrôleur de domaine local.|  
|Remarques et résolution|Pour plus d'informations, examinez les journaux des événements Système et Services d'annuaire.|  
  
|||  
|-|-|  
|ID d’événement|2228|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|Sévérité|Erreur|  
|Message|L'ID de génération de l'ordinateur virtuel, stocké dans la base de données Active Directory de ce contrôleur de domaine, diffère de sa valeur active. Cependant, aucun fichier de configuration de clonage de contrôleur de domaine virtuel (DCCloneConfig.xml) n'a pu être trouvé. Le clonage du contrôleur de domaine n'a donc pas été tenté. Si une opération de clonage a été tentée, assurez-vous qu'un fichier DCCloneConfig.xml est fourni dans l'un des emplacements pris en charge. De plus, l'adresse IP de ce contrôleur de domaine entre en conflit avec celle d'un autre contrôleur. Pour s'assurer qu'aucune interruption de service ne se produit, le contrôleur de domaine a été configuré afin de démarrer en mode de restauration des services d'annuaire (DSRM).<br /><br />Données supplémentaires :<br /><br />Adresse IP en double : %1|  
|Remarques et résolution|Ce mécanisme de protection arrête les contrôleurs de domaine dupliqués quand cela est possible (cela n'est pas le cas avec le protocole DHCP, par exemple). Ajoutez un fichier DcCloneConfig.xml valide, supprimez l'indicateur DSRM, puis réessayez le clonage.|  
  
|||  
|-|-|  
|ID d’événement|29218|  
|Source|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Sévérité|Erreur|  
|Message|Échec du clonage du contrôleur de domaine virtuel. L'opération de clonage n'a pas pu se terminer et le contrôleur de domaine cloné a été redémarré en mode de restauration des services d'annuaire (DSRM).<br /><br />Passez en revue les événements consignés dans les journaux et le fichier %systemroot%\debug\dcpromo.log pour plus d'informations sur les erreurs qui correspondent à la tentative de clonage du contrôleur de domaine virtuel et pour savoir si cette image de clone est réutilisable.<br /><br />Si une ou plusieurs entrées de journal indiquent qu'il est impossible de retenter le processus de clonage, l'image doit être détruite de manière sécurisée. Sinon, vous pouvez corriger les erreurs, effacer l'indicateur de démarrage DSRM, puis redémarrer normalement. Après le redémarrage, l'opération de clonage est relancée.|  
|Remarques et résolution|Examinez les journaux des événements Système et Services d'annuaire, ainsi que le fichier dcpromo.log, pour plus d'informations sur les raisons de l'échec du clonage.|  
  
|||  
|-|-|  
|ID d’événement|29219|  
|Source|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Sévérité|Informationnel|  
|Message|Le clonage du contrôleur de domaine virtuel a réussi.|  
|Remarques et résolution|Il s'agit d'un événement de succès. Il ne représente un problème que s'il est inattendu.|  
  
|||  
|-|-|  
|ID d’événement|29248|  
|Source|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Sévérité|Erreur|  
|Message|Le clonage du contrôleur de domaine virtuel n'a pas pu obtenir la notification Winlogon. Le code d'erreur retourné est %1 (%2).<br /><br />Pour plus d'informations sur cette erreur, consultez %systemroot%\debug\dcpromo.log pour déterminer les erreurs qui correspondent à la tentative de clonage d'un contrôleur de domaine virtuel.|  
|Remarques et résolution|Contactez les services de support technique Microsoft|  
  
|||  
|-|-|  
|ID d’événement|29249|  
|Source|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Sévérité|Erreur|  
|Message|Le clonage du contrôleur de domaine virtuel n'a pas pu analyser le fichier de configuration du contrôleur de domaine virtuel.<br /><br />Le code HRESULT retourné est %1.<br /><br />Le fichier de configuration est :%2<br /><br />Corrigez les erreurs dans le fichier de configuration et réessayez l'opération de clonage.<br /><br />Pour plus d'informations sur cette erreur, consultez %systemroot%\debug\dcpromo.log.|  
|Remarques et résolution|Examinez le fichier dclconeconfig.xml en recherchant les erreurs de syntaxe à l'aide d'un éditeur XML, ainsi que le fichier de schéma DCCloneConfigSchema.xsd.|  
  
|||  
|-|-|  
|ID d’événement|29250|  
|Source|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Sévérité|Erreur|  
|Message|Échec du clonage du contrôleur de domaine virtuel. Des logiciels ou des services actuellement activés sur le contrôleur de domaine virtuel cloné ne sont pas présents dans la liste des applications autorisées pour le clonage du contrôleur de domaine virtuel.<br /><br />Les entrées manquantes sont reprises ci-après :<br /><br />%2<br /><br />%1 (le cas échéant) a servi de liste d'inclusion définie.<br /><br />L'opération de clonage ne peut pas se terminer si des applications ne prenant pas en charge le clonage sont installées.<br /><br />Exécutez l'applet de commande PowerShell Active Directory Get-ADDCCloningExcludedApplicationList pour déterminer les applications qui sont installées sur l'ordinateur cloné sans être incluses dans la liste des applications autorisées, puis ajoutez-les à cette liste si elles sont compatibles avec le clonage de contrôleur de domaine virtuel. Si des applications sont incompatibles, désinstallez-les avant de relancer le clonage.<br /><br />Le processus de clonage de contrôleur de domaine virtuel recherche le fichier de la liste des applications autorisées, du nom de CustomDCCloneAllowList.xml, selon l'ordre de recherche ci-après. Le premier fichier trouvé est utilisé et les suivants sont ignorés :<br /><br />1. Nom de la valeur dans le Registre : HKey_Local_Machine\System\CurrentControlSet\Services\NTDS\Parameters\AllowListFolder<br /><br />2. Même répertoire que celui où réside le dossier du répertoire de travail de DSA<br /><br />3. %windir%\NTDS<br /><br />4. Support amovible lisible/inscriptible dans l'ordre de la lettre de lecteur à la racine du lecteur|  
|Remarques et résolution|Suivez les instructions du message.|  
  
|||  
|-|-|  
|ID d’événement|29251|  
|Source|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Sévérité|Erreur|  
|Message|Le clonage du contrôleur de domaine virtuel n'a pas pu réinitialiser les adresses IP de l'ordinateur clone.<br /><br />Le code d'erreur retourné est %1 (%2).<br /><br />Cette erreur peut être due à une mauvaise configuration des sections de configuration réseau dans le fichier de configuration du contrôleur de domaine virtuel.<br /><br />Pour plus d'informations sur les erreurs qui correspondent à la réinitialisation des adresses IP lors de tentatives de clonage d'un contrôleur de domaine virtuel, consultez %systemroot%\debug\dcpromo.log.<br /><br />Vous trouverez plus d’informations sur la réinitialisation des adresses IP sur l’ordinateur cloné à https://go.microsoft.com/fwlink/?LinkId=208030|  
|Remarques et résolution|Assurez-vous que les informations IP définies dans le fichier dccloneconfig.xml sont valides et qu'elles ne représentent pas un doublon de la machine source d'origine.|  
  
|||  
|-|-|  
|ID d’événement|29253|  
|Source|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Sévérité|Erreur|  
|Message|Échec du clonage du contrôleur de domaine virtuel. Le contrôleur de domaine clone n'a pas pu localiser le maître d'opérations du contrôleur de domaine principal dans le domaine d'accueil de l'ordinateur cloné.<br /><br />Le code d'erreur retourné est %1 (%2).<br /><br />Vérifiez que le contrôleur de domaine principal dans le domaine d'accueil de l'ordinateur cloné est affecté à un contrôleur de domaine actif, est en ligne et est opérationnel. Vérifiez que l'ordinateur cloné dispose d'une connectivité LDAP/RPC au contrôleur de domaine principal sur les ports et les protocoles requis.|  
|Remarques et résolution|Assurez-vous que les informations d'adresse IP et DNS sont définies pour le contrôleur de domaine cloné. Utilisez Dcdiag.exe locatorcheck pour vérifier si l’ECDP est en ligne, utilisez Nltest.exe/Server :*<PDCE>* /dclist :*<domain>* à valide RPC, obtenez une capture réseau de l’ECDP lors de la le clonage échoue et analyser le trafic.|  
  
|||  
|-|-|  
|ID d’événement|29254|  
|Source|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Sévérité|Erreur|  
|Message|Échec de la liaison du clonage du contrôleur de domaine virtuel au contrôleur de domaine principal %1.<br /><br />Le code d’erreur retourné est %2 (%3).<br /><br />Vérifiez que le contrôleur de domaine principal %1 est en ligne et opérationnel. Vérifiez que l'ordinateur cloné dispose d'une connectivité LDAP/RPC au contrôleur de domaine principal sur les ports et les protocoles requis.|  
|Remarques et résolution|Assurez-vous que les informations d'adresse IP et DNS sont définies pour le contrôleur de domaine cloné. Utilisez Dcdiag.exe locatorcheck pour vérifier si l’ECDP est en ligne, utilisez Nltest.exe/Server :*<PDCE>* /dclist :*<domain>* à valide RPC, obtenez une capture réseau de l’ECDP lors de la le clonage échoue et analyser le trafic.|  
  
|||  
|-|-|  
|ID d’événement|29255|  
|Source|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Sévérité|Erreur|  
|Message|Échec du clonage du contrôleur de domaine virtuel.<br /><br />Une tentative de création d'objets sur le contrôleur de domaine principal %1 requis pour l'image clonée a retourné l'erreur %2 (%3).<br /><br />Vérifiez que le contrôleur de domaine cloné possède les privilèges nécessaires pour se cloner lui-même. Vérifiez les événements associés dans le journal d'événements du service d'annuaire sur le contrôleur de domaine principal %1.|  
|Remarques et résolution|Recherchez l'erreur spécifique dans Microsoft TechNet, La Base de connaissances Microsoft et les blogs Microsoft pour déterminer sa signification usuelle, puis résolvez le problème en fonction de ces informations.|  
  
|||  
|-|-|  
|ID d’événement|29256|  
|Source|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Sévérité|Erreur|  
|Message|Échec de la tentative de définition de l'indicateur Démarrage en mode restauration des services d'annuaire avec le code d'erreur %1.<br /><br />Pour plus d'informations sur les erreurs, voir %systemroot%\debug\dcpromo.log.|  
|Remarques et résolution|Pour plus d'informations, examinez le journal des services d'annuaire et dcpromo.log. Examinez les journaux des événements Application et Système. Vérifiez si une application tierce ne bloque pas l'utilisation des privilèges.|  
  
|||  
|-|-|  
|ID d’événement|29257|  
|Source|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Sévérité|Erreur|  
|Message|Le clonage du contrôleur de domaine virtuel a été effectué. Une tentative pour redémarrer l'ordinateur a échoué avec le code d'erreur %1.<br /><br />Redémarrez l'ordinateur pour terminer l'opération de clonage.|  
|Remarques et résolution|Examinez les journaux des événements Application et Système. Vérifiez si une application tierce ne bloque pas l'utilisation des privilèges.|  
  
|||  
|-|-|  
|ID d’événement|29264|  
|Source|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Sévérité|Erreur|  
|Message|Échec de la tentative d'effacement de l'indicateur Démarrage en mode restauration des services d'annuaire avec le code d'erreur %1.<br /><br />Pour plus d'informations sur les erreurs, voir %systemroot%\debug\dcpromo.log.|  
|Remarques et résolution|Pour plus d'informations, examinez le journal des services d'annuaire et dcpromo.log. Examinez les journaux des événements Application et Système. Vérifiez si une application tierce ne bloque pas l'utilisation des privilèges.|  
  
|||  
|-|-|  
|ID d’événement|29265|  
|Source|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Sévérité|Informationnel|  
|Message|Le clonage du contrôleur de domaine virtuel a réussi. Le fichier de configuration de clonage de contrôleur de domaine virtuel %1 a été renommé en %2.|  
|Remarques et résolution|Non applicable, il s'agit d'un événement de succès.|  
  
|||  
|-|-|  
|ID d’événement|29266|  
|Source|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Sévérité|Erreur|  
|Message|Le clonage du contrôleur de domaine virtuel a réussi. La tentative de renommage du fichier de configuration de clonage de contrôleur de domaine virtuel %1 a échoué avec le code d'erreur %2 (%3).|  
|Remarques et résolution|Renommez manuellement le fichier dccloneconfig.xml.|  
  
|||  
|-|-|  
|ID d’événement|29267|  
|Source|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Sévérité|Erreur|  
|Message|Échec de la vérification de la liste autorisée des applications pour le clonage du contrôleur de domaine virtuel.<br /><br />Le code d'erreur retourné est %1 (%2).<br /><br />Cette erreur peut être due à un problème de syntaxe dans le fichier liste d’autorisation de clonage (fichier défini : %3). Pour plus d'informations sur cette erreur, consultez %systemroot%\debug\dcpromo.log.|  
|Remarques et résolution|Suivez les instructions de l'événement.|  
  
##### <a name="error-messages"></a>Messages d'erreur  
Il n'y a pas d'erreurs interactives directes pour les échecs de clonage de contrôleur de domaine virtualisé. Toutes les informations relatives au clonage sont enregistrées dans les journaux Système et Services d'annuaire, ainsi que dans les journaux de promotion du contrôleur de domaine dans dcpromo.log. Toutefois, si le serveur démarre en mode de restauration des services d'annuaire (DSRM), enquêtez immédiatement, car la promotion ou le clonage ont échoué.  
  
Examinez en premier lieu le fichier dcpromo.log à la recherche d'un échec du clonage. En fonction du problème listé, il peut être nécessaire d'examiner ensuite les journaux Services d'annuaire et Système pour approfondir les diagnostics.  
  
#### <a name="known-issues-and-support-scenarios"></a>Problèmes connus et scénarios de prise en charge  
Vous trouverez ci-dessous les problèmes courants rencontrés pendant le processus de développement de Windows Server 2012. Tous ces problèmes sont « normaux » et il existe une solution de contournement valide ou une technique plus appropriée pour les éviter initialement. Certains d'entre eux seront éventuellement résolus dans les prochaines versions de Windows Server 2012.  
  
|||  
|-|-|  
|**Problème**|**Échec du clonage, DSRM**|  
|**Symptômes**|Le clone démarre en mode de restauration des services d'annuaire|  
|**Résolution et remarques**|Validez toutes les étapes suivies à partir des sections Déploiement d'un contrôleur de domaine virtualisé et [Méthodologie générale pour la résolution des problèmes de clonage d'un contrôleur de domaine](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_GeneralMethodology)<br /><br />Description dans l'article de la Base de connaissances n° 2742844.|  
  
|||  
|-|-|  
|**Problème**|**Baux IP supplémentaires lorsque vous utilisez DHCP pour cloner**|  
|**Symptômes**|Après le clonage réussi d'un contrôleur de domaine à l'aide du protocole DHCP, le premier démarrage du clone entraîne l'attribution d'un bail DHCP. Quand le serveur est renommé et qu'il redémarre en tant que contrôleur de domaine, un second bail DHCP est attribué. La première adresse IP n'est pas libérée et vous vous retrouvez avec un bail « fantôme ».|  
|**Résolution et remarques**|Supprimez manuellement le bail d'adresse inutilisé du protocole DHCP ou laissez-le expirer normalement. Description dans l’article de la Base de connaissances n°2742836.|  
  
|||  
|-|-|  
|**Problème**|**Le clonage échoue en mode DSRM après une très longue attente**|  
|**Symptômes**|Le clonage semble suspendu au niveau de l'étape « Le clonage du contrôleur de domaine est achevé à X% » pendant 8 à 15 minutes. Après cela, le clonage échoue et entraîne un démarrage en mode DSRM.|  
|**Résolution et remarques**|L'ordinateur cloné ne peut pas obtenir d'adresse IP dynamique de la part du serveur DHCP ou SLAAC, il utilise une adresse IP en double ou ne parvient pas à trouver le contrôleur de domaine principal. Les multiples tentatives effectuées par le processus de clonage entraînent un retard. Résolvez le problème réseau pour permettre le clonage.<br /><br />Description dans l'article de la Base de connaissances n° 2742844.|  
  
|||  
|-|-|  
|**Problème**|**Le clonage ne recrée pas tous les noms de principal du service**|  
|**Symptômes**|Si un ensemble de noms de principal du service (SPN, Service Principal Names) *en trois parties* comprend un nom NetBIOS avec un port, ainsi qu'un nom NetBIOS identique sans port, l'entrée sans port n'est pas recréée avec le nouveau nom d'ordinateur. Exemple :<br /><br />customspn/DC1:200/app1 UTILISATION NON VALIDE DE SYMBOLES *(ceci est recréé avec le nouveau nom d'ordinateur)*<br /><br />customspn/DC1/app1 UTILISATION NON VALIDE DE SYMBOLES *(ceci est recréé avec le nouveau nom d'ordinateur)*<br /><br />Les noms complets sont recréés et les noms de principal du service qui n'ont pas trois parties sont recréés, indépendamment des ports. Les exemples suivants sont recréés correctement sur le clone :<br /><br />customspn/DC1:202 UTILISATION NON VALIDE DE SYMBOLES *(ceci est recréé)*<br /><br />customspn/DC1 UTILISATION NON VALIDE DE SYMBOLES *(ceci est recréé)*<br /><br />customspn/DC1.corp.contoso.com:202 UTILISATION NON VALIDE DE SYMBOLES *(ceci est un nom recréé)*<br /><br />customspn/DC1.corp.contoso.com UTILISATION NON VALIDE DE SYMBOLES *(ceci est recréé)*|  
|**Résolution et remarques**|Il s'agit d'une limitation du processus de changement de nom du contrôleur de domaine liée à Windows, et pas seulement au clonage. La logique de changement de nom des noms de principal du service en trois parties n'est gérée dans aucun scénario. La plupart des services Windows inclus ne sont pas affectés, car ils recréent les noms de principal du service manquants en fonction des besoins. Pour d'autres applications, il peut s'avérer nécessaire d'entrer manuellement le nom de principal du service pour résoudre le problème.<br /><br />Description dans l’article de la Base de connaissances n°2742874.|  
  
|||  
|-|-|  
|**Problème**|**Le clonage échoue et démarre en mode DSRM, erreurs réseau générales**|  
|**Symptômes**|Le clone démarre en mode de réparation des services d'annuaire. Des erreurs réseau générales se sont produites.|  
|Résolution et remarques|Assurez-vous que le nouveau clone n'a pas une adresse MAC statique en double affectée par le contrôleur de domaine source. Vous pouvez déterminer si un ordinateur virtuel utilise des adresses MAC statiques en exécutant la commande suivante sur l'hôte de l'hyperviseur, aussi bien pour les ordinateurs virtuels sources que les ordinateurs virtuels clones :<br /><br />Get-VM -VMName *test-vm* &#124; Get-VMNetworkAdapter &#124; fl *<br /><br />Remplacez l'adresse MAC par une adresse statique unique ou passez à l'utilisation d'adresses MAC dynamiques.<br /><br />Description dans l'article de la Base de connaissances n° 2742844|  
  
|||  
|-|-|  
|**Problème**|**Le clonage échoue et démarre en mode DSRM en tant que doublon du contrôleur de domaine source**|  
|**Symptômes**|Un nouveau clone démarre sans clonage. Le fichier dccloneconfig.xml n'est pas renommé et le serveur démarre en mode de restauration des services d'annuaire. Le journal des événements Services d'annuaire affiche l'erreur 2164<br /><br />*<COMPUTERNAME>* Impossible de démarrer le service DsRoleSvc pour cloner le contrôleur de domaine virtuel local.|  
|**Résolution et remarques**|Examinez les paramètres de service du service Serveur de rôles DS (DsRoleSvc) et assurez-vous que son type de démarrage est manuel. Vérifiez qu'aucun programme tiers n'empêche le démarrage de ce service.<br /><br />Pour plus d'informations sur la façon de récupérer ce contrôleur de domaine secondaire tout en veillant à ce que les mises à jour soient répliquées en sortie, voir l'article n° 2742970 dans la Base de connaissances Microsoft.|  
  
|||  
|-|-|  
|**Problème**|**Le clonage échoue et démarre en mode DSRM, erreur 8610**|  
|**Symptômes**|Le clone démarre en mode de restauration des services d'annuaire. Dcpromo.log affiche l'erreur 8610 (c'est-à-dire ERROR_DS_ROLE_NOT_VERIFIED 8610 ou 0x21A2)|  
|**Résolution et remarques**|Se produit si le contrôleur de domaine principal est détectable mais qu'il n'a pas effectué une réplication suffisante pour pouvoir endosser le rôle. C'est le cas, par exemple, si le clonage a démarré et qu'un autre administrateur déplace le rôle FSMO de l'émulateur de contrôleur de domaine principal vers un nouveau contrôleur de domaine.<br /><br />Description dans l’article de la Base de connaissances n°2742916.|  
  
|||  
|-|-|  
|**Problème**|**Le clonage échoue et démarre en mode DSRM, erreurs réseau générales**|  
|**Symptômes**|Le clone démarre en mode de restauration des services d'annuaire. Des erreurs réseau générales se sont produites.|  
|**Résolution et remarques**|Assurez-vous que le nouveau clone n'a pas une adresse MAC statique en double affectée par le contrôleur de domaine source. Vous pouvez déterminer si un ordinateur virtuel utilise des adresses MAC statiques en exécutant la commande suivante sur l'hôte Hyper-V, aussi bien pour les ordinateurs virtuels sources que les ordinateurs virtuels clones :<br /><br />Get-VM -VMName *test-vm* &#124; Get-VMNetworkAdapter &#124; fl *<br /><br />Remplacez l'adresse MAC par une adresse statique unique ou passez à l'utilisation d'adresses MAC dynamiques.<br /><br />Description dans l'article de la Base de connaissances n° 2742844.|  
  
|||  
|-|-|  
|**Problème**|**Le clonage échoue et démarre en mode DSRM**|  
|**Symptômes**|Le clone démarre en mode de réparation des services d'annuaire|  
|**Résolution et remarques**|Assurez-vous que le fichier dccloneconfig.xml contient la définition de schéma (voir sampledccloneconfig.xml, ligne 2) :<br /><br />**<d3c:DCCloneConfig xmlns:d3c="uri:microsoft.com:schemas:DCCloneConfig">**<br /><br />Description dans l'article de la Base de connaissances n° 2742844|  
  
|||  
|-|-|  
|Problème|**Aucun serveur d’ouverture de session n’est la journalisation des erreurs disponibles en mode DSRM**|  
|**Symptômes**|Le clone démarre en mode de réparation des services d'annuaire. Vous essayez de vous connecter et recevez l'erreur suivante :<br /><br />**Il n’y a aucun serveur d’ouverture de session est disponible pour traiter la demande d’ouverture de session**|  
|**Résolution et remarques**|Assurez-vous que vous vous connectez avec le compte Administrateur DSRM et non le compte de domaine. Utilisez la flèche gauche et tapez le nom d'utilisateur :<br /><br />**.\administrator**<br /><br />Description dans l’article de la Base de connaissances n°2742908|  
  
|||  
|-|-|  
|**Problème**|**Source du clonage échoue en mode DSRM, erreur**|  
|**Symptômes**|Durant le clonage, erreur 8437 « Échec de la création d'objets du contrôleur de domaine clone sur le contrôleur de domaine principal » (0x20f5)|  
|**Résolution et remarques**|Un nom d'ordinateur dupliqué a été défini dans le fichier DCCloneConfig.xml en tant que contrôleur de domaine source ou contrôleur de domaine existant. Le nom d'ordinateur doit également être au format des noms d'ordinateurs NetBIOS (15 caractères au maximum, pas un nom de domaine complet).<br /><br />Corrigez le fichier dccloneconfig.xml en définissant un nom unique et valide.<br /><br />Description dans l’article de la Base de connaissances n°2742959|  
  
|||  
|-|-|  
|**Problème**|**Nouveau-addccloneconfigfile erreur « l’index était hors limites »**|  
|**Symptômes**|Durant l'exécution de l'applet de commande new-addccloneconfigfile, vous recevez l'erreur suivante :<br /><br />L'index était hors limites. Il ne doit pas être négatif et doit être inférieur à la taille de la collection.|  
|**Résolution et remarques**|Vous devez exécuter l'applet de commande dans une console Windows PowerShell avec des privilèges d'administrateur élevés. Cette erreur est due à l'absence d'appartenance au groupe Administrateurs local de l'ordinateur.<br /><br />Description dans l’article de la Base de connaissances n°2742927|  
  
|||  
|-|-|  
|**Problème**|**Échec du clonage, contrôleur de domaine dupliqué**|  
|**Symptômes**|Le clone démarre sans clonage et duplique le contrôleur de domaine source existant|  
|**Résolution et remarques**|L'ordinateur a été copié et a démarré. Toutefois, il ne contient aucun fichier DcCloneConfig.xml dans l'un des emplacements pris en charge. En outre, il n'a pas d'adresse IP en double par rapport au contrôleur de domaine source. Le contrôleur de domaine doit être supprimé correctement pour éviter toute perte de données.<br /><br />Description dans l’article de la Base de connaissances n°2742970|  
  
|||  
|-|-|  
|**Problème**|**New-ADDCCloneConfigFile échoue avec le serveur n’est pas opérationnelle erreur lorsqu’il vérifie si le contrôleur de domaine source est un membre du groupe de contrôleurs de domaine clonables si un catalogue global n’est pas disponible.**|  
|**Symptômes**|Durant l'exécution de l'applet de commande New-ADDCCloneConfigFile pour créer un fichier dccloneconfig.xml, vous recevez l'erreur suivante :<br /><br />Code - le serveur n’est pas opérationnel|  
|**Résolution et remarques**|Vérifiez la connectivité à un catalogue global à partir du serveur sur lequel vous exécutez New-ADDCCloneConfigFile, puis assurez-vous que l'appartenance du contrôleur de domaine source au groupe Contrôleurs de domaine clonables a été répliquée vers ce catalogue global.<br /><br />Exécutez la commande suivante pour vider la mémoire cache du localisateur de contrôleurs de domaine au cas où un catalogue global ou un contrôleur de domaine aurait été mis hors connexion récemment :<br /><br />Code - nltest /dsgetdc: /GC /FORCE|  
  
### <a name="advanced-troubleshooting"></a>Résolution avancée des problèmes  
Ce module vise à enseigner la résolution avancée des problèmes à l'aide d'exemples de journaux de *travail* et d'explications. Si vous comprenez en quoi consiste le fonctionnement d'un contrôleur de domaine virtualisé, vous pouvez identifier facilement les défaillances dans votre environnement. Ces journaux sont présentés par source, dans l'ordre croissant des événements *attendus* (même quand il s'agit d'avertissements et d'erreurs) liés à un contrôleur de domaine cloné au sein de chaque journal.  
  
#### <a name="cloning-a-domain-controller"></a>Clonage d'un contrôleur de domaine  
Dans cet exemple, le contrôleur de domaine clone utilise le protocole DHCP pour obtenir une adresse IP, réplique le répertoire SYSVOL à l'aide des services FRS ou DFSR (consultez le journal approprié si nécessaire). En outre, il sert de catalogue global et utilise un fichier dccloneconfig.xml vide.  
  
##### <a name="directory-services-event-log"></a>Journal des événements Services d'annuaire  
Le journal Services d'annuaire contient la majorité des informations opérationnelles relatives au clonage en fonction des événements. L'hyperviseur change l'ID de génération d'ordinateur virtuel (ce qui est identifié par le service NTDS), puis invalide le pool RID et change l'ID d'invocation. Le nouvel ID de génération d'ordinateur virtuel est défini. Le serveur réplique les données Active Directory en entrée. Le service DFSR est arrêté. Sa base de données, qui héberge SYSVOL, est supprimée, ce qui entraîne une synchronisation forcée faisant autorité en entrée. La limite supérieure du numéro USN est configurée.  
  
||||  
|-|-|-|  
|**ID d’événement**|**Source**|**Message**|  
|**2160**|ActiveDirectory_DomainService|Les services de domaine Active Directory locaux ont trouvé un fichier de configuration de clonage de contrôleur de domaine virtuel.<br /><br />Le fichier de configuration de clonage de contrôleur de domaine virtuel se trouve à l'emplacement suivant :<br /><br />*<path>* \DCCloneConfig.xml<br /><br />L'existence du fichier de configuration de clonage de contrôleur de domaine virtuel indique que le contrôleur de domaine virtuel local est un clone d'un autre contrôleur de domaine virtuel. Les services de domaine Active Directory commenceront à se dupliquer eux-mêmes.|  
|**2191**|ActiveDirectory_DomainService|Les services de domaine Active Directory ont défini la valeur de Registre suivante pour désactiver les mises à jour DNS.<br /><br />Clé de Registre :<br /><br />SYSTEM\CurrentControlSet\Services\Netlogon\Parameters<br /><br />Valeur de Registre :<br /><br />UseDynamicDns<br /><br />Données de valeur de Registre :<br /><br />0<br /><br />Durant le processus de clonage, l'ordinateur local peut temporairement porter le même nom d'ordinateur que l'ordinateur source du clonage. Les inscriptions d'enregistrement DNS A et AAAA sont désactivées pendant cette période. Ainsi, les clients ne peuvent pas envoyer de demandes à l'ordinateur local qui fait l'objet du clonage. Le processus de clonage réactivera les mises à jour DNS quand le clonage sera terminé.|  
|**2191**|ActiveDirectory_DomainService|Les services de domaine Active Directory ont défini la valeur de Registre suivante pour désactiver les mises à jour DNS.<br /><br />Clé de Registre :<br /><br />SYSTEM\CurrentControlSet\Services\Dnscache\Parameters<br /><br />Valeur de Registre :<br /><br />RegistrationEnabled<br /><br />Données de valeur de Registre :<br /><br />0<br /><br />Durant le processus de clonage, l'ordinateur local peut temporairement porter le même nom d'ordinateur que l'ordinateur source du clonage. Les inscriptions d'enregistrement DNS A et AAAA sont désactivées pendant cette période. Ainsi, les clients ne peuvent pas envoyer de demandes à l'ordinateur local qui fait l'objet du clonage. Le processus de clonage réactivera les mises à jour DNS quand le clonage sera terminé.<br /><br />« Information 7/2/2012 3:12:49 PM Microsoft-Windows-ActiveDirectory_DomainService 2191-Configuration interne » les Services de domaine Active Directory définir la valeur de Registre suivante pour désactiver les mises à jour DNS.<br /><br />Clé de Registre :<br /><br />SYSTEM\CurrentControlSet\Services\Tcpip\Parameters<br /><br />Valeur de Registre :<br /><br />DisableDynamicUpdate<br /><br />Données de valeur de Registre :<br /><br />1<br /><br />Durant le processus de clonage, l'ordinateur local peut temporairement porter le même nom d'ordinateur que l'ordinateur source du clonage. Les inscriptions d'enregistrement DNS A et AAAA sont désactivées pendant cette période. Ainsi, les clients ne peuvent pas envoyer de demandes à l'ordinateur local qui fait l'objet du clonage. Le processus de clonage réactivera les mises à jour DNS quand le clonage sera terminé.|  
|**2172**|ActiveDirectory_DomainService|Lisez l'attribut msDS-GenerationId de l'objet ordinateur du contrôleur de domaine.<br /><br />Valeur de l'attribut msDS-GenerationId :<br /><br />*<Number>*|  
|**2170**|ActiveDirectory_DomainService|Un changement d'ID de génération a été détecté.<br /><br />ID de génération mis en cache dans les services d'annuaire (ancienne valeur) :<br /><br />*<Number>*<br /><br />ID de génération actuellement dans l'ordinateur virtuel (nouvelle valeur) :<br /><br />*<Number>*<br /><br />Le changement d'ID de génération se produit après l'application d'une capture instantanée d'ordinateur virtuel, après une opération d'importation d'ordinateur virtuel ou après une opération de migration dynamique. Les services de domaine Active Directory vont créer un ID d'appel pour récupérer le contrôleur de domaine. Les contrôleurs de domaine virtualisés ne doivent pas être restaurés à l'aide de captures instantanées d'ordinateurs virtuels. La méthode prise en charge pour restaurer le contenu d'une base de données des services de domaine Active Directory consiste à restaurer une sauvegarde de l'état du système effectuée à l'aide d'une application de sauvegarde compatible avec les services de domaine Active Directory.|  
|**1109**|ActiveDirectory_DomainService|L'attribut invocationID pour ce serveur d'annuaire a été modifié. Le numéro de séquence de mise à jour le plus élevé au moment où la sauvegarde a été créée est le suivant :<br /><br />Attribut InvocationID (ancienne valeur) :<br /><br />*<GUID>*<br /><br />Attribut InvocationID (nouvelle valeur) :<br /><br />*<GUID>*<br /><br />Numéro de séquence de la mise à jour :<br /><br />*<Number>*<br /><br />L'attribut invocationID est modifié quand un serveur d'annuaire est restauré à partir d'un support de sauvegarde, est configuré pour héberger une partition d'annuaire d'application accessible en écriture, a été redémarré après l'application d'un instantané d'ordinateur virtuel, après une opération d'importation d'ordinateur virtuel, ou après une opération de migration dynamique. Les contrôleurs de domaine virtualisés ne doivent pas être restaurés à l'aide de captures instantanées d'ordinateurs virtuels. La méthode prise en charge pour restaurer le contenu d'une base de données des services de domaine Active Directory consiste à restaurer une sauvegarde de l'état du système effectuée à l'aide d'une application de sauvegarde compatible avec les services de domaine Active Directory.|  
|**1000**|ActiveDirectory_DomainService|Démarrage des services de domaine Microsoft Active Directory terminé.|  
|**1394**|ActiveDirectory_DomainService|Tous les problèmes empêchant les mises à jour de la base de données des services de domaine Active Directory ont été résolus. Les nouvelles mises à jour de la base de données des services de domaine Active Directory réussissent. Le service Net Logon a redémarré|  
|**2163**|ActiveDirectory_DomainService|Le service DsRoleSvc a démarré pour cloner le contrôleur de domaine virtuel local.|  
|**326**|NTDS ISAM|NTDS (536) NTDSA : Le moteur de base de données a attaché une base de données (1, C:\Windows\NTDS\ntds.dit). (Délai = 0 seconde)<br /><br />Séquence de minutage interne : [1] 0.000, [2] 0.000, [3] 0.000, [4] 0.000, [5] 0.000, [6] 0.016, [7] 0.000, [8] 0.000, [9] 0.000, [10] 0.000, [11] 0.000, [12] 0.000.<br /><br />Mémoire cache enregistrée : 1|  
|**103**|NTDS ISAM|NTDS (536) NTDSA : Le moteur de base de données a arrêté l'instance (0).<br /><br />Arrêt brutal : 0<br /><br />Séquence de minutage interne : [1] 0.000, [2] 0.000, [3] 0.000, [4] 0.000, [5] 0.032, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.031, [10] 0.000, [11] 0.000, [12] 0.000, [13] 0.000, [14] 0.000, [15] 0.000.|  
|**102**|NTDS ISAM|NTDS (536) NTDSA : Le moteur de base de données (6.02.8225.0000) démarre une nouvelle instance (0).|  
|**105**|NTDS ISAM|NTDS (536) NTDSA : Le moteur de base de données a démarré une nouvelle instance (0). (Délai = 0 seconde)<br /><br />Séquence de minutage interne : [1] 0.016, [2] 0.000, [3] 0.015, [4] 0.078, [5] 0.000, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.046, [10] 0.000, [11] 0.000.|  
|**1004**|ActiveDirectory_DomainService|Les services de domaine Active Directory ont été arrêtés correctement.|  
|**102**|NTDS ISAM|NTDS (536) NTDSA : Le moteur de base de données (6.02.8225.0000) démarre une nouvelle instance (0).|  
|**326**|NTDS ISAM|NTDS (536) NTDSA : Le moteur de base de données a attaché une base de données (1, C:\Windows\NTDS\ntds.dit). (Délai = 0 seconde)<br /><br />Séquence de minutage interne : [1] 0.000, [2] 0.015, [3] 0.016, [4] 0.000, [5] 0.031, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.000, [10] 0.000, [11] 0.000, [12] 0.000.<br /><br />Mémoire cache enregistrée : 1|  
|**105**|NTDS ISAM|NTDS (536) NTDSA : Le moteur de base de données a démarré une nouvelle instance (0). (Délai = 1 seconde)<br /><br />Séquence de minutage interne : [1] 0.031, [2] 0.000, [3] 0.000, [4] 0.391, [5] 0.000, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.031, [10] 0.000, [11] 0.000.|  
|**1109**|ActiveDirectory_DomainService|L'attribut invocationID pour ce serveur d'annuaire a été modifié. Le numéro de séquence de mise à jour le plus élevé au moment où la sauvegarde a été créée est le suivant :<br /><br />Attribut InvocationID (ancienne valeur) :<br /><br />*<GUID>*<br /><br />Attribut InvocationID (nouvelle valeur) :<br /><br />*<GUID>*<br /><br />Numéro de séquence de la mise à jour :<br /><br />*<Number>*<br /><br />L'attribut invocationID est modifié quand un serveur d'annuaire est restauré à partir d'un support de sauvegarde, est configuré pour héberger une partition d'annuaire d'application accessible en écriture, a été redémarré après l'application d'un instantané d'ordinateur virtuel, après une opération d'importation d'ordinateur virtuel, ou après une opération de migration dynamique. Les contrôleurs de domaine virtualisés ne doivent pas être restaurés à l'aide de captures instantanées d'ordinateurs virtuels. La méthode prise en charge pour restaurer le contenu d'une base de données des services de domaine Active Directory consiste à restaurer une sauvegarde de l'état du système effectuée à l'aide d'une application de sauvegarde compatible avec les services de domaine Active Directory.|  
|**1168**|ActiveDirectory_DomainService|Erreur interne : Une erreur des services de domaine Active Directory s'est produite.<br /><br />Données supplémentaires<br /><br />Valeur d'erreur (format décimal) :<br /><br />2<br /><br />Valeur d'erreur (format hexadécimal) :<br /><br />2<br /><br />ID interne :<br /><br />7011658|  
|**1110**|ActiveDirectory_DomainService|La promotion de ce contrôleur de domaine en un catalogue global sera retardée de l'intervalle suivant.<br /><br />Intervalle (minutes) :<br /><br />5<br /><br />Ce retard est nécessaire afin que les partitions d'annuaire requises puissent être préparées avant l'annonce du catalogue global. Dans le Registre, vous pouvez spécifier le nombre de secondes pendant lequel l'agent système d'annuaire devra attendre avant de promouvoir le contrôleur de domaine en un catalogue global. Pour plus d'informations concernant la valeur de Registre de l'annonce de retard du catalogue global, consultez le Guide du kit de ressources des systèmes distribués|  
|**103**|NTDS ISAM|NTDS (536) NTDSA : Le moteur de base de données a arrêté l'instance (0).<br /><br />Arrêt brutal : 0<br /><br />Séquence de minutage interne : [1] 0.000, [2] 0.000, [3] 0.000, [4] 0.000, [5] 0.047, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.016, [10] 0.000, [11] 0.000, [12] 0.000, [13] 0.000, [14] 0.000, [15] 0.000.|  
|**1004**|ActiveDirectory_DomainService|Les services de domaine Active Directory ont été arrêtés correctement.|  
|**1539**|ActiveDirectory_DomainService|Les services de domaine Active Directory n'ont pas pu désactiver le cache logiciel d'écriture disque sur le disque dur suivant.<br /><br />Disque dur :<br /><br />c:<br /><br />Des données peuvent être perdues lors des échecs du système|  
|**2179**|ActiveDirectory_DomainService|L'attribut msDS-GenerationId de l'objet ordinateur du contrôleur de domaine a été paramétré comme suit :<br /><br />Attribut GenerationID :<br /><br />*<Number>*|  
|**2173**|ActiveDirectory_DomainService|Échec de lecture de l'attribut msDS-GenerationId de l'objet ordinateur du contrôleur de domaine. La raison peut en être un échec de transaction de base de données, ou l'absence d'ID de génération dans la base de données locale. L'attribut msDS-GenerationId n'existe pas durant le premier redémarrage après dcpromo ou le contrôleur de domaine n'est pas un contrôleur de domaine virtuel.<br /><br />Données supplémentaires<br /><br />Code d'échec :<br /><br />6|  
|**1000**|ActiveDirectory_DomainService|Démarrage des services de domaine Microsoft Active Directory terminé, version 6.2.8225.0|  
|**1394**|ActiveDirectory_DomainService|Tous les problèmes empêchant les mises à jour de la base de données des services de domaine Active Directory ont été résolus. Les nouvelles mises à jour de la base de données des services de domaine Active Directory réussissent. Le service Net Logon a redémarré.|  
|**1128**|ActiveDirectory_DomainService|1128 - Vérificateur de cohérence des données « Une connexion de réplication a été créée du service d'annuaire source vers le service d'annuaire local. »<br /><br />Service d'annuaire source :<br /><br />CN=NTDS Settings,*<Domain Controller DN>*<br /><br />Service d'annuaire local :<br /><br />CN=NTDS Settings, *<Domain Controller DN>*<br /><br />Données supplémentaires<br /><br />Code de la raison :<br /><br />0x2<br /><br />ID interne du point de création :<br /><br />f0a025d|  
|**1999**|ActiveDirectory_DomainService|Le service d'annuaire source a optimisé le numéro de séquence de mise à jour (USN) présenté par le service d'annuaire cible. Les services d'annuaire source et cible ont un partenaire de réplication commun. Le service d'annuaire cible est à jour par rapport au partenaire de réplication commun, et le service d'annuaire source a été installé à partir d'une sauvegarde de ce partenaire.<br /><br />Identificateur du service d'annuaire cible :<br /><br />*<GUID> (<FQDN>)*<br /><br />Identificateur du service d'annuaire commun :<br /><br />*<GUID>*<br /><br />Numéro USN de la propriété commune :<br /><br />*<Number>*<br /><br />Par conséquent, le vecteur de mise à jour récente du service d'annuaire de destination a été configuré avec les paramètres suivants.<br /><br />Précédent USN de l'objet :<br /><br />0<br /><br />Précédent USN de la propriété :<br /><br />0<br /><br />GUID de la base de données :<br /><br />*<GUID>*<br /><br />USN de l'objet :<br /><br />*<Number>*<br /><br />USN de la propriété :<br /><br />*<Number>*|  
  
##### <a name="system-event-log"></a>Journal des événements Système  
Les prochaines indications relatives aux opérations de clonage se trouvent dans le journal des événements Système. Comme l'hyperviseur indique à l'ordinateur invité qu'il a été cloné ou restauré à partir d'une capture instantanée, le contrôleur de domaine invalide immédiatement son pool RID pour éviter de dupliquer les principaux de sécurité plus tard. Durant le clonage, plusieurs opérations et messages se succèdent de manière attendue. La plupart concernent le démarrage et l'arrêt des services, ainsi que les erreurs attendues qui en sont à l'origine. Une fois le processus terminé, le journal des événements Système consigne la réussite globale du clonage.  
  
||||  
|-|-|-|  
|**ID d’événement**|**Source**|**Message**|  
|**16654**|Directory-Services-SAM|Un pool d'identificateurs de comptes (RID) a été invalidé Ceci peut se produire dans les cas attendus suivants :<br /><br />1. Un contrôleur de domaine est restauré à partir de la sauvegarde.<br /><br />2. Un contrôleur de domaine exécuté sur un ordinateur virtuel est restauré à partir de la capture instantanée.<br /><br />3. Un administrateur a invalidé le pool manuellement|  
|**7036**|Gestionnaire de contrôle des services|Le service Services de domaine Active Directory est entré dans l'état en cours d'exécution.|  
|**7036**|Gestionnaire de contrôle des services|Le service Centre de distribution de clés Kerberos est entré dans l'état en cours d'exécution.|  
|**3096**|Netlogon|Le contrôleur de domaine principal pour ce domaine est introuvable.|  
|**7036**|Gestionnaire de contrôle des services|Le service Gestionnaire de comptes de sécurité est entré dans l'état en cours d'exécution.|  
|**7036**|Gestionnaire de contrôle des services|Le service Serveur est entré dans l'état en cours d'exécution.|  
|**7036**|Gestionnaire de contrôle des services|Le service Netlogon est entré dans l'état en cours d'exécution.|  
|**7036**|Gestionnaire de contrôle des services|Le service Services Web Active Directory est entré dans l'état en cours d'exécution.|  
|**7036**|Gestionnaire de contrôle des services|Le service de réplication DFS est entré dans l'état en cours d'exécution.|  
|**7036**|Gestionnaire de contrôle des services|Le service de réplication de fichiers est entré dans l'état en cours d'exécution.|  
|**14533**|Microsoft-Windows-DfsSvc|DFS a fini la création de tous les espaces de noms.|  
|**14531**|Microsoft-Windows-DfsSvc|Le serveur DFS a fini de s'initialiser.|  
|**7036**|Gestionnaire de contrôle des services|Le service Espace de noms DFS est entré dans l'état en cours d'exécution.|  
|**7023**|Gestionnaire de contrôle des services|Le service de messagerie intersite s'est arrêté avec l'erreur :<br /><br />Le serveur spécifié ne peut pas exécuter l'opération demandée.|  
|**7036**|Gestionnaire de contrôle des services|Le service de messagerie intersite est entré dans l'état d'arrêt.|  
|**5806**|Netlogon|Les mises à jour dynamiques ont été désactivées manuellement sur ce contrôleur de domaine.<br /><br />ACTION UTILISATEUR<br /><br />Reconfigurer ce contrôleur de domaine pour utiliser les mises à jour dynamiques ou ajouter manuellement les enregistrements DNS du fichier « %SystemRoot%\System32\Config\Netlogon.dns » vers la base de données DNS.|  
|**16651**|Directory-Services-SAM|La demande de nouveau pool d'identificateurs de comptes a échoué. L'opération sera tentée jusqu'à ce que la demande réussisse. L'erreur est<br /><br />L'opération FSMO demandée a échoué. Le propriétaire FSMO actuel n'a pas pu être contacté.|  
|**7036**|Gestionnaire de contrôle des services|Le service Serveur DNS est entré dans l'état en cours d'exécution.|  
|**7036**|Gestionnaire de contrôle des services|Le service Serveur de rôles DS est entré dans l'état en cours d'exécution.|  
|**7036**|Gestionnaire de contrôle des services|Le service Netlogon est entré dans l'état d'arrêt.|  
|**7036**|Gestionnaire de contrôle des services|Le service de réplication de fichiers est entré dans l'état d'arrêt.|  
|**7036**|Gestionnaire de contrôle des services|Le service Centre de distribution de clés Kerberos est entré dans l'état d'arrêt.|  
|**7036**|Gestionnaire de contrôle des services|Le service Serveur DNS est entré dans l'état d'arrêt.|  
|**7036**|Gestionnaire de contrôle des services|Le service Services de domaine Active Directory est entré dans l'état d'arrêt.|  
|**7036**|Gestionnaire de contrôle des services|Le service Netlogon est entré dans l'état en cours d'exécution.|  
|**7040**|Gestionnaire de contrôle des services|Le type de démarrage du service Services de domaine Active Directory a été changé de Démarrage automatique à Désactivé.|  
|**7036**|Gestionnaire de contrôle des services|Le service Netlogon est entré dans l'état d'arrêt.|  
|**7036**|Gestionnaire de contrôle des services|Le service de réplication de fichiers est entré dans l'état en cours d'exécution.|  
|**29219**|DirectoryServices-DSROLE-Server|Le clonage du contrôleur de domaine virtuel a réussi.|  
|**29223**|DirectoryServices-DSROLE-Server|Ce serveur est maintenant un contrôleur de domaine.|  
|**29265**|DirectoryServices-DSROLE-Server|Le clonage du contrôleur de domaine virtuel a réussi. Le fichier de configuration de clonage de contrôleur de domaine virtuel C:\Windows\NTDS\DCCloneConfig.xml a été renommé en C:\Windows\NTDS\DCCloneConfig.20120207-151533.xml.|  
|**1074**|User32|Le processus C:\Windows\system32\lsass.exe (DC2) a lancé le redémarrage de l'ordinateur DC2 pour le compte de l'utilisateur AUTORITE NT\Système pour la raison suivante : Système d'exploitation : reconfiguration (planifiée)<br /><br />Code de la raison : 0x80020004<br /><br />Type d'extinction : redémarrer<br /><br />Commentaire : "|  
  
##### <a name="dcpromolog"></a>DCPROMO.LOG  
Le fichier Dcpromo.log contient la partie de promotion réelle du clonage que le journal des événements Services d'annuaire ne décrit pas. Comme le journal ne fournit pas le niveau d'explication des entrées du journal des événements, cette section du module contient des informations supplémentaires.  
  
Le processus de promotion signifie que le clonage commence, que le contrôleur de domaine perd sa configuration actuelle et est promu à nouveau à l'aide de la base de données Active Directory existante (comme une promotion IFM). Le contrôleur de domaine réplique ensuite les deltas des changements entrants pour Active Directory et SYSVOL, et le clonage est terminé.  
  
> [!NOTE]  
> Le journal a été changé dans ce module pour des raisons de lisibilité. La colonne de date a été supprimée.  
  
> [!NOTE]  
> Pour plus d'explications sur le fichier dcpromo.log, voir les informations relatives à la présentation et la résolution des problèmes d'administration simplifiée des services AD DS dans Windows Server 2012.  
>   
> [https://go.microsoft.com/fwlink/p/?LinkId=237244](https://go.microsoft.com/fwlink/p/?LinkId=237244)  
  
-   Démarrer la promotion basée sur le clonage  
  
-   Définir l'indicateur du mode de restauration des services d'annuaire pour que le serveur ne redémarre pas normalement comme le clone d'origine, ce qui entraînerait des conflits au niveau des noms ou du service d'annuaire  
  
-   Mettre à jour le journal des événements Services d'annuaire  
  
```  
15:14:01 [INFO] vDC Cloneing: Setting Boot into DSRM flag succeeded.  
15:14:01 [WARNING] Cannot get user Token for Format Message: 1725l  
15:14:01 [INFO] vDC Cloning: Created vDCCloningUpdate event.  
15:14:01 [INFO] vDC Cloning: Created vDCCloningComplete event.  
```  
  
-   Arrêter le service NetLogon pour que le contrôleur de domaine ne fasse pas de publication  
  
```  
15:14:01 [INFO] Stopping service NETLOGON  
15:14:01 [INFO] ControlService(STOP) on NETLOGON returned 1(gle=0)  
15:14:01 [INFO] DsRolepWaitForService: waiting for NETLOGON to enter one of 7 states  
15:14:01 [INFO] DsRolepWaitForService: QueryServiceStatus on NETLOGON returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:02 [INFO] DsRolepWaitForService: QueryServiceStatus on NETLOGON returned 1 (gle=0), SvcStatus.dwCS=1  
15:14:02 [INFO] DsRolepWaitForService: exiting because NETLOGON entered STOPPED state  
15:14:02 [INFO] DsRolepWaitForService(for any end state) on NETLOGON service returned 0  
15:14:02 [INFO] ControlService(STOP) on NETLOGON returned 0(gle=1062)  
15:14:02 [INFO] Exiting service-stop loop after service NETLOGON entered STOPPED state  
15:14:02 [INFO] StopService on NETLOGON returned 0  
15:14:02 [INFO] Configuring service NETLOGON to 1 returned 0  
15:14:02 [INFO] Updating service status to 4  
15:14:02 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
```  
  
-   Examinez le fichier dccloneconfig.xml pour y rechercher les personnalisations de l'administrateur.  
  
-   Dans cet exemple, il s'agit d'un fichier vide. Tous les paramètres sont donc automatiquement générés et l'adressage IP automatique est nécessaire à partir du réseau  
  
```  
15:14:02 [INFO] vDC Cloning: Clone config file C:\Windows\NTDS\DCCloneConfig.xml is considered to be a blank file (containing 0 bytes)  
15:14:02 [INFO] vDC Cloning: Parsing clone config file C:\Windows\NTDS\DCCloneConfig.xml returned HRESULT 0x0  
```  
  
-   Vérifier qu'il n'y a aucun service ou programme installé qui ne fait pas partie des fichiers DefaultDCCloneAllowList.xml ou CustomDCCloneAllowList.xml  
  
```  
15:14:02 [INFO] vDC Cloning: Checking allowed list:  
15:14:03 [INFO] vDC Cloning: Completed checking allowed list:  
15:14:03 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
```  
  
-   Activer le protocole DHCP sur les cartes réseau, car les informations IP n'ont pas été spécifiées par l'administrateur  
  
```  
15:14:03 [INFO] vDC Cloning: Enable DHCP:  
15:14:03 [INFO] WMI Instance: Win32_NetworkAdapterConfiguration.Index=12  
15:14:03 [INFO] Method: EnableDHCP  
15:14:03 [INFO] HRESULT code: 0x0 (0)  
15:14:03 [INFO] Return Value: 0x0 (0)  
15:14:03 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:14:03 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
```  
  
-   Localiser l'émulateur de contrôleur de domaine principal (PDC, Primary Domain Controller)  
  
-   Définir le site du clone (généré automatiquement dans le cas présent)  
  
-   Définir le nom du clone (généré automatiquement dans le cas présent)  
  
```  
15:14:03 [INFO] vDC Cloning: Found PDC. Name: DC1.root.fabrikam.com  
15:14:04 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:14:04 [INFO] vDC Cloning: Winlogon UI Notification #1: Domain Controller cloning is at 5% completion...  
15:14:05 [INFO] vDC Cloning: Winlogon UI Notification #2: Domain Controller cloning is at 10% completion...  
15:14:05 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:14:05 [INFO] Site of the cloned DC: Default-First-Site-Name  
```  
  
-   Créer l'objet ordinateur clone  
  
-   Renommer le clone pour correspondre au nouveau nom  
  
```  
15:14:05 [INFO] vDC Cloning: Clone DC objects are created on PDC.  
15:14:05 [INFO] Name of the cloned DC: DC2-CL0001  
15:14:05 [INFO] DsRolepSetRegStringValue on System\CurrentControlSet\Services\NTDS\Parameters\CloneMachineName to DC2-CL0001 returned 0  
15:14:05 [INFO] vDC Cloning: Save CloneMachineName in registry: 0x0 (0)  
```  
  
-   Fournir les paramètres de promotion, en fonction du fichier dccloneconfig.xml précédent ou des règles de génération automatique  
  
```  
15:14:05 [INFO] vDC Cloning: Promotion parameters setting:  
15:14:05 [INFO] DNS Domain Name: root.fabrikam.com  
15:14:05 [INFO] Replica Partner: \\DC1.root.fabrikam.com  
15:14:05 [INFO] Site Name: Default-First-Site-Name  
15:14:05 [INFO] DS Database Path: C:\Windows\NTDS  
15:14:05 [INFO] DS Log Path: C:\Windows\NTDS  
15:14:05 [INFO] SysVol Root Path: C:\Windows\SYSVOL  
15:14:05 [INFO] Account: root.fabrikam.com\DC2-CL0001$  
15:14:05 [INFO] Options: DSROLE_DC_CLONING (0x800400)  
```  
  
-   Démarrer la promotion  
  
```  
15:14:05 [INFO] Promote DC as a clone  
15:14:05 [INFO] vDC Cloning: Winlogon UI Notification #3: Domain Controller cloning is at 15% completion...  
15:14:05 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:14:05 [INFO] vDC Cloning: Winlogon UI Notification #4: Domain Controller cloning is at 16% completion...  
15:14:05 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:14:05 [INFO] Validate supplied paths  
15:14:05 [INFO] Validating path C:\Windows\NTDS.  
15:14:05 [INFO] Path is a directory  
15:14:05 [INFO] Path is on a fixed disk drive.  
15:14:05 [INFO] Validating path C:\Windows\NTDS.  
15:14:05 [INFO] Path is a directory  
15:14:05 [INFO] Path is on a fixed disk drive.  
15:14:05 [INFO] Validating path C:\Windows\SYSVOL.  
15:14:05 [INFO] Path is on a fixed disk drive.  
15:14:05 [INFO] Path is on an NTFS volume  
15:14:05 [INFO] vDC Cloning: Winlogon UI Notification #5: Domain Controller cloning is at 17% completion...  
15:14:05 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:14:05 [INFO] Start the worker task  
15:14:05 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:14:05 [INFO] vDC Cloning: Winlogon UI Notification #6: Domain Controller cloning is at 20% completion...  
15:14:05 [INFO] Request for promotion returning 0  
15:14:05 [INFO] vDC Cloning: Winlogon UI Notification #7: Domain Controller cloning is at 21% completion...  
15:14:05 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
```  
  
-   Arrêter et configurer tous les services liés aux services AD DS (NTDS, NTFRS/DFSR, KDC, DNS)  
  
> [!NOTE]  
> Le service DNS met beaucoup de temps à s'arrêter, ce qui est prévu dans ce scénario, car il utilise les zones intégrées à Active Directory qui n'étaient plus disponibles même avant l'arrêt du service NTDS (consultez les événements DNS décrits plus loin dans cette section du module).  
  
```  
15:14:15 [INFO] Stopping service NTDS  
15:14:15 [INFO] Stopping service NtFrs  
15:14:15 [INFO] ControlService(STOP) on NtFrs returned 1(gle=0)  
15:14:15 [INFO] DsRolepWaitForService: waiting for NtFrs to enter one of 7 states  
15:14:15 [INFO] DsRolepWaitForService: QueryServiceStatus on NtFrs returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:16 [INFO] DsRolepWaitForService: QueryServiceStatus on NtFrs returned 1 (gle=0), SvcStatus.dwCS=1  
15:14:16 [INFO] DsRolepWaitForService: exiting because NtFrs entered STOPPED state  
15:14:16 [INFO] DsRolepWaitForService(for any end state) on NtFrs service returned 0  
15:14:16 [INFO] ControlService(STOP) on NtFrs returned 0(gle=1062)  
15:14:16 [INFO] Exiting service-stop loop after service NtFrs entered STOPPED state  
15:14:16 [INFO] StopService on NtFrs returned 0  
15:14:16 [INFO] Configuring service NtFrs to 1 returned 0  
15:14:16 [INFO] Stopping service Kdc  
15:14:16 [INFO] ControlService(STOP) on Kdc returned 1(gle=0)  
15:14:16 [INFO] DsRolepWaitForService: waiting for Kdc to enter one of 7 states  
15:14:16 [INFO] DsRolepWaitForService: QueryServiceStatus on Kdc returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:17 [INFO] DsRolepWaitForService: QueryServiceStatus on Kdc returned 1 (gle=0), SvcStatus.dwCS=1  
15:14:17 [INFO] DsRolepWaitForService: exiting because Kdc entered STOPPED state  
15:14:17 [INFO] DsRolepWaitForService(for any end state) on Kdc service returned 0  
15:14:17 [INFO] ControlService(STOP) on Kdc returned 0(gle=1062)  
15:14:17 [INFO] Exiting service-stop loop after service Kdc entered STOPPED state  
15:14:17 [INFO] StopService on Kdc returned 0  
15:14:17 [INFO] Configuring service Kdc to 1 returned 0  
15:14:17 [INFO] Stopping service DNS  
15:14:17 [INFO] ControlService(STOP) on DNS returned 1(gle=0)  
15:14:17 [INFO] DsRolepWaitForService: waiting for DNS to enter one of 7 states  
15:14:17 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:18 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:19 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:20 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:21 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:22 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:23 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:24 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:25 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:26 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:27 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:28 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:29 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:30 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:31 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:32 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:33 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:34 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:35 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:36 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:37 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:38 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:39 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:40 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:41 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:42 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:43 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:44 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:45 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:46 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:47 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:48 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:49 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:50 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:51 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:52 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:53 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:54 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:55 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:56 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:57 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:58 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:59 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:15:00 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=1  
15:15:00 [INFO] DsRolepWaitForService: exiting because DNS entered STOPPED state  
15:15:00 [INFO] DsRolepWaitForService(for any end state) on DNS service returned 0  
15:15:00 [INFO] ControlService(STOP) on DNS returned 0(gle=1062)  
15:15:00 [INFO] Exiting service-stop loop after service DNS entered STOPPED state  
15:15:00 [INFO] StopService on DNS returned 0  
15:15:00 [INFO] Configuring service DNS to 1 returned 0  
15:15:00 [INFO] ControlService(STOP) on NTDS returned 1(gle=1062)  
15:15:00 [INFO] DsRolepWaitForService: waiting for NTDS to enter one of 7 states  
15:15:00 [INFO] DsRolepWaitForService: QueryServiceStatus on NTDS returned 1 (gle=0), SvcStatus.dwCS=3  
15:15:01 [INFO] DsRolepWaitForService: QueryServiceStatus on NTDS returned 1 (gle=0), SvcStatus.dwCS=1  
15:15:01 [INFO] DsRolepWaitForService: exiting because NTDS entered STOPPED state  
15:15:01 [INFO] DsRolepWaitForService(for any end state) on NTDS service returned 0  
15:15:01 [INFO] ControlService(STOP) on NTDS returned 0(gle=1062)  
15:15:01 [INFO] Exiting service-stop loop after service NTDS entered STOPPED state  
15:15:01 [INFO] StopService on NTDS returned 0  
15:15:01 [INFO] Configuring service NTDS to 1 returned 0  
15:15:01 [INFO] Configuring service NTDS  
15:15:01 [INFO] Configuring service NTDS to 64 returned 0  
15:15:01 [INFO] vDC Cloning: Winlogon UI Notification #8: Domain Controller cloning is at 22% completion...  
15:15:01 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:15:01 [INFO] vDC Cloning: Winlogon UI Notification #9: Domain Controller cloning is at 25% completion...  
15:15:01 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
```  
  
-   Forcer la synchronisation temporelle NT5DS (NTP) avec un autre contrôleur de domaine (généralement l'émulateur de contrôleur de domaine principal)  
  
```  
15:15:02 [INFO] Forcing time sync  
```  
  
-   Contacter un contrôleur de domaine qui détient le compte de contrôleur de domaine source du clone  
  
-   Vider tous les tickets Kerberos existants  
  
```  
15:15:02 [INFO] Searching for a domain controller for the domain root.fabrikam.com that contains the account DC2$  
15:15:02 [INFO] Located domain controller DC1.root.fabrikam.com for domain root.fabrikam.com  
15:15:02 [INFO] vDC Cloning: Winlogon UI Notification #10: Domain Controller cloning is at 26% completion...  
15:15:02 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:15:02 [INFO] Directing kerberos authentication to DC1.root.fabrikam.com returns 0  
15:15:02 [INFO] DsRolepFlushKerberosTicketCache() successfully flushed the Kerberos ticket cache  
15:15:02 [INFO] vDC Cloning: Winlogon UI Notification #11: Domain Controller cloning is at 27% completion...  
15:15:02 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:15:02 [INFO] Using site Default-First-Site-Name for server \\DC1.root.fabrikam.com  
15:15:02 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:15:02 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
```  
  
-   Arrêter le service NetLogon et définir son type de démarrage  
  
```  
15:15:02 [INFO] Stopping service NETLOGON  
15:15:02 [INFO] Stopping service NETLOGON  
15:15:02 [INFO] vDC Cloning: Winlogon UI Notification #12: Domain Controller cloning is at 29% completion...  
15:15:02 [INFO] ControlService(STOP) on NETLOGON returned 1(gle=0)  
15:15:02 [INFO] DsRolepWaitForService: waiting for NETLOGON to enter one of 7 states  
15:15:02 [INFO] DsRolepWaitForService: QueryServiceStatus on NETLOGON returned 1 (gle=0), SvcStatus.dwCS=3  
15:15:03 [INFO] DsRolepWaitForService: QueryServiceStatus on NETLOGON returned 1 (gle=0), SvcStatus.dwCS=1  
15:15:03 [INFO] DsRolepWaitForService: exiting because NETLOGON entered STOPPED state  
15:15:03 [INFO] DsRolepWaitForService(for any end state) on NETLOGON service returned 0  
15:15:03 [INFO] ControlService(STOP) on NETLOGON returned 0(gle=1062)  
15:15:03 [INFO] Exiting service-stop loop after service NETLOGON entered STOPPED state  
15:15:03 [INFO] StopService on NETLOGON returned 0  
15:15:03 [INFO] Configuring service NETLOGON to 1 returned 0  
15:15:03 [INFO] Stopped NETLOGON  
15:15:03 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:15:03 [INFO] vDC Cloning: Winlogon UI Notification #13: Domain Controller cloning is at 30% completion...  
```  
  
-   Configurer l'exécution automatique des services DFSR/NTFRS  
  
-   Supprimer leurs fichiers de base de données existants pour forcer une synchronisation ne faisant pas autorité de SYSVOL au prochain démarrage du service  
  
```  
15:15:03 [INFO] Configuring service DFSR  
15:15:03 [INFO] Configuring service DFSR to 256 returned 0  
15:15:03 [INFO] Configuring service NTFRS  
15:15:03 [INFO] Configuring service NTFRS to 256 returned 0  
15:15:03 [INFO] Removing DFSR Database files for SysVol  
15:15:03 [INFO] Removing FRS Database files in C:\Windows\ntfrs\jet  
15:15:03 [INFO] Removed C:\Windows\ntfrs\jet\log\edb.log  
15:15:03 [INFO] Removed C:\Windows\ntfrs\jet\log\edbres00001.jrs  
15:15:03 [INFO] Removed C:\Windows\ntfrs\jet\log\edbres00002.jrs  
15:15:03 [INFO] Removed C:\Windows\ntfrs\jet\log\edbtmp.log  
15:15:03 [INFO] Removed C:\Windows\ntfrs\jet\ntfrs.jdb  
15:15:03 [INFO] Removed C:\Windows\ntfrs\jet\sys\edb.chk  
15:15:03 [INFO] Removed C:\Windows\ntfrs\jet\temp\tmp.edb  
15:15:04 [INFO] Created system volume path  
15:15:04 [INFO] Configuring service DFSR  
15:15:04 [INFO] Configuring service DFSR to 128 returned 0  
15:15:04 [INFO] Configuring service NTFRS  
15:15:04 [INFO] Configuring service NTFRS to 128 returned 0  
15:15:04 [INFO] vDC Cloning: Winlogon UI Notification #14: Domain Controller cloning is at 40% completion...  
15:15:04 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
```  
  
-   Démarrer le processus de promotion à l'aide du fichier de base de données NTDS existant  
  
-   Contacter le maître RID  
  
> [!NOTE]  
> Le service AD DS n'est pas réellement installé ici. Il s'agit d'une ancienne instrumentation du journal  
  
```  
15:15:04 [INFO] Installing the Directory Service  
15:15:04 [INFO] Calling NtdsInstall for root.fabrikam.com  
15:15:04 [INFO] Starting Active Directory Domain Services installation  
15:15:04 [INFO] Validating user supplied options  
15:15:04 [INFO] Determining a site in which to install  
15:15:04 [INFO] Examining an existing forest...  
15:15:04 [INFO] Starting a replication cycle between DC1.root.fabrikam.com and the RID operations master (2008r2-01.root.fabrikam.com), so that the new replica will be able to create users, groups, and computer objects...  
15:15:04 [INFO] Configuring the local computer to host Active Directory Domain Services  
15:15:04 [INFO] EVENTLOG (Warning): NTDS General / Service Control : 1539  
Active Directory Domain Services could not disable the software-based disk write cache on the following hard disk.  
Hard disk:  
c:  
Data might be lost during system failures.  
15:15:10 [INFO] EVENTLOG (Informational): NTDS General / Internal Processing : 2041  
Duplicate event log entries were suppressed.  
See the previous event log entry for details. An entry is considered a duplicate if  
the event code and all of its insertion parameters are identical. The time period for  
this run of duplicates is from the time of the previous event to the time of this event.  
Event Code:  
80000603  
Number of duplicate entries:   
2  
15:15:10 [INFO] EVENTLOG (Informational): NTDS General / Internal Configuration : 2121  
This Active Directory Domain Services server is disabling the Recycle Bin. Deleted objects may not be undeleted at this time.  
```  
  
-   Changer l'ID d'appel existant dans la base de données des ordinateurs sources  
  
-   Créer un objet paramètres NTDS pour ce clone  
  
-   Répliquer dans Active Directory le delta des objets du contrôleur de domaine partenaire  
  
> [!NOTE]  
> Même si tous les objets sont répertoriés comme étant répliqués, il s'agit simplement des métadonnées nécessaires pour englober les mises à jour. Tous les objets inchangés dans la base de données NTDS clonée existent déjà. Ils ne nécessitent pas de nouvelle réplication, à l'instar d'une promotion IFM.  
  
```  
15:15:10 [INFO] EVENTLOG (Informational): NTDS Replication / Replication : 1109  
The invocationID attribute for this directory server has been changed. The highest update sequence number at the time the backup was created is as follows:  
InvocationID attribute (old value):  
24e7b22f-4706-402d-9b4f-f2690f730b40  
InvocationID attribute (new value):  
f74cefb2-89c2-442c-b1ba-3234b0ed62f8  
Update sequence number:  
20520  
The invocationID is changed when a directory server is restored from backup media, is configured to host a writeable application directory partition, has been resumed after a virtual machine snapshot has been applied, after a virtual machine import operation, or after a live migration operation. Virtualized domain controllers should not be restored using virtual machine snapshots. The supported method to restore or rollback the content of an Active Directory Domain Services database is to restore a system state backup made with an Active Directory Domain Services-aware backup application.  
15:15:10 [INFO] EVENTLOG (Error): NTDS General / Internal Processing : 1168  
Internal error: An Active Directory Domain Services error has occurred.  
Additional Data  
Error value (decimal):  
2  
Error value (hexadecimal):  
2  
Internal ID:  
7011658  
15:15:11 [INFO] Creating the NTDS Settings object for this Active Directory Domain Controller on the remote AD DC DC1.root.fabrikam.com...  
15:15:11 [INFO] Replicating the schema directory partition  
15:15:11 [INFO] Replicated the schema container.  
15:15:12 [INFO] Active Directory Domain Services updated the schema cache.  
15:15:12 [INFO] Replicating the configuration directory partition  
15:15:12 [INFO] Replicating data CN=Configuration,DC=root,DC=fabrikam,DC=com: Received 2612 out of approximately 2612 objects and 94 out of approximately 94 distinguished name (DN) values...  
15:15:12 [INFO] Replicated the configuration container.  
15:15:13 [INFO] Replicating critical domain information...  
15:15:13 [INFO] Replicating data DC=root,DC=fabrikam,DC=com: Received 109 out of approximately 109 objects and 35 out of approximately 35 distinguished name (DN) values...  
15:15:13 [INFO] Replicated the critical objects in the domain container.  
```  
  
-   Remplir, si nécessaire, les partitions de catalogue global avec les mises à jour manquantes  
  
-   Effectuer la partie AD DS critique de la promotion  
  
```  
15:15:13 [INFO] EVENTLOG (Informational): NTDS General / Global Catalog : 1110  
Promotion of this domain controller to a global catalog will be delayed for the following interval.  
Interval (minutes):  
5  
This delay is necessary so that the required directory partitions can be prepared before the global catalog is advertised. In the registry, you can specify the number of seconds that the directory system agent will wait before promoting the local domain controller to a global catalog. For more information about the Global Catalog Delay Advertisement registry value, see the Resource Kit Distributed Systems Guide.  
15:15:14 [INFO] EVENTLOG (Informational): NTDS General / Service Control : 1000  
Microsoft Active Directory Domain Services startup complete, version 6.2.8225.0   
15:15:15 [INFO] Creating new domain users, groups, and computer objects  
15:15:16 [INFO] Completing Active Directory Domain Services installation  
15:15:16 [INFO] NtdsInstall for root.fabrikam.com returned 0  
15:15:16 [INFO] DsRolepInstallDs returned 0  
15:15:16 [INFO] Installed Directory Service  
```  
  
-   Effectuer la réplication entrante de SYSVOL  
  
```  
15:15:16 [INFO] vDC Cloning: Winlogon UI Notification #15: Domain Controller cloning is at 60% completion...  
15:15:16 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:15:18 [INFO] Completed system volume replication  
15:15:18 [INFO] vDC Cloning: Winlogon UI Notification #16: Domain Controller cloning is at 70% completion...  
15:15:18 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:15:18 [INFO] SetProductType to 2 [LanmanNT] returned 0  
15:15:18 [INFO] Set the product type  
15:15:18 [INFO] vDC Cloning: Winlogon UI Notification #17: Domain Controller cloning is at 71% completion...  
15:15:18 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:15:18 [INFO] vDC Cloning: Winlogon UI Notification #18: Domain Controller cloning is at 72% completion...  
15:15:18 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:15:18 [INFO] Set the system volume path for NETLOGON  
15:15:18 [INFO] vDC Cloning: Winlogon UI Notification #19: Domain Controller cloning is at 73% completion...  
15:15:18 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:15:18 [INFO] Replicating non critical information  
15:15:18 [INFO] User specified to not replicate non-critical data  
15:15:18 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:15:18 [INFO] vDC Cloning: Winlogon UI Notification #20: Domain Controller cloning is at 80% completion...  
15:15:18 [INFO] Stopped the DS  
15:15:18 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:15:18 [INFO] vDC Cloning: Winlogon UI Notification #21: Domain Controller cloning is at 90% completion...  
15:15:18 [INFO] Configuring service NTDS  
15:15:18 [INFO] Configuring service NTDS to 16 returned 0  
```  
  
-   Activer l'inscription DNS du client  
  
```  
15:15:18 [INFO] vDC Cloning: Set DisableDynamicUpdate reg value to 0 to enable dynamic update records registration.  
15:15:18 [INFO] vDC Cloning: Set UseDynamicDns reg value to 1 to enable dynamic update records registration.  
15:15:18 [INFO] vDC Cloning: Set RegistrationEnabled reg value to 1 to enable dynamic update records registration.  
```  
  
-   Exécutez les modules SYSPREP spécifiés par l'élément <SysprepInformation> du fichier DefaultDCCloneAllowList.xml.  
  
```  
15:15:18 [INFO] vDC Cloning: Running sysprep providers.  
15:15:32 [INFO] vDC Cloning: Completed running sysprep providers.  
```  
  
-   La promotion du clonage est terminée  
  
-   Supprimer l'indicateur de démarrage DSRM pour permettre au serveur de démarrer normalement la prochaine fois  
  
-   Renommer le fichier dccloneconfig.xml pour ne pas qu'il soit lu à nouveau au prochain démarrage  
  
-   Redémarrer l'ordinateur  
  
```  
15:15:32 [INFO] The attempted domain controller operation has completed  
15:15:32 [INFO] Updating service status to 4  
15:15:32 [INFO] DsRolepSetOperationDone returned 0  
15:15:32 [INFO] vDC Cloning: Set vDCCloningComplete event.  
15:15:32 [INFO] vDC Cloneing: Clearing Boot into DSRM flag succeeded.  
15:15:32 [INFO] vDC Cloning: Winlogon UI Notification #22: Cloning Domain Controller succeeded. Now rebooting...  
15:15:33 [INFO] vDC Cloning: Renamed vDC clone configuration file.  
15:15:33 [INFO] vDC Cloning: The old name is: C:\Windows\NTDS\DCCloneConfig.xml  
15:15:33 [INFO] vDC Cloning: The new name is: C:\Windows\NTDS\DCCloneConfig.20120207-151533.xml  
15:15:34 [INFO] vDC Cloning: Release Ipv4 on interface 'Wired Ethernet Connection 2', result=0.  
15:15:34 [INFO] vDC Cloning: Release Ipv6 on interface 'Wired Ethernet Connection 2', result=0.  
15:15:34 [INFO] Rebooting machine  
```  
  
##### <a name="active-directory-web-services-event-log"></a>Journal des événements des services Web Active Directory  
Durant le clonage, la base de données NTDS.DIT est souvent hors connexion pendant de longues périodes. Le service Services Web Active Directory enregistre au moins un événement à cette occasion. Une fois le clonage terminé, le service Services Web Active Directory démarre, constate qu'il n'y a pas encore de certificat d'ordinateur valide (il peut y en avoir un ou non, selon qu'un certificat Microsoft PKI avec inscription automatique est déployé ou non dans votre environnement), puis démarre l'instance du nouveau contrôleur de domaine.  
  
||||  
|-|-|-|  
|**ID d’événement**|**Source**|**Message**|  
|**1202**|Événements de l'instance des services Web Active Directory|Cet ordinateur héberge désormais l'instance d'annuaire spécifiée, mais les services Web Active Directory n'ont pas pu la traiter. Les services Web Active Directory vont réessayer cette opération régulièrement.<br /><br />Instance de l'annuaire : NTDS<br /><br />Port LDAP de l'instance de l'annuaire : 389<br /><br />Port SSL de l'instance de l'annuaire : 636|  
|**1000**|Événements de l'instance des services Web Active Directory|Démarrage des services Web Active Directory|  
|**1008**|Événements de l'instance des services Web Active Directory|Les services Web Active Directory ont réussi à réduire leurs privilèges de sécurité|  
|**1100**|Événements de l'instance des services Web Active Directory|Les valeurs spécifiées dans la section <appsettings> du fichier de configuration des services Web Active Directory ont été chargées sans erreurs.|  
|**1400**|Événements de l'instance des services Web Active Directory|Événements de certificat des services Web Active Directory. Les services Web Active Directory n'ont pas pu trouver un certificat de serveur sous le nom de certificat spécifié. Un certificat est requis pour utiliser des connexions SSL/TLS. Pour utiliser des connexions SSL/TLS, vérifiez qu'un certificat d'authentification de serveur valide provenant d'une autorité de certification (CA) approuvée est installé sur l'ordinateur.<br /><br />Nom du certificat : *<Server FQDN>*|  
|**1100**|Événements de l'instance des services Web Active Directory|Les valeurs spécifiées dans la section <appsettings> du fichier de configuration des services Web Active Directory ont été chargées sans erreurs.|  
|**1200**|Événements de l'instance des services Web Active Directory|Les services Web Active Directory traitent l'instance d'annuaire spécifiée.<br /><br />Instance de l'annuaire : NTDS<br /><br />Port LDAP de l'instance de l'annuaire : 389<br /><br />Port SSL de l'instance de l'annuaire : 636|  
  
##### <a name="dns-server-event-log"></a>Journal des événements du serveur DNS  
Le service DNS est brièvement interrompu à quelques reprises durant le clonage, car il reste en cours d'exécution pendant que la base de données AD DS est hors connexion. Cela se produit si vous utilisez le DNS intégré à Active Directory, mais pas si vous utilisez le DNS principal ou le DNS secondaire. Ces erreurs sont enregistrées plusieurs fois. Une fois le clonage terminé, le DNS revient en ligne normalement.  
  
||||  
|-|-|-|  
|**ID d’événement**|**Source**|**Message**|  
|**4013**|DNS-Server-Service|Le serveur DNS attend que les services de domaine Active Directory indiquent que la synchronisation initiale du répertoire est terminée. Ce service du serveur DNS ne peut pas démarrer tant que la synchronisation initiale n'est pas terminée car les données DNS essentielles ne sont peut-être pas encore répliquées sur ce contrôleur de domaine. Si les événements du journal des événements des services de domaine Active Directory indiquent un problème de résolution de noms DNS, envisagez d'ajouter l'adresse IP d'un autre serveur DNS de ce domaine à la liste des serveurs DNS dans les propriétés de protocole Internet de cet ordinateur. Cet événement sera consigné toutes les deux minutes jusqu'à ce que les services de domaine Active Directory indiquent que la synchronisation initiale est terminée.|  
|**4015**|DNS-Server-Service|Le serveur DNS a rencontré une erreur critique venant d'Active Directory. Vérifiez qu'Active Directory fonctionne correctement. Les informations de débogage d'erreur étendues (qui peuvent être vides) sont """". Les données d'événement contiennent l'erreur.|  
|**4000**|DNS-Server-Service|Le serveur DNS n'a pas pu ouvrir Active Directory.  Ce serveur DNS est configuré pour obtenir et utiliser les informations de l'annuaire pour cette zone et il ne peut pas charger la zone sans celui-ci.  Vérifiez qu'Active Directory fonctionne correctement et rechargez la zone. Les données d'événement sont le code d'erreur.|  
|**4013**|DNS-Server-Service|Le serveur DNS attend que les services de domaine Active Directory indiquent que la synchronisation initiale du répertoire est terminée. Ce service du serveur DNS ne peut pas démarrer tant que la synchronisation initiale n'est pas terminée car les données DNS essentielles ne sont peut-être pas encore répliquées sur ce contrôleur de domaine. Si les événements du journal des événements des services de domaine Active Directory indiquent un problème de résolution de noms DNS, envisagez d'ajouter l'adresse IP d'un autre serveur DNS de ce domaine à la liste des serveurs DNS dans les propriétés de protocole Internet de cet ordinateur. Cet événement sera consigné toutes les deux minutes jusqu'à ce que les services de domaine Active Directory indiquent que la synchronisation initiale est terminée.|  
|**2**|DNS-Server-Service|Le serveur DNS a démarré.|  
|**4**|DNS-Server-Service|Le serveur DNS a terminé le chargement en arrière-plan des zones. Toutes les zones sont à présent disponibles pour des mises à jour DNS et des transferts de zone, comme le permet chaque configuration de zone.|  
  
##### <a name="file-replication-service-event-log"></a>Journal des événements du service de réplication de fichiers  
Durant le clonage, le service de réplication de fichiers effectue une synchronisation ne faisant pas autorité à partir d'un partenaire. En effet, le clonage supprime les fichiers de base de données NTFRS et laisse le contenu du répertoire SYSVOL intact, ce qui permet de l'utiliser en tant que données des valeurs de départ. Les deux tentatives de synchronisation sont attendues.  
  
||||  
|-|-|-|  
|**ID d’événement**|**Source**|**Message**|  
|**13562**|NtFrs|Ci-dessous se trouve un résumé des avertissements et des erreurs rencontrés par le service de réplication des fichiers lors de l'interrogation du contrôleur de domaine DC2.root.fabrikam.com concernant les informations de configuration du jeu de réplicas FRS.<br /><br />Impossible de lier à un contrôleur de domaine. Nouvelle tentative lors du prochain cycle d'interrogation|  
|**13502**|NtFrs|Le service de réplication de fichiers est en cours d'arrêt.|  
|**13565**|NtFrs|Le service de réplication de fichiers initialise le volume système avec des données venant d'un autre contrôleur de domaine. L'ordinateur DC2 ne peut pas devenir un contrôleur de domaine avant que le traitement ne soit effectué. Le volume système sera ensuite partagé en tant que SYSVOL.<br /><br />Pour vérifier le partage SYSVOL, à l'invite de commandes, entrez :<br /><br />net share<br /><br />Lorsque le service de réplication de fichiers aura effectué le processus d'initialisation, le partage SYSVOL apparaîtra.<br /><br />L'initialisation du volume système peut prendre un certain temps. Cette durée dépend de la quantité de données dans le volume système, de la disponibilité d'autres contrôleurs de domaine et de l'intervalle de réplication entre les contrôleurs de domaine.|  
|**13501**|NtFrs|Le service de réplication de fichiers démarre|  
|**13502**|NtFrs|Le service de réplication de fichiers est en cours d'arrêt.|  
|**13503**|NtFrs|Le service de réplication de fichiers s'est arrêté.|  
|**13565**|NtFrs|Le service de réplication de fichiers initialise le volume système avec des données venant d'un autre contrôleur de domaine. L'ordinateur DC2 ne peut pas devenir un contrôleur de domaine avant que le traitement ne soit effectué. Le volume système sera ensuite partagé en tant que SYSVOL.<br /><br />Pour vérifier le partage SYSVOL, à l'invite de commandes, entrez :<br /><br />net share<br /><br />Lorsque le service de réplication de fichiers aura effectué le processus d'initialisation, le partage SYSVOL apparaîtra.<br /><br />L'initialisation du volume système peut prendre un certain temps. Cette durée dépend de la quantité de données dans le volume système, de la disponibilité d'autres contrôleurs de domaine et de l'intervalle de réplication entre les contrôleurs de domaine.|  
|**13501**|NtFrs|Le service de réplication de fichiers démarre.|  
|**13553**|NtFrs|Le service de réplication de fichiers a correctement ajouté cet ordinateur au jeu de réplica suivant :<br /><br />« VOLUME SYSTÈME DE DOMAINE (PARTAGE SYSVOL) »<br /><br />Les informations en rapport avec cet événement sont affichées ci-dessous :<br /><br />Nom DNS de l’ordinateur est  *<Domain Controller FQDN>*<br /><br />Nom de membre de jeu de réplica est *<Domain Controller>*<br /><br />Le chemin d’accès de réplica jeu racine est *<path>*<br /><br />Chemin du répertoire intermédiaire réplica est *<path>*<br /><br />Chemin d’accès de répertoire de travail réplica est *<path>*|  
|**13520**|NtFrs|Le Service de réplication de fichiers a déplacé les fichiers préexistants <path>à *<path>* \NtFrs_PreExisting___See_EventLog.<br /><br />Le Service de réplication de fichiers peut supprimer les fichiers dans *<path>* \NtFrs_PreExisting___See_EventLog à tout moment. Fichiers peuvent être enregistrés à partir de la suppression en les copiant hors *<path>* \NtFrs_PreExisting___See_EventLog. La copie des fichiers dans c:\windows\sysvol\domain peut provoquer des conflits de nom si les fichiers existent déjà sur d'autres partenaires de réplication.<br /><br />Dans certains cas, le Service de réplication de fichiers peut copier un fichier depuis *<path>* \NtFrs_PreExisting___See_EventLog vers *<path>* au lieu de répliquer le fichier à partir d’autres partenaires de réplication.<br /><br />Espace disque peut être libéré à tout moment en supprimant les fichiers dans *<path>* \NtFrs_PreExisting___See_EventLog. »|  
|**13508**|NtFrs|Service de réplication de fichiers a des problèmes à activer la réplication de *\\ \\ <Domain Controller FQDN>* à *<Domain Controller>* pour *<path>* à l’aide de la<br /><br />Nom DNS *\\ \\ <Domain Controller FQDN>*. FRS va essayer à nouveau.<br /><br />Ci-dessous sont certaines des raisons de cet avertissement.<br /><br />[1] FRS ne peut pas résoudre correctement le nom DNS *\\ \\ <Domain Controller FQDN>* à partir de cet ordinateur.<br /><br />[2] FRS n’est pas en cours d’exécution *\\ \\ <Domain Controller FQDN>*.<br /><br />[3] Les informations de topologie dans Active Directory pour ce réplica n'ont pas été répliquées vers tous les contrôleurs de domaine.<br /><br />Ce message du journal des événements apparaîtra une fois par connexion. Une fois que le problème aura été résolu, vous verrez un autre message indiquant que la connexion a été établie.|  
|**13509**|NtFrs|Le Service de réplication de fichiers a activé la réplication de *\\ \\ <Domain Controller FQDN>* à *<Domain Controller>* pour *<Path>* après plusieurs tentatives.|  
|**13516**|NtFrs|Le Service de réplication de fichiers n’empêche plus l’ordinateur *<Domain Controller>* de devenir un contrôleur de domaine. Le volume système a été correctement initialisé et le service Netlogon a été averti du fait que le volume système est maintenant prêt à être partagé en tant que SYSVOL.<br /><br />Entrez « net share » pour vérifier le partage SYSVOL.|  
  
##### <a name="dfs-replication-event-log"></a>Journal des événements de la réplication DFS  
Durant le clonage, le service DFSR effectue une synchronisation ne faisant pas autorité à partir d'un partenaire. En effet, le clonage supprime les fichiers de base de données DFSR et laisse le contenu du répertoire SYSVOL intact, ce qui permet de l'utiliser en tant que données des valeurs de départ. Les deux tentatives de synchronisation sont attendues.  
  
||||  
|-|-|-|  
|**ID d’événement**|**Source**|**Message**|  
|**1004**|DFSR|Le service de réplication DFS a démarré.|  
|**1314**|DFSR|Le service de réplication DFS a correctement configuré les fichiers journaux de débogage.<br /><br />Informations supplémentaires :<br /><br />Chemin d'accès aux fichiers journaux de débogage : C:\Windows\debug|  
|**6102**|DFSR|Le service de réplication DFS a inscrit le fournisseur WMI|  
|**1206**|DFSR|Le service de réplication DFS a contacté le contrôleur de domaine DC2.corp.contoso.com pour accéder aux informations de configuration.|  
|**1210**|DFSR|Le service de réplication DFS a créé un écouteur RPC pour les demandes de réplication entrantes.<br /><br />Informations supplémentaires :<br /><br />Port : 0"|  
|**4614**|DFSR|Le service de réplication DFS a initialisé SYSVOL dans le chemin d'accès local C:\Windows\SYSVOL\domain et attend d'effectuer la réplication initiale. Le dossier répliqué demeurera dans son état de synchronisation initial jusqu'à ce qu'il se réplique avec son partenaire. Si le serveur va être promu contrôleur de domaine, le contrôleur de domaine ne sera pas publié et ne fonctionnera pas comme contrôleur de domaine jusqu'à ce que ce problème soit résolu. Cela peut se produire si le partenaire spécifié est lui-même dans l'état de synchronisation initial, ou si des violations de partage sont rencontrées sur ce serveur ou sur le partenaire de synchronisation. Si cet événement s'est produit pendant la migration de SYSVOL depuis le service de réplication de fichiers (FRS) vers le service de réplication DFS, les modifications ne seront pas répliquées jusqu'à ce que ce problème soit résolu. Le dossier SYSVOL sur ce serveur pourrait devenir désynchronisé avec les autres contrôleurs de domaine.<br /><br />Informations supplémentaires :<br /><br />Nom du dossier répliqué : Partage SYSVOL<br /><br />ID du dossier répliqué : *<GUID>*<br /><br />Nom du groupe de réplication : Volume système de domaine<br /><br />ID du groupe de réplication : *<GUID>*<br /><br />ID de membre : *<GUID>*<br /><br />Lecture seule : 0|  
|**4604**|DFSR|Le service de réplication DFS a initialisé la réplication sur le dossier répliqué dans le chemin d'accès local C:\Windows\SYSVOL\domain. Ce membre a terminé la synchronisation initiale de SYSVOL avec le partenaire dc1.corp.contoso.com.  Pour vérifier la présence du partage SYSVOL, ouvrez une fenêtre d'invite de commandes et tapez « net share ».<br /><br />Informations supplémentaires :<br /><br />Nom du dossier répliqué : Partage SYSVOL<br /><br />ID du dossier répliqué : *<GUID>*<br /><br />Nom du groupe de réplication : Volume système de domaine<br /><br />ID du groupe de réplication : *<GUID>*<br /><br />ID de membre : *<GUID>*<br /><br />Partenaire de synchronisation : *<domain controller FQDN>*|  
  
## <a name="BKMK_TshootVDCSafeRestore"></a>Résolution des problèmes de restauration sécurisée des contrôleurs de domaine virtualisés  
  
### <a name="tools-for-troubleshooting"></a>Outils de résolution des problèmes  
  
#### <a name="logging-options"></a>Options de journalisation  
Les journaux intégrés sont l'outil le plus important pour la résolution des problèmes de restauration de capture instantanée sécurisée du contrôleur de domaine. Par défaut, tous ces journaux sont activés et configurés pour fournir un mode détaillé maximal.  
  
|||  
|-|-|  
|**Opération**|**Log**|  
|**Création d’instantanés**|-Événements viewer\Applications et services services\microsoft\windows\hyper-V-Worker|  
|**Restauration de capture instantanée**|-Événements viewer\Applications et des services\Service<br />-Observateur d’événements\journaux Windows\Système<br />-Événements événements\journaux Windows\Application<br />-Événements viewer\Applications et services services\Service de réplication<br />-Événements viewer\Applications et services des services\réplication DFS<br />-Événements viewer\Applications et des services\dns<br />-Événements viewer\Applications et services services\microsoft\windows\hyper-V-Worker|  
  
#### <a name="tools-and-commands-for-troubleshooting-domain-controller-configuration"></a>Outils et commandes pour la résolution des problèmes de configuration des contrôleurs de domaine  
Pour résoudre les problèmes non décrits par les journaux, utilisez les outils suivants comme point de départ :  
  
-   Dcdiag.exe  
  
-   Repadmin.exe  
  
-   Moniteur réseau 3.4  
  
#### <a name="BKMK_TshhotSafeRestore"></a>Méthodologie générale pour la résolution des problèmes de restauration sécurisée des contrôleurs de domaine  
  
1.  Est-ce que la restauration prévue de la capture instantanée présente des problèmes ?  
  
    1.  Examinez le journal des événements Services d'annuaire  
  
        1.  Existe-t-il des erreurs de restauration de capture instantanée ?  
  
        2.  Existe-t-il des erreurs de réplication Active Directory ?  
  
    2.  Examinez le journal des événements Système  
  
        1.  Existe-t-il des erreurs de communication ?  
  
        2.  Existe-t-il des erreurs Active Directory ?  
  
2.  Est-ce que la restauration sécurisée de capture instantanée est inattendue ?  
  
    1.  Examiner les journaux d'audit de l'hyperviseur pour déterminer l'origine de la restauration  
  
    2.  Contacter tous les administrateurs de l'hyperviseur et les interroger pour savoir qui a restauré l'ordinateur virtuel sans notification  
  
3.  Est-ce que le serveur implémente la protection de restauration USN sans restauration sécurisée ?  
  
    1.  Examiner le journal des événements Services d'annuaire à la recherche d'un hyperviseur ou de services d'intégration non pris en charge  
  
    2.  Examiner le système d'exploitation et valider l'exécution de Windows Server 2012 ?  
  
### <a name="BKMK_TshootSpecificSafeRestore"></a>Résolution des problèmes spécifiques  
  
#### <a name="events"></a>Événements  
Tous les événements de restauration de capture instantanée sécurisée pour des contrôleurs de domaine virtualisés sont écrits dans le journal des événements Services d'annuaire de l'ordinateur virtuel du contrôleur de domaine clone. Les journaux des événements Application, Système, Service de réplication de fichiers et Réplication DFS peuvent également contenir des informations utiles pour résoudre les problèmes d'échec de restauration.  
  
Voici les événements de restauration sécurisée pour Windows Server 2012 dans le journal des événements Services d'annuaire.  
  
|||  
|-|-|  
|**ID d’événement**|**2170**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Niveau de gravité**|Warning|  
|**Message**|Un changement d'ID de génération a été détecté.<br /><br />ID de génération mis en cache dans les services d'annuaire (ancienne valeur) : %1<br /><br />ID de génération actuellement dans l'ordinateur virtuel (nouvelle valeur) : %2<br /><br />Le changement d'ID de génération se produit après l'application d'une capture instantanée d'ordinateur virtuel, après une opération d'importation d'ordinateur virtuel ou après une opération de migration dynamique. *<COMPUTERNAME>* Crée un nouvel ID d’appel pour récupérer le contrôleur de domaine. Les contrôleurs de domaine virtualisés ne doivent pas être restaurés à l'aide de captures instantanées d'ordinateurs virtuels. La méthode prise en charge pour restaurer le contenu d'une base de données des services de domaine Active Directory consiste à restaurer une sauvegarde de l'état du système effectuée à l'aide d'une application de sauvegarde compatible avec les services de domaine Active Directory.|  
|**Notes de publication et de résolution**|Il s'agit d'un événement de succès si la capture instantanée est attendue. Sinon, examinez le journal des événements Hyper-V-Worker ou contactez l'administrateur de l'hyperviseur.|  
  
|||  
|-|-|  
|**ID d’événement**|**2174**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Niveau de gravité**|Informationnel|  
|**Message**|Le contrôleur de domaine n'est ni un clone du contrôleur de domaine virtuel, ni une capture instantanée de contrôleur de domaine virtuel restauré.|  
|**Notes de publication et de résolution**|Événement attendu durant le démarrage des contrôleurs de domaine physiques ou des contrôleurs de domaine virtualisés qui ne sont pas restaurés à partir d'une capture instantanée|  
  
|||  
|-|-|  
|**ID d’événement**|**2181**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Niveau de gravité**|Informationnel|  
|**Message**|La transaction a été annulée en raison de la restauration de l'ordinateur virtuel à un état antérieur.  Cela se produit après l'application d'une capture instantanée d'ordinateur virtuel, après une opération d'importation d'ordinateur virtuel ou après une opération de migration dynamique.|  
|**Notes de publication et de résolution**|Attendu durant la restauration d'une capture instantanée. Les transactions suivent le changement d'ID de génération d'ordinateur virtuel|  
  
|||  
|-|-|  
|**ID d’événement**|**2185**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Niveau de gravité**|Informationnel|  
|**Message**|*<COMPUTERNAME>* arrêté le service FRS ou DFSR utilisé pour répliquer le dossier SYSVOL.<br /><br />Nom du service : %1<br /><br />Active Directory a détecté que l'ordinateur virtuel qui héberge le contrôleur de domaine a été ramené à un état antérieur. *<COMPUTERNAME>* doit initialiser une restauration ne faisant pas autorité sur le réplica SYSVOL local. Ceci se fait en arrêtant le service de réplication FRS ou DFSR utilisé pour répliquer le dossier SYSVOL et en le démarrant avec les clés et valeurs de Registre appropriées pour déclencher la restauration. L'événement 2187 sera journalisé au redémarrage du service FRS ou DFSR.|  
|**Notes de publication et de résolution**|Attendu durant la restauration d'une capture instantanée. Toutes les données SYSVOL de ce contrôleur de domaine sont remplacées par la copie d'un contrôleur de domaine partenaire.|  
|**ID d’événement**|2186|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Niveau de gravité**|Erreur|  
|**Message**|*<COMPUTERNAME>* Impossible d’arrêter le service FRS ou DFSR utilisé pour répliquer le dossier SYSVOL.<br /><br />Nom du service : %1<br /><br />Code d’erreur : %2<br /><br />Message d’erreur : %3<br /><br />Active Directory a détecté que l'ordinateur virtuel qui héberge le contrôleur de domaine a été ramené à un état antérieur. *<COMPUTERNAME>* doit initialiser une restauration ne faisant pas autorité sur le réplica SYSVOL local. Ceci se fait en arrêtant le service de réplication FRS ou DFSR utilisé pour répliquer le dossier SYSVOL et en le démarrant avec les clés et valeurs de Registre appropriées pour déclencher la restauration. *<COMPUTERNAME>* Impossible d’arrêter le service en cours d’exécution actuel et ne peut pas terminer la restauration ne faisant pas autorité. Effectuez manuellement une restauration ne faisant pas autorité.|  
|**Notes de publication et de résolution**|Pour plus d'informations, examinez les journaux des événements Système, FRS et DFSR.|  
  
|||  
|-|-|  
|**ID d’événement**|**2187**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Niveau de gravité**|Informationnel|  
|**Message**|*<COMPUTERNAME>* Démarrer le service FRS ou DFSR utilisé pour répliquer le dossier SYSVOL.<br /><br />Nom du service : %1<br /><br />Active Directory a détecté que l'ordinateur virtuel qui héberge le contrôleur de domaine a été ramené à un état antérieur. *<COMPUTERNAME>* nécessaire pour initialiser une restauration ne faisant pas autorité sur le réplica SYSVOL local. Ceci a été fait en arrêtant le service de réplication FRS ou DFSR utilisé pour répliquer le dossier SYSVOL et en le démarrant avec les clés et valeurs de Registre appropriées pour déclencher la restauration.|  
|**Notes de publication et de résolution**|Attendu durant la restauration d'une capture instantanée. Toutes les données SYSVOL de ce contrôleur de domaine sont remplacées par la copie d'un contrôleur de domaine partenaire.|  
  
|||  
|-|-|  
|**ID d’événement**|**2188**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Niveau de gravité**|Erreur|  
|**Message**|*<COMPUTERNAME>* Impossible de démarrer le service FRS ou DFSR utilisé pour répliquer le dossier SYSVOL.<br /><br />Nom du service : %1<br /><br />Code d’erreur : %2<br /><br />Message d’erreur : %3<br /><br />Active Directory a détecté que l'ordinateur virtuel qui héberge le contrôleur de domaine a été ramené à un état antérieur. *<COMPUTERNAME>* doit initialiser une restauration ne faisant pas autorité sur le réplica SYSVOL local. Pour ce faire, arrêtez le service de réplication FRS ou DFSR utilisé pour répliquer le dossier SYSVOL et démarrez-le avec les clés et valeurs de Registre appropriées pour déclencher la restauration. *<COMPUTERNAME>* Impossible de démarrer le service FRS ou DFSR utilisé pour répliquer le dossier SYSVOL et ne peut pas terminer la restauration ne faisant pas autorité. Effectuez manuellement une restauration ne faisant pas autorité et redémarrez le service.|  
|**Notes de publication et de résolution**|Pour plus d'informations, examinez les journaux des événements Système, FRS et DFSR.|  
  
|||  
|-|-|  
|**ID d’événement**|**2189**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Niveau de gravité**|Informationnel|  
|**Message**|*<COMPUTERNAME>* Définissez les valeurs de Registre suivantes pour initialiser le réplica SYSVOL lors d’une restauration ne faisant pas autorité :<br /><br />Clé de Registre : %1<br /><br />Valeur de Registre : %2<br /><br />Données de valeur de Registre : %3<br /><br />Active Directory a détecté que l'ordinateur virtuel qui héberge le contrôleur de domaine a été ramené à un état antérieur. *<COMPUTERNAME>* doit initialiser une restauration ne faisant pas autorité sur le réplica SYSVOL local. Ceci se fait en arrêtant le service FRS ou DFSR utilisé pour répliquer le dossier SYSVOL et en le démarrant avec les clés et valeurs de Registre appropriées pour déclencher la restauration.|  
|**Notes de publication et de résolution**|Attendu durant la restauration d'une capture instantanée. Toutes les données SYSVOL de ce contrôleur de domaine sont remplacées par la copie d'un contrôleur de domaine partenaire.|  
  
|||  
|-|-|  
|**ID d’événement**|**2190**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Niveau de gravité**|Erreur|  
|**Message**|*<COMPUTERNAME>* Impossible de définir des valeurs de Registre suivantes pour initialiser le réplica SYSVOL lors d’une restauration ne faisant pas autorité :<br /><br />Clé de Registre : %1<br /><br />Valeur de Registre : %2<br /><br />Données de valeur de Registre : %3<br /><br />Code d’erreur : %4<br /><br />Message d’erreur : %5<br /><br />Active Directory a détecté que l'ordinateur virtuel qui héberge le contrôleur de domaine a été ramené à un état antérieur. *<COMPUTERNAME>* doit initialiser une restauration ne faisant pas autorité sur le réplica SYSVOL local. Ceci se fait en arrêtant le service FRS ou DFSR utilisé pour répliquer le dossier SYSVOL et en le démarrant avec les clés et valeurs de Registre appropriées pour déclencher la restauration. *<COMPUTERNAME>* Impossible de définir les valeurs de Registre ci-dessus et ne peut pas terminer la restauration ne faisant pas autorité. Effectuez manuellement une restauration ne faisant pas autorité.|  
|**Notes de publication et de résolution**|Examinez les journaux des événements Application et Système. Vérifiez si des applications tierces ne bloquent pas les mises à jour du Registre.|  
  
|||  
|-|-|  
|**ID d’événement**|**2200**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Niveau de gravité**|Informationnel|  
|**Message**|Active Directory a détecté que l'ordinateur virtuel qui héberge le contrôleur de domaine a été ramené à un état antérieur. *<COMPUTERNAME>* Initialise la réplication pour mettre à jour le contrôleur de domaine. L'événement 2201 sera consigné au terme de la réplication.|  
|**Notes de publication et de résolution**|Attendu durant la restauration d'une capture instantanée. Marque le début de la réplication Active Directory entrante.|  
  
|||  
|-|-|  
|**ID d’événement**|**2201**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Niveau de gravité**|Informationnel|  
|**Message**|Active Directory a détecté que l'ordinateur virtuel qui héberge le contrôleur de domaine a été ramené à un état antérieur. *<COMPUTERNAME>* a terminé la réplication pour mettre à jour le contrôleur de domaine.|  
|**Notes de publication et de résolution**|Attendu durant la restauration d'une capture instantanée. Marque la fin de la réplication Active Directory entrante.|  
  
|||  
|-|-|  
|**ID d’événement**|**2202**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Niveau de gravité**|Erreur|  
|**Message**|Active Directory a détecté que l'ordinateur virtuel qui héberge le contrôleur de domaine a été ramené à un état antérieur. *<COMPUTERNAME>* réplication en échec pour mettre à jour le contrôleur de domaine. Le contrôleur de domaine sera mis à jour après la prochaine réplication périodique.|  
|**Notes de publication et de résolution**|Examinez les journaux des événements Services d'annuaire et Système. Utilisez repadmin.exe pour tenter de forcer la réplication et notez les défaillances.|  
  
|||  
|-|-|  
|**ID d’événement**|**2204**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Niveau de gravité**|Informationnel|  
|**Message**|*<COMPUTERNAME>* a détecté une modification de l’ID de génération de machine virtuelle. Cette modification signifie que le contrôleur de domaine virtuel a été ramené à un état antérieur. *<COMPUTERNAME>* effectue les opérations suivantes pour protéger le contrôleur de domaine ainsi rétabli contre une possible divergence des données et pour protéger la création des principaux de sécurité avec des SID dupliqués :<br /><br />Créer un ID d'appel<br /><br />Invalider le pool RID actuel<br /><br />La propriété des rôles FSMO sera validée à la prochaine réplication entrante. Pendant ce laps de temps, si le contrôleur de domaine détenait un rôle FSMO, ce rôle sera indisponible.<br /><br />Démarrer l'opération de restauration du service de réplication SYSVOL.<br /><br />Démarrer la réplication pour rétablir à l’état le plus récent le contrôleur de domaine qui a été ramené à un état antérieur.<br /><br />Demander un nouveau pool RID.|  
|**Notes de publication et de résolution**|Attendu durant la restauration d'une capture instantanée. Ceci explique toutes les diverses opérations de réinitialisation qui doivent se produire durant le processus de restauration sécurisée.|  
  
|||  
|-|-|  
|**ID d’événement**|**2205**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Niveau de gravité**|Informationnel|  
|**Message**|*<COMPUTERNAME>* invalidé le pool RID actuel après que le contrôleur de domaine virtuel a été ramené à un état antérieur.|  
|**Notes de publication et de résolution**|Attendu durant la restauration d'une capture instantanée. Le pool RID local doit être détruit, car le contrôleur de domaine a « voyagé dans le temps ». En outre, des identificateurs RID ont peut-être déjà été affectés.|  
  
|||  
|-|-|  
|**ID d’événement**|**2206**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Niveau de gravité**|ERREUR|  
|**Message**|*<COMPUTERNAME>* Impossible d’invalider le pool RID actuel après que le contrôleur de domaine virtuel a été ramené à un état antérieur.<br /><br />Données supplémentaires :<br /><br />Code d’erreur : %1<br /><br />Valeur de l’erreur : %2|  
|**Notes de publication et de résolution**|Examinez les journaux des événements Services d'annuaire et Système. Assurez-vous que le maître RID est en ligne et qu'il est joignable à partir de ce serveur via Dcdiag.exe /test:ridmanager|  
  
|||  
|-|-|  
|**ID d’événement**|**2207**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Niveau de gravité**|ERREUR|  
|**Message**|*<COMPUTERNAME>* Échec de restauration une fois que le contrôleur de domaine virtuel a été ramené à un état antérieur. Un redémarrage dans DSRM a été demandé. Pour plus d'informations, vérifiez les événements antérieurs.|  
|**Notes de publication et de résolution**|Examinez les journaux des événements Services d'annuaire et Système.|  
  
|||  
|-|-|  
|**ID d’événement**|**2208**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Niveau de gravité**|Informationnel|  
|**Message**|*<COMPUTERNAME>* supprimer les bases de données DFSR pour initialiser le réplica SYSVOL lors d’une restauration ne faisant pas autorité.|  
|**Notes de publication et de résolution**|Attendu durant la restauration d'une capture instantanée. Cela permet de s'assurer que le service DFSR effectue une synchronisation ne faisant pas autorité du répertoire SYSVOL à partir d'un contrôleur de domaine partenaire. Notez que les autres dossiers répliqués DFSR situés sur le même volume que SYSVOL font également l'objet d'une synchronisation ne faisant pas autorité (il n'est pas recommandé d'utiliser des contrôleurs de domaine pour héberger des dossiers DFSR personnalisés situés sur le même volume que SYSVOL).|  
  
|||  
|-|-|  
|**ID d’événement**|**2209**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Niveau de gravité**|Erreur|  
|**Message**|*<COMPUTERNAME>* Échec de suppression des bases de données DFSR.<br /><br />Données supplémentaires :<br /><br />Code d’erreur : %1<br /><br />Valeur de l’erreur : %2<br /><br />Active Directory a détecté que l'ordinateur virtuel qui héberge le contrôleur de domaine a été ramené à un état antérieur. *<COMPUTERNAME>* doit initialiser une restauration ne faisant pas autorité sur le réplica SYSVOL local. Pour DFSR, ceci se fait en arrêtant le service DFSR, en supprimant les bases de données DFSR et en redémarrant le service. Après le redémarrage, DFSR recrée les bases de données et démarre la synchronisation initiale.|  
|**Notes de publication et de résolution**|Examinez le journal des événements DFSR.|  
  
#### <a name="error-messages"></a>Messages d'erreur  
Il n'y a pas d'erreurs interactives directes pour les échecs de restauration de capture instantanée sécurisée d'un contrôleur de domaine virtualisé. Toutes les informations relatives au clonage sont enregistrées dans les journaux des événements Services d'annuaire. Naturellement, toutes les erreurs de réplication critique ou de publication de serveur se manifestent ailleurs par d'autres symptômes.  
  
#### <a name="known-issues-and-support-scenarios"></a>Problèmes connus et scénarios de prise en charge  
Le [méthodologie générale pour la résolution des problèmes domaine contrôleur de restauration sécurisée](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_TshootSpecificSafeRestore) sont généralement suffisants pour résoudre la plupart des problèmes.  
  
|||  
|-|-|  
|**Problème**|**Ne peut pas créer de nouvelles entités de sécurité sur le contrôleur de domaine restauré récemment de manière sécurisée**|  
|**Symptômes**|Après la restauration d'une capture instantanée, les tentatives de création d'un principal de sécurité (utilisateur, ordinateur, groupe) sur ce contrôleur de domaine échouent comme suit :<br /><br />Erreur 0x2010<br /><br />Le service d'annuaire n'a pas pu allouer un identificateur relatif.|  
|**Résolution et remarques**|Ce problème est dû aux informations obsolètes du rôle FSMO du maître RID de l'ordinateur restauré. Si le rôle est déplacé vers ce contrôleur de domaine (ou un autre) après qu'une capture instantanée a été effectuée puis restaurée, le contrôleur de domaine restauré n'a pas connaissance du maître RID jusqu'à la fin de la réplication initiale.<br /><br />Pour résoudre le problème, permettez à la réplication Active Directory de s'effectuer en entrée sur le contrôleur de domaine restauré. Si le problème persiste, assurez-vous que tous les contrôleurs de domaine ont la même information valide en ce qui concerne l'identité du contrôleur de domaine hôte du maître RID.|  
  
|||  
|-|-|  
|**Problème**|**Les contrôleurs de domaine restauré ne pas partager SYSVOL, publier**|  
|**Symptômes**|Après la restauration d'une capture instantanée, un ou plusieurs contrôleurs de domaine n'effectuent aucune publication, ne partagent pas sysvol et ne disposent pas d'un contenu SYSVOL actualisé|  
|**Résolution et remarques**|Les partenaires en amont du contrôleur de domaine n'ont aucun réplica SYSVOL de travail qui se réplique correctement avec les services DFSR ou FRS. Ce problème n'est pas lié à la restauration sécurisée. Toutefois, il est susceptible de se manifester en tant que problème de restauration sécurisée, car le client ignore qu'un autre problème de réplication affecte les contrôleurs de domaine non restaurés|  
  
### <a name="advanced-troubleshooting"></a>Résolution avancée des problèmes  
Ce module vise à enseigner la résolution avancée des problèmes à l'aide d'exemples de journaux de *travail* et d'explications. Si vous comprenez en quoi consiste le fonctionnement d'un contrôleur de domaine virtualisé, vous pouvez identifier facilement les défaillances dans votre environnement. Ces journaux sont présentés par source, dans l'ordre croissant des événements *attendus* liés à un contrôleur de domaine cloné au sein de chaque journal.  
  
#### <a name="restoring-a-domain-controller-that-replicates-sysvol-using-dfsr"></a>Restauration d'un contrôleur de domaine qui réplique SYSVOL via les services DFSR  
  
##### <a name="directory-services-event-log"></a>Journal des événements Services d'annuaire  
Le journal Services d'annuaire contient la majorité des informations opérationnelles relatives à la restauration sécurisée. L'hyperviseur change l'ID de génération d'ordinateur virtuel (ce qui est identifié par le service NTDS), puis invalide le pool RID et change l'ID d'invocation. Le nouvel ID de génération d'ordinateur virtuel est défini. Le serveur réplique les données Active Directory en entrée. Le service DFSR est arrêté. Sa base de données, qui héberge SYSVOL, est supprimée, ce qui entraîne une synchronisation forcée faisant autorité en entrée. La limite supérieure du numéro USN est configurée.  
  
||||  
|-|-|-|  
|**ID d’événement**|**Source**|**Message**|  
|**2170**|ActiveDirectory_DomainService|Un changement d'ID de génération a été détecté.<br /><br />ID de génération mis en cache dans les services d'annuaire (ancienne valeur) :<br /><br />*<number>*<br /><br />ID de génération actuellement dans l'ordinateur virtuel (nouvelle valeur) :<br /><br />*<number>*<br /><br />Le changement d'ID de génération se produit après l'application d'une capture instantanée d'ordinateur virtuel, après une opération d'importation d'ordinateur virtuel ou après une opération de migration dynamique. Les services de domaine Active Directory vont créer un ID d'appel pour récupérer le contrôleur de domaine. Les contrôleurs de domaine virtualisés ne doivent pas être restaurés à l'aide de captures instantanées d'ordinateurs virtuels. La méthode prise en charge pour restaurer le contenu d'une base de données des services de domaine Active Directory consiste à restaurer une sauvegarde de l'état du système effectuée à l'aide d'une application de sauvegarde compatible avec les services de domaine Active Directory.|  
|**2181**|ActiveDirectory_DomainService|La transaction a été annulée en raison de la restauration de l'ordinateur virtuel à un état antérieur.  Cela se produit après l'application d'une capture instantanée d'ordinateur virtuel, après une opération d'importation d'ordinateur virtuel ou après une opération de migration dynamique.|  
|**2204**|ActiveDirectory_DomainService|Les services de domaine Active Directory (AD DS) ont détecté une modification de l'ID de génération de l'ordinateur virtuel. Cette modification signifie que le contrôleur de domaine virtuel a été ramené à un état antérieur. Les services de domaine Active Directory effectueront les opérations suivantes pour protéger le contrôleur de domaine ainsi rétabli contre une possible divergence des données et pour protéger la création des principaux de sécurité avec des SID en doublon :<br /><br />Créer un ID d'appel<br /><br />Invalider le pool RID actuel<br /><br />La propriété des rôles FSMO sera validée à la prochaine réplication entrante. Pendant ce laps de temps, si le contrôleur de domaine détenait un rôle FSMO, ce rôle sera indisponible.<br /><br />Démarrer l'opération de restauration du service de réplication SYSVOL.<br /><br />Démarrer la réplication pour rétablir à l'état le plus récent le contrôleur de domaine qui a été ramené à un état antérieur.<br /><br />Demander un nouveau pool RID.|  
|**2181**|ActiveDirectory_DomainService|La transaction a été annulée en raison de la restauration de l'ordinateur virtuel à un état antérieur.  Cela se produit après l'application d'une capture instantanée d'ordinateur virtuel, après une opération d'importation d'ordinateur virtuel ou après une opération de migration dynamique.|  
|**1109**|ActiveDirectory_DomainService|L'attribut invocationID pour ce serveur d'annuaire a été modifié. Le numéro de séquence de mise à jour le plus élevé au moment où la sauvegarde a été créée est le suivant :<br /><br />Attribut InvocationID (ancienne valeur) :<br /><br />*<GUID>*<br /><br />Attribut InvocationID (nouvelle valeur) :<br /><br />*<GUID>*<br /><br />Numéro de séquence de la mise à jour :<br /><br />*<number>*<br /><br />L'attribut invocationID est modifié quand un serveur d'annuaire est restauré à partir d'un support de sauvegarde, est configuré pour héberger une partition d'annuaire d'application accessible en écriture, a été redémarré après l'application d'un instantané d'ordinateur virtuel, après une opération d'importation d'ordinateur virtuel, ou après une opération de migration dynamique. Les contrôleurs de domaine virtualisés ne doivent pas être restaurés à l'aide de captures instantanées d'ordinateurs virtuels. La méthode prise en charge pour restaurer le contenu d'une base de données des services de domaine Active Directory consiste à restaurer une sauvegarde de l'état du système effectuée à l'aide d'une application de sauvegarde compatible avec les services de domaine Active Directory.|  
|**2179**|ActiveDirectory_DomainService|L'attribut msDS-GenerationId de l'objet ordinateur du contrôleur de domaine a été paramétré comme suit :<br /><br />Attribut GenerationID :<br /><br />*<number>*|  
|**2200**|ActiveDirectory_DomainService|Active Directory a détecté que l'ordinateur virtuel qui héberge le contrôleur de domaine a été ramené à un état antérieur. Les services de domaine Active Directory initialisent la réplication pour mettre à jour le contrôleur de domaine. L'événement 2201 sera consigné au terme de la réplication.|  
|**2201**|ActiveDirectory_DomainService|Active Directory a détecté que l'ordinateur virtuel qui héberge le contrôleur de domaine a été ramené à un état antérieur. Les services de domaine Active Directory ont terminé la réplication pour mettre à jour le contrôleur de domaine.|  
|**2185**|ActiveDirectory_DomainService|Les services de domaine Active Directory ont arrêté le service FRS ou DFSR utilisé pour répliquer le dossier SYSVOL.<br /><br />Nom du service :<br /><br />DFSR<br /><br />Active Directory a détecté que l'ordinateur virtuel qui héberge le contrôleur de domaine a été ramené à un état antérieur. Les services de domaine Active Directory doivent initialiser une restauration ne faisant pas autorité sur le réplica SYSVOL local. Ceci se fait en arrêtant le service de réplication FRS ou DFSR utilisé pour répliquer le dossier SYSVOL et en le démarrant avec les clés et valeurs de Registre appropriées pour déclencher la restauration. L'événement 2187 sera journalisé au redémarrage du service FRS ou DFSR.|  
|**2208**|ActiveDirectory_DomainService|Les services de domaine Active Directory ont supprimé les bases de données DFSR pour initialiser un réplica SYSVOL lors d'une restauration ne faisant pas autorité.<br /><br />Active Directory a détecté que l'ordinateur virtuel qui héberge le contrôleur de domaine a été ramené à un état antérieur. Les services de domaine Active Directory doivent initialiser une restauration ne faisant pas autorité sur le réplica SYSVOL local. Pour DFSR, ceci se fait en arrêtant le service DFSR, en supprimant les bases de données DFSR et en redémarrant le service. Lors du redémarrage DFSR sera reconstruire les bases de données et démarre la synchronisation initiale. "|  
|**2187**|ActiveDirectory_DomainService|Les services de domaine Active Directory ont démarré le service FRS ou DFSR utilisé pour répliquer le dossier SYSVOL.<br /><br />Nom du service :<br /><br />DFSR<br /><br />Active Directory a détecté que l'ordinateur virtuel qui héberge le contrôleur de domaine a été ramené à un état antérieur. Les services de domaine Active Directory ont dû initialiser une restauration ne faisant pas autorité sur le réplica SYSVOL local. Ceci a été fait en arrêtant le service de réplication FRS ou DFSR utilisé pour répliquer le dossier SYSVOL et en le démarrant avec les clés et valeurs de Registre appropriées pour déclencher la restauration. "|  
|**1587**|ActiveDirectory_DomainService|Ce service d'annuaire a été restauré ou a été configuré pour héberger une partition d'annuaire d'application. Par conséquent, son identité de réplication a été modifiée. Un partenaire a demandé des modifications de réplication en utilisant notre ancienne identité. Le numéro de séquence de démarrage a été ajusté.<br /><br />Le service d'annuaire de destination correspondant au GUID d'objet suivant a demandé le démarrage des modifications à un numéro de séquence de mise à jour (USN) précédant l'USN où le service d'annuaire local a été restauré à partir du média de sauvegarde.<br /><br />GUID de l'objet :<br /><br />*<GUID> (<FQDN of partner domain controller>)*<br /><br />Numéro de séquence de mise à jour au moment de la restauration :<br /><br />*<number>*<br /><br />Par conséquent, le vecteur de mise à jour récente du service d'annuaire de destination a été configuré avec les paramètres suivants.<br /><br />Précédent GUID de la base de données :<br /><br />*<GUID>*<br /><br />Précédent USN de l'objet :<br /><br />*<number>*<br /><br />Précédent USN de la propriété :<br /><br />*<number>*<br /><br />Nouveau GUID de la base de données :<br /><br />*<GUID>*<br /><br />Nouvel USN de l'objet :<br /><br />*<number>*<br /><br />Nouvel USN de la propriété :<br /><br />*<number>*|  
  
##### <a name="system-event-log"></a>Journal des événements Système  
Le journal des événements Système note l'heure de l'ordinateur durant la remise en ligne d'un ordinateur virtuel et sa synchronisation avec l'heure de l'hôte. Le pool RID est invalidé, et les services DFSR ou FRS redémarrent.  
  
||||  
|-|-|-|  
|**ID d’événement**|**Source**|**Message**|  
|**1**|Kernel-General|L’heure système est passée de *?<now>* à partir de *< date/heure de capture instantanée >*.<br /><br />Raison de la modification : Une application ou un composant système a changé l'heure.|  
|**16654**|Directory-Services-SAM|Un pool d'identificateurs de comptes (RID) a été invalidé Ceci peut se produire dans les cas attendus suivants :<br /><br />1. Un contrôleur de domaine est restauré à partir de la sauvegarde.<br /><br />2. Un contrôleur de domaine exécuté sur un ordinateur virtuel est restauré à partir de la capture instantanée.<br /><br />3. Un administrateur a invalidé le pool manuellement.<br /><br />Pour plus d'informations, consultez https://go.microsoft.com/fwlink/?LinkId=226247.|  
|**7036**|Gestionnaire de contrôle des services|Le service de réplication DFS est entré dans l'état d'arrêt.|  
|**7036**|Gestionnaire de contrôle des services|Le service de réplication DFS est entré dans l'état en cours d'exécution.|  
  
##### <a name="application-event-log"></a>Journal des événements Application  
Le journal des événements Application note l'arrêt et le démarrage de la base de données DFSR.  
  
||||  
|-|-|-|  
|**ID d’événement**|**Source**|**Message**|  
|**103**|ESENT|DFSRs (1360) \\ \\.\C:\System Volume Information\DFSR\database *_<GUID>* \dfsr.db : Le moteur de base de données a arrêté l'instance (0).<br /><br />Arrêt brutal : 0<br /><br />Séquence de minutage interne : [1] 0.000, [2] 0.000, [3] 0.000, [4] 0.000, [5] 0.141, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.000, [10] 0.000, [11] 0.016, [12] 0.000, [13] 0.000, [14] 0.000, [15] 0.000.|  
|**102**|ESENT|DFSRs (532) \\ \\.\C:\System Volume Information\DFSR\database *_<GUID>* \dfsr.db : Le moteur de base de données (6.02.8189.0000) démarre une nouvelle instance (0).|  
|**105**|ESENT|DFSRs (532) \\ \\.\C:\System Volume Information\DFSR\database *_<GUID>* \dfsr.db : Le moteur de base de données a démarré une nouvelle instance (0). (Délai = 0 seconde)<br /><br />Séquence de minutage interne : [1] 0.000, [2] 0.000, [3] 0.000, [4] 0.000, [5] 0.000, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.031, [10] 0.000, [11] 0.000.|  
|||DFSRs (532) \\ \\.\C:\System Volume Information\DFSR\database *_<GUID>* \dfsr.db : Le moteur de base de données a créé une base de données (1, \\ \\.\C:\System Volume Information\DFSR\database *_<GUID>* \dfsr.db). (Délai = 0 seconde)<br /><br />Séquence de temporisation interne : [1] 0.000, [2] 0.000, [3] 0.016, [4] 0.062, [5] 0.000, [6] 0.016, [7] 0.000, [8] 0.000, [9] 0.015, [10] 0.000, [11] 0.000.|  
  
##### <a name="dfs-replication-event-log"></a>Journal des événements de la réplication DFS  
Le service DFSR est arrêté. La base de données qui contient SYSVOL est supprimée, ce qui entraîne une synchronisation forcée faisant autorité en entrée.  
  
||||  
|-|-|-|  
|**ID d’événement**|**Source**|**Message**|  
|**1006**|DFSR|Le service de réplication DFS est en cours d'arrêt.|  
|**1008**|DFSR|Le service de réplication DFS s'est arrêté.|  
|**1002**|DFSR|Le service de réplication DFS est en cours de démarrage.|  
|**1004**|DFSR|Le service de réplication DFS a démarré.|  
|**1314**|DFSR|Le service de réplication DFS a correctement configuré les fichiers journaux de débogage.<br /><br />Informations supplémentaires :<br /><br />Chemin d'accès aux fichiers journaux de débogage : C:\Windows\debug|  
|**6102**|DFSR|Le service de réplication DFS a inscrit le fournisseur WMI.|  
|**1206**|DFSR|Le contrôleur de domaine a contacté de service de réplication DFS *<domain controller FQDN>* pour accéder aux informations de configuration.|  
|**1210**|DFSR|Le service de réplication DFS a créé un écouteur RPC pour les demandes de réplication entrantes.<br /><br />Informations supplémentaires :<br /><br />Port : 0|  
|**4614**|DFSR|Le service de réplication DFS a initialisé SYSVOL dans le chemin d'accès local C:\Windows\SYSVOL\domain et attend d'effectuer la réplication initiale. Le dossier répliqué demeurera dans son état de synchronisation initial jusqu'à ce qu'il se réplique avec son partenaire. Si le serveur va être promu contrôleur de domaine, le contrôleur de domaine ne sera pas publié et ne fonctionnera pas comme contrôleur de domaine jusqu'à ce que ce problème soit résolu. Cela peut se produire si le partenaire spécifié est lui-même dans l'état de synchronisation initial, ou si des violations de partage sont rencontrées sur ce serveur ou sur le partenaire de synchronisation. Si cet événement s'est produit pendant la migration de SYSVOL depuis le service de réplication de fichiers (FRS) vers le service de réplication DFS, les modifications ne seront pas répliquées jusqu'à ce que ce problème soit résolu. Le dossier SYSVOL sur ce serveur pourrait devenir désynchronisé avec les autres contrôleurs de domaine.<br /><br />Informations supplémentaires :<br /><br />Nom du dossier répliqué : Partage SYSVOL<br /><br />ID du dossier répliqué : *<GUID>*<br /><br />Nom du groupe de réplication : Volume système de domaine<br /><br />ID du groupe de réplication : *<GUID>*<br /><br />ID de membre : *<GUID>*<br /><br />Lecture seule : 0|  
|**4604**|DFSR|Le service de réplication DFS a initialisé la réplication sur le dossier répliqué dans le chemin d'accès local C:\Windows\SYSVOL\domain. Ce membre a terminé la synchronisation initiale de SYSVOL avec le partenaire dc1.corp.contoso.com.  Pour vérifier la présence du partage SYSVOL, ouvrez une fenêtre d'invite de commandes et tapez « net share ».<br /><br />Informations supplémentaires :<br /><br />Nom du dossier répliqué : Partage SYSVOL<br /><br />ID du dossier répliqué : *<GUID>*<br /><br />Nom du groupe de réplication : Volume système de domaine<br /><br />ID du groupe de réplication : *<GUID>*<br /><br />ID de membre : *<GUID>*<br /><br />Partenaire de synchronisation : *<partner domain controller FQDN>*|  
  
#### <a name="restoring-a-domain-controller-that-replicates-sysvol-using-frs"></a>Restauration d'un contrôleur de domaine qui réplique SYSVOL via les services FRS  
Le journal des événements de réplication de fichiers est utilisé à la place du journal des événements DFSR dans ce cas. Le journal des événements Application consigne également différents événements liés au service FRS. Sinon, les messages des journaux des événements Services d'annuaire et Système sont généralement les mêmes et se présentent dans le même ordre que décrit précédemment.  
  
##### <a name="file-replication-service-event-log"></a>Journal des événements du service de réplication de fichiers  
Le service FRS est arrêté et redémarré avec la valeur D2 BURFLAGS pour une synchronisation de SYSVOL ne faisant pas autorité.  
  
||||  
|-|-|-|  
|**ID d’événement**|**Source**|**Message**|  
|**13502**|NTFRS|Le service de réplication de fichiers est en cours d'arrêt.|  
|**13503**|NTFRS|Le service de réplication de fichiers s'est arrêté.|  
|**13501**|NTFRS|Le service de réplication de fichiers démarre|  
|**13512**|NTFRS|Le service de réplication de fichiers a détecté du cache d'écriture sur disque activé sur le lecteur contenant l'annuaire c:\windows\ntfrs\jet sur l'ordinateur DC4. Le service de réplication de fichiers peut ne pas être rétabli lorsque l'alimentation du lecteur est interrompue et que les mises à jour critiques sont perdues.|  
|**13565**|NTFRS|Le service de réplication de fichiers initialise le volume système avec des données venant d'un autre contrôleur de domaine. L'ordinateur DC4 ne peut pas devenir un contrôleur de domaine avant que le traitement ne soit effectué. Le volume système sera ensuite partagé en tant que SYSVOL.<br /><br />Pour vérifier le partage SYSVOL, à l'invite de commandes, entrez :<br /><br />net share<br /><br />Lorsque le service de réplication de fichiers aura effectué le processus d'initialisation, le partage SYSVOL apparaîtra.<br /><br />L'initialisation du volume système peut prendre un certain temps. Cette durée dépend de la quantité de données dans le volume système, de la disponibilité d'autres contrôleurs de domaine et de l'intervalle de réplication entre les contrôleurs de domaine.|  
|**13520**|NTFRS|Le Service de réplication de fichiers a déplacé les fichiers préexistants *<path>* à *<path>* \NtFrs_PreExisting___See_EventLog.<br /><br />Le Service de réplication de fichiers peut supprimer les fichiers dans *<path>* \NtFrs_PreExisting___See_EventLog à tout moment. Fichiers peuvent être enregistrés à partir de la suppression en les copiant hors *<path>* \NtFrs_PreExisting___See_EventLog. Copie des fichiers dans *<path>* peut provoquer des conflits de nom si les fichiers existent déjà sur d’autres partenaires de réplication.<br /><br />Dans certains cas, le Service de réplication de fichiers peut copier un fichier depuis *<path>* \NtFrs_PreExisting___See_EventLog vers *<path>* au lieu de répliquer le fichier à partir d’autres partenaires de réplication.<br /><br />Espace disque peut être libéré à tout moment en supprimant les fichiers dans *<path>* \NtFrs_PreExisting___See_EventLog.|  
|**13553**|NTFRS|Le service de réplication de fichiers a correctement ajouté cet ordinateur au jeu de réplica suivant :<br /><br />« VOLUME SYSTÈME DE DOMAINE (PARTAGE SYSVOL) »<br /><br />Les informations en rapport avec cet événement sont affichées ci-dessous :<br /><br />Nom DNS de l’ordinateur est «*<domain controller FQDN>*»<br /><br />Nom de membre de jeu de réplica est «*<domain controller name>*»<br /><br />Chemin de racine de jeu de réplica est «*<path>*»<br /><br />Chemin d’accès du répertoire intermédiaire de réplica est «*<path>* »<br /><br />Chemin d’accès de répertoire de travail réplica est «*<path>*»|  
|**13554**|NTFRS|Le service de réplication de fichiers a correctement ajouté les connexions affichées ci-dessous au jeu de réplica :<br /><br />« VOLUME SYSTÈME DE DOMAINE (PARTAGE SYSVOL) »<br /><br />Trafic entrant à partir de «*<partner domain controller FQDN>*»<br /><br />Émission vers «*<partner domain controller FQDN>*»<br /><br />Des informations supplémentaires peuvent apparaître dans des messages de journaux d'événements ultérieurs.|  
|**13516**|NTFRS|Le service de réplication de fichiers n'empêche plus l'ordinateur DC4 de devenir un contrôleur de domaine. Le volume système a été correctement initialisé et le service Netlogon a été averti du fait que le volume système est maintenant prêt à être partagé en tant que SYSVOL.<br /><br />Entrez « net share » pour vérifier le partage SYSVOL.|  
  
##### <a name="application-event-log"></a>Journal des événements Application  
La base de données FRS s'arrête et démarre. Elle est vidée ensuite en raison de l'opération D2 BURFLAGS.  
  
||||  
|-|-|-|  
|**ID d’événement**|**Source**|**Message**|  
|**327**|ESENT|ntfrs (1424) Le moteur de base de données a détaché une base de données (1, c:\windows\ntfrs\jet\ntfrs.jdb). (Délai = 0 seconde)<br /><br />Séquence de temporisation interne : [1] 0.000, [2] 0.015, [3] 0.000, [4] 0.000, [5] 0.000, [6] 0.516, [7] 0.000, [8] 0.000, [9] 0.000, [10] 0.000, [11] 0.063, [12] 0.000.<br /><br />Mémoire cache réactivée : 0|  
|**103**|ESENT|ntfrs (1424) Le moteur de base de données a arrêté l'instance (0).<br /><br />Arrêt brutal : 0<br /><br />Séquence de minutage interne : [1] 0.000, [2] 0.000, [3] 0.000, [4] 0.000, [5] 0.000, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.031, [10] 0.000, [11] 0.016, [12] 0.000, [13] 0.000, [14] 0.047, [15] 0.000.|  
|**102**|ESENT|ntfrs (3000) Le moteur de base de données (6.02.8189.0000) démarre une nouvelle instance (0).|  
|**105**|ESENT|ntfrs (3000) Le moteur de base de données a démarré une nouvelle instance (0). (Délai = 0 seconde)<br /><br />Séquence de minutage interne : [1] 0.000, [2] 0.000, [3] 0.000, [4] 0.000, [5] 0.000, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.062, [10] 0.000, [11] 0.141.|  
|**103**|ESENT|ntfrs (3000) Le moteur de base de données a arrêté l’instance (0).<br /><br />Arrêt brutal : 0<br /><br />Séquence de minutage interne : [1] 0.000, [2] 0.000, [3] 0.000, [4] 0.000, [5] 0.000, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.000, [10] 0.000, [11] 0.000, [12] 0.000, [13] 0.015, [14] 0.000, [15] 0.000.|  
|**102**|ESENT|ntfrs (3000) Le moteur de base de données (6.02.8189.0000) démarre une nouvelle instance (0).|  
|**105**|ESENT|ntfrs (3000) Le moteur de base de données a démarré une nouvelle instance (0). (Délai = 0 seconde)<br /><br />Séquence de minutage interne : [1] 0.000, [2] 0.000, [3] 0.000, [4] 0.000, [5] 0.000, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.078, [10] 0.000, [11] 0.109.|  
|**325**|ESENT|ntfrs (3000) Le moteur de base de données a créé une base de données (1, c:\windows\ntfrs\jet\ntfrs.jdb). (Délai = 0 seconde)<br /><br />Séquence de minutage interne : [1] 0.000, [2] 0.000, [3] 0.016, [4] 0.016, [5] 0.000, [6] 0.015, [7] 0.000, [8] 0.000, [9] 0.078, [10] 0.016, [11] 0.000.|  
|**103**|ESENT|ntfrs (3000) Le moteur de base de données a arrêté l’instance (0).<br /><br />Arrêt brutal : 0<br /><br />Séquence de minutage interne : [1] 0.000, [2] 0.000, [3] 0.000, [4] 0.000, [5] 0.078, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.125, [10] 0.016, [11] 0.000, [12] 0.000, [13] 0.000, [14] 0.000, [15] 0.000.|  
|**102**|ESENT|ntfrs (3000) Le moteur de base de données (6.02.8189.0000) démarre une nouvelle instance (0).|  
|**105**|ESENT|ntfrs (3000) Le moteur de base de données a démarré une nouvelle instance (0). (Délai = 0 seconde)<br /><br />Séquence de minutage interne : [1] 0.016, [2] 0.000, [3] 0.000, [4] 0.094, [5] 0.000, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.032, [10] 0.000, [11] 0.000.|  
|**326**|ESENT|ntfrs (3000) Le moteur de base de données a attaché une base de données (1, c:\windows\ntfrs\jet\ntfrs.jdb). (Délai = 0 seconde)<br /><br />Séquence de minutage interne : [1] 0.000, [2] 0.015, [3] 0.000, [4] 0.000, [5] 0.016, [6] 0.015, [7] 0.000, [8] 0.000, [9] 0.000, [10] 0.000, [11] 0.000, [12] 0.000.<br /><br />Mémoire cache enregistrée : 1|  
  


