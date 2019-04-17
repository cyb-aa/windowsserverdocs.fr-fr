---
ms.assetid: 249ba1be-b0d3-4a77-99af-3699074a2b6e
title: "Domaine virtualisés Controller Troubleshooting"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 63f96e7a3035bc25f7a7a349acb6bf26f62a2a3f
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="virtualized-domain-controller-troubleshooting"></a>Domaine virtualisés Controller Troubleshooting

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Cette rubrique fournit une méthodologie détaillée sur la résolution des problèmes de la fonctionnalité du contrôleur de domaine virtualisé.  
  
-   [Résolution des problèmes virtualisée de clonage de contrôleur de domaine](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_TshootVDCCloning)  
  
-   [Résolution des problèmes de restauration sécurisée des contrôleurs de domaine virtualisés](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_TshootVDCSafeRestore)  
  
## <a name="BKMK_Intro"></a>Introduction  
Le meilleur moyen d’améliorer vos compétences en matière de résolution des problèmes est build un laboratoire de test et d’examiner rigoureusement les scénarios de travail normaux. Si vous rencontrez des erreurs, ils sont plus évidentes et plus faciles à comprendre, car vous avez une base solide du fonctionne de la promotion du contrôleur de domaine. Cela vous permet également de développer vos compétences d’analyse réseau et d’analyse. Cela vaut pour toutes les technologies de déploiement de contrôleur de domaine pas simplement virtualisés, les systèmes distribués.  
  
Les éléments essentiels pour la résolution avancée des problèmes de configuration du contrôleur de domaine sont:  
  
1.  Analyse linéaire combinée avec le focus et attention aux détails.  
  
2.  Présentation d’analyse de capture réseau  
  
3.  Comprendre les journaux intégrés  
  
Sont premier et deuxième dépassent le cadre de cette rubrique, mais le troisième peut être expliqué en détail. Dépannage de contrôleur de domaine virtualisé nécessite une méthode logique et linéaire. La clé consiste à aborder la question à l’aide des données fournies et ne recourir aux outils et analyses complexes lorsque vous avez épuisé les informations de sortie et la journalisation.  
  
## <a name="BKMK_TshootVDCCloning"></a>Résolution des problèmes virtualisée de clonage de contrôleur de domaine  
Cette section traite des sujets:  
  
-   [Outils de dépannage](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_Tools)  
  
-   [Options de journalisation](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_LoggingOptions)  
  
-   [Méthodologie générale pour la résolution des problèmes de domaine le clonage du contrôleur](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_GeneralMethodology)  
  
-   [Server Core et le journal des événements](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_ServerCoreEvents)  
  
-   [Résolution des problèmes spécifiques](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_SpecificProblems)  
  
La stratégie de résolution des problèmes de clonage de contrôleur de domaine virtualisés suit le format général suivant:  
  
![résolution des problèmes de contrôleur de domaine virtuel](media/Virtualized-Domain-Controller-Troubleshooting/ADDS_VDC_TroublehsootingFlowchart.png)  
  
### <a name="BKMK_Tools"></a>Outils de dépannage  
  
#### <a name="BKMK_LoggingOptions"></a>Options de journalisation  
Les journaux intégrés sont l’outil le plus important pour la résolution des problèmes de clonage de contrôleur de domaine. Tous ces journaux sont activés et configurés par défaut pour un maximum de commentaires.  
  
|||  
|-|-|  
|**Opération**|**Journal**|  
|**Le clonage**|-Observateur d’événements\journaux Windows\Système<br />-Event événements\journaux des applications et des services\Service<br />-%systemroot%\debug\dcpromo.log|  
|**Promotion**|-%systemroot%\debug\dcpromo.log<br />-Event événements\journaux des applications et des services\Service<br />-Observateur d’événements\journaux Windows\Système<br />-Event événements\journaux des applications et services services\Service de réplication<br />-Event événements\journaux des applications et des services\réplication DFS|  
  
#### <a name="tools-and-commands-for-troubleshooting-domain-controller-configuration"></a>Outils et commandes pour la résolution des problèmes de Configuration du contrôleur de domaine  
Pour résoudre les problèmes non décrits par les journaux, utilisez les outils suivants comme point de départ:  
  
-   Dcdiag.exe  
  
-   Repadmin.exe  
  
-   Moniteur réseau 3.4  
  
### <a name="BKMK_GeneralMethodology"></a>Méthodologie générale pour la résolution des problèmes de domaine le clonage du contrôleur  
  
1.  Est-ce que l’ordinateur virtuel démarre en Mode de réparation DS (DSRM)? Cela indique la résolution des problèmes sont nécessaire. Pour ouvrir une session en mode DSRM, utilisez **. \Administrator** de compte et spécifier le mot de passe DSRM.  
  
    1.  Examinez le fichier Dcpromo.log.  
  
        1.  Étapes de clonage initiales réussissent mais échec de la promotion du contrôleur de domaine?  
  
        2.  Erreurs indiquent des problèmes avec le contrôleur de domaine local ou l’environnement AD DS, tels que les erreurs sont renvoyées à partir de l’émulateur de contrôleur de domaine principal?  
  
    2.  Examinez les journaux des événements système et Services Active Directory et que le fichier dccloneconfig.xml et CustomDCCloneAllowList.xml  
  
        1.  Une application incompatible doit se trouver dans le fichier CustomDCCloneAllowList.xml liste verte?  
  
        2.  Est le nom d’ordinateur ou adresse IP dupliquée ou non valide dans le fichier dccloneconfig.xml?  
  
        3.  Le site Active Directory n’est pas valide dans le fichier dccloneconfig.xml?  
  
        4.  L’adresse IP n’est définie dans le dccloningconfig.xml et aucun serveur DHCP n’est disponible?  
  
        5.  L’émulateur de contrôleur de domaine principal n’est en ligne et accessible via le protocole RPC?  
  
        6.  Est le contrôleur de domaine membre du groupe contrôleurs de domaine clonables? Est l’autorisation **autoriser un contrôleur de domaine créer un clone de lui-même** définie sur la racine du domaine de ce groupe?  
  
        7.  Le fichier Dccloneconfig.xml contient des erreurs de syntaxe qui empêchent l’analyse correctes?  
  
        8.  L’hyperviseur est pris en charge?  
  
        9. Avez-vous la promotion du contrôleur de domaine échouent après le démarrage du clonage?  
  
        10. A été le nombre maximal de noms de contrôleurs de domaine générée automatiquement (9 999) a dépassé?  
  
        11. L’adresse MAC est dupliquée?  
  
2.  Est le nom d’hôte du clone le même que le contrôleur de domaine source?  
  
    1.  Existe-t-il un fichier Dccloneconfig.xml dans un des emplacements autorisés?  
  
3.  Est l’ordinateur virtuel démarre en mode normal et le clonage terminé, mais le contrôleur de domaine ne fonctionne pas correctement?  
  
    1.  Tout d’abord, vérifiez si le nom d’hôte est modifié sur le clone. Si le nom d’hôte est différent, le clonage au moins partiellement terminée.  
  
    2.  Le contrôleur de domaine a une adresse IP dupliquée du contrôleur de domaine source à partir du fichier dccloneconfig.xml, mais le contrôleur de domaine source était hors connexion durant le clonage?  
  
    3.  Si le contrôleur de domaine publie, traitez le problème comme n’importe quel problème que POST-promotion le clonage.  
  
    4.  Si le contrôleur de domaine ne publie pas, examinez les journaux des événements Service d’annuaire, système, Application, la réplication de fichiers et réplication DFS pour les erreurs POST-promotion.  
  
#### <a name="disabling-dsrm-boot"></a>La désactivation de démarrage en mode DSRM  
Après le démarrage en mode DSRM en raison d’une erreur, diagnostiquer la cause de l’échec et si le fichier dcpromo.log n’indique pas que le clonage ne peut pas être retentée, corrigez la cause de l’échec et réinitialiser l’indicateur DSRM. Un clone ne retourne pas en mode normal sur son propre au prochain redémarrage; Vous devez supprimer l’indicateur de démarrage du Mode de restauration des services d’annuaire afin de retenter le clonage. Toutes ces étapes nécessitent une exécution en tant qu’administrateur avec élévation de privilèges.  
  
##### <a name="removing-dsrm-with-msconfigexe"></a>Suppression du mode DSRM avec Msconfig.exe  
Pour activer le démarrage en mode DSRM à l’aide d’une interface graphique utilisateur, utilisez l’outil de Configuration du système:  
  
1.  Exécutez msconfig.exe  
  
2.  Sur le **démarrage** sous **Options de démarrage**, désélectionnez **démarrage sans échec** (elle est déjà sélectionnée avec l’option **réparer Active Directory** activé)  
  
3.  Cliquez sur OK et redémarrez lorsque vous y êtes invité  
  
##### <a name="removing-dsrm-with-bcdeditexe"></a>Suppression du mode DSRM avec Bcdedit.exe  
Pour désactiver le démarrage en mode DSRM à partir de la ligne de commande, utilisez l’éditeur de magasin de données de Configuration de démarrage:  
  
1.  Ouvrez une invite de commandes et exécuter:  
  
    ```  
    Bcdedit.exe /deletevalue safeboot  
    ```  
  
2.  Redémarrez l’ordinateur avec:  
  
    ```  
    Shutdown.exe /t /0 /r  
    ```  
  
> [!NOTE]  
> Bcdedit.exe fonctionne aussi dans une console Windows PowerShell. Les commandes sont:  
>   
> Bcdedit.exe /deletevalue safeboot  
>   
> Redémarrer l’ordinateur  
  
### <a name="BKMK_ServerCoreEvents"></a>Server Core et le journal des événements  
Les journaux des événements contiennent de nombreuses informations utiles sur les opérations de clonage de contrôleur de domaine virtualisé. Par défaut, une installation d’ordinateur Windows Server 2012 est une installation Server Core, ce qui signifie qu’il y a pas d’interface graphique et par conséquent, aucun moyen d’exécuter le composant logiciel enfichable Observateur d’événements local.  
  
Pour passer en revue les journaux des événements sur un serveur exécutant une installation Server Core:  
  
-   Exécutez l’outil Wevtutil.exe localement  
  
-   Exécution applet de commande PowerShell Get-WinEvent localement  
  
-   Si vous avez activé les règles avancées du pare-feu Windows pour les groupes «Gestion à distance du journal des événements» (ou des ports équivalents) autoriser les communications entrantes, vous pouvez gérer le journal des événements à distance via Eventvwr.exe, wevtutil.exe ou Get-Winevent. Cela est possible sur une installation Server Core à l’aide de NETSH.exe, la stratégie de groupe ou la nouvelle applet de commande Set-NetFirewallRule de Windows PowerShell 3.0.  
  
> [!WARNING]  
> N’essayez pas d’ajouter l’interpréteur de commandes graphique à l’ordinateur lorsqu’il est en mode DSRM. Windows (CBS) de la pile de traitements ne peut pas fonctionner correctement en Mode sans échec ou DSRM. Tente d’ajouter des fonctionnalités ou rôles en mode DSRM termine pas et laissent l’ordinateur dans un état instable jusqu'à ce qu’il soit démarré normalement. Dans la mesure où un clone de contrôleur de domaine virtualisé en mode DSRM ne peut pas démarrer normalement et ne doit pas être démarré normalement dans la plupart des cas, il est impossible de rajouter l’interpréteur de commandes graphique. Ainsi, non pris en charge et peut vous laisser le serveur inutilisable.  
  
### <a name="BKMK_SpecificProblems"></a>Résolution des problèmes spécifiques  
  
#### <a name="events"></a>Événements  
Événements de clonage de contrôleur de domaine virtualisé tous les écrivent dans le journal des événements Services d’annuaire du contrôleur de domaine clone VM. Les journaux des événements Application, Service de réplication de fichiers et réplication DFS peut également contenir des informations de dépannage utiles pour le clonage a échoué. Échecs lors de l’appel RPC à l’émulateur PDC peuvent être disponibles dans le journal des événements sur l’émulateur de contrôleur de domaine principal.  
  
Vous trouverez ci-dessous les événements de clonage Windows Server 2012 dans le journal des événements Services d’annuaire, avec des remarques et des suggestions de résolution des erreurs.  
  
##### <a name="directory-services-event-log"></a>Journal des événements Services Active  
  
|||  
|-|-|  
|**ID d’événement**|**2160**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravité**|Information|  
|**Message**|Local * <COMPUTERNAME> * a trouvé un fichier de configuration de clonage de contrôleur de domaine virtuel.<br /><br />Le fichier de configuration de clonage de contrôleur de domaine virtuel se trouve à: %1<br /><br />L’existence du fichier configuration de clonage de contrôleur de domaine virtuel indique que le contrôleur de domaine virtuel local est un clone d’un autre contrôleur de domaine virtuel. Le * <COMPUTERNAME> * commence à se cloner.|  
|**Remarques et résolution**|Ceci est un événement de succès et seulement un problème si inattendue. Examinez le répertoire de travail DSA, % systemroot%\ntds et la racine de tous les disques locaux ou amovibles pour le fichier dcclconeconfig.XML dans.|  
  
|||  
|-|-|  
|**ID d’événement**|**2161**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravité**|Information|  
|**Message**|Local * <COMPUTERNAME> * n’a pas trouvé le fichier de configuration de clonage de contrôleur de domaine virtuel. L’ordinateur local n’est pas un contrôleur de domaine cloné.|  
|**Remarques et résolution**|Ceci est un événement de succès et seulement un problème si inattendue. Examinez le répertoire de travail DSA, % systemroot%\ntds et la racine de tous les disques locaux ou amovibles pour le fichier dcclconeconfig.XML dans.|  
  
|||  
|-|-|  
|**ID d’événement**|**2162**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravité**|Erreur|  
|**Message**|Clonage de contrôleur de domaine virtuel a échoué.<br /><br />Vérifiez les événements consignés dans les journaux des événements système et %systemroot%\debug\dcpromo.log pour plus d’informations sur les erreurs qui correspondent à la tentative de clonage de contrôleur de domaine virtuel.<br /><br />Code d’erreur: %1|  
|**Remarques et résolution**|Suivez les instructions message, cette erreur est un fourre-tout.|  
  
|||  
|-|-|  
|**ID d’événement**|**2163**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravité**|Information|  
|**Message**|Le service DsRoleSvc a démarré pour cloner le contrôleur de domaine virtuel local.|  
|**Remarques et résolution**|Ceci est un événement de succès et seulement un problème si inattendue. Examinez le répertoire de travail DSA, % systemroot%\ntds et la racine de tous les disques locaux ou amovibles pour le fichier dcclconeconfig.XML dans.|  
  
|||  
|-|-|  
|**ID d’événement**|**2164**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravité**|Erreur|  
|**Message**|*<COMPUTERNAME>*n’a pas pu démarrer le service DsRoleSvc pour cloner le contrôleur de domaine virtuel local.|  
|**Remarques et résolution**|Examinez les paramètres de service pour le service de serveur de rôles DS (DsRoleSvc) et assurez-vous que son type de démarrage est défini sur manuel. Vérifiez qu’aucun programme tiers n’empêche le démarrage de ce service.|  
  
|||  
|-|-|  
|**ID d’événement**|**2165**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravité**|Erreur|  
|**Message**|*<COMPUTERNAME>*n’a pas pu démarrer un thread durant le clonage du contrôleur de domaine virtuel local.<br /><br />Code d’erreur: %1<br /><br />Message d’erreur: %2<br /><br />Nom du thread: %3|  
|**Remarques et résolution**|Contactez le Support technique Microsoft|  
  
|||  
|-|-|  
|**ID d’événement**|**2166**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravité**|Erreur|  
|**Message**|*<COMPUTERNAME>*a besoin du service RPCSS pour lancer le redémarrage en mode DSRM. En attente de RPCSS d’initialisation dans un état en cours d’exécution a échoué.<br /><br />Code d’erreur: %1|  
|**Remarques et résolution**|Examinez le journal des événements système et les paramètres de service pour le service serveur RPC (Rpcss)|  
  
|||  
|-|-|  
|**ID d’événement**|**2167**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravité**|Erreur|  
|**Message**|*<COMPUTERNAME>*n’a pas pu initialiser les informations de contrôleur de domaine virtuel. Consultez les entrées précédentes du journal des événements pour plus d’informations.<br /><br />Données supplémentaires<br /><br />Code d’échec: %1|  
|**Remarques et résolution**|Suivez les instructions message, cette erreur est un fourre-tout.|  
  
|||  
|-|-|  
|**ID d’événement**|**2168**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravité**|Information|  
|**Message**|Microsoft-Windows-ActiveDirectory_DomainService<br /><br />Le contrôleur de domaine est en cours d’exécution sur un hyperviseur pris en charge. ID de génération d’ordinateur virtuel est détecté.<br /><br />Valeur actuelle de l’ID de génération d’ordinateur virtuel: %1|  
|**Remarques et résolution**|Ceci est un événement de succès et seulement un problème si inattendue.|  
  
|||  
|-|-|  
|**ID d’événement**|**2169**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravité**|Information|  
|**Message**|Il n’existe aucun ID de génération d’ordinateur virtuel détecté. Le contrôleur de domaine est hébergé sur un ordinateur physique, une version de bas niveau d’Hyper-V ou un hyperviseur qui ne prend pas en charge l’ID de génération d’ordinateur virtuel.<br /><br />Données supplémentaires<br /><br />Code d’échec retourné lorsque la vérification de la génération d’ordinateur virtuel: %1|  
|**Remarques et résolution**|Il s’agit d’un événement de succès si vous ne prévoyez ne pas de cloner. Sinon, examinez le journal des événements système et passez en revue la documentation du produit hyperviseur.|  
  
|||  
|-|-|  
|**ID d’événement**|**2170**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravité**|Avertissement|  
|**Message**|Un changement d’ID de génération a été détecté.<br /><br />ID de génération mis en cache dans les services d’annuaire (ancienne valeur): %1<br /><br />ID de génération actuellement dans l’ordinateur virtuel (nouvelle valeur): %2<br /><br />Le changement d’ID de génération se produit après l’application d’une capture instantanée d’ordinateur virtuel, après une opération d’importation de machine virtuelle ou après une opération de migration dynamique. *<COMPUTERNAME>*Crée un ID d’appel pour récupérer le contrôleur de domaine. Contrôleurs de domaine virtualisés ne doivent pas être restaurés à l’aide de captures instantanées d’ordinateur virtuel. La méthode prise en charge pour restaurer le contenu d’une base de données des Services de domaine Active Directory consiste à restaurer une sauvegarde de l’état système effectuée avec une application de sauvegarde prenant en charge les Services de domaine Active Directory.|  
|**Remarques et résolution**|Il s’agit d’un événement de succès si l’intention de cloner. Sinon, examinez le journal des événements système.|  
  
|||  
|-|-|  
|**ID d’événement**|**2171**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravité**|Information|  
|**Message**|Aucun changement d’ID de génération n’a été détecté.<br /><br />ID de génération mis en cache dans les services d’annuaire (ancienne valeur): %1<br /><br />ID de génération actuellement dans l’ordinateur virtuel (nouvelle valeur): %2|  
|**Remarques et résolution**|Il s’agit d’un événement de succès si vous ne prévoyez ne pas de cloner, il doit être visible à chaque redémarrage d’un contrôleur de domaine virtualisé. Sinon, examinez le journal des événements système.|  
  
|||  
|-|-|  
|**ID d’événement**|**2172**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravité**|Information|  
|**Message**|Lisez l’attribut msDS-GenerationId de l’objet ordinateur du contrôleur de domaine.<br /><br />attribut msDS-GenerationId valeur: % 1|  
|**Remarques et résolution**|Il s’agit d’un événement de succès si l’intention de cloner. Sinon, examinez le journal des événements système.|  
  
