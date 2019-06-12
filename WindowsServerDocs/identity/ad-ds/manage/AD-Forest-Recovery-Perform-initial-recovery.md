---
title: Récupération de forêt AD - effectuer une récupération initiale
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: 9883d337520c3920f8638ddfe5f6bd393e31fd2f
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66442862"
---
# <a name="perform-initial-recovery"></a>Effectuer une récupération initiale  

>S'applique à : Windows Server 2016, Windows Server 2012 et 2012 R2, Windows Server 2008 et 2008 R2

Cette section comprend les étapes suivantes :  

- [Restaurer le premier contrôleur de domaine accessible en écriture dans chaque domaine](#restore-the-first-writeable-domain-controller-in-each-domain)  
- [Se reconnecter chaque contrôleur de domaine accessible en écriture restauré au réseau](#reconnect-each-restored-writeable-domain-controller-to-a-common-network)  
- [Ajouter le catalogue global à un contrôleur de domaine dans le domaine racine de forêt](#add-the-global-catalog-to-a-domain-controller-in-the-forest-root-domain)  

## <a name="restore-the-first-writeable-domain-controller-in-each-domain"></a>Restaurer le premier contrôleur de domaine accessible en écriture dans chaque domaine  

À partir d’un contrôleur de domaine accessible en écriture dans le domaine racine de forêt, d’effectuer les étapes décrites dans cette section afin de restaurer le premier contrôleur de domaine. Le domaine racine de forêt est important, car il stocke les groupes Administrateurs du schéma et administrateurs de l’entreprise. Cela vous permet également de mettre à jour de la hiérarchie d’approbation de la forêt. En outre, le domaine racine de forêt conserve généralement le serveur de racine DNS pour l’espace de noms DNS de la forêt. Par conséquent, la zone DNS intégrées à Active Directory pour ce domaine contienne les enregistrements de ressource alias (CNAME) pour tous les autres contrôleurs de domaine dans la forêt (qui sont requis pour la réplication) et les enregistrements de ressources DNS de catalogue global. 
  
Après avoir récupéré le domaine racine de forêt, répétez ces étapes pour récupérer les autres domaines dans la forêt. Vous pouvez récupérer plusieurs domaines simultanément ; Toutefois, toujours récupérer un domaine parent avant de récupérer un enfant pour éviter toute interruption dans la hiérarchie d’approbation ou de la résolution de noms DNS. 
  
Pour chaque domaine que vous récupérez, ne restaurer qu’un seul contrôleur de domaine accessible en écriture à partir de la sauvegarde. Il s’agit de la partie la plus importante de la récupération, car le contrôleur de domaine doit avoir une base de données ne soit pas influencé par tout ce qui a provoqué la forêt à échouer. Il est important de disposer d’une sauvegarde de confiance est soigneusement testée avant d’être introduit dans l’environnement de production. 
  
Puis procédez comme suit. Les procédures permettant d’exécuter certaines étapes sont dans [récupération de forêt AD - procédures](AD-Forest-Recovery-Procedures.md). 
  
1. Si vous envisagez de restaurer un serveur physique, vérifiez que le câble réseau de la cible du contrôleur de domaine n’est pas attaché et par conséquent n’est pas connecté au réseau de production. Pour une machine virtuelle, vous pouvez supprimer la carte réseau ou utiliser une carte réseau qui est attachée à un autre réseau où vous pouvez tester le processus de récupération pendant isolé du réseau de production. 
  
2. Comme il s’agit du premier contrôleur de domaine accessible en écriture dans le domaine, vous devez effectuer une restauration ne faisant pas autoritée des services AD DS et une restauration faisant autorité de SYSVOL. L’opération de restauration doivent être effectuées à l’aide d’une sauvegarde compatible Active Directory et restaurer l’application, telle que la sauvegarde de Windows Server (autrement dit, vous ne devez pas restaurer le contrôleur de domaine à l’aide des méthodes non prises en charge comme la restauration d’un instantané de machine virtuelle). 
   - Une restauration faisant autorité de SYSVOL est nécessaire car la réplication de SYSVOL répliqué dossier doit être démarré après la récupération d’urgence. Tous les contrôleurs de domaine suivants sont ajoutés dans le domaine doivent resynchroniser leur dossier SYSVOL avec une copie du dossier qui a été sélectionné pour être faisant autorité avant que le dossier peut être publié. 

   > [!CAUTION]
   > Effectuer une opération de restauration faisant autorité (ou primaire) de SYSVOL uniquement pour le premier contrôleur de domaine à restaurer dans le domaine racine de forêt. Incorrectement exécution d’opérations de restauration principale de SYSVOL sur les autres contrôleurs de domaine conduit à des conflits de réplication de données SYSVOL. 

   - Il existe deux options effectuer une restauration ne faisant pas autoritée des services AD DS et une restauration faisant autorité de SYSVOL :  
   - Effectuer une récupération complète du serveur et puis forcer une synchronisation faisant autoritée de SYSVOL. Pour les procédures détaillées, consultez [exécution d’une récupération complète du serveur](AD-Forest-Recovery-Perform-a-Full-Recovery.md) et [effectuera une synchronisation faisant autoritée de SYSVOL de réplication DFSR](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md). 
   - Effectuer une récupération de serveur complète suivie d’une restauration d’état système. Cette option nécessite que vous créez à l’avance les deux types de sauvegardes : une sauvegarde complète du serveur et une sauvegarde de l’état système. Pour les procédures détaillées, consultez [exécution d’une récupération complète du serveur](AD-Forest-Recovery-Perform-a-Full-Recovery.md) et [effectuer une restauration ne faisant pas autoritée des Services de domaine Active Directory](AD-Forest-Recovery-Nonauthoritative-Restore.md). 
  
3. Après la restauration et de redémarrer le contrôleur de domaine accessible en écriture, vérifiez que l’échec n’affectait pas les données sur le contrôleur de domaine. Si les données du contrôleur de domaine sont endommagées, puis répétez l’étape 2 avec une autre sauvegarde. 
   - Si le contrôleur de domaine restauré héberge un rôle de maître d’opérations, vous devrez peut-être ajouter l’entrée de Registre suivante pour éviter les services AD DS ne soit pas disponible jusqu'à ce qu’il a terminé la réplication d’une partition d’annuaire inscriptible :  

      **HKLM\System\CurrentControlSet\Services\NTDS\Parameters\Repl Perform Initial Synchronizations**  
  
      Créer l’entrée avec le type de données **REG_DWORD** et la valeur **0**. Une fois la forêt est entièrement récupérée, vous pouvez réinitialiser la valeur de cette entrée à **1**, ce qui nécessite un contrôleur de domaine redémarre et entrant contient des rôles de maître d’opérations d’avoir réussi AD DS et la réplication sortante avec son connue des partenaires de réplica avant qu’il publie lui-même en tant que contrôleur de domaine et commence à fournir des services aux clients. Pour plus d’informations sur la configuration requise de la synchronisation initiale, consultez l’article [305476](https://support.microsoft.com/kb/305476). 
  
      Passez aux étapes suivantes uniquement après la restauration et vérifier les données et avant de joindre cet ordinateur au réseau de production. 
  
4. Si vous pensez que l’échec de la forêt a été liée à network intrusion ou attaque malveillante, réinitialiser les mots de passe de compte pour tous les comptes d’administration, y compris les membres des administrateurs de l’entreprise, Admins du domaine, administrateurs du schéma, serveur, opérateurs de compte Groupes d’opérateurs et ainsi de suite. La réinitialisation des mots de passe de compte d’administration doit être effectuée avant les contrôleurs sont installés lors de la phase de la récupération de forêt de domaine supplémentaire. 
5. Le premier contrôleur de domaine restauré dans le domaine racine de forêt, prendre tous les rôles de maître d’opérations de l’échelle du domaine et de forêt. Informations d’identification de l’entreprise et administrateurs du schéma sont nécessaires pour prendre les rôles de maître d’opérations de la forêt. 
  
     Dans chaque domaine enfant, prendre les rôles de maître d’opérations de l’échelle du domaine. Bien que vous pourrez conserver les rôles de maître d’opérations sur le DC restauré que temporairement, prise de ces rôles garantit concernant les DC à ce stade, les hôtes dans le processus de récupération de forêt. Dans le cadre de votre processus postérieures à la récupération, vous pouvez redistribuer les rôles de maître d’opérations en fonction des besoins. Pour plus d’informations sur la prise des rôles de maître d’opérations, consultez [prendre un rôle de maître d’opérations](AD-forest-recovery-seizing-operations-master-role.md). Pour obtenir des recommandations sur l’emplacement où placer les rôles de maître d’opérations, consultez [quelles sont les maîtres d’opérations ?](https://technet.microsoft.com/library/cc779716.aspx). 
  
6. Nettoyer les métadonnées de tous les autres contrôleurs de domaine accessible en écriture dans le domaine racine de forêt que vous restaurez pas à partir de la sauvegarde (tous les contrôleurs dans le domaine à l’exception de ce premier contrôleur de domaine accessible en écriture). Si vous utilisez la version des utilisateurs Active Directory et les ordinateurs ou les Sites Active Directory et les Services qui est inclus avec Windows Server 2008 ou version ultérieure ou serveur distant pour Windows Vista ou versions ultérieures, le nettoyage des métadonnées est effectué automatiquement lorsque vous supprimez un objet de contrôleur de domaine. En outre, l’objet serveur et un objet ordinateur pour le contrôleur de domaine supprimé sont également supprimés automatiquement. Pour plus d’informations, consultez [nettoyage des métadonnées de supprimer des contrôleurs de domaine accessible en écriture](AD-Forest-Recovery-Cleaning-Metadata.md). 
  
     Nettoyage des métadonnées d’empêche le dupliquer des objets de paramètres NTDS si AD DS est installé sur un contrôleur de domaine dans un autre site. Potentiellement, cela peut également enregistrer le vérificateur de cohérence des connaissances (KCC) le processus de création de liens de réplication lorsque les contrôleurs de domaine eux-mêmes ne peuvent pas être présentes. En outre, dans le cadre du nettoyage des métadonnées, les enregistrements de ressources DNS du localisateur pour tous les autres contrôleurs de domaine dans le domaine sont supprimés à partir de DNS. 
  
     Jusqu'à ce que les métadonnées de tous les autres contrôleurs de domaine dans le domaine sont supprimée, ce contrôleur de domaine, s’il s’agissait d’un maître RID avant la récupération, pas suppose que le rôle de maître RID et par conséquent ne pourrez pas émettre de nouveaux RID. Vous pouvez voir les ID d’événement 16650 dans le journal système dans l’Observateur d’événement indiquant cet échec, mais vous devez voir les ID d’événement 16648 indiquant la réussite un certain temps une fois que vous avez supprimé les métadonnées. 
  
7. Si vous avez des zones DNS qui sont stockées dans AD DS, assurez-vous que le service serveur DNS local est installé et en cours d’exécution sur le contrôleur de domaine que vous avez restaurée. Si ce contrôleur de domaine n’était pas un serveur DNS avant la défaillance de la forêt, vous devez installer et configurer le serveur DNS. 
  
    > [!NOTE]
    > Si le contrôleur de domaine restauré s’exécute Windows Server 2008, vous devez installer le correctif logiciel dans l’article [975654](https://support.microsoft.com/kb/975654) ou le serveur à un réseau isolé temporairement afin d’installer le serveur DNS. Le correctif logiciel n’est pas requis pour toutes les autres versions de Windows Server. 
  
     Dans le domaine racine de forêt, configurez le DC restauré avec sa propre adresse IP (ou une adresse de bouclage, par exemple 127.0.0.1) en tant que serveur DNS préféré. Vous pouvez configurer ce paramètre dans les propriétés TCP/IP de l’adaptateur de réseau local (LAN). Il s’agit du premier serveur DNS dans la forêt. Pour plus d’informations, consultez [configurer TCP/IP pour utiliser DNS](https://technet.microsoft.com/library/cc779282\(WS.10\).aspx). 
  
     Dans chaque domaine enfant, configurez le DC restauré avec l’adresse IP du premier serveur DNS dans le domaine racine de forêt en tant que serveur DNS préféré. Vous pouvez configurer ce paramètre dans les propriétés TCP/IP de la carte réseau. Pour plus d’informations, consultez [configurer TCP/IP pour utiliser DNS](https://technet.microsoft.com/library/cc779282\(WS.10\).aspx). 
  
     Dans les zones DNS _msdcs et de domaine, supprimez les enregistrements NS de contrôleurs de domaine qui n’existent plus après le nettoyage des métadonnées. Vérifiez si les enregistrements SRV des contrôleurs de domaine nettoyés ont été supprimés. Pour aider à accélérer la suppression de l’enregistrement SRV DNS, exécutez :  
  
    ```  
    nltest.exe /dsderegdns:server.domain.tld  
    ```  
  
8. Augmenter la valeur du pool RID disponible par 100 000. Pour plus d’informations, consultez [déclenchant la valeur de pools RID disponibles](AD-Forest-Recovery-Raise-RID-Pool.md). Si vous avez des raisons de croire que le déclenchement du Pool RID par 100 000 est insuffisante pour votre cas particulier, vous devez déterminer l’augmentation de la plus basse est toujours sûr à utiliser. Les RID sont une ressource limitée qui ne doit pas être utilisée d’inutilement. 
  
     Si de nouvelles entités de sécurité ont été créées dans le domaine après l’heure de la sauvegarde que vous utilisez pour la restauration, ces entités de sécurité peuvent avoir des droits d’accès sur certains objets. Ces principaux de sécurité n’existe plus après la récupération, car la récupération a été rétabli à la sauvegarde ; Toutefois, leurs droits d’accès peuvent encore exister. Si le pool RID disponible n’est pas déclenché après une restauration, nouvel utilisateur des objets qui sont créés après que la récupération de forêt peut obtenir l’ID de sécurité sont identiques (SID) et peut avoir accès à ces objets, ce qui n’a pas été conçu à l’origine. 
  
     Pour illustrer cela, prenons l’exemple du nouvel employé nommé Amy mentionné dans l’introduction. L’objet utilisateur pour Amy n’existe plus après l’opération de restauration, car il a été créé après la sauvegarde a été utilisée pour restaurer le domaine. Toutefois, les droits d’accès qui ont été affectées à cet objet de l’utilisateur peuvent conserver après l’opération de restauration. Si le SID pour cet objet utilisateur est réaffecté à un nouvel objet après l’opération de restauration, le nouvel objet obtiendrait ces droits d’accès. 
  
9. Invalider le pool RID actuel. Le pool RID actuel est invalidé après un état du système est restauré. Mais si une restauration d’état système n’a pas été effectuée, le pool RID actuel doit être invalidés pour empêcher le DC restauré à partir de la nouvelle émission RID à partir du pool RID qui a été affecté au moment de que la sauvegarde a été créée. Pour plus d’informations, consultez [invalidant le pool RID actuel](AD-Forest-Recovery-Invaildate-RID-Pool.md). 
  
    > [!NOTE]
    > La première fois que vous essayez de créer un objet avec un identificateur de sécurité une fois que vous invalidez le pool RID, vous recevrez une erreur. La tentative de création d’un objet déclenche une demande d’un nouveau pool RID. Nouvelle tentative de l’opération réussit, car le nouveau pool RID sera alloué. 
  
10. Réinitialiser le mot de passe du compte ordinateur de ce contrôleur de domaine à deux reprises. Pour plus d’informations, consultez [réinitialiser le mot de passe du compte ordinateur du contrôleur de domaine](AD-Forest-Recovery-Reset-Computer-Account-DC.md). 
  
11. Réinitialiser le mot de passe krbtgt deux fois. Pour plus d’informations, consultez [réinitialiser le mot de passe krbtgt](AD-Forest-Recovery-Resetting-the-krbtgt-password.md). 
  
     L’historique de mot de passe krbtgt étant deux mots de passe, réinitialiser les mots de passe à deux reprises pour supprimer le mot de passe (prefailure) d’origine à partir de l’historique du mot de passe. 
  
    > [!NOTE]
    > Si la récupération de forêt est en réponse à une violation de la sécurité, vous pouvez également réinitialiser les mots de passe d’approbation. Pour plus d’informations, consultez [réinitialiser un mot de passe d’approbation d’un côté de l’approbation](AD-Forest-Recovery-Reset-Trust.md). 
  
12. Si la forêt possède plusieurs domaines et que le DC restauré était un serveur de catalogue global avant la défaillance, désactivez le **catalogue Global** case à cocher dans les propriétés de paramètres NTDS pour supprimer le catalogue global du contrôleur de domaine. L’exception à cette règle est le cas courant d’une forêt avec un domaine. Dans ce cas, il n’est pas nécessaire de supprimer le catalogue global. Pour plus d’informations, consultez [suppression du catalogue global](AD-Forest-Recovery-Remove-GC.md). 
  
     En restaurant un catalogue global à partir d’une sauvegarde qui est plus récente que les autres sauvegardes qui sont utilisés pour restaurer des contrôleurs de domaine dans d’autres domaines, vous risquez d’introduire des objets en attente. Prenons l’exemple suivant. Dans le domaine A, DC1 est restauré à partir d’une sauvegarde qui a été effectuée à l’heure T1. Dans le domaine B, DC2 est restauré à partir d’une sauvegarde de catalogue global qui a été effectuée à l’heure T2. Supposons que T2 est plus récente que celle de T1, et certains objets ont été créées entre T1 et T2. Une fois que ces contrôleurs de domaine sont restaurés, DC2, qui est un catalogue global, contient des données plus récentes pour réplica partiel du domaine A au domaine A contient lui-même. DC2, contient dans ce cas, les objets en attente, car ces objets ne sont pas présents sur DC1. 
  
     La présence d’objets en attente peut entraîner des problèmes. Par exemple, les messages électroniques ne peuvent pas être remis à un utilisateur dont l’objet utilisateur a été déplacé entre des domaines. Après avoir mis le contrôleur de domaine obsolète ou d’un serveur de catalogue global en ligne, les deux instances de l’objet utilisateur apparaissent dans le catalogue global. Les deux objets ont la même adresse de messagerie ; Par conséquent, les messages électroniques ne peut pas être remis. 
  
     Le deuxième problème est qu’un compte d’utilisateur qui n’existe plus peut apparaître dans la liste d’adresses globale. Un troisième problème est qu’un groupe universel qui n’existe plus peut apparaître dans un jeton d’accès utilisateur. 
  
     Si vous n’avez restauré un contrôleur de domaine qui a été un catalogue global, soit par inadvertance ou car il s’agit de la sauvegarde Solitaire que vous avez confiance, nous recommandons d’interdire l’occurrence des objets en attente en désactivant le catalogue global peu après l’opération de restauration Terminer. La désactivation de l’indicateur de catalogue global entraîne l’ordinateur perdre tous ses réplicas partielles (partitions) et le fait d’isoler lui-même à l’état de contrôleur de domaine régulier. 
  
13. Configurer le Service de temps Windows. Dans le domaine racine de forêt, configurez l’émulateur de contrôleur de domaine principal pour synchroniser l’heure à partir d’une source externe. Pour plus d’informations, consultez [configurer le service de temps de Windows sur l’émulateur de contrôleur de domaine principal dans le domaine racine de forêt](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731191%28v=ws.10%29). 
  
## <a name="reconnect-each-restored-writeable-domain-controller-to-a-common-network"></a>Se reconnecter chaque contrôleur de domaine accessible en écriture restaurée à un réseau commun

À ce stade, vous devez avoir un contrôleur de domaine restauré (et les étapes de récupération effectuées) dans le domaine racine de forêt et dans chacun des autres domaines. Joindre ces contrôleurs de domaine à un réseau commun qui est isolé du reste de l’environnement et effectuez les étapes suivantes afin de valider la réplication et l’intégrité de la forêt.

> [!NOTE]
> Lorsque vous joignez les contrôleurs de domaine physiques à un réseau isolé, vous devrez peut-être modifier leurs adresses IP. Par conséquent, les adresses IP des enregistrements DNS sera incorrects. Étant donné que le serveur de catalogue global n’est pas disponible, les mises à jour dynamiques sécurisées DNS échoue. Contrôleurs de domaine virtuels sont plus avantageux dans ce cas car ils peuvent être jointes à un nouveau réseau virtuel sans modifier leurs adresses IP. Il s’agit de l’une des raisons pourquoi les contrôleurs de domaine virtuels sont recommandés en tant que les contrôleurs de domaine de premier à restaurer pendant la récupération de forêt. 
  
Après la validation, joindre les contrôleurs de domaine pour le réseau de production et effectuez les étapes pour vérifier l’intégrité de la réplication de forêt.

- Pour corriger la résolution de noms, créer des enregistrements de délégation DNS et configurer les indications de racine et de transfert DNS en fonction des besoins. Exécutez **repadmin /replsum** pour vérifier la réplication entre contrôleurs de domaine. 
- Si le contrôleur de domaine restauré n’est pas des partenaires de réplication directe, récupération de la réplication sera beaucoup plus rapide en créant des objets de connexion temporaire entre eux. 
- Pour valider le nettoyage des métadonnées, exécutez **Repadmin /viewlist \\** * pour obtenir la liste de tous les contrôleurs de domaine dans la forêt. Exécutez **Nltest /DCList :** *< domaine\>*  pour obtenir la liste de tous les contrôleurs de domaine dans le domaine. 
- Pour vérifier l’intégrité du contrôleur de domaine et DNS, exécutez DCDiag /v pour signaler des erreurs sur tous les contrôleurs de domaine dans la forêt. 

## <a name="add-the-global-catalog-to-a-domain-controller-in-the-forest-root-domain"></a>Ajouter le catalogue global à un contrôleur de domaine dans le domaine racine de forêt

Un catalogue global est requis pour ces raisons et :  
  
- Pour activer les ouvertures de session pour les utilisateurs. 
- Pour activer le service Net Logon s’exécutant sur les contrôleurs de domaine dans chaque domaine enfant pour vous inscrire et supprimer des enregistrements sur le serveur DNS dans le domaine racine. 
  
Bien qu’il est préférable que la contrôleur de domaine racine de la forêt deviennent un catalogue global, il est possible de choisir un des contrôleurs de domaine restaurés pour devenir un catalogue global. 
  
> [!NOTE]
> Un contrôleur de domaine ne sera pas publié en tant que serveur de catalogue global jusqu'à ce qu’il a terminé une synchronisation complète de toutes les partitions d’annuaire dans la forêt. Par conséquent, le contrôleur de domaine doit être forcée pour la réplication avec chacun des contrôleurs de domaine restaurés dans la forêt. 
>
> Surveiller le Service d’annuaire journal des événements dans l’Observateur d’événements pour l’événement ID 1119, ce qui indique que ce contrôleur de domaine est un serveur de catalogue global, ou vérifiez que la clé de Registre suivante a la valeur 1 :  
>
> **HKLM\System\CurrentControlSet\Services\NTDS\Parameters\Global Catalog Promotion Complete**  
  
Pour plus d’informations, consultez [Ajout du catalogue global](AD-Forest-Recovery-Add-GC.md). 
  
À ce stade, vous devez avoir une forêt stable, avec un contrôleur de domaine pour chaque domaine et un catalogue global dans la forêt. Vous devez effectuer une sauvegarde de nouveau de chacun des contrôleurs de domaine que vous venez de restaurer. Vous pouvez maintenant commencer à redéployer les autres contrôleurs de domaine dans la forêt en installant AD DS. 

## <a name="next-steps"></a>Étapes suivantes

- [Récupération de la forêt Active Directory : conditions préalables](AD-Forest-Recovery-Prerequisties.md)  
- [Récupération de forêt AD - concevoir un plan de récupération personnalisée-forêt](AD-Forest-Recovery-Devising-a-Plan.md)  
- [Récupération de forêt AD - identifier le problème](AD-Forest-Recovery-Identify-the-Problem.md)
- [Récupération de forêt AD - déterminer comment récupérer](AD-Forest-Recovery-Determine-how-to-Recover.md)
- [Récupération de forêt AD - effectuer une récupération initiale](AD-Forest-Recovery-Perform-initial-recovery.md)  
- [Récupération de la forêt Active Directory : procédures](AD-Forest-Recovery-Procedures.md)  
- [Récupération de forêt AD - Forum aux Questions](AD-Forest-Recovery-FAQ.md)  
- [Récupération de forêt AD - récupération d’un domaine unique au sein d’une forêt Multidomain](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
- [Récupération de forêt AD - récupération de forêt avec les contrôleurs de domaine Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md)  
