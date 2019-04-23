---
ms.assetid: 5ab76733-804d-4f30-bee6-cb672ad5075a
title: Résolution des problèmes de déploiement de contrôleur de domaine
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 54ae67c520f2874199982ca790ced5db346f0c16
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877570"
---
# <a name="troubleshooting-domain-controller-deployment"></a>Résolution des problèmes de déploiement de contrôleur de domaine

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cette rubrique présente une méthodologie détaillée sur la résolution des problèmes de configuration et de déploiement des contrôleurs de domaine.  
  
-   [Présentation du dépannage](../../ad-ds/deploy/Troubleshooting-Domain-Controller-Deployment.md#BKMK_Intro)  
  
-   [Options de dépannage](../../ad-ds/deploy/Troubleshooting-Domain-Controller-Deployment.md#BKMK_Options)  
  
## <a name="BKMK_Intro"></a>Présentation du dépannage  
![Résolution des problèmes](media/Troubleshooting-Domain-Controller-Deployment/adds_deploy_troubleshooting.png)  
  
## <a name="BKMK_Options"></a>Options de dépannage  
  
### <a name="logging-options"></a>Options de journalisation  
Les journaux intégrés représentent le meilleur moyen de résoudre les problèmes liés à la promotion et à la rétrogradation des contrôleurs de domaine. Tous ces journaux sont activés et configurés pour un maximum de commentaires par défaut.  
  
|Phase|Journal|  
|---------|-------|  
|Opérations Windows PowerShell ADDSDeployment ou du Gestionnaire de serveur|-   %systemroot%\debug\dcpromoui.log<br /><br />-   %systemroot%\debug\dcpromoui*.log|  
|Installation/Promotion du contrôleur de domaine|-   %systemroot%\debug\dcpromo.log<br /><br />-   %systemroot%\debug\dcpromo*.log<br /><br />-Observateur d’événements\journaux Windows\Système<br /><br />-Événements événements\journaux Windows\Application<br /><br />-Événements viewer\Applications et des services\Service<br /><br />-Événements viewer\Applications et services services\Service de réplication<br /><br />-Événements viewer\Applications et services des services\réplication DFS|  
|Mise à niveau de forêt ou de domaine|-   %systemroot%\debug\adprep\\<datetime>\adprep.log<br /><br />-   %systemroot%\debug\adprep\\<datetime>\csv.log<br /><br />-   %systemroot%\debug\adprep\\<datetime>\dspecup.log<br /><br />-   %systemroot%\debug\adprep\\<datetime>\ldif.log*|  
|Moteur de déploiement Windows PowerShell ADDSDeployment du Gestionnaire de serveur|-Événements viewer\Applications et services des services\microsoft\windows\directoryservices-deployment\opérationnel|  
|Services de maintenance Windows|-   %systemroot%\Logs\CBS\\*<br /><br />-   %systemroot%\servicing\sessions\sessions.xml<br /><br />-   %systemroot%\winsxs\poqexec.log<br /><br />-   %systemroot%\winsxs\pending.xml|  
  
### <a name="tools-and-commands-for-troubleshooting-domain-controller-configuration"></a>Outils et commandes pour la résolution des problèmes de configuration des contrôleurs de domaine  
Pour résoudre les problèmes non décrits par les journaux, utilisez les outils suivants comme point de départ :  
  
-   Dcdiag.exe  
  
-   Repadmin.exe  
  
-   [AutoRuns.exe](https://technet.microsoft.com/sysinternals/bb963902.aspx), Gestionnaire des tâches et MSInfo32.exe  
  
-   [Moniteur réseau 3.4](https://www.microsoft.com/download/en/details.aspx?displaylang=en&id=4865) (ou un outil de capture et d'analyse de réseau tiers)  
  
### <a name="general-methodology-for-troubleshooting-domain-controller-configuration"></a>Méthodologie générale pour la résolution des problèmes de configuration des contrôleurs de domaine  
  
1.  Est-ce qu'un simple problème de syntaxe est à l'origine de l'erreur ?  
  
    1.  Avez-vous fait une faute de frappe ou oublié de fournir un argument à Windows PowerShell ADDSDeployment ? Par exemple, si vous utilisez Windows PowerShell ADDSDeployment, avez-vous oublié d'ajouter l'argument requis **-domainname** avec un nom valide ?  
  
    2.  Examinez soigneusement la sortie de console Windows PowerShell pour identifier exactement la raison de l'échec de l'analyse de la ligne de commande fournie.  
  
2.  Est-ce que l'erreur est un échec de la configuration requise ?  
  
    1.  De nombreuses erreurs qui s'affichaient en tant que résultats de promotion irrécupérables sont maintenant évitées avec l'outil de vérification de la configuration requise.  
  
    2.  Lisez soigneusement le texte des erreurs de configuration requise, celles-ci fournissent les indications nécessaires pour résoudre la plupart des problèmes, car il s'agit de scénarios contrôlés.  
  
3.  Est-ce que l'erreur est survenue pendant la promotion et est par conséquent irrécupérable ?  
  
    1.  Examinez soigneusement les résultats : de nombreuses erreurs ont des explications simples, par exemple des mots de passe incorrects, une erreur de résolution de nom de réseau ou des contrôleurs de domaine hors connexion critiques.  
  
    2.  Recherchez dans les journaux Dcpromoui.log et dcpromo.log les erreurs affichées dans la sortie, puis revenez en arrière pour afficher des indications sur la raison de la défaillance.  
  
        1.  Effectuez toujours une comparaison avec un exemple de journal opérationnel  
  
        2.  Recherchez les erreurs dans les journaux ADPrep uniquement si les résultats indiquent un problème d'extension de schéma ou de la préparation de la forêt ou du domaine.  
  
        3.  Recherchez les erreurs dans le journal des événements DirectoryServices-Deployment uniquement si le journal Dcpromoui.log ne fournit pas assez de détails ou se termine arbitrairement en raison d'une exception non gérée dans le processus de configuration.  
  
    3.  Recherchez dans les journaux des événements système, des applications ou des services d'annuaire d'autres indicateurs d'un problème de configuration. La promotion du contrôleur de domaine est souvent juste un symptôme d'une autre configuration réseau incorrecte qui peut affecter tous les systèmes distribués.  
  
    4.  Utilisez dcdiag.exe et repadmin.exe pour valider l'intégrité de la forêt globale et indiquer des configurations incorrectes minimes qui peuvent empêcher une promotion du contrôleur de domaine supplémentaire.  
  
    5.  Utilisez AutoRuns.exe, le Gestionnaire des tâches ou MSinfo32.exe pour rechercher sur l'ordinateur des logiciels tiers qui peuvent interférer.  
  
        1.  Supprimez les logiciels tiers (il ne suffit pas de les désactiver, car cela n'empêche pas le chargement des pilotes).  
  
    6.  Installez NetMon 3.4 sur l'ordinateur sur lequel la promotion a échoué ainsi que sur le contrôleur de domaine partenaire de réplication et analysez le processus de promotion avec des captures réseau recto verso.  
  
        1.  Effectuez une comparaison avec votre environnement de laboratoire opérationnel pour comprendre à quoi ressemble une promotion correcte et où se situe l'échec.  
  
        2.  À ce stade, les erreurs sont probablement liées aux objets de la forêt, aux modifications de sécurité autres que par défaut ou au réseau, et ce nouveau contrôleur de domaine est victime des configurations incorrectes du système DNS, des pare-feu, des logiciels de protection des intrusions hôtes ou d'autres facteurs externes.  
  
### <a name="troubleshooting-specific-problems"></a>Résolution de problèmes spécifiques  
  
#### <a name="events-and-error-messages"></a>Événements et messages d'erreur  
La promotion et la rétrogradation des contrôleurs de domaine retournent toujours un code à la fin de l'opération et, à la différence de la plupart des programmes, ne retournent pas zéro en cas de réussite. Pour afficher le code à la fin de la configuration d'un contrôleur de domaine, vous avez plusieurs possibilités :  
  
1.  Quand vous utilisez le Gestionnaire de serveur, examinez les résultats de la promotion dans les dix secondes qui précèdent le redémarrage automatique.  
  
2.  Quand vous utilisez Windows PowerShell ADDSDeployment, examinez les résultats de la promotion dans les dix secondes qui précèdent le redémarrage automatique. Vous pouvez également choisir de ne pas redémarrer automatiquement à la fin de l'opération. Vous devez ajouter le pipeline **Format-List** pour faciliter la lecture de la sortie. Exemple :  
  
    ```  
    Install-addsdomaincontroller <options> -norebootoncompletion:$true | format-list  
  
    ```  
  
    Les erreurs qui s'affichent pendant la vérification et la validation de la configuration requise n'étant pas conservées après un redémarrage, elles sont visibles dans tous les cas. Exemple :  
  
  ![Résolution des problèmes](media/Troubleshooting-Domain-Controller-Deployment/ADDS_PSPrereqError.png)  
  
3.  Dans tout scénario, examinez dcpromo.log et dcpromoui.log.  
  
    > [!NOTE]  
    > Certaines des erreurs répertoriées ci-dessous ne sont plus possibles en raison des modifications de configuration de contrôleur de domaine et de système d'exploitation dans les systèmes d'exploitation ultérieurs. Les nouveaux codes Windows PowerShell ADDSDeployment empêchent également certaines erreurs, ce qui n'est pas le cas de dcpromo.exe /unattend ; voilà une autre raison incontestable de transférer l'ensemble de votre automatisation actuelle de DCPromo qui est déconseillé vers Windows PowerShell ADDSDeployment.  
  
La promotion et la rétrogradation retournent les codes des messages de réussite suivants.  
  
|Code d'erreur|Explication|Remarque|  
|--------------|---------------|--------|  
|1|Sortie, réussite|Vous devez quand même redémarrer, cela indique juste que l'indicateur de redémarrage automatique a été supprimé|  
|2|Sortie, réussite, redémarrage nécessaire||  
|3|Sortie, réussite, avec une défaillance non critique|Généralement affiché quand l'avertissement de délégation DNS est retourné. En l'absence de configuration de la délégation DNS, utilisez :<br /><br />-creatednsdelegation:$false|  
|4|Sortie, réussite, avec une défaillance non critique, redémarrage nécessaire|Généralement affiché quand l'avertissement de délégation DNS est retourné. En l'absence de configuration de la délégation DNS, utilisez :<br /><br />-creatednsdelegation:$false|  
  
La promotion et la rétrogradation retournent les codes des messages d'échec suivants. Il est également probable qu'il existe un message d'erreur étendue ; lisez toujours attentivement l'intégralité du message d'erreur et non uniquement la partie numérique.  
  
|Code d'erreur|Explication|Solution suggérée|  
|--------------|---------------|------------------------|  
|11|La promotion du contrôleur de domaine est déjà en cours d'exécution|N'exécutez pas plusieurs instances de promotion du contrôleur de domaine en même temps pour le même ordinateur cible|  
|12|L'utilisateur doit être administrateur|Ouvrez une session en tant que membre du groupe Administrateurs intégré et vérifiez que vous procédez à une élévation avec contrôle de compte d'utilisateur (UAC)|  
|13|L'autorité de certification est installée|Vous ne pouvez pas rétrograder ce contrôleur de domaine, car il s'agit également d'une autorité de certification. Ne supprimez pas l'autorité de certification avant d'avoir soigneusement effectué l'inventaire de son utilisation ; si elle émet des certificats, la suppression du rôle provoque un arrêt. Il est déconseillé d'exécuter des autorités de certification sur les contrôleurs de domaine|  
|14|Exécution en mode de démarrage sans échec|Démarrez le serveur en mode normal|  
|15|La modification du rôle est en cours ou nécessite un redémarrage|Vous devez redémarrer le serveur (en raison des modifications de configuration antérieures) avant la promotion|  
|16|Exécution sur la plateforme incorrecte|*Probablement pas à obtenir cette erreur*|  
|17|Aucun lecteur NTFS 5 n'existe|Cette erreur n'est pas possible dans Windows Server 2012, qui nécessite au moins %systemdrive% formaté en NTFS|  
|18|Espace insuffisant dans windir|Libérez de l'espace sur le volume %systemdrive% à l'aide de cleanmgr.exe|  
|19|Changement de nom en attente. Un redémarrage est nécessaire.|Redémarrez le serveur|  
|20|La syntaxe du nom d'ordinateur est incorrecte|Renommez l'ordinateur avec un nom valide|  
|21|Ce contrôleur de domaine est propriétaire des rôles FSMO, est un catalogue global et/ou est un serveur DNS.|Ajouter **- demoteoperationmasterrole** lors de l’utilisation **- forceremoval**.|  
|22|TCP/IP doit être installé ou ne fonctionne pas|Vérifiez que TCP/IP est configuré et lié, et qu'il fonctionne correctement sur l'ordinateur|  
|23|Le client DNS doit être configuré en premier|Définissez un serveur DNS principal quand vous ajoutez un nouveau contrôleur de domaine à un domaine|  
|24|Les informations d'identification fournies ne sont pas valides ou il manque des éléments requis|Vérifiez que vos nom d'utilisateur et mot de passe sont corrects|  
|25|Le contrôleur de domaine du domaine spécifié est introuvable|Validez les paramètres des clients DNS et les règles de pare-feu|  
|26|Impossible de lire la liste de domaines dans la forêt|Validez les paramètres des clients DNS, la fonctionnalité LDAP et les règles de pare-feu|  
|27|Nom de domaine absent|Indiquez un domaine pendant la promotion ou la rétrogradation|  
|28|Nom de domaine incorrect|Choisissez un autre nom de domaine DNS valide pendant la promotion|  
|29|Le domaine parent n'existe pas|Vérifiez le domaine parent spécifié pendant la création d'un domaine enfant ou d'arborescence|  
|30|Domaine absent de la forêt|Vérifiez le domaine parent fourni|  
|31|Le domaine enfant existe déjà|Indiquez un autre nom de domaine|  
|32|Nom de domaine NetBIOS incorrect|Indiquez un nom de domaine NetBIOS valide|  
|33|Le chemin d'accès aux fichiers IFM n'est pas valide|Validez le chemin d'accès au dossier d'installation à partir du support|  
|34|La base de données IFM est incorrecte|Utilisez l'installation à partir du support appropriée pour ce système d'exploitation et ce rôle (même version de système d'exploitation, même type de contrôleur de domaine : contrôleur de domaine en lecture seule ou contrôleur de domaine en lecture-écriture)|  
|35|SYSKEY manquant|L'installation à partir du support est chiffrée et vous devez fournir un SYSKEY valide pour l'utiliser|  
|37|Le chemin d'accès pour la base de données NTDS ou ses journaux n'est pas valide|Remplacez le chemin d'accès de la base de données et des journaux par un volume NTFS fixe, et non un lecteur mappé ou un chemin d'accès UNC|  
|38|Le volume n'a pas assez d'espace pour la base de données NTDS ou les journaux|Libérez de l'espace à l'aide de cleanmgr.exe, ajoutez davantage d'espace disque, effacez manuellement de l'espace en déplaçant ailleurs les données inutiles|  
|39|Le chemin d'accès à SYSVOL est incorrect|Remplacez le chemin d'accès du dossier SYSVOL par un volume NTFS fixe, et non un lecteur mappé ou un chemin d'accès UNC|  
|40|Nom de site non valide|Indiquez un nom de site existant|  
|41|Mot de passe nécessaire pour le mode sans échec|Indiquez un mot de passe pour le compte DSRM ; il ne peut pas être vide, quel que soit le mode de configuration de la stratégie de mot de passe|  
|42|Le mot de passe pour le mode sans échec ne répond pas aux critères (promotion uniquement)|Indiquez un mot de passe pour le compte DSRM qui répond aux règles configurées de la stratégie de mot de passe|  
|43|Le mot de passe d'administrateur ne répond pas aux critères (rétrogradation uniquement)|Indiquez un mot de passe pour le compte d'administrateur local qui répond aux règles configurées de la stratégie de mot de passe|  
|44|Le nom spécifié pour la forêt n'est pas valide|Indiquez un nom de domaine DNS racine de forêt valide|  
|45|Une forêt avec le nom spécifié existe déjà|Indiquez un autre nom de domaine DNS racine de forêt|  
|46|Le nom spécifié pour l'arborescence n'est pas valide|Indiquez un nom de domaine DNS d'arborescence valide|  
|47|Une arborescence avec le nom spécifié existe déjà|Indiquez un autre nom de domaine DNS d'arborescence|  
|48|Le nom d'arborescence ne tient pas dans la structure de la forêt|Indiquez un autre nom de domaine DNS d'arborescence|  
|49|Le domaine spécifié n'existe pas|Vérifiez le nom de domaine que vous avez tapé|  
|50|Pendant la rétrogradation, le dernier contrôleur de domaine a été détecté même si ce n'est pas vrai, ou le dernier contrôleur de domaine a été spécifié, mais ce n'est pas vrai|N'indiquez pas **Dernier contrôleur de domaine du domaine** (**-lastdomaincontrollerindomain**) si ce n'est pas vrai. Utilisez **- ignorelastdcindomainmismatch** à substituer si il s’agit réellement du dernier contrôleur de domaine et il existe des métadonnées de contrôleur de domaine fantômes|  
|51|Des partitions d'application existent sur ce contrôleur de domaine|Indiquez **Supprimer les partitions d'application** (**-removeapplicationpartitions**)|  
|52|Un argument de ligne de commande requis est manquant (autrement dit, un fichier de réponses doit être spécifié sur la ligne de commande)|*Vu uniquement avec dcpromo / unattend, qui est déconseillé. Consultez la documentation plus ancienne*|  
|53|La promotion/rétrogradation a échoué, l'ordinateur doit être redémarré pour nettoyage|Examinez l'erreur étendue et les journaux|  
|54|La promotion/rétrogradation a échoué|Examinez l'erreur étendue et les journaux|  
|55|La promotion/rétrogradation a été annulée par l'utilisateur|Examinez l'erreur étendue et les journaux|  
|56|La promotion/rétrogradation a été annulée par l'utilisateur, l'ordinateur doit être redémarré pour nettoyage|Examinez l'erreur étendue et les journaux|  
|58|Un nom de site doit être spécifié pendant la promotion du contrôleur de domaine en lecture seule|Vous devez spécifier un site pour un contrôleur de domaine en lecture seule, il n'en détecte pas un automatiquement comme un contrôleur de domaine en lecture-écriture|  
|59|Pendant la rétrogradation, ce contrôleur de domaine est le dernier serveur DNS pour l'une de ses zones|Indiquez qu'il s'agit du **Dernier serveur DNS du domaine** ou utilisez **-ignorelastdnsserverfordomain**|  
|60|Un contrôleur de domaine exécutant Windows Server 2008 ou version ultérieure doit être présent dans le domaine pour promouvoir le contrôleur de domaine en lecture seule|Promouvez au moins un contrôleur de domaine accessible en écriture Windows Server 2008 ou version ultérieure|  
|61|Vous ne pouvez pas installer les services de domaine Active Directory avec DNS dans un domaine existant qui n'héberge pas déjà un serveur DNS|*Impossible d’obtenir cette erreur*|  
|62|Le fichier de réponses ne possède pas de section [DCInstall]|*Vu uniquement avec dcpromo / unattend, qui est déconseillé. Consultez la documentation plus ancienne.*|  
|63|Le niveau fonctionnel de forêt est sous Windows Server 2003|Augmentez le niveau fonctionnel de forêt jusqu'au niveau natif de Windows Server 2003 au minimum. Windows 2000 et Windows NT 4.0 ne sont plus des systèmes d'exploitation pris en charge|  
|64|La promotion a échoué, car une détection binaire des composants a échoué|Installez le rôle AD DS|  
|65|La promotion a échoué, car une installation binaire des composants a échoué|Installez le rôle AD DS|  
|66|La promotion a échoué, car la détection du système d'exploitation a échoué|Examinez l'erreur étendue et les journaux ; le serveur ne parvient pas à retourner la version de son système d'exploitation. Il est probable que l'ordinateur devra être réinstallé, car son intégrité globale est hautement suspecte|  
|68|Le partenaire de réplication n'est pas valide|Utilisez repadmin.exe ou **Get-ADReplication\***  PowerShell de Windows pour valider l’intégrité de contrôleur de domaine partenaire|  
|69|Le port requis est déjà utilisé par une autre application|Utilisez **netstat.exe -anob** pour localiser les processus qui sont incorrectement affectés à des ports AD DS réservés|  
|70|Le contrôleur du domaine racine de la forêt doit être un catalogue global|*Vu uniquement avec dcpromo / unattend, qui est déconseillé. Consultez la documentation plus ancienne*|  
|71|Le serveur DNS est déjà installé|Ne spécifiez pas d'installer DNS (**-installDNS**) si le service DNS est déjà installé|  
|72|L'ordinateur exécute les services Bureau à distance en mode non-administrateur|Vous ne pouvez pas promouvoir ce contrôleur de domaine, car il s'agit également d'un serveur des services Bureau à distance configuré pour plus de deux utilisateurs administrateurs. Ne supprimez pas les services Bureau à distance avant d'avoir soigneusement effectué l'inventaire de leur utilisation ; s'ils sont utilisés par des applications ou des utilisateurs finaux, la suppression provoque un arrêt|  
|73|Le niveau fonctionnel de forêt spécifié n'est pas valide.|Indiquez un niveau fonctionnel de forêt valide|  
|74|Le niveau fonctionnel de domaine spécifié n'est pas valide.|Indiquez un niveau fonctionnel de domaine valide|  
|75|Impossible de déterminer la stratégie de réplication de mot de passe par défaut.|Validez que la stratégie de réplication de mot de passe de contrôleur de domaine en lecture seule existe et est accessible|  
|76|Les groupes de sécurité répliqués/non répliqués spécifiés ne sont pas valides|Confirmez que vous avez tapé des comptes d'utilisateur et de domaine valides quand vous avez spécifié une stratégie de réplication de mot de passe|  
|77|L'argument spécifié n'est pas valide|Examinez l'erreur étendue et les journaux|  
|78|Échec de l'examen de la forêt Active Directory|Examinez l'erreur étendue et les journaux|  
|79|Le contrôleur de domaine en lecture seule ne peut pas être promu, car la commande rodcprep n'a pas été exécutée|Utilisez Windows Server 2012 pour préparer la forêt ou utilisez **adprep.exe /rodcprep**|  
|80|La commande Domainprep n'a pas été exécutée|Utilisez Windows Server 2012 pour préparer le domaine ou utilisez **adprep.exe /domainprep**|  
|81|La commande Forestprep n'a pas été exécutée|Utilisez Windows Server 2012 pour préparer la forêt ou utilisez **adprep.exe /forestprep**|  
|82|Incompatibilité du schéma de la forêt|Utilisez Windows Server 2012 pour préparer la forêt ou utilisez **adprep.exe /forestprep**|  
|83|Référence non prise en charge|*Probablement pas à obtenir cette erreur*|  
|84|Impossible de détecter un compte de contrôleur de domaine|Confirmez que l'attribut de contrôle de compte d'utilisateur correct est défini pour les contrôleurs de domaine existants.|  
|85|Impossible de sélectionner un compte de contrôleur de domaine pour la phase 2|Ce message est retourné si vous indiquez « Utiliser un compte existant » mais qu'aucun compte n'est trouvé ou qu'une erreur survient pendant la recherche de compte. Vérifiez que vous avez fourni le compte intermédiaire de contrôleur de domaine en lecture seule correct|  
|86|Exécution de la phase 2 de la promotion nécessaire|Ce message est retourné si vous promouvez un autre contrôleur de domaine, mais qu'il existe déjà un compte et que l'option « Autoriser la réinstallation » n'a pas été spécifiée|  
|87|Il existe un compte de contrôleur de domaine de type incompatible|Renommez l'ordinateur avant la promotion si vous n'essayez pas la connexion à un contrôleur de domaine inoccupé. Vous devez attacher pour le compte du contrôleur de domaine inoccupé en utilisant **- useexistingaccount** et l’argument correct en lecture seule ou en écriture, selon le type de compte|  
|88|L'administrateur de serveur spécifié n'est pas valide|Vous avez spécifié un compte non valide pour la délégation de l'administration du contrôleur de domaine en lecture seule. Vérifiez que le compte spécifié est un groupe ou un utilisateur valide|  
|89|Le maître RID du domaine spécifié est hors connexion.|Utilisez **netdom.exe query fsmo** pour détecter le maître RID. Mettez-le en ligne et rendez-le accessible au contrôleur de domaine que vous promouvez|  
|90|Le maître d'opérations des noms de domaine est hors connexion.|Utilisez **netdom.exe query fsmo** pour détecter le maître d'opérations des noms de domaine. Mettez-le en ligne et rendez-le accessible au contrôleur de domaine que vous promouvez|  
|91|Impossible de détecter si le processus est wow64|*Impossible d’obtenir cette erreur de plus, le système d’exploitation est 64 bits*|  
|92|Le processus Wow64 n'est pas pris en charge|*Impossible d’obtenir cette erreur de plus, le système d’exploitation est 64 bits*|  
|93|Le service du contrôleur de domaine n'est pas exécuté pour la rétrogradation non forcée|Démarrez les services de domaine Active Directory|  
|94|Le mot de passe de l'administrateur local ne répond pas aux conditions requises : vide ou non requis|Fournissez un mot de passe non vide et vérifiez que la stratégie de mot de passe local nécessite un mot de passe|  
|95|Impossible de rétrograder le dernier contrôleur de domaine Windows Server 2008 ou version ultérieure dans le domaine où les contrôleurs de domaine en lecture seule dynamiques existent|Vous devez d'abord rétrograder tous les contrôleurs de domaine en lecture seule avant de pouvoir rétrograder tous les contrôleurs de domaine accessibles en écriture Windows Server 2008 ou version ultérieure|  
|96|Impossible de désinstaller les fichiers binaires du service d'annuaire|*Vu uniquement avec dcpromo / unattend, qui est déconseillé. Consultez la documentation plus ancienne*|  
|97|La version du niveau fonctionnel de forêt est supérieure à celle du système d'exploitation du domaine enfant|Indiquez un niveau fonctionnel de domaine enfant supérieur ou égal au niveau fonctionnel de forêt|  
|98|L'installation/la désinstallation binaire des composants est en cours.|*Vu uniquement avec dcpromo / unattend, qui est déconseillé. Consultez la documentation plus ancienne*|  
|99|Le niveau fonctionnel de forêt est trop faible (l'erreur ne concerne que Windows Server 2012)|Augmentez le niveau fonctionnel de forêt jusqu'au niveau natif de Windows Server 2003 au minimum. Windows 2000 et Windows NT 4.0 ne sont plus des systèmes d'exploitation pris en charge|  
|100|Le niveau fonctionnel de domaine est trop faible (l'erreur ne concerne que Windows Server 2012)|Augmentez le niveau fonctionnel de domaine jusqu'au niveau natif de Windows Server 2003 au minimum. Windows 2000 et Windows NT 4.0 ne sont plus des systèmes d'exploitation pris en charge|  
  
#### <a name="knownlikely-issues-and-support-scenarios"></a>Problèmes connus/probables et scénarios de prise en charge  
Vous trouverez ci-dessous les problèmes courants rencontrés pendant le processus de développement de Windows Server 2012. Tous ces problèmes sont « normaux » et il existe une solution de contournement valide ou une technique plus appropriée pour les éviter initialement. Un grand nombre de ces comportements sont identiques dans Windows Server 2008 R2 et les systèmes d'exploitation plus anciens, mais la réécriture du déploiement des services AD DS génère un intérêt accru envers ces problèmes.  
  
|Problème|La rétrogradation d'un contrôleur de domaine laisse DNS s'exécuter sans zones|  
|---------|-----------------------------------------------------------------|  
|Symptômes|Le serveur répond toujours aux demandes DNS mais n'a aucune information de zone|  
|Résolution et remarques|Quand vous supprimez le rôle AD DS, supprimez également le rôle Serveur DNS ou définissez le service Serveur DNS comme étant désactivé. Gardez à l'esprit que le client DNS doit pointer vers un autre serveur que lui-même. Si vous utilisez Windows PowerShell, exécutez la commande suivante après avoir rétrogradé le serveur :<br /><br />Code - windowsfeature désinstaller dns<br /><br />ou Gestionnaire de configuration<br /><br />Code - set-service dns - starttype désactivé<br />arrêter le service dns|  
  
|Problème|La promotion d'un ordinateur Windows Server 2012 en un domaine en une partie existant ne configure pas updatetopleveldomain=1 ni allowsinglelabeldnsdomain=1|  
|---------|----------------------------------------------------------------------------------------------------------------------------------------------------|  
|Symptômes|L'inscription d'enregistrement dynamique DNS ne se produit pas|  
|Résolution et remarques|Définissez ces valeurs en utilisant les stratégies de groupes Netlogon et DNS. Microsoft a commencé à bloquer la création de domaine en une partie dans Windows Server 2008 ; vous pouvez utiliser l'outil de migration Active Directory (ADMT) ou l'outil de changement de nom pour passer à une structure de domaine DNS approuvée.|  
  
|Problème|La rétrogradation du dernier contrôleur de domaine dans un domaine échoue s'il existe des comptes de contrôleur de domaine en lecture seule inoccupés et précréés|  
|---------|------------------------------------------------------------------------------------------------------------|  
|Symptômes|La rétrogradation échoue avec le message suivant :<br /><br />**Dcpromo.General.54**<br /><br />Les services de domaine Active Directory n'ont pas trouvé un autre contrôleur de domaine Active Directory pour transférer les données restantes dans la partition d'annuaire CN=Schema,CN=Configuration,DC=corp,DC=contoso,DC=com.<br /><br />« Le format du nom de domaine spécifié n'est pas valide. »|  
|Résolution et remarques|Supprimez tous les comptes de contrôleur de domaine en lecture seule précréés restants avant de rétrograder un domaine en utilisant **Dsa.msc** ou **Ntdsutil.exe metadata cleanup**.|  
  
|Problème|La préparation automatisée de la forêt et du domaine n'exécute pas GPPREP|  
|---------|---------------------------------------------------------------|  
|Symptômes|La fonctionnalité de planification interdomaine pour la stratégie de groupe, le mode de planification du jeu de stratégie résultant (RSOP), nécessite les autorisations sur le système de fichiers et Active Directory mises à jour pour la stratégie de groupe existante. Sans Gpprep, vous ne pouvez pas utiliser la planification RSOP sur plusieurs domaines.|  
|Résolution et remarques|Exécutez **adprep.exe /gpprep** manuellement pour tous les domaines qui n'étaient pas auparavant préparés pour Windows Server 2003, Windows Server 2008 ni Windows Server 2008 R2. Les administrateurs ne doivent exécuter GPPrep qu'une seule fois dans l'historique d'un domaine et non avec chaque mise à niveau. Cette commande n'est pas exécutée par le processus adprep automatique car, si vous avez déjà défini des autorisations personnalisées appropriées, l'opération peut provoquer une nouvelle réplication de tout le contenu du dossier SYSVOL sur tous les contrôleurs de domaine.|  
  
|Problème|L'installation à partir du support ne peut pas effectuer de vérification quand elle pointe vers un chemin d'accès UNC|  
|---------|------------------------------------------------------------------|  
|Symptômes|Erreur retournée :<br /><br />Code - chemin d’accès de média n’a pas pu être valider. Exception d’appel de « GetDatabaseInfo » avec les arguments « 2 ». Le dossier n’est pas valide.|  
|Résolution et remarques|Vous devez stocker des fichiers IFM sur un disque local, et non un chemin d'accès UNC distant. Ce blocage intentionnel empêche une promotion partielle du serveur en raison d'une interruption réseau.|  
  
|Problème|Un avertissement relatif à la délégation DNS s'affiche à deux reprises pendant la promotion du contrôleur de domaine|  
|---------|-------------------------------------------------------------------------|  
|Symptômes|Avertissement retourné *à deux reprises* lors de la promotion à l’aide de ADDSDeployment Windows PowerShell :<br /><br />Code - « une délégation pour ce serveur DNS ne peut pas être créée, car la zone parente faisant autorité est introuvable ou elle n’exécute pas Windows DNS server. Si vous intégrez à une infrastructure DNS existante, vous devez créer manuellement une délégation avec ce serveur DNS dans la zone parente pour garantir la résolution de noms fiable en dehors du domaine. Sinon, aucune action n’est requise. »|  
|Résolution et remarques|Ignorez le message. Windows PowerShell ADDSDeployment affiche d'abord l'avertissement pendant la vérification de la configuration requise, puis une nouvelle fois pendant la configuration du contrôleur de domaine. Si vous ne voulez pas configurer la délégation DNS, utilisez l'argument :<br /><br />Code - - creatednsdelegation : $false<br /><br />N'ignorez *pas* les vérifications de la configuration requise afin de supprimer ce message|  
  
|Problème|La spécification d'informations d'identification UPN ou externes au domaine pendant la configuration retourne des erreurs trompeuses|  
|---------|--------------------------------------------------------------------------------------------|  
|Symptômes|Le Gestionnaire de serveur retourne une erreur :<br /><br />Code - Exception appelant « DNSOption » avec les Arguments « 6 »<br /><br />Windows PowerShell ADDSDeployment retourne une erreur :<br /><br />Code - vérification des autorisations d’utilisateur a échoué. Vous devez fournir le nom du domaine auquel appartient ce compte d’utilisateur.|  
|Résolution et remarques|Vérifiez que vous fournissez des informations d'identification de domaine valides sous la forme **domaine\utilisateur**.|  
  
|Problème|La suppression du rôle DirectoryServices-DomainController à l'aide de Dism.exe aboutit à l'impossibilité de redémarrer le serveur|  
|---------|---------------------------------------------------------------------------------------------------|  
|Symptômes|Si vous utilisez Dism.exe pour supprimer le rôle AD DS avant de rétrograder un contrôleur de domaine de manière appropriée, le serveur ne démarre plus normalement et affiche l'erreur suivante :<br /><br />Code - état : 0 x 000000000<br />Info (informations) : Une erreur inattendue s’est produite.|  
|Résolution et remarques|Démarrez en mode de réparation des services d'annuaire à l'aide de *Maj+F8*. Rajoutez le rôle AD DS, puis rétrogradez de force le contrôleur de domaine. Vous pouvez également restaurer l'état du système à partir de la sauvegarde. N'utilisez pas Dism.exe pour la suppression du rôle AD DS ; l'utilitaire ne connaît pas les contrôleurs de domaine.|  
  
|Problème|L'installation d'une nouvelle forêt échoue quand forestmode a la valeur Win2012|  
|---------|--------------------------------------------------------------------|  
|Symptômes|La promotion avec Windows PowerShell ADDSDeployment retourne une erreur :<br /><br />Code -  Test.VerifyDcPromoCore.DCPromo.General.74<br /><br />Échec de la vérification des conditions préalables pour la promotion du contrôleur de domaine. Le niveau fonctionnel du domaine spécifié n’est pas valide|  
|Résolution et remarques|Ne spécifiez pas le mode fonctionnel de forêt Win2012 sans spécifier *également* le mode fonctionnel de domaine Win2012. Voici un exemple qui fonctionnera sans erreurs :<br /><br />Code - forestmode - Win2012 - domainmode Win2012]|  
  
|||  
|-|-|  
|Problème|Le fait de cliquer sur Vérifier dans la zone de sélection Installation à partir du support semble n'avoir aucun effet|  
|Symptômes|Quand vous spécifiez un chemin d'accès à un dossier IFM, le fait de cliquer sur le bouton **Vérifier** ne retourne jamais un message ni ne semble avoir un effet.|  
|Résolution et remarques|Le bouton **Vérifier** retourne uniquement des erreurs en cas de problèmes. Sinon, il vous permet de sélectionner le bouton **Suivant** si vous avez indiqué un chemin d'accès IFM. Vous devez cliquer sur **Vérifier** pour continuer si vous avez sélectionné IFM.|  
  
|||  
|-|-|  
|Problème|La rétrogradation avec le Gestionnaire de serveur ne fournit pas de commentaires avant la fin de l'opération.|  
|Symptômes|Quand vous utilisez le Gestionnaire de serveur pour supprimer le rôle AD DS et rétrograder un contrôleur de domaine, aucun commentaire n'est donné tant que la rétrogradation n'est pas terminée ou n'a pas échoué.|  
|Résolution et remarques|Il s'agit d'une limitation du Gestionnaire de serveur. Pour les commentaires, utilisez l'applet de commande Windows PowerShell ADDSDeployment :<br /><br />Code - addsdomaincontroller désinstaller|  
  
|||  
|-|-|  
|Problème|Le bouton Vérifier de l'option Installation à partir du support ne détecte pas le support du contrôleur de domaine en lecture seule fourni pour le contrôleur de domaine en lecture-écriture, ou inversement.|  
|Symptômes|Pendant la promotion d'un nouveau contrôleur de domaine à l'aide d'IFM et en fournissant un support incorrect à IFM (par exemple un support de contrôleur de domaine en lecture seule pour un contrôleur de domaine en lecture-écriture, ou un support de contrôleur de domaine en lecture-écriture pour un contrôleur de domaine en lecture seule), le bouton Vérifier ne retourne pas d'erreur. Plus tard, la promotion échoue avec l'erreur suivante :<br /><br />Code - une erreur s’est produite en essayant de configurer cet ordinateur comme contrôleur de domaine. <br />La promotion de l’installation à partir du support d’un contrôleur de domaine en lecture seule ne peut pas démarrer car la base de données source spécifiée n’est pas autorisée. Uniquement les bases de données à partir d’autres contrôleurs peuvent être utilisés pour la promotion IFM d’un RODC.|  
|Résolution et remarques|Le bouton Vérifier ne valide que l'intégrité globale d'IFM. Ne fournissez pas le type IFM incorrect à un serveur. Redémarrez le serveur avant de retenter la promotion avec le support approprié.|  
  
|||  
|-|-|  
|Problème|La promotion d'un contrôleur de domaine en lecture seule dans un compte d'ordinateur précréé échoue|  
|Symptômes|Quand vous utilisez Windows PowerShell ADDSDeployment pour promouvoir un nouveau contrôleur de domaine en lecture seule avec un compte d'ordinateur intermédiaire, vous recevez l'erreur suivante :<br /><br />Code - jeu de paramètres ne peut pas être résolu à l’aide de paramètres nommés spécifiés.    <br />InvalidArgument : ParameterBindingException<br />    + FullyQualifiedErrorId : AmbiguousParameterSet,Microsoft.DirectoryServices.Deployment.PowerShell.Commands.Install|  
|Résolution et remarques|Ne fournissez pas des paramètres déjà définis sur un compte de contrôleur de domaine en lecture seule précréé. Par exemple :<br /><br />Code - readonlyreplica-<br />-installdns<br />-donotconfigureglobalcatalog<br />-sitename<br />-installdns|  
  
|||  
|-|-|  
|Problème|Le fait de sélectionner ou de désélectionner « Redémarrer automatiquement le serveur de destination, si nécessaire » n'a aucun effet|  
|Symptômes|Si la sélection (ou désélectionnez) l’option du Gestionnaire de serveur **redémarrer automatiquement chaque serveur de destination si nécessaire** whendemoting un contrôleur de domaine via une suppression de rôle, le serveur redémarre toujours, quel que soit le choix.|  
|Résolution et remarques|Ceci est intentionnel. Le processus de rétrogradation redémarre le serveur, quelle que soit la valeur de ce paramètre.|  
  
|||  
|-|-|  
|Problème|Dcpromo.log affiche « [erreur] la définition de la sécurité sur les fichiers du serveur a échoué avec la valeur 2 »|  
|Symptômes|La rétrogradation d'un contrôleur de domaine s'est déroulée sans problèmes, mais l'examen du journal dcpromo indique l'erreur suivante :<br /><br />Le code - [error] définition de la sécurité sur les fichiers du serveur a échoué avec 2|  
|Résolution et remarques|Ignorez cette erreur, elle est prévue et cosmétique.|  
  
|||  
|-|-|  
|Problème|La vérification adprep de la configuration requise échoue avec l'erreur « Impossible d'effectuer un contrôle de conflit de schéma Exchange »|  
|Symptômes|Quand vous tentez de promouvoir un contrôleur de domaine Windows Server 2012 dans une forêt Windows Server 2003, Windows Server 2008 ou Windows Server 2008 R2, la vérification de la configuration requise échoue avec l'erreur :<br /><br />Code - vérification des conditions préalables pour la préparation d’Active Directory a échoué. Impossible d’effectuer un contrôle de conflit de schéma Exchange pour le domaine *<domain name>* (Exception : le serveur RPC n’est pas disponible)<br /><br />Le journal adprep.log affiche l'erreur suivante :<br /><br />Code - Adprep Impossible d’extraire données à partir du serveur *<domain controller>*<br /><br />via Windows Management Instrumentation (WMI).|  
|Résolution et remarques|Le nouveau contrôleur de domaine ne peut pas accéder à WMI via les protocoles DCOM/RPC sur les contrôleurs de domaine existants. Jusqu'à présent, trois causes ont été identifiées :<br /><br />-Une règle de pare-feu bloque accès aux contrôleurs de domaine existants<br /><br />-Le compte SERVICE réseau est manquant dans la « ouverture de session en tant que service » (SeServiceLogonRight) des privilèges sur les contrôleurs de domaine existants<br /><br />-NTLM est désactivé sur les contrôleurs de domaine, à l’aide de stratégies de sécurité décrites dans [présentation de la Restriction de l’authentification NTLM](https://technet.microsoft.com/library/dd560653(WS.10).aspx)|  
  
|||  
|-|-|  
|Problème|La création d'une forêt AD DS affiche toujours un avertissement DNS|  
|Symptômes|Pendant la création d'une forêt AD DS et de la zone DNS sur le nouveau contrôleur de domaine pour lui-même, vous recevez toujours le message d'avertissement suivant :<br /><br />Code - une erreur a été détectée dans la configuration DNS. <br />Aucun des serveurs DNS utilisés par cet ordinateur a répondu dans l’intervalle de délai d’attente.<br />(code d’erreur 0x000005B4 « ERROR_TIMEOUT »)|  
|Résolution et remarques|Ignorez le message. Cet avertissement est intentionnel sur le premier contrôleur de domaine dans le domaine racine d'une nouvelle forêt, au cas où vous envisageriez de pointer vers une zone et un serveur DNS existants.|  
  
|||  
|-|-|  
|Problème|L'argument -whatif Windows PowerShell retourne des informations incorrectes sur le serveur DNS|  
|Symptômes|Si vous utilisez l'argument **-whatif** pendant la configuration d'un contrôleur de domaine avec **-installdns:$true** implicite ou explicite, la sortie produite affiche :<br /><br />Code - « serveur DNS : Non »|  
|Résolution et remarques|Ignorez le message. DNS est installé et configuré correctement.|  
  
|||  
|-|-|  
|Problème|Après la promotion, l'ouverture de session échoue avec « Espace insuffisant pour traiter cette commande »|  
|Symptômes|Après avoir promu un contrôleur de domaine, puis fermé la session et retenté de l'ouvrir de façon interactive, l'erreur suivante s'affiche :<br /><br />Code - pas de suffisamment de stockage est disponible pour traiter cette commande|  
|Résolution et remarques|Le contrôleur de domaine n'a pas été redémarré après la promotion, en raison d'une erreur ou car vous avez spécifié l'argument Windows PowerShell ADDSDeployment **-norebootoncompletion**. Redémarrez le contrôleur de domaine.|  
  
|||  
|-|-|  
|Problème|Le bouton Suivant n'est pas disponible dans la page Options du contrôleur de domaine|  
|Symptômes|Même si vous avez défini un mot de passe, le bouton **Suivant** dans la page **Options du contrôleur de domaine** du Gestionnaire de serveur n'est pas disponible. Aucun site n'est répertorié dans le menu **Nom du site**.|  
|Résolution et remarques|Vous disposez de plusieurs sites AD DS et au moins l'un d'entre eux n'a pas de sous-réseaux ; ce futur contrôleur de domaine appartient à l'un de ces sous-réseaux. Vous devez sélectionner manuellement le sous-réseau à partir du menu déroulant Nom du site. Vous devez également passer en revue tous les sites AD DS à l'aide de DSSITE.MSC ou utiliser la commande Windows PowerShell suivante pour rechercher tous les sous-réseaux manquants des sites :<br /><br />Code - get-adreplicationsite-filtre *-sous-réseaux de la propriété &#124; where-object { ! $_.subnets - eq "\*»} &#124; nom de format-table|  
  
|||  
|-|-|  
|Problème|La promotion ou la rétrogradation échoue avec le message « Impossible de démarrer le service »|  
|Symptômes|Si vous tentez une promotion, une rétrogradation ou un clonage d'un contrôleur de domaine, l'erreur suivante s'affiche :<br /><br />Code - le service ne peut pas être démarré, car il est désactivé ou il n’a aucun périphérique activé associé » (0 x 80070422)<br /><br />L'erreur peut être interactive, se présenter comme un événement ou être écrite dans un journal tel que dcpromoui.log ou dcpromo.log|  
|Résolution et remarques|Le service de serveur de rôles du service d'annuaire (DsRoleSvc) est désactivé. Par défaut, ce service est installé pendant l'installation de rôle AD DS et le type de démarrage Manuel est défini. Ne désactivez pas ce service. Réaffectez-lui la valeur Manuel et autorisez le démarrage et l'arrêt des opérations de rôles du service d'annuaire à la demande. Ce comportement est normal.|  
  
|||  
|-|-|  
|Problème|Le Gestionnaire de serveur vous avertit quand même que vous devez promouvoir le contrôleur de domaine|  
|Symptômes|Si vous promouvez un contrôleur de domaine en utilisant la commande dcpromo.exe /unattend déconseillée ou mettez à niveau un contrôleur de domaine Windows Server 2008 R2 existant en place vers Windows Server 2012, le Gestionnaire de serveur affiche quand même la tâche de configuration de post-déploiement **Promouvoir ce serveur en contrôleur de domaine**.|  
|Résolution et remarques|Cliquez sur le lien de l'avertissement de post-déploiement et le message disparaît définitivement. Ce comportement est cosmétique et prévu.|  
  
|||  
|-|-|  
|Problème|Le script de déploiement du Gestionnaire de serveur n'a pas l'élément d'installation du rôle|  
|Symptômes|Si vous promouvez un contrôleur de domaine à l'aide du Gestionnaire de serveur et enregistrez le script de déploiement Windows PowerShell, il n'inclut pas l'applet de commande d'installation du rôle ni les arguments (install-windowsfeature -name ad-domain-services -includemanagementtools). Sans le rôle, le contrôleur de domaine ne peut pas être configuré.|  
|Résolution et remarques|Ajoutez manuellement cette applet de commande et les arguments à tous les scripts. Ce comportement est prévu et normal.|  
  
|||  
|-|-|  
|Problème|Le script de déploiement du Gestionnaire de serveur ne s'appelle pas PS1|  
|Symptômes|Si vous promouvez un contrôleur de domaine à l'aide du Gestionnaire de serveur et enregistrez le script de déploiement Windows PowerShell, le fichier reçoit un nom temporaire aléatoire et n'est pas nommé comme un fichier PS1.|  
|Résolution et remarques|Renommez manuellement le fichier. Ce comportement est prévu et normal.|  
  
|Problème|Dcpromo /unattend autorise des niveaux fonctionnels non pris en charge|  
|-|-|  
|Symptômes|Si vous promouvez un contrôleur de domaine à l'aide de dcpromo /unattend avec l'exemple de fichier de réponses suivant :<br /><br />Code -<br /><br />[DCInstall]<br />NewDomain=Forest<br /><br />ReplicaOrNewDomain=Domain<br /><br />NewDomainDNSName=corp.contoso.com<br /><br />SafeModeAdminPassword=Safepassword@6<br /><br />DomainNetbiosName=corp<br /><br />DNSOnNetwork=Yes<br /><br />AutoConfigDNS=Yes<br /><br />RebootOnSuccess=NoAndNoPromptEither<br /><br />RebootOnCompletion=No<br /><br />*DomainLevel=0*<br /><br />*ForestLevel=0*<br /><br />La promotion échoue avec les erreurs suivantes dans dcpromoui.log :<br /><br />Code - dcpromoui EA4.5B8 0089 13:31:50.783 entrée CArgumentsSpec::ValidateArgument DomainLevel<br /><br />dcpromoui EA4.5B8 008A 13:31:50.783 valeur pour DomainLevel est égal à 0<br /><br />dcpromoui EA4.5B8 008 13:31:50.783 le code de sortie est 77<br /><br />dcpromoui EA4.5B8 008C 13:31:50.783 l’argument spécifié n’est pas valide.<br /><br />journal de fermeture de 13:31:50.783 D 008 dcpromoui EA4.5B8<br /><br />dcpromoui EA4.5B8 0032 13:31:50.830 le code de sortie est 77<br /><br />Le niveau 0 représente Windows 2000, qui n'est pas pris en charge dans Windows Server 2012.|  
|Résolution et remarques|N'utilisez pas la commande dcpromo /unattend déconseillée et comprenez qu'elle vous permet de spécifier des paramètres non valides qui échouent par la suite. Ce comportement est prévu et normal.|  

|Problème|Promotion « se bloque » à la création d’objet Paramètres NTDS et se termine jamais|  
|-|-|  
|Symptômes|Si vous promouvez un réplica de contrôleur de domaine ou RODC, la promotion atteint « création l’objet Paramètres NTDS » et ne passe jamais ou se termine. Les journaux arrêtent également la mise à jour.|  
|Résolution et remarques|Il s'agit d'un problème connu qui survient quand les informations d'identification du compte d'administrateur local intégré sont fournies avec un mot de passe correspondant au compte d'administrateur de domaine intégré. Cela entraîne une défaillance du moteur d'installation principal qui ne génère pas d'erreur, mais attend en revanche indéfiniment (quasi-boucle). Ce comportement est attendu - quoique indésirables - comportement.<br /><br />Pour réparer le serveur :<br /><br />1.  Redémarrez-le.<br /><br />1.  Dans AD, supprimez le compte d’ordinateur de ce serveur membre (encore ne sera pas un compte de contrôleur de domaine)<br /><br />1.  Sur ce serveur, retirez-le de force du domaine<br /><br />1.  Sur ce serveur, supprimez le rôle AD DS.<br /><br />1.  Redémarrer<br /><br />1.  Ajoutez de nouveau le rôle AD DS et tentez de nouveau la promotion, en vérifiant que vous fournissez toujours les informations d'identification au format ***domaine\administrateur*** à la promotion de contrôleur de domaine et non uniquement le compte d'administrateur local intégré|  
