---
title: "Récupération de la forêt ActiveDirectory - effectuer une récupération initiale"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adfs
ms.openlocfilehash: fc2ec09c5b96b76229d532adc6a254c8108d8940
ms.sourcegitcommit: ac73f0f0dca04be731b928183bfdffc97d82c277
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/02/2018
---
# <a name="perform-initial-recovery"></a>Effectuer une récupération initiale  

>S’applique à: Windows Server2016, Windows Server2012 et 2012R2, Windows Server2008 et 2008R2

 Cette section comprend les étapes suivantes:  
  
-   [Restaurer le premier contrôleur de domaine accessible en écriture dans chaque domaine](#Restore-the-first-writeable-domain-controller-in-each-domain)  

-   [Vous reconnecter chaque contrôleur de domaine accessible en écriture restauré au réseau](#Reconnect-each-restored-writeable-domain-controller-to-a-common-network)  
  
-   [Ajouter le catalogue global à un contrôleur de domaine dans le domaine racine de forêt](#Add-the-global-catalog-to-a-domain-controller-in-the-forest-root-domain)  
  
  
## <a name="restore-the-first-writeable-domain-controller-in-each-domain"></a>Restaurer le premier contrôleur de domaine accessible en écriture dans chaque domaine  
 À partir d’un contrôleur de domaine accessible en écriture dans le domaine racine de forêt, effectuez les étapes décrites dans cette section pour restaurer le premier contrôleur de domaine. Le domaine racine de forêt est important car il stocke les groupes Administrateurs du schéma et administrateurs de l’entreprise. Il permet également de mettre à jour de la hiérarchie d’approbation de la forêt. En outre, le domaine racine de forêt conserve généralement le serveur de racine DNS pour l’espace de noms DNS de la forêt. Par conséquent, la zone DNS intégrées à ActiveDirectory pour ce domaine contienne les enregistrements de ressource alias (CNAME) pour tous les autres contrôleurs de domaine dans la forêt (qui sont requis pour la réplication) et les enregistrements de ressource DNS de catalogue global.  
  
 Après avoir récupéré le domaine racine de forêt, répétez la même procédure pour récupérer les domaines restants dans la forêt. Vous pouvez récupérer plusieurs domaines simultanément; toutefois, toujours récupérer un domaine parent avant de récupérer un enfant pour éviter une interruption dans la hiérarchie d’approbation ou de la résolution de noms DNS.  
  
 Pour chaque domaine que vous récupérez, ne restaurez qu’un seul contrôleur de domaine accessible en écriture à partir de la sauvegarde. Il s’agit de la partie la plus importante de la récupération, car le contrôleur de domaine doit disposer d’une base de données qui ne soit pas influencé par tout ce qui a provoqué la forêt échec. Il est important de disposer d’une sauvegarde de confiance est testée avant d’être introduit dans l’environnement de production.  
  
 Puis procédez comme suit. Procédures pour exécuter certaines étapes sont dans [récupération de forêt ActiveDirectory - procédures ](AD-Forest-Recovery-Procedures.md).  
  
1.  Si vous envisagez de restaurer un serveur physique, assurez-vous que le câble réseau de la cible de contrôleur de domaine n’est pas attaché et par conséquent n’est pas connecté au réseau de production. Pour un ordinateur virtuel, vous pouvez supprimer la carte réseau ou utiliser une carte réseau qui est attachée à un autre réseau où vous pouvez tester le processus de récupération pendant isolé du réseau de production.  
  
2.  Dans la mesure où il s’agit du premier contrôleur de domaine accessible en écriture dans le domaine, vous devez effectuer une restauration ne faisant pas autoritée des services ADDS et une restauration faisant autorité de SYSVOL. L’opération de restauration doit être effectuée à l’aide d’une sauvegarde ActiveDirectory prenant en charge et restaurer l’application, telles que la sauvegarde Windows Server (autrement dit, vous ne devez pas restaurer le contrôleur de domaine à l’aide des méthodes non pris en charge telles que la restauration d’un instantané d’ordinateur virtuel).  
  
     Une restauration faisant autorité de SYSVOL est obligatoire car la réplication de SYSVOL répliqué dossier doit être démarré après la récupération d’urgence. Tous les contrôleurs de domaine suivants sont ajoutés dans le domaine doivent resynchroniser leur dossier SYSVOL avec une copie du dossier qui a été sélectionné pour être faisant autorité avant que le dossier peut être publié.  
  
    > [!CAUTION]
    >  Effectuer une opération de restauration faisant autorité (ou principal) de SYSVOL uniquement pour le premier contrôleur de domaine peut être restauré dans le domaine racine de forêt. Incorrectement opérations de restauration principale de SYSVOL sur les autres contrôleurs de domaine conduit à des conflits de réplication de données SYSVOL.  
  
     Il existe deux options effectuer une restauration ne faisant pas autoritée des services ADDS et une restauration faisant autorité de SYSVOL:  
  
    -   Effectuer une récupération complète du serveur et puis puis forcez une synchronisation faisant autoritée de SYSVOL. Pour obtenir des procédures détaillées, voir [exécution d’une récupération complète du serveur](AD-Forest-Recovery-Perform-a-Full-Recovery.md) et [effectuer une synchronisation faisant autoritée de SYSVOL répliqué DFSR ](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md).  
  
    -   Effectuer une récupération complète du serveur suivie d’une restauration d’état du système. Cette option nécessite que vous créez à l’avance les deux types de sauvegardes: une sauvegarde complète du serveur et une sauvegarde de l’état système. Pour obtenir des procédures détaillées, voir [exécution d’une récupération complète du serveur](AD-Forest-Recovery-Perform-a-Full-Recovery.md) et [effectuer une restauration ne faisant pas autoritée des Services de domaine ActiveDirectory ](AD-Forest-Recovery-Nonauthoritative-Restore.md).  
  
3.  Après avoir restauré et redémarrez le contrôleur de domaine accessible en écriture, vérifiez que l’échec n’a pas affecté les données sur le contrôleur de domaine. Si les données de contrôleur de domaine sont endommagées, puis répétez l’étape2 avec une autre sauvegarde.  
  
     Si le contrôleur de domaine héberge un rôle de maître d’opérations, vous devrez peut-être ajouter l’entrée de Registre suivante pour éviter les services ADDS n’est ne pas disponible jusqu'à ce qu’il a terminé la réplication d’une partition d’annuaire accessible en écriture:  
  
     **HKLM\System\CurrentControlSet\Services\NTDS\Parameters\Repl effectuer des synchronisations initiales**  
  
     Créer l’entrée avec le type de données **REG_DWORD** et la valeur **0**. Une fois la forêt est récupérée complètement, vous pouvez réinitialiser la valeur de cette entrée **1**, qui requiert un contrôleur de domaine redémarre et conserve des rôles de maître d’opérations réussie ADDS entrant et sortante réplication avec ses partenaires de réplication connus avant qu’il publie lui-même en tant que contrôleur de domaine et commence à fournir des services aux clients. Pour plus d’informations sur la configuration requise de la synchronisation initiale, voir l’article [305476](https://support.microsoft.com/kb/305476).  
  
     Passez aux étapes suivantes une fois que vous restaurez et vérifiez les données et avant de joindre cet ordinateur au réseau de production.  
  
4.  Si vous pensez que l’échec de la forêt a été liée à des intrusions réseau ou les attaques malveillantes, réinitialisez les mots de passe pour tous les comptes d’administration, y compris les membres des administrateurs de l’entreprise, Admins du domaine, administrateurs du schéma, opérateurs de serveur, les groupes Opérateurs de compte et ainsi de suite. La réinitialisation des mots de passe de compte d’administration doit être effectuée avant de domaine supplémentaire contrôleurs sont installés lors de la phase de la récupération de forêt.  
  
5.  Sur le premier contrôleur de domaine restauré dans le domaine racine de forêt, prenez tous les rôles de maître d’opérations au niveau du domaine et de forêt. Informations d’identification des administrateurs de l’entreprise et administrateurs du schéma sont nécessaires pour prendre les rôles de maître d’opérations de forêt.  
  
     Dans chaque domaine enfant, prenez les rôles de maître d’opérations à l’échelle du domaine. Bien que vous posez conserver les rôles de maître d’opérations sur le contrôleur de domaine restauré que temporairement, prise de ces rôles garantit concernant lequel contrôleur de domaine héberge les à ce stade dans le processus de récupération de forêt. Dans le cadre du processus d’après la récupération, vous pouvez redistribuer les rôles de maître d’opérations en fonction des besoins. Pour plus d’informations sur la prise des rôles de maître d’opérations, voir [prendre un rôle de maître d’opérations](AD-forest-recovery-seizing-operations-master-role.md). Pour obtenir des recommandations sur le placement des rôles de maître d’opérations, voir [quelles sont les maîtres d’opérations?](https://technet.microsoft.com/en-us/library/cc779716.aspx).  
  
6.  Nettoyage des métadonnées de tous les autres contrôleurs de domaine accessible en écriture dans le domaine racine de forêt que vous ne restaurez pas de sauvegarde (tous les contrôleurs du domaine à l’exception de ce premier contrôleur de domaine accessible en écriture). Si vous utilisez la version d’utilisateurs ActiveDirectory et les ordinateurs ou les Sites ActiveDirectory et Services qui est inclus avec Windows Server2008 ou version ultérieure ou serveur distant pour WindowsVista ou version ultérieure, nettoyage des métadonnées est effectué automatiquement lorsque vous supprimez un objet contrôleur de domaine. En outre, l’objet serveur et un objet ordinateur pour le contrôleur de domaine supprimé sont également supprimés automatiquement. Pour plus d’informations, voir [nettoyage des métadonnées de supprimé des contrôleurs de domaine accessible en écriture](AD-Forest-Recovery-Cleaning-Metadata.md).  
  
     Nettoyage des métadonnées empêche les dupliquer des objets Paramètres NTDS si les services ADDS est installé sur un contrôleur de domaine dans un autre site. Potentiellement, cela peut également enregistrer le vérificateur de cohérence des connaissances (KCC) le processus de création des liens de réplication lorsque les contrôleurs de domaine eux-mêmes peut ne pas être présents. En outre, dans le cadre de nettoyage des métadonnées, les enregistrements de ressource DNS du localisateur du contrôleur de domaine pour tous les autres contrôleurs de domaine dans le domaine disparaîtront de DNS.  
  
     Jusqu'à ce que les métadonnées de tous les autres contrôleurs de domaine dans le domaine sont supprimée, ce contrôleur de domaine, s’il s’agissait d’un maître d’opérations RID avant la récupération, considérera pas le rôle de maître RID et par conséquent pas sera en mesure d’émettre de RID de nouveau. Vous pouvez voir les ID d’événement 16650dans le journal système dans l’Observateur d’événements indiquant que cet échec, mais vous devez voir l’ID d’événement 16648 indiquant une réussite temps après avoir nettoyé les métadonnées.  
  
7.  Si vous avez des zones DNS qui sont stockées dans ADDS, assurez-vous que le service serveur DNS local est installé et en cours d’exécution sur le contrôleur de domaine que vous avez restauré. Si ce contrôleur de domaine n’est pas un serveur DNS avant la défaillance de la forêt, vous devez installer et configurer le serveur DNS.  
  
    > [!NOTE]
    >  Si le contrôleur de domaine restauré exécute Windows Server2008, vous devez installer le correctif logiciel dans l’article [975654](https://support.microsoft.com/kb/975654) ou le serveur à un réseau isolé temporairement afin d’installer le serveur DNS. Le correctif n’est pas requis pour les autres versions de Windows Server.  
  
     Dans le domaine racine de forêt, configurez le contrôleur de domaine restauré avec sa propre adresse IP (ou une adresse de bouclage, tels que 127.0.0.1) en tant que serveur de DNS préféré de. Vous pouvez configurer ce paramètre dans les propriétés TCP/IP de la carte réseau local (LAN). Il s’agit du premier serveur DNS dans la forêt. Pour plus d’informations, voir [configurer TCP/IP pour utiliser DNS](https://technet.microsoft.com/library/cc779282\(WS.10\).aspx).  
  
     Dans chaque domaine enfant, configurez le contrôleur de domaine restauré avec l’adresse IP du premier serveur DNS dans le domaine racine de forêt en tant que serveur de DNS préféré de. Vous pouvez configurer ce paramètre dans les propriétés TCP/IP de la carte réseau. Pour plus d’informations, voir [configurer TCP/IP pour utiliser DNS](https://technet.microsoft.com/library/cc779282\(WS.10\).aspx).  
  
     Dans les zones DNS _msdcs et le domaine, supprimez les enregistrements NS des contrôleurs de domaine qui n’existent plus après le nettoyage des métadonnées. Vérifiez si des contrôleurs de domaine nettoyés des enregistrements SRV ont été supprimés. Pour vous aider à accélérer la suppression de l’enregistrement SRV DNS, exécutez:  
  
    ```  
    nltest.exe /dsderegdns:server.domain.tld  
    ```  
  
8.  Augmenter la valeur du pool RID disponible en 100000. Pour plus d’informations, voir [augmenter la valeur de pools RID disponibles](AD-Forest-Recovery-Raise-RID-Pool.md). Si vous avez des raisons de croire qu’augmenter le Pool RID 100000 est insuffisant pour votre cas particulier, vous devez déterminer l’augmentation de la plus basse est toujours utiliser en toute sécuritée. Identificateurs RID est une ressource finie qui ne doit pas être utilisée d’inutilement.  
  
     Si de nouvelles entités de sécurité ont été créées dans le domaine après l’heure de la sauvegarde que vous utilisez pour la restauration, ces entités de sécurité peuvent avoir des droits d’accès sur certains objets. Ces entités de sécurité n’existent plus après la récupération, car la récupération a été rétabli à la sauvegarde; toutefois, leurs droits d’accès peuvent encore exister. Si le pool RID disponible n’est pas déclenché après une restauration, nouvel utilisateur qui sont créés après que la récupération de la forêt peut obtenir l’ID de sécurité sont identiques (SID) des objets et pourrait avoir accès à ces objets, ce qui n’a pas été conçu à l’origine.  
  
     Pour illustrer, examinez l’exemple de l’employé de nouveau nommé Amy qui a été mentionnée dans l’introduction. L’objet utilisateur pour Amy n’existe plus après l’opération de restauration, car il a été créé après la sauvegarde qui a été utilisée pour restaurer le domaine. Toutefois, les droits d’accès qui ont été affectés à cet objet de l’utilisateur peuvent conserver après l’opération de restauration. Si le SID pour cet objet utilisateur est réaffecté à un nouvel objet après l’opération de restauration, le nouvel objet serait obtenir ces droits d’accès.  
  
9. Invalider le pool RID actuel. Le pool RID actuel est invalidé après la restauration d’un état système. Toutefois, si une restauration d’état du système n’a pas été effectuée, le pool RID actuel doit être invalidé pour empêcher le contrôleur de domaine restauré à partir de la nouvelle émission RID à partir du pool RID qui a été affecté au moment de que la sauvegarde a été créée. Pour plus d’informations, voir [invalidant le pool RID actuel](AD-Forest-Recovery-Invaildate-RID-Pool.md).  
  
    > [!NOTE]
    >  La première fois que vous tentez de créer un objet avec un identificateur de sécurité une fois que vous invalidez le pool RID, vous recevrez une erreur. La tentative de création d’un objet déclenche une demande d’un nouveau pool RID. Nouvelle tentative de l’opération réussit, car le nouveau pool RID sera alloué.  
  
10. Réinitialiser le mot de passe du compte ordinateur de ce contrôleur de domaine deux fois. Pour plus d’informations, voir [réinitialiser le mot de passe du compte ordinateur du contrôleur de domaine](AD-Forest-Recovery-Reset-Computer-Account-DC.md).  
  
11. Réinitialiser le mot de passe krbtgt deux fois. Pour plus d’informations, voir [réinitialiser le mot de passe krbtgt](AD-Forest-Recovery-Resetting-the-krbtgt-password.md).  
  
     L’historique du mot de passe krbtgt étant deux mots de passe, réinitialisez les mots de passe deux fois pour supprimer le mot de passe (prefailure) d’origine de l’historique du mot de passe.  
  
    > [!NOTE]
    >  Si la récupération de forêt est en réponse à une violation de la sécurité, vous pouvez également réinitialiser les mots de passe d’approbation. Pour plus d’informations, voir [réinitialiser un mot de passe d’approbation d’un côté de l’approbation](AD-Forest-Recovery-Reset-Trust.md).  
  
12. Si la forêt a plusieurs domaines et le contrôleur de domaine restauré a été avant la défaillance du serveur de catalogue global, désactivez le **catalogue Global** case à cocher dans les propriétés des paramètres NTDS pour supprimer le catalogue global du contrôleur de domaine. L’exception à cette règle est le cas courant d’une forêt avec un domaine. Dans ce cas, il n’est pas nécessaire de supprimer le catalogue global. Pour plus d’informations, voir [suppression du catalogue global](AD-Forest-Recovery-Remove-GC.md).  
  
     En restaurant un catalogue global à partir d’une sauvegarde est plus récent que les autres sauvegardes qui sont utilisés pour restaurer des contrôleurs de domaine dans d’autres domaines, vous pouvez introduire des objets en attente. Prenons l’exemple suivant. Dans le domaine A, DC1 est restauré à partir d’une sauvegarde qui a été effectuée à l’heure T1. Dans le domaine B, DC2 est restauré à partir d’une sauvegarde de catalogue global a été prise à l’heure T2. Supposons que T2 est plus récente que T1 et que certains objets ont été créés entre T1 et T2. Une fois que ces contrôleurs de domaine sont restaurés, DC2, qui est un catalogue global, contient des données plus récentes pour réplica partiel de domaine de que contient le domaine A lui-même. DC2, dans ce cas, contient des objets en attente, car ces objets ne sont pas présents sur DC1.  
  
     La présence d’objets en attente peut entraîner des problèmes. Par exemple, les messages électroniques ne peuvent pas être remis à un utilisateur dont l’objet utilisateur a été déplacé entre domaines. Une fois que vous mettez le contrôleur de domaine obsolète ou le serveur de catalogue global en ligne, les deux instances de l’objet utilisateur apparaissent dans le catalogue global. Les deux objets possèdent la même adresse de messagerie; par conséquent, les messages électroniques ne peuvent pas être remis.  
  
     Un deuxième problème est qu’un compte d’utilisateur qui n’existe plus peut toujours apparaître dans la liste d’adresses globale. Un troisième problème est qu’un groupe universel n’existe plus peut toujours apparaître dans un jeton d’accès utilisateur.  
  
     Si vous n’avez restauré un contrôleur de domaine qui a été un catalogue global, soit par inadvertance ou parce que la sauvegarde Solitaire qui vous approuvé a été, nous recommandons d’interdire l’occurrence d’objets en attente en désactivant le catalogue global immédiatement après l’opération de restauration est terminée. La désactivation de l’indicateur de catalogue global entraîne l’ordinateur perdre tous ses réplicas partielles (partitions) et le fait d’isoler lui-même à l’état du contrôleur de domaine standard.  
  
13. Configurer le Service de temps Windows. Dans le domaine racine de forêt, configurez l’émulateur PDC pour synchroniser l’heure à partir d’une source de temps externe. Pour plus d’informations, voir [configurer le service de temps Windows sur l’émulateur de contrôleur de domaine principal dans le domaine racine de forêt](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731191%28v=ws.10%29).  
  
 

## <a name="reconnect-each-restored-writeable-domain-controller-to-a-common-network"></a>Reconnectez chaque contrôleur de domaine accessible en écriture restauré à un réseau commun  
 À ce stade, vous devez avoir un contrôleur de domaine restauré (et les étapes de récupération) dans le domaine racine de forêt et dans chacun des autres domaines. Rejoignez ces contrôleurs de domaine à un réseau commun est isolé du reste de l’environnement et effectuez les étapes suivantes pour valider la réplication et l’intégrité de la forêt.  
  
> [!NOTE]
>  Lorsque vous joignez les contrôleurs de domaine physiques à un réseau isolé, vous devrez peut-être modifier leurs adresses IP. Par conséquent, les adresses IP des enregistrements DNS sera incorrects. Dans la mesure où un serveur de catalogue global n’est pas disponible, les mises à jour dynamiques sécurisées DNS échoue. Contrôleurs de domaine virtuels sont plus avantageux dans ce cas, car ils peuvent être joint à un nouveau réseau virtuel sans modifier leurs adresses IP. Il s’agit de l’une des raisons pourquoi recommandés de contrôleurs de domaine virtuels en tant que contrôleurs de domaine de premier à restaurer lors de la récupération de forêt.  
  
 Après la validation, joindre les contrôleurs de domaine pour le réseau de production et effectuez les étapes pour vérifier l’intégrité de la réplication de forêt.  
  
-   Pour corriger la résolution de noms, créer des enregistrements de délégation DNS et configurer les indications de racine et de transfert DNS en fonction des besoins. Exécutez **repadmin /replsum** pour vérifier la réplication entre les contrôleurs de domaine.  
  
-   Si le contrôleur de domaine restauré n’est pas des partenaires de réplication directe, récupération de la réplication sera beaucoup plus rapidement en créant des objets de connexion temporaire entre eux.  
  
-   Pour valider le nettoyage des métadonnées, exécutez **Repadmin /viewlist \ *** pour obtenir la liste de tous les contrôleurs de domaine dans la forêt. Exécutez **Nltest /DCList:***< domain\ >* pour obtenir la liste de tous les contrôleurs de domaine dans le domaine.  
  
-   Pour vérifier l’état du contrôleur de domaine et DNS, exécutez DCDiag /v pour signaler les erreurs sur tous les contrôleurs de domaine dans la forêt.  
  
  

## <a name="add-the-global-catalog-to-a-domain-controller-in-the-forest-root-domain"></a>Ajouter le catalogue global à un contrôleur de domaine dans le domaine racine de forêt  
 Un catalogue global est nécessaire pour celles-ci et d’autres raisons:  
  
-   Pour activer les ouvertures de session pour les utilisateurs.  
  
-   Pour activer le service Net Logon en cours d’exécution sur les contrôleurs de domaine dans chaque domaine enfant pour inscrire et supprimer des enregistrements sur le serveur DNS dans le domaine racine.  
  
 Bien qu’il est préférable que la contrôleur de domaine racine de la forêt devient un catalogue global, il est possible de choisir un des contrôleurs de domaine restaurés à devenir un catalogue global.  
  
> [!NOTE]
>  Un contrôleur de domaine ne sera pas publié en tant que serveur de catalogue global jusqu'à ce qu’il a terminé une synchronisation complète de toutes les partitions d’annuaire de la forêt. Par conséquent, le contrôleur de domaine doit être forcé à répliquer avec chacun des contrôleurs de domaine restaurés dans la forêt.  
>   
>  Surveiller le Service d’annuaire journal des événements dans l’Observateur d’événements pour l’événement ID1119, qui indique que ce contrôleur de domaine est un serveur de catalogue global, ou vérifiez que la clé de Registre suivante a la valeur 1:  
>   
>  **Promotion du catalogue HKLM\System\CurrentControlSet\Services\NTDS\Parameters\Global complète**  
  
 Pour plus d’informations, voir [Ajout du catalogue global](AD-Forest-Recovery-Add-GC.md).  
  
 À ce stade, vous devez avoir une forêt stable, avec un contrôleur de domaine pour chaque domaine et un catalogue global dans la forêt. Vous devez effectuer une nouvelle sauvegarde de chacun des contrôleurs de domaine que vous venez de restaurer. Vous pouvez maintenant commencer à redéployer les autres contrôleurs de domaine dans la forêt en installant ADDS.  

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
  