|||  
|-|-|  
|**ID d’événement**|**2173**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravité**|Information|  
|**Message**|N’a pas pu lire l’attribut msDS-GenerationId de l’objet ordinateur du contrôleur de domaine. Cela peut résulter d’échec de la transaction de base de données, ou l’id de génération n’existe pas dans la base de données locale. L’attribut msDS-GenerationId n’existe pas durant le premier redémarrage après que dcpromo ou le contrôleur de domaine n’est pas un contrôleur de domaine virtuel.<br /><br />Données supplémentaires<br /><br />Code d’échec: %1|  
|**Remarques et résolution**|Il s’agit d’un événement de succès si l’intention de cloner et il est le premier redémarrage de l’ordinateur virtuel une fois le clonage est terminée. Il peut également être ignoré sur les contrôleurs de domaine non virtuels. Sinon, examinez le journal des événements système.|  
  
|||  
|-|-|  
|**ID d’événement**|**2174**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravité**|Information|  
|**Message**|Le contrôleur de domaine est un instantané de contrôleur de domaine virtuel restauré ni un clone de contrôleur de domaine virtuel.|  
|**Remarques et résolution**|Il s’agit d’un événement de succès si vous ne prévoyez ne pas de cloner. Sinon, examinez le journal des événements système.|  
  
|||  
|-|-|  
|**ID d’événement**|**2175**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravité**|Erreur|  
|**Message**|Fichier de configuration de clonage de contrôleur de domaine virtuel existe sur une plateforme non prise en charge.|  
|**Remarques et résolution**|Cela se produit lorsqu’un fichier dccloneconfig.xml est trouvé, mais un ID de génération d’ordinateur virtuel est introuvable, par exemple, si un fichier dccloneconfig.xml est détecté sur un ordinateur physique ou sur un hyperviseur qui ne prend pas en charge les ID de génération de machine virtuelle-.|  
  
|||  
|-|-|  
|**ID d’événement**|**2176**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravité**|Information|  
|**Message**|Renommer le fichier de configuration de clonage de contrôleur de domaine virtuel.<br /><br />Données supplémentaires<br /><br />Ancien fichier nom: %1<br /><br />Nouveau fichier nom: %2|  
|**Remarques et résolution**|Changement de nom attendu au démarrage d’un ordinateur virtuel source vers le haut, car l’ID de génération d’ordinateur virtuel n’a pas changé. Cela empêche le contrôleur de domaine source à partir de la tentative de clonage.|  
  
|||  
|-|-|  
|**ID d’événement**|**2177**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravité**|Erreur|  
|**Message**|Modification du nom de fichier de configuration de clonage de contrôleur de domaine virtuel a échoué.<br /><br />Données supplémentaires<br /><br />Nom du fichier: %1<br /><br />Défaillance du code: %2 %3|  
|**Remarques et résolution**|Renommez tentative prévu lors du démarrage d’un ordinateur virtuel source vers le haut, car l’ID de génération d’ordinateur virtuel n’a pas changé. Cela empêche le contrôleur de domaine source à partir de la tentative de clonage. Renommez le fichier et examiner les produits tiers installés qui peuvent empêcher le changement de nom de fichier manuellement.|  
  
|||  
|-|-|  
|**ID d’événement**|**2178**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravité**|Information|  
|**Message**|Détecté le fichier de configuration de clonage de contrôleur de domaine virtuel, mais l’ID de génération d’ordinateur virtuel n’a pas été modifié. Le contrôleur de domaine local est la source du clonage du contrôleur de domaine. Renommez le fichier de configuration de clonage.|  
|**Remarques et résolution**|Attendu au démarrage d’un ordinateur virtuel source vers le haut, car l’ID de génération d’ordinateur virtuel n’a pas changé. Cela empêche le contrôleur de domaine source à partir de la tentative de clonage.|  
  
|||  
|-|-|  
|**ID d’événement**|**2179**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravité**|Information|  
|**Message**|L’attribut msDS-GenerationId de l’objet ordinateur du contrôleur de domaine a été défini pour le paramètre suivant:<br /><br />Attribut GenerationID: %1|  
|**Remarques et résolution**|Ceci est un événement de succès et seulement un problème si inattendue.|  
  
|||  
|-|-|  
|**ID d’événement**|**2180**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravité**|Avertissement|  
|**Message**|Impossible de définir l’attribut msDS-GenerationId de l’objet ordinateur du contrôleur de domaine.<br /><br />Données supplémentaires<br /><br />Code d’échec: %1|  
|**Remarques et résolution**|Examinez le journal des événements système et Dcpromo.log. Recherchez l’erreur spécifique dans Microsoft TechNet, Knowledgebase et blogs Microsoft pour déterminer sa signification et puis résolvez le problème en fonction des résultats.|  
  
|||  
|-|-|  
|**ID d’événement**|**2182**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravité**|Information|  
|**Message**|Événement interne: le Service d’annuaire a été demandé de cloner un DSA à distance:|  
|**Remarques et résolution**|Ceci est un événement de succès et seulement un problème si inattendue.|  
  
|||  
|-|-|  
|**ID d’événement**|**2183**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravité**|Information|  
|**Message**|Événement interne: * <COMPUTERNAME> * effectué la demande de clonage de l’Agent de système d’annuaire distant.<br /><br />D’origine du contrôleur de domaine nom: %3<br /><br />La demande de clonage du contrôleur de domaine nom: %4<br /><br />La demande de clonage du contrôleur de domaine site: %5<br /><br />Données supplémentaires<br /><br />Erreur valeur: %1 %2|  
|**Remarques et résolution**|Ceci est un événement de succès et seulement un problème si inattendue.|  
  
|||  
|-|-|  
|**ID d’événement**|**2184**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravité**|Erreur|  
|**Message**|*<COMPUTERNAME>*Impossible de créer un compte de contrôleur de domaine pour le contrôleur de domaine cloné.<br /><br />D’origine du contrôleur de domaine nom: %1<br /><br />Nombre autorisé de cloné DC:% 2<br /><br />La limite du nombre de comptes de contrôleur de domaine pouvant être générés par clonage * <COMPUTERNAME> *a été dépassé.|  
|**Remarques et résolution**|Un nom de contrôleur de domaine source unique peut générer uniquement automatiquement que 9 999 fois si les contrôleurs de domaine ne sont pas rétrogradés, en fonction de la convention d’affectation de noms. Utilisez le <computername> élément dans le code XML pour générer un nouveau nom ou clone unique à partir d’un contrôleur de domaine nommé de manière distincte.|  
  
|||  
|-|-|  
|**ID d’événement**|**2191**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravité**|Information|  
|**Message**|*<COMPUTERNAME>*Définissez la valeur de Registre suivante pour désactiver les mises à jour DNS.<br /><br />Clé de Registre: %1<br /><br />Valeur de Registre: %2<br /><br />Données de la valeur du Registre: %3<br /><br />Pendant le processus de clonage, l’ordinateur local peut avoir le même nom d’ordinateur que l’ordinateur source clone pendant une courte période. DNS A et l’inscription des enregistrements AAAA sont désactivées pendant cette période afin des clients ne peuvent pas envoyer de demandes à l’ordinateur local en cours de clonage. Le processus de clonage activer les mises à jour DNS à nouveau une fois le clonage est terminé.|  
|**Remarques et résolution**|Ceci est un événement de succès et seulement un problème si inattendue.|  
  
|||  
|-|-|  
|**ID d’événement**|**2192**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravité**|Erreur|  
|**Message**|*<COMPUTERNAME>*Impossible de définir la valeur de Registre suivante pour désactiver les mises à jour DNS.<br /><br />Clé de Registre: %1<br /><br />Valeur de Registre: %2<br /><br />Données de la valeur du Registre: %3<br /><br />Code d’erreur: %4<br /><br />Message d’erreur: %5<br /><br />Pendant le processus de clonage, l’ordinateur local peut avoir le même nom d’ordinateur que l’ordinateur source clone pendant une courte période. DNS A et l’inscription des enregistrements AAAA sont désactivées pendant cette période afin des clients ne peuvent pas envoyer de demandes à l’ordinateur local en cours de clonage.|  
|**Remarques et résolution**|Examinez les journaux des événements Application et système. Vérifiez si une application tierce ne bloque pas les mises à jour du Registre.|  
  
|||  
|-|-|  
|**ID d’événement**|**2193**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravité**|Information|  
|**Message**|*<COMPUTERNAME>*Définissez la valeur de Registre suivante pour activer les mises à jour DNS.<br /><br />Clé de Registre: %1<br /><br />Valeur de Registre: %2<br /><br />Données de la valeur du Registre: %3<br /><br />Pendant le processus de clonage, l’ordinateur local peut avoir le même nom d’ordinateur que l’ordinateur source clone pendant une courte période. DNS A et l’inscription des enregistrements AAAA sont désactivées pendant cette période afin des clients ne peuvent pas envoyer de demandes à l’ordinateur local en cours de clonage.|  
|**Remarques et résolution**|Ceci est un événement de succès et seulement un problème si inattendue.|  
  
|||  
|-|-|  
|**ID d’événement**|**2194**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravité**|Erreur|  
|**Message**|*<COMPUTERNAME>*Impossible de définir la valeur de Registre suivante pour activer les mises à jour DNS.<br /><br />Clé de Registre: %1<br /><br />Valeur de Registre: %2<br /><br />Données de la valeur du Registre: %3<br /><br />Code d’erreur: %4<br /><br />Message d’erreur: %5<br /><br />Pendant le processus de clonage, l’ordinateur local peut avoir le même nom d’ordinateur que l’ordinateur source clone pendant une courte période. DNS A et l’inscription des enregistrements AAAA sont désactivées pendant cette période afin des clients ne peuvent pas envoyer de demandes à l’ordinateur local en cours de clonage.|  
|**Remarques et résolution**|Examinez les journaux des événements Application et système. Vérifiez si une application tierce ne bloque pas les mises à jour du Registre.|  
  
|||  
|-|-|  
|**ID d’événement**|**2195**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravité**|Erreur|  
|**Message**|Échec de démarrage DSRM.<br /><br />Code d’erreur: %1<br /><br />Message d’erreur: %2<br /><br />Quand le clonage de contrôleur de domaine virtuel a échoué ou fichier de configuration de clonage de contrôleur de domaine virtuel s’affiche sur un hyperviseur non pris en charge, la machine locale redémarre en mode DSRM pour la résolution des problèmes. Échec de démarrage DSRM.|  
|**Remarques et résolution**|Examinez les journaux des événements Application et système. Vérifiez si une application tierce ne bloque pas les mises à jour du Registre.|  
  
|||  
|-|-|  
|**ID d’événement**|**2196**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravité**|Erreur|  
|**Message**|Échec d’activation du privilège d’arrêt.<br /><br />Code d’erreur: %1<br /><br />Message d’erreur: %2<br /><br />Quand le clonage de contrôleur de domaine virtuel a échoué ou fichier de configuration de clonage de contrôleur de domaine virtuel s’affiche sur un hyperviseur non pris en charge, la machine locale redémarre en mode DSRM pour la résolution des problèmes. Échec de l’activation du privilège d’arrêt.|  
|**Remarques et résolution**|Examinez les journaux des événements Application et système. Vérifiez si une application tierce ne bloque pas l’utilisation des privilèges.|  
  
|||  
|-|-|  
|**ID d’événement**|**2197**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravité**|Erreur|  
|**Message**|Échec de l’arrêt du système.<br /><br />Code d’erreur: %1<br /><br />Message d’erreur: %2<br /><br />Quand le clonage de contrôleur de domaine virtuel a échoué ou fichier de configuration de clonage de contrôleur de domaine virtuel s’affiche sur un hyperviseur non pris en charge, la machine locale redémarre en mode DSRM pour la résolution des problèmes. Échec de l’arrêt du système que vous lancez.|  
|**Remarques et résolution**|Examinez les journaux des événements Application et système. Vérifiez si une application tierce ne bloque pas l’utilisation des privilèges.|  
  
|||  
|-|-|  
|**ID d’événement**|**2198**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravité**|Erreur|  
|**Message**|*<COMPUTERNAME>*n’a pas pu créer ou modifier l’objet contrôleur de domaine cloné suivant.<br /><br />Données supplémentaires:<br /><br />Objet:<br /><br />%1<br /><br />Valeur d’erreur: %2<br /><br />%3|  
|**Remarques et résolution**|Recherchez l’erreur spécifique dans Microsoft TechNet, Knowledgebase et blogs Microsoft pour déterminer sa signification et puis résolvez le problème en fonction des résultats.|  
  
|||  
|-|-|  
|**ID d’événement**|**2199**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravité**|Erreur|  
|**Message**|*<COMPUTERNAME>*n’a pas pu créer l’objet contrôleur de domaine cloné suivant, car l’objet existe déjà.<br /><br />Données supplémentaires:<br /><br />Contrôleur de domaine source:<br /><br />%1<br /><br />Objet:<br /><br />%2|  
|**Remarques et résolution**|Valider le fichier dccloneconfig.xml n’a pas spécifié de contrôleur de domaine existant ou que les copies du fichier dccloneconfig.xml ont été utilisés sur plusieurs clones sans modification du nom. Si la collision est toujours inattendue, déterminez quel est l’administrateur promu Contactez-le pour si le contrôleur de domaine existant doit être rétrogradé, les métadonnées de contrôleur de domaine existant nettoyé, ou si le clone doit utiliser un autre nom.|  
  
|||  
|-|-|  
|**ID d’événement**|**2203**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravité**|Erreur|  
|**Message**|Échec de la dernière clonage du contrôleur de domaine virtuel. Cela est le premier redémarrage depuis, par conséquent, il doit être vient du clonage. Toutefois, aucun fichier de configuration de clonage de contrôleur domaine virtuel existe ni changement d’ID de génération de machine virtuelle est détecté. Démarrage en mode DSRM.<br /><br />Dernière clonage du contrôleur de domaine virtuel a échoué: %1<br /><br />Fichier de configuration de clonage de contrôleur de domaine virtuel existe: %2<br /><br />Changement d’ID de génération de machine virtuelle est détecté: %3|  
|**Remarques et résolution**|Attendu si le clonage a échoué précédemment, en raison du fichier dccloneconfig.xml manquant ou non valide|  
  
|||  
|-|-|  
|ID d’événement|2210|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|Gravité|Erreur|  
|Message|<COMPUTERNAME> Impossible de créer des objets pour le contrôleur de domaine clone.<br /><br />Données supplémentaires:<br /><br />Id de clone: %6<br /><br />Nom du contrôleur de domaine clone: %1<br /><br />Boucle de reprise: %2<br /><br />Valeur d’exception: %3<br /><br />Valeur d’erreur: %4<br /><br />DSID: %5|  
|Remarques et résolution|Examinez les journaux des événements système et Services Active Directory et le fichier dcpromo.log, pour plus d’informations sur la raison pour laquelle le clonage a échoué.|  
  
|||  
|-|-|  
|ID d’événement|2211|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|Gravité|Information|  
|Message|<COMPUTERNAME> a créé des objets pour le contrôleur de domaine clone.<br /><br />Données supplémentaires:<br /><br />Id de clone: %3<br /><br />Nom du contrôleur de domaine clone: %1<br /><br />Boucle de reprise: %2|  
|Remarques et résolution|Ceci est un événement de succès et seulement un problème si inattendue.|  
  
|||  
|-|-|  
|ID d’événement|2212|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|Gravité|Information|  
|Message|<COMPUTERNAME> a commencé à créer des objets pour le contrôleur de domaine clone.<br /><br />Données supplémentaires:<br /><br />Id de clone: %1<br /><br />Nom du clone: %2<br /><br />Site clone: %3<br /><br />Cloner RODC: %4|  
|Remarques et résolution|Ceci est un événement de succès et seulement un problème si inattendue.|  
  
|||  
|-|-|  
|ID d’événement|2213|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|Gravité|Information|  
|Message|<COMPUTERNAME> Création d’un nouvel objet KrbTgt pour le clonage de contrôleur de domaine en lecture seule.<br /><br />Données supplémentaires:<br /><br />Id de clone: %1<br /><br />Guid du nouvel objet KrbTgt: %2|  
|Remarques et résolution|Ceci est un événement de succès et seulement un problème si inattendue.|  
  
|||  
|-|-|  
|ID d’événement|2214|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|Gravité|Information|  
|Message|<COMPUTERNAME> Crée un objet ordinateur pour le contrôleur de domaine clone.<br /><br />Données supplémentaires:<br /><br />Id de clone: %1<br /><br />Contrôleur de domaine initial: %2<br /><br />Contrôleur de domaine clone: %3|  
|Remarques et résolution|Ceci est un événement de succès et seulement un problème si inattendue.|  
  
|||  
|-|-|  
|ID d’événement|2215|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|Gravité|Information|  
|Message|<COMPUTERNAME> Ajoute le contrôleur de domaine clone au site suivant.<br /><br />Données supplémentaires:<br /><br />Id de clone: %1<br /><br />Site: %2|  
|Remarques et résolution|Ceci est un événement de succès et seulement un problème si inattendue.|  
  
|||  
|-|-|  
|ID d’événement|2216|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|Gravité|Information|  
|Message|<COMPUTERNAME> Crée un conteneur de serveurs pour le contrôleur de domaine clone.<br /><br />Données supplémentaires:<br /><br />Id de clone: %1<br /><br />Conteneur de serveurs: %2|  
|Remarques et résolution|Ceci est un événement de succès et seulement un problème si inattendue.|  
  
|||  
|-|-|  
|ID d’événement|2217|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|Gravité|Information|  
|Message|<COMPUTERNAME> Crée un objet serveur pour le contrôleur de domaine clone.<br /><br />Données supplémentaires:<br /><br />Id de clone: %1<br /><br />Objet serveur: %2|  
|Remarques et résolution|Ceci est un événement de succès et seulement un problème si inattendue.|  
  
|||  
|-|-|  
|ID d’événement|2218|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|Gravité|Information|  
|Message|<COMPUTERNAME> Crée un objet Paramètres NTDS pour le contrôleur de domaine clone.<br /><br />Données supplémentaires:<br /><br />Id de clone: %1<br /><br />Objet: %2|  
|Remarques et résolution|Ceci est un événement de succès et seulement un problème si inattendue.|  
  
|||  
|-|-|  
|ID d’événement|2219|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|Gravité|Information|  
|Message|<COMPUTERNAME> va créer des objets de connexion pour le contrôleur de domaine en lecture seule clone.<br /><br />Données supplémentaires:<br /><br />Id de clone: %1|  
|Remarques et résolution|Ceci est un événement de succès et seulement un problème si inattendue.|  
  
|||  
|-|-|  
|ID d’événement|2220|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|Gravité|Information|  
|Message|<COMPUTERNAME> va créer des objets SYSVOL pour le contrôleur de domaine en lecture seule clone.<br /><br />Données supplémentaires:<br /><br />Id de clone: %1|  
|Remarques et résolution|Ceci est un événement de succès et seulement un problème si inattendue.|  
  
|||  
|-|-|  
|ID d’événement|2221|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|Gravité|Erreur|  
|Message|<COMPUTERNAME> Impossible de générer un mot de passe aléatoire pour le contrôleur de domaine cloné.<br /><br />Données supplémentaires:<br /><br />Id de clone: %1<br /><br />Nom du contrôleur de domaine clone: %2<br /><br />Erreur: %3 %4|  
|Remarques et résolution|Examinez le journal des événements système pour plus d’informations sur la raison pour laquelle le mot de passe de compte d’ordinateur a pas pu être créé.|  
  
|||  
|-|-|  
|ID d’événement|2222|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|Gravité|Erreur|  
|Message|<COMPUTERNAME> n’a pas pu définir le mot de passe pour le contrôleur de domaine cloné.<br /><br />Données supplémentaires:<br /><br />Id de clone: %1<br /><br />Nom du contrôleur de domaine clone: %2<br /><br />Erreur: %3 %4|  
|Remarques et résolution|Examinez le journal des événements système pour plus d’informations sur la raison pour laquelle le mot de passe de compte d’ordinateur ne peut pas être définie.|  
  
