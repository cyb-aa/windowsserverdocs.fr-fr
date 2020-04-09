---
ms.assetid: 8a3cf2ae-2511-4eea-afd5-a43179a78613
title: Mises à jour des composants Services d'annuaire
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: cde839feda47d55415b2b6cc1026a7a3e6515a44
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80823092"
---
# <a name="directory-services-component-updates"></a>Mises à jour des composants Services d'annuaire

>S’applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

**Auteur**: Justin Turner, ingénieur du support technique senior avec le groupe Windows  
  
> [!NOTE]  
> Ce contenu est écrit par un ingénieur du support client Microsoft et est destiné aux administrateurs expérimentés et aux architectes système qui recherchent des explications techniques plus approfondies des fonctionnalités et des solutions Windows Server 2012 R2 que n'en proposent généralement les rubriques de TechNet. Toutefois, il n'a pas subi les mêmes passes de correction. De ce fait, une partie du langage peut sembler moins finalisée que le contenu de TechNet.  
  
Cette leçon explique les mises à jour des composants des services d’annuaire dans Windows Server 2012 R2.  
  
## <a name="what-you-will-learn"></a>Ce que vous allez apprendre  
Expliquez les nouvelles mises à jour des composants des services d’annuaire suivants :  
  
-   Expliquez les nouvelles mises à jour des composants des services d’annuaire suivants :  
  
    -   [Niveaux fonctionnels de domaines et de forêt](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_FL)  
  
    -   [Désapprobation de NTFRS](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_NTFRS)  
  
    -   [Modifications de l'optimiseur de requête LDAP](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_LDAPQuery)  
  
    -   [Améliorations de l'événement 1644](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_1644)  
  
    -   [Amélioration du débit de réplication Active Directory](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_ADRepl)  
  
## <a name="domain-and-forest-functional-levels"></a><a name="BKMK_FL"></a>Niveaux fonctionnels de domaine et de forêt  
  
### <a name="overview"></a>Overview  
La section fournit une brève introduction aux modifications apportées au niveau fonctionnel du domaine et de la forêt.  
  
### <a name="new-dfl-and-ffl"></a>Nouveaux DFL et FFL  
Avec la version, il existe de nouveaux niveaux fonctionnels de domaine et de forêt :  
  
-   Niveau fonctionnel de la forêt : Windows Server 2012 R2  
  
-   Niveau fonctionnel du domaine : Windows Server 2012 R2  
  
### <a name="the-windows-server-2012-r2-domain-functional-level-enables-support-for-the-following"></a>Le niveau fonctionnel de domaine Windows Server 2012 R2 permet la prise en charge des éléments suivants :  
  
1.  Protections côté DC pour *les utilisateurs protégés*  
  
    *Les utilisateurs protégés* qui s’authentifient auprès d’un domaine Windows Server 2012 R2 **ne peuvent plus**:  
  
    -   s’authentifier avec l’authentification NTLM ;  
  
    -   utiliser les suites de chiffrement DES ou RC4 dans la pré-authentification Kerberos ;  
  
    -   être délégués en utilisant la délégation non contrainte ou contrainte ;  
  
    -   renouveler les tickets TGT utilisateur au-delà de la durée de vie initiale de 4 heures.  
  
2.  Stratégies d'authentification  
  
    Nouvelles stratégies de Active Directory basées sur les forêts qui peuvent être appliquées aux comptes dans les domaines Windows Server 2012 R2 pour contrôler les hôtes auxquels un compte peut se connecter et appliquer des conditions de contrôle d’accès pour l’authentification aux services s’exécutant en tant que compte  
  
3.  Silos de stratégies d'authentification  
  
    Nouvel objet de Active Directory basé sur une forêt qui peut créer une relation entre l’utilisateur, les comptes de service administrés et les comptes d’ordinateur à utiliser pour classer les comptes pour les stratégies d’authentification ou pour l’isolation d’authentification.  
  
Pour plus d’informations, voir How to configure protected Accounts.  
  
Outre les fonctionnalités ci-dessus, le niveau fonctionnel de domaine Windows Server 2012 R2 garantit que tous les contrôleurs de domaine du domaine exécutent Windows Server 2012 R2.  
Le niveau fonctionnel de la forêt Windows Server 2012 R2 ne fournit aucune nouvelle fonctionnalité, mais il garantit que tout nouveau domaine créé dans la forêt fonctionne automatiquement au niveau fonctionnel du domaine Windows Server 2012 R2.  
  
### <a name="minimum-dfl-enforced-on-new-domain-creation"></a>DFL minimale appliquée lors de la création d’un nouveau domaine  
Windows Server 2008 DFL est le niveau fonctionnel minimal pris en charge lors de la création d’un nouveau domaine.  
  
> [!NOTE]  
> La désapprobation du service FRS s’effectue en supprimant la possibilité d’installer un nouveau domaine avec un niveau fonctionnel de domaine inférieur à Windows Server 2008 avec Gestionnaire de serveur ou via Windows PowerShell.  
  
### <a name="lowering-the-forest-and-domain-functional-levels"></a>Réduction des niveaux fonctionnels de forêt et de domaine  
Les niveaux fonctionnels de forêt et de domaine sont définis sur Windows Server 2012 R2 par défaut sur le nouveau domaine et la création d’une nouvelle forêt, mais peuvent être abaissés à l’aide de Windows PowerShell.  
  
Pour augmenter ou diminuer le niveau fonctionnel de la forêt à l’aide de Windows PowerShell, utilisez l’applet de commande **Set-ADForestMode** .  
  
**Pour définir le contoso.com FFL en mode Windows Server 2008 :**  
  
```sql  
Set-ADForestMode -ForestMode Windows2008Forest -Identity contoso.com  
```  
  
Pour augmenter ou diminuer le niveau fonctionnel du domaine à l’aide de Windows PowerShell, utilisez l’applet de commande Set-ADDomainMode.  
  
**Pour définir le mode contoso.com DFL sur Windows Server 2008, procédez comme suit :**  
  
```powershell  
Set-ADDomainMode -DomainMode Windows2008Domain -Identity contoso.com  
```  
  
La promotion d’un contrôleur de domaine exécutant Windows Server 2012 R2 en tant que réplica supplémentaire dans un domaine existant exécutant 2003 DFL fonctionne.  
  
Création d’un nouveau domaine dans une forêt existante  
  
![mises à jour des services d’annuaire](media/Directory-Services-component-updates/GTR_ADDS_FFL.gif)  
  
### <a name="adprep"></a>Prep  
Cette version ne contient aucune nouvelle opération de forêt ou de domaine.  
  
Ces fichiers. ldf contiennent des modifications de schéma pour **Device Registration service**.  
  
1.  Sch59  
  
2.  Sch61  
  
3.  Sch62  
  
4.  Sch63  
  
5.  Sch64  
  
6.  Sch65  
  
7.  Sch67  
  
**Dossiers de travail :**  
  
1.  Sch66  
  
**MSODS**  
  
1.  Sch60  
  
**Stratégies et silos d’authentification**  
  
1.  Sch68  
  
2.  Sch69  
  
## <a name="deprecation-of-ntfrs"></a><a name="BKMK_NTFRS"></a>Dépréciation de NTFRS  
  
### <a name="overview"></a>Overview  
FRS est déconseillé dans Windows Server 2012 R2.  La désapprobation du service FRS s’effectue en appliquant un niveau fonctionnel de domaine minimal (DFL) de Windows Server 2008.  Cette mise en application est présente uniquement si le nouveau domaine est créé à l’aide de Gestionnaire de serveur ou de Windows PowerShell.  
  
Pour spécifier le niveau fonctionnel du domaine, utilisez le paramètre-DomainMode avec les applets de commande Install-ADDSForest ou Install-ADDSDomain.  Les valeurs prises en charge pour ce paramètre peuvent être un entier valide ou une valeur de chaîne énumérée correspondante. Par exemple, pour définir le niveau du mode de domaine sur Windows Server 2008 R2, vous pouvez spécifier une valeur de 4 ou « Win2008R2 ».  Lors de l’exécution de ces applets de commande à partir du serveur 2012 R2, les valeurs valides sont les suivantes : Windows Server 2008 (3, Win2008) Windows Server 2008 R2 (4, Win2008R2) Windows Server 2012 (5, Win2012) et Windows Server 2012 R2 (6, Win2012R2). Le niveau fonctionnel du domaine ne peut pas être inférieur à celui de la forêt, mais il peut être supérieur.  Étant donné que le service FRS est déconseillé dans cette version, Windows Server 2003 (2, Win2003) n’est pas un paramètre reconnu avec ces applets de commande lorsqu’il est exécuté à partir de Windows Server 2012 R2.  
  
![mises à jour des services d’annuaire](media/Directory-Services-component-updates/GTR_ADDS_PS_Install2003DFL.gif)  
  
![mises à jour des services d’annuaire](media/Directory-Services-component-updates/GTR_ADDS_PS_InstallDFL2.gif)  
  
## <a name="ldap-query-optimizer-changes"></a><a name="BKMK_LDAPQuery"></a>Modifications de l’optimiseur de requête LDAP  
  
### <a name="overview"></a>Overview  
L’algorithme de l’optimiseur de requête LDAP a été réévalué et optimisé.  Le résultat est l’amélioration des performances de l’efficacité de la recherche LDAP et du temps de recherche LDAP pour les requêtes complexes.  
  
> [!NOTE]
> Du <strong>développeur :</strong>améliorations des performances des recherches à l’aide des améliorations apportées à la requête mappage à partir d’une requête LDAP à une requête ESE.  Les filtres LDAP au-delà d’un certain niveau de complexité empêchent la sélection d’index optimisés, ce qui réduit considérablement les performances (1 000 fois ou plus). Cette modification modifie la façon dont nous sélectionnons les index pour les requêtes LDAP afin d’éviter ce problème.  
> 
> [!NOTE]
> Une révision complète de l’algorithme de l’optimiseur de requête LDAP, ce qui donne :  
> 
> -   Temps de recherche plus rapide  
> -   Les gains d’efficacité permettent d’effectuer davantage de contrôleurs de contrôle  
> -   Moins d’appels au support technique concernant les problèmes de performances AD  
> -   Arrière-plan porté sur Windows Server 2008 R2 (KB 2862304)  
  
### <a name="background"></a>Arrière-plan  
La possibilité de rechercher des Active Directory est un service principal fourni par les contrôleurs de domaine.  D’autres services et applications sectorielles s’appuient sur des recherches Active Directory.  Les opérations d’entreprise peuvent cesser d’être arrêtées si cette fonctionnalité n’est pas disponible.  En tant que service de base et fortement utilisé, il est impératif que les contrôleurs de domaine gèrent efficacement le trafic de recherche LDAP.  L’algorithme de l’optimiseur de requête LDAP tente d’optimiser les recherches LDAP en mappant les filtres de recherche LDAP à un jeu de résultats qui peut être satisfait via des enregistrements déjà indexés dans la base de données.  Cet algorithme a été réévalué et optimisé.  Le résultat est l’amélioration des performances de l’efficacité de la recherche LDAP et du temps de recherche LDAP pour les requêtes complexes.  
  
### <a name="details-of-change"></a>Détails de la modification  
Une recherche LDAP contient les éléments suivants :  
  
-   Emplacement (en-tête NC, UO, objet) dans la hiérarchie pour commencer la recherche  
  
-   Filtre de recherche  
  
-   Liste d’attributs à retourner.  
  
Le processus de recherche peut être résumé comme suit :  
  
1.  Simplifiez le filtre de recherche si possible.  
  
2.  Sélectionnez un jeu de clés d’index qui renverra le plus petit ensemble couvert.  
  
3.  Effectuez une ou plusieurs intersections des clés d’index pour réduire le jeu couvert.  
  
4.  Pour chaque enregistrement dans le jeu couvert, évaluez l’expression de filtre ainsi que la sécurité. Si le filtre prend la valeur TRUE et que l’accès est accordé, retourne cet enregistrement au client.  
  
Le travail d’optimisation des requêtes LDAP modifie les étapes 2 et 3, afin de réduire la taille du jeu couvert. Plus précisément, l’implémentation actuelle sélectionne des clés d’index dupliquées et effectue des intersections redondantes.  
  
### <a name="comparison-between-old-and-new-algorithm"></a>Comparaison entre l’ancien et le nouvel algorithme  
La cible de la recherche LDAP inefficace dans cet exemple est un contrôleur de domaine Windows Server 2012.  La recherche se termine dans environ 44 secondes suite à un échec de la recherche d’un index plus efficace.  
  
```  
adfind -b dc=blue,dc=contoso,dc=com -f "(| (& (|(cn=justintu) (postalcode=80304) (userprincipalname=justintu@blue.contoso.com)) (|(objectclass=person) (cn=justintu)) ) (&(cn=justintu)(objectclass=person)))" -stats >>adfind.txt  
  
Using server: WINSRV-DC1.blue.contoso.com:389  
  
<removed search results>  
  
Statistics  
=====  
Elapsed Time: 44640 (ms)  
Returned 324 entries of 553896 visited - (0.06%)  
  
Used Filter:  
 ( |  ( &  ( |  (cn=justintu)  (postalCode=80304)  (userPrincipalName=justintu@blue.contoso.com) )  ( |  (objectClass=person)  (cn=justintu) ) )  ( &  (cn=justintu)  (objectClass=person) ) )   
  
Used Indices:  
 DNT_index:516615:N  
  
Pages Referenced          : 4619650  
Pages Read From Disk      : 973  
Pages Pre-read From Disk  : 180898  
Pages Dirtied             : 0  
Pages Re-Dirtied          : 0  
Log Records Generated     : 0  
Log Record Bytes Generated: 0  
```  
  
### <a name="sample-results-using-the-new-algorithm"></a>Exemples de résultats utilisant le nouvel algorithme  
Cet exemple répète exactement la même recherche que ci-dessus, mais cible un contrôleur de domaine Windows Server 2012 R2.  La même recherche se termine en moins d’une seconde en raison des améliorations apportées à l’algorithme de l’optimiseur de requête LDAP.  
  
```  
adfind -b dc=blue,dc=contoso,dc=com -f "(| (& (|(cn=justintu) (postalcode=80304) (userprincipalname=dhunt@blue.contoso.com)) (|(objectclass=person) (cn=justintu)) ) (&(cn=justintu)(objectclass=person)))" -stats >>adfindBLUE.txt  
  
Using server: winblueDC1.blue.contoso.com:389  
  
.<removed search results>  
  
Statistics  
=====  
Elapsed Time: 672 (ms)  
Returned 324 entries of 648 visited - (50.00%)  
  
Used Filter:  
 ( |  ( &  ( |  (cn=justintu)  (postalCode=80304)  (userPrincipalName=justintu@blue.contoso.com) )  ( |  (objectClass=person)  (cn=justintu) ) )  ( &  (cn=justintu)  (objectClass=person) ) )   
  
Used Indices:  
 idx_userPrincipalName:648:N  
 idx_postalCode:323:N  
 idx_cn:1:N  
  
Pages Referenced          : 15350  
Pages Read From Disk      : 176  
Pages Pre-read From Disk  : 2  
Pages Dirtied             : 0  
Pages Re-Dirtied          : 0  
Log Records Generated     : 0  
Log Record Bytes Generated: 0  
```  
  
-   Si l’optimisation de l’arborescence est impossible :  
  
    -   Par exemple : une expression de l’arborescence était sur une colonne non indexée  
  
    -   Enregistrer une liste d’index qui empêchent l’optimisation  
  
    -   Exposé via le suivi ETW et l’ID d’événement 1644  
  
        ![mises à jour des services d’annuaire](media/Directory-Services-component-updates/GTR_ADDS_Event1644.gif)  
  
### <a name="to-enable-the-stats-control-in-ldp"></a><a name="BKMK_EnableStats"></a>Pour activer le contrôle stats dans LDP  
  
1.  Ouvrez LDP. exe, puis connectez-vous et liez un contrôleur de domaine.  
  
2.  Dans le menu **options** , cliquez sur **contrôles**.  
  
3.  Dans la boîte de dialogue contrôles, développez le menu déroulant **charger** , cliquez sur **statistiques de recherche** , puis cliquez sur **OK**.  
  
    ![mises à jour des services d’annuaire](media/Directory-Services-component-updates/GTR_ADDS_Controls.gif)  
  
4.  Dans le menu **Parcourir** , cliquez sur **Rechercher** .  
  
5.  Dans la boîte de dialogue Rechercher, sélectionnez le bouton **options** .  
  
6.  Vérifiez que la case à cocher **étendue** est activée dans la boîte de dialogue Options de recherche et sélectionnez **OK**.  
  
    ![mises à jour des services d’annuaire](media/Directory-Services-component-updates/GTR_ADDS_SearchOptions.gif)  
  
### <a name="try-this-use-ldp-to-return-query-statistics"></a>Essayez ceci : utilisez LDP pour retourner des statistiques sur les requêtes  
Procédez comme suit sur un contrôleur de domaine, ou à partir d’un client ou d’un serveur joint à un domaine sur lequel les outils de AD DS installés.  Répétez les étapes suivantes pour cibler votre contrôleur de Windows Server 2012 et votre contrôleur de Windows Server 2012 R2.  
  
1.  Passez en revue l’article [« création d’applications Microsoft ad compatibles plus efficaces »](https://msdn.microsoft.com/library/ms808539.aspx) et reportez-vous à cette rubrique en fonction des besoins.  
  
2.  À l’aide de LDP, activez les statistiques de recherche (consultez [pour activer le contrôle des statistiques dans LDP](../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_EnableStats)).  
  
3.  Exécutez plusieurs recherches LDAP et observez les informations statistiques en haut des résultats.  Vous allez répéter la même recherche dans d’autres activités pour les documenter dans un fichier texte du bloc-notes.  
  
4.  Effectuer une recherche LDAP que l’optimiseur de requête peut optimiser en raison des index d’attributs  
  
5.  Essayez de créer une recherche qui prend beaucoup de temps pour s’exécuter (vous pouvez augmenter l’option de **limite de temps** pour que la recherche n’expire pas).  
  
### <a name="additional-resources"></a>Ressources supplémentaires  
[Que sont les recherches Active Directory ?](https://technet.microsoft.com/library/cc783845(v=ws.10).aspx)  
  
[Comment Active Directory les recherches dans le travail](https://technet.microsoft.com/library/cc755809(v=WS.10).aspx)  
  
[Création d’applications Microsoft Active Directory plus efficaces](https://msdn.microsoft.com/library/ms808539.aspx)  
  
[951581](https://support.microsoft.com/kb/951581) les requêtes LDAP sont exécutées plus lentement que prévu dans le service d’annuaire AD ou AD LDS/Adam et l’ID d’événement 1644 peut être enregistré  
  
## <a name="1644-event-improvements"></a><a name="BKMK_1644"></a>améliorations des événements 1644  
  
### <a name="overview"></a>Overview  
Cette mise à jour ajoute des statistiques de résultats de recherche LDAP supplémentaires à l’ID d’événement 1644 afin de faciliter la résolution des problèmes.  En outre, il existe une nouvelle valeur de Registre qui peut être utilisée pour activer la journalisation sur un seuil basé sur la durée.  Ces améliorations ont été apportées dans Windows Server 2012 et Windows Server 2008 R2 SP1 via KB [2800945](https://support.microsoft.com/kb/2800945) et seront mises à la disposition de windows Server 2008 SP2.  
  
> [!NOTE]  
> -   Des statistiques de recherche LDAP supplémentaires sont ajoutées à l’ID d’événement 1644 pour aider à résoudre les problèmes de recherche LDAP inefficaces ou coûteux  
> -   Vous pouvez maintenant spécifier un seuil de temps de recherche (par exemple, Journalisation des événements 1644 pour les recherches qui prennent plus de 100 millisecondes) au lieu de spécifier les valeurs de seuil de résultat de recherche coûteuses et inefficaces  
  
### <a name="background"></a>Arrière-plan  
Lors de la résolution des problèmes de performances de Active Directory, il devient évident que l’activité de recherche LDAP peut contribuer au problème.  Vous décidez d’activer la journalisation pour que vous puissiez voir les requêtes LDAP coûteuses ou inefficaces traitées par le contrôleur de domaine.  Pour activer la journalisation, vous devez définir la valeur de diagnostics de l’ingénierie de champ et éventuellement spécifier les valeurs de seuil des résultats de recherche coûteux/inefficaces.  Lorsque vous activez le niveau de journalisation de l’ingénierie de champs à 5, toute recherche qui répond à ces critères est consignée dans le journal des événements des services d’annuaire avec l’ID d’événement 1644.  
  
L’événement contient :  
  
-   Adresse IP et port du client  
  
-   Nœud de départ  
  
-   Filtre  
  
-   Étendue de recherche  
  
-   Sélection d’attribut  
  
-   Contrôles serveur  
  
-   Entrées visitées  
  
-   Entrées retournées  
  
Toutefois, les données clés sont manquantes dans l’événement, telles que la durée de l’opération de recherche et l’index (le cas échéant) utilisé.  
  
#### <a name="additional-search-statistics-added-to-event-1644"></a>Statistiques de recherche supplémentaires ajoutées à l’événement 1644  
  
-   Index utilisés  
  
-   Pages référencées  
  
-   Pages lues à partir du disque  
  
-   Pages prélues à partir du disque  
  
-   Nettoyer les pages modifiées  
  
-   Pages modifiées modifiées  
  
-   Durée de la recherche  
  
-   Attributs empêchant l’optimisation  
  
#### <a name="new-time-based-threshold-registry-value-for-event-1644-logging"></a>Valeur de Registre du nouveau seuil basé sur l’heure pour la journalisation de l’événement 1644  
Au lieu de spécifier les valeurs de seuil de résultat de recherche coûteuses et inefficaces, vous pouvez spécifier le seuil de temps de recherche.  Si vous souhaitez consigner tous les résultats de recherche qui ont pris 50 ms ou une version ultérieure, vous devez spécifier 50 décimal/32 hex (en plus de la valeur de l’ingénierie des champs).  
  
```  
Windows Registry Editor Version 5.00  
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NTDS\Parameters]  
"Search Time Threshold (msecs)"=dword:00000032  
```  
  
#### <a name="comparison-of-the-old-and-new-event-id-1644"></a>Comparaison de l’ancien et du nouvel ID d’événement 1644  
ANCIEN  
  
![mises à jour des services d’annuaire](media/Directory-Services-component-updates/GTR_ADDS_Event1644_2012.gif)  
  
NOUVEAU  
  
![mises à jour des services d’annuaire](media/Directory-Services-component-updates/GTR_ADDS_Event1644_2012R2.gif)  
  
#### <a name="try-this-use-the-event-log-to-return-query-statistics"></a>Essayez ceci : utilisez le journal des événements pour retourner les statistiques des requêtes  
  
1.  Répétez les étapes suivantes pour cibler votre contrôleur de Windows Server 2012 et votre contrôleur de Windows Server 2012 R2. Observez l’ID d’événement 1644s sur les deux contrôleurs de contrôleur après chaque recherche.  
  
2.  À l’aide de regedit, activez la journalisation de l’ID d’événement 1644 à l’aide d’un seuil basé sur la durée sur le contrôleur de Windows Server 2012 R2 et l’ancienne méthode sur le contrôleur de Windows Server 2012.  
  
3.  Exécutez plusieurs recherches LDAP qui dépassent le seuil et observez les informations statistiques en haut des résultats.  Utilisez les requêtes LDAP que vous avez documentées précédemment et répétez les mêmes recherches.  
  
4.  Effectuez une recherche LDAP que l’optimiseur de requête ne peut pas optimiser, car un ou plusieurs attributs ne sont pas indexés.  
  
## <a name="active-directory-replication-throughput-improvement"></a><a name="BKMK_ADRepl"></a>Amélioration du débit de réplication Active Directory  
  
### <a name="overview"></a>Overview  
La réplication Active Directory utilise RPC pour le transport de réplication. Par défaut, RPC utilise une mémoire tampon de transmission de 8 Ko et une taille de paquet de 5 Ko. Cela a pour effet de faire en sorte que l’instance émettrice transmette trois paquets (environ 15 k de données) et qu’elle doit ensuite attendre un aller-retour réseau avant d’en envoyer davantage. En supposant un temps d’aller-retour 3 ms, le débit le plus élevé est de Active Directory maximal, même sur des réseaux 1Gbps ou 10 Gbit/s.  
  
> [!NOTE]  
> -   Cette mise à jour ajuste le débit de réplication Active Directory maximal de Active Directory maximal à environ 600 Mbits/s.  
>   
>     -   Elle augmente la taille de la mémoire tampon d’envoi RPC, ce qui réduit le nombre de boucles réseau.  
> -   L’effet sera le plus perceptible sur un réseau à haut débit et à latence élevée.  
  
Cette mise à jour augmente le débit maximal à environ 600 Mbits/s en remplaçant la taille de la mémoire tampon d’envoi RPC comprise entre 8 Ko et 256ko.  Cette modification permet à la taille de la fenêtre TCP de croître au-delà de 8 Ko, ce qui réduit le nombre d’allers-retours réseau.  
  
> [!NOTE]  
> Il n’existe aucun paramètre configurable pour modifier ce comportement.  
  
### <a name="additional-resources"></a>Ressources supplémentaires  
[Fonctionnement du modèle de réplication Active Directory](https://technet.microsoft.com/library/cc772726(v=WS.10).aspx)  
  


