---
ms.assetid: 8a3cf2ae-2511-4eea-afd5-a43179a78613
title: "Mises à jour du composant Directory Services"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ac42591450038240ced273555fb01e66b1ff5546
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="directory-services-component-updates"></a>Mises à jour du composant Directory Services

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

**Auteur**: Justin Turner, ingénieur Support résolution Senior avec le groupe de Windows  
  
> [!NOTE]  
> Ce contenu est écrit par un ingénieur du support client Microsoft et est destiné aux administrateurs expérimentés et architectes système qui recherchent des explications techniques plus approfondies des fonctionnalités et des solutions Windows Server2012R2 que proposent généralement les rubriques sur TechNet. Toutefois, il n’a pas subi les mêmes passes, afin de la partie du langage peut sembler que moins finalisée que ce qui se trouvent généralement sur TechNet.  
  
Elle explique les mises à jour des composants Services ActiveDirectory dans Windows Server2012R2.  
  
## <a name="what-you-will-learn"></a>Ce que vous allez apprendre  
Expliquez les nouvelles mises à jour des composants Services d’annuaire:  
  
-   Expliquez les nouvelles mises à jour des composants Services d’annuaire:  
  
    -   [Niveaux fonctionnels de domaine et de forêt](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_FL)  
  
    -   [Désapprobation de NTFRS](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_NTFRS)  
  
    -   [Modifications de l’optimiseur de requête LDAP](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_LDAPQuery)  
  
    -   [Événement1644](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_1644)  
  
    -   [Amélioration du débit de réplication ActiveDirectory](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_ADRepl)  
  
## <a name="BKMK_FL"></a>Niveaux fonctionnels de domaine et de forêt  
  
### <a name="overview"></a>Vue d’ensemble  
La section fournit une brève présentation des forêt et un domaine modifications au niveau fonctionnelles.  
  
### <a name="new-dfl-and-ffl"></a>FFL et nouveau niveau fonctionnel du domaine  
Avec la version, il existe de nouveaux niveaux fonctionnels de domaine et de forêt:  
  
-   Niveau fonctionnel de forêt: Windows Server2012R2  
  
-   Niveau fonctionnel du domaine: Windows Server2012R2  
  
### <a name="the-windows-server-2012-r2-domain-functional-level-enables-support-for-the-following"></a>Windows Server2012R2 niveau fonctionnel du domaine permet la prise en charge pour les éléments suivants:  
  
1.  Protections côté contrôleur de domaine pour *utilisateurs protégés*  
  
    *Protégé par les utilisateurs* l’authentification à un domaine Windows Server2012R2 peut **n’est plus**:  
  
    -   S’authentifier avec l’authentification NTLM  
  
    -   Utiliser les suites de chiffrement DES ou RC4 dans la pré-authentification Kerberos  
  
    -   Être délégués avec délégation non contrainte ou contrainte  
  
    -   Renouveler les tickets utilisateur (TGT) au-delà de la durée de vie initiale de 4 heures  
  
2.  Stratégies d’authentification  
  
    Nouvelle forêt ActiveDirectory stratégies qui peuvent être appliquées aux comptes dans des domaines Windows Server2012R2 pour contrôler les hôtes un compte peut ouverture de session à partir d’et appliquer des conditions de contrôle d’accès pour l’authentification aux services en cours d’exécution en tant que compte  
  
3.  Silos de stratégies d’authentification  
  
    Nouvelle forêt ActiveDirectory objet qui peut créer une relation entre l’utilisateur, service géré et comptes d’ordinateur pour être utilisées pour classer les comptes pour les stratégies d’authentification ou pour l’isolation de l’authentification.  
  
Voir comment configurer des comptes protégés pour plus d’informations.  
  
Outre les fonctionnalités ci-dessus, le niveau fonctionnel du domaine Windows Server2012R2 permet de s’assurer que tout contrôleur de domaine dans le domaine exécute Windows Server2012R2.  
Le niveau fonctionnel de forêt Windows Server2012R2 ne fournit pas de nouvelles fonctionnalités, mais il garantit que tout nouveau domaine créé dans la forêt fonctionne automatiquement au niveau fonctionnel du domaine Windows Server2012R2.  
  
### <a name="minimum-dfl-enforced-on-new-domain-creation"></a>Niveau fonctionnel du domaine minimal appliqué sur la création du nouveau domaine  
Le niveau fonctionnel du domaine Windows Server2008 est le niveau fonctionnel minimal pris en charge sur la création du nouveau domaine.  
  
