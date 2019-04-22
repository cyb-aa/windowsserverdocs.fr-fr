---
title: Récupération de forêt AD - déterminer comment récupérer la forêt
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: 30d23d977b4d7cd320d78ff340120df7013dee4c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817730"
---
# <a name="determine-how-to-recover-the-forest"></a>Déterminer comment récupérer la forêt

>S'applique à : Windows Server 2016, Windows Server 2012 et 2012 R2, Windows Server 2008 et 2008 R2

Récupération d’un ensemble de la forêt Active Directory implique soit restaurer à partir de la sauvegarde ou la réinstallation des Services de domaine Active Directory (AD DS) sur chaque contrôleur de domaine (DC) dans la forêt. Récupération de la forêt restaure chaque domaine dans la forêt à son état au moment de la dernière sauvegarde de confiance. Par conséquent, l’opération de restauration entraîne la perte d’au moins les données d’Active Directory suivantes :

- Tous les objets qui ont été ajoutées après la dernière sauvegarde de confiance (par exemple, les utilisateurs et ordinateurs)
- Toutes les mises à jour qui ont été apportées aux objets existants dans la mesure où le dernier approuvé sauvegarde
- Toutes les modifications qui ont été apportées à la partition de configuration ou de la partition de schéma dans AD DS (telles que les modifications de schéma) depuis la dernière sauvegarde de confiance

