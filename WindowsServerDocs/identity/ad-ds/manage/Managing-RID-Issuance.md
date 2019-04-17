---
ms.assetid: aac117a7-aa7a-4322-96ae-e3cc22ada036
title: "La gestion de l’émission RID"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: f84bcc1aa32e9993903e094fc43feffcbe16a05b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="managing-rid-issuance"></a>La gestion de l’émission RID

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Cette rubrique décrit la modification pour le rôle FSMO du maître RID, y compris la nouvelle émission et la fonctionnalité dans le maître RID et comment analyser et résoudre les problèmes d’émission RID d’analyse.  
  
-   [La gestion de l’émission RID](../../ad-ds/manage/Managing-RID-Issuance.md#BKMK_Manage)  
  
-   [Résolution des problèmes d’émission RID](../../ad-ds/manage/Managing-RID-Issuance.md#BKMK_Tshoot)  
  
Des informations supplémentaires sont disponibles sur le [AskDS Blog](http://blogs.technet.com/b/askds/archive/2012/08/10/managing-rid-issuance-in-windows-server-2012.aspx).  
  
## <a name="BKMK_Manage"></a>La gestion de l’émission RID  
Par défaut, un domaine a la capacité pour contenir environ un milliard principaux de sécurité, tels que les utilisateurs, groupes et ordinateurs. Naturellement, il n’existe aucun domaine avec autant d’objets activement utilisés. Toutefois, le support technique Microsoft a rencontré les cas où:  
  
-   Logiciel ou des scripts d’administration accidentellement en bloc de configuration créé les utilisateurs, groupes et des ordinateurs.  
  
-   Plusieurs groupes de distribution et de sécurité inutilisés ont été créés par des utilisateurs délégués.  
  
-   Plusieurs contrôleurs de domaine ont été rétrogradés, restaurée, ou le nettoyage des métadonnées  
  
-   Récupérations de forêts ont été effectuées.  
  
-   L’opération InvalidateRidPool a été souvent effectuée.  
  
-   La valeur de Registre de taille de bloc de RID a été augmentée de façon incorrecte  
  
Toutes ces situations épuisent les identificateurs RID inutilement, souvent par erreur. Après de nombreuses années, quelques environnements insuffisant identificateurs RID et obligés de migrer vers un nouveau domaine ou d’effectuer des récupérations de forêts.  
  
Windows Server 2012 traite des problèmes avec l’allocation RID qui ont uniquement sont devenues problématiques qu’avec l’ancienneté et l’omniprésence d’Active Directory. Ceux-ci incluent une meilleure journalisation des événements, des limites plus appropriées et la capacité, en cas d’urgence, de doubler la taille globale de l’espace RID global pour un domaine.  
  
### <a name="periodic-consumption-warnings"></a>Avertissements de la consommation périodiques  
Windows Server 2012 ajoute l’événement d’espace RID global de suivi qui fournit tôt lorsque les principales étapes sont dépassés. Le modèle calcule le pourcentage de dix (10) utiliser Marquer dans le pool global et consigne un événement lorsque atteinte. Puis il calcule la prochaine dix pour cent utilisé restant et le cycle des événements continue. Comme l’espace RID global s’épuise, événements seront accélèrent car les dix pour cent sont plus rapidement dans un pool en baisse (mais blocage du journal des événements empêche plus d’une entrée par heure). Le journal des événements système sur chaque contrôleur de domaine écrit l’événement d’avertissement Directory-Services-SAM 16658.  
  
En supposant un défaut 30 bits espace RID global, le premier événement est consigné lors de l’allocation du pool contenant le 107 374 182<sup>nd</sup> RID. Le débit des événements s’accélère naturellement jusqu’au dernier point de contrôle de 100 000 avec 110 événements générés au total. Le comportement est similaire pour un espace RID global 31 bits déverrouillé: début au niveau 214 748 365 et fin avec 117 événements.  
  
> [!IMPORTANT]  
> Cet événement n’est pas prévu; Recherchez l’utilisateur, ordinateur et processus de création de groupe immédiatement dans le domaine. Création d’objets de domaine Active Directory plus de 100 millions d’est sort de l’ordinaire.  
  
![Émission RID](media/Managing-RID-Issuance/ADDS_RID_TR_EventWaypoints2.png)  
  
### <a name="rid-pool-invalidation-events"></a>Événements d’invalidation du Pool RID  
Nouvelles alertes d’événement signalent qu’un pool RID du contrôleur de domaine local a été supprimé. Ces informations et peuvent être prévues, en particulier en raison de la nouvelle fonctionnalité VDC. Consultez la liste d’événements pour plus d’informations sur l’événement.  
  
### <a name="BKMK_RIDBlockMaxSize"></a>Limite de taille de bloc de RID  
En règle générale, un contrôleur de domaine demande allocations RID en blocs de 500 identificateurs RID à la fois. Vous pouvez remplacer cette valeur par défaut à l’aide de la valeur REG_DWORD de Registre suivante sur un contrôleur de domaine:  
  
```  
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NTDS\RID Values  
RID Block Size  
  
```  
  
Avant Windows Server 2012, il n’a aucune valeur maximale est appliquée dans cette clé de Registre, à l’exception de la valeur maximale DWORD implicite (qui a une valeur de 0xffffffff ou 4294967295). Cette valeur est considérablement supérieure à l’espace RID global total. Les administrateurs parfois inappropriée ou configuré taille de bloc de RID avec des valeurs qui épuisaient l’espace RID global à un rythme considérable.  
  
Dans Windows Server 2012, vous ne pouvez pas définir cette valeur de Registre supérieure à 15 000 en décimal (0x3a98 en hexadécimal). Cela empêche une allocation de RID volumineuse.  
  
Si vous définissez la valeur *supérieur* à 15 000, la valeur est traitée comme 15 000 et le contrôleur de domaine consigne l’événement 16653 dans le journal des événements Services d’annuaire à chaque redémarrage jusqu'à ce que la valeur soit corrigée.  
  
### <a name="BKMK_GlobalRidSpaceUnlock"></a>Déverrouillage de la taille de l’espace RID global  
Avant Windows Server 2012, l’espace RID global était limité à 2<sup>30</sup> (ou 1 073 741 823) identificateurs RID au total. Une fois atteinte, uniquement un domaine migration ou la forêt récupération pour une période antérieure autorisés création du nouveau SID - récupération d’urgence, par toute mesure. À compter de Windows Server 2012, le 2<sup>31</sup> bits peut être déverrouillée pour augmenter le pool 2,147,483,648 RID global.  
  
Les services AD DS stockent ce paramètre dans un attribut masqué spécial nommé **SidCompatibilityVersion** dans le contexte RootDSE de tous les contrôleurs de domaine. Cet attribut n’est pas lisible à l’aide d’ADSIEdit, LDP ou autres outils. Pour voir une augmentation de l’espace RID global, examinez le journal des événements système pour l’événement d’avertissement 16655 de Directory-Services-SAM ou utilisez la commande Dcdiag suivante:  
  
```  
Dcdiag.exe /TEST:RidManager /v | find /i "Available RID Pool for the Domain"  
  
```  
  
Si vous augmentez le pool RID global, le pool disponible passe à 2 147 483 647 au lieu de la valeur par défaut 1 073 741 823. Par exemple:  
  
![Émission RID](media/Managing-RID-Issuance/ADDS_RID_TR_Dcdiag.png)  
  
> [!WARNING]  
> Ce déverrouillage est destiné *uniquement* afin d’éviter le manque d’identificateurs RID et doit être utilisé *uniquement* conjointement avec l’application d’un plafond RID (voir la section suivante). Ne le définissez pas «manière préemptive» dans les environnements qui ont des millions d’identificateurs RID restants et faible croissance, comme des problèmes de compatibilité peuvent existent avec des identificateurs SID générés à partir du pool RID déverrouillé.  
>   
> Cette opération de déverrouillage ne peut pas être rétablie ni supprimée, sauf par une récupération de forêt complète vers des sauvegardes antérieures.  
  
#### <a name="important-caveats"></a>Réserves importantes  
Windows Server 2003 et des contrôleurs de domaine Windows Server 2008 ne peut pas émettre de RID quand l’espace RID global pool 31<sup>st</sup> bits est déverrouillé. Contrôleurs de domaine Windows Server 2008 R2 *pouvez* utiliser le 31<sup>st</sup> bit RID *, mais seulement si* correctif logiciel [2642658 Ko](https://support.microsoft.com/kb/2642658) installé. Contrôleurs de domaine non pris en charge et non corrigés traitent le pool RID global comme épuisé quand il est déverrouillé.  
  
Cette fonctionnalité n’est pas appliquée par n’importe quel niveau fonctionnel du domaine; Veillez à qui existent uniquement Windows Server 2012 ou les contrôleurs de domaine Windows Server 2008 R2 mis à jour dans le domaine.  
  
#### <a name="implementing-unlocked-global-rid-space"></a>Implémentation d’espace RID Global déverrouillé  
Pour déverrouiller le pool RID au niveau du 31<sup>st</sup> bit après avoir reçu l’alerte de plafond RID (voir ci-dessous) effectuez les opérations suivantes:  
  
1.  Vérifiez que le maître RID rôle s’exécute sur un contrôleur de domaine Windows Server 2012. Dans le cas contraire, transférez-le vers un contrôleur de domaine Windows Server 2012.  
  
2.  Exécutez LDP.exe  
  
3.  Cliquez sur le **connexion** menu, cliquez sur **Connect** pour le maître RID de Windows Server 2012 sur le port 389, puis cliquez sur **liaison** en tant qu’administrateur de domaine.  
  
4.  Cliquez sur le **Parcourir** menu, cliquez sur **modifier**.  
  
5.  Vérifiez que **DN** est vide.  
  
6.  Dans **modifier l’entrée attribut**, tapez:  
  
    ```  
    SidCompatibilityVersion  
    ```  
  
7.  Dans **valeurs**, tapez:  
  
    ```  
    1  
    ```  
  
8.  Vérifiez que **ajouter** est sélectionné dans **opération** et cliquez sur **entrée**. Cette mise à jour le **liste d’entrées**.  
  
9. Sélectionnez le **synchrone** et **étendu** options, puis cliquez sur **exécuter**.  
  
    ![Émission RID](media/Managing-RID-Issuance/ADDS_RID_TR_LDPModify.png)  
  
10. En cas de succès, le LDP sortie fenêtre affiche:  
  
    ```  
    ***Call Modify...  
     ldap_modify_ext_s(Id, '(null)',[1] attrs, SvrCtrls, ClntCtrls);  
    modified "".  
  
    ```  
  
    ![Émission RID](media/Managing-RID-Issuance/ADDS_RID_TR_LDPModifySuccess.png)  
  
11. Vérifiez le pool RID global a augmenté en examinant le journal des événements système sur ce contrôleur de domaine pour l’événement d’information Directory-Services-SAM 16655.  
  
### <a name="rid-ceiling-enforcement"></a>Application d’un plafond de RID  
Pour offrir une mesure de protection et élever les privilèges d’administration reconnaissance, Windows Server 2012 introduit un plafond artificiel sur la plage RID globale à dix (10 %) identificateurs RID restants dans l’espace global. Au sein d’un (1) pour cent du plafond artificiel, les contrôleurs de domaine demande pools RID écrire événement d’avertissement Directory-Services-SAM 16656 au journal des événements système. Quand ils atteignent le plafond de dix pour cent sur le FSMO du maître RID, il écrit l’événement Directory-Services-SAM 16657 dans le journal des événements système et n’allouera pas d’autres pools RID tant que le plafond. Cela vous oblige à évaluer l’état du maître RID dans le domaine et de résoudre les éventuels dont le contrôle de l’allocation RID; Cela protège également les domaines contre l’épuisement de tout l’espace RID.  
  
Ce plafond est codé en dur à dix pour cent restante de l’espace RID disponible. Autrement dit, le plafond est activé lorsque le maître RID alloue un pool qui inclut le RID correspondant à quatre-vingt-dix (90) pour cent de l’espace RID global.  
  
-   Pour les domaines par défaut, le premier point de déclenchement est 2<sup>30</sup>-1 * 0,90 = 966,367,640 (ou 107 374 183 identificateurs RID restants).  
  
-   Pour les domaines avec un espace RID de 31 bits déverrouillé, le seuil de déclenchement est 2<sup>31</sup>-1 * 0,90 = 1,932,735,282 identificateurs RID (ou 214 748 365 identificateurs RID restants).  
  
Lors du déclenchement, le maître RID affecte attribut Active Directory **msDS-RIDPoolAllocationEnabled** (nom commun **ms-DS-RID-Pool-Allocation-Enabled**) false sur l’objet:  
  
CN = RID Manager$, CN = System, DC =*<domain>*  
  
Il écrit l’événement 16657 et empêche davantage d’émission de bloc RID à tous les contrôleurs de domaine. Contrôleurs de domaine continuent de consommer les pools RID en attente déjà émis à leur.  
  
Pour supprimer le bloc et autoriser l’allocation du pool RID continuer, choisissez la valeur TRUE. Sur la prochaine allocation RID effectuée par le maître RID, l’attribut reprendra sa valeur NOT SET par défaut. Après cela, il n’existe aucun plafonds supplémentaires et par la suite, l’espace RID global s’exécute, nécessitant une migration de domaine ou de récupération de forêt.  
  
#### <a name="removing-the-ceiling-block"></a>Suppression du bloc du plafond  
Pour supprimer le bloc une fois le plafond artificiel, procédez comme suit:  
  
1.  Vérifiez que le maître RID rôle s’exécute sur un contrôleur de domaine Windows Server 2012. Dans le cas contraire, transférez-le vers un contrôleur de domaine Windows Server 2012.  
  
2.  Exécutez LDP.exe.  
  
3.  Cliquez sur le **connexion** menu, cliquez sur *Connect* pour le maître RID de Windows Server 2012 sur le port 389, puis cliquez sur **liaison** en tant qu’administrateur de domaine.  
  
4.  Cliquez sur le **affichage** menu, cliquez sur **arborescence**, puis pour le **DN de Base de** sélectionner le contexte de nommage de domaine du maître RID propre. Cliquez sur **Ok**.  
  
5.  Dans le volet de navigation, explorez le **CN = System** conteneur et cliquez sur le **CN = RID Manager$** objet. Avec le bouton droit dessus, puis cliquez sur **modifier**.  
  
6.  Dans modifier l’attribut d’entrée, tapez:  
  
    ```  
    MsDS-RidPoolAllocationEnabled  
    ```  
  
7.  Dans **valeurs**, tapez (en majuscules):  
  
    ```  
    TRUE  
    ```  
  
8.  Sélectionnez **remplacer** dans **opération** et cliquez sur **entrée**. Cette mise à jour le **liste d’entrées**.  
  
9. Activer la **synchrone** et **étendu** options, puis cliquez sur **exécuter**:  
  
    ![Émission RID](media/Managing-RID-Issuance/ADDS_RID_TR_LDPRaiseCeiling.png)  
  
10. En cas de succès, le LDP sortie fenêtre affiche:  
  
    ```  
    ***Call Modify...  
    ldap_modify_ext_s(ld, 'CN=RID Manager$,CN=System,DC=<domain>',[1] attrs, SvrCtrls, ClntCtrls);  
    Modified "CN=RID Manager$,CN=System,DC=<domain>".  
  
    ```  
  
    ![Émission RID](media/Managing-RID-Issuance/ADDS_RID_TR_LDPRaiseCeilingSuccess.png)  
  
### <a name="other-rid-fixes"></a>Autres correctifs RID  
Les systèmes d’exploitation Windows Server précédents avait un pool RID quand fuite attribut rIDSetReferences était manquant. Pour résoudre ce problème sur les contrôleurs de domaine qui exécutent Windows Server 2008 R2, installez le correctif logiciel de [2618669 Ko](https://support.microsoft.com/kb/2618669).  
  
### <a name="unfixed-rid-issues"></a>Problèmes RID non résolus  
Il y a toujours été une fuite RID en cas d’échec de la création de compte; Lorsque vous créez un compte, échec épuise toujours un RID. L’exemple courant consiste à créer un utilisateur avec un mot de passe qui ne satisfait pas la complexité.  
  
### <a name="rid-fixes-for-earlier-versions-of-windows-server"></a>Correctifs RID pour les versions antérieures de Windows Server  
Tous les correctifs et modifications ci-dessus ont des correctifs Windows Server 2008 R2. Il n’existe actuellement aucun correctif de Windows Server 2008 planifié ou en cours d’exécution.  
  
## <a name="BKMK_Tshoot"></a>Résolution des problèmes d’émission RID  
  
### <a name="introduction-to-troubleshooting"></a>Introduction à la résolution des problèmes  
La résolution des problèmes d’émission RID nécessite une méthode logique et linéaire. Sauf si vous analysez vos journaux d’événements soigneusement pour les erreurs et avertissements déclenchés par RID, les premières indications d’un problème seront susceptibles d’être des créations de comptes a échoué. Pour résoudre les problèmes d’émission RID est de comprendre quand le symptôme est prévu ou non; de nombreux problèmes d’émission RID peuvent affecter qu’un seul contrôleur de domaine et n’ont rien à faire avec les améliorations apportées aux composants. Ce simple diagramme ci-dessous permet de prendre les décisions plus claire:  
  
![Émission RID](media/Managing-RID-Issuance/adds_rid_issuance_troubleshooting.png)  
  
### <a name="troubleshooting-options"></a>Options de dépannage  
  
#### <a name="logging-options"></a>Options de journalisation  
Toute la journalisation d’émission RID se produit dans le journal des événements système, sous source Directory-Services-SAM. La journalisation est activée et configurée pour un maximum de commentaires par défaut. Si aucune entrée n’est enregistrée pour les nouvelles modifications de composants dans Windows Server 2012, traitez le problème comme un classique (également appelés traditionnels, antérieur à Windows Server 2012) problème d’émission RID vu dans Windows 2008 R2 ou des systèmes d’exploitation plus anciens.  
  
#### <a name="utilities-and-commands-for-troubleshooting"></a>Utilitaires et commandes pour la résolution des problèmes  
Pour résoudre les problèmes non décrits par les journaux indiqués ci-dessus, plus particulièrement les anciennes problèmes d’émission RID - utilisez la liste suivante d’outils comme point de départ:  
  
-   Dcdiag.exe  
  
-   Repadmin.exe  
  
-   Moniteur réseau 3.4  
  
### <a name="general-methodology-for-troubleshooting-domain-controller-configuration"></a>Méthodologie générale pour la résolution des problèmes de Configuration du contrôleur de domaine  
  
1.  L’erreur causée par un simple problème de disponibilité de contrôleur de domaine ou d’autorisations?  
  
    1.  Vous essayez de créer une sécurité principal sans les autorisations nécessaires? Examinez la sortie pour les erreurs d’accès refusé.  
  
    2.  Un contrôleur de domaine est disponible? Examinez les messages d’erreur ou LDAP ou domaine contrôleur disponibilité retournés.  
  
2.  Est l’erreur retournée mentionne spécifiquement identificateurs RID et est suffisamment spécifique pour l’utiliser comme indication? Dans ce cas, suivez les instructions.  
  
3.  Est l’erreur retournée mentionne spécifiquement identificateurs RID, mais est sinon non spécifiques? Par exemple, «Windows ne peut pas créer l’objet car le Service d’annuaire n’a pas pu allouer un identificateur relatif.»  
  
    1.  Examinez le journal des événements système sur le contrôleur de domaine pour «hérités» (antérieurs à Windows Server 2012) détaillés des événements RID dans [demande de Pool RID](https://technet.microsoft.com/en-us/library/ee406152(WS.10).aspx) (16642, 16643, 16644, 16645, 16656).  
  
    2.  Examinez les événements système sur le contrôleur de domaine et le maître RID de nouveaux événements indiquant des blocs détaillés ci-dessous dans cette rubrique (16655, 16656, 16657).  
  
    3.  Valider l’intégrité de la réplication Active Directory avec Repadmin.exe et le maître RID disponibilité avec **Dcdiag.exe/test: RidManager /v**. Activer les captures réseau recto verso entre le contrôleur de domaine et le maître RID si ces tests ne sont pas concluants.  
  
### <a name="troubleshooting-specific-problems"></a>Résolution des problèmes spécifiques  
Ouvrez une session les nouveaux messages suivants dans le journal des événements système sur les contrôleurs de domaine Windows Server 2012. Automatisée AD health suivi des systèmes, tels que System Center Operations Manager, doit analyser ces événements; tous sont importants, et d’autres indicateurs des problèmes critiques du domaine.  
  
|||  
|-|-|  
|ID d’événement|16653|  
|Source|Directory-Services-SAM|  
|Gravité|Avertissement|  
|Message|Une taille de pool d’identificateurs de comptes (RID) a été configurée par un administrateur est supérieure au nombre maximal pris en charge. La valeur maximale %1 sera utilisée lorsque le contrôleur de domaine est le maître RID.<br /><br />Pour plus d’informations, voir [limite de taille de bloc de RID](../../ad-ds/manage/../../ad-ds/manage/../../ad-ds/manage/../../ad-ds/manage/Managing-RID-Issuance.md#BKMK_RIDBlockMaxSize).|  
|Remarques et résolution|La valeur maximale pour la taille de bloc RID est maintenant 15 000 en décimal (3A98 hexadécimal). Un contrôleur de domaine ne peuvent pas demander plus de 15 000 identificateurs RID. Cet événement est consigné à chaque redémarrage jusqu'à ce que la valeur est définie sur une valeur égale ou inférieure ce maximum.|  
  
|||  
|-|-|  
|ID d’événement|16654|  
|Source|Directory-Services-SAM|  
|Gravité|Information|  
|Message|Un pool d’identificateurs de comptes (RID) a été invalidé. Cela peut se produire dans les cas attendus suivants:<br /><br />1. un contrôleur de domaine est restauré à partir de la sauvegarde.<br /><br />2. un contrôleur de domaine en cours d’exécution sur un ordinateur virtuel est restauré à partir d’une capture instantanée.<br /><br />3. un administrateur a invalidé le pool manuellement.<br /><br />https://go.microsoft.com/fwlink/?LinkId=226247 pour plus d’informations, voir.|  
|Remarques et résolution|Si cet événement est inattendu, contactez tous les administrateurs de domaine et déterminer celui qui a effectué l’action. Le journal des événements Services d’annuaire contient également davantage d’informations sur lorsqu’une de ces étapes a été effectuée.|  
  
|||  
|-|-|  
|ID d’événement|16655|  
|Source|Directory-Services-SAM|  
|Gravité|Information|  
|Message|La valeur maximale globale pour les identificateurs de comptes (RID) a été augmentée pour %1.|  
|Remarques et résolution|Si cet événement est inattendu, contactez tous les administrateurs de domaine et déterminer celui qui a effectué l’action. Cet événement indique l’augmentation de la RID global pool taille au-delà de la valeur par défaut 2<sup>30</sup>et ne sera pas appliquée automatiquement; uniquement par une action administrative.|  
  
|||  
|-|-|  
|ID d’événement|16656|  
|Source|Directory-Services-SAM|  
|Gravité|Avertissement|  
|Message|La valeur maximale globale pour les identificateurs de comptes (RID) a été augmentée pour %1.|  
|Remarques et résolution|Action requise! Un pool d’identificateurs de comptes (RID) a été alloué à ce contrôleur de domaine. La valeur du pool indique que ce domaine a consommé une partie importante du total disponibles compte des identificateurs.<br /><br />Un mécanisme de protection sera activé quand le domaine atteindra le seuil suivant du totales identificateurs de comptes disponibles restants: %1.  Le mécanisme de protection empêche la création de comptes jusqu'à ce que vous réactiviez manuellement l’allocation des identificateurs de comptes sur le contrôleur de domaine du maître RID.<br /><br />https://go.microsoft.com/fwlink/?LinkId=228610 pour plus d’informations, voir.|  
  
|||  
|-|-|  
|ID d’événement|16657|  
|Source|Directory-Services-SAM|  
|Gravité|Erreur|  
|Message|Action requise! Ce domaine a consommé une partie importante du totales disponibles compte des identificateurs (RID). Un mécanisme de protection a été activé, car les totales identificateurs de comptes disponibles restants est inférieur à: X% [argument de plafond artificiel].<br /><br />Le mécanisme de protection empêche la création de comptes jusqu'à ce que vous réactiviez manuellement l’allocation des identificateurs de comptes sur le contrôleur de domaine du maître RID.<br /><br />Il est extrêmement important qu’effectuer certains diagnostics avant de réactiver le compte de création pour vous assurer de ce domaine ne consomme pas les identificateurs de comptes à une vitesse anormalement élevée. Tout problème identifié doit être résolu avant de réactiver la création du compte.<br /><br />Épuisement des identificateurs de compte dans le domaine après lequel la création de comptes sera définitivement désactivée dans ce domaine peut entraîner l’échec pour diagnostiquer et résoudre tout problème sous-jacent provoquant une vitesse anormalement élevée de la consommation d’identificateurs de comptes.<br /><br />https://go.microsoft.com/fwlink/?LinkId=228610 pour plus d’informations, voir.|  
|Remarques et résolution|Contactez tous les administrateurs de domaine et informez-les qu’aucun autre principal de sécurité ne peut être créée dans ce domaine jusqu'à ce que cette protection soit remplacée. Pour plus d’informations sur la façon de remplacer la protection et d’augmenter éventuellement le RID global du pool, consultez [Global RID espace déverrouillage de la taille](../../ad-ds/manage/../../ad-ds/manage/../../ad-ds/manage/../../ad-ds/manage/Managing-RID-Issuance.md#BKMK_GlobalRidSpaceUnlock).|  
  
|||  
|-|-|  
|ID d’événement|16658|  
|Source|Directory-Services-SAM|  
|Gravité|Avertissement|  
|Message|Cet événement est une mise à jour périodique sur la quantité totale restante d’identificateurs de comptes disponibles (RID). Le nombre d’identificateurs de comptes restants est environ: %1.<br /><br />Identificateurs de comptes sont utilisés comme comptes sont créés lorsqu’ils sont épuisés, qu'aucun compte ne peut être créé dans le domaine.<br /><br />https://go.microsoft.com/fwlink/?LinkId=228745 pour plus d’informations, voir.|  
|Remarques et résolution|Contactez tous les administrateurs de domaine et informez-les que consommation RID a franchi une étape importante; déterminer si ce comportement est attendu ou non par l’examen des modèles de création de sécurité tiers de confiance. Pour voir jamais cet événement est extrêmement rares, car il signifie qu’au moins 100 millions d’identificateurs RID ont été allouées.|  
  
## <a name="see-also"></a>Voir aussi  
[La gestion de l’émission RID dans Windows Server 2012](http://blogs.technet.com/b/askds/archive/2012/08/10/managing-rid-issuance-in-windows-server-2012.aspx)  
  