> [!NOTE]  
> La désapprobation des FRS est obtenue en supprimant la possibilité d’installer un nouveau domaine avec un niveau fonctionnel du domaine inférieur à Windows Server2008 avec le Gestionnaire de serveur ou via Windows PowerShell.  
  
### <a name="lowering-the-forest-and-domain-functional-levels"></a>Ce qui réduit les niveaux fonctionnels de forêt et du domaine  
Les niveaux fonctionnels de forêt et du domaine sont définis pour Windows Server2012R2 par défaut sur la création du nouveau domaine et de la nouvelle forêt, mais peuvent être réduits à l’aide de Windows PowerShell.  
  
Pour augmenter ou diminuer le niveau fonctionnel de forêt à l’aide de Windows PowerShell, utilisez le **Set-ADForestMode** applet de commande.  
  
**Pour définir le contoso.com FFL en mode Windows Server2008:**  
  
```sql  
Set-ADForestMode -ForestMode Windows2008Forest -Identity contoso.com  
```  
  
Pour augmenter ou diminuer le niveau fonctionnel du domaine à l’aide de Windows PowerShell, utilisez l’applet de commande Set-ADDomainMode.  
  
**Pour définir le niveau fonctionnel du domaine contoso.com en mode Windows Server2008:**  
  
```powershell  
Set-ADDomainMode -DomainMode Windows2008Domain -Identity contoso.com  
```  
  
La promotion d’un contrôleur de domaine exécutant Windows Server2012R2 en tant qu’un réplica supplémentaire dans un domaine existant en cours d’exécution DFL2003 fonctionne.  
  
Création du nouveau domaine dans une forêt existante  
  
![Mises à jour des services d’annuaire](media/Directory-Services-component-updates/GTR_ADDS_FFL.gif)  
  
### <a name="adprep"></a>ADPREP  
Il n’existe aucune nouvelle forêt ou opérations de domaine dans cette version.  
  
Ces fichiers .ldf contiennent des modifications de schéma pour la **Device Registration Service**.  
  
1.  Sch59  
  
2.  Sch61  
  
3.  Sch62  
  
4.  Sch63  
  
5.  Sch64  
  
6.  Sch65  
  
7.  Sch67  
  
**Dossiers de travail:**  
  
1.  Sch66  
  
**MSODS:**  
  
1.  Sch60  
  
**Silos et stratégies d’authentification**  
  
1.  Sch68  
  
2.  Sch69  
  
## <a name="BKMK_NTFRS"></a>Désapprobation de NTFRS  
  
### <a name="overview"></a>Vue d’ensemble  
FRS est obsolète dans Windows Server2012R2.  La désapprobation des FRS s’effectue en appliquant un niveau fonctionnel de domaine minimal (DFL) de Windows Server2008.  Cette application est disponible uniquement si le nouveau domaine est créé à l’aide du Gestionnaire de serveur ou Windows PowerShell.  
  
Vous utilisez le paramètre - DomainMode avec les applets de commande Install-ADDSForest ou Install-ADDSDomain pour spécifier le niveau fonctionnel du domaine.  Les valeurs prises en charge pour ce paramètre peuvent être un entier valide ou une valeur de chaîne énuméré correspondante. Par exemple, pour définir le niveau de mode domaine vers Windows Server2008R2, vous pouvez spécifier une valeur de 4 ou à «Win2008R2».  Lors de l’exécution de ces applets de commande à partir de Server2012R2 valide les valeurs sont celles pour Windows Server2008 (3, Win2008) (4, Win2008R2) de Windows Server2008R2 (5, Win2012) de Windows Server2012 et Windows Server2012R2 (6, Win2012R2). Le niveau fonctionnel du domaine ne peut pas être inférieur au niveau fonctionnel de forêt, mais il peut être supérieur.  Dans la mesure où FRS est obsolète dans cette version, Windows Server2003 (2, Win2003) n’est pas un paramètre reconnu avec ces applets de commande lors de l’exécution de Windows Server2012R2.  
  
![Mises à jour des services d’annuaire](media/Directory-Services-component-updates/GTR_ADDS_PS_Install2003DFL.gif)  
  
![Mises à jour des services d’annuaire](media/Directory-Services-component-updates/GTR_ADDS_PS_InstallDFL2.gif)  
  
## <a name="BKMK_LDAPQuery"></a>Modifications de l’optimiseur de requête LDAP  
  
### <a name="overview"></a>Vue d’ensemble  
L’algorithme optimiseur de requête LDAP a été réévalué et optimisée.  Le résultat est l’amélioration des performances dans l’efficacité des recherches LDAP et la durée de recherche LDAP de requêtes complexes.  
  
