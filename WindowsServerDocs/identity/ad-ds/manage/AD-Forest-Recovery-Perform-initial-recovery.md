---
title: Récupération de la forêt Active Directory-effectuer la récupération initiale
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: 7d592198187d44927f643b45e7a8bb4c2eec2a69
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80823902"
---
# <a name="perform-initial-recovery"></a>Effectuer la récupération initiale  

>S’applique à : Windows Server 2016, Windows Server 2012 et 2012 R2, Windows Server 2008 et 2008 R2

Cette section comprend les étapes suivantes :  

- [Restaurer le premier contrôleur de domaine accessible en écriture dans chaque domaine](#restore-the-first-writeable-domain-controller-in-each-domain)  
- [Reconnecter chaque contrôleur de domaine accessible en écriture restauré au réseau](#reconnect-each-restored-writeable-domain-controller-to-a-common-network)  
- [Ajouter le catalogue global à un contrôleur de domaine dans le domaine racine de la forêt](#add-the-global-catalog-to-a-domain-controller-in-the-forest-root-domain)  

## <a name="restore-the-first-writeable-domain-controller-in-each-domain"></a>Restaurer le premier contrôleur de domaine accessible en écriture dans chaque domaine  

À partir d’un contrôleur de domaine accessible en écriture dans le domaine racine de forêt, effectuez les étapes de cette section afin de restaurer le premier contrôleur de domaine. Le domaine racine de forêt est important car il stocke les groupes Administrateurs du schéma et administrateurs de l’entreprise. Il permet également de conserver la hiérarchie d’approbation dans la forêt. En outre, le domaine racine de forêt contient généralement le serveur racine DNS pour l’espace de noms DNS de la forêt. Par conséquent, la zone DNS intégrée à l’Active Directory pour ce domaine contient les enregistrements de ressource alias (CNAMe) pour tous les autres contrôleurs de domaine de la forêt (qui sont requis pour la réplication) et les enregistrements de ressource DNS du catalogue global. 
  
Après avoir récupéré le domaine racine de la forêt, répétez les mêmes étapes pour récupérer les domaines restants dans la forêt. Vous pouvez récupérer plusieurs domaines simultanément. Toutefois, récupérez toujours un domaine parent avant de récupérer un enfant pour empêcher toute interruption dans la hiérarchie d’approbation ou la résolution de nom DNS. 
  
Pour chaque domaine que vous récupérez, restaurez un seul contrôleur de domaine accessible en écriture à partir d’une sauvegarde. Il s’agit de la partie la plus importante de la récupération, car celle-ci doit disposer d’une base de données qui n’a pas été influencée par ce qui a provoqué l’échec de la forêt. Il est important de disposer d’une sauvegarde approuvée qui est minutieusement testée avant d’être introduite dans l’environnement de production. 
  
Ensuite, procédez comme suit. Les procédures relatives à l’exécution de certaines étapes sont décrites dans les procédures de récupération de la [forêt Active Directory](AD-Forest-Recovery-Procedures.md). 
  
1. Si vous envisagez de restaurer un serveur physique, assurez-vous que le câble réseau du contrôleur de réseau cible n’est pas attaché et qu’il n’est donc pas connecté au réseau de production. Pour une machine virtuelle, vous pouvez supprimer la carte réseau ou utiliser une carte réseau attachée à un autre réseau où vous pouvez tester le processus de récupération tout en étant isolé du réseau de production. 
  
2. Étant donné qu’il s’agit du premier contrôleur de domaine accessible en écriture dans le domaine, vous devez effectuer une restauration ne faisant pas autorité de AD DS et une restauration faisant autorité de SYSVOL. L’opération de restauration doit être effectuée à l’aide d’une application de sauvegarde et de restauration Active Directory, telle que Sauvegarde Windows Server (autrement dit, vous ne devez pas restaurer le contrôleur de périphérique à l’aide de méthodes non prises en charge, telles que la restauration d’un instantané de machine virtuelle). 
   - Une restauration faisant autorité de SYSVOL est nécessaire, car la réplication du dossier répliqué SYSVOL doit être démarrée après une défaillance. Tous les contrôleurs de domaine suivants ajoutés au domaine doivent resynchroniser leur dossier SYSVOL avec une copie du dossier qui a été sélectionné pour faire autorité avant que le dossier puisse être publié. 

   > [!CAUTION]
   > Effectuer une opération de restauration faisant autorité (ou principale) de SYSVOL uniquement pour le premier contrôleur de domaine à restaurer dans le domaine racine de forêt. L’exécution incorrecte des opérations de restauration principales du volume SYSVOL sur d’autres contrôleurs de contrôle provoque des conflits de réplication des données SYSVOL. 

   - Deux options effectuent une restauration ne faisant pas autorité de AD DS et une restauration faisant autorité de SYSVOL :  
   - Effectuez une récupération complète du serveur, puis forcez une synchronisation de SYSVOL faisant autorité. Pour obtenir des procédures détaillées, consultez [exécution d’une récupération complète du serveur](AD-Forest-Recovery-Perform-a-Full-Recovery.md) et [exécution d’une synchronisation faisant autorité d’un SYSVOL répliqué par DFSR](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md). 
   - Effectuez une récupération complète du serveur suivie d’une restauration de l’état du système. Cette option nécessite de créer les deux types de sauvegardes à l’avance : une sauvegarde complète du serveur et une sauvegarde de l’état du système. Pour obtenir des procédures détaillées, consultez [exécution d’une récupération complète du serveur](AD-Forest-Recovery-Perform-a-Full-Recovery.md) et [exécution d’une restauration ne faisant pas autorité de Active Directory Domain Services](AD-Forest-Recovery-Nonauthoritative-Restore.md). 
  
3. Après avoir restauré et redémarré le DC inscriptible, vérifiez que l’échec n’a pas affecté les données du contrôleur de périphérique. Si les données du DC sont endommagées, répétez l’étape 2 avec une autre sauvegarde. 
   - Si le contrôleur de domaine restauré héberge un rôle de maître d’opérations, vous devrez peut-être ajouter l’entrée de Registre suivante pour éviter que AD DS soit indisponible tant qu’il n’a pas terminé la réplication d’une partition d’annuaire accessible en écriture :  

      **HKLM\System\CurrentControlSet\Services\NTDS\Parameters\Repl effectuer les synchronisations initiales**  
  
      Créez l’entrée avec le type de données **REG_DWORD** et la valeur **0**. Une fois la forêt entièrement récupérée, vous pouvez réinitialiser la valeur de cette entrée à **1**, ce qui nécessite qu’un contrôleur de domaine redémarre et conserve les rôles de maître d’opérations pour réussir AD DS réplication entrante et sortante avec ses partenaires de réplication connus avant de se publier en tant que contrôleur de domaine et de fournir des services aux clients. Pour plus d’informations sur la configuration requise pour la synchronisation initiale, voir l’article [305476](https://support.microsoft.com/kb/305476)de la base de connaissances. 
  
      Passez aux étapes suivantes uniquement après avoir restauré et vérifié les données et avant de joindre cet ordinateur au réseau de production. 
  
4. Si vous pensez que la défaillance à l’ensemble de la forêt était liée à une intrusion de réseau ou à une attaque malveillante, réinitialisez les mots de passe de compte pour tous les comptes d’administration, y compris les membres des groupes Administrateurs de l’entreprise, admins du domaine, administrateurs du schéma, opérateurs de serveur, opérateurs de compte, etc. La réinitialisation des mots de passe des comptes d’administration doit être effectuée avant l’installation de contrôleurs de domaine supplémentaires lors de la phase suivante de la récupération de la forêt. 
5. Sur le premier contrôleur de domaine restauré dans le domaine racine de forêt, saisissez tous les rôles de maître d’opérations à l’ensemble du domaine et de la forêt. Les informations d’identification administrateurs de l’entreprise et administrateurs du schéma sont nécessaires pour prendre les rôles de maître d’opérations à l’ensemble de la forêt. 
  
     Dans chaque domaine enfant, prenez les rôles de maître d’opérations à l’ensemble du domaine. Même si vous ne pouvez conserver les rôles de maître d’opérations que temporairement sur le contrôleur de périphérique restauré, la prise de ces rôles vous garantit quel contrôleur de bus les héberge à ce stade du processus de récupération de forêt. Dans le cadre de votre processus de récupération après récupération, vous pouvez redistribuer les rôles de maître d’opérations en fonction des besoins. Pour plus d’informations sur la prise des rôles de maître d’opérations, consultez [prise d’un rôle de maître d’opérations](AD-forest-recovery-seizing-operations-master-role.md). Pour obtenir des recommandations sur l’emplacement des rôles de maître d’opérations, consultez [que sont les maîtres d’opérations ?](https://technet.microsoft.com/library/cc779716.aspx). 
  
6. Nettoyez les métadonnées de tous les autres contrôleurs de domaine accessibles en écriture dans le domaine racine de la forêt que vous ne restaurez pas à partir de la sauvegarde (tous les contrôleurs de domaine accessibles en écriture dans le domaine sauf ce premier contrôleur de domaine). Si vous utilisez la version de Active Directory utilisateurs et ordinateurs ou Active Directory sites et services inclus avec Windows Server 2008 ou version ultérieure ou les outils d’aide à la version de Windows Vista ou version ultérieure, le nettoyage des métadonnées s’effectue automatiquement lorsque vous supprimez un objet de contrôleur de site. En outre, l’objet serveur et l’objet ordinateur pour le contrôleur de périphérique supprimé sont également supprimés automatiquement. Pour plus d’informations, consultez [nettoyage des métadonnées des contrôleurs de](AD-Forest-Recovery-Cleaning-Metadata.md)l’accès en écriture supprimés. 
  
     Le nettoyage des métadonnées empêche la duplication possible des objets NTDS-Settings si AD DS est installé sur un contrôleur de site d’un autre site. Potentiellement, cela peut également enregistrer le vérificateur de cohérence des connaissances (KCC) le processus de création de liens de réplication lorsque les contrôleurs de contrôle eux-mêmes ne sont pas présents. En outre, dans le cadre du nettoyage des métadonnées, les enregistrements de ressources DNS du localisateur de contrôleur de domaine pour tous les autres contrôleurs de domaine du domaine seront supprimés du DNS. 
  
     Tant que les métadonnées de tous les autres contrôleurs de domaine du domaine n’ont pas été supprimées, ce contrôleur de domaine, s’il s’agissait d’un maître RID avant la récupération, ne prendra pas en part le rôle de maître RID et ne pourra donc pas émettre de nouveaux RID. Vous pouvez voir l’ID d’événement 16650 dans le journal système dans observateur d’événements indiquant cet échec, mais vous devriez voir l’ID d’événement 16648 indiquant que l’opération a réussi un peu, alors que vous avez nettoyé les métadonnées. 
  
7. Si vous avez des zones DNS qui sont stockées dans AD DS, assurez-vous que le service serveur DNS local est installé et en cours d’exécution sur le contrôleur de domaine que vous avez restauré. Si ce contrôleur de domaine n’était pas un serveur DNS avant la défaillance de la forêt, vous devez installer et configurer le serveur DNS. 
  
    > [!NOTE]
    > Si le contrôleur de domaine restauré exécute Windows Server 2008, vous devez installer le correctif logiciel dans l’article [975654](https://support.microsoft.com/kb/975654) de la base de connaissances ou connecter temporairement le serveur à un réseau isolé pour installer le serveur DNS. Le correctif logiciel n’est pas requis pour les autres versions de Windows Server. 
  
     Dans le domaine racine de forêt, configurez le contrôleur de domaine restauré avec sa propre adresse IP (ou une adresse de bouclage, telle que 127.0.0.1) comme serveur DNS préféré. Vous pouvez configurer ce paramètre dans les propriétés TCP/IP de la carte réseau local (LAN). Il s’agit du premier serveur DNS de la forêt. Pour plus d’informations, consultez [Configurer TCP/IP pour utiliser DNS](https://technet.microsoft.com/library/cc779282\(WS.10\).aspx). 
  
     Dans chaque domaine enfant, configurez le contrôleur de domaine restauré avec l’adresse IP du premier serveur DNS du domaine racine de la forêt en tant que serveur DNS préféré. Vous pouvez configurer ce paramètre dans les propriétés TCP/IP de la carte réseau. Pour plus d’informations, consultez [Configurer TCP/IP pour utiliser DNS](https://technet.microsoft.com/library/cc779282\(WS.10\).aspx). 
  
     Dans les zones DNS du _msdcs et du domaine, supprimez les enregistrements NS des contrôleurs de domaine qui n’existent plus après le nettoyage des métadonnées. Vérifiez si les enregistrements SRV des contrôleurs de contrôle d’accès ont été supprimés. Pour accélérer la suppression de l’enregistrement SRV DNS, exécutez :  
  
    ```  
    nltest.exe /dsderegdns:server.domain.tld  
    ```  
  
8. Augmentez la valeur du pool RID disponible par 100 000. Pour plus d’informations, consultez [élévation de la valeur des pools RID disponibles](AD-Forest-Recovery-Raise-RID-Pool.md). Si vous avez des raisons de croire que le fait d’augmenter le pool RID de 100 000 est insuffisant pour votre situation particulière, vous devez déterminer l’augmentation la plus faible qui peut encore être utilisée en toute sécurité. Les RID sont une ressource finie qui ne doit pas être utilisée inutilement. 
  
     Si de nouveaux principaux de sécurité ont été créés dans le domaine après l’heure de la sauvegarde que vous utilisez pour la restauration, ces principaux de sécurité peuvent avoir des droits d’accès sur certains objets. Ces principaux de sécurité n’existent plus après la récupération, car la récupération est revenue à la sauvegarde ; Toutefois, leurs droits d’accès peuvent toujours exister. Si le pool RID disponible n’est pas déclenché après une restauration, les nouveaux objets utilisateur créés après la récupération de la forêt peuvent obtenir des ID de sécurité (SID) identiques et peuvent avoir accès à ces objets, ce qui n’était pas prévu à l’origine. 
  
     Pour illustrer cela, prenons l’exemple du nouvel employé nommé Amy qui a été mentionné dans l’introduction. L’objet utilisateur pour Amy n’existe plus après l’opération de restauration, car il a été créé après la sauvegarde qui a été utilisée pour restaurer le domaine. Toutefois, tous les droits d’accès qui ont été attribués à cet objet utilisateur peuvent être conservés après l’opération de restauration. Si le SID de cet objet utilisateur est réaffecté à un nouvel objet après l’opération de restauration, le nouvel objet obtient ces droits d’accès. 
  
9. Invalidez le pool RID actuel. Le pool RID actuel est invalidé après une restauration de l’état du système. Toutefois, si une restauration de l’état du système n’a pas été effectuée, le pool RID actuel doit être invalidé pour empêcher le contrôleur de réseau restauré de réémettre les RID à partir du pool RID affecté au moment de la création de la sauvegarde. Pour plus d’informations, consultez [invalidation du pool RID actuel](AD-Forest-Recovery-Invaildate-RID-Pool.md). 
  
    > [!NOTE]
    > La première fois que vous tentez de créer un objet avec un SID après avoir invalidé le pool RID, vous recevez une erreur. La tentative de création d’un objet déclenche une demande de nouveau pool RID. La nouvelle tentative de l’opération a échoué, car le nouveau pool RID sera alloué. 
  
10. Réinitialisez deux fois le mot de passe du compte d’ordinateur de ce contrôleur de périphérique. Pour plus d’informations, consultez [réinitialisation du mot de passe du compte d’ordinateur du contrôleur de domaine](AD-Forest-Recovery-Reset-Computer-Account-DC.md). 
  
11. Réinitialisez le mot de passe krbtgt à deux reprises. Pour plus d’informations, consultez [réinitialisation du mot de passe krbtgt](AD-Forest-Recovery-Resetting-the-krbtgt-password.md). 
  
     Étant donné que l’historique du mot de passe krbtgt est deux mots de passe, réinitialisez les mots de passe à deux reprises pour supprimer le mot de passe d’origine (prédéfaillance) de l’historique 
  
    > [!NOTE]
    > Si la récupération de la forêt est en réponse à une violation de la sécurité, vous pouvez également réinitialiser les mots de passe d’approbation. Pour plus d’informations, consultez [réinitialisation d’un mot de passe d’approbation sur un côté de l’approbation](AD-Forest-Recovery-Reset-Trust.md). 
  
12. Si la forêt possède plusieurs domaines et que le contrôleur de domaine restauré était un serveur de catalogue global avant la défaillance, désactivez la case à cocher **catalogue global** dans les propriétés des paramètres NTDS pour supprimer le catalogue global du contrôleur de domaine. L’exception à cette règle est le cas courant d’une forêt avec un seul domaine. Dans ce cas, il n’est pas nécessaire de supprimer le catalogue global. Pour plus d’informations, consultez [suppression du catalogue global](AD-Forest-Recovery-Remove-GC.md). 
  
     En restaurant un catalogue global à partir d’une sauvegarde qui est plus récente que les autres sauvegardes utilisées pour restaurer les contrôleurs de domaine dans d’autres domaines, vous pouvez introduire des objets en attente. Prenons l’exemple suivant. Dans le domaine A, DC1 est restauré à partir d’une sauvegarde qui a été effectuée à l’heure T1. Dans le domaine B, DC2 est restauré à partir d’une sauvegarde de catalogue global qui a été effectuée à l’heure T2. Supposons que T2 est plus récent que T1 et que certains objets ont été créés entre T1 et T2. Une fois ces contrôleurs de domaine restaurés, DC2, qui est un catalogue global, contient les données les plus récentes pour le réplica partiel du domaine A, alors que le domaine A contient lui-même. DC2, dans ce cas, contient des objets en attente, car ces objets ne sont pas présents sur DC1. 
  
     La présence d’objets en attente peut entraîner des problèmes. Par exemple, les messages électroniques peuvent ne pas être remis à un utilisateur dont l’objet utilisateur a été déplacé entre des domaines. Une fois le contrôleur de service ou le serveur de catalogue global mis à jour en ligne, les deux instances de l’objet utilisateur s’affichent dans le catalogue global. Les deux objets ont la même adresse de messagerie ; par conséquent, les messages électroniques ne peuvent pas être remis. 
  
     Un deuxième problème est qu’un compte d’utilisateur qui n’existe plus peut toujours apparaître dans la liste d’adresses globale. Un troisième problème est qu’un groupe universel qui n’existe plus peut toujours apparaître dans le jeton d’accès d’un utilisateur. 
  
     Si vous avez restauré un contrôleur de service qui était un catalogue global, soit par inadvertance, soit par la sauvegarde solitaires que vous avez approuvée, nous vous recommandons d’empêcher l’occurrence d’objets en attente en désactivant le catalogue global peu de temps après l’opération de restauration. La désactivation de l’indicateur de catalogue global entraînera la perte par l’ordinateur de tous ses réplicas partiels (partitions) et de la redélégation à l’état de contrôleur de jeu normal. 
  
13. Configurez le service de temps Windows. Dans le domaine racine de forêt, configurez l’émulateur de contrôleur de domaine principal pour synchroniser l’heure à partir d’une source de temps externe. Pour plus d’informations, consultez [configurer le service de temps Windows sur l’émulateur de contrôleur de domaine principal dans le domaine racine de la forêt](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731191%28v=ws.10%29). 
  
## <a name="reconnect-each-restored-writeable-domain-controller-to-a-common-network"></a>Reconnecter chaque contrôleur de domaine inscriptible restauré à un réseau commun

À ce niveau, vous devez avoir un contrôleur de domaine restauré (et les étapes de récupération effectuées) dans le domaine racine de la forêt et dans chacun des domaines restants. Joignez ces contrôleurs de réseau à un réseau commun isolé du reste de l’environnement et suivez les étapes ci-dessous pour valider l’intégrité et la réplication de la forêt.

> [!NOTE]
> Quand vous joignez les contrôleurs de réseau physiques à un réseau isolé, vous devrez peut-être modifier leurs adresses IP. Par conséquent, les adresses IP des enregistrements DNS sont incorrectes. Comme un serveur de catalogue global n’est pas disponible, les mises à jour dynamiques sécurisées pour DNS échouent. Dans ce cas, les contrôleurs de réseau virtuels sont plus avantageux car ils peuvent être joints à un nouveau réseau virtuel sans modifier leurs adresses IP. C’est l’une des raisons pour lesquelles les contrôleurs de domaine virtuels sont recommandés comme premier contrôleur de domaine à restaurer au cours de la récupération de la forêt. 
  
Après la validation, joignez les contrôleurs de réseau au réseau de production et suivez les étapes pour vérifier l’intégrité de la réplication de la forêt.

- Pour résoudre le problème de résolution de noms, créez des enregistrements de délégation DNS et configurez le transfert DNS et les indications de racine en fonction des besoins. Exécutez **repadmin/replsum.** pour vérifier la réplication entre les contrôleurs de contrôle. 
- Si les contrôleurs de service restaurés ne sont pas des partenaires de réplication directe, la récupération de la réplication est beaucoup plus rapide en créant des objets de connexion temporaires entre eux. 
- Pour valider le nettoyage des métadonnées, exécutez **repadmin/viewlist \\** * pour obtenir la liste de tous les contrôleurs de contrôle dans la forêt. Exécutez **Nltest/DCList :** *<\>de domaine* pour obtenir la liste de tous les contrôleurs de domaine du domaine. 
- Pour vérifier l’intégrité du contrôleur de domaine et du DNS, exécutez DCDiag/v pour signaler les erreurs sur tous les contrôleurs de domaine de la forêt. 

## <a name="add-the-global-catalog-to-a-domain-controller-in-the-forest-root-domain"></a>Ajouter le catalogue global à un contrôleur de domaine dans le domaine racine de la forêt

Un catalogue global est nécessaire pour les raisons suivantes :  
  
- Pour activer les ouvertures de session pour les utilisateurs. 
- Pour activer le service d’ouverture de session réseau s’exécutant sur les contrôleurs de domaine dans chaque domaine enfant afin d’inscrire et de supprimer des enregistrements sur le serveur DNS dans le domaine racine. 
  
Bien qu’il soit préférable que le contrôleur de source de la forêt devienne un catalogue global, il est possible de choisir l’un des contrôleurs de service restaurés pour devenir un catalogue global. 
  
> [!NOTE]
> Un contrôleur de périphérique ne sera pas publié en tant que serveur de catalogue global tant qu’il n’aura pas effectué une synchronisation complète de toutes les partitions d’annuaire de la forêt. Par conséquent, le contrôleur de périphérique doit être forcé à répliquer avec chacun des contrôleurs de contrôle de la forêt. 
>
> Surveillez le journal des événements du service d’annuaire dans observateur d’événements pour l’ID d’événement 1119, qui indique que ce contrôleur de périphérique est un serveur de catalogue global, ou vérifiez que la clé de Registre suivante a la valeur 1 :  
>
> **Promotion du catalogue HKLM\System\CurrentControlSet\Services\NTDS\Parameters\Global terminée**  
  
Pour plus d’informations, consultez [Ajout du catalogue global](AD-Forest-Recovery-Add-GC.md). 
  
À ce niveau, vous devez disposer d’une forêt stable, avec un contrôleur de domaine pour chaque domaine et un catalogue global dans la forêt. Vous devez effectuer une nouvelle sauvegarde de chacun des contrôleurs de contrôle que vous venez de restaurer. Vous pouvez maintenant commencer à redéployer d’autres contrôleurs de contrôle dans la forêt en installant AD DS. 

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
