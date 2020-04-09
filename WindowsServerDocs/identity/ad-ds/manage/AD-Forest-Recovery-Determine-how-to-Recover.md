---
title: Récupération de la forêt Active Directory-déterminer comment récupérer la forêt
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: fea55dc5551198f7bc06afb2ec38077398b9cf77
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80824052"
---
# <a name="determine-how-to-recover-the-forest"></a>Déterminer comment récupérer la forêt

>S’applique à : Windows Server 2016, Windows Server 2012 et 2012 R2, Windows Server 2008 et 2008 R2

La récupération d’une forêt Active Directory entière implique de la restaurer à partir de la sauvegarde ou de la réinstallation de Active Directory Domain Services (AD DS) sur chaque contrôleur de domaine (DC) de la forêt. La récupération de la forêt restaure chaque domaine de la forêt à son état au moment de la dernière sauvegarde approuvée. Par conséquent, l’opération de restauration entraînera la perte d’au moins les données de Active Directory suivantes :

- Tous les objets (tels que les utilisateurs et les ordinateurs) qui ont été ajoutés après la dernière sauvegarde approuvée
- Toutes les mises à jour apportées aux objets existants depuis la dernière sauvegarde approuvée
- Toutes les modifications apportées à la partition de configuration ou à la partition de schéma dans AD DS (telles que les modifications de schéma) depuis la dernière sauvegarde approuvée