> [!NOTE]  
> **Depuis le développeur:**améliorations dans les performances des recherches par le biais des améliorations dans le mappage de l’annuaire LDAP de requête pour la requête ESE.  Les filtres LDAP dépassent un certain niveau de complexité empêchent la sélection d’index optimisé, entraînant considérablement une diminution des performances (1000 x ou plus). Cette modification modifie la manière qui nous sélectionner indices pour les requêtes LDAP éviter ce problème.  
  
> [!NOTE]  
> Changer de l’algorithme optimiseur de requête LDAP, ce qui:  
>   
> -   Temps de recherche plus rapides  
> -   Autoriser les contrôleurs de domaine faire plus efficacité  
> -   Moins d’appels au support technique émet relatives aux performances des annonces  
> -   Sauvegarder portée dans Windows Server2008R2 (KB2862304)  
  
### <a name="background"></a>En arrière-plan  
La possibilité de rechercher dans ActiveDirectory est un service de base fourni par les contrôleurs de domaine.  Autres services et les applications métier s’appuient sur les recherches ActiveDirectory.  Opérations de l’entreprise peuvent cesser d’un arrêt si cette fonctionnalité n’est pas disponible.  En tant que base et service fortement sollicitée, il est impératif que les contrôleurs de domaine gérer efficacement le trafic de recherche LDAP.  L’algorithme optimiseur de requête LDAP tente d’effectuer des recherches LDAP efficace que possible en mappant des filtres de recherche LDAP à un jeu de résultats qui peut être satisfait via des enregistrements déjà indexés dans la base de données.  Cet algorithme a été réévalué et optimisé.  Le résultat est l’amélioration des performances dans l’efficacité des recherches LDAP et la durée de recherche LDAP de requêtes complexes.  
  
### <a name="details-of-change"></a>Détails de la modification  
Une recherche LDAP contient:  
  
-   Un emplacement (head CN, unité d’organisation, objet) de la hiérarchie pour lancer la recherche  
  
-   Un filtre de recherche  
  
-   Une liste des attributs à renvoyer  
  
Le processus de recherche peut être résumé comme suit:  
  
1.  Simplifier le filtre de recherche si possible.  
  
2.  Sélectionnez un jeu de clés d’Index qui retourne le plus petit jeu couvert.  
  
3.  Effectuez une ou plusieurs des éléments sécants des clés d’Index, afin de réduire le jeu couvert.  
  
4.  Pour chaque enregistrement dans le jeu couvert, évaluer l’expression de filtre, ainsi que la sécurité. Si le filtre a la valeur TRUE et l’accès est accordé, puis retourne cet enregistrement pour le client.  
  
Le travail d’optimisation de requête LDAP modifie les étapes2 et 3, afin de réduire la taille de la plage couverte. Plus spécifiquement, l’implémentation actuelle sélectionne les clés d’Index en double et effectue les éléments sécants redondants.  
  
### <a name="comparison-between-old-and-new-algorithm"></a>Comparaison entre les ancienne et nouvel algorithme  
La cible de la recherche LDAP inefficace dans cet exemple est un contrôleur de domaine Windows Server2012.  La recherche se termine en 44secondes environ, à la suite de trouver un index plus efficace.  
  
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
  
### <a name="sample-results-using-the-new-algorithm"></a>Exemple de résultats à l’aide de l’algorithme de nouveau  
Cet exemple répète la même recherche exactement comme indiqué ci-dessus, mais ciblant un contrôleur de domaine Windows Server2012R2.  La même recherche se termine en moins d’une seconde en raison des améliorations dans l’algorithme optimiseur de requête LDAP.  
  
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
  
-   Si elle est impossible d’optimiser l’arborescence:  
  
    -   Par exemple: une expression dans l’arborescence était sur une colonne n'indexée pas  
  
    -   Enregistrer une liste d’index qui empêchent l’optimisation  
  
    -   Exposé via le suivi ETW et événement 1644 ID  
  
        ![Mises à jour des services d’annuaire](media/Directory-Services-component-updates/GTR_ADDS_Event1644.gif)  
  
### <a name="BKMK_EnableStats"></a>Pour activer le contrôle de statistiques dans LDP  
  
1.  Ouvrez LDP.exe et vous connecter et lier à un contrôleur de domaine.  
  
2.  Sur le **Options** menu, cliquez sur **contrôles**.  
  
3.  Dans la boîte de dialogue de contrôles, développez le **charge prédéfinies** menu déroulant, cliquez sur **statistiques de la recherche** puis cliquez sur **OK**.  
  
    ![Mises à jour des services d’annuaire](media/Directory-Services-component-updates/GTR_ADDS_Controls.gif)  
  
