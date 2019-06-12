---
ms.assetid: 8a3cf2ae-2511-4eea-afd5-a43179a78613
title: Mises à jour du composant Directory Services
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: fe27b61abe196a2148ced18806be904ebd555fcc
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66442894"
---
# <a name="directory-services-component-updates"></a>Mises à jour du composant Directory Services

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

**Auteur**: Justin Turner, ingénieur Support résolution Senior auprès du groupe Windows  
  
> [!NOTE]  
> Ce contenu est écrit par un ingénieur du support client Microsoft et est destiné aux administrateurs expérimentés et aux architectes système qui recherchent des explications techniques plus approfondies des fonctionnalités et des solutions Windows Server 2012 R2 que n'en proposent généralement les rubriques de TechNet. Toutefois, il n'a pas subi les mêmes passes de correction. De ce fait, une partie du langage peut sembler moins finalisée que le contenu de TechNet.  
  
Cette leçon explique les mises à jour du composant de Services d’annuaire dans Windows Server 2012 R2.  
  
## <a name="what-you-will-learn"></a>Ce que vous allez apprendre  
Expliquez les nouvelles mises à jour des composants Services d’annuaire :  
  
-   Expliquez les nouvelles mises à jour des composants Services d’annuaire :  
  
    -   [Niveaux fonctionnels de domaines et de forêt](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_FL)  
  
    -   [Désapprobation de NTFRS](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_NTFRS)  
  
    -   [Modifications de l'optimiseur de requête LDAP](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_LDAPQuery)  
  
    -   [Améliorations de l'événement 1644](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_1644)  
  
    -   [Amélioration du débit de réplication Active Directory](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_ADRepl)  
  
## <a name="BKMK_FL"></a>Niveaux fonctionnels de domaine et de forêt  
  
### <a name="overview"></a>Vue d'ensemble  
La section fournit une brève introduction aux modifications au niveau fonctionnelles des domaines et forêts.  
  
### <a name="new-dfl-and-ffl"></a>FFL et le nouveau niveau fonctionnel du domaine  
Avec la version, il existe de nouveaux niveaux fonctionnels de domaine et de forêt :  
  
-   Niveau fonctionnel de forêt : Windows Server 2012 R2  
  
-   Niveau fonctionnel du domaine : Windows Server 2012 R2  
  
### <a name="the-windows-server-2012-r2-domain-functional-level-enables-support-for-the-following"></a>Windows Server 2012 R2 niveau fonctionnel du domaine permet la prise en charge pour les éléments suivants :  
  
1.  Les protections côté contrôleur de domaine pour *utilisateurs protégés*  
  
    *Les utilisateurs protégés par* l’authentification auprès d’un domaine Windows Server 2012 R2 peut **n’est plus**:  
  
    -   Authentifier avec l’authentification NTLM  
  
    -   Utilisez les suites de chiffrement DES ou RC4 dans la pré-authentification Kerberos  
  
    -   Être délégués avec délégation non contrainte ou contrainte  
  
    -   Renouveler les tickets utilisateur (TGT) au-delà de la durée de vie initiale de 4 heures  
  
2.  Stratégies d'authentification  
  
    Basée sur une nouvelle forêt Active Directory stratégies qui peuvent être appliquées aux comptes dans des domaines Windows Server 2012 R2 pour contrôler les hôtes un compte peut l’authentification à partir d’et appliquer des conditions de contrôle d’accès pour l’authentification pour les services qui s’exécutent en tant que compte  
  
3.  Silos de stratégies d'authentification  
  
    Nouvelle forêt Active Directory objet qui peut créer une relation entre les comptes d’utilisateur, service géré et ordinateur pour être utilisée pour classer les comptes pour les stratégies d’authentification ou pour l’isolation de l’authentification.  
  
Consultez Comment configurer des comptes protégés pour plus d’informations.  
  
Outre les fonctionnalités ci-dessus, le niveau fonctionnel du domaine Windows Server 2012 R2 permet de s’assurer que tout contrôleur de domaine dans le domaine exécute Windows Server 2012 R2.  
Le niveau fonctionnel de forêt Windows Server 2012 R2 ne fournit pas toutes les nouvelles fonctionnalités, mais il garantit que tout nouveau domaine créé dans la forêt fonctionne automatiquement au niveau fonctionnel du domaine Windows Server 2012 R2.  
  
### <a name="minimum-dfl-enforced-on-new-domain-creation"></a>Niveau fonctionnel du domaine minimale appliqué sur la création d’un domaine  
Le niveau fonctionnel du domaine Windows Server 2008 est le niveau fonctionnel minimal pris en charge sur la création d’un domaine.  
  
> [!NOTE]  
> La dépréciation de FRS s’effectue en supprimant la possibilité d’installer un nouveau domaine avec un niveau fonctionnel de domaine inférieur à Windows Server 2008 avec le Gestionnaire de serveur ou via Windows PowerShell.  
  
### <a name="lowering-the-forest-and-domain-functional-levels"></a>En réduisant les niveaux fonctionnels de forêt et du domaine  
Les niveaux fonctionnels de forêt et du domaine sont définies sur Windows Server 2012 R2 par défaut sur la création d’un domaine et de la nouvelle forêt, mais peuvent être réduits à l’aide de Windows PowerShell.  
  
Pour augmenter ou diminuer le niveau fonctionnel de forêt à l’aide de Windows PowerShell, utilisez le **Set-ADForestMode** applet de commande.  
  
**Pour définir le contoso.com FFL en mode Windows Server 2008 :**  
  
```sql  
Set-ADForestMode -ForestMode Windows2008Forest -Identity contoso.com  
```  
  
Pour augmenter ou diminuer le niveau fonctionnel de domaine à l’aide de Windows PowerShell, utilisez l’applet de commande Set-ADDomainMode.  
  
**Pour définir le niveau fonctionnel du domaine contoso.com en mode Windows Server 2008 :**  
  
```powershell  
Set-ADDomainMode -DomainMode Windows2008Domain -Identity contoso.com  
```  
  
Promotion d’un contrôleur de domaine exécutant Windows Server 2012 R2 en tant qu’un réplica supplémentaire dans un domaine existant en cours d’exécution 2003 DFL fonctionne.  
  
Création du nouveau domaine dans une forêt existante  
  
![mises à jour des services d’annuaire](media/Directory-Services-component-updates/GTR_ADDS_FFL.gif)  
  
### <a name="adprep"></a>ADPREP  
Il n’existe aucune nouvelle forêt ou des opérations de domaine dans cette version.  
  
Ces fichiers .ldf contiennent des modifications de schéma pour le **Device Registration Service**.  
  
1.  Sch59  
  
2.  Sch61  
  
3.  Sch62  
  
4.  Sch63  
  
5.  Sch64  
  
6.  Sch65  
  
7.  Sch67  
  
**Dossiers de travail :**  
  
1.  Sch66  
  
**MSODS :**  
  
1.  Sch60  
  
**Silos et stratégies d’authentification**  
  
1.  Sch68  
  
2.  Sch69  
  
## <a name="BKMK_NTFRS"></a>Désapprobation de NTFRS  
  
### <a name="overview"></a>Vue d'ensemble  
FRS est obsolète dans Windows Server 2012 R2.  La dépréciation de FRS s’effectue en appliquant un niveau fonctionnel de domaine minimal (DFL) de Windows Server 2008.  Cette mise en œuvre est présent uniquement si le nouveau domaine est créé à l’aide du Gestionnaire de serveur ou Windows PowerShell.  
  
Le paramètre - DomainMode et le Install-ADDSForest ou les applets de commande Install-ADDSDomain vous permettent de spécifier le niveau fonctionnel du domaine.  Les valeurs prises en charge pour ce paramètre peuvent être un entier valide ou une valeur correspondante de la chaîne énumérée. Par exemple, pour définir le niveau de mode de domaine vers Windows Server 2008 R2, vous pouvez spécifier une valeur de 4 ou à « Win2008R2 ».  Lors de l’exécution de ces applets de commande de Server 2012 R2 valide les valeurs sont celles pour Windows Server 2008 (3, Win2008) Windows Server 2008 R2 (4, Win2008R2) (5, Win2012) de Windows Server 2012 et Windows Server 2012 R2 (6, Win2012R2). Le niveau fonctionnel du domaine ne peut pas être inférieur à celui de la forêt, mais il peut être supérieur.  Dans la mesure où FRS est obsolète dans cette version, Windows Server 2003 (2, Win2003) n’est pas un paramètre reconnu avec ces applets de commande lors de l’exécution de Windows Server 2012 R2.  
  
![mises à jour des services d’annuaire](media/Directory-Services-component-updates/GTR_ADDS_PS_Install2003DFL.gif)  
  
![mises à jour des services d’annuaire](media/Directory-Services-component-updates/GTR_ADDS_PS_InstallDFL2.gif)  
  
## <a name="BKMK_LDAPQuery"></a>Modifications de l’optimiseur de requête LDAP  
  
### <a name="overview"></a>Vue d'ensemble  
L’algorithme d’optimiseur de requête LDAP a été réévaluée et optimisée.  Le résultat est l’amélioration des performances dans l’efficacité des recherches LDAP et la durée de recherche LDAP de requêtes complexes.  
  
> [!NOTE]
> <strong>Le développeur :</strong>améliorations dans les performances des recherches grâce aux améliorations apportées dans le mappage à partir de LDAP de requête à la requête du moteur ESE.  Au-delà d’un certain niveau de complexité des filtres LDAP empêchent la sélection d’index optimisé, ce qui entraîne des performances considérablement réduites (1 000 x ou plus). Ce changement modifie la façon dans lequel nous sélectionnons des indices pour les requêtes LDAP afin d’éviter ce problème.  
> 
> [!NOTE]
> Un remaniement complet de l’algorithme optimiseur de requête LDAP, ce qui entraîne :  
> 
> -   Temps de recherche plus rapides  
> -   Gains d’efficacité autorisent les DC à aller plus loin  
> -   Moins d’appels au support technique relatives aux performances d’Active Directory émet  
> -   Back déplacée vers Windows Server 2008 R2 (2862304 Ko)  
  
### <a name="background"></a>Arrière-plan  
La fonctionnalité de recherche Active Directory est un service principal fourni par les contrôleurs de domaine.  Autres services et les applications métier s’appuient sur des recherches Active Directory.  Opérations de l’entreprise peuvent cesser d’arrêter si cette fonctionnalité n’est pas disponible.  Comme un service très utilisé et de core, il est impératif que les contrôleurs de domaine gèrent efficacement le trafic de recherche LDAP.  L’algorithme d’optimiseur de requête LDAP tente d’effectuer les recherches LDAP efficaces que possible en mappant les filtres de recherche LDAP à un jeu de résultats qui peut être satisfait par le biais d’enregistrements déjà indexés dans la base de données.  Cet algorithme a été réévalué et optimisé.  Le résultat est l’amélioration des performances dans l’efficacité des recherches LDAP et la durée de recherche LDAP de requêtes complexes.  
  
### <a name="details-of-change"></a>Détails de la modification  
Une recherche LDAP contient :  
  
-   Un emplacement (head CN, unité d’organisation, objet) au sein de la hiérarchie pour commencer la recherche  
  
-   Un filtre de recherche  
  
-   Une liste d’attributs à retourner  
  
Le processus de recherche peut être résumé comme suit :  
  
1.  Simplifiez le filtre de recherche si possible.  
  
2.  Sélectionnez un jeu de clés d’Index qui retournera le plus petit jeu couvert.  
  
3.  Effectuer une ou plusieurs des intersections de clés d’Index, pour réduire le jeu couvert.  
  
4.  Pour chaque enregistrement dans le jeu couvert, évaluer l’expression de filtre, ainsi que la sécurité. Si le filtre a la valeur TRUE et que l’accès est accordé, puis revenez cet enregistrement pour le client.  
  
Le travail d’optimisation de requête LDAP modifie les étapes 2 et 3, afin de réduire la taille de la plage couverte. Plus précisément, l’implémentation actuelle sélectionne les clés d’Index en double et effectue des intersections redondantes.  
  
### <a name="comparison-between-old-and-new-algorithm"></a>Comparaison entre les ancienne et nouvel algorithme  
La cible de la recherche LDAP inefficace dans cet exemple est un contrôleur de domaine Windows Server 2012.  La recherche se termine en environ 44 secondes à la suite de ne pas trouver un index plus efficace.  
  
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
  
### <a name="sample-results-using-the-new-algorithm"></a>Exemples de résultats à l’aide de l’algorithme de nouvelles  
Cet exemple répète la même recherche exacte que ci-dessus, mais cible un contrôleur de domaine Windows Server 2012 R2.  La même recherche se termine en moins d’une seconde en raison des améliorations dans l’algorithme d’optimiseur de requête LDAP.  
  
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
  
-   S’il est impossible d’optimiser l’arborescence :  
  
    -   Par exemple : une expression dans l’arborescence a été sur une colonne non indexée  
  
    -   Enregistrer une liste d’index qui empêchent l’optimisation  
  
    -   Exposées via le suivi ETW et les ID d’événement 1644  
  
        ![mises à jour des services d’annuaire](media/Directory-Services-component-updates/GTR_ADDS_Event1644.gif)  
  
### <a name="BKMK_EnableStats"></a>Pour activer le contrôle de statistiques dans LDP  
  
1.  Ouvrez LDP.exe et vous connecter et lier à un contrôleur de domaine.  
  
2.  Sur le **Options** menu, cliquez sur **contrôles**.  
  
3.  Dans la boîte de dialogue de contrôles, développez le **Load Predefined** menu déroulant, cliquez sur **recherche statistiques** puis cliquez sur **OK**.  
  
    ![mises à jour des services d’annuaire](media/Directory-Services-component-updates/GTR_ADDS_Controls.gif)  
  
4.  Sur le **Parcourir** menu, cliquez sur **recherche**  
  
5.  Dans la boîte de dialogue de recherche, sélectionnez le **Options** bouton.  
  
6.  Vérifiez le **étendu** case à cocher est activée sur la boîte de dialogue Options de recherche, puis sélectionnez **OK**.  
  
    ![mises à jour des services d’annuaire](media/Directory-Services-component-updates/GTR_ADDS_SearchOptions.gif)  
  
### <a name="try-this-use-ldp-to-return-query-statistics"></a>Essayez ceci : Utiliser LDP pour retourner des statistiques de requête  
Procédez comme suit sur un contrôleur de domaine, ou à partir d’un client à un domaine ou un serveur qui a installé les outils AD DS.  Répétez ce qui suit ciblant votre contrôleur de domaine Windows Server 2012 et votre contrôleur de domaine Windows Server 2012 R2.  
  
1.  Examinez le [« Création plus efficace Microsoft AD activé Applications »](https://msdn.microsoft.com/library/ms808539.aspx) article et d’y revenir si nécessaire.  
  
2.  Utilisation de LDP, activer les statistiques de recherche (consultez [pour activer le contrôle de statistiques dans LDP](../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_EnableStats))  
  
3.  Pour effectuer des recherches LDAP plusieurs et observer les informations statistiques en haut des résultats.  Vous allez répéter la même recherche dans d’autres activités donc document dans un fichier texte du bloc-notes.  
  
4.  Effectuer une recherche LDAP que l’optimiseur de requête doit être en mesure d’optimiser en raison de l’index d’attributs  
  
5.  Tenter de construire une recherche qui prend beaucoup de temps (il pouvez que vous voulez augmenter la **limite de temps** option afin de la recherche n’expire pas).  
  
### <a name="additional-resources"></a>Ressources complémentaires  
[Quelles sont les recherches Active Directory ?](https://technet.microsoft.com/library/cc783845(v=ws.10).aspx)  
  
[Comment les recherches Active Directory](https://technet.microsoft.com/library/cc755809(v=WS.10).aspx)  
  
[Création d’Applications d’annuaire Microsoft Active plus efficaces](https://msdn.microsoft.com/library/ms808539.aspx)  
  
[951581](https://support.microsoft.com/kb/951581) requêtes LDAP sont exécutées plus lentement que prévu dans Active Directory ou le service d’annuaire AD LDS/ADAM et 1644 d’ID d’événement peuvent être consignés  
  
## <a name="BKMK_1644"></a>Événement 1644  
  
### <a name="overview"></a>Vue d'ensemble  
Cette mise à jour ajoute des statistiques supplémentaires des résultats de recherche LDAP pour l’ID d’événement 1644 pour vous aider à des fins de dépannage.  En outre, il est une nouvelle valeur de Registre qui peut être utilisée pour activer la journalisation sur un seuil basé sur le temps.  Ces améliorations ont été apportées disponibles dans Windows Server 2012 et Windows Server 2008 R2 SP1 via Ko [2800945](https://support.microsoft.com/kb/2800945) et sera disponible pour Windows Server 2008 SP2.  
  
> [!NOTE]  
> -   Les statistiques de recherche LDAP supplémentaires sont ajoutés à l’ID d’événement 1644 pour faciliter le dépannage des recherches LDAP inefficaces ou coûteuses  
> -   Vous pouvez désormais spécifier un seuil de temps de recherche (par exemple). Enregistrer l’événement 1644 pour les recherches prend plus de 100 ms) au lieu de spécifier les valeurs de seuil de résultat recherche cher et Inefficient  
  
### <a name="background"></a>Arrière-plan  
Lors de la résolution des problèmes de performances d’Active Directory, il devient évident qu’activité de recherche LDAP peut-être contribuer au problème.  Vous décidez d’activer la journalisation afin que vous puissiez voir coûteux ou inefficaces des requêtes LDAP traitées par le contrôleur de domaine.  Pour activer la journalisation, vous devez définir la valeur de diagnostics Field Engineering et pourrez éventuellement spécifier les valeurs de seuil des résultats de recherche coûteux / inefficace.  Lors de l’activation de l’ingénierie niveau de journalisation sur la valeur 5, une recherche qui répond à ces critères est consignée dans le journal des événements Services d’annuaire avec un ID d’événement 1644.  
  
L’événement contient :  
  
-   Client IP et port  
  
-   Nœud de démarrage  
  
-   Filter  
  
-   Étendue de recherche  
  
-   Sélection d’un attribut  
  
-   Contrôles serveur  
  
-   Entrées visitées  
  
-   Entrées retournées  
  
Toutefois, les données de clé sont manquantes à partir de l’événement telles que le temps passé sur l’opération de recherche et quel (le cas échéant) l’index a été utilisé.  
  
#### <a name="additional-search-statistics-added-to-event-1644"></a>Statistiques de recherche supplémentaires ajoutés à l’événement 1644  
  
-   Index utilisés  
  
-   Pages référencées  
  
-   Pages lues à partir du disque  
  
-   Pages prélecture à partir du disque  
  
-   Pages nettoyées modifiées  
  
-   Pages de modifications modifiées  
  
-   Délai de recherche  
  
-   Attributs empêchant l’optimisation  
  
#### <a name="new-time-based-threshold-registry-value-for-event-1644-logging"></a>Nouvelle valeur de Registre temporels de seuil pour la journalisation de l’événement 1644  
Au lieu de spécifier les valeurs de seuil de résultat recherche cher et Inefficient, vous pouvez spécifier le seuil de temps de recherche.  Si vous voulu pour vous connecter tous les résultats de recherche qui a duré 50 ms ou supérieur, vous devez spécifier 50 décimal / 32 hex (outre la définition de la valeur Field Engineering).  
  
```  
Windows Registry Editor Version 5.00  
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NTDS\Parameters]  
"Search Time Threshold (msecs)"=dword:00000032  
```  
  
#### <a name="comparison-of-the-old-and-new-event-id-1644"></a>Comparaison entre l’ancien et nouveaux ID d’événement 1644  
ANCIEN  
  
![mises à jour des services d’annuaire](media/Directory-Services-component-updates/GTR_ADDS_Event1644_2012.gif)  
  
NOUVEAU  
  
![mises à jour des services d’annuaire](media/Directory-Services-component-updates/GTR_ADDS_Event1644_2012R2.gif)  
  
#### <a name="try-this-use-the-event-log-to-return-query-statistics"></a>Essayez ceci : Utiliser le journal des événements pour retourner des statistiques de requête  
  
1.  Répétez ce qui suit ciblant votre contrôleur de domaine Windows Server 2012 et votre contrôleur de domaine Windows Server 2012 R2. Observez l’événement ID 1644s sur les deux contrôleurs de domaine après chaque recherche.  
  
2.  Regedit, activez la journalisation d’ID d’événement 1644 à l’aide d’un seuil basés sur le contrôleur de domaine Windows Server 2012 R2 et de l’ancienne méthode sur le contrôleur de domaine Windows Server 2012.  
  
3.  Pour effectuer des recherches LDAP plusieurs qui dépassent le seuil et observer les informations statistiques en haut des résultats.  Utiliser les requêtes LDAP que vous documenté précédemment et répétez les mêmes recherches.  
  
4.  Effectuer une recherche LDAP que l’optimiseur de requête n’est pas en mesure d’optimiser, car un ou plusieurs attributs ne sont pas indexés.  
  
## <a name="BKMK_ADRepl"></a>Amélioration du débit de réplication d’annuaire Active  
  
### <a name="overview"></a>Vue d'ensemble  
La réplication Active Directory utilise RPC pour son transport de réplication. Par défaut, RPC utilise un tampon de transmission de 8 Ko et une taille de paquet 5 Ko. Cela a pour effet net où l’instance envoi transmettre des paquets de trois (environ 15K de données), puis devoir attendre pour un réseau complet avant d’envoyer plus. En supposant un 3ms temps d’aller-retour, le débit le plus élevé serait d’environ 40Mbps, même sur les réseaux de 10 Gbits/s ou de 1 Gbit/s.  
  
> [!NOTE]  
> -   Cette mise à jour ajuste le débit maximal de la réplication Active Directory à partir de 40Mbps à environ 600 Mbits/s.  
>   
>     -   Elle augmente la taille de mémoire tampon d’envoi RPC ce qui réduit le nombre de réseau les allers-retours  
> -   L’effet sera plus perceptible sur haute vitesse, réseau à latence élevée.  
  
Cette mises à jour augmentent le débit maximal à environ 600 Mbits/s en modifiant la taille de mémoire tampon d’envoi RPC à partir de 8 Ko à 256 Ko.  Cette modification permet à la taille de fenêtre TCP à croître au-delà de 8K, réduisant le nombre de réseau des boucles.  
  
> [!NOTE]  
> Il n’existe aucun paramètre configurable pour modifier ce comportement.  
  
### <a name="additional-resources"></a>Ressources complémentaires  
[Fonctionnement du modèle de réplication Active Directory](https://technet.microsoft.com/library/cc772726(v=WS.10).aspx)  
  


