---
title: "Récupération de la forêt ActiveDirectory - déterminer comment récupérer la forêt"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adfs
ms.openlocfilehash: cc2525068225644b0964a2726bd9c0dcf2e0cba0
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="determine-how-to-recover-the-forest"></a>Déterminer comment récupérer la forêt  

>S’applique à: Windows Server2016, Windows Server2012 et 2012R2, Windows Server2008 et 2008R2

 Récupération d’un ensemble de la forêt ActiveDirectory implique soit la restauration à partir de la sauvegarde ou la réinstallation des Services de domaine ActiveDirectory (ADDS) sur chaque contrôleur de domaine (DC) dans la forêt. Récupération de la forêt restaure chaque domaine dans la forêt à son état au moment de la dernière sauvegarde approuvée. Par conséquent, l’opération de restauration entraîne la perte d’au moins les données ActiveDirectory suivantes:  
  
-   Tous les objets qui ont été ajoutés après la dernière sauvegarde approuvée (par exemple, les utilisateurs et ordinateurs)  
  
-   Toutes les mises à jour qui ont été apportées aux objets existants dans la mesure où le dernier approuvé sauvegarde  
  
-   Toutes les modifications apportées à la partition de configuration ou de la partition de schéma dans les services ADDS (par exemple, les modifications de schéma) depuis la dernière sauvegarde approuvée  
  
 Pour chaque domaine dans la forêt, le mot de passe d’un compte d’administrateur de domaine doit être connu. Il s’agit de préférence, le mot de passe du compte administrateur intégré, qui ne doit pas être désactivé. Vous devez également connaître le mot de passe DSRM pour effectuer une restauration d’état du système d’un contrôleur de domaine. En règle générale, il est recommandé d’archiver le compte d’administrateur et l’historique du mot de passe DSRM en lieu sûr pour tant que les sauvegardes sont valides, autrement dit, dans la période de durée de vie de temporisation ou dans l’objet supprimé la durée de vie si la Corbeille ActiveDirectory est activée. Vous pouvez également synchroniser le mot de passe DSRM avec un compte d’utilisateur de domaine afin de rendre plus facile à mémoriser. Pour plus d’informations, voir l’article [961320](https://support.microsoft.com/kb/961320). Synchroniser le compte DSRM doit être effectuée avant la récupération de forêt, dans le cadre de la préparation.  
  
> [!NOTE]
>  Le compte administrateur est un membre du groupe Administrateurs intégré par défaut, tout comme les groupes Admins du domaine et administrateurs de l’entreprise. Ce groupe possède le contrôle total sur tous les contrôleurs de domaine dans le domaine.  
  
## <a name="determining-which-backups-to-use"></a>Déterminer les sauvegardes à utiliser  
 Sauvegardez au moins deux contrôleurs de domaine accessible en écriture pour chaque domaine régulièrement afin d’avoir plusieurs sauvegardes à choisir. Notez que vous ne pouvez pas utiliser la sauvegarde d’un contrôleur de domaine en lecture seule (RODC) pour restaurer un contrôleur de domaine accessible en écriture. Nous vous conseillons de restaurer les contrôleurs de domaine à l’aide de sauvegardes qui ont été effectuées quelques jours avant l’occurrence de l’échec. En règle générale, vous devez déterminer un compromis entre l’ancienneté et le safeness des données restaurées. Choix d’une sauvegarde plus récente récupère les données plus utiles, mais cela peut augmenter le risque de réintroduire des données dangereuses dans la forêt restaurée.  
  
 La restauration de sauvegardes de l’état système repose sur le serveur de la sauvegarde et le système d’exploitation d’origine. Par exemple, vous ne devez pas restaurer une sauvegarde de l’état système vers un autre serveur. Dans ce cas, l’avertissement suivant peut s’afficher:  
  
 «La sauvegarde spécifiée est d’un serveur différent de celui en cours. Nous vous déconseillons d’effectuer une récupération de l’état système avec la sauvegarde sur un autre serveur, car le serveur peut devenir inutilisable. Vous êtes sûr de que vouloir utiliser cette sauvegarde pour récupérer le serveur actuel?»  
  
 Si vous avez besoin pour la restauration d’ActiveDirectory sur un autre matériel, créer des sauvegardes de serveur complète et prévoyez d’effectuer une récupération complète du serveur.  
  
> [!IMPORTANT]
>  À compter de Windows Server2008, il n'est pas pris en charge pour restaurer la sauvegarde de l’état système à une nouvelle installation de Windows Server sur le nouveau matériel ou le même matériel. Si Windows Server est réinstallé sur le même matériel, comme indiqué plus loin dans ce guide, vous pouvez restaurer le contrôleur de domaine dans cet ordre:  
>   
>  1.  Effectuer une restauration complète du serveur afin de restaurer le système d’exploitation et tous les fichiers et applications.  
> 2.  Effectuer une restauration d’état du système à l’aide de wbadmin.exe afin de marquer SYSVOL comme faisant autorité.  
>   
>  Pour plus d’informations, voir Microsoft l’article [249694](https://support.microsoft.com/kb/249694).  
  
 Si l’heure de l’occurrence de l’échec est inconnu, recherchez davantage pour identifier les sauvegardes qui contiennent le dernier état de sécurité de la forêt. Cette approche est moins souhaitable. Par conséquent, nous vous conseillons vivement de conserver les journaux détaillés sur l’état d’intégrité des services ADDS sur une base quotidienne afin que, s’il existe un échec de la forêt, le temps approximatif de défaillance peut être identifié. Vous devez également garder une copie locale des sauvegardes pour activer la récupération plus rapide.  
  
 Si la Corbeille ActiveDirectory est activée, la durée de vie de sauvegarde est égale à la **deletedObjectLifetime** valeur ou la **tombstoneLifetime** valeur, si elle est inférieure. Pour plus d’informations, voir [la Corbeille ActiveDirectory Step-by-Step Guide](https://go.microsoft.com/fwlink/?LinkId=178657) (https://go.microsoft.com/fwlink/?LinkId=178657).  
  
 Comme alternative, vous pouvez également utiliser le (Dsamain.exe) ActiveDirectory de base de données montage outil et un outil Active accès protocole LDAP (Lightweight), tels que Ldp.exe ou utilisateurs ActiveDirectory et les ordinateurs, pour identifier quelle sauvegarde inclut le dernier état de sécurité de la forêt. L’outil de montage de base de données ActiveDirectory, qui est inclus dans Windows Server2008 et Windows Server versions ultérieures, expose les données ActiveDirectory qui sont stockées dans les sauvegardes ou les captures instantanées en tant qu’un serveur LDAP. Ensuite, vous pouvez utiliser un outil LDAP pour parcourir les données. Cette approche présente l’avantage de ne pas exiger de redémarrage de n’importe quel contrôleur de domaine dans Services restaurer Mode annuaire (DSRM) pour examiner le contenu de la sauvegarde des services ADDS.  
  
 Pour plus d’informations sur l’utilisation de la base de données ActiveDirectory outil de montage, voir la [outil de montage de base de données ActiveDirectory Step-by-Step Guide](https://technet.microsoft.com/library/cc753609\(WS.10\).aspx).  
  
 Vous pouvez également utiliser le **ntdsutil instantané** commande pour créer des captures instantanées de la base de données ActiveDirectory. En planifiant une tâche à créer régulièrement des captures instantanées, vous pouvez obtenir des copies supplémentaires de la base de données ActiveDirectory au fil du temps. Vous pouvez utiliser ces copies pour mieux identifier lors de l’échec de la forêt, puis choisissez la meilleure sauvegarde à restaurer. Pour créer des captures instantanées, utilisez la version de **ntdsutil** qui est fourni avec Windows Server2008 ou le serveur Administration outils (distant) pour WindowsVista ou version ultérieure. Le contrôleur de domaine cible peut exécuter n’importe quelle version de Windows Server. Pour plus d’informations sur l’utilisation de la **ntdsutil instantané** de commande, voir [instantané](https://technet.microsoft.com/library/cc731620\(WS.10\).aspx).  
  
  
## <a name="determining-which-domain-controllers-to-restore"></a>Déterminer quels contrôleurs de domaine à restaurer  
 Options d’ergonomie le processus de restauration sont un facteur important lorsque vous décidez de contrôleur de domaine à restaurer. Il est recommandé de disposer d’un contrôleur de domaine dédié pour chaque domaine qui est le contrôleur de domaine par défaut pour une restauration. Une contrôleur de domaine rend plus facile de manière fiable planifier et exécuter la récupération de la forêt, car vous utilisez la même configuration source qui a été utilisée pour effectuer de restauration dédiée restaurer des tests. Vous pouvez la récupération de script et pas le contenu avec des configurations différentes, telles que si le contrôleur de domaine contienne des opérations de rôles de maître ou non, ou qu’il s’agisse d’un serveur de catalogue global ou DNS ou non.  
  
> [!NOTE]
>  Alors que cela n’est pas recommandé pour restaurer un détenteur du rôle de maître d’opérations pour des raisons de simplicité, certaines organisations peuvent choisir de restaurer un des autres avantages. Par exemple la restauration du maître RID peut contribuer à éviter des problèmes avec la gestion des identificateurs RID pendant la récupération.  
  
 Choisir un contrôleur de domaine qui répond le mieux aux critères suivants:  
  
-   Un contrôleur de domaine qui est accessible en écriture. Ce champ est obligatoire.  
  
-   Un contrôleur de domaine exécutant Windows Server2012 en tant qu’un ordinateur virtuel sur un hyperviseur qui prend en charge VM-GenerationID. Ce contrôleur de domaine peut être utilisé en tant que source pour le clonage.  
  
-   Un contrôleur de domaine qui est accessible, physiquement ou sur un réseau virtuel et, de préférence, situé dans un centre de données. De cette façon, vous pouvez facilement isoler à partir du réseau lors de la récupération de forêt.  
  
-   Un contrôleur de domaine qui dispose d’une sauvegarde de serveur complète. Une bonne sauvegarde est une sauvegarde qui peut être restaurée avec succès, a été effectuée quelques jours avant la défaillance et contient des données très utiles que possible.  
  
-   Un contrôleur de domaine qui était un serveur de système DNS (Domain Name) avant la défaillance. Cela permet d’économiser du temps nécessaire à la réinstallation DNS.  
  
-   Si vous utilisez également les Services de déploiement Windows, cliquez sur un contrôleur de domaine qui n’est pas configuré pour utiliser le déverrouillage réseau BitLocker. Dans ce cas, le déverrouillage réseau BitLocker n’est pas prise en charge pour être utilisé pour le premier contrôleur de domaine que vous restaurez à partir de la sauvegarde pendant une récupération de forêt.  
  
     Le déverrouillage réseau BitLocker en tant que le *uniquement* protecteur de clé *ne peut pas* être utilisé sur les contrôleurs de domaine dans lequel vous avez déployé les Services de déploiement Windows (WDS), car cela entraîne dans un scénario où nécessite que le premier contrôleur de domaine ActiveDirectory et WDS fonctionne pour le déverrouiller. Mais avant de restaurer le premier contrôleur de domaine, ActiveDirectory n’est pas encore disponible pour les services de déploiement Windows, afin qu’il ne peut pas déverrouiller.  
  
     Pour déterminer si un contrôleur de domaine est configuré pour utiliser le déverrouillage réseau BitLocker, vérifiez qu’un certificat de déverrouillage réseau est identifié dans la clé de Registre suivante:  
  
     HKEY_LOCAL_MACHINESoftwarePoliciesMicrosoftSystemCertificatesFVE_NKP  
  
 Mettre à jour les procédures de sécurité lors de la gestion ou de la restauration des fichiers de sauvegarde qui incluent ActiveDirectory. L’urgence qui accompagne la récupération de la forêt peut entraîner par inadvertance surplombant les meilleures pratiques de sécurité. Pour plus d’informations, consultez la section intitulée «Établissement domaine contrôleur et restaurer des stratégies de sauvegarde» dans [Guide des meilleures pratiques pour sécuriser les Installations active et les opérations de Day: partie](https://technet.microsoft.com/library/bb727066.aspx).  
  
## <a name="identify-the-current-forest-structure-and-dc-functions"></a>Identifier la structure de la forêt actuelle et les fonctions de contrôleur de domaine  
 Déterminer la structure de la forêt actuelle en identifiant tous les domaines dans la forêt. Créer une liste de tous les contrôleurs de domaine dans chaque domaine, notamment les contrôleurs de domaine qui ont des sauvegardes et des contrôleurs de domaine virtualisés qui peuvent être une source pour le clonage. Une liste de contrôleurs de domaine pour le domaine racine de forêt sera le plus important, car vous serez d’abord récupérer ce domaine. Après avoir restauré le domaine racine de forêt, vous pouvez obtenir une liste des autres domaines, les contrôleurs de domaine et les sites de la forêt à l’aide des composants logiciels enfichables d’ActiveDirectory.  
  
 Préparer une table qui affiche les fonctions de chaque contrôleur de domaine dans le domaine, comme illustré dans l’exemple suivant. Cela vous aidera à revenir à la configuration préalable d’échec de la forêt après la récupération.  
  
|Nom du contrôleur de domaine|Système d'exploitation|FSMO|DANS LE CATALOGUE GLOBAL|RODC|Sauvegarde|DNS|Server Core|ORDINATEUR VIRTUEL|VM-GenID|  
|-------------|----------------------|----------|--------|----------|------------|---------|-----------------|--------|---------------|  
|DC_1|Windows Server2012|Contrôleur de schéma, maître d’attribution de noms de domaine|Oui|N°|Oui|N°|N°|Oui|Oui|  
|DC_2|Windows Server2012|None|Oui|N°|Oui|Oui|N°|Oui|Oui|  
|DC_3|Windows Server2012|Maître d’infrastructure|N°|N°|N°|Oui|Oui|Oui|Oui|  
|DC_4|Windows Server2012|Émulateur PDC, maître RID|Oui|N°|N°|N°|N°|Oui|N°|  
|DC_5|Windows Server2012|None|N°|N°|Oui|Oui|N°|Oui|Oui|  
|RODC_1|Windows Server2008R2|None|Oui|Oui|Oui|Oui|Oui|Oui|N°|  
|RODC_2|Windows Server2008|None|Oui|Oui|N°|Oui|Oui|Oui|N°|  
  
 Pour chaque domaine dans la forêt, identifier un contrôleur de domaine accessible en écriture seule disposant d’une sauvegarde de la base de données ActiveDirectory pour ce domaine approuvée. Soyez prudent lorsque vous effectuez une sauvegarde pour restaurer un contrôleur de domaine. Si le jour et la cause de l’échec sont connus environ, la recommandation générale consiste à utiliser une sauvegarde qui a été effectuée quelques jours avant cette date.  
  
 Dans cet exemple, il existe quatre candidats à la sauvegarde: DC_1, DC_2, DC_4 et DC_5. De ces candidats à la sauvegarde, vous ne restaurez qu’un seul. Le contrôleur de domaine recommandée est DC_5 pour les raisons suivantes:  
  
-   Il répond aux conditions requises pour l’utiliser en tant que source pour le clonage de contrôleur de domaine virtualisé, qui est, il exécute Windows Server2012 comme un contrôleur de domaine virtuel sur un hyperviseur que prend en charge VM-GenerationID, exécute logiciel qui est autorisé à être cloné (ou qui peuvent être supprimées si elle n’est pas en mesure d’être cloné). Après la restauration, le rôle d’émulateur PDC est remis à serveur et peuvent être ajoutés au groupe de contrôleurs de domaine clonables pour le domaine.  
  
-   Il s’exécute une installation complète de Windows Server2012. Un contrôleur de domaine qui exécute une installation Server Core peut être moins pratique en tant que cible de récupération.  
  
-   Il est un serveur DNS. Par conséquent, les DNS n’a pas à être réinstallé.  
  
> [!NOTE]
>  DC_5 n’étant pas un serveur de catalogue global, il présente également un avantage dans la mesure où le catalogue global n’a pas besoin d’être supprimées après la restauration. Mais si le contrôleur de domaine est également qu'un serveur de catalogue global n’est pas un facteur déterminant, car à partir de Windows Server2012, tous les contrôleurs de domaine sont des serveurs de catalogue global par défaut et la suppression et ajout du catalogue global après que la restauration est recommandée dans le cadre du processus de récupération de forêt dans tous les cas.  
  
## <a name="recover-the-forest-in-isolation"></a>Récupération de la forêt d’isolation  
 Le scénario par défaut est d’arrêter tous les contrôleurs de domaine accessible en écriture avant le premier contrôleur de domaine restauré est rétablie production. Cela garantit que toutes les données dangereuses ne répliquent pas dans la forêt récupérée. Il est particulièrement important arrêter tous les détenteurs du rôle de maître d’opérations.  
  
> [!NOTE]
>  Il peut arriver lorsque vous déplacez le premier contrôleur de domaine que vous envisagez de récupération pour chaque domaine pour un réseau isolé, tout en permettant aux autres contrôleurs de domaine rester en ligne afin de réduire les temps d’arrêt du système. Par exemple, si vous récupérez à partir d’une mise à niveau du schéma a échoué, vous pouvez choisir de conserver des contrôleurs de domaine en cours d’exécution sur le réseau de production pendant que vous effectuez la procédure de récupération de manière isolée.  
  
 Si vous utilisez des contrôleurs de domaine virtualisés, vous pouvez les déplacer à un réseau virtuel qui est isolé du réseau de production où vous allez effectuer la récupération. Déplacement des contrôleurs de domaine virtualisés à un réseau distinct présente deux avantages:  
  
-   Contrôleurs de domaine restaurés ne sont pas autorisés à partir de répétition du problème qui a provoqué la récupération de la forêt, car elles sont isolées.  
  
-   Le clonage de contrôleur de domaine virtualisé peut être effectué sur le réseau distinct afin qu’un nombre critiques de contrôleurs de domaine peut être en cours d’exécution et testé avant qu’ils sont ramenés au réseau de production.  
  
 Si vous utilisez des contrôleurs de domaine sur du matériel physique, déconnectez le câble réseau du premier contrôleur de domaine que vous souhaitez restaurer dans le domaine racine de forêt. Si possible, également déconnecter les câbles réseau de tous les autres contrôleurs de domaine. Cela empêche les contrôleurs de domaine à partir de la réplication, si elles sont démarrées par inadvertance pendant le processus de récupération de forêt.  
  
 Dans une forêt importante qui s’étend sur plusieurs sites, il peut être difficile de garantir que tous les contrôleurs de domaine accessible en écriture sont arrêtés. Pour cette raison, la procédure de récupération, telles que la réinitialisation du compte d’ordinateur et le compte krbtgt, de plus de nettoyage des métadonnées: sont conçus pour vous assurer que les contrôleurs de domaine accessible en écriture récupérés ne répliquent pas avec les contrôleurs de domaine accessible en écriture dangereux (au cas où certains sont toujours en ligne dans la forêt).  
  
 Toutefois, uniquement par la mise hors ligne des contrôleurs de domaine accessible en écriture pourrez vous garantir que la réplication ne se produit pas? Par conséquent, chaque fois que possible, vous devez déployer la technologie de gestion à distance qui peut vous aider à arrêter et d’isoler physiquement les contrôleurs de domaine accessible en écriture lors de la récupération de forêt.  
  
 RODC peut continuer à fonctionner en mode hors connexion des contrôleurs de domaine accessible en écriture. Aucun autre contrôleur de domaine directement ne répliquer les modifications à partir de n’importe quel RODC, en particulier, aucune modification de conteneur schéma ou de Configuration, afin qu’ils ne présentent pas les mêmes risques que les contrôleurs de domaine accessible en écriture lors de la récupération. Une fois que tous les contrôleurs de domaine accessible en écriture sont récupérés et en ligne, vous devez recréer toutes les le RODC.  
  
 RODC continueront à autoriser l’accès aux ressources locales qui sont mises en cache leurs sites respectifs pendant les opérations de récupération sont en parallèle. Les ressources locales qui ne sont pas mises en cache sur le RODC auront les demandes d’authentification transmis à un contrôleur de domaine accessible en écriture. Ces demandes échoue, car les contrôleurs de domaine accessible en écriture sont hors connexion. Certaines opérations telles que les modifications de mot de passe ne fonctionnent pas jusqu'à ce que vous récupérez des contrôleurs de domaine accessible en écriture.  
  
 Si vous utilisez une architecture hub and spoke réseau, vous pouvez vous concentrer tout d’abord sur la récupération des contrôleurs de domaine accessible en écriture dans les sites hub. Une version ultérieure, vous pouvez recréer le RODC dans des sites distants.  
  
## <a name="next-steps"></a>Étapes suivantes
-   [Récupération de la forêt ActiveDirectory - Configuration requise](AD-Forest-Recovery-Prerequisties.md)  
-   [Récupération de la forêt ActiveDirectory - concevoir un plan de récupération de forêt personnalisé](AD-Forest-Recovery-Devising-a-Plan.md)  
- [Récupération de la forêt ActiveDirectory - identifier le problème](AD-Forest-Recovery-Identify-the-Problem.md)
-   [Récupération de la forêt ActiveDirectory - déterminer comment récupérer](AD-Forest-Recovery-Determine-how-to-Recover.md)
-   [Récupération de la forêt ActiveDirectory - effectuer une récupération initiale](AD-Forest-Recovery-Perform-initial-recovery.md)  
-   [Récupération de la forêt ActiveDirectory - procédures](AD-Forest-Recovery-Procedures.md)  
-   [Récupération de la forêt ActiveDirectory - Forum aux Questions](AD-Forest-Recovery-FAQ.md)  
-   [Récupération de la forêt ActiveDirectory - récupération d’un domaine unique au sein d’une forêt Multidomain](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
-   [Récupération de la forêt ActiveDirectory - récupération de forêt avec des contrôleurs de domaine Windows Server2003](AD-Forest-Recovery-Windows-Server-2003.md)  