Pour chaque domaine dans la forêt, le mot de passe d’un compte d’administrateur de domaine doit être connu. Il s’agit de préférence, le mot de passe du compte administrateur intégré. Vous devez également connaître le mot de passe DSRM pour effectuer une restauration d’état système d’un contrôleur de domaine. En général, il est conseillé d’archiver le compte administrateur et un historique de mot de passe DSRM en lieu sûr pour tant que les sauvegardes sont valides, autrement dit, au sein de la durée de vie de désactivation ou de la durée de vie des objets supprimés période si Active Directory recycler Bin est activé. Vous pouvez également synchroniser le mot de passe DSRM avec un compte d’utilisateur de domaine pour le rendre plus facile à mémoriser. Pour plus d’informations, consultez l’article [961320](https://support.microsoft.com/kb/961320). Synchroniser le compte DSRM doit être effectuée avant la récupération de forêt, dans le cadre des étapes de préparation.

> [!NOTE]
> Le compte administrateur est un membre du groupe Administrateurs intégré par défaut, comme le sont les groupes Admins du domaine et administrateurs de l’entreprise. Ce groupe a un contrôle total de tous les contrôleurs de domaine dans le domaine.

## <a name="determining-which-backups-to-use"></a>Détermination des sauvegardes à utiliser

Sauvegarder au moins deux contrôleurs de domaine accessible en écriture pour chaque domaine régulièrement afin d’avoir plusieurs sauvegardes sélectionnables. Notez que vous ne pouvez pas utiliser la sauvegarde d’un contrôleur de domaine en lecture seule (RODC) pour restaurer un contrôleur de domaine accessible en écriture. Nous vous recommandons de restaurer les contrôleurs de domaine à l’aide de sauvegardes qui ont été effectuées à quelques jours avant l’occurrence de l’échec. En règle générale, vous devez déterminer un compromis entre l’ancienneté et le safeness les données restaurées. Choix d’une sauvegarde plus récente récupère des données plus utiles, mais il peut augmenter le risque de réintroduction de données dangereuses dans la forêt restaurée.

Dépend de la restauration des sauvegardes d’état du système sur le serveur de la sauvegarde et le système d’exploitation d’origine. Par exemple, vous ne devez pas restaurer une sauvegarde de l’état système vers un autre serveur. Dans ce cas, vous pouvez voir l’avertissement suivant :

« La sauvegarde spécifiée est d’un autre serveur que celui en cours. Nous vous déconseillons d’effectuer une récupération de l’état système avec la sauvegarde sur un autre serveur, car le serveur risque de devenir inutilisable. Êtes-vous sûr à utiliser cette sauvegarde pour la récupération du serveur actuel ? »

Si vous avez besoin restaurer Active Directory sur un autre matériel, créer des sauvegardes de serveur complet et pour effectuer une récupération complète du serveur.

> [!IMPORTANT]
> À compter de Windows Server 2008, il n'est pas pris en charge pour restaurer la sauvegarde de l’état système vers une nouvelle installation de Windows Server sur le nouveau matériel ou le même matériel. Si Windows Server est réinstallé sur le même matériel, comme indiqué plus loin dans ce guide, vous pouvez restaurer le contrôleur de domaine dans cet ordre :
>
> 1. Effectuer une restauration complète du serveur pour restaurer le système d’exploitation et tous les fichiers et applications.
> 2. Effectuer une restauration d’état système à l’aide de wbadmin.exe pour marquer SYSVOL comme faisant autorité.
>
> Pour plus d’informations, voir base de connaissances Microsoft l’article [249694](https://support.microsoft.com/kb/249694).

Si l’heure de l’occurrence de l’échec est inconnu, approfondir vos recherches pour identifier les sauvegardes qui contiennent le dernier état sans échec de la forêt. Cette approche est moins précise. Par conséquent, nous vous recommandons fortement de conserver les journaux détaillés de l’état d’intégrité des services AD DS sur une base quotidienne afin que, en l’absence d’un échec de la forêt, l’heure approximative de défaillance peut être identifié. Vous devez également garder une copie locale des sauvegardes afin d’activer une récupération plus rapide.

Si la Corbeille Active Directory est activée, la durée de vie de sauvegarde est égale à la **deletedObjectLifetime** valeur ou la **tombstoneLifetime** valeur étant retenue. Pour plus d’informations, consultez [Active Directory Step Guide Corbeille](https://go.microsoft.com/fwlink/?LinkId=178657) (https://go.microsoft.com/fwlink/?LinkId=178657).

Comme alternative, vous pouvez également utiliser l’outil de montage de base de données Active Directory (Dsamain.exe) et un outil Lightweight Directory Access Protocol (LDAP), tels que Ldp.exe ou utilisateurs Active Directory et les ordinateurs, pour identifier la sauvegarde a le dernier état sans échec de la forêt. L’outil de montage de base de données Active Directory, qui est inclus dans Windows Server 2008 et les systèmes d’exploitation de Windows Server ultérieurs, expose des données d’Active Directory qui sont stockées dans les sauvegardes ou de captures instantanées avec un serveur LDAP. Ensuite, vous pouvez utiliser un outil LDAP pour parcourir les données. Cette approche présente l’avantage de ne pas nécessiter de redémarrer un contrôleur de domaine dans Services Restore Mode annuaire (DSRM) pour examiner le contenu de la sauvegarde des services AD DS.

Pour plus d’informations sur l’utilisation de la base de données Active Directory outil de montage, consultez le [Guide de pas à pas Active Directory de base de données le montage outil](https://technet.microsoft.com/library/cc753609\(WS.10\).aspx).

Vous pouvez également utiliser le **ntdsutil instantané** commande pour créer des captures instantanées de la base de données Active Directory. En planifiant une tâche pour créer régulièrement des instantanés, vous pouvez obtenir des copies supplémentaires de la base de données Active Directory au fil du temps. Vous pouvez utiliser ces copies pour mieux identifier lors de l’échec de la forêt s’est produite, puis choisissez la meilleure sauvegarde à restaurer. Pour créer des captures instantanées, utilisez la version de **ntdsutil** qui est fourni avec Windows Server 2008 ou le serveur Administration outils distant (RSAT) pour Windows Vista ou version ultérieure. La contrôleur de domaine cible peut exécuter n’importe quelle version de Windows Server. Pour plus d’informations sur l’utilisation de la **ntdsutil instantané** de commande, consultez [instantané](https://technet.microsoft.com/library/cc731620\(WS.10\).aspx).

## <a name="determining-which-domain-controllers-to-restore"></a>Déterminer les contrôleurs de domaine à restaurer

Faciliter le processus de restauration est un facteur important lorsque vous décidez de contrôleur de domaine à restaurer. Il est recommandé d’avoir un contrôleur de domaine dédié pour chaque domaine qui est le contrôleur de domaine préféré pour une restauration. Une restauration dédiée DC rend plus facile de manière fiable planifier et exécuter la récupération de forêt, car vous utilisez la même configuration de source qui a été utilisée pour effectuer restaurer des tests. Vous pouvez générer un script la récupération et se disputent pas avec des configurations différentes, par exemple si le contrôleur de domaine contienne des opérations de rôles de maître ou non, ou s’il s’agit d’un serveur de catalogue global ou un DNS ou non.

> [!NOTE]
> S’il n’est pas recommandé pour restaurer un détenteur du rôle de maître d’opérations pour des raisons de simplicité, certaines organisations peuvent choisir de restaurer un pour les autres avantages. Restauration par exemple le maître RID peut aider à éviter les problèmes avec la gestion des RID lors de la récupération.  

Choisissez un contrôleur de domaine qui répond le mieux aux critères suivants :

- Un contrôleur de domaine qui est accessible en écriture. Ce champ est obligatoire.

- Un contrôleur de domaine exécutant Windows Server 2012 en tant qu’une machine virtuelle sur un hyperviseur qui prend en charge VM-GenerationID. Ce contrôleur de domaine peut être utilisé en tant que source pour le clonage.
- Un contrôleur de domaine qui est accessible, physiquement ou sur un réseau virtuel et de préférence situé dans un centre de données. De cette façon, vous pouvez facilement isoler à partir du réseau lors de la récupération de forêt.
- Un contrôleur de domaine qui a une sauvegarde de serveur complète. Une bonne sauvegarde est une sauvegarde qui peut être restaurée avec succès, a été effectuée quelques jours avant la défaillance et contient des données plus utiles que possible.
- Un contrôleur de domaine qui était un serveur de système DNS (Domain Name) avant la défaillance. Cette opération enregistre le temps nécessaire pour réinstaller le DNS.
- Si vous utilisez également des Services de déploiement Windows, choisissez un contrôleur de domaine qui n’est pas configuré pour utiliser le déverrouillage réseau BitLocker. Dans ce cas, le déverrouillage réseau BitLocker n’est pas pris en charge à utiliser pour le premier contrôleur de domaine que vous restaurez à partir de la sauvegarde pendant une récupération de forêt.

   Le déverrouillage réseau BitLocker en tant que le *uniquement* protecteur de clé *ne peut pas* être utilisé sur les contrôleurs de domaine dans lequel vous avez déployé les Services de déploiement Windows (WDS), car cela entraîne dans un scénario où le premier contrôleur de domaine nécessite Active Directory et WDS fonctionne pour le déverrouiller. Mais avant de restaurer le premier contrôleur de domaine, Active Directory n’est pas encore disponible pour WDS, donc il ne peut pas déverrouiller.

   Pour déterminer si un contrôleur de domaine est configuré pour utiliser le déverrouillage réseau BitLocker, vérifiez qu’un certificat de déverrouillage réseau est identifié dans la clé de Registre suivante :

   HKEY_LOCAL_MACHINESoftwarePoliciesMicrosoftSystemCertificatesFVE_NKP

Maintenir des procédures de sécurité lors de la gestion des exceptions et de restauration des fichiers de sauvegarde incluant Active Directory. Le degré d’urgence qui accompagne la récupération de forêt peut entraîner par inadvertance surplombant les meilleures pratiques de sécurité. Pour plus d’informations, consultez la section intitulée « Mise en place domaine contrôleur et restaurer les stratégies de sauvegarde » dans [Guide des meilleures pratiques pour sécuriser les Installations Active Directory et les opérations de Day : Partie II](https://technet.microsoft.com/library/bb727066.aspx).

## <a name="identify-the-current-forest-structure-and-dc-functions"></a>Identifier la structure de la forêt actuelle et les fonctions du contrôleur de domaine

Déterminer la structure actuelle de la forêt en identifiant tous les domaines de la forêt. Dressez la liste de tous les contrôleurs de domaine dans chaque domaine, en particulier les contrôleurs de domaine qui ont des sauvegardes et des contrôleurs de domaine virtualisés qui peuvent être une source pour le clonage. Une liste des contrôleurs de domaine pour le domaine racine de forêt sera le plus important, car vous serez d’abord récupérer ce domaine. Après avoir restauré le domaine racine de forêt, vous pouvez obtenir une liste des autres domaines, les contrôleurs de domaine et les sites dans la forêt à l’aide des composants logiciels enfichables d’Active Directory.

Préparer un tableau qui indique les fonctions de chaque contrôleur de domaine dans le domaine, comme indiqué dans l’exemple suivant. Cela vous aidera à revenir à la configuration avant échec de la forêt après la récupération.

|Nom de contrôleur de domaine|Système d’exploitation|FSMO|GC|RODC|Secours|DNS|Server Core|VM|VM-GenID|  
|-------------|----------------------|----------|--------|----------|------------|---------|-----------------|--------|---------------|  
|DC_1|Windows Server 2012|Contrôleur de schéma, maître d’attribution de noms de domaine|Oui|Non|Oui|Non|Non|Oui|Oui|  
|DC_2|Windows Server 2012|Aucune|Oui|Non|Oui|Oui|Non|Oui|Oui|  
|DC_3|Windows Server 2012|Maître d'infrastructure|Non|Non|Non|Oui|Oui|Oui|Oui|  
|DC_4|Windows Server 2012|Émulateur PDC, maître RID|Oui|Non|Non|Non|Non|Oui|Non|  
|DC_5|Windows Server 2012|Aucune|Non|Non|Oui|Oui|Non|Oui|Oui|  
|RODC_1|Windows Server 2008 R2|Aucune|Oui|Oui|Oui|Oui|Oui|Oui|Non|  
|RODC_2|Windows Server 2008|Aucune|Oui|Oui|Non|Oui|Oui|Oui|Non|  

Pour chaque domaine dans la forêt, identifier un contrôleur de domaine accessible en écriture unique qui a une sauvegarde fiable de la base de données Active Directory pour ce domaine. Soyez prudent lorsque vous choisissez une sauvegarde à restaurer un contrôleur de domaine. Si le jour et la cause de l’échec sont connues environ, la recommandation générale consiste à utiliser une sauvegarde qui a été effectuée quelques jours avant cette date.
  
Dans cet exemple, il existe quatre candidats de sauvegarde : DC_1 DC_2, DC_4 et DC_5. De ces candidats de sauvegarde, vous la restaurez uniquement. Le contrôleur de domaine recommandée est DC_5 pour les raisons suivantes :  

- Il répond aux conditions requises pour l’utiliser en tant que source pour le clonage de contrôleur de domaine virtualisés, qui est, il exécute Windows Server 2012 comme un contrôleur de domaine virtuel sur un hyperviseur qui prend en charge VM-GenerationID, exécute le logiciel qui est autorisé à être cloné (ou qui peut être supprimé s’il n’est pas en mesure de clone (d). Après la restauration, le rôle d’émulateur PDC sera remis à que server et il peuvent être ajoutés au groupe de contrôleurs de domaine clonables pour le domaine.  
- Il s’exécute une installation complète de Windows Server 2012. Un contrôleur de domaine qui exécute une installation Server Core peut être moins pratique en tant que cible pour la récupération.  
- Il est un serveur DNS. Par conséquent, DNS ne devra pas être réinstallé.  

> [!NOTE]
> Étant donné que DC_5 n’est pas un serveur de catalogue global, il présente également l’avantage dans la mesure où le catalogue global ne doit-elle pas être supprimées après la restauration. Mais si le contrôleur de domaine est également qu'un serveur de catalogue global n’est pas un facteur décisif car, à compter de Windows Server 2012, tous les contrôleurs de domaine sont des serveurs de catalogue global par défaut et la suppression et ajout du catalogue global, une fois la restauration est recommandée dans le cadre de la forêt le processus de restauration dans tous les cas.  

## <a name="recover-the-forest-in-isolation"></a>Récupérer la forêt de manière isolée

Le scénario par défaut est d’arrêter tous les contrôleurs de domaine accessible en écriture avant le premier contrôleur de domaine restauré est remis en production. Cela garantit que toutes les données dangereuses ne sont pas répliqué dans la forêt récupérée. Il est particulièrement important arrêter tous les détenteurs du rôle de maître d’opérations.  

> [!NOTE]
> Il peut arriver lorsque vous déplacez le premier contrôleur de domaine que vous souhaitez récupérer pour chaque domaine à un réseau isolé, tout en permettant aux autres contrôleurs de domaine de rester en ligne afin de réduire les temps d’arrêt du système. Par exemple, si vous récupérez à partir d’une mise à niveau du schéma a échoué, vous pouvez choisir de conserver les contrôleurs de domaine en cours d’exécution sur le réseau de production pendant que vous effectuez les étapes de récupération de manière isolée.

Si vous utilisez des contrôleurs de domaine virtualisés, vous pouvez les déplacer à un réseau virtuel est isolé du réseau de production où vous allez effectuer la récupération. Déplacement de contrôleurs de domaine virtualisés à un réseau distinct présente deux avantages :  

- Les contrôleurs de domaine restaurés ne peuvent pas se reproduise le problème qui a provoqué la récupération de forêt, car elles sont isolées.  
- Clonage de contrôleur de domaine virtualisé peut être effectué sur le réseau distinct afin qu’un nombre critiques des contrôleurs de domaine peut être en cours d’exécution et testé avant qu’ils sont récupérés dans le réseau de production.

Si les contrôleurs de domaine sont en cours d’exécution sur un matériel physique, déconnectez le câble réseau du premier contrôleur de domaine que vous envisagez de restaurer le domaine racine de forêt. Si possible, également déconnecter les câbles réseau de tous les autres contrôleurs de domaine. Cela empêche des contrôleurs de domaine de réplication, si elles sont démarrées par inadvertance pendant le processus de récupération de forêt.  

Dans une forêt volumineuse qui est répartie sur plusieurs emplacements, il peut être difficile de garantir que tous les contrôleurs de domaine accessible en écriture sont arrêtés. Pour cette raison, les étapes de récupération, telles que la réinitialisation du compte d’ordinateur et le compte krbtgt, en outre pour le nettoyage des métadonnées, sont conçus pour vous assurer que les contrôleurs de domaine accessible en écriture récupérés ne répliquent pas avec des contrôleurs de domaine accessible en écriture dangereux (au cas où certaines sont toujours en ligne dans le forêt).  
  
Toutefois, uniquement par la mise hors connexion de contrôleurs de domaine accessible en écriture pouvez vous garantir que la réplication ne se produit pas. Par conséquent, si possible, vous devez déployer la technologie de gestion à distance qui peut vous aider à arrêter et isoler physiquement les contrôleurs de domaine accessible en écriture lors de la récupération de forêt.  
  
RODC peut continuer à fonctionner en mode hors connexion des contrôleurs de domaine accessible en écriture. Aucun autre contrôleur de domaine directement ne répliquer les modifications à partir de n’importe quel RODC, en particulier, aucune Configuration ou schéma conteneur change, afin qu’ils ne présentent pas les mêmes risques que les contrôleurs de domaine accessible en écriture pendant la récupération. Une fois que tous les contrôleurs de domaine accessible en écriture sont récupérées et en ligne, vous devez recréer toutes les le RODC.  
  
RODC continueront à autoriser l’accès aux ressources locales qui sont mis en cache dans leurs sites respectifs, tandis que les opérations de récupération sont en cours en parallèle. Les ressources locales qui ne sont pas mis en cache sur le RODC aura des requêtes d’authentification envoyées à un contrôleur de domaine accessible en écriture. Ces demandes échouent, car les contrôleurs de domaine accessible en écriture sont hors connexion. Certaines opérations telles que les modifications de mot de passe ne fonctionnent pas jusqu'à ce que vous récupérez des contrôleurs de domaine accessible en écriture.  
  
Si vous utilisez une architecture de réseau hub-and-spoke, vous pouvez vous concentrer tout d’abord sur la récupération des contrôleurs de domaine accessible en écriture dans les sites de concentrateur. Une version ultérieure, vous pouvez reconstruire le RODC dans les sites distants.  
  
## <a name="next-steps"></a>Étapes suivantes

- [Récupération de forêt AD - conditions préalables](AD-Forest-Recovery-Prerequisties.md)  
- [Récupération de forêt AD - concevoir un plan de récupération personnalisée-forêt](AD-Forest-Recovery-Devising-a-Plan.md)  
- [Récupération de forêt AD - identifier le problème](AD-Forest-Recovery-Identify-the-Problem.md)
- [Récupération de forêt AD - déterminer comment récupérer](AD-Forest-Recovery-Determine-how-to-Recover.md)
- [Récupération de forêt AD - effectuer une récupération initiale](AD-Forest-Recovery-Perform-initial-recovery.md)  
- [Récupération de forêt AD - procédures](AD-Forest-Recovery-Procedures.md)  
- [Récupération de forêt AD - Forum aux Questions](AD-Forest-Recovery-FAQ.md)  
- [Récupération de forêt AD - récupération d’un domaine unique au sein d’une forêt Multidomain](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
- [Récupération de forêt AD - récupération de forêt avec les contrôleurs de domaine Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md)  