4.  Sur le **Parcourir** menu, cliquez sur **recherche**  
  
5.  Dans la boîte de dialogue recherche, sélectionnez le **Options** bouton.  
  
6.  Vérifiez le **étendu** case à cocher est activée dans la boîte de dialogue Options de recherche et sélectionnez **OK**.  
  
    ![Mises à jour des services d’annuaire](media/Directory-Services-component-updates/GTR_ADDS_SearchOptions.gif)  
  
### <a name="try-this-use-ldp-to-return-query-statistics"></a>Procédez comme suit: Utilisez LDP pour renvoyer les statistiques de demande  
Effectuez les opérations suivantes sur un contrôleur de domaine, ou à partir d’un client appartenant à un domaine ou un serveur qui est installés les outils ADDS.  Répétez les opérations suivantes ciblant votre contrôleur de domaine Windows Server2012 et votre contrôleur de domaine de Windows Server2012R2.  
  
1.  Passez en revue les [«Création plus efficace MicrosoftApplications ActiveDirectory»](https://msdn.microsoft.com/library/ms808539.aspx) article et référer en fonction des besoins.  
  
2.  Utilisation de LDP, activer les statistiques de recherche (voir [pour activer le contrôle de statistiques dans LDP](../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_EnableStats))  
  
3.  Plusieurs des recherches LDAP et observez les informations statistiques en haut des résultats.  Vous serez amené à répéter la même recherche dans d’autres activités donc les document dans un fichier de texte bloc-notes.  
  
4.  Effectuer une recherche LDAP qui l’optimiseur de requête doit être en mesure d’optimiser en raison de l’index d’attributs  
  
5.  De construire qui prend beaucoup de temps pour effectuer une recherche (vous pouvez choisir d’augmenter la **limite de temps** option afin de la recherche ne prend pas de délai d’attente).  
  
### <a name="additional-resources"></a>Ressources supplémentaires  
[Quelles sont les recherches ActiveDirectory?](https://technet.microsoft.com/library/cc783845(v=ws.10).aspx)  
  
[Comment les recherches ActiveDirectory](https://technet.microsoft.com/library/cc755809(v=WS.10).aspx)  
  
[Création d’Applications d’annuaire MicrosoftActive plus efficaces](https://msdn.microsoft.com/library/ms808539.aspx)  
  
[951581](https://support.microsoft.com/kb/951581) requêtes LDAP sont exécutées plus lentement que prévu dans ActiveDirectory ou service d’annuaire ADLDS/ADAM et 1644 d’ID d’événement peuvent être enregistré.  
  
## <a name="BKMK_1644"></a>Événement1644  
  
### <a name="overview"></a>Vue d’ensemble  
Cette mise à jour ajoute des statistiques des résultats de recherche LDAP supplémentaires à l’événement 1644 ID pour vous aider à des fins de dépannage.  En outre, il est une nouvelle valeur de Registre qui peut être utilisée pour activer la journalisation sur un seuil de temps.  Ces améliorations ont été apportées disponibles dans Windows Server2012 et Windows Server2008R2SP1 via Ko [2800945](https://support.microsoft.com/kb/2800945) et seront disponibles pour Windows Server2008SP2.  
  
> [!NOTE]  
> -   Statistiques de la recherche LDAP supplémentaires sont ajoutés à l’événement 1644 ID pour faciliter le dépannage des recherches LDAP inefficaces ou coûteux  
> -   Vous pouvez maintenant spécifier un seuil de délai de recherche (par ex. Journal des événements 1644 pour les recherches nécessite plus de 100millisecondes) au lieu de spécifier les valeurs de seuil de résultat recherche coûteux et Inefficient  
  
### <a name="background"></a>En arrière-plan  
Lors de la résolution des problèmes de performance ActiveDirectory, il apparaît qu’activité de recherche LDAP peut-être la cause du problème.  Vous décidez d’activer la journalisation afin que vous pouvez voir les requêtes LDAP coûteux ou inefficaces traités par le contrôleur de domaine.  Afin d’activer la journalisation, vous devez définir la valeur de diagnostics ingénierie et pouvez éventuellement spécifier les valeurs de seuil des résultats de recherche coûteux / inefficace.  Lors de l’activation de l’ingénierie niveau d’enregistrement sur une valeur de 5, une recherche qui répond à ces critères est consignée dans le journal des événements Services d’annuaire avec un événement 1644 ID.  
  
L’événement contient:  
  
-   Client IP et le port  
  
-   Nœud de démarrage  
  
-   Filtre  
  
-   Étendue de la recherche  
  
-   Sélection des attributs  
  
-   Contrôles serveur  
  
-   Entrées visitées  
  
-   Entrées retournées  
  
Toutefois, les données de clé sont manquantes à partir de l’événement telles que la quantité de temps passé sur l’opération de recherche et ce que (le cas échéant) index a été utilisé.  
  
#### <a name="additional-search-statistics-added-to-event-1644"></a>Statistiques de recherche supplémentaires ajoutés à l’événement 1644  
  
-   Index utilisés  
  
-   Pages de référence  
  
-   Pages lues à partir du disque  
  
-   Pages prélecture à partir du disque  
  
-   Nouvelles pages modifiées  
  
-   Pages modifiées modifiés  
  
-   Délai de recherche  
  
-   Prévention de l’optimisation des attributs  
  
#### <a name="new-time-based-threshold-registry-value-for-event-1644-logging"></a>Nouvelle valeur de Registre de seuil de temps pour l’enregistrement de l’événement 1644  
Au lieu de spécifier les valeurs de seuil de résultat recherche coûteux et Inefficient, vous pouvez spécifier le seuil de délai de recherche.  Si vous souhaité pour se connecter à tous les résultats de recherche qui ont eu 50ms ou supérieur, vous devez spécifier 50 décimal / 32hex (outre la définition de la valeur de champ ingénierie).  
  
```  
Windows Registry Editor Version 5.00  
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NTDS\Parameters]  
"Search Time Threshold (msecs)"=dword:00000032  
```  
  
#### <a name="comparison-of-the-old-and-new-event-id-1644"></a>Comparaison de l’événement ancienne et nouvel ID1644  
ANCIEN  
  
![Mises à jour des services d’annuaire](media/Directory-Services-component-updates/GTR_ADDS_Event1644_2012.gif)  
  
Nouveau  
  
![Mises à jour des services d’annuaire](media/Directory-Services-component-updates/GTR_ADDS_Event1644_2012R2.gif)  
  
#### <a name="try-this-use-the-event-log-to-return-query-statistics"></a>Procédez comme suit: Utilisez le journal des événements pour renvoyer les statistiques de demande  
  
1.  Répétez les opérations suivantes ciblant votre contrôleur de domaine Windows Server2012 et votre contrôleur de domaine de Windows Server2012R2. Observez l’événement ID1644s sur les deux contrôleurs de domaine après chaque recherche.  
  
2.  À l’aide de regedit, activez l’enregistrement d’événement 1644 ID à l’aide d’un seuil de temps sur le contrôleur de domaine Windows Server2012R2 et de l’ancienne méthode sur le contrôleur de domaine Windows Server2012.  
  
3.  Effectuez plusieurs recherches LDAP dépassent le seuil et observez les informations statistiques en haut des résultats.  Utiliser les requêtes LDAP que décrits précédemment et répétez les mêmes recherches.  
  
4.  Effectuer une recherche LDAP qui l’optimiseur de requête n’est pas en mesure d’optimiser car un ou plusieurs attributs ne sont pas indexés.  
  
## <a name="BKMK_ADRepl"></a>Amélioration du débit de réplication ActiveDirectory  
  
### <a name="overview"></a>Vue d’ensemble  
La réplication ActiveDirectory utilise RPC pour le transport de réplication. Par défaut, RPC utilise un tampon de transmission de 8Ko et une taille de paquet 5Ko. Cela a pour effet net où l’instance envoi transmettre des paquets de trois (environ 15K de données), puis tenu d’attendre pour un réseau aller-retour avant d’envoyer plus. En supposant un 3ms temps de traduction, le débit plus élevé serait d’environ 40, même sur des réseaux de 10Gbit/s ou de 1Gbit/s.  
  
> [!NOTE]  
> -   Cette mise à jour ajuste le débit de réplication ActiveDirectory maximal de 40 à environ 600Mbits/s.  
>   
>     -   Elle augmente la taille de mémoire tampon d’envoi RPC ce qui réduit le nombre de réseau allers-retours  
> -   L’effet sera plus visible à grande vitesse, réseau à latence élevée.  
  
Cette mise à jour augmente le débit maximal à environ 600Mbits/s en modifiant la taille de mémoire tampon d’envoi RPC de 8Ko à 256Ko.  Cette modification permet à la taille de fenêtre TCP à croître au-delà de 8Ko, ce qui réduit le nombre de réseau allers-retours.  
  
> [!NOTE]  
> Il n’existe aucun paramètre configurable pour modifier ce comportement.  
  
### <a name="additional-resources"></a>Ressources supplémentaires  
[Comment fonctionne le modèle de réplication ActiveDirectory](https://technet.microsoft.com/library/cc772726(v=WS.10).aspx)  
  


