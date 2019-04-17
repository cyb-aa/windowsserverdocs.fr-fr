---
ms.assetid: 5ab76733-804d-4f30-bee6-cb672ad5075a
title: "Résolution des problèmes de déploiement de contrôleur de domaine"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 254209b0da6a7bc0cd5f3880e14a7e5b16822cdd
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="troubleshooting-domain-controller-deployment"></a>Résolution des problèmes de déploiement de contrôleur de domaine

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Cette rubrique présente une méthodologie détaillée sur la résolution des problèmes de déploiement et configuration du contrôleur de domaine.  
  
-   [Introduction à la résolution des problèmes](../../ad-ds/deploy/Troubleshooting-Domain-Controller-Deployment.md#BKMK_Intro)  
  
-   [Options de dépannage](../../ad-ds/deploy/Troubleshooting-Domain-Controller-Deployment.md#BKMK_Options)  
  
## <a name="BKMK_Intro"></a>Introduction à la résolution des problèmes  
![Résolution des problèmes](media/Troubleshooting-Domain-Controller-Deployment/adds_deploy_troubleshooting.png)  
  
## <a name="BKMK_Options"></a>Options de dépannage  
  
### <a name="logging-options"></a>Options de journalisation  
Les journaux intégrés sont le meilleur moyen de résolution des problèmes de promotion du contrôleur de domaine et la rétrogradation. Tous ces journaux sont activés et configurés par défaut pour un maximum de commentaires.  
  
|Phase|Journal|  
|---------|-------|  
|Gestionnaire de serveur ou WindowsPowerShellADDSDeployment opérations|-%systemroot%\debug\dcpromoui.log<br /><br />-%systemroot%\debug\dcpromoui*.log|  
|Installation/Promotion du contrôleur de domaine|-%systemroot%\debug\dcpromo.log<br /><br />-%systemroot%\debug\dcpromo*.log<br /><br />-Observateur d’événements\journaux Windows\Système<br /><br />-Observateur d’événements\journaux Windows\Application<br /><br />-Event événements\journaux des applications et des services\Service<br /><br />-Event événements\journaux des applications et services services\Service de réplication<br /><br />-Event événements\journaux des applications et des services\réplication DFS|  
|Mise à niveau de forêt ou du domaine|-%systemroot%\debug\adprep\\<datetime>\adprep.log<br /><br />-%systemroot%\debug\adprep\\<datetime>\csv.log<br /><br />-%systemroot%\debug\adprep\\<datetime>\dspecup.log<br /><br />-%systemroot%\debug\adprep\\<datetime>\ldif.log*|  
|Moteur de déploiement de WindowsPowerShellADDSDeployment du Gestionnaire de serveur|-Event événements\journaux des applications et des services\microsoft\windows\directoryservices-deployment\opérationnel|  
|Services de maintenance Windows|-%systemroot%\Logs\CBS\\*<br /><br />-%systemroot%\servicing\sessions\sessions.xml<br /><br />-%systemroot%\winsxs\poqexec.log<br /><br />-%systemroot%\winsxs\pending.xml|  
  
### <a name="tools-and-commands-for-troubleshooting-domain-controller-configuration"></a>Outils et commandes pour la résolution des problèmes de Configuration du contrôleur de domaine  
Pour résoudre les problèmes non décrits par les journaux, utilisez les outils suivants comme point de départ:  
  
-   Dcdiag.exe  
  
-   Repadmin.exe  
  
-   [AutoRuns.exe](https://technet.microsoft.com/sysinternals/bb963902.aspx), Gestionnaire des tâches et MSInfo32.exe  
  
-   [Network Monitor 3.4](https://www.microsoft.com/download/en/details.aspx?displaylang=en&id=4865) (ou un outil de capture et d’analyse de réseau tiers)  
  
### <a name="general-methodology-for-troubleshooting-domain-controller-configuration"></a>Méthodologie générale pour la résolution des problèmes de Configuration du contrôleur de domaine  
  
1.  Un simple problème de syntaxe provoque l’erreur?  
  
    1.  Avez-vous une faute de frappe ou oublié de fournir un argument à WindowsPowerShellADDSDeployment? Par exemple, si vous utilisez WindowsPowerShellADDSDeployment, avez-vous oublié d’ajouter l’argument requis **- domainname** avec un nom valide?  
  
    2.  Examinez la sortie de console Windows PowerShell avec soin pour voir exactement pourquoi il ne parvient pas à analyser la ligne de commande fournie.  
  
2.  L’erreur n’est un échec de la configuration requise?  
  
    1.  De nombreuses erreurs qui permettant de s’affichent en tant que résultats de promotion irrécupérables sont maintenant évitées avec l’outil de vérification de la configuration requise.  
  
    2.  Examinez le texte des erreurs de configuration requise attentivement, ils fournissent les indications nécessaires pour résoudre la plupart des problèmes, qu’ils sont de scénarios contrôlés.  
  
3.  Est l’erreur pendant la promotion et par conséquent irrécupérable?  
  
    1.  Examinez soigneusement les résultats: de nombreuses erreurs ont des explications simples tels que les mots de passe incorrects, la résolution de noms de réseau ou les contrôleurs de domaine hors connexion critiques.  
  
    2.  Pour les erreurs affichées dans la sortie, puis en arrière à partir de pour afficher des indications de la cause du problème, examinez le Dcpromoui.log et dcpromo.log.  
  
        1.  Toujours comparer à un exemple de journal opérationnel  
  
        2.  Examinez les erreurs journaux ADPrep uniquement si les résultats indiquent un problème de l’extension du schéma ou de la préparation de la forêt ou le domaine.  
  
        3.  Examinez le journal des événements pour les erreurs DirectoryServices-Deployment uniquement si Dcpromoui.log ne dispose pas de détails ou se termine arbitrairement en raison d’une exception non gérée dans le processus de configuration.  
  
    3.  Examinez les journaux des événements Services d’annuaire, du système et Application d’autres indicateurs d’un problème de configuration. Souvent, la promotion du contrôleur de domaine est juste un symptôme d’une autre configuration réseau incorrecte qui peut affecter tous les systèmes distribués.  
  
    4.  Utilisez dcdiag.exe et repadmin.exe pour valider l’intégrité de la forêt globale et indiquer des configurations incorrectes minimes qui peuvent éviter toute autre promotion du contrôleur de domaine.  
  
    5.  Utilisez AutoRuns.exe, le Gestionnaire des tâches ou MSinfo32.exe pour examiner l’ordinateur pour un logiciel tiers qui peut-être interférer.  
  
        1.  Supprimer un logiciel tiers (ne pas simplement désactiver le logiciel; cela n’empêche pas le chargement des pilotes).  
  
    6.  Installez NetMon 3.4 sur l’ordinateur qui ne parvient pas à promouvoir ainsi le contrôleur de domaine de partenaire de réplication et d’analyser le processus de promotion avec les captures réseau recto verso.  
  
        1.  Effectuez une comparaison avec votre environnement de laboratoire opérationnel pour comprendre ce à quoi ressemble une promotion correcte et où se situe l’échec.  
  
        2.  À ce stade, les erreurs sont probablement liées aux objets de la forêt, les modifications de sécurité par défaut ou le réseau, et ce nouveau contrôleur de domaine est victime des erreurs de configuration dans DNS, pare-feu, logiciel de protection des intrusions hôtes ou d’autres facteurs externes.  
  
### <a name="troubleshooting-specific-problems"></a>Résolution des problèmes spécifiques  
  
#### <a name="events-and-error-messages"></a>Événements et Messages d’erreur  
Promotion du contrôleur de domaine et la rétrogradation toujours renvoie un code à la fin de l’opération et, contrairement à la plupart des programmes, ne retournent pas zéro pour la réussite. Pour afficher le code à la fin d’une configuration de contrôleur de domaine, vous disposez de plusieurs options:  
  
1.  Lorsque vous utilisez le Gestionnaire de serveur, examinez les résultats de la promotion dans les dix secondes qui précèdent le redémarrage automatique.  
  
2.  Lorsque vous utilisez WindowsPowerShellADDSDeployment, examinez les résultats de la promotion dans les dix secondes qui précèdent le redémarrage automatique. Sinon, choisissez de ne pas redémarrer automatiquement à la fin. Vous devez ajouter le **Format-List** pipeline pour faciliter la lecture de la sortie. Par exemple:  
  
    ```  
    Install-addsdomaincontroller <options> -norebootoncompletion:$true | format-list  
  
    ```  
  
    Erreurs de validation des conditions préalables et la vérification ne continuent pas une session sur un redémarrage, afin qu’ils soient visibles dans tous les cas. Par exemple:  
  
  ![Résolution des problèmes](media/Troubleshooting-Domain-Controller-Deployment/ADDS_PSPrereqError.png)  
  
3.  Dans tout scénario, examinez le dcpromo.log et dcpromoui.log.  
  
    > [!NOTE]  
    > Certaines des erreurs répertoriées ci-dessous ne sont plus possibles en raison des modifications de configuration de contrôleur système d’exploitation et de domaine dans les systèmes d’exploitation ultérieurs. Les nouveaux codes WindowsPowerShellADDSDeployment empêche également certaines erreurs, mais le dcpromo.exe/le /unattend pas; il s’agit d’une autre raison incontestable de transférer l’ensemble de votre automatisation actuelle de Dcpromo qui est déconseillé pour WindowsPowerShellADDSDeployment.  
  
La promotion et la rétrogradation retournent la réussite suivante codes des messages.  
  
|Code d’erreur|Explication|Remarque|  
|--------------|---------------|--------|  
|1|Sortie, réussite|Vous devez redémarrer, cela indique juste que le redémarrage automatique indicateur a été supprimé.|  
|2|Sortie, réussite, redémarrage nécessaire||  
|3|Sortie, réussite, avec une défaillance non critique|En règle générale consultés dans l’avertissement de délégation DNS. Si vous n’est ne pas configuré la délégation DNS, utilisez:<br /><br />-creatednsdelegation: $false|  
|4|Sortie, réussite, avec une défaillance non critique, redémarrage nécessaire|En règle générale consultés dans l’avertissement de délégation DNS. Si vous n’est ne pas configuré la délégation DNS, utilisez:<br /><br />-creatednsdelegation: $false|  
  
La promotion et la rétrogradation retournent l’erreur suivante codes des messages. Il est également susceptible d’être un message d’erreur étendue; toujours lire l’erreur complet avec précaution, pas seulement la partie numérique.  
  
|Code d’erreur|Explication|Solution suggérée|  
|--------------|---------------|------------------------|  
|11|Promotion du contrôleur de domaine est déjà en cours d’exécution|N’exécutez pas plusieurs instances de promotion du contrôleur de domaine en même temps pour le même ordinateur cible|  
|12|L’utilisateur doit être administrateur|Ouverture de session en tant que membre du groupe Administrateurs intégré et vérifiez que vous êtes élévation avec UAC|  
|13|Autorité de certification est installée.|Vous ne pouvez pas rétrograder ce contrôleur de domaine, car il s’agit également d’une autorité de Certification. Ne supprimez pas l’autorité de certification avant d’avoir soigneusement inventaire de son utilisation; si elle émet des certificats, la suppression du rôle provoque un arrêt. Il est déconseillé d’autorités de certification en cours d’exécution sur les contrôleurs de domaine|  
|14|En cours d’exécution en mode de démarrage sans échec|Démarrez le serveur en mode normal|  
|15|La modification du rôle est en cours ou nécessite un redémarrage|Vous devez redémarrer le serveur (en raison de modifications de configuration antérieures) avant la promotion|  
|16|En cours d’exécution sur la plateforme incorrecte|*Pas de chances que cette erreur*|  
|17|Aucun lecteur NTFS 5 n’existe|Cette erreur n’est pas possible dans Windows Server2012, qui nécessite au moins %SystemDrive% formaté en NTFS|  
|18|Espace insuffisant dans windir|Libérer de l’espace sur le volume %SystemDrive% à l’aide de cleanmgr.exe|  
|19|Nom de modification en attente, nécessite un redémarrage|Redémarrez le serveur|  
|20|Nom de l’ordinateur est la syntaxe non valide|Renommer l’ordinateur avec un nom valide|  
|21|Ce contrôleur de domaine conserve les rôles FSMO, est un catalogue global et/ou est un serveur DNS|Ajouter **- demoteoperationmasterrole** lors de l’utilisation **- forceremoval**.|  
|22|TCP/IP doit être installé ou ne fonctionne pas|Vérifiez l’ordinateur a TCP/IP configuré, lié et fonctionne correctement|  
|23|Client DNS doit être configuré en premier|Définir un serveur DNS principal lorsque vous ajoutez un nouveau contrôleur de domaine à un domaine|  
|24|Informations d’identification fournies sont les éléments requis manquants ou non valides|Vérifiez votre nom d’utilisateur et un mot de passe est correct|  
|25|Contrôleur de domaine pour le domaine spécifié est introuvable|Valider les paramètres du client DNS, les règles de pare-feu|  
|26|Liste des domaines ne parvient pas à partir de la forêt|Valider les paramètres du client DNS, les fonctionnalités LDAP, les règles de pare-feu|  
|27|Nom de domaine absent|Spécifier un domaine pendant la promotion ou la rétrogradation|  
|28|Nom de domaine incorrect|Choisissez un nom de domaine DNS différent et valide lors de la promotion|  
|29|Le domaine parent n’existe pas|Vérifiez le domaine parent spécifié lors de la création d’un nouveau domaine enfant ou domaine d’arborescence|  
|30|Domaine absent de la forêt|Vérifiez le nom de domaine fourni|  
|31|Domaine enfant existe déjà|Spécifiez un nom de domaine différent|  
|32|Nom de domaine NetBIOS incorrect|Spécifiez un nom de domaine NetBIOS valide|  
|33|Chemin d’accès aux fichiers IFM n’est pas valide|Valider votre chemin d’accès au dossier d’installation à partir du support|  
|34|La base de données IFM est incorrecte|Utilisez le correct installer à partir du support pour ce système d’exploitation et le rôle (même version de système d’exploitation, même type de contrôleur de domaine - RODC et RWDC)|  
|35|SYSKEY manquant|L’installation à partir du média est chiffrée et vous devez fournir un SYSKEY valide pour l’utiliser|  
|37|Chemin d’accès pour la base de données NTDS ou ses journaux n’est pas valide|Modifier le chemin d’accès de base de données et des journaux pour un volume NTFS fixe, et non un lecteur mappé ou un chemin d’accès UNC|  
|38|Volume ne dispose pas de suffisamment d’espace pour la base de données NTDS ou les journaux|Libérer de l’espace à l’aide de cleanmgr.exe, ajoutez davantage d’espace disque, effacez manuellement espace en déplaçant ailleurs les données inutiles|  
|39|Chemin d’accès de SYSVOL n’est pas valide|Modifier le chemin d’accès du dossier SYSVOL sur un volume NTFS fixe, et non un lecteur mappé ou un chemin d’accès UNC|  
|40|Nom de site non valide|Fournir un nom de site existant|  
|41|Est nécessaire de spécifier un mot de passe pour le mode sans échec|Fournir un mot de passe pour le compte DSRM, il ne peut pas être vide, quel que soit la configuration de la stratégie de mot de passe|  
|42|Mot de passe en mode sans échec ne répond pas aux critères (promotion uniquement)|Fournir un mot de passe pour le compte DSRM qui répond aux règles configurées de la stratégie de mot de passe|  
|43|Mot de passe administrateur ne répond pas aux critères (rétrogradation uniquement)|Fournir un mot de passe pour le compte administrateur local qui répond à la stratégie de mot de passe configuré les règles|  
|44|Le nom spécifié pour la forêt n’est pas valide|Spécifiez un nom de domaine DNS racine forêt valide|  
|45|Une forêt avec le nom spécifié existe déjà|Choisissez un nom de domaine DNS racine autre forêt|  
|46|Le nom de l’arborescence spécifié n’est pas valide|Spécifiez un nom de domaine DNS valide|  
|47|Une arborescence avec le nom spécifié existe déjà|Choisissez un nom de domaine DNS autre arborescence|  
|48|Le nom d’arborescence ne tient pas dans la structure de forêt|Choisissez un nom de domaine DNS autre arborescence|  
|49|Le domaine spécifié n’existe pas|Vérifiez votre nom de domaine typé|  
|50|Pendant la rétrogradation, le dernier contrôleur de domaine a été détecté même si elle n’est pas, ou dernier contrôleur de domaine a été spécifié, mais il n’est pas|Ne spécifiez pas **dernier contrôleur de domaine dans le domaine** (**- lastdomaincontrollerindomain**), sauf si elle a la valeur true. Utilisez **- ignorelastdcindomainmismatch** de remplacement si c’est vraiment le dernier contrôleur de domaine et il existe des métadonnées du contrôleur de domaine fantômes|  
|51|Partitions d’application existent sur ce contrôleur de domaine|Spécifiez le **supprimer les Partitions d’Application** (**- removeapplicationpartitions**)|  
|52|Obligatoire argument de ligne de commande est manquant (autrement dit, un fichier de réponses doit être spécifié sur la ligne de commande)|*Vu uniquement avec dcpromo//unattend, qui est déconseillée. Consultez la documentation plus ancienne*|  
|53|La promotion/rétrogradation a échoué, l’ordinateur doit être redémarré pour nettoyage|Examinez l’erreur étendue et les journaux|  
|54|La promotion/rétrogradation a échoué|Examinez l’erreur étendue et les journaux|  
|55|La promotion/rétrogradation a été annulée par l’utilisateur|Examinez l’erreur étendue et les journaux|  
|56|La promotion/rétrogradation a été annulée par l’utilisateur, l’ordinateur doit être redémarré pour nettoyage|Examinez l’erreur étendue et les journaux|  
|58|Un nom de site doit être spécifié lors de la promotion RODC|Vous devez spécifier un site pour un RODC, il ne sera pas détecter automatiquement comme un RWDC|  
|59|Pendant la rétrogradation, ce contrôleur de domaine est le dernier serveur DNS pour l’une de ses zones|Spécifier qu’il s’agit du **dernier serveur DNS dans le domaine** ou utilisez **- ignorelastdnsserverfordomain**|  
|60|Un contrôleur de domaine exécutant Windows Server2008 ou version ultérieure doit être présent dans le domaine pour promouvoir le RODC|Promouvez au moins Windows Server2008 ou ultérieure modèle de contrôleur de domaine accessible en écriture|  
|61|Vous ne pouvez pas installer les Services de domaine ActiveDirectory avec DNS dans un domaine existant qui n’héberge pas déjà DNS|*Pas possible d’obtenir cette erreur*|  
|62|Fichier de réponses ne dispose pas d’une section [DCInstall]|*Vu uniquement avec dcpromo//unattend, qui est déconseillée. Consultez la documentation plus ancienne.*|  
|63|Niveau fonctionnel de forêt est sous windows server 2003|Augmenter le niveau fonctionnel de forêt au moins Windows Server2003 natif. Windows2000 et WindowsNT4.0 ne sont plus des systèmes d’exploitation pris en charge|  
|64|La promotion a échoué en raison de l’échec de la détection binaire des composants|Installer le rôle ADDS|  
|65|La promotion a échoué en raison de l’échec d’installation binaire des composants|Installer le rôle ADDS|  
|66|La promotion a échoué en raison de l’échec de la détection du système d’exploitation|Examinez l’erreur étendue et les journaux; le serveur ne peut pas renvoyer sa version de système d’exploitation. Il est probable que l’ordinateur devra être réinstallé, car son intégrité globale est hautement suspecte|  
|68|Partenaire de réplication n’est pas valide|Utilisez repadmin.exe ou **Get-ADReplication\ *** Windows PowerShell pour valider l’intégrité du contrôleur de domaine partenaire|  
|69|Le Port requis est déjà en cours d’utilisation par une autre application|Utilisez **netstat.exe - anob** pour localiser les processus qui sont incorrectement affectés à des ports ADDS réservés|  
|70|Le contrôleur de domaine de racine de forêt doit être un catalogue global|*Vu uniquement avec dcpromo//unattend, qui est déconseillée. Consultez la documentation plus ancienne*|  
|71|Serveur DNS est déjà installé.|Ne spécifiez pas pour installer le système DNS (**- installDNS**) si le service DNS est déjà installé.|  
|72|Ordinateur exécutant les Services Bureau à distance en mode non-administrateur|Vous ne pouvez pas promouvoir ce contrôleur de domaine, car il s’agit également d’un serveur de services Bureau à distance configuré pour plus de deux utilisateurs administrateurs. Ne supprimez pas les services Bureau à distance avant d’avoir soigneusement inventaire de son utilisation; si elle est utilisée par les applications ou les utilisateurs finaux, la suppression provoque un arrêt|  
|73|Le niveau fonctionnel de forêt spécifié n’est pas valide.|Indiquez un niveau fonctionnel de forêt valide|  
|74|Le niveau fonctionnel du domaine spécifié n’est pas valide.|Indiquez un niveau fonctionnel de domaine valide|  
|75|Impossible de déterminer la stratégie de réplication de mot de passe par défaut.|Valider que la stratégie de réplication de mot de passe RODC existe et est accessible|  
|76|Groupes de sécurité répliqués/non répliqués spécifiés ne sont pas valides|Valider que vous avez tapé des comptes de domaine et d’utilisateur valides lors de la spécification d’une stratégie de réplication de mot de passe|  
|77|L’argument spécifié n’est pas valide|Examinez l’erreur étendue et les journaux|  
|78|N’a pas pu examiner la forêt ActiveDirectory|Examinez l’erreur étendue et les journaux|  
|79|RODC ne peut pas être promu, car rodcprep n’a pas été effectuée.|Utilisez Windows Server2012 pour préparer la forêt ou utilisez **adprep.exe /rodcprep**|  
|80|DomainPrep n’a pas été effectuée.|Utilisez Windows Server2012 pour préparer le domaine ou utilisez **adprep.exe /domainprep**|  
|81|ForestPrep n’a pas été effectuée.|Utilisez Windows Server2012 pour préparer la forêt ou utilisez **adprep.exe /forestprep**|  
|82|Incompatibilité du schéma de forêt|Utilisez Windows Server2012 pour préparer la forêt ou utilisez **adprep.exe /forestprep**|  
|83|Référence (SKU) non pris en charge|*Pas de chances que cette erreur*|  
|84|Impossible de détecter un compte de contrôleur de domaine|Valider que contrôleurs de domaine existants ont l’attribut de contrôle de compte d’utilisateur correct défini.|  
|85|Impossible de sélectionner un compte de contrôleur de domaine pour la phase 2|Retourné si vous indiquez «Utiliser le compte existant» mais qu’aucun compte n’est trouvé ou qu’il existe une erreur lors de la recherche de compte. Assurez-vous que vous avez fourni le compte RODC intermédiaire correct|  
|86|Besoin d’exécuter la promotion de phase 2|Retourné si vous promouvez un contrôleur de domaine supplémentaire, mais il existe un compte existant et «Autoriser la réinstallation» n’a été spécifiée|  
|87|Il existe un compte de contrôleur de domaine de type incompatible|Renommez l’ordinateur avant la promotion, si vous n’essayez pas joindre à un contrôleur de domaine inoccupé. Vous devez joindre pour le compte de contrôleur de domaine inoccupé à l’aide **- useexistingaccount** et l’argument accessible en écriture ou en lecture seule correct, en fonction du type de compte|  
|88|L’administrateur du serveur spécifié n’est pas valide|Vous avez spécifié un compte non valide pour la délégation de l’administration RODC. Vérifiez que le compte spécifié est un utilisateur valide ou un groupe|  
|89|Le maître RID du domaine spécifié est hors connexion.|Utilisez **netdom.exe query fsmo** pour détecter le maître RID. Mettre en ligne et rendez-le accessible au contrôleur de domaine que vous promouvez|  
|90|Maître d’attribution de noms de domaine est hors connexion.|Utilisez **netdom.exe query fsmo** pour détecter le maître d’attribution de noms de domaine. Mettre en ligne et rendez-le accessible au contrôleur de domaine que vous promouvez|  
|91|Impossible de détecter si le processus est wow64|*Pas possible d’obtenir cette erreur, car le système d’exploitation est 64bits*|  
|92|Le processus WOW64 n’est pas pris en charge.|*Pas possible d’obtenir cette erreur, car le système d’exploitation est 64bits*|  
|93|Service de contrôleur de domaine n’est pas en cours d’exécution pour la rétrogradation non forcée|Démarrer le service ADDS|  
|94|Mot de passe administrateur local ne répond pas à la configuration requise: vide ou non requis|Fournir un mot de passe non vide et assurez-vous que la stratégie de mot de passe local nécessite un mot de passe|  
|95|Impossible de rétrograder le dernier Windows Server2008 ou ultérieure contrôleur de domaine dans le domaine où RODC dynamiques existent|Vous devez d’abord rétrograder tous les RODC avant de pouvoir rétrograder tous les Windows Server2008 ou version ultérieure contrôleurs de domaine accessible en écriture|  
|96|Impossible de désinstaller les fichiers binaires du service d’annuaire|*Vu uniquement avec dcpromo//unattend, qui est déconseillée. Consultez la documentation plus ancienne*|  
|97|Forêt version du niveau fonctionnel est supérieure à celle du système d’exploitation domaine enfant|Fournir un domaine enfant fonctionnel même ou supérieur le niveau fonctionnel de forêt|  
|98|Binaire installation/désinstallation des composants est en cours.|*Vu uniquement avec dcpromo//unattend, qui est déconseillée. Consultez la documentation plus ancienne*|  
|99|Niveau fonctionnel de forêt est trop faible (erreur ne concerne Windows Server2012)|Augmenter le niveau fonctionnel de forêt au moins natif de Windows Server2003. Windows2000 et WindowsNT4.0 ne sont plus des systèmes d’exploitation pris en charge|  
|100|Niveau fonctionnel du domaine est trop faible (erreur ne concerne Windows Server2012)|Augmenter le niveau fonctionnel du domaine au moins natif de Windows Server2003. Windows2000 et WindowsNT4.0 ne sont plus des systèmes d’exploitation pris en charge|  
  
#### <a name="knownlikely-issues-and-support-scenarios"></a>Problèmes connus/probables et scénarios de prise en charge  
Voici les problèmes courants rencontrés pendant le processus de développement de Windows Server 2012. Tous ces problèmes sont «normaux» et ont une solution de contournement valide ou technique plus appropriée pour les éviter en premier lieu. La plupart de ces comportements sont identiques dans Windows Server2008R2 et des systèmes d’exploitation plus anciens, mais la réécriture du déploiement des services ADDS envers des problèmes.  
  
|Problème|La rétrogradation d’un contrôleur de domaine laisse DNS s’exécuter sans zones|  
|---------|-----------------------------------------------------------------|  
|Symptômes|Serveur toujours répond aux demandes DNS mais n’a aucune information de zone|  
|Résolution et remarques|Lorsque vous supprimez le rôle ADDS, également supprimer le rôle de serveur DNS ou définissez le service serveur DNS sur désactivé. Pensez à pointez le client DNS vers un autre serveur que lui-même. Si vous utilisez Windows PowerShell, exécutez la commande suivante après avoir rétrogradé le serveur:<br /><br />Code - dns-windowsfeature désinstaller<br /><br />ou<br /><br />Code - set-service dns - starttype désactivé<br />STOP-service dns|  
  
|Problème|La promotion d’un serveur Windows Server2012dans un domaine en une partie existant ne configure pas updatetopleveldomain = 1 ni allowsinglelabeldnsdomain = 1|  
|---------|----------------------------------------------------------------------------------------------------------------------------------------------------|  
|Symptômes|Inscription d’enregistrement dynamique DNS n’a pas lieu.|  
|Résolution et remarques|Définissez ces valeurs en utilisant les stratégies de groupe Netlogon et DNS. Microsoft a commencé à bloquer la création de domaine en une partie dans Windows Server2008; vous pouvez utiliser ADMT ou l’outil de renommage de domaine pour modifier une structure de domaine DNS approuvée.|  
  
|Problème|La rétrogradation du dernier contrôleur de domaine dans un domaine échoue s’il existe des comptes seule inoccupés et précréés|  
|---------|------------------------------------------------------------------------------------------------------------|  
|Symptômes|La rétrogradation échoue avec le message:<br /><br />**Dcpromo.General.54**<br /><br />Les Services de domaine ActiveDirectory n’a pas trouvé un autre contrôleur de domaine ActiveDirectory pour transférer les données restantes dans la partition d’annuaire CN = Schema, CN = Configuration, DC = corp, DC = contoso, DC = com.<br /><br />«Le format de nom de domaine spécifié n’est pas valide.»|  
|Résolution et remarques|Supprimer tout restant créés à l’avance les comptes de contrôleur avant de rétrograder un domaine, à l’aide de **DSA.msc** ou **nettoyage des métadonnées Ntdsutil.exe**.|  
  
|Problème|Préparation de forêt et domaine automatique ne s’exécute pas GPPREP|  
|---------|---------------------------------------------------------------|  
|Symptômes|Fonctionnalité de planification interdomaine pour la stratégie de groupe, Mode de planification du jeu de stratégie résultant (RSOP), nécessite un système de fichiers mis à jour et les autorisations ActiveDirectory pour la stratégie de groupe existant. Sans Gpprep, vous ne pouvez pas utiliser la planification RSOP sur plusieurs domaines.|  
|Résolution et remarques|Exécutez **adprep.exe /gpprep** manuellement pour tous les domaines qui n’étaient pas auparavant préparés pour Windows Server2003, Windows Server2008 ou Windows Server2008R2. Les administrateurs doivent exécuter GPPrep qu’une seule fois dans l’historique d’un domaine et non avec chaque mise à niveau. Il n’est pas exécutée par adprep automatique car si vous avez déjà défini des autorisations personnalisées appropriées, tout le contenu SYSVOL nouvelle réplication sur tous les contrôleurs de domaine.|  
  
|Problème|Installation à partir du support ne parvient pas à vérifier quand elle pointe vers un chemin d’accès UNC|  
|---------|------------------------------------------------------------------|  
|Symptômes|Erreur retournée:<br /><br />Code - chemin d’accès de média n’a pas pu être valider. Exception appelant «GetDatabaseInfo» avec des arguments «2». Le dossier n’est pas valide.|  
|Résolution et remarques|Vous devez stocker les fichiers IFM sur un disque local, pas un chemin UNC distant. Ce blocage intentionnel empêche la promotion partielle du serveur en raison d’une interruption du réseau.|  
  
|Problème|Avertissement de délégation DNS apparaissent deux fois lors de la promotion du contrôleur de domaine|  
|---------|-------------------------------------------------------------------------|  
|Symptômes|Avertissement retourné *deux fois* lors de la promotion à l’aide de WindowsPowerShellADDSDeployment:<br /><br />Code - «Impossible de créer une délégation pour ce serveur DNS car la zone parente faisant autorité est introuvable ou elle n’exécute pas le DNS de Windows server. Si vous procédez à l’intégration avec une infrastructure DNS existante, vous devez créer manuellement une délégation à ce serveur DNS dans la zone parente pour garantir la résolution de noms fiable en dehors du domaine. Dans le cas contraire, aucune action n’est requise.»|  
|Résolution et remarques|Ignorer. WindowsPowerShellADDSDeployment affiche l’avertissement tout d’abord au cours de la vérification de la configuration requise, puis à nouveau lors de la configuration du contrôleur de domaine. Si vous ne souhaitez pas configurer la délégation DNS, utilisez l’argument:<br /><br />Code - - creatednsdelegation: $false<br /><br />Faire *pas* ignorer la vérification préalable afin de supprimer ce message|  
  
|Problème|Spécification des UPN ou des informations d’identification de domaine lors de la configuration retourne des erreurs trompeuses|  
|---------|--------------------------------------------------------------------------------------------|  
|Symptômes|Le Gestionnaire de serveur retourne une erreur:<br /><br />Exception appelant «DNSOption» avec des Arguments «6» - le code<br /><br />WindowsPowerShellADDSDeployment retourne une erreur:<br /><br />Le code - vérification des autorisations de l’utilisateur a échoué. Vous devez fournir le nom du domaine auquel appartient le compte d’utilisateur.|  
|Résolution et remarques|Assurez-vous que vous fournissez des informations d’identification de domaine valides sous la forme de **domaine\utilisateur**.|  
  
|Problème|La suppression du rôle DirectoryServices-DomainController à l’aide de Dism.exe aboutit au serveur non démarrable|  
|---------|---------------------------------------------------------------------------------------------------|  
|Symptômes|Si vous utilisez Dism.exe pour supprimer le rôle ADDS avant de rétrograder un contrôleur de domaine de manière fluide, le serveur n’est plus démarre normalement et affiche l’erreur:<br /><br />-Code d’état: 0 x 000000000<br />Informations: Une erreur inattendue s’est produite.|  
|Résolution et remarques|Démarrage en Mode de réparation des Services Active à l’aide *MAJ + F8*. Ajouter le rôle ADDS, puis rétrogradez le contrôleur de domaine. Vous pouvez également restaurer l’état du système à partir de la sauvegarde. N’utilisez pas Dism.exe pour la suppression de rôle ADDS; l’utilitaire ne connaît pas de contrôleurs de domaine.|  
  
|Problème|L’installation d’une nouvelle forêt échoue quand ForestMode a la valeur win2012|  
|---------|--------------------------------------------------------------------|  
|Symptômes|La promotion à l’aide de WindowsPowerShellADDSDeployment retourne une erreur:<br /><br />Code - Test.VerifyDcPromoCore.DCPromo.General.74<br /><br />Vérification des conditions préalables pour la promotion du contrôleur de domaine a échoué. Le niveau fonctionnel du domaine spécifié n’est pas valide|  
|Résolution et remarques|Ne spécifiez pas un mode fonctionnel de forêt de Win2012sans *également* spécifiant un mode fonctionnel de domaine de Win2012. Voici un exemple qui fonctionnera sans erreurs:<br /><br />Code - forestmode - Win2012 - mode_domaine Win2012]|  
  
|||  
|-|-|  
|Problème|En cliquant sur Vérifier dans l’installation à partir de la zone de sélection multimédia s’affiche pour ne rien faire|  
|Symptômes|Lorsque vous spécifiez un chemin d’accès à un dossier IFM, en cliquant sur le **vérifier** bouton jamais retourne un message ou apparaît rien à faire.|  
|Résolution et remarques|Le **vérifier** bouton retourne uniquement les erreurs s’il existe des problèmes. Dans le cas contraire, il rend le **suivant** bouton sélectionnable si vous avez indiqué un chemin d’accès IFM. Vous devez cliquer sur **vérifier** pour continuer si vous avez sélectionné IFM.|  
  
|||  
|-|-|  
|Problème|La rétrogradation avec le Gestionnaire de serveur ne fournit pas de commentaires avant la fin.|  
|Symptômes|Lorsque vous utilisez le Gestionnaire de serveur pour supprimer le rôle ADDS et rétrograder un contrôleur de domaine, il n’existe aucun commentaire indiqué jusqu'à ce que la rétrogradation ait terminé ou échoué.|  
|Résolution et remarques|Il s’agit d’une limitation du Gestionnaire de serveur. Pour les commentaires, utilisez l’applet de commande WindowsPowerShellADDSDeployment:<br /><br />Code - Uninstall-addsdomaincontroller|  
  
|||  
|-|-|  
|Problème|Installation à partir du média vérifier ne détecte pas ce média RODC fourni pour le contrôleur de domaine accessible en écriture, ou vice versa.|  
|Symptômes|Lors de la promotion d’un nouveau contrôleur de domaine à l’aide d’IFM et en fournissant un support incorrect à IFM - par exemple, media RODC pour un contrôleur de domaine accessible en écriture, ou un support pour un RODC - le bouton Vérifier ne retourne pas d’une erreur. Plus tard, la promotion échoue avec l’erreur:<br /><br />Code - une erreur s’est produite en essayant de configurer cet ordinateur comme contrôleur de domaine. <br />La promotion Install-From-Media d’un Read-Only le contrôleur de domaine ne peut pas démarrer car la base de données source spécifiée n’est pas autorisé. Uniquement les bases de données à partir d’autres contrôleurs peuvent être utilisés pour la promotion IFM d’un RODC.|  
|Résolution et remarques|Vérifier uniquement valide l’intégrité globale d’IFM. Ne fournissez pas le type IFM incorrect à un serveur. Redémarrez le serveur avant de tenter la promotion à nouveau avec le support approprié.|  
  
|||  
|-|-|  
|Problème|La promotion d’un RODC dans un compte d’ordinateur précréé échoue|  
|Symptômes|Lorsque vous utilisez WindowsPowerShellADDSDeployment pour promouvoir un nouveau RODC avec un compte d’ordinateur intermédiaire, erreur:<br /><br />Code - jeu de paramètres ne peuvent pas être résolu à l’aide de paramètres nommés spécifié.    <br />InvalidArgument: ParameterBindingException<br />    + FullyQualifiedErrorId: AmbiguousParameterSet, Microsoft.DirectoryServices.Deployment.PowerShell.Commands.Install|  
|Résolution et remarques|Ne fournissez pas de paramètres déjà définis sur un compte RODC précréé. Ceux-ci sont les suivantes:<br /><br />Code - - readonlyreplica<br />-installdns<br />-donotconfigureglobalcatalog<br />-sitename<br />-installdns|  
  
|||  
|-|-|  
|Problème|Désélectionner «Redémarrer automatiquement chaque serveur de destination si nécessaire» n’a aucun effet|  
|Symptômes|Si sélectionner des (ou ne pas) l’option Gestionnaire de serveur **redémarrer automatiquement chaque serveur de destination si nécessaire** whendemoting un contrôleur de domaine par le biais de suppression du rôle, le serveur redémarre toujours, quel que soit le choix.|  
|Résolution et remarques|Ceci est intentionnel. Le processus de rétrogradation redémarre le serveur, quelle que soit ce paramètre.|  
  
|||  
|-|-|  
|Problème|Dcpromo.log affiche «[erreur] définition de la sécurité sur les fichiers de serveur n’a pas pu avec 2»|  
|Symptômes|Rétrogradation d’un contrôleur de domaine est terminée sans problème, mais l’examen du journal dcpromo indique l’erreur:<br /><br />Le code - [erreur] définition de la sécurité sur les fichiers du serveur a échoué avec 2|  
|Résolution et remarques|Ignorer, l’erreur est prévue et cosmétique.|  
  
|||  
|-|-|  
|Problème|La vérification Adprep échoue avec l’erreur «Impossible d’effectuer le contrôle de conflit de schéma Exchange»|  
|Symptômes|Lorsque vous tentez de promouvoir un contrôleur de domaine Windows Server2012 en forêt Windows Server2008R2, Windows Server2008 ou Windows Server2003 existant, la vérification échoue avec l’erreur:<br /><br />Le code - vérification des conditions préalables pour la préparation d’ActiveDirectory a échoué. Impossible d’effectuer le contrôle de conflit de schéma Exchange pour le domaine *<domain name>* (Exception: le serveur RPC n’est pas disponible)<br /><br />adprep.log affiche l’erreur:<br /><br />Code - Adprep Impossible de récupérer les données du serveur*<domain controller>*<br /><br />Par le biais de WindowsManagementInstrumentation (WMI).|  
|Résolution et remarques|Le nouveau contrôleur de domaine ne peut pas accéder à WMI via des protocoles DCOM/RPC sur les contrôleurs de domaine existant. La date de fin, ont été trois causes possibles:<br /><br />-Un pare-feu règle bloque l’accès aux contrôleurs de domaine existants<br /><br />-Le compte SERVICE réseau est manquant dans le «d’ouverture de session en tant que service» (SeServiceLogonRight) des privilèges sur les contrôleurs de domaine existants<br /><br />-NTLM est désactivé sur les contrôleurs de domaine, à l’aide de stratégies de sécurité décrites dans [présentation de la Restriction de l’authentification NTLM](https://technet.microsoft.com/library/dd560653(WS.10).aspx)|  
  
|||  
|-|-|  
|Problème|Création d’une nouvelle forêt ADDS toujours affiche l’avertissement DNS|  
|Symptômes|Lorsque la création d’une nouvelle forêt ADDS et la création de la zone DNS sur le nouveau contrôleur de domaine pour lui-même, vous toujours message d’avertissement:<br /><br />Code - une erreur a été détectée dans la configuration DNS. <br />Aucun des serveurs DNS utilisés par cet ordinateur a répondu dans l’intervalle de délai d’expiration.<br />(code d’erreur 0x000005B4 «ERROR_TIMEOUT»)|  
|Résolution et remarques|Ignorer. Cet avertissement est intentionnel sur le premier contrôleur de domaine dans le domaine racine d’une nouvelle forêt, au cas où vous envisageriez de pointer vers un serveur DNS existant et le fuseau horaire.|  
  
|||  
|-|-|  
|Problème|Windows PowerShell - whatif argument renvoie des informations de serveur DNS incorrectes|  
|Symptômes|Si vous utilisez le **- whatif** argument lors de la configuration d’un contrôleur de domaine avec implicite ou explicite **- installdns: $true**, la sortie produite affiche:<br /><br />Code - «serveur DNS: non»|  
|Résolution et remarques|Ignorer. DNS est installé et configuré correctement.|  
  
|||  
|-|-|  
|Problème|Après la promotion, ouverture de session échoue avec «espace insuffisant est disponible pour traiter cette commande»|  
|Symptômes|Après avoir promouvoir un contrôleur de domaine et fermer la session et tentent de se connecter manière interactive, erreur:<br /><br />Code - espace insuffisant est disponible pour traiter cette commande|  
|Résolution et remarques|Le contrôleur de domaine n’a pas été redémarré après la promotion, en raison d’une erreur ou parce que vous avez spécifié l’argument WindowsPowerShellADDSDeployment **- norebootoncompletion **. Redémarrez le contrôleur de domaine.|  
  
|||  
|-|-|  
|Problème|Le bouton suivant n’est pas disponible sur la page Options du contrôleur de domaine|  
|Symptômes|Même si vous avez défini un mot de passe, le **suivant** bouton sur le **Options du contrôleur de domaine** page dans le Gestionnaire de serveur n’est pas disponible. Il n’existe aucun site répertorié dans le **nom du Site** menu.|  
|Résolution et remarques|Vous disposez de plusieurs sites ADDS et au moins un manque de sous-réseaux; ce futur contrôleur de domaine appartient à l’un de ces sous-réseaux. Vous devez sélectionner manuellement le sous-réseau dans le menu déroulant du nom du Site. Vous devez également passer en revue tous les sites ActiveDirectory à l’aide de DSSITE.MSC ou utilisez la commande Windows PowerShell suivante pour rechercher tous les sous-réseaux manquants des sites:<br /><br />Code - get-adreplicationsite-filtre *-sous-réseaux propriété et #124; wHERE-object {! $_.subnets - eq «\ *»} et #124; nom de format-table|  
  
|||  
|-|-|  
|Problème|La promotion ou la rétrogradation échoue avec le message «le service ne peut pas être démarré»|  
|Symptômes|Si vous essayez de promotion, une rétrogradation ou le clonage de contrôleur de domaine, vous recevez erreur:<br /><br />Code - Impossible de démarrer le service, car il est désactivé ou qu’il n’a aucun périphérique activé associé» (0 x 80070422)<br /><br />Cette erreur peut être interactive, un événement, ou écrite dans un journal tel que dcpromoui.log ou dcpromo.log|  
|Résolution et remarques|Le service de serveur de rôles DS (DsRoleSvc) est désactivé. Par défaut, ce service est installé pendant l’installation du rôle ADDS et définir un type de démarrage manuel. Ne désactivez pas ce service. Valeur manuel et autoriser les opérations de rôles DS démarrer et arrêter à la demande. Ce comportement est normal.|  
  
|||  
|-|-|  
|Problème|Le Gestionnaire de serveur vous avertit quand même que vous devez promouvoir le contrôleur de domaine|  
|Symptômes|Si vous promouvez un contrôleur de domaine à l’aide de la déconseillées dcpromo.exe/le /unattend ou mettre à niveau un contrôleur de domaine Windows Server2008R2 existant en place vers Windows Server2012, le Gestionnaire de serveur affiche toujours la tâche de configuration de post-déploiement **promouvoir ce serveur en contrôleur de domaine **.|  
|Résolution et remarques|Cliquez sur le lien d’avertissement de post-déploiement et le message disparaît définitivement. Ce comportement est cosmétique et prévu.|  
  
|||  
|-|-|  
|Problème|Script de déploiement du Gestionnaire de serveur manquant d’installation du rôle|  
|Symptômes|Si vous promouvez un contrôleur de domaine à l’aide du Gestionnaire de serveur et enregistrez le script de déploiement de Windows PowerShell, il n’inclut pas de l’applet de commande installation du rôle et les arguments (install-windowsfeature-nom ad-domain-services - includemanagementtools). Sans le rôle, le contrôleur de domaine ne peut pas être configuré.|  
|Résolution et remarques|Ajoutez manuellement cette applet de commande et les arguments à tous les scripts. Ce comportement est prévu et normal.|  
  
|||  
|-|-|  
|Problème|Script de déploiement du Gestionnaire de serveur s’appelle pas PS1|  
|Symptômes|Si vous promouvez un contrôleur de domaine à l’aide du Gestionnaire de serveur et enregistrez le script de déploiement de Windows PowerShell, le fichier est nommé avec un nom temporaire aléatoire et non comme un fichier PS1.|  
|Résolution et remarques|Renommez manuellement le fichier. Ce comportement est prévu et normal.|  
  
|Problème|Dcpromo//unattend autorise des niveaux fonctionnels non pris en charge|  
|-|-|  
|Symptômes|Si vous promouvez un contrôleur de domaine à l’aide de dcpromo//unattend avec l’exemple de fichier de réponses suivant:<br /><br />Code:<br /><br />[DCInstall]<br />NewDomain = forêt<br /><br />ReplicaOrNewDomain = domaine<br /><br />NewDomainDNSName = corp.contoso.com<br /><br />SafeModeAdminPassword =Safepassword@6<br /><br />DomainNetbiosName = corp<br /><br />DNSOnNetwork = Oui<br /><br />AutoConfigDNS = Oui<br /><br />RebootOnSuccess = NoAndNoPromptEither<br /><br />RebootOnCompletion = non<br /><br />*DomainLevel = 0*<br /><br />*ForestLevel = 0*<br /><br />La promotion échoue avec les erreurs suivantes dans dcpromoui.log:<br /><br />Code - dcpromoui 0089 EA4.5B8 13:31:50.783 entrée CArgumentsSpec::ValidateArgument DomainLevel<br /><br />Dcpromoui EA4.5B8 008A 13:31:50.783 valeur pour DomainLevel est 0<br /><br />Dcpromoui EA4.5B8 008 13:31:50.783 code de sortie est 77<br /><br />Dcpromoui EA4.5B8 008C 13:31:50.783 l’argument spécifié n’est pas valide.<br /><br />Journal de fermeture de 13:31:50.783 D 008 dcpromoui EA4.5B8<br /><br />Dcpromoui0032 EA4.5B8 13:31:50.830 code de sortie est 77<br /><br />Niveau 0 est Windows2000, qui n’est pas pris en charge dans Windows Server2012.|  
|Résolution et remarques|Ne pas utiliser le dcpromo déconseillées/le /unattend et comprendre qu’elle vous permet de spécifier des paramètres non valides qui échouent par la suite. Ce comportement est prévu et normal.|  

|Problème|La promotion «se bloque» pendant la création d’objet Paramètres NTDS et n’est jamais achevée|  
|-|-|  
|Symptômes|Si vous promouvez un réplica du contrôleur de domaine ou les RODC, la promotion atteint «créer l’objet Paramètres NTDS» et s’achève jamais ou se termine. Les journaux arrêtent également la mise à jour.|  
|Résolution et remarques|Il s’agit d’un problème connu qui survient en fournissant des informations d’identification du compte administrateur local intégré avec un mot de passe correspondant au compte d’administrateur de domaine intégré. Cela entraîne une défaillance du moteur de base d’installation qui ne prend pas d’erreur, mais attend indéfiniment (quasi-boucle). Ce scénario est prévu - quoique indésirables - comportement.<br /><br />Pour réparer le serveur:<br /><br />1. Redémarrez-le.<br /><br />1. Dans ActiveDirectory, supprimez le compte d’ordinateur de ce serveur membre (encore ne sera pas un compte de contrôleur de domaine)<br /><br />1. Sur ce serveur, retirez-le de force du domaine<br /><br />1. Sur ce serveur, supprimez le rôle ADDS.<br /><br />1. Redémarrage<br /><br />1. Ajoutez de nouveau la promotion de rôle et tentez de nouveau les services ADDS, garantissant que vous fournissez toujours le ***domaine\administrateur*** mis en forme les informations d’identification à la promotion du contrôleur de domaine et pas seulement le compte administrateur local intégré|  