|||  
|-|-|  
|ID d’événement|2223|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|Gravité|Information|  
|Message|<COMPUTERNAME> correctement définir le mot de passe du compte ordinateur du contrôleur de domaine cloné.<br /><br />Données supplémentaires:<br /><br />Id de clone: %1<br /><br />Nom du contrôleur de domaine clone: %2<br /><br />Nombre total de tentatives: %3|  
|Remarques et résolution|Ceci est un événement de succès et seulement un problème si inattendue.|  
  
|||  
|-|-|  
|ID d’événement|2224|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|Gravité|Erreur|  
|Message|Clonage de contrôleur de domaine virtuel a échoué. %1 suivants existent des comptes de Service géré sur l’ordinateur cloné:<br /><br />%2<br /><br />Pour le clonage réussisse, tous les comptes de Service administrés doivent être supprimés. Cela est possible à l’aide de l’applet de commande PowerShell Remove-ADComputerServiceAccount.|  
|Remarques et résolution|Attendu durant l’utilisation de comptes de service administrés autonomes (pas compte de service administré de groupe). Faire *pas* suivez les conseils d’événement pour supprimer le compte, il est écrit correctement. Utilisez Uninstall-AdServiceAccount - [https://technet.microsoft.com/library/hh852310](https://technet.microsoft.com/library/hh852310).<br /><br />Comptes de service administrés autonomes - apparus dans Windows Server 2008 R2 - ont été remplacés dans Windows Server 2012 avec les comptes de service administrés groupe (gMSA). Comptes Gmsa prennent en charge le clonage.|  
  
|||  
|-|-|  
|ID d’événement|2225|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|Gravité|Information|  
|Message|Les secrets mis en cache du principal de sécurité suivant ont été supprimés à partir du contrôleur de domaine local:<br /><br />%1<br /><br />Après le clonage de contrôleur de domaine en lecture seule, les secrets mis en cache auparavant sur le contrôleur de domaine en lecture seule source clonage seront supprimés sur le contrôleur de domaine cloné.|  
|Remarques et résolution|Ceci est un événement de succès et seulement un problème si inattendue.|  
  
|||  
|-|-|  
|ID d’événement|2226|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|Gravité|Erreur|  
|Message|Impossible de supprimer les secrets mis en cache du principal de sécurité suivant à partir du contrôleur de domaine local:<br /><br />%1<br /><br />Erreur: %2 (%3)<br /><br />Après avoir cloné un contrôleur de domaine en lecture seule, les secrets mis en cache auparavant sur la nécessité de contrôleur de domaine en lecture seule source clonage à supprimer sur le clone afin de réduire le risque qu’une personne malveillante peut obtenir ces informations d’identification à partir du clone volé ou compromis. Si l’entité de sécurité est un compte à privilèges élevés et doit être protégé contre ce, utilisez l’opération rootDSE rODCPurgeAccount pour effacer manuellement ses secrets sur le contrôleur de domaine local.|  
|Remarques et résolution|Examinez les journaux des événements système et Services Active Directory pour plus d’informations.|  
  
|||  
|-|-|  
|ID d’événement|2227|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|Gravité|Erreur|  
|Message|Exception est levée lors de la tentative de suppression des secrets mis en cache à partir du contrôleur de domaine local.<br /><br />Données supplémentaires:<br /><br />Valeur d’exception: %1<br /><br />Valeur d’erreur: %2<br /><br />DSID: %3<br /><br />Après avoir cloné un contrôleur de domaine en lecture seule, les secrets mis en cache auparavant sur la nécessité de contrôleur de domaine en lecture seule source clonage à supprimer sur le clone afin de réduire le risque qu’une personne malveillante peut obtenir ces informations d’identification à partir du clone volé ou compromis. Si une de ces entités de sécurité est un compte à privilèges élevés et doit être protégé contre ce, utilisez l’opération rootDSE rODCPurgeAccount pour effacer manuellement ses secrets sur le contrôleur de domaine local.|  
|Remarques et résolution|Examinez les journaux des événements système et Services Active Directory pour plus d’informations.|  
  
|||  
|-|-|  
|ID d’événement|2228|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|Gravité|Erreur|  
|Message|L’ID de génération d’ordinateur virtuel dans la base de données Active Directory de ce contrôleur de domaine est différente de la valeur actuelle de cet ordinateur virtuel. Toutefois, un fichier de configuration de clonage de contrôleur domaine virtuel (DCCloneConfig.xml) introuvable afin de clonage de contrôleur de domaine a été tenté pas. Si un opération de clonage de contrôleur de domaine a été tentée, assurez-vous qu’un fichier DCCloneConfig.xml est fourni dans l’un des emplacements pris en charge. En outre, l’adresse IP de ce contrôleur de domaine est en conflit avec l’adresse IP du contrôleur de domaine un autre. Pour vous assurer aucune interruption de service se produit, le contrôleur de domaine a été configuré pour un démarrage en mode DSRM.<br /><br />Données supplémentaires:<br /><br />L’adresse IP dupliquée: %1|  
|Remarques et résolution|Ce mécanisme de protection arrête les contrôleurs de domaine dupliqués quand cela est possible (cela n’est ne pas le cas avec DHCP, par exemple). Ajouter un fichier DcCloneConfig.xml valide, supprimez l’indicateur DSRM et réessayez le clonage.|  
  
|||  
|-|-|  
|ID d’événement|29218|  
|Source|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Gravité|Erreur|  
|Message|Clonage de contrôleur de domaine virtuel a échoué. L’opération de clonage n’a pas pu se terminer et le contrôleur de domaine cloné a été redémarré en Mode restauration des Services d’annuaire (DSRM).<br /><br />Veuillez consignés des événements et %systemroot%\debug\dcpromo.log pour plus d’informations sur les erreurs qui correspondent au contrôleur de domaine virtuel le clonage tentative et ou non cette image de clone peut être réutilisée.<br /><br />Si une ou plusieurs entrées de journal indiquent que le processus de clonage ne peut pas être retentée, l’image doit être détruite de manière sécurisée. Dans le cas contraire vous pouvez corriger les erreurs, effacer l’indicateur de démarrage DSRM et redémarrer normalement; après le redémarrage, l’opération de clonage va être retentée.|  
|Remarques et résolution|Examinez les journaux des événements système et Services Active Directory et le fichier dcpromo.log, pour plus d’informations sur la raison pour laquelle le clonage a échoué.|  
  
|||  
|-|-|  
|ID d’événement|29219|  
|Source|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Gravité|Information|  
|Message|Clonage de contrôleur de domaine virtuel a réussi.|  
|Remarques et résolution|Ceci est un événement de succès et seulement un problème si inattendue.|  
  
|||  
|-|-|  
|ID d’événement|29248|  
|Source|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Gravité|Erreur|  
|Message|Clonage de contrôleur de domaine virtuel n’a pas pu obtenir la Notification Winlogon. Le code d’erreur retourné est %1 (%2).<br /><br />Pour plus d’informations sur cette erreur, consultez %systemroot%\debug\dcpromo.log pour les erreurs qui correspondent à la tentative de clonage de contrôleur de domaine virtuel.|  
|Remarques et résolution|Contactez le Support technique Microsoft|  
  
|||  
|-|-|  
|ID d’événement|29249|  
|Source|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Gravité|Erreur|  
|Message|Clonage de contrôleur de domaine virtuel n’a pas pu analyser le fichier de configuration de contrôleur de domaine virtuel.<br /><br />Le code HRESULT retourné est %1.<br /><br />Le fichier de configuration est: %2<br /><br />Corrigez les erreurs dans le fichier de configuration et réessayez l’opération de clonage.<br /><br />Pour plus d’informations sur cette erreur, consultez % systemroot%\debug\dcpromo.log.|  
|Remarques et résolution|Examinez le fichier dclconeconfig.XML en recherchant des erreurs de syntaxe à l’aide d’un éditeur XML et le fichier de schéma DCCloneConfigSchema.xsd.|  
  
|||  
|-|-|  
|ID d’événement|29250|  
|Source|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Gravité|Erreur|  
|Message|Clonage de contrôleur de domaine virtuel a échoué. Des logiciels ou services actuellement activés sur le contrôleur de domaine virtuel cloné qui sont pas présent dans la liste des applications autorisées pour le clonage de contrôleur de domaine virtuel.<br /><br />Voici les entrées manquantes:<br /><br />%2<br /><br />%1 (le cas échéant) a été utilisé en tant que la liste d’inclusion définie.<br /><br />Impossible de terminer l’opération de clonage s’il existe des applications non clonables installées.<br /><br />Exécutez Active Directory PowerShell Cmdlet Get-ADDCCloningExcludedApplicationList pour déterminer les applications qui sont installées sur l’ordinateur cloné, mais pas incluses dans la liste verte et les ajouter à la liste verte si elles sont compatibles avec le clonage de contrôleur de domaine virtuel. Si une de ces applications ne sont pas compatible avec le clonage de contrôleur de domaine virtuel, désinstallez-les avant de réessayer l’opération de clonage.<br /><br />Le processus de clonage de contrôleur de domaine virtuel recherche le fichier de liste d’application autorisée, CustomDCCloneAllowList.xml, selon l’ordre de recherche suivants; le premier fichier trouvé est utilisé et tous les autres sont ignorés:<br /><br />1. le nom de la valeur du Registre: HKey_Local_Machine\System\CurrentControlSet\Services\NTDS\Parameters\AllowListFolder<br /><br />2. le même répertoire que celui où réside le dossier de répertoire de travail DSA<br /><br />3. %windir%\NTDS<br /><br />4. support amovible en lecture/écriture dans l’ordre de la lettre de lecteur à la racine du lecteur|  
|Remarques et résolution|Suivez les instructions du message|  
  
|||  
|-|-|  
|ID d’événement|29251|  
|Source|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Gravité|Erreur|  
|Message|Clonage de contrôleur de domaine virtuel n’a pas pu réinitialiser les adresses IP de l’ordinateur clone.<br /><br />Le code d’erreur retourné est %1 (%2).<br /><br />Cette erreur peut être dû à une configuration incorrecte dans les sections de configuration réseau dans le fichier de configuration de contrôleur de domaine virtuel.<br /><br />Consultez %systemroot%\debug\dcpromo.log pour plus d’informations sur les erreurs qui correspondent aux adresses IP réinitialisation lors de tentatives de clonage de contrôleur de domaine virtuel.<br /><br />Vous trouverez plus d’informations sur la réinitialisation des adresses IP sur l’ordinateur cloné à https://go.microsoft.com/fwlink/?LinkId=208030|  
|Remarques et résolution|Vérifiez les informations IP définies dans le fichier dccloneconfig.xml sont valides et qu’il n’existe pas déjà de l’ordinateur source d’origine.|  
  
|||  
|-|-|  
|ID d’événement|29253|  
|Source|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Gravité|Erreur|  
|Message|Clonage de contrôleur de domaine virtuel a échoué. Le contrôleur de domaine clone n’a pas pu localiser le maître d’opérations de contrôleur principal de domaine dans le domaine d’accueil de l’ordinateur cloné de l’ordinateur cloné.<br /><br />Le code d’erreur retourné est %1 (%2).<br /><br />Vérifiez que le contrôleur de domaine principal dans le domaine d’accueil de l’ordinateur cloné est affecté à un contrôleur de domaine actif, est en ligne et est opérationnel. Vérifiez que l’ordinateur cloné dispose d’une connectivité LDAP/RPC au contrôleur de domaine principal via les ports et protocoles requis.|  
|Remarques et résolution|Validez l’adresse IP contrôleur de domaine cloné et informations DNS sont définies. Utilisez Dcdiag.exe locatorcheck pour vérifier si l’émulateur est en ligne, utilisez Nltest.exe/Server:* <PDCE> * /dclist:* <domain> * valide RPC, obtenez une capture réseau à partir de l’émulateur pendant le clonage échoue et analyser le trafic.|  
  
|||  
|-|-|  
|ID d’événement|29254|  
|Source|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Gravité|Erreur|  
|Message|Clonage de contrôleur de domaine virtuel n’a pas pu établir une liaison au contrôleur de domaine principal %1.<br /><br />Le code d’erreur retourné est %2 (%3).<br /><br />Vérifiez que le contrôleur de domaine principal %1 est en ligne et est opérationnel. Vérifiez que l’ordinateur cloné dispose d’une connectivité LDAP/RPC au contrôleur de domaine principal via les ports et protocoles requis.|  
|Remarques et résolution|Validez l’adresse IP contrôleur de domaine cloné et informations DNS sont définies. Utilisez Dcdiag.exe locatorcheck pour vérifier si l’émulateur est en ligne, utilisez Nltest.exe/Server:* <PDCE> * /dclist:* <domain> * valide RPC, obtenez une capture réseau à partir de l’émulateur pendant le clonage échoue et analyser le trafic.|  
  
|||  
|-|-|  
|ID d’événement|29255|  
|Source|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Gravité|Erreur|  
|Message|Clonage de contrôleur de domaine virtuel a échoué.<br /><br />Une tentative de création d’objets sur le contrôleur de domaine principal %1 requis pour l’image clonée a retourné l’erreur %2 (%3).<br /><br />Vérifiez que le contrôleur de domaine cloné est autorisé à se cloner. Recherchez les événements connexes dans le journal des événements Service d’annuaire sur le contrôleur de domaine principal %1.|  
|Remarques et résolution|Recherchez l’erreur spécifique dans Microsoft TechNet, Knowledgebase et blogs Microsoft pour déterminer sa signification et puis résolvez le problème en fonction des résultats.|  
  
|||  
|-|-|  
|ID d’événement|29256|  
|Source|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Gravité|Erreur|  
|Message|Échec d’une tentative pour définir le démarrage dans l’indicateur de Mode de restauration des Services d’annuaire avec le code d’erreur %1.<br /><br />Consultez %systemroot%\debug\dcpromo.log pour plus d’informations sur les erreurs.|  
|Remarques et résolution|Examinez le journal Services d’annuaire et dcpromo.log pour plus d’informations. Examinez les journaux des événements Application et système. Vérifiez si une application tierce ne bloque pas l’utilisation des privilèges.|  
  
|||  
|-|-|  
|ID d’événement|29257|  
|Source|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Gravité|Erreur|  
|Message|Clonage de contrôleur de domaine virtuel a été effectué. Échec d’une tentative pour redémarrer l’ordinateur avec le code d’erreur %1.<br /><br />Redémarrez l’ordinateur pour terminer l’opération de clonage.|  
|Remarques et résolution|Examinez les journaux des événements Application et système. Vérifiez si une application tierce ne bloque pas l’utilisation des privilèges.|  
  
|||  
|-|-|  
|ID d’événement|29264|  
|Source|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Gravité|Erreur|  
|Message|Échec d’une tentative pour effacer le démarrage dans l’indicateur de Mode de restauration des Services d’annuaire avec le code d’erreur %1.<br /><br />Consultez %systemroot%\debug\dcpromo.log pour plus d’informations sur les erreurs.|  
|Remarques et résolution|Examinez le journal Services d’annuaire et dcpromo.log pour plus d’informations. Examinez les journaux des événements Application et système. Vérifiez si une application tierce ne bloque pas l’utilisation des privilèges.|  
  
|||  
|-|-|  
|ID d’événement|29265|  
|Source|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Gravité|Information|  
|Message|Clonage de contrôleur de domaine virtuel a réussi. Le domaine virtuel fichier clonage de contrôleur configuration %1 a été renommé en %2.|  
|Remarques et résolution|Non applicable, il s’agit d’un événement de succès.|  
  
|||  
|-|-|  
|ID d’événement|29266|  
|Source|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Gravité|Erreur|  
|Message|Clonage de contrôleur de domaine virtuel a réussi. La tentative de renommer le fichier de configuration clonage domaine virtuel contrôleur %1 a échoué avec le code d’erreur %2 (%3).|  
|Remarques et résolution|Renommez manuellement le fichier dccloneconfig.xml.|  
  
|||  
|-|-|  
|ID d’événement|29267|  
|Source|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Gravité|Erreur|  
|Message|Clonage de contrôleur de domaine virtuel n’a pas pu vérifier que le clonage de contrôleur de domaine virtuel de la liste des applications autorisées.<br /><br />Le code d’erreur retourné est %1 (%2).<br /><br />Cette erreur peut être provoquée par une syntaxe fichier liste d’autorisation erreur dans le clone (est le fichier défini: %3). Pour plus d’informations sur cette erreur, consultez % systemroot%\debug\dcpromo.log.|  
|Remarques et résolution|Suivez les instructions de l’événement|  
  
##### <a name="error-messages"></a>Messages d’erreur  
Il n’y a aucuns erreurs interactives directes pour le clonage du contrôleur de domaine virtualisés a échoué; toutes les informations relatives au clonage consigne dans le système et services d’annuaire et la promotion du contrôleur de domaine consigne dans dcpromo.log. Toutefois, si le serveur démarre en Mode de restauration des services d’annuaire, enquêtez immédiatement, car la promotion ou le clonage a échoué.  
  
Le fichier dcpromo.log est le premier emplacement pour rechercher un échec du clonage. En fonction du problème listé, il peut être nécessaire d’examiner ensuite les Services d’annuaire et les journaux système pour approfondir les Diagnostics.  
  
#### <a name="known-issues-and-support-scenarios"></a>Problèmes connus et scénarios de prise en charge  
Voici les problèmes courants rencontrés pendant le processus de développement de Windows Server 2012. Tous ces problèmes sont «normaux» et ont une solution de contournement valide ou technique plus appropriée pour les éviter en premier lieu. Certains peuvent être résolu dans les versions ultérieures de Windows Server 2012.  
  
|||  
|-|-|  
|**Problème**|**Échec du clonage, DSRM**|  
|**Symptômes**|Le clone démarre en Mode de restauration des Services d’annuaire|  
|**Résolution et remarques**|Validez toutes les étapes suivies à partir des sections déploiement de contrôleur de domaine virtualisé et [méthodologie générale pour la résolution des problèmes de clonage de contrôleur de domaine](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_GeneralMethodology)<br /><br />Décrit dans la base de connaissances 2742844.|  
  
|||  
|-|-|  
|**Problème**|**Baux d’IP supplémentaires lorsque vous utilisez DHCP pour cloner**|  
|**Symptômes**|Après le clonage réussi d’un contrôleur de domaine et à l’aide de DHCP, le premier démarrage du clone un bail DHCP. Puis, lorsque le serveur est renommé et redémarré en tant qu’un contrôleur de domaine, il faut un second bail DHCP. La première adresse IP n’est pas libérée et vous vous retrouvez avec un bail «fantôme»|  
|**Résolution et remarques**|Manuellement, supprimer le bail d’adresse inutilisé dans DHCP ou laissez-le expirer normalement. Décrit dans la base de connaissances 2742836.|  
  
|||  
|-|-|  
|**Problème**|**Le clonage échoue en mode DSRM après une très longue attente**|  
|**Symptômes**|Le clonage semble suspendre à «clonage du contrôleur de domaine est x % d’avancement» pour entre 8 et 15 minutes. Après cela, le clonage échoue et démarre en mode DSRM.|  
|**Résolution et remarques**|L’ordinateur cloné ne peut pas obtenir une adresse IP dynamique DHCP ou SLAAC, utilise une adresse IP en double ou ne peut pas trouver le contrôleur de domaine principal. Les multiples tentatives effectuées par le clonage entraînent un retard. Résolvez le problème réseau pour permettre le clonage.<br /><br />Décrit dans la base de connaissances 2742844.|  
  
|||  
|-|-|  
|**Problème**|**Le clonage ne recrée pas tous les noms principaux de service**|  
|**Symptômes**|Si un ensemble de *trois parties* noms de principal du service (SPN) inclut à la fois un nom NetBIOS avec un port et un nom NetBIOS identique sans port, l’entrée sans port n’est pas recréé avec le nouveau nom d’ordinateur. Par exemple:<br /><br />customspn / DC1: 200 / app1 utilisation non valide de symboles *Ceci est recréé avec le nouveau nom d’ordinateur*<br /><br />customspn/DC1/app1 utilisation non valide de symboles *Ceci est recréé avec le nouveau nom d’ordinateur*<br /><br />Les noms complets sont recréés et les noms principaux de service sans trois parties sont recréés, indépendamment des ports. Par exemple, ils sont recréés correctement sur le clone:<br /><br />customspn / DC1: 202 valide utilisation de symboles *Ceci est recréé*<br /><br />customspn/DC1 utilisation non valide de symboles *Ceci est recréé*<br /><br />customspn/DC1.corp.contoso.com:202 utilisation non valide de symboles *Ceci est recréé nom*<br /><br />customspn/DC1.corp.contoso.com utilisation non valide de symboles *Ceci est recréé*|  
|**Résolution et remarques**|Il s’agit d’une limitation du processus rename contrôleur domaine dans Windows, pas seulement au clonage. SPN trois parties ne sont pas gérés par la logique de changement de nom dans n’importe quel scénario. Plus les services Windows inclus ne sont pas affectés par ce problème, car ils recréent les SPN manquants en fonction des besoins. Autres applications peuvent nécessiter l’entrer manuellement le SPN pour résoudre le problème.<br /><br />Décrit dans la base de connaissances 2742874.|  
  
|||  
|-|-|  
|**Problème**|**Le clonage échoue et entraîne un démarrage en mode DSRM. erreurs réseau générales**|  
|**Symptômes**|Le clone démarre en Mode réparation Services d’annuaire. Il existe général d’erreurs de mise en réseau.|  
|Résolution et remarques|Assurez-vous que le nouveau clone n’est pas une adresse MAC statique en double affectée par le contrôleur de domaine source; Vous pouvez voir si un ordinateur virtuel utilise des adresses MAC statiques en exécutant cette commande sur l’hôte hyperviseur pour ordinateurs virtuels source et de clone:<br /><br />Get-VM - VMName *test-vm* & #124; Get-VMNetworkAdapter & #124; fl *<br /><br />Modifier l’adresse MAC pour une adresse statique unique ou passez à l’aide des adresses MAC dynamiques.<br /><br />N° 2742844|  
  
|||  
|-|-|  
|**Problème**|**Le clonage échoue et démarre en mode DSRM en tant qu’un doublon d’un contrôleur de domaine source**|  
|**Symptômes**|Un nouveau clone démarre sans clonage. Le fichier dccloneconfig.xml n’est pas renommé et le serveur démarre en Mode de restauration des services d’annuaire. Le journal des événements Services d’annuaire l’erreur 2164<br /><br />*<COMPUTERNAME>*n’a pas pu démarrer le service DsRoleSvc pour cloner le contrôleur de domaine virtuel local.|  
|**Résolution et remarques**|Examinez les paramètres de service pour le service de serveur de rôles DS (DsRoleSvc) et assurez-vous que son type de démarrage est défini sur manuel. Vérifiez qu’aucun programme tiers n’empêche le démarrage de ce service.<br /><br />Pour plus d’informations sur la façon de récupérer ce contrôleur de domaine secondaire tout en garantissant que les mises à jour soient répliquées en sortie, voir l’article Microsoft KB 2742970.|  
  
|||  
|-|-|  
|**Problème**|**Le clonage échoue et démarre en mode DSRM, erreur 8610**|  
|**Symptômes**|Le clone démarre en Mode de restauration des Services d’annuaire. Affiche erreur 8610 (c'est-à-dire ERROR_DS_ROLE_NOT_VERIFIED 8610 ou 0x21A2)|  
|**Résolution et remarques**|Se produit si le contrôleur de domaine principal est détectable mais qu’il n’a pas effectué une réplication suffisante pour pouvoir endosser le rôle. Par exemple, si le clonage a démarré et qu’un autre administrateur déplace le rôle FSMO PDCE vers un nouveau contrôleur de domaine.<br /><br />Décrit dans la base de connaissances 2742916.|  
  
|||  
|-|-|  
|**Problème**|**Le clonage échoue et entraîne un démarrage en mode DSRM. erreurs réseau générales**|  
|**Symptômes**|Le clone démarre en Mode de restauration des Services d’annuaire. Il existe général d’erreurs de mise en réseau.|  
|**Résolution et remarques**|Assurez-vous que le nouveau clone n’est pas une adresse MAC statique en double affectée par le contrôleur de domaine source; Vous pouvez voir si un ordinateur virtuel utilise des adresses MAC statiques en exécutant cette commande sur l’ordinateur hôte Hyper-V pour la source et le clone les ordinateurs virtuels:<br /><br />Get-VM - VMName *test-vm* & #124; Get-VMNetworkAdapter & #124; fl *<br /><br />Modifier l’adresse MAC pour une adresse statique unique ou passez à l’aide des adresses MAC dynamiques.<br /><br />Décrit dans la base de connaissances 2742844.|  
  
|||  
|-|-|  
|**Problème**|**Le clonage échoue et démarre en mode DSRM**|  
|**Symptômes**|Le clone démarre en Mode de réparation des Services de répertoire|  
|**Résolution et remarques**|Assurez-vous que le fichier dccloneconfig.xml contient la définition de schéma (voir sampledccloneconfig.xml, ligne 2):<br /><br />**< d3c:DCCloneConfig xmlns:d3c="uri:microsoft.com:schemas:DCCloneConfig» >**<br /><br />N° 2742844|  
  
|||  
|-|-|  
|Problème|**Aucun serveur d’accès n’est la journalisation des erreurs disponibles en mode DSRM.**|  
|**Symptômes**|Le clone démarre en Mode réparation Services d’annuaire. Vous tentez d’ouverture de session et recevez l’erreur:<br /><br />**Il n’y a aucun serveur d’ouverture de session est disponible pour traiter la demande d’ouverture de session**|  
|**Résolution et remarques**|Vérifiez que vous vous connectez avec le compte administrateur DSRM et non le compte de domaine. Utilisez la flèche gauche et tapez un nom d’utilisateur:<br /><br />**. \administrator**<br /><br />N° 2742908|  
  
|||  
|-|-|  
|**Problème**|**Source du clonage échoue en mode DSRM, erreur**|  
|**Symptômes**|Durant le clonage, échoue 8437 «créer le clone du contrôleur de domaine des objets sur le contrôleur de domaine principal n’a pas pu» (0x20f5)|  
|**Résolution et remarques**|Nom d’ordinateur dupliqué a été définie dans le fichier DCCloneConfig.xml en tant que la source de contrôleur de domaine ou contrôleur de domaine existant. Le nom d’ordinateur doit également être dans le format de nom de l’ordinateur NetBIOS (15 caractères au maximum, pas un nom de domaine complet).<br /><br />Corrigez le fichier dccloneconfig.xml en définissant un nom unique et valide.<br /><br />N° 2742959|  
  
|||  
|-|-|  
|**Problème**|**Nouveau-addccloneconfigfile erreur «l’index était hors limites»**|  
|**Symptômes**|Lorsque vous exécutez l’applet de commande Nouveau-addccloneconfigfile, vous recevez des erreurs:<br /><br />L’index était hors limites. Doit être non négatif et inférieur à la taille de la collection.|  
|**Résolution et remarques**|Vous devez exécuter l’applet de commande dans une console Windows PowerShell d’administrateur élevés. Cette erreur est due à un manque de membre du groupe Administrateurs local sur l’ordinateur.<br /><br />N° 2742927|  
  
|||  
|-|-|  
|**Problème**|**Échec du clonage, contrôleur de domaine dupliqué**|  
|**Symptômes**|Le clone démarre sans clonage et duplique existant de contrôleur de domaine source|  
|**Résolution et remarques**|L’ordinateur a été copié et démarré mais ne contient pas d’un fichier DcCloneConfig.xml dans un des emplacements pris en charge et ne disposait pas une adresse IP dupliquée avec le contrôleur de domaine source. Le contrôleur de domaine doit être supprimé correctement afin d’éviter toute perte de données.<br /><br />N° 2742970|  
  
|||  
|-|-|  
|**Problème**|**New-ADDCCloneConfigFile échoue avec le serveur n’est pas opérationnel erreur lorsqu’il vérifie si le contrôleur de domaine source est un membre du groupe de contrôleurs de domaine clonables si un catalogue global n’est pas disponible.**|  
|**Symptômes**|Lorsque vous exécutez New-ADDCCloneConfigFile pour créer un fichier dccloneconfig.xml, vous recevez des erreurs:<br /><br />Code - le serveur n’est pas opérationnel|  
|**Résolution et remarques**|Vérifiez la connectivité à un catalogue global à partir du serveur sur lequel vous exécutez New-ADDCCloneConfigFile et vérifiez que l’appartenance du contrôleur de domaine source dans le groupe de contrôleurs de domaine clonables a été répliqué vers ce catalogue global.<br /><br />Exécutez la commande suivante comme un moyen de vider le cache du localisateur de contrôleur de domaine pour les cas où un catalogue global ou un contrôleur de domaine ont peut-être été effectuée en mode hors connexion récemment:<br /><br />Code - nltest/dsgetdc: /FORCE / GC|  
  
### <a name="advanced-troubleshooting"></a>Dépannage avancé  
Ce module vise à enseigner la résolution avancée des problèmes à l’aide de *fonctionne* exemples avec une explication de ce qui s’est produite de journaux. Si vous comprenez ce à quoi ressemble une opération de contrôleur de domaine virtualisé, échecs sont évidents dans votre environnement. Ces journaux sont présentés par source, dans l’ordre croissant de *prévu* événements (même quand ils sont des avertissements et erreurs) liés à un contrôleur de domaine cloné au sein de chaque journal.  
  
#### <a name="cloning-a-domain-controller"></a>Le clonage d’un contrôleur de domaine  
Dans cet exemple, le contrôleur de domaine clone utilise le protocole DHCP pour obtenir une adresse IP, réplique SYSVOL FRS ou DFSR (consultez le journal approprié si nécessaire), est un catalogue global et utilise un fichier dccloneconfig.xml vide.  
  
##### <a name="directory-services-event-log"></a>Journal des événements Services Active  
Le journal Services d’annuaire contient la majorité des informations opérationnelles clonage en fonction des événements. L’hyperviseur change l’ID de génération d’ordinateur virtuel et le service NTDS il note puis invalide le pool RID et change l’ID d’invocation. Le nouvel ID de génération d’ordinateur virtuel est défini et le serveur réplique Active Directory, les données entrant. Le service DFSR est arrêté et sa base de données qui héberge SYSVOL est supprimé, forcer une synchronisation ne faisant pas autorité entrant. La limite supérieure du numéro USN est ajustée.  
  
||||  
|-|-|-|  
|**ID d’événement**|**Source**|**Message**|  
|**2160**|ActiveDirectory_DomainService|Les Services de domaine Active Directory local a trouvé un fichier de configuration clonage du contrôleur de domaine virtuel.<br /><br />Le fichier de configuration clonage du contrôleur de domaine virtuel se trouve à:<br /><br />*<path>*\DCCloneConfig.Xml<br /><br />L’existence du fichier configuration de clonage de contrôleur de domaine virtuel indique que le contrôleur de domaine virtuel local est un clone d’un autre contrôleur de domaine virtuel. Les Services de domaine Active Directory commenceront à se cloner.|  
|**2191**|ActiveDirectory_DomainService|Les Services de domaine Active Directory définir la valeur de Registre suivante pour désactiver les mises à jour DNS.<br /><br />Clé de Registre:<br /><br />SYSTEM\CurrentControlSet\Services\Netlogon\Parameters<br /><br />Valeur de Registre:<br /><br />UseDynamicDns<br /><br />Données de la valeur du Registre:<br /><br />0<br /><br />Pendant le processus de clonage, l’ordinateur local peut avoir le même nom d’ordinateur que l’ordinateur source clone pendant une courte période. DNS A et l’inscription des enregistrements AAAA sont désactivées pendant cette période afin des clients ne peuvent pas envoyer de demandes à l’ordinateur local en cours de clonage. Le processus de clonage activer les mises à jour DNS à nouveau une fois le clonage est terminé.|  
|**2191**|ActiveDirectory_DomainService|Les Services de domaine Active Directory définir la valeur de Registre suivante pour désactiver les mises à jour DNS.<br /><br />Clé de Registre:<br /><br />SYSTEM\CurrentControlSet\Services\Dnscache\Parameters<br /><br />Valeur de Registre:<br /><br />RegistrationEnabled<br /><br />Données de la valeur du Registre:<br /><br />0<br /><br />Pendant le processus de clonage, l’ordinateur local peut avoir le même nom d’ordinateur que l’ordinateur source clone pendant une courte période. DNS A et l’inscription des enregistrements AAAA sont désactivées pendant cette période afin des clients ne peuvent pas envoyer de demandes à l’ordinateur local en cours de clonage. Le processus de clonage activer les mises à jour DNS à nouveau une fois le clonage est terminé.<br /><br />«Information 7/2/2012 3:12:49 PM Microsoft-Windows-ActiveDirectory_DomainService 2191-Configuration interne» les Services de domaine Active Directory définir la valeur de Registre suivante pour désactiver les mises à jour DNS.<br /><br />Clé de Registre:<br /><br />SYSTEM\CurrentControlSet\Services\Tcpip\Parameters<br /><br />Valeur de Registre:<br /><br />DisableDynamicUpdate<br /><br />Données de la valeur du Registre:<br /><br />1<br /><br />Pendant le processus de clonage, l’ordinateur local peut avoir le même nom d’ordinateur que l’ordinateur source clone pendant une courte période. DNS A et l’inscription des enregistrements AAAA sont désactivées pendant cette période afin des clients ne peuvent pas envoyer de demandes à l’ordinateur local en cours de clonage. Le processus de clonage activer les mises à jour DNS à nouveau une fois le clonage est terminé.|  
|**2172**|ActiveDirectory_DomainService|Lisez l’attribut msDS-GenerationId de l’objet ordinateur du contrôleur de domaine.<br /><br />valeur de l’attribut msDS-GenerationId:<br /><br />*<Number>*|  
|**2170**|ActiveDirectory_DomainService|Un changement d’ID de génération a été détecté.<br /><br />ID de génération mis en cache dans les services d’annuaire (ancienne valeur):<br /><br />*<Number>*<br /><br />ID de génération actuellement dans l’ordinateur virtuel (nouvelle valeur):<br /><br />*<Number>*<br /><br />Le changement d’ID de génération se produit après l’application d’une capture instantanée d’ordinateur virtuel, après une opération d’importation de machine virtuelle ou après une opération de migration dynamique. Les Services de domaine Active Directory crée un ID d’appel pour récupérer le contrôleur de domaine. Contrôleurs de domaine virtualisés ne doivent pas être restaurés à l’aide de captures instantanées d’ordinateur virtuel. La méthode prise en charge pour restaurer le contenu d’une base de données des Services de domaine Active Directory consiste à restaurer une sauvegarde de l’état système effectuée avec une application de sauvegarde prenant en charge les Services de domaine Active Directory.|  
|**1109**|ActiveDirectory_DomainService|L’attribut invocationID pour ce serveur d’annuaire a été modifié. Le numéro de séquence de mise à jour la plus élevé au moment de que la création de la sauvegarde est le suivant:<br /><br />Attribut InvocationID (ancienne valeur):<br /><br />*<GUID>*<br /><br />Attribut InvocationID (nouvelle valeur):<br /><br />*<GUID>*<br /><br />Numéro de séquence de mise à jour:<br /><br />*<Number>*<br /><br />L’attribut invocationID est modifié quand un serveur d’annuaire est restauré à partir du média de sauvegarde, est configuré pour héberger une partition d’annuaire d’application accessible en écriture, a été redémarré après une capture instantanée d’ordinateur virtuel a été appliquée, après une opération d’importation de machine virtuelle, ou après une opération de migration dynamique. Contrôleurs de domaine virtualisés ne doivent pas être restaurés à l’aide de captures instantanées d’ordinateur virtuel. La méthode prise en charge pour restaurer le contenu d’une base de données des Services de domaine Active Directory consiste à restaurer une sauvegarde de l’état système effectuée avec une application de sauvegarde prenant en charge les Services domaine Active Directory.|  
|**1000**|ActiveDirectory_DomainService|Démarrage des Services de domaine Microsoft Active Directory terminé.|  
|**1394**|ActiveDirectory_DomainService|Tous les problèmes empêchant les mises à jour à la base de données des Services de domaine Active Directory ont été résolus. Nouvelles mises à jour la base de données des Services de domaine Active Directory réussissent. Le service Net Logon a redémarré.|  
|**2163**|ActiveDirectory_DomainService|Le service DsRoleSvc a démarré pour cloner le contrôleur de domaine virtuel local.|  
|**326**|NTDS ISAM|NTDSA NTDS (536): Le moteur de base de données attaché une base de données (1, C:\Windows\NTDS\ntds.dit). (Heure = 0 seconde)<br /><br />Séquence de minutage interne: [1] 0.000, [2] 0.000, [3] 0.000, [4] 0.000, [5] 0.000, [6] 0.016, [7] 0.000, [8] 0.000, [9] 0.000, [10] 0.000, [11] 0.000, [12] 0.000.<br /><br />Enregistré Cache: 1|  
|**103**|NTDS ISAM|NTDSA NTDS (536): Le moteur de base de données arrêté l’instance (0).<br /><br />Arrêt brutal: 0<br /><br />Séquence de minutage interne: [1] 0.000, [2] 0.000, [3] 0.000, [4] 0.000, [5] 0.032, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.031, [10] 0.000, [11] 0.000, [12] 0.000, [13] 0.000, [14] 0.000, [15] 0.000.|  
|**102**|NTDS ISAM|NTDSA NTDS (536): Le moteur de base de données (6.02.8225.0000) démarre une nouvelle instance (0).|  
|**105**|NTDS ISAM|NTDSA NTDS (536): Le moteur de base de données démarré une nouvelle instance (0). (Heure = 0 seconde)<br /><br />Séquence de minutage interne: [1] 0.016, [2] 0.000, [3] 0.015, [4] 0.078, [5] 0.000, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.046, [10] 0.000, [11] 0.000.|  
|**1004**|ActiveDirectory_DomainService|Les Services de domaine Active Directory ont été arrêtés correctement.|  
|**102**|NTDS ISAM|NTDSA NTDS (536): Le moteur de base de données (6.02.8225.0000) démarre une nouvelle instance (0).|  
|**326**|NTDS ISAM|NTDSA NTDS (536): Le moteur de base de données attaché une base de données (1, C:\Windows\NTDS\ntds.dit). (Heure = 0 seconde)<br /><br />Séquence de minutage interne: [1] 0.000, [2] 0.015, [3] 0.016, [4] 0.000, [5] 0.031, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.000, [10] 0.000, [11] 0.000, [12] 0.000.<br /><br />Enregistré Cache: 1|  
|**105**|NTDS ISAM|NTDSA NTDS (536): Le moteur de base de données démarré une nouvelle instance (0). (Durée = 1 seconde)<br /><br />Séquence de minutage interne: [1] 0.031, [2] 0.000, [3] 0.000, [4] 0.391, [5] 0.000, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.031, [10] 0.000, [11] 0.000.|  
|**1109**|ActiveDirectory_DomainService|L’attribut invocationID pour ce serveur d’annuaire a été modifié. Le numéro de séquence de mise à jour la plus élevé au moment de que la création de la sauvegarde est le suivant:<br /><br />Attribut InvocationID (ancienne valeur):<br /><br />*<GUID>*<br /><br />Attribut InvocationID (nouvelle valeur):<br /><br />*<GUID>*<br /><br />Numéro de séquence de mise à jour:<br /><br />*<Number>*<br /><br />L’attribut invocationID est modifié quand un serveur d’annuaire est restauré à partir du média de sauvegarde, est configuré pour héberger une partition d’annuaire d’application accessible en écriture, a été redémarré après une capture instantanée d’ordinateur virtuel a été appliquée, après une opération d’importation de machine virtuelle, ou après une opération de migration dynamique. Contrôleurs de domaine virtualisés ne doivent pas être restaurés à l’aide de captures instantanées d’ordinateur virtuel. La méthode prise en charge pour restaurer le contenu d’une base de données des Services de domaine Active Directory consiste à restaurer une sauvegarde de l’état système effectuée avec une application de sauvegarde prenant en charge les Services domaine Active Directory.|  
|**1168**|ActiveDirectory_DomainService|Erreur interne: Services de domaine Active Directory une erreur se sont produite.<br /><br />Données supplémentaires<br /><br />Valeur d’erreur (décimal):<br /><br />2<br /><br />Valeur d’erreur (hexadécimal):<br /><br />2<br /><br />ID interne:<br /><br />7011658|  
|**1110**|ActiveDirectory_DomainService|La promotion du contrôleur de domaine d’un catalogue global sera retardée de l’intervalle suivant.<br /><br />Intervalle (minutes):<br /><br />5<br /><br />Ce délai est nécessaire pour que les partitions d’annuaire requis peuvent être préparées avant que le catalogue global est publié. Dans le Registre, vous pouvez spécifier le nombre de secondes que l’agent de système d’annuaire devra attendre avant de promouvoir le contrôleur de domaine local à un catalogue global. Pour plus d’informations sur la valeur de Registre annonce de retard du catalogue Global, voir le Guide de systèmes distribués Kit de ressources|  
|**103**|NTDS ISAM|NTDSA NTDS (536): Le moteur de base de données arrêté l’instance (0).<br /><br />Arrêt brutal: 0<br /><br />Séquence de minutage interne: [1] 0.000, [2] 0.000, [3] 0.000, [4] 0.000, [5] 0.047, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.016, [10] 0.000, [11] 0.000, [12] 0.000, [13] 0.000, [14] 0.000, [15] 0.000.|  
|**1004**|ActiveDirectory_DomainService|Les Services de domaine Active Directory ont été arrêtés correctement.|  
|**1539**|ActiveDirectory_DomainService|Les Services de domaine Active Directory n’a pas pu désactiver le cache d’écriture disque basé sur le logiciel sur le disque dur suivant.<br /><br />Disque dur:<br /><br />c:<br /><br />Données peuvent être perdues pendant les défaillances du système|  
|**2179**|ActiveDirectory_DomainService|L’attribut msDS-GenerationId de l’objet ordinateur du contrôleur de domaine a été défini pour le paramètre suivant:<br /><br />Attribut GenerationID:<br /><br />*<Number>*|  
|**2173**|ActiveDirectory_DomainService|N’a pas pu lire l’attribut msDS-GenerationId de l’objet ordinateur du contrôleur de domaine. Cela peut résulter d’échec de la transaction de base de données, ou l’id de génération n’existe pas dans la base de données locale. L’attribut msDS-GenerationId n’existe pas durant le premier redémarrage après que dcpromo ou le contrôleur de domaine n’est pas un contrôleur de domaine virtuel.<br /><br />Données supplémentaires<br /><br />Code d’échec:<br /><br />6|  
|**1000**|ActiveDirectory_DomainService|Démarrage des Services de domaine Microsoft Active Directory terminé, version 6.2.8225.0|  
|**1394**|ActiveDirectory_DomainService|Tous les problèmes empêchant les mises à jour à la base de données des Services de domaine Active Directory ont été résolus. Nouvelles mises à jour la base de données des Services de domaine Active Directory réussissent. Le service Net Logon a redémarré.|  
|**1128**|ActiveDirectory_DomainService|Vérificateur de cohérence 1128 «une connexion de réplication a été créée à partir du service d’annuaire source pour le service d’annuaire local.<br /><br />Service d’annuaire source:<br /><br />CN = NTDS Settings,*<Domain Controller DN>*<br /><br />Service d’annuaire local:<br /><br />CN = NTDS Settings,*<Domain Controller DN>*<br /><br />Données supplémentaires<br /><br />Code motif:<br /><br />0 x 2<br /><br />ID interne du Point de création:<br /><br />f0a025d|  
|**1999**|ActiveDirectory_DomainService|Le service d’annuaire source a optimisé le numéro de séquence de mise à jour (USN) présenté par le service d’annuaire de destination. Les services d’annuaire source et de destination ont un partenaire de réplication commun. Le service d’annuaire de destination est à jour avec le partenaire de réplication commun, et le service d’annuaire source a été installé à l’aide d’une sauvegarde de ce partenaire.<br /><br />Identificateur du service de répertoire de destination:<br /><br />*<GUID> (<FQDN>)*<br /><br />Identificateur du service annuaire commun:<br /><br />*<GUID>*<br /><br />USN de la propriété commune:<br /><br />*<Number>*<br /><br />Par conséquent, le vecteur de mise à la destination du service d’annuaire a été configuré avec les paramètres suivants.<br /><br />USN de l’objet précédent:<br /><br />0<br /><br />USN de la propriété précédent:<br /><br />0<br /><br />Base de données GUID:<br /><br />*<GUID>*<br /><br />USN de l’objet:<br /><br />*<Number>*<br /><br />USN de la propriété:<br /><br />*<Number>*|  
  
##### <a name="system-event-log"></a>Journal des événements système  
Les prochaines indications relatives aux opérations de clonage se trouvent dans le journal des événements système. Comme l’hyperviseur indique l’ordinateur invité qu’il a été cloné ou restauré à partir d’une capture instantanée, le contrôleur de domaine invalide immédiatement son pool RID pour éviter de dupliquer les principaux de sécurité plus tard. Comme le clonage se poursuit, diverses opérations attendues et des messages s’affichent, principalement autour de démarrage et arrêt des services et certaines prévu à l’origine des erreurs. Après la réussite globale du clonage les notes du journal des événements système.  
  
||||  
|-|-|-|  
|**ID d’événement**|**Source**|**Message**|  
|**16654**|Directory-Services-SAM|Un pool d’identificateurs de comptes (RID) a été invalidé. Cela peut se produire dans les cas attendus suivants:<br /><br />1. un contrôleur de domaine est restauré à partir de la sauvegarde.<br /><br />2. un contrôleur de domaine en cours d’exécution sur un ordinateur virtuel est restauré à partir d’une capture instantanée.<br /><br />3. un administrateur a invalidé le pool manuellement|  
|**7036**|Gestionnaire de contrôle de service|Le service Services de domaine Active Directory est entré dans l’état en cours d’exécution.|  
|**7036**|Gestionnaire de contrôle de service|Le service Centre de Distribution de clés Kerberos est entré dans l’état en cours d’exécution.|  
|**3096**|Accès réseau|Le contrôleur de domaine principal pour ce domaine n’a pas pu être localisé.|  
|**7036**|Gestionnaire de contrôle de service|Le service Gestionnaire de comptes de sécurité est entré dans l’état en cours d’exécution.|  
|**7036**|Gestionnaire de contrôle de service|Le service serveur est entré dans l’état en cours d’exécution.|  
|**7036**|Gestionnaire de contrôle de service|Le service Netlogon est entré dans l’état en cours d’exécution.|  
|**7036**|Gestionnaire de contrôle de service|Le service Services Web Active Directory est entré dans l’état en cours d’exécution.|  
|**7036**|Gestionnaire de contrôle de service|Le service de réplication DFS est entré dans l’état en cours d’exécution.|  
|**7036**|Gestionnaire de contrôle de service|Le service de réplication de fichiers est entré dans l’état en cours d’exécution.|  
|**14533**|Microsoft-Windows-DfsSvc|DFS a fini la création de tous les espaces de noms.|  
|**14531**|Microsoft-Windows-DfsSvc|Serveur DFS a fini de s’initialiser.|  
|**7036**|Gestionnaire de contrôle de service|Le service DFS Namespace est entré dans l’état en cours d’exécution.|  
|**7023**|Gestionnaire de contrôle de service|Le service de messagerie Intersite s’est arrêté avec l’erreur suivante:<br /><br />Le serveur spécifié ne peut pas effectuer l’opération demandée.|  
|**7036**|Gestionnaire de contrôle de service|Le service de messagerie Intersite est entré dans l’état d’arrêt.|  
|**5806**|Accès réseau|Mises à jour dynamiques ont été désactivées manuellement sur ce contrôleur de domaine.<br /><br />ACTION DE L’UTILISATEUR<br /><br />Reconfigurer ce contrôleur de domaine pour utiliser les mises à jour dynamiques ou ajouter manuellement les enregistrements DNS à partir du fichier '% SystemRoot%\System32\Config\Netlogon.dns' pour la base de données DNS.»|  
|**16651**|Directory-Services-SAM|La demande de nouveau pool d’identificateurs de comptes a échoué. L’opération sera tentée jusqu'à ce que la demande réussisse. L’erreur est<br /><br />L’opération FSMO demandée a échoué. Le propriétaire FSMO actuel n’a pas pu être contacté.|  
|**7036**|Gestionnaire de contrôle de service|Le service serveur DNS est entré dans l’état en cours d’exécution.|  
|**7036**|Gestionnaire de contrôle de service|Le service de serveur de rôles DS est entré dans l’état en cours d’exécution.|  
|**7036**|Gestionnaire de contrôle de service|Le service Netlogon est entré dans l’état d’arrêt.|  
|**7036**|Gestionnaire de contrôle de service|Le service de réplication de fichiers est entré dans l’état d’arrêt.|  
|**7036**|Gestionnaire de contrôle de service|Le service Centre de Distribution de clés Kerberos est entré dans l’état d’arrêt.|  
|**7036**|Gestionnaire de contrôle de service|Le service serveur DNS est entré dans l’état d’arrêt.|  
|**7036**|Gestionnaire de contrôle de service|Le service Services de domaine Active Directory est entré dans l’état d’arrêt.|  
|**7036**|Gestionnaire de contrôle de service|Le service Netlogon est entré dans l’état en cours d’exécution.|  
|**7040**|Gestionnaire de contrôle de service|Le type de démarrage du service Services de domaine Active Directory a été modifié de démarrage automatique à désactivé.|  
|**7036**|Gestionnaire de contrôle de service|Le service Netlogon est entré dans l’état d’arrêt.|  
|**7036**|Gestionnaire de contrôle de service|Le service de réplication de fichiers est entré dans l’état en cours d’exécution.|  
|**29219**|DirectoryServices-DSROLE-serveur.|Clonage de contrôleur de domaine virtuel a réussi.|  
|**29223**|DirectoryServices-DSROLE-serveur.|Ce serveur est maintenant un contrôleur de domaine.|  
|**29265**|DirectoryServices-DSROLE-serveur.|Clonage de contrôleur de domaine virtuel a réussi. Le fichier de configuration C:\Windows\NTDS\DCCloneConfig.xml de clonage de contrôleur de domaine virtuel a été renommé en C:\Windows\NTDS\DCCloneConfig.20120207-151533.xml.|  
|**1074**|User32|Le processus C:\Windows\system32\lsass.exe (DC2) a lancé le redémarrage de l’ordinateur DC2 pour le compte d’utilisateur Autorite NT\SYSTÈME pour la raison suivante: système d’exploitation: Reconfiguration (planifiée)<br /><br />Code motif: 0 x 80020004<br /><br />Type d’extinction: redémarrer<br /><br />Commentaire:»|  
  
##### <a name="dcpromolog"></a>DCPROMO. JOURNAL  
Le fichier Dcpromo.log contient la partie de promotion réelle du clonage que le journal des événements Services d’annuaire ne décrit pas. Dans la mesure où le journal ne fournit pas le niveau d’explication les entrées de journal des événements, cette section du module contient des informations supplémentaires.  
  
Le processus de promotion signifie que le clonage commence, le contrôleur de domaine perd sa configuration actuelle et est promu à l’aide de la base de données Active Directory existante (comme une promotion IFM), puis le contrôleur de domaine réplique les deltas des changements entrants d’Active Directory et SYSVOL et le clonage est terminée.  
  
> [!NOTE]  
> Le journal a été modifié dans ce module pour améliorer la lisibilité, en supprimant la colonne de date.  
  
> [!NOTE]  
> Pour une explication plue du fichier dcpromo.log, voir la présentation et résolution des problèmes Administration simplifiée AD DS dans Windows Server 2012.  
>   
> [https://go.microsoft.com/fwlink/p/?LinkId=237244](https://go.microsoft.com/fwlink/p/?LinkId=237244)  
  
-   Démarrer la promotion basée sur le clone  
  
-   Définir l’indicateur de Mode de restauration des Services d’annuaire afin que le serveur ne démarre pas sauvegarder normalement en tant que le clone d’origine et cause d’attribution de noms ou les collisions de Service d’annuaire  
  
-   Mise à jour le journal des événements Services d’annuaire  
  
```  
15:14:01 [INFO] vDC Cloneing: Setting Boot into DSRM flag succeeded.  
15:14:01 [WARNING] Cannot get user Token for Format Message: 1725l  
15:14:01 [INFO] vDC Cloning: Created vDCCloningUpdate event.  
15:14:01 [INFO] vDC Cloning: Created vDCCloningComplete event.  
```  
  
-   Arrêter le service NetLogon afin que le contrôleur de domaine ne publie pas  
  
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
  
-   Examinez le fichier dccloneconfig.xml pour les personnalisations spécifié par l’administrateur.  
  
-   Dans cet exemple, il est un fichier vide, afin que tous les paramètres sont générés automatiquement et l’adressage IP automatique est requis à partir du réseau  
  
```  
15:14:02 [INFO] vDC Cloning: Clone config file C:\Windows\NTDS\DCCloneConfig.xml is considered to be a blank file (containing 0 bytes)  
15:14:02 [INFO] vDC Cloning: Parsing clone config file C:\Windows\NTDS\DCCloneConfig.xml returned HRESULT 0x0  
```  
  
-   Ne valider qu’aucun service ou programme installé qui ne font pas partie du fichier DefaultDCCloneAllowList.xml ou CustomDCCloneAllowList.xml  
  
```  
15:14:02 [INFO] vDC Cloning: Checking allowed list:  
15:14:03 [INFO] vDC Cloning: Completed checking allowed list:  
15:14:03 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
```  
  
-   Activer DHCP sur les cartes réseau, dans la mesure où les informations IP n’a pas été spécifiées par l’administrateur  
  
```  
15:14:03 [INFO] vDC Cloning: Enable DHCP:  
15:14:03 [INFO] WMI Instance: Win32_NetworkAdapterConfiguration.Index=12  
15:14:03 [INFO] Method: EnableDHCP  
15:14:03 [INFO] HRESULT code: 0x0 (0)  
15:14:03 [INFO] Return Value: 0x0 (0)  
15:14:03 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:14:03 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
```  
  
-   Recherchez l’émulateur PDC  
  
-   Définir le site du clone (généré automatiquement dans ce cas)  
  
-   Définir le nom du clone (généré automatiquement dans ce cas)  
  
```  
15:14:03 [INFO] vDC Cloning: Found PDC. Name: DC1.root.fabrikam.com  
15:14:04 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:14:04 [INFO] vDC Cloning: Winlogon UI Notification #1: Domain Controller cloning is at 5% completion...  
15:14:05 [INFO] vDC Cloning: Winlogon UI Notification #2: Domain Controller cloning is at 10% completion...  
15:14:05 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:14:05 [INFO] Site of the cloned DC: Default-First-Site-Name  
```  
  
-   Créer l’objet ordinateur clone  
  
-   Renommer le clone pour correspondre au nouveau nom  
  
```  
15:14:05 [INFO] vDC Cloning: Clone DC objects are created on PDC.  
15:14:05 [INFO] Name of the cloned DC: DC2-CL0001  
15:14:05 [INFO] DsRolepSetRegStringValue on System\CurrentControlSet\Services\NTDS\Parameters\CloneMachineName to DC2-CL0001 returned 0  
15:14:05 [INFO] vDC Cloning: Save CloneMachineName in registry: 0x0 (0)  
```  
  
-   Indiquez les paramètres de la promotion, basés sur le fichier dccloneconfig.xml précédent ou des règles de génération automatique  
  
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
  
-   Arrêter et configurer tous les services AD DS liés (NTDS, NTFRS/DFSR, KDC, DNS)  
  
> [!NOTE]  
> Le service DNS prend du temps d’arrêt est prévu dans ce scénario, comme il utilise les zones intégrées à Active Directory qui n’étaient plus disponibles même avant le service NTDS arrêté - voir les événements DNS décrits plus loin dans cette section du module.  
  
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
  
-   Forcer la synchronisation date / heure NT5DS (NTP) avec un autre contrôleur de domaine (généralement le PDCE)  
  
```  
15:15:02 [INFO] Forcing time sync  
```  
  
-   Contacter un contrôleur de domaine qui détient le compte de contrôleur de domaine source du clone  
  
-   Vider les tickets Kerberos existants  
  
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
  
-   Configurer les services DFSR/NTFRS pour exécuter automatiquement  
  
-   Supprimer leurs fichiers de base de données existants pour forcer la synchronisation ne faisant pas autorité de SYSVOL au prochain démarrage du service  
  
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
  
-   Démarrer le processus de promotion à l’aide du fichier de base de données NTDS existant  
  
-   Contacter le maître RID  
  
> [!NOTE]  
> Le service AD DS n’est pas réellement installé ici, il s’agit d’ancienne instrumentation dans le journal  
  
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
  
-   Modifier l’ID d’appel existant qui existaient dans la base de données des ordinateurs source  
  
-   Créer un nouvel objet Paramètres NTDS pour ce clone  
  
-   Répliquer dans delta d’objet Active Directory à partir du contrôleur de domaine partenaire  
  
> [!NOTE]  
> Même si tous les objets sont répertoriés comme étant répliqués, il s’agit uniquement des métadonnées nécessaires pour englober les mises à jour. Tous les objets inchangés dans la base de données NTDS clonée existent déjà et ne nécessitent pas de nouvelle réplication, tout comme à l’aide de la promotion basée sur IFM.  
  
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
  
-   Remplir les partitions de catalogue global en fonction des besoins avec toutes les mises à jour manquantes  
  
-   Terminer la partie AD DS critique de la promotion  
  
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
  
-   Activer l’inscription DNS du client  
  
```  
15:15:18 [INFO] vDC Cloning: Set DisableDynamicUpdate reg value to 0 to enable dynamic update records registration.  
15:15:18 [INFO] vDC Cloning: Set UseDynamicDns reg value to 1 to enable dynamic update records registration.  
15:15:18 [INFO] vDC Cloning: Set RegistrationEnabled reg value to 1 to enable dynamic update records registration.  
```  
  
-   Exécutez les modules SYSPREP spécifiés par le fichier DefaultDCCloneAllowList.xml <SysprepInformation> élément.  
  
```  
15:15:18 [INFO] vDC Cloning: Running sysprep providers.  
15:15:32 [INFO] vDC Cloning: Completed running sysprep providers.  
```  
  
-   La promotion du clonage est terminé  
  
-   Supprimer l’indicateur de démarrage DSRM pour le serveur démarre normalement la prochaine fois  
  
-   Renommez le fichier dccloneconfig.xml afin qu’il n’est pas lu à nouveau au prochain démarrage  
  
-   Redémarrez l’ordinateur  
  
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
  
##### <a name="active-directory-web-services-event-log"></a>Journal des événements de Services Web Active Directory  
Durant le clonage, le NTDS. Base de données DIT est souvent hors connexion pendant de longues périodes. Le service ADWS connecte au moins un événement pour cela. Une fois le clonage terminé, le service ADWS Directory démarre, constate qu’il n’est pas encore un certificat d’ordinateur valide encore (il peut y avoir ou ne peut pas être, en fonction de votre environnement de déploiement d’une PKI Microsoft avec inscription automatique ou non), puis démarre l’instance pour le nouveau contrôleur de domaine.  
  
||||  
|-|-|-|  
|**ID d’événement**|**Source**|**Message**|  
|**1202**|Événements d’Instance de services Web Active Directory|Cet ordinateur héberge désormais l’instance d’annuaire spécifié, mais les Services Web Active Directory pas pu la traiter. Services Web Active Directory vont réessayer cette opération régulièrement.<br /><br />Instance d’annuaire: NTDS<br /><br />Port LDAP de l’instance active: 389<br /><br />Instance d’annuaire port SSL: 636|  
|**1000**|Événements d’Instance de services Web Active Directory|Démarrage de Services Web Active Directory|  
|**1008**|Événements d’Instance de services Web Active Directory|Services Web Active Directory a réussi à réduire leurs privilèges de sécurité|  
|**1100**|Événements d’Instance de services Web Active Directory|Les valeurs spécifiées dans le <appsettings> section du fichier de configuration pour les Services Web Active Directory ont été chargées sans erreurs.|  
|**1400**|Événements d’Instance de services Web Active Directory|Événements de certificat «Services Web Active Directory pas pu trouver un certificat de serveur avec le nom du certificat spécifié. Un certificat est nécessaire pour utiliser des connexions SSL/TLS. Pour utiliser des connexions SSL/TLS, vérifiez qu’un certificat d’authentification serveur valide à partir d’une autorité de Certification approuvée (CA) est installé sur l’ordinateur.<br /><br />Nom du certificat:*<Server FQDN>*|  
|**1100**|Événements d’Instance de services Web Active Directory|Les valeurs spécifiées dans le <appsettings> section du fichier de configuration pour les Services Web Active Directory ont été chargées sans erreurs.|  
|**1200**|Événements d’Instance de services Web Active Directory|Services Web Active Directory traitent l’instance d’annuaire spécifié.<br /><br />Instance d’annuaire: NTDS<br /><br />Port LDAP de l’instance active: 389<br /><br />Instance d’annuaire port SSL: 636|  
  
##### <a name="dns-server-event-log"></a>Journal des événements DNS Server  
Le service DNS rencontrer les pannes attendues brèves durant le clonage, le service DNS est toujours en cours d’exécution pendant que la base de données AD DS est hors connexion. Cela se produit si vous utilisez le DNS intégré à Active Directory, mais ne pas si vous utilisez Standard principal ou secondaire DNS. Ces erreurs sont enregistrées plusieurs fois. Une fois le clonage terminé, DNS revient en ligne normalement.  
  
||||  
|-|-|-|  
|**ID d’événement**|**Source**|**Message**|  
|**4013**|Service de serveur DNS|Le serveur DNS est en attente pour les Services de domaine Active Directory (AD DS) signaler que la synchronisation initiale du répertoire est terminée. Le service serveur DNS ne peut pas démarrer tant que la synchronisation initiale est terminée, car les données DNS essentielles ne peut-être pas encore répliquées sur ce contrôleur de domaine. Si des événements dans le journal des événements de domaine Active Directory indiquent qu’il existe un problème avec la résolution de noms DNS, envisagez d’ajouter l’adresse IP d’un autre serveur DNS pour ce domaine à la liste des serveurs DNS dans les propriétés du protocole Internet de cet ordinateur. Cet événement être consigné toutes les deux minutes jusqu'à ce que les services AD DS a signalé que la synchronisation initiale est terminée avec succès.|  
|**4015**|Service de serveur DNS|Le serveur DNS a rencontré une erreur critique à partir d’Active Directory. Vérifiez qu’Active Directory fonctionne correctement. Les informations de débogage d’erreur étendue (qui peuvent être vides) sont «»». Les données d’événement contient l’erreur.|  
|**4000**|Service de serveur DNS|Le serveur DNS n’a pas pu ouvrir Active Directory.  Ce serveur DNS est configuré pour obtenir et utiliser les informations de l’annuaire pour cette zone et ne peut pas charger la zone sans celui-ci.  Vérifiez qu’Active Directory fonctionne correctement et rechargez la zone. Les données d’événement sont le code d’erreur.|  
|**4013**|Service de serveur DNS|Le serveur DNS est en attente pour les Services de domaine Active Directory (AD DS) signaler que la synchronisation initiale du répertoire est terminée. Le service serveur DNS ne peut pas démarrer tant que la synchronisation initiale est terminée, car les données DNS essentielles ne peut-être pas encore répliquées sur ce contrôleur de domaine. Si des événements dans le journal des événements de domaine Active Directory indiquent qu’il existe un problème avec la résolution de noms DNS, envisagez d’ajouter l’adresse IP d’un autre serveur DNS pour ce domaine à la liste des serveurs DNS dans les propriétés du protocole Internet de cet ordinateur. Cet événement être consigné toutes les deux minutes jusqu'à ce que les services AD DS a signalé que la synchronisation initiale est terminée avec succès.|  
|**2**|Service de serveur DNS|Le serveur DNS a démarré.|  
|**4**|Service de serveur DNS|Le serveur DNS a terminé le chargement en arrière-plan des zones. Toutes les zones sont désormais disponibles pour les mises à jour DNS et les transferts de zones, comme autorisées par leur chaque configuration de zone.|  
  
##### <a name="file-replication-service-event-log"></a>Fichier réplication Service journal des événements  
Le Service de réplication faisant à partir d’un partenaire durant le clonage. Le clonage supprime ce en supprimant les fichiers de base de données NTFRS et laisse le contenu de SYSVOL intact, pour une utilisation en tant que données de valeurs de départ. Les deux tentatives de synchronisation sont attendues.  
  
||||  
|-|-|-|  
|**ID d’événement**|**Source**|**Message**|  
|**13562**|NtFrs|Voici un résumé des avertissements et erreurs rencontrés par le Service de réplication de fichiers lors de l’interrogation du contrôleur de domaine DC2.root.fabrikam.com pour la réplication FRS définie les informations de configuration.<br /><br />Impossible de lier à un contrôleur de domaine. Nouvelle tentative lors du prochain cycle d’interrogation|  
|**13502**|NtFrs|Le Service de réplication est en cours d’arrêt.|  
|**13565**|NtFrs|Service de réplication de fichiers initialise le volume système avec des données à partir d’un autre contrôleur de domaine. L’ordinateur DC2 ne peut pas devenir un contrôleur de domaine jusqu'à ce que ce processus est terminé. Le volume système sera ensuite partagé en tant que SYSVOL.<br /><br />Pour vérifier le partage SYSVOL, à l’invite de commandes, tapez:<br /><br />commande NET<br /><br />Lorsque le Service de réplication de fichiers termine le processus d’initialisation, le partage SYSVOL apparaîtra.<br /><br />L’initialisation du volume système peut prendre un certain temps. La durée dépend de la quantité de données dans le volume système, la disponibilité des autres contrôleurs de domaine et l’intervalle de réplication entre les contrôleurs de domaine.|  
|**13501**|NtFrs|Démarre le Service de réplication de fichiers|  
|**13502**|NtFrs|Le Service de réplication est en cours d’arrêt.|  
|**13503**|NtFrs|Le Service de réplication de fichiers s’est arrêté.|  
|**13565**|NtFrs|Service de réplication de fichiers initialise le volume système avec des données à partir d’un autre contrôleur de domaine. L’ordinateur DC2 ne peut pas devenir un contrôleur de domaine jusqu'à ce que ce processus est terminé. Le volume système sera ensuite partagé en tant que SYSVOL.<br /><br />Pour vérifier le partage SYSVOL, à l’invite de commandes, tapez:<br /><br />commande NET<br /><br />Lorsque le Service de réplication de fichiers termine le processus d’initialisation, le partage SYSVOL apparaîtra.<br /><br />L’initialisation du volume système peut prendre un certain temps. La durée dépend de la quantité de données dans le volume système, la disponibilité des autres contrôleurs de domaine et l’intervalle de réplication entre les contrôleurs de domaine.|  
|**13501**|NtFrs|Démarre le Service de réplication de fichiers.|  
|**13553**|NtFrs|Le Service de réplication de fichiers a correctement ajouté cet ordinateur au jeu de réplica suivant:<br /><br />«DOMAIN SYSTEM VOLUME (PARTAGE SYSVOL)»<br /><br />Informations relatives à cet événement sont indiquées ci-dessous:<br /><br />Nom DNS de l’ordinateur est*<Domain Controller FQDN>*<br /><br />Nom de membre du jeu de réplica est*<Domain Controller>*<br /><br />Est le chemin d’accès de réplica set racine*<path>*<br /><br />Chemin d’accès de répertoire intermédiaire réplica est*<path>*<br /><br />Chemin d’accès de répertoire de travail réplica est*<path>*|  
|**13520**|NtFrs|Le Service de réplication de fichiers a déplacé les fichiers préexistants de <path>à * <path> *\NtFrs_PreExisting___See_EventLog.<br /><br />Le Service de réplication de fichiers peut supprimer les fichiers dans * <path> *\NtFrs_PreExisting___See_EventLog à tout moment. Fichiers peuvent être enregistrés à partir de la suppression en les copiant hors * <path> *\NtFrs_PreExisting___See_EventLog. Copier les fichiers dans c:\windows\sysvol\domain peut provoquer des conflits de nom si les fichiers existent déjà sur d’autres partenaires de réplication.<br /><br />Dans certains cas, le Service de réplication de fichiers peut copier un fichier à partir de * <path> *\NtFrs_PreExisting___See_EventLog vers * <path> * au lieu de répliquer le fichier à partir d’autres partenaires de réplication.<br /><br />Espace disque peut être libéré à tout moment en supprimant les fichiers dans * <path> *\NtFrs_PreExisting___See_EventLog.»|  
|**13508**|NtFrs|Service de réplication de fichiers à qu’il ne pose pas à activer la réplication de * \\\\ <Domain Controller FQDN> * à * <Domain Controller> * pour * <path> * à l’aide de le<br /><br />DNS name *\\\\<Domain Controller FQDN>*. FRS va essayer à nouveau.<br /><br />Voici quelques raisons que vous pourriez voir cet avertissement.<br /><br />[1] FRS ne peut pas résoudre correctement le nom DNS * \\\\ <Domain Controller FQDN> * à partir de cet ordinateur.<br /><br />[2] FRS n’est pas en cours d’exécution * \\\\ <Domain Controller FQDN> *.<br /><br />[3] les informations de topologie dans les Services de domaine Active Directory pour ce réplica n’a pas encore été répliquées sur tous les contrôleurs de domaine.<br /><br />Ce message du journal des événements apparaîtra une fois par connexion, une fois le problème est résolu vous verrez un autre message du journal des événements indiquant que la connexion a été établie.|  
|**13509**|NtFrs|Le Service de réplication de fichiers a activé la réplication de * \\\\ <Domain Controller FQDN> * à * <Domain Controller> * pour * <Path> * après plusieurs tentatives.|  
|**13516**|NtFrs|Le Service de réplication de fichiers n’empêche plus l’ordinateur * <Domain Controller> * de devenir un contrôleur de domaine. Le volume système a été correctement initialisé et le service Netlogon a été averti que le volume système est maintenant prêt à être partagé en tant que SYSVOL.<br /><br />Entrez «net share» pour vérifier le partage SYSVOL.»|  
  
##### <a name="dfs-replication-event-log"></a>Journal des événements la réplication DFS  
Le service DFSR faisant à partir d’un partenaire durant le clonage. Le clonage supprime ce en supprimant les fichiers de base de données DFSR et laisse le contenu de SYSVOL intact, pour une utilisation en tant que données de valeurs de départ. Les deux tentatives de synchronisation sont attendues.  
  
||||  
|-|-|-|  
|**ID d’événement**|**Source**|**Message**|  
|**1004**|DFSR|Le service de réplication DFS a démarré.|  
|**1314**|DFSR|Le service de réplication DFS a correctement configuré les fichiers journaux de débogage.<br /><br />Informations supplémentaires:<br /><br />Chemin du fichier journal de débogage: C:\Windows\debug|  
|**6102**|DFSR|Le service de réplication DFS a inscrit le fournisseur WMI|  
|**1206**|DFSR|Le service de réplication DFS réussi à contacter le contrôleur de domaine DC2.corp.contoso.com pour accéder aux informations de configuration.|  
|**1210**|DFSR|Le service de réplication DFS configurer correctement un écouteur RPC pour les demandes de réplication entrant.<br /><br />Informations supplémentaires:<br /><br />Port: 0»|  
|**4614**|DFSR|Le service de réplication DFS a initialisé SYSVOL dans le chemin d’accès local C:\Windows\SYSVOL\domain et attend d’effectuer la réplication initiale. Le dossier répliqué demeurera dans l’état de la synchronisation initiale jusqu'à ce qu’il a été répliqué avec son partenaire. Si le serveur a été va être promu contrôleur de domaine, le contrôleur de domaine ne sera pas publier et fonctionner comme un contrôleur de domaine jusqu'à ce que ce problème est résolu. Cela peut se produire si le partenaire spécifié est également dans l’état de la synchronisation initiale, ou si des violations de partage sont rencontrées sur ce serveur ou le partenaire de synchronisation. Si cet événement s’est produite lors de la migration de SYSVOL dans le Service de réplication de fichiers (FRS) à la réplication DFS, modifications ne seront pas répliquées jusqu'à ce que ce problème est résolu. Cela peut provoquer le dossier SYSVOL sur ce serveur pourrait devenir désynchronisé avec les autres contrôleurs de domaine.<br /><br />Informations supplémentaires:<br /><br />Nom du dossier répliqué: Partage SYSVOL<br /><br />ID du dossier répliqué:*<GUID>*<br /><br />Nom du groupe de réplication: Domain System Volume.<br /><br />ID du groupe de réplication:*<GUID>*<br /><br />ID du membre:*<GUID>*<br /><br />En lecture seule: 0|  
|**4604**|DFSR|Le service de réplication DFS initialisé avec succès le dossier SYSVOL répliqué au chemin d’accès local C:\Windows\SYSVOL\domain. Ce membre a terminé la synchronisation initiale de SYSVOL avec le partenaire dc1.corp.contoso.com. Pour vérifier la présence du partage SYSVOL, ouvrez une fenêtre d’invite de commandes et tapez «"net share"».<br /><br />Informations supplémentaires:<br /><br />Nom du dossier répliqué: Partage SYSVOL<br /><br />ID du dossier répliqué:*<GUID>*<br /><br />Nom du groupe de réplication: Domain System Volume.<br /><br />ID du groupe de réplication:*<GUID>*<br /><br />ID du membre:*<GUID>*<br /><br />Partenaire de synchronisation:*<domain controller FQDN>*|  
  
## <a name="BKMK_TshootVDCSafeRestore"></a>Résolution des problèmes de restauration sécurisée des contrôleurs de domaine virtualisés  
  
### <a name="tools-for-troubleshooting"></a>Outils de dépannage  
  
#### <a name="logging-options"></a>Options de journalisation  
Les journaux intégrés sont l’outil le plus important pour la résolution des problèmes de restauration de capture instantanée sécurisée de contrôleur de domaine. Tous ces journaux sont activés et configurés par défaut pour un maximum de commentaires.  
  
|||  
|-|-|  
|**Opération**|**Journal**|  
|**Création de capture instantanée**|-Event événements\journaux des applications et services services\microsoft\windows\hyper-V-Worker|  
|**Restauration de capture instantanée**|-Event événements\journaux des applications et des services\Service<br />-Observateur d’événements\journaux Windows\Système<br />-Observateur d’événements\journaux Windows\Application<br />-Event événements\journaux des applications et services services\Service de réplication<br />-Event événements\journaux des applications et des services\réplication DFS<br />-Event des services\dns d’événements\journaux des applications et services<br />-Event événements\journaux des applications et services services\microsoft\windows\hyper-V-Worker|  
  
#### <a name="tools-and-commands-for-troubleshooting-domain-controller-configuration"></a>Outils et commandes pour la résolution des problèmes de Configuration du contrôleur de domaine  
Pour résoudre les problèmes non décrits par les journaux, utilisez les outils suivants comme point de départ:  
  
-   Dcdiag.exe  
  
-   Repadmin.exe  
  
-   Moniteur réseau 3.4  
  
#### <a name="BKMK_TshhotSafeRestore"></a>Méthodologie générale pour la résolution des problèmes de restauration sécurisée des contrôleurs de domaine  
  
1.  Est la restauration de capture instantanée sécurisée prévue, mais des problèmes?  
  
    1.  Examinez le journal des événements Services d’annuaire  
  
        1.  Existe-t-il des erreurs de restauration de capture instantanée?  
  
        2.  Existe-t-il des erreurs de réplication Active Directory?  
  
    2.  Examinez le journal des événements système  
  
        1.  Existe-t-il des erreurs de communication?  
  
        2.  Existe-t-il des erreurs Active Directory?  
  
2.  La restauration de capture instantanée sécurisée est inattendue?  
  
    1.  Examinez les journaux d’audit d’hyperviseur pour déterminer l’origine une restauration  
  
    2.  Contactez tous les administrateurs de l’hyperviseur et les interroger pour qui a restauré l’ordinateur virtuel sans notification  
  
3.  Est la protection des restaurations USN implémentation serveur et sans restauration sécurisée?  
  
    1.  Examinez le journal des événements Services d’annuaire pour un hyperviseur ou intégration les services non pris en charge  
  
    2.  Examinez le système d’exploitation et valider l’exécution de Windows Server 2012?  
  
### <a name="BKMK_TshootSpecificSafeRestore"></a>Résolution des problèmes spécifiques  
  
#### <a name="events"></a>Événements  
Tous les virtualisés écrivent des événements dans le journal des événements Services d’annuaire de l’ordinateur virtuel du contrôleur de domaine restauré de restauration de capture instantanée sécurisée de contrôleur de domaine. Les journaux des événements Application, système, Service de réplication de fichiers et réplication DFS peut également contenir des informations de dépannage utiles d’échec de restauration.  
  
Voici les événements spécifiques à la restauration sécurisées de Windows Server 2012 dans le journal des événements Services d’annuaire.  
  
|||  
|-|-|  
|**ID d’événement**|**2170**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravité**|Avertissement|  
|**Message**|Un changement d’ID de génération a été détecté.<br /><br />ID de génération mis en cache dans les services d’annuaire (ancienne valeur): %1<br /><br />ID de génération actuellement dans l’ordinateur virtuel (nouvelle valeur): %2<br /><br />Le changement d’ID de génération se produit après l’application d’une capture instantanée d’ordinateur virtuel, après une opération d’importation de machine virtuelle ou après une opération de migration dynamique. *<COMPUTERNAME>*Crée un ID d’appel pour récupérer le contrôleur de domaine. Contrôleurs de domaine virtualisés ne doivent pas être restaurés à l’aide de captures instantanées d’ordinateur virtuel. La méthode prise en charge pour restaurer le contenu d’une base de données des Services de domaine Active Directory consiste à restaurer une sauvegarde de l’état système effectuée avec une application de sauvegarde prenant en charge les Services de domaine Active Directory.|  
|**Remarques et résolution**|Il s’agit d’un événement de succès si la capture instantanée est attendue. Si ce n’est pas le cas, examinez le journal des événements Hyper-V-Worker ou contactez l’administrateur de l’hyperviseur.|  
  
|||  
|-|-|  
|**ID d’événement**|**2174**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravité**|Information|  
|**Message**|Le contrôleur de domaine est un instantané de contrôleur de domaine virtuel restauré ni un clone de contrôleur de domaine virtuel.|  
|**Remarques et résolution**|Événement attendu lors du démarrage des contrôleurs de domaine physiques ou des contrôleurs de domaine virtualisés ne pas restaurées à partir d’une capture instantanée|  
  
|||  
|-|-|  
|**ID d’événement**|**2181**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravité**|Information|  
|**Message**|La transaction a été abandonnée en raison de l’ordinateur virtuel en cours ramené à un état antérieur.  Cela se produit après l’application d’une capture instantanée d’ordinateur virtuel, après une opération d’importation de machine virtuelle ou après une opération de migration dynamique.|  
|**Remarques et résolution**|Attendu lors de la restauration d’une capture instantanée. Les transactions suivent le changement d’ID de génération d’ordinateur virtuel|  
  
|||  
|-|-|  
|**ID d’événement**|**2185**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravité**|Information|  
|**Message**|*<COMPUTERNAME>*arrêté le service FRS ou DFSR utilisé pour répliquer le dossier SYSVOL.<br /><br />Nom du service: %1<br /><br />Active Directory a détecté que l’ordinateur virtuel qui héberge le contrôleur de domaine a été ramené à un état antérieur. *<COMPUTERNAME>*doit initialiser une restauration ne faisant pas autorité sur le réplica SYSVOL local. Cette opération est effectuée en arrêtant le service FRS ou DFSR utilisé pour répliquer le dossier SYSVOL et en le démarrant avec les clés de Registre appropriées et les valeurs pour déclencher la restauration. L’événement 2187 sera journalisé au redémarrage du service FRS ou DFSR.|  
|**Remarques et résolution**|Attendu lors de la restauration d’une capture instantanée. Toutes les données SYSVOL sur ce contrôleur de domaine est remplacée par la copie d’un partenaire du contrôleur de domaine.|  
|**ID d’événement**|2186|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravité**|Erreur|  
|**Message**|*<COMPUTERNAME>*Impossible d’arrêter le service FRS ou DFSR utilisé pour répliquer le dossier SYSVOL.<br /><br />Nom du service: %1<br /><br />Code d’erreur: %2<br /><br />Message d’erreur: %3<br /><br />Active Directory a détecté que l’ordinateur virtuel qui héberge le contrôleur de domaine a été ramené à un état antérieur. *<COMPUTERNAME>*doit initialiser une restauration ne faisant pas autorité sur le réplica SYSVOL local. Pour cela, l’arrêt du service de réplication FRS ou DFSR utilisé pour répliquer le dossier SYSVOL et en le démarrant avec les clés de Registre appropriées et les valeurs pour déclencher la restauration. *<COMPUTERNAME>*Impossible d’arrêter le service actuellement exécuté et ne peut pas effectuer la restauration ne faisant pas autorité. Veuillez effectuer manuellement une restauration ne faisant pas autorité.|  
|**Remarques et résolution**|Examinez les journaux des événements système, FRS et DFSR pour plus d’informations.|  
  
|||  
|-|-|  
|**ID d’événement**|**2187**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravité**|Information|  
|**Message**|*<COMPUTERNAME>*Démarrer le service FRS ou DFSR utilisé pour répliquer le dossier SYSVOL.<br /><br />Nom du service: %1<br /><br />Active Directory a détecté que l’ordinateur virtuel qui héberge le contrôleur de domaine a été ramené à un état antérieur. *<COMPUTERNAME>*a dû initialiser une restauration ne faisant pas autorité sur le réplica SYSVOL local. Ceci a été fait en arrêtant le service FRS ou DFSR utilisé pour répliquer le dossier SYSVOL et en le démarrant avec les clés de Registre appropriées et les valeurs pour déclencher la restauration.|  
|**Remarques et résolution**|Attendu lors de la restauration d’une capture instantanée. Toutes les données SYSVOL sur ce contrôleur de domaine est remplacée par la copie d’un partenaire du contrôleur de domaine.|  
  
|||  
|-|-|  
|**ID d’événement**|**2188**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravité**|Erreur|  
|**Message**|*<COMPUTERNAME>*n’a pas pu démarrer le service FRS ou DFSR utilisé pour répliquer le dossier SYSVOL.<br /><br />Nom du service: %1<br /><br />Code d’erreur: %2<br /><br />Message d’erreur: %3<br /><br />Active Directory a détecté que l’ordinateur virtuel qui héberge le contrôleur de domaine a été ramené à un état antérieur. *<COMPUTERNAME>*doit initialiser une restauration ne faisant pas autorité sur le réplica SYSVOL local. Cela est fait en arrêtant le service FRS ou DFSR utilisé pour répliquer le dossier SYSVOL et en le démarrant avec les clés de Registre appropriées et les valeurs pour déclencher la restauration. *<COMPUTERNAME>*n’a pas pu démarrer le service FRS ou DFSR utilisé pour répliquer le dossier SYSVOL et ne peut pas effectuer la restauration ne faisant pas autorité. Veuillez effectuer une restauration ne faisant pas autorité manuellement et redémarrer le service.|  
|**Remarques et résolution**|Examinez les journaux des événements système, FRS et DFSR pour plus d’informations.|  
  
|||  
|-|-|  
|**ID d’événement**|**2189**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravité**|Information|  
|**Message**|*<COMPUTERNAME>*Définissez les valeurs de Registre suivantes pour initialiser un réplica SYSVOL lors d’une restauration ne faisant pas autorité:<br /><br />Clé de Registre: %1<br /><br />Valeur de Registre: %2<br /><br />Données de la valeur du Registre: %3<br /><br />Active Directory a détecté que l’ordinateur virtuel qui héberge le contrôleur de domaine a été ramené à un état antérieur. *<COMPUTERNAME>*doit initialiser une restauration ne faisant pas autorité sur le réplica SYSVOL local. Cela est fait en arrêtant le service FRS ou DFSR utilisé pour répliquer le dossier SYSVOL et en le démarrant avec les clés de Registre appropriées et les valeurs pour déclencher la restauration.|  
|**Remarques et résolution**|Attendu lors de la restauration d’une capture instantanée. Toutes les données SYSVOL sur ce contrôleur de domaine est remplacée par la copie d’un partenaire du contrôleur de domaine.|  
  
|||  
|-|-|  
|**ID d’événement**|**2190**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravité**|Erreur|  
|**Message**|*<COMPUTERNAME>*Impossible de définir les valeurs de Registre suivantes pour initialiser le réplica SYSVOL lors d’une restauration ne faisant pas autorité:<br /><br />Clé de Registre: %1<br /><br />Valeur de Registre: %2<br /><br />Données de la valeur du Registre: %3<br /><br />Code d’erreur: %4<br /><br />Message d’erreur: %5<br /><br />Active Directory a détecté que l’ordinateur virtuel qui héberge le rôle de contrôleur de domaine a été ramené à un état antérieur. *<COMPUTERNAME>*doit initialiser une restauration ne faisant pas autorité sur le réplica SYSVOL local. Cela est fait en arrêtant le service FRS ou DFSR utilisé pour répliquer le dossier SYSVOL et en le démarrant avec les clés de Registre appropriées et les valeurs pour déclencher la restauration. *<COMPUTERNAME>*Impossible de définir les valeurs de Registre ci-dessus et ne peut pas effectuer la restauration ne faisant pas autorité. Veuillez effectuer manuellement une restauration ne faisant pas autorité.|  
|**Remarques et résolution**|Examinez les journaux des événements Application et système. Examiner les applications tierces ne bloque pas les mises à jour du Registre.|  
  
|||  
|-|-|  
|**ID d’événement**|**2200**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravité**|Information|  
|**Message**|Active Directory a détecté que l’ordinateur virtuel qui héberge le contrôleur de domaine a été ramené à un état antérieur. *<COMPUTERNAME>*Initialise la réplication pour mettre à jour le contrôleur de domaine. Événement 2201 sera consigné lorsque la réplication est terminée.|  
|**Remarques et résolution**|Attendu lors de la restauration d’une capture instantanée. Marque le début de la réplication Active Directory entrante.|  
  
|||  
|-|-|  
|**ID d’événement**|**2201**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravité**|Information|  
|**Message**|Active Directory a détecté que l’ordinateur virtuel qui héberge le contrôleur de domaine a été ramené à un état antérieur. *<COMPUTERNAME>*a terminé la réplication pour mettre à jour le contrôleur de domaine.|  
|**Remarques et résolution**|Attendu lors de la restauration d’une capture instantanée. Marque la fin de la réplication Active Directory entrante.|  
  
|||  
|-|-|  
|**ID d’événement**|**2202**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravité**|Erreur|  
|**Message**|Active Directory a détecté que l’ordinateur virtuel qui héberge le contrôleur de domaine a été ramené à un état antérieur. *<COMPUTERNAME>*Échec de réplication pour mettre à jour le contrôleur de domaine. Le contrôleur de domaine est être mis à jour après la prochaine réplication périodique.|  
|**Remarques et résolution**|Examinez les journaux des événements Services d’annuaire et système. Utilisez repadmin.exe pour tenter de forcer la réplication et notez les défaillances.|  
  
|||  
|-|-|  
|**ID d’événement**|**2204**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravité**|Information|  
|**Message**|*<COMPUTERNAME>*a détecté une modification de l’ID de génération d’ordinateur virtuel. Cette modification signifie que le contrôleur de domaine virtuel a été ramené à un état antérieur. *<COMPUTERNAME>*effectue les opérations suivantes pour protéger le contrôleur de domaine ainsi rétabli contre une possible divergence des données et pour protéger la création des principaux de sécurité avec des SID en doublon:<br /><br />Créer un ID d’appel<br /><br />Invalider le pool RID actuel<br /><br />Propriété des rôles FSMO sera validée à la prochaine réplication entrante. Au cours de cette fenêtre si le contrôleur de domaine détenait un rôle FSMO, ce rôle n’est pas disponible.<br /><br />Démarrer l’opération de restauration du service de réplication SYSVOL.<br /><br />Démarrer la réplication pour mettre le contrôleur de domaine ainsi rétabli à l’état le plus récent.<br /><br />Demander un nouveau pool RID.|  
|**Remarques et résolution**|Attendu lors de la restauration d’une capture instantanée. Ceci explique toutes les diverses opérations de réinitialisation qui se produit dans le cadre du processus de restauration sécurisée.|  
  
|||  
|-|-|  
|**ID d’événement**|**2205**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravité**|Information|  
|**Message**|*<COMPUTERNAME>*a invalidé le pool RID actuel après que le contrôleur de domaine virtuel a été ramené à un état antérieur.|  
|**Remarques et résolution**|Attendu lors de la restauration d’une capture instantanée. Le pool RID local doit être détruit, car le contrôleur de domaine ait le temps parcourue et ils peuvent ont déjà été émis.|  
  
|||  
|-|-|  
|**ID d’événement**|**2206**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravité**|ERREUR|  
|**Message**|*<COMPUTERNAME>*n’a pas pu invalider le pool RID actuel après que le contrôleur de domaine virtuel a été ramené à un état antérieur.<br /><br />Données supplémentaires:<br /><br />Code d’erreur: %1<br /><br />Valeur d’erreur: %2|  
|**Remarques et résolution**|Examinez les journaux des événements Services d’annuaire et système. Valider que le maître RID est en ligne peut être atteint à partir de ce serveur via Dcdiag.exe/test: RidManager|  
  
|||  
|-|-|  
|**ID d’événement**|**2207**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravité**|ERREUR|  
|**Message**|*<COMPUTERNAME>*n’a pas pu être restauré après que le contrôleur de domaine virtuel a été ramené à un état antérieur. Un redémarrage dans DSRM a été demandé. Vérifiez les événements précédents pour plus d’informations.|  
|**Remarques et résolution**|Examinez les journaux des événements Services d’annuaire et système.|  
  
|||  
|-|-|  
|**ID d’événement**|**2208**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravité**|Information|  
|**Message**|*<COMPUTERNAME>*supprimer les bases de données DFSR pour initialiser le réplica SYSVOL lors d’une restauration ne faisant pas autorité.|  
|**Remarques et résolution**|Attendu lors de la restauration d’une capture instantanée. Cela garantit que DFSR synchronise faisant SYSVOL à partir d’un contrôleur de domaine partenaire. Notez que toutes les autres DFSR dossiers répliqués sur le même volume que SYSVOL synchronise également faisant (domaine contrôleurs sont recommandés pas à l’hôte personnalisé que DFSR définit sur le même volume que SYSVOL).|  
  
|||  
|-|-|  
|**ID d’événement**|**2209**|  
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravité**|Erreur|  
|**Message**|*<COMPUTERNAME>*Impossible de supprimer les bases de données DFSR.<br /><br />Données supplémentaires:<br /><br />Code d’erreur: %1<br /><br />Valeur d’erreur: %2<br /><br />Active Directory a détecté que l’ordinateur virtuel qui héberge le contrôleur de domaine a été ramené à un état antérieur. *<COMPUTERNAME>*doit initialiser une restauration ne faisant pas autorité sur le réplica SYSVOL local. Pour DFSR, ceci se fait en arrêtant le service DFSR, la suppression des bases de données DFSR et redémarrer le service. Après le redémarrage, DFSR reconstruire les bases de données et démarre la synchronisation initiale.|  
|**Remarques et résolution**|Examinez le journal des événements DFSR.|  
  
#### <a name="error-messages"></a>Messages d’erreur  
Il en existe pas d’erreurs interactives directes pour l’échec de la restauration de capture instantanée sécurisée du contrôleur de domaine virtualisé; Il enregistre toutes les informations relatives au clonage dans les journaux des événements Services d’annuaire. Naturellement, les erreurs de publicité serveur ou de la réplication critiques se manifestent ailleurs.  
  
#### <a name="known-issues-and-support-scenarios"></a>Problèmes connus et scénarios de prise en charge  
Le [méthodologie générale pour la résolution des problèmes domaine contrôleur de restauration sécurisée](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_TshootSpecificSafeRestore) sont généralement suffisants pour résoudre la plupart des problèmes.  
  
|||  
|-|-|  
|**Problème**|**Ne peut pas créer de nouvelles entités de sécurité sur le contrôleur de domaine restauré récemment de manière sécurisée**|  
|**Symptômes**|Après la restauration d’une capture instantanée, tente de créer un nouveau principal de sécurité (utilisateur, ordinateur, groupe) sur ce contrôleur de domaine échouent:<br /><br />Erreur 0 x 2010<br /><br />Le service d’annuaire n’a pas pu allouer un identificateur relatif.|  
|**Résolution et remarques**|Ce problème est dû aux informations obsolètes du rôle FSMO du maître RID de l’ordinateur restauré. Si le rôle est déplacé vers contrôleur de domaine ou dans un autre après qu’une capture instantanée a été effectuée et restaurée ensuite, le contrôleur de domaine restauré n’aura pas connaissance du maître RID jusqu'à ce que la réplication initiale est terminée.<br /><br />Pour résoudre le problème, autoriser la fin de la réplication Active Directory entrante pour le contrôleur de domaine restauré. Si fonctionne toujours ne pas, vérifiez que tous les contrôleurs de domaine ont la même information qui héberge le contrôleur de domaine le maître RID.|  
  
|||  
|-|-|  
|**Problème**|**Contrôleurs de domaine restauré ne pas partager SYSVOL, publier**|  
|**Symptômes**|Après la restauration d’une capture instantanée, un ou plusieurs contrôleurs de domaine n’effectuent aucune publication, ne partagent pas sysvol et n’ont pas de contenu SYSVOL à jour|  
|**Résolution et remarques**|Les partenaires en amont du contrôleur de domaine n’ont pas d’un réplica SYSVOL de travail qui se réplique correctement avec DFSR ou FRS. Ce problème n’est pas lié à la restauration sécurisée mais est susceptible de se manifester en tant qu’un problème de restauration sécurisée, car le client n’a pas connaissance de l’autre problème de réplication qui affectent les contrôleurs de domaine non restaurés|  
  
### <a name="advanced-troubleshooting"></a>Dépannage avancé  
Ce module vise à enseigner la résolution avancée des problèmes à l’aide de *fonctionne* exemples avec une explication de ce qui s’est produite de journaux. Si vous comprenez ce à quoi ressemble une opération de contrôleur de domaine virtualisé, échecs sont évidents dans votre environnement. Ces journaux sont présentés par source, dans l’ordre croissant de *prévu* événements liés à un contrôleur de domaine cloné au sein de chaque journal.  
  
#### <a name="restoring-a-domain-controller-that-replicates-sysvol-using-dfsr"></a>Restauration d’un contrôleur de domaine qui réplique SYSVOL via les services DFSR  
  
##### <a name="directory-services-event-log"></a>Journal des événements Services Active  
Le journal Services d’annuaire contient la majorité des informations opérationnelles de restauration sécurisée. L’hyperviseur change l’ID de génération d’ordinateur virtuel et le service NTDS il note puis invalide le pool RID et change l’ID d’invocation. Le nouvel ID de génération d’ordinateur virtuel est set et le serveur réplique entrant de données Active Directory. Le service DFSR est arrêté et sa base de données qui héberge SYSVOL est supprimé, forcer une synchronisation ne faisant pas autorité entrant. La limite supérieure du numéro USN est ajustée.  
  
||||  
|-|-|-|  
|**ID d’événement**|**Source**|**Message**|  
|**2170**|ActiveDirectory_DomainService|Un changement d’ID de génération a été détecté.<br /><br />ID de génération mis en cache dans les services d’annuaire (ancienne valeur):<br /><br />*<number>*<br /><br />ID de génération actuellement dans l’ordinateur virtuel (nouvelle valeur):<br /><br />*<number>*<br /><br />Le changement d’ID de génération se produit après l’application d’une capture instantanée d’ordinateur virtuel, après une opération d’importation de machine virtuelle ou après une opération de migration dynamique. Les Services de domaine Active Directory crée un ID d’appel pour récupérer le contrôleur de domaine. Contrôleurs de domaine virtualisés ne doivent pas être restaurés à l’aide de captures instantanées d’ordinateur virtuel. La méthode prise en charge pour restaurer le contenu d’une base de données des Services de domaine Active Directory consiste à restaurer une sauvegarde de l’état système effectuée avec une application de sauvegarde prenant en charge les Services de domaine Active Directory.»|  
|**2181**|ActiveDirectory_DomainService|La transaction a été abandonnée en raison de l’ordinateur virtuel en cours ramené à un état antérieur.  Cela se produit après l’application d’une capture instantanée d’ordinateur virtuel, après une opération d’importation de machine virtuelle ou après une opération de migration dynamique.|  
|**2204**|ActiveDirectory_DomainService|Les Services de domaine Active Directory a détecté une modification de l’ID de génération d’ordinateur virtuel. Cette modification signifie que le contrôleur de domaine virtuel a été ramené à un état antérieur. Les Services de domaine Active Directory effectue les opérations suivantes pour protéger le contrôleur de domaine ainsi rétabli contre une possible divergence des données et pour protéger la création des principaux de sécurité avec des SID en doublon:<br /><br />Créer un ID d’appel<br /><br />Invalider le pool RID actuel<br /><br />Propriété des rôles FSMO sera validée à la prochaine réplication entrante. Au cours de cette fenêtre si le contrôleur de domaine détenait un rôle FSMO, ce rôle n’est pas disponible.<br /><br />Démarrer l’opération de restauration du service de réplication SYSVOL.<br /><br />Démarrer la réplication pour mettre le contrôleur de domaine ainsi rétabli à l’état le plus récent.<br /><br />Demander un nouveau pool RID.»|  
|**2181**|ActiveDirectory_DomainService|La transaction a été abandonnée en raison de l’ordinateur virtuel en cours ramené à un état antérieur.  Cela se produit après l’application d’une capture instantanée d’ordinateur virtuel, après une opération d’importation de machine virtuelle ou après une opération de migration dynamique.|  
|**1109**|ActiveDirectory_DomainService|L’attribut invocationID pour ce serveur d’annuaire a été modifié. Le numéro de séquence de mise à jour la plus élevé au moment de que la création de la sauvegarde est le suivant:<br /><br />Attribut InvocationID (ancienne valeur):<br /><br />*<GUID>*<br /><br />Attribut InvocationID (nouvelle valeur):<br /><br />*<GUID>*<br /><br />Numéro de séquence de mise à jour:<br /><br />*<number>*<br /><br />L’attribut invocationID est modifié quand un serveur d’annuaire est restauré à partir du média de sauvegarde, est configuré pour héberger une partition d’annuaire d’application accessible en écriture, a été redémarré après une capture instantanée d’ordinateur virtuel a été appliquée, après une opération d’importation de machine virtuelle, ou après une opération de migration dynamique. Contrôleurs de domaine virtualisés ne doivent pas être restaurés à l’aide de captures instantanées d’ordinateur virtuel. La méthode prise en charge pour restaurer le contenu d’une base de données des Services de domaine Active Directory consiste à restaurer une sauvegarde de l’état système effectuée avec une application de sauvegarde prenant en charge les Services domaine Active Directory.»|  
|**2179**|ActiveDirectory_DomainService|L’attribut msDS-GenerationId de l’objet ordinateur du contrôleur de domaine a été défini pour le paramètre suivant:<br /><br />Attribut GenerationID:<br /><br />*<number>*|  
|**2200**|ActiveDirectory_DomainService|Active Directory a détecté que l’ordinateur virtuel qui héberge le contrôleur de domaine a été ramené à un état antérieur. Les Services de domaine Active Directory initialise la réplication pour mettre à jour le contrôleur de domaine. Événement 2201 sera consigné lorsque la réplication est terminée.|  
|**2201**|ActiveDirectory_DomainService|Active Directory a détecté que l’ordinateur virtuel qui héberge le contrôleur de domaine a été ramené à un état antérieur. Les Services de domaine Active Directory a terminé la réplication pour mettre à jour le contrôleur de domaine.|  
|**2185**|ActiveDirectory_DomainService|Les Services de domaine Active Directory arrêté le service FRS ou DFSR utilisé pour répliquer le dossier SYSVOL.<br /><br />Nom du service:<br /><br />DFSR<br /><br />Active Directory a détecté que l’ordinateur virtuel qui héberge le contrôleur de domaine a été ramené à un état antérieur. Les Services de domaine Active Directory doivent initialiser une restauration ne faisant pas autorité sur le réplica SYSVOL local. Cette opération est effectuée en arrêtant le service FRS ou DFSR utilisé pour répliquer le dossier SYSVOL et en le démarrant avec les clés de Registre appropriées et les valeurs pour déclencher la restauration. L’événement 2187 sera journalisé au redémarrage du service FRS ou DFSR.»|  
|**2208**|ActiveDirectory_DomainService|Les Services de domaine Active Directory supprimé les bases de données DFSR pour initialiser un réplica SYSVOL lors d’une restauration ne faisant pas autorité.<br /><br />Active Directory a détecté que l’ordinateur virtuel qui héberge le contrôleur de domaine a été ramené à un état antérieur. Les Services de domaine Active Directory doit initialiser une restauration ne faisant pas autorité sur le réplica SYSVOL local. Pour DFSR, ceci se fait en arrêtant le service DFSR, la suppression des bases de données DFSR et redémarrer le service. Après le redémarrage, DFSR reconstruire les bases de données et démarre la synchronisation initiale.»|  
|**2187**|ActiveDirectory_DomainService|Les Services de domaine Active Directory démarré le service FRS ou DFSR utilisé pour répliquer le dossier SYSVOL.<br /><br />Nom du service:<br /><br />DFSR<br /><br />Active Directory a détecté que l’ordinateur virtuel qui héberge le contrôleur de domaine a été ramené à un état antérieur. Services de domaine Active Directory a dû initialiser une restauration ne faisant pas autorité sur le réplica SYSVOL local. Ceci a été fait en arrêtant le service FRS ou DFSR utilisé pour répliquer le dossier SYSVOL et en le démarrant avec les clés de Registre appropriées et les valeurs pour déclencher la restauration. "|  
|**1587**|ActiveDirectory_DomainService|Ce service d’annuaire a été restauré ou a été configuré pour héberger une partition d’annuaire d’application. Par conséquent, son identité de réplication a changé. Un partenaire a demandé des modifications de la réplication à l’aide de notre ancienne identité. Le numéro de séquence de démarrage a été ajusté.<br /><br />Le service d’annuaire de destination correspondant à l’objet suivant que GUID a demandé le démarrage des modifications à un USN précédant l’USN à laquelle le service d’annuaire local a été restauré à partir du média de sauvegarde.<br /><br />GUID de l’objet:<br /><br />*<GUID> (<FQDN of partner domain controller>)*<br /><br />USN au moment de la restauration:<br /><br />*<number>*<br /><br />Par conséquent, le vecteur de mise à la destination du service d’annuaire a été configuré avec les paramètres suivants.<br /><br />Base de données précédente GUID:<br /><br />*<GUID>*<br /><br />USN de l’objet précédent:<br /><br />*<number>*<br /><br />USN de la propriété précédent:<br /><br />*<number>*<br /><br />Nouvelle base de données GUID:<br /><br />*<GUID>*<br /><br />Nouvel objet USN:<br /><br />*<number>*<br /><br />Nouvelle propriété USN:<br /><br />*<number>*|  
  
##### <a name="system-event-log"></a>Journal des événements système  
Le journal des événements système note l’heure de l’ordinateur qui se produit lors de la remise d’un ordinateur virtuel en ligne et la synchronisation avec l’heure de l’hôte. Le pool RID est invalidé, et les services DFSR ou FRS sont redémarrés.  
  
||||  
|-|-|-|  
|**ID d’événement**|**Source**|**Message**|  
|**1**|Noyau-général|L’heure système est passée de *?<now>* à partir de *< heure/date de capture instantanée >*.<br /><br />Raison de la modification: Une application ou un composant système a changé l’heure.|  
|**16654**|Directory-Services-SAM|Un pool d’identificateurs de comptes (RID) a été invalidé. Cela peut se produire dans les cas attendus suivants:<br /><br />1. un contrôleur de domaine est restauré à partir de la sauvegarde.<br /><br />2. un contrôleur de domaine en cours d’exécution sur un ordinateur virtuel est restauré à partir d’une capture instantanée.<br /><br />3. un administrateur a invalidé le pool manuellement.<br /><br />https://go.microsoft.com/fwlink/?LinkId=226247 pour plus d’informations, voir.|  
|**7036**|Gestionnaire de contrôle de service|Le service de réplication DFS est entré dans l’état d’arrêt.|  
|**7036**|Gestionnaire de contrôle de service|Le service de réplication DFS est entré dans l’état en cours d’exécution.|  
  
##### <a name="application-event-log"></a>Journal des événements application  
Le journal des événements Application note de la base de données DFSR arrêter et démarrer.  
  
||||  
|-|-|-|  
|**ID d’événement**|**Source**|**Message**|  
|**103**|ESENT|DFSRs (1360) \\\.\C:\System Volume Information\DFSR\database*_<GUID>*\dfsr.db: le moteur de base de données a arrêté l’instance (0).<br /><br />Arrêt brutal: 0<br /><br />Séquence de minutage interne: [1] 0.000, [2] 0.000, [3] 0.000, [4] 0.000, [5] 0.141, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.000, [10] 0.000, [11] 0.016, [12] 0.000, [13] 0.000, [14] 0.000, [15] 0.000.|  
|**102**|ESENT|DFSRs (532) \\\.\C:\System Volume Information\DFSR\database*_<GUID>*\dfsr.db: le moteur de base de données (6.02.8189.0000) démarre une nouvelle instance (0).|  
|**105**|ESENT|DFSRs (532) \\\.\C:\System Volume Information\DFSR\database*_<GUID>*\dfsr.db: le moteur de base de données a démarré une nouvelle instance (0). (Heure = 0 seconde)<br /><br />Séquence de minutage interne: [1] 0.000, [2] 0.000, [3] 0.000, [4] 0.000, [5] 0.000, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.031, [10] 0.000, [11] 0.000.|  
|||DFSRs (532) \\\.\C:\System Volume Information\DFSR\database*_<GUID>*\dfsr.db: le moteur de base de données a créé une nouvelle base de données (1, \\\.\C:\System Volume Information\DFSR\database*_<GUID>*\dfsr.db). (Heure = 0 seconde)<br /><br />Séquence de minutage interne: [1] 0.000, [2] 0.000, [3] 0.016, [4] 0.062, [5] 0.000, [6] 0.016, [7] 0.000, [8] 0.000, [9] 0.015, [10] 0.000, [11] 0.000.|  
  
##### <a name="dfs-replication-event-log"></a>Journal des événements la réplication DFS  
Le service DFSR est arrêté et la base de données qui contient SYSVOL est supprimé, forcer une synchronisation ne faisant pas autorité entrant.  
  
||||  
|-|-|-|  
|**ID d’événement**|**Source**|**Message**|  
|**1006**|DFSR|Le service de réplication DFS est en cours d’arrêt.|  
|**1008**|DFSR|Le service de réplication DFS s’est arrêté.|  
|**1002**|DFSR|Démarre le service de réplication DFS.|  
|**1004**|DFSR|Le service de réplication DFS a démarré.|  
|**1314**|DFSR|Le service de réplication DFS a correctement configuré les fichiers journaux de débogage.<br /><br />Informations supplémentaires:<br /><br />Chemin du fichier journal de débogage: C:\Windows\debug|  
|**6102**|DFSR|Le service de réplication DFS a inscrit le fournisseur WMI.|  
|**1206**|DFSR|Le contrôleur de domaine a contacté de service de réplication DFS * <domain controller FQDN> * pour accéder aux informations de configuration.|  
|**1210**|DFSR|Le service de réplication DFS configurer correctement un écouteur RPC pour les demandes de réplication entrant.<br /><br />Informations supplémentaires:<br /><br />Port: 0|  
|**4614**|DFSR|Le service de réplication DFS a initialisé SYSVOL dans le chemin d’accès local C:\Windows\SYSVOL\domain et attend d’effectuer la réplication initiale. Le dossier répliqué demeurera dans l’état de la synchronisation initiale jusqu'à ce qu’il a été répliqué avec son partenaire. Si le serveur a été va être promu contrôleur de domaine, le contrôleur de domaine ne sera pas publier et fonctionner comme un contrôleur de domaine jusqu'à ce que ce problème est résolu. Cela peut se produire si le partenaire spécifié est également dans l’état de la synchronisation initiale, ou si des violations de partage sont rencontrées sur ce serveur ou le partenaire de synchronisation. Si cet événement s’est produite lors de la migration de SYSVOL dans le Service de réplication de fichiers (FRS) à la réplication DFS, modifications ne seront pas répliquées jusqu'à ce que ce problème est résolu. Cela peut provoquer le dossier SYSVOL sur ce serveur pourrait devenir désynchronisé avec les autres contrôleurs de domaine.<br /><br />Informations supplémentaires:<br /><br />Nom du dossier répliqué: Partage SYSVOL<br /><br />ID du dossier répliqué:*<GUID>*<br /><br />Nom du groupe de réplication: Domain System Volume.<br /><br />ID du groupe de réplication:*<GUID>*<br /><br />ID du membre:*<GUID>*<br /><br />En lecture seule: 0|  
|**4604**|DFSR|Le service de réplication DFS initialisé avec succès le dossier SYSVOL répliqué au chemin d’accès local C:\Windows\SYSVOL\domain. Ce membre a terminé la synchronisation initiale de SYSVOL avec le partenaire dc1.corp.contoso.com. Pour vérifier la présence du partage SYSVOL, ouvrez une fenêtre d’invite de commandes et tapez "net share".<br /><br />Informations supplémentaires:<br /><br />Nom du dossier répliqué: Partage SYSVOL<br /><br />ID du dossier répliqué:*<GUID>*<br /><br />Nom du groupe de réplication: Domain System Volume.<br /><br />ID du groupe de réplication:*<GUID>*<br /><br />ID du membre:*<GUID>*<br /><br />Partenaire de synchronisation:*<partner domain controller FQDN>*|  
  
#### <a name="restoring-a-domain-controller-that-replicates-sysvol-using-frs"></a>Restauration d’un contrôleur de domaine qui réplique SYSVOL via les services FRS  
Le journal des événements de réplication de fichiers est utilisé au lieu du journal des événements DFSR dans ce cas. Le journal des événements Application écrit également différents événements liés au service FRS. Dans le cas contraire, les Services d’annuaire et journal des événements système de messages sont généralement les mêmes et dans le même ordre que précédemment décrits.  
  
##### <a name="file-replication-service-event-log"></a>Fichier réplication Service journal des événements  
Le service FRS est arrêté et redémarré avec la valeur D2 BURFLAGS pour une synchronisation de SYSVOL.  
  
||||  
|-|-|-|  
|**ID d’événement**|**Source**|**Message**|  
|**13502**|NTFRS|Le Service de réplication est en cours d’arrêt.|  
|**13503**|NTFRS|Le Service de réplication de fichiers s’est arrêté.|  
|**13501**|NTFRS|Démarre le Service de réplication de fichiers|  
|**13512**|NTFRS|Le Service de réplication de fichiers a détecté un cache d’écriture disque activé sur le lecteur contenant l’annuaire c:\windows\ntfrs\jet sur l’ordinateur DC4. Le Service de réplication de fichiers ne peuvent pas récupérer lors de l’alimentation du lecteur est interrompue et mises à jour critiques sont perdues.|  
|**13565**|NTFRS|Service de réplication de fichiers initialise le volume système avec des données à partir d’un autre contrôleur de domaine. L’ordinateur DC4 ne peut pas devenir un contrôleur de domaine jusqu'à ce que ce processus est terminé. Le volume système sera ensuite partagé en tant que SYSVOL.<br /><br />Pour vérifier le partage SYSVOL, à l’invite de commandes, tapez:<br /><br />commande NET<br /><br />Lorsque le Service de réplication de fichiers termine le processus d’initialisation, le partage SYSVOL apparaîtra.<br /><br />L’initialisation du volume système peut prendre un certain temps. La durée dépend de la quantité de données dans le volume système, la disponibilité des autres contrôleurs de domaine et l’intervalle de réplication entre les contrôleurs de domaine.»|  
|**13520**|NTFRS|Le Service de réplication de fichiers a déplacé les fichiers préexistants de * <path> * à * <path> *\NtFrs_PreExisting___See_EventLog.<br /><br />Le Service de réplication de fichiers peut supprimer les fichiers dans * <path> *\NtFrs_PreExisting___See_EventLog à tout moment. Fichiers peuvent être enregistrés à partir de la suppression en les copiant hors * <path> *\NtFrs_PreExisting___See_EventLog. Copie des fichiers dans * <path> * peut provoquer des conflits de nom si les fichiers existent déjà sur d’autres partenaires de réplication.<br /><br />Dans certains cas, le Service de réplication de fichiers peut copier un fichier à partir de * <path> *\NtFrs_PreExisting___See_EventLog vers * <path> * au lieu de répliquer le fichier à partir d’autres partenaires de réplication.<br /><br />Espace disque peut être libéré à tout moment en supprimant les fichiers dans * <path> *\NtFrs_PreExisting___See_EventLog.|  
|**13553**|NTFRS|Le Service de réplication de fichiers a correctement ajouté cet ordinateur au jeu de réplica suivant:<br /><br />«DOMAIN SYSTEM VOLUME (PARTAGE SYSVOL)»<br /><br />Informations relatives à cet événement sont indiquées ci-dessous:<br /><br />Nom DNS de l’ordinateur est «*<domain controller FQDN>*»<br /><br />Nom de membre du jeu de réplica est «*<domain controller name>*»<br /><br />Le chemin d’accès de réplica set racine est «*<path>*»<br /><br />Chemin d’accès de répertoire intermédiaire le réplica est «* <path> * »<br /><br />Chemin d’accès de répertoire de travail réplica est «*<path>*»|  
|**13554**|NTFRS|Le Service de réplication de fichiers a correctement ajouté les connexions affichées ci-dessous au jeu de réplica:<br /><br />«DOMAIN SYSTEM VOLUME (PARTAGE SYSVOL)»<br /><br />Réception depuis «*<partner domain controller FQDN>*»<br /><br />Émission vers «*<partner domain controller FQDN>*»<br /><br />Informations supplémentaires peuvent apparaître dans les messages de journaux d’événements ultérieurs.|  
|**13516**|NTFRS|Le Service de réplication de fichiers n’empêche plus l’ordinateur DC4 de devenir un contrôleur de domaine. Le volume système a été correctement initialisé et le service Netlogon a été averti que le volume système est maintenant prêt à être partagé en tant que SYSVOL.<br /><br />Entrez «net share» pour vérifier le partage SYSVOL.|  
  
##### <a name="application-event-log"></a>Journal des événements application  
La base de données FRS s’arrête et démarre et sont supprimés en raison de l’opération D2 BURFLAGS.  
  
||||  
|-|-|-|  
|**ID d’événement**|**Source**|**Message**|  
|**327**|ESENT|NTFRS (1424) le moteur de base de données détaché une base de données (1, c:\windows\ntfrs\jet\ntfrs.jdb). (Heure = 0 seconde)<br /><br />Séquence de minutage interne: [1] 0.000, [2] 0.015, [3] 0.000, [4] 0.000, [5] 0.000, [6] 0.516, [7] 0.000, [8] 0.000, [9] 0.000, [10] 0.000, [11] 0.063, [12] 0.000.<br /><br />Réactivée Cache: 0|  
|**103**|ESENT|NTFRS (1424) le moteur de base de données a arrêté l’instance (0).<br /><br />Arrêt brutal: 0<br /><br />Séquence de minutage interne: [1] 0.000, [2] 0.000, [3] 0.000, [4] 0.000, [5] 0.000, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.031, [10] 0.000, [11] 0.016, [12] 0.000, [13] 0.000, [14] 0.047, [15] 0.000.|  
|**102**|ESENT|NTFRS (3000) le moteur de base de données (6.02.8189.0000) démarre une nouvelle instance (0).|  
|**105**|ESENT|NTFRS (3000) le moteur de base de données a démarré une nouvelle instance (0). (Heure = 0 seconde)<br /><br />Séquence de minutage interne: [1] 0.000, [2] 0.000, [3] 0.000, [4] 0.000, [5] 0.000, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.062, [10] 0.000, [11] 0.141.|  
|**103**|ESENT|NTFRS (3000) le moteur de base de données a arrêté l’instance (0).<br /><br />Arrêt brutal: 0<br /><br />Séquence de minutage interne: [1] 0.000, [2] 0.000, [3] 0.000, [4] 0.000, [5] 0.000, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.000, [10] 0.000, [11] 0.000, [12] 0.000, [13] 0.015, [14] 0.000, [15] 0.000.|  
|**102**|ESENT|NTFRS (3000) le moteur de base de données (6.02.8189.0000) démarre une nouvelle instance (0).|  
|**105**|ESENT|NTFRS (3000) le moteur de base de données a démarré une nouvelle instance (0). (Heure = 0 seconde)<br /><br />Séquence de minutage interne: [1] 0.000, [2] 0.000, [3] 0.000, [4] 0.000, [5] 0.000, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.078, [10] 0.000, [11] 0.109.|  
|**325**|ESENT|NTFRS (3000) le moteur de base de données créé une nouvelle base de données (1, c:\windows\ntfrs\jet\ntfrs.jdb). (Heure = 0 seconde)<br /><br />Séquence de minutage interne: [1] 0.000, [2] 0.000, [3] 0.016, [4] 0.016, [5] 0.000, [6] 0.015, [7] 0.000, [8] 0.000, [9] 0.078, [10] 0.016, [11] 0.000.|  
|**103**|ESENT|NTFRS (3000) le moteur de base de données a arrêté l’instance (0).<br /><br />Arrêt brutal: 0<br /><br />Séquence de minutage interne: [1] 0.000, [2] 0.000, [3] 0.000, [4] 0.000, [5] 0.078, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.125, [10] 0.016, [11] 0.000, [12] 0.000, [13] 0.000, [14] 0.000, [15] 0.000.|  
|**102**|ESENT|NTFRS (3000) le moteur de base de données (6.02.8189.0000) démarre une nouvelle instance (0).|  
|**105**|ESENT|NTFRS (3000) le moteur de base de données a démarré une nouvelle instance (0). (Heure = 0 seconde)<br /><br />Séquence de minutage interne: [1] 0.016, [2] 0.000, [3] 0.000, [4] 0.094, [5] 0.000, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.032, [10] 0.000, [11] 0.000.|  
|**326**|ESENT|NTFRS (3000) le moteur de base de données attaché une base de données (1, c:\windows\ntfrs\jet\ntfrs.jdb). (Heure = 0 seconde)<br /><br />Séquence de minutage interne: [1] 0.000, [2] 0.015, [3] 0.000, [4] 0.000, [5] 0.016, [6] 0.015, [7] 0.000, [8] 0.000, [9] 0.000, [10] 0.000, [11] 0.000, [12] 0.000.<br /><br />Enregistré Cache: 1|  
  