Pour chaque domaine de la forêt, le mot de passe d’un compte d’administrateur de domaine doit être connu. De préférence, il s’agit du mot de passe du compte administrateur intégré. Vous devez également connaître le mot de passe DSRM pour effectuer une restauration de l’état du système d’un contrôleur de réseau. En général, il est recommandé d’archiver le compte administrateur et l’historique des mots de passe DSRM dans un endroit sûr tant que les sauvegardes sont valides, c’est-à-dire dans la durée de vie de désactivation ou dans la durée de vie des objets supprimés si Active Directory corbeille est activée. Vous pouvez également synchroniser le mot de passe DSRM avec un compte d’utilisateur de domaine afin de le rendre plus facile à mémoriser. Pour plus d’informations, consultez l’article [961320](https://support.microsoft.com/kb/961320)de la base de connaissances. La synchronisation du compte DSRM doit être effectuée avant la récupération de la forêt, dans le cadre de la préparation.

> [!NOTE]
> Le compte administrateur est membre du groupe Administrateurs intégré par défaut, tout comme les groupes Admins du domaine et administrateurs de l’entreprise. Ce groupe dispose d’un contrôle total sur tous les contrôleurs de domaine du domaine.

## <a name="determining-which-backups-to-use"></a>Détermination des sauvegardes à utiliser

Sauvegardez régulièrement au moins deux contrôleurs de domaine inscriptibles pour chaque domaine, afin de pouvoir choisir entre plusieurs sauvegardes. Notez que vous ne pouvez pas utiliser la sauvegarde d’un contrôleur de domaine en lecture seule (RODC) pour restaurer un contrôleur de domaine accessible en écriture. Nous vous recommandons de restaurer les contrôleurs de l’entreprise à l’aide de sauvegardes qui ont pris quelques jours avant l’occurrence de la défaillance. En général, vous devez déterminer un compromis entre le dernier et la sécurité des données restaurées. Le choix d’une sauvegarde plus récente permet de récupérer des données plus utiles, mais cela peut augmenter le risque de réintroduire des données dangereuses dans la forêt restaurée.

La restauration des sauvegardes de l’état du système dépend du système d’exploitation et du serveur d’origine de la sauvegarde. Par exemple, vous ne devez pas restaurer une sauvegarde de l’état du système sur un autre serveur. Dans ce cas, l’avertissement suivant peut s’afficher :

«La sauvegarde spécifiée est d’un serveur différent de celui en cours. Nous vous déconseillons d’effectuer une récupération de l’état du système avec la sauvegarde sur un autre serveur, car le serveur peut devenir inutilisable. Voulez-vous vraiment utiliser cette sauvegarde pour récupérer le serveur actuel ?»

Si vous devez restaurer Active Directory sur un autre matériel, créez des sauvegardes complètes du serveur et planifiez l’exécution d’une récupération complète du serveur.

> [!IMPORTANT]
> À partir de Windows Server 2008, la restauration de la sauvegarde de l’état du système vers une nouvelle installation de Windows Server sur un nouveau matériel ou sur le même matériel n’est pas prise en charge. Si Windows Server est réinstallé sur le même matériel, comme recommandé plus loin dans ce guide, vous pouvez restaurer le contrôleur de domaine dans cet ordre :
>
> 1. Effectuez une restauration complète du serveur afin de restaurer le système d’exploitation et tous les fichiers et applications.
> 2. Effectuez une restauration de l’état du système à l’aide de Wbadmin. exe afin de marquer SYSVOL comme faisant autorité.
>
> Pour plus d’informations, consultez l’article [249694](https://support.microsoft.com/kb/249694)de la base de connaissances Microsoft.

Si l’heure de l’échec est inconnue, examinez plus en détail pour identifier les sauvegardes qui contiennent le dernier État sûr de la forêt. Cette approche est moins souhaitable. Par conséquent, nous vous recommandons vivement de conserver quotidiennement des journaux détaillés sur l’état d’intégrité de AD DS, de sorte que, en cas de défaillance à l’échelle de la forêt, la durée approximative de l’échec peut être identifiée. Vous devez également conserver une copie locale des sauvegardes pour accélérer la récupération.

Si Active Directory corbeille est activée, la durée de vie de la sauvegarde est égale à la valeur de **deletedObjectLifetime** ou à la valeur **tombstoneLifetime** , la valeur la plus petite étant retenue. Pour plus d’informations, consultez [Guide pas à pas de la corbeille Active Directory](https://go.microsoft.com/fwlink/?LinkId=178657) (https://go.microsoft.com/fwlink/?LinkId=178657).

Vous pouvez également utiliser l’outil de montage de base de données Active Directory (DSAMAIN. exe) et un outil LDAP (Lightweight Directory Access Protocol), tels que LDP. exe ou Active Directory des utilisateurs et des ordinateurs, pour identifier la sauvegarde qui a le dernier état de sécurité de la forêt. L’outil de montage de base de données Active Directory, qui est inclus dans les systèmes d’exploitation Windows Server 2008 et versions ultérieures, expose Active Directory données stockées dans les sauvegardes ou les captures instantanées en tant que serveur LDAP. Ensuite, vous pouvez utiliser un outil LDAP pour parcourir les données. Cette approche présente l’avantage de ne pas vous obliger à redémarrer un contrôleur de service en mode de restauration des services d’annuaire (DSRM) pour examiner le contenu de la sauvegarde de AD DS.

Pour plus d’informations sur l’utilisation de l’outil de montage de base de données Active Directory, consultez le [Guide pas à pas de l’outil de montage de base de données Active Directory](https://technet.microsoft.com/library/cc753609\(WS.10\).aspx).

Vous pouvez également utiliser la commande de **capture instantanée Ntdsutil** pour créer des instantanés de la base de données Active Directory. En planifiant une tâche pour créer régulièrement des instantanés, vous pouvez obtenir des copies supplémentaires de la base de données Active Directory dans le temps. Vous pouvez utiliser ces copies pour mieux identifier le moment où la défaillance s’est produite au niveau de la forêt, puis choisir la sauvegarde à restaurer. Pour créer des instantanés, utilisez la version de **Ntdsutil** fournie avec windows Server 2008 ou le outils D’ADMINISTRATION de serveur distant (RSAT) pour Windows Vista ou version ultérieure. Le contrôleur de périphérique cible peut exécuter n’importe quelle version de Windows Server. Pour plus d’informations sur l’utilisation de la commande de **capture instantanée Ntdsutil** , consultez [instantané](https://technet.microsoft.com/library/cc731620\(WS.10\).aspx).

## <a name="determining-which-domain-controllers-to-restore"></a>Détermination des contrôleurs de domaine à restaurer

La facilité du processus de restauration est un facteur important lorsque vous décidez du contrôleur de domaine à restaurer. Il est recommandé de disposer d’un contrôleur de domaine dédié pour chaque domaine qui est le contrôleur de domaine préféré pour une restauration. Un contrôleur de service de restauration dédié facilite la planification et l’exécution de la récupération de forêt, car vous utilisez la même configuration source que celle utilisée pour effectuer des tests de restauration. Vous pouvez créer un script pour la récupération et ne pas rivaliser avec différentes configurations, par exemple si le contrôleur de domaine contient des rôles de maître d’opérations ou non, ou s’il s’agit d’un serveur de catalogue global ou DNS.

> [!NOTE]
> Bien qu’il ne soit pas recommandé de restaurer un détenteur du rôle de maître d’opérations dans un souci de simplicité, certaines organisations peuvent choisir d’en restaurer une pour d’autres avantages. Par exemple, la restauration du maître RID peut aider à éviter les problèmes de gestion des RID lors de la récupération.  

Choisissez un contrôleur de périphérique qui répond le mieux aux critères suivants :

- Contrôleur de périphérique qui est accessible en écriture. Cette donnée est obligatoire.

- Un contrôleur de périphérique exécutant Windows Server 2012 en tant que machine virtuelle sur un hyperviseur qui prend en charge VM-GenerationID. Ce contrôleur de périphérique peut être utilisé comme source pour le clonage.
- Un contrôleur de réseau accessible, physiquement ou sur un réseau virtuel, et de préférence situé dans un centre de contenu. De cette façon, vous pouvez facilement isoler le réseau au cours de la récupération de la forêt.
- Un contrôleur de périphérique qui dispose d’une bonne sauvegarde complète du serveur. Une sauvegarde correcte est une sauvegarde qui peut être restaurée correctement, a été effectuée quelques jours avant la défaillance et contient autant de données utiles que possible.
- Un contrôleur de domaine qui était un serveur DNS (Domain Name System) avant la défaillance. Cela vous permet de gagner du temps pour réinstaller DNS.
- Si vous utilisez également les services de déploiement Windows, choisissez un contrôleur de réseau qui n’est pas configuré pour utiliser le déverrouillage réseau BitLocker. Dans ce cas, le déverrouillage réseau BitLocker n’est pas pris en charge pour le premier contrôleur de réseau que vous restaurez à partir d’une sauvegarde pendant une récupération de forêt.

   Le déverrouillage réseau BitLocker, car le *seul* protecteur de clé *ne peut pas* être utilisé sur les contrôleurs de réseau où vous avez déployé les services de déploiement Windows (WDS), car cela entraîne un scénario dans lequel le premier contrôleur de réseau exige que Active Directory et WDS fonctionnent pour le déverrouillage. Mais avant de restaurer le premier contrôleur de service, Active Directory n’est pas encore disponible pour WDS, il ne peut donc pas être déverrouillé.

   Pour déterminer si un contrôleur de réseau est configuré pour utiliser le déverrouillage réseau BitLocker, vérifiez qu’un certificat de déverrouillage réseau est identifié dans la clé de Registre suivante :

   HKEY_LOCAL_MACHINESoftwarePoliciesMicrosoftSystemCertificatesFVE_NKP

Tenez à jour les procédures de sécurité lors du traitement ou de la restauration des fichiers de sauvegarde incluant des Active Directory. L’urgence qui accompagne la récupération de la forêt peut déboucher sur les meilleures pratiques de sécurité. Pour plus d’informations, consultez la section intitulée « établissement des stratégies de sauvegarde et de restauration des contrôleurs de domaine » dans [Best Practice Guide (en anglais) pour sécuriser les Installations Active Directory et les opérations quotidiennes : partie 2](https://technet.microsoft.com/library/bb727066.aspx).

## <a name="identify-the-current-forest-structure-and-dc-functions"></a>Identifier la structure actuelle de la forêt et les fonctions DC

Déterminez la structure actuelle de la forêt en identifiant tous les domaines de la forêt. Dressez la liste de tous les contrôleurs de domaine dans chaque domaine, en particulier les contrôleurs de domaine qui ont des sauvegardes et des contrôleurs de domaine virtualisés qui peuvent être une source de clonage. La liste des contrôleurs de domaine pour le domaine racine de forêt est la plus importante, car vous allez d’abord récupérer ce domaine. Une fois que vous avez restauré le domaine racine de forêt, vous pouvez obtenir la liste des autres domaines, contrôleurs de domaine et sites de la forêt à l’aide des composants logiciels enfichables Active Directory.

Préparez une table qui affiche les fonctions de chaque contrôleur de domaine dans le domaine, comme illustré dans l’exemple suivant. Cela vous permettra de revenir à la configuration de la forêt après l’échec après la récupération.

|Nom du contrôleur de périphérique|Système d’exploitation|FSMO|GC|RODC|Secours|DNS|Minimale|VM|Machine virtuelle-GenID|  
|-------------|----------------------|----------|--------|----------|------------|---------|-----------------|--------|---------------|  
|DC_1|Windows Server 2012|Contrôleur de schéma, maître d’attribution de noms de domaine|Oui|Non|Oui|Non|Non|Oui|Oui|  
|DC_2|Windows Server 2012|Aucune|Oui|Non|Oui|Oui|Non|Oui|Oui|  
|DC_3|Windows Server 2012|Maître d'infrastructure|Non|Non|Non|Oui|Oui|Oui|Oui|  
|DC_4|Windows Server 2012|Émulateur PDC, maître RID|Oui|Non|Non|Non|Non|Oui|Non|  
|DC_5|Windows Server 2012|Aucune|Non|Non|Oui|Oui|Non|Oui|Oui|  
|RODC_1|Windows Server 2008 R2|Aucune|Oui|Oui|Oui|Oui|Oui|Oui|Non|  
|RODC_2|Windows Server 2008|Aucune|Oui|Oui|Non|Oui|Oui|Oui|Non|  

Pour chaque domaine de la forêt, identifiez un contrôleur de domaine inscriptible unique qui dispose d’une sauvegarde approuvée de la base de données Active Directory pour ce domaine. Soyez prudent lorsque vous choisissez une sauvegarde pour restaurer un contrôleur de périphérique. Si le jour et la cause de l’échec sont approximativement connus, la recommandation générale consiste à utiliser une sauvegarde qui a été effectuée quelques jours avant cette date.
  
Dans cet exemple, il existe quatre candidats de sauvegarde : DC_1, DC_2, DC_4 et DC_5. Parmi ces candidats de sauvegarde, vous ne pouvez en restaurer qu’un seul. Le contrôleur de périphérique recommandé est DC_5 pour les raisons suivantes :  

- Elle répond à la configuration requise pour l’utiliser en tant que source pour le clonage de contrôleur de service virtualisé, autrement dit, elle exécute Windows Server 2012 en tant que contrôleur de périphérique virtuel sur un hyperviseur qui prend en charge VM-GenerationID, exécute les logiciels qui sont autorisés à être clonés (ou qui peuvent être supprimés s’ils ne peuvent pas être clonés). Après la restauration, le rôle d’émulateur de contrôleur de domaine principal est pris pour ce serveur et il peut être ajouté au groupe contrôleurs de domaine clonables pour le domaine.  
- Il exécute une installation complète de Windows Server 2012. Un contrôleur de périphérique qui exécute une installation Server Core peut être moins pratique en tant que cible pour la récupération.  
- Il s’agit d’un serveur DNS. Par conséquent, il n’est pas nécessaire de réinstaller DNS.  

> [!NOTE]
> Étant donné que DC_5 n’est pas un serveur de catalogue global, il présente également un avantage en ce que le catalogue global n’a pas besoin d’être supprimé après la restauration. Mais si le contrôleur de l’état de l’alimentation est également un serveur de catalogue global, ce qui n’est pas un facteur déterminant, car à compter de Windows Server 2012, tous les contrôleurs de contrôle sont des serveurs de catalogue global par défaut, et il est recommandé de supprimer et d’ajouter le catalogue global après la restauration dans tous les cas.  

## <a name="recover-the-forest-in-isolation"></a>Récupérer la forêt de manière isolée

Le scénario par défaut consiste à arrêter tous les contrôleurs de contrôle de l’écriture avant que le premier contrôleur de périphérique restauré soit rétabli en production. Cela garantit que toutes les données dangereuses ne sont pas répliquées dans la forêt récupérée. Il est particulièrement important d’arrêter tous les détenteurs du rôle de maître d’opérations.  

> [!NOTE]
> Dans certains cas, vous pouvez déplacer le premier contrôleur de domaine que vous envisagez de récupérer pour chaque domaine vers un réseau isolé tout en autorisant les autres contrôleurs de domaine à rester en ligne afin de minimiser les temps d’arrêt du système. Par exemple, si vous récupérez à partir d’un échec de mise à niveau de schéma, vous pouvez choisir de conserver les contrôleurs de domaine en cours d’exécution sur le réseau de production tout en effectuant des étapes de récupération isolées.

Si vous exécutez des contrôleurs de service virtualisés, vous pouvez les déplacer vers un réseau virtuel qui est isolé du réseau de production où vous allez effectuer la récupération. Le déplacement de contrôleurs de réseau virtualisés vers un réseau distinct offre deux avantages :  

- Les contrôleurs de contrôleur récupérés ne peuvent pas réoccurrencer le problème à l’origine de la récupération de la forêt, car ils sont isolés.  
- Le clonage de contrôleur de réseau virtualisé peut être effectué sur le réseau distinct afin qu’un nombre critique de contrôleurs de service puisse être exécuté et testé avant d’être redirigé vers le réseau de production.

Si vous exécutez des contrôleurs de domaine sur du matériel physique, déconnectez le câble réseau du premier contrôleur de domaine que vous envisagez de restaurer dans le domaine racine de forêt. Si possible, déconnectez également les câbles réseau de tous les autres contrôleurs de réseau. Cela empêche les contrôleurs de contrôle de la réplication, s’ils sont démarrés par inadvertance pendant le processus de récupération de la forêt.  

Dans une forêt importante qui est répartie sur plusieurs emplacements, il peut être difficile de garantir que tous les contrôleurs de domaines inscriptibles sont arrêtés. Pour cette raison, les étapes de récupération (telles que la réinitialisation du compte d’ordinateur et le compte krbtgt, en plus du nettoyage des métadonnées) sont conçues pour garantir que les contrôleurs de service inscriptibles récupérés ne sont pas répliqués avec des contrôleurs de service potentiellement inscriptibles dangereux (au cas où d’autres sont encore en ligne dans la forêt).  
  
Toutefois, la réplication ne se produit qu’en prenant en charge les contrôleurs de l’écriture hors connexion. Par conséquent, chaque fois que cela est possible, vous devez déployer une technologie de gestion à distance qui peut vous aider à arrêter et à isoler physiquement les contrôleurs de l’accès en écriture pendant la récupération de la forêt.  
  
Les RODC peuvent continuer à fonctionner alors que les contrôleurs de contrôle accessibles en écriture sont hors connexion. Aucun autre contrôleur de domaine ne réplique directement les modifications à partir d’un RODC, en particulier aucun changement de schéma ou de conteneur de configuration, de sorte qu’ils ne présentent pas le même risque que les contrôleurs de domaine accessibles en écriture pendant la récupération. Une fois que tous les contrôleurs de service accessibles en écriture sont récupérés et en ligne, vous devez régénérer tous les contrôleurs RODC.  
  
Les RODC continuent d’autoriser l’accès aux ressources locales mises en cache dans leurs sites respectifs, tandis que les opérations de récupération se poursuivent en parallèle. Les ressources locales qui ne sont pas mises en cache sur le RODC comporteront des demandes d’authentification transmises à un contrôleur de domaine accessible en écriture. Ces requêtes échouent, car les contrôleurs de contrôle accessibles en écriture sont hors connexion. Certaines opérations, telles que les modifications de mot de passe, ne fonctionnent pas tant que vous n’avez pas récupéré les contrôleurs de réseau inscriptibles.  
  
Si vous utilisez une architecture de réseau Hub and spoke, vous pouvez vous concentrer tout d’abord sur la récupération des contrôleurs de réseau accessibles en écriture dans les sites hub. Ultérieurement, vous pouvez régénérer les contrôleurs de l’accès en lecture seule dans les sites distants.  
  
## <a name="next-steps"></a>Étapes suivantes

- [Récupération de la forêt Active Directory : conditions préalables](AD-Forest-Recovery-Prerequisties.md)  
- [Récupération de la forêt Active Directory-élaboration d’un plan de récupération de forêt personnalisé](AD-Forest-Recovery-Devising-a-Plan.md)  
- [Récupération de la forêt Active Directory-identifier le problème](AD-Forest-Recovery-Identify-the-Problem.md)
- [Récupération de la forêt Active Directory-déterminer comment récupérer](AD-Forest-Recovery-Determine-how-to-Recover.md)
- [Récupération de la forêt Active Directory-effectuer la récupération initiale](AD-Forest-Recovery-Perform-initial-recovery.md)  
- [Récupération de la forêt Active Directory : procédures](AD-Forest-Recovery-Procedures.md)  
- [Récupération de la forêt Active Directory-Forum aux questions](AD-Forest-Recovery-FAQ.md)  
- [Récupération de la forêt Active Directory-récupération d’un domaine unique au sein d’une forêt à plusieurs domaines](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
- [Récupération de forêt AD-récupération de forêt avec les contrôleurs de domaine Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md)  
