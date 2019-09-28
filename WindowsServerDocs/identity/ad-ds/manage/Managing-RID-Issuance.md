---
ms.assetid: aac117a7-aa7a-4322-96ae-e3cc22ada036
title: Gestion de l’émission RID
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: dd265fecce06b849bd14d4d6b81503aba7311656
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71390074"
---
# <a name="managing-rid-issuance"></a>Gestion de l’émission RID

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cette rubrique décrit la modification apportée au rôle FSMO du maître RID, y compris la nouvelle fonctionnalité d'émission et d'analyse dans le maître RID ainsi que la façon d'analyser l'émission RID et de résoudre les problèmes associés.  
  
-   [Gestion de l’émission RID](../../ad-ds/manage/Managing-RID-Issuance.md#BKMK_Manage)  
  
-   [Résolution des problèmes d’émission RID](../../ad-ds/manage/Managing-RID-Issuance.md#BKMK_Tshoot)  
  
Pour plus d’informations, consultez le [blog, voir](http://blogs.technet.com/b/askds/archive/2012/08/10/managing-rid-issuance-in-windows-server-2012.aspx).  
  
## <a name="BKMK_Manage"></a>Gestion de l’émission RID  
Par défaut, un domaine peut contenir environ un milliard de principaux de sécurité, tels que des utilisateurs, des groupes et des ordinateurs. Bien entendu, il n'existe aucun domaine avec autant d'objets activement utilisés. Toutefois, le support technique Microsoft a rencontré les cas suivants :  
  
-   L'approvisionnement de scripts d'administration ou de logiciels a créé accidentellement des utilisateurs, des groupes et des ordinateurs en bloc.  
  
-   De nombreux groupes de distribution et de sécurité inutilisés ont été créés par des utilisateurs délégués.  
  
-   De nombreux contrôleurs de domaine ont été rétrogradés ou restaurés, ou des métadonnées ont été nettoyées.  
  
-   Des récupérations de forêts ont été effectuées.  
  
-   L'opération InvalidateRidPool a été souvent effectuée.  
  
-   La valeur de Registre de la taille de bloc de RID a été augmentée de façon incorrecte.  
  
Toutes ces situations épuisent les identificateurs RID inutilement, souvent par erreur. Après de nombreuses années, les identificateurs RID sont devenus insuffisants pour quelques environnements qui ont été obligés de migrer vers un nouveau domaine ou d'effectuer des récupérations de forêts.  
  
Windows Server 2012 traite les questions liées à l'allocation RID qui ne sont devenues problématiques qu'avec l'ancienneté et l'omniprésence d'Active Directory. Celles-ci incluent une meilleure journalisation des événements, des limites plus appropriées et la capacité d’urgence à doubler la taille globale de l’espace RID global pour un domaine.  
  
### <a name="periodic-consumption-warnings"></a>Avertissements liés à la consommation périodiques  
Windows Server 2012 ajoute un suivi des événements liés à l'espace RID global qui émet un premier avertissement quand des limites cruciales sont franchies. Le modèle calcule la limite de dix (10) pour cent d'identificateurs RID utilisés dans le pool global et consigne un événement quand elle est atteinte. Il calcule ensuite les dix pour cent d'identificateurs RID utilisés suivants dans le pool restant et le cycle des événements continue. À mesure que l'espace RID global s'épuise, les événements s'accélèrent car les dix pour cent sont plus rapidement atteints dans un pool en baisse (mais le blocage du journal des événements empêche plus d'une entrée par heure). Le journal des événements système sur chaque contrôleur de domaine écrit l'événement d'avertissement Directory-Services-SAM 16658.  
  
En supposant un espace RID global de 30 bits par défaut, le premier événement est consigné pendant l'allocation du pool contenant le 107 374 182<sup>e</sup> identificateur RID. Le débit des événements s'accélère naturellement jusqu'au dernier point de contrôle de 100 000 avec 110 événements générés au total. Le comportement est semblable pour un espace RID global de 31 bits déverrouillé : début au niveau 214 748 365 et fin avec 117 événements.  
  
> [!IMPORTANT]  
> Cet événement n'est pas prévu ; examinez l'utilisateur, l'ordinateur et les processus de création de groupes immédiatement dans le domaine. La création de plus de 100 millions d'objets des services de domaine Active Directory sort de l'ordinaire.  
  
![Émission RID](media/Managing-RID-Issuance/ADDS_RID_TR_EventWaypoints2.png)  
  
### <a name="rid-pool-invalidation-events"></a>Événements d'invalidation du pool RID  
De nouvelles alertes d'événement signalent qu'un pool RID de contrôleur de domaine local a été ignoré. Ces alertes s'affichent à titre d'information et peuvent être prévues, en particulier en raison de la nouvelle fonctionnalité de contrôleur de domaine virtuel. Voir la liste d'événements ci-dessous pour obtenir des détails sur l'événement.  
  
### <a name="BKMK_RIDBlockMaxSize"></a>Limite de taille de bloc RID  
Un contrôleur de domaine demande généralement des allocations RID en blocs de 500 identificateurs RID à la fois. Vous pouvez remplacer cette valeur par défaut avec la valeur de Registre REG_DWORD suivante sur un contrôleur de domaine :  
  
```  
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NTDS\RID Values  
RID Block Size  
  
```  
  
Avant Windows Server 2012, aucune valeur maximale n'était appliquée dans cette clé de Registre, à l'exception de la valeur maximale DWORD implicite (qui est de 0xffffffff ou 4294967295). Cette valeur est considérablement supérieure à l'espace RID global total. Les administrateurs ont parfois configuré de façon inappropriée ou involontaire la taille de bloc de RID avec des valeurs qui épuisaient l'espace RID global à un rythme considérable.  
  
Dans Windows Server 2012, vous ne pouvez pas définir une valeur de Registre supérieure à 15 000 en décimal (0x3A98 en hexadécimal). Cela permet d'empêcher une allocation RID imprévue massive.  
  
Si vous affectez une valeur *supérieure* à 15 000, elle est traitée comme égale à 15 000 et le contrôleur de domaine consigne l'événement 16653 dans le journal des événements des services d'annuaire à chaque redémarrage jusqu'à ce que la valeur soit corrigée.  
  
### <a name="BKMK_GlobalRidSpaceUnlock"></a>Déverrouillage de la taille de l’espace RID global  
Avant Windows Server 2012, l'espace RID global était limité à 2<sup>30</sup> (ou 1 073 741 823) identificateurs RID au total. Une fois cette limite atteinte, seule une migration de domaine ou une récupération de forêt vers une période antérieure autorisait la création de nouveaux identificateurs, une récupération en cas d'urgence à tous points de vue. À compter de Windows Server 2012, la taille de 2<sup>31</sup> bits peut être déverrouillée pour augmenter le pool global jusqu'à 2 147 483 648 identificateurs RID.  
  
Les services de domaine Active Directory stockent ce paramètre dans un attribut masqué spécial nommé **SidCompatibilityVersion** dans le contexte RootDSE de tous les contrôleurs de domaine. Cet attribut n'est pas lisible à l'aide d'ADSIEdit, de LDP ni d'autres outils. Pour voir une augmentation de l'espace RID global, recherchez dans le journal des événements système l'événement d'avertissement 16655 de Directory-Services-SAM ou utilisez la commande Dcdiag suivante :  
  
```  
Dcdiag.exe /TEST:RidManager /v | find /i "Available RID Pool for the Domain"  
  
```  
  
Si vous augmentez le pool RID global, le pool disponible passe à 2 147 483 647 au lieu de la valeur 1 073 741 823 par défaut. Exemple :  
  
![Émission RID](media/Managing-RID-Issuance/ADDS_RID_TR_Dcdiag.png)  
  
> [!WARNING]  
> Ce déverrouillage est destiné *uniquement* à empêcher le manque d'identificateurs RID et doit être utilisé *uniquement* avec l'application d'un plafond RID (voir la section suivante). Ne le définissez pas « à titre préventif » dans des environnements qui ont des millions d'identificateurs RID restants et une faible croissance, car des problèmes de compatibilité des applications peuvent exister avec des identificateurs SID générés à partir du pool RID déverrouillé.  
>   
> Cette opération de déverrouillage ne peut pas être rétablie ni supprimée, sauf par une récupération de forêt complète vers des sauvegardes antérieures.  
  
#### <a name="important-caveats"></a>Réserves importantes  
Les contrôleurs de domaine Windows Server 2003 et Windows Server 2008 ne peuvent pas émettre de RID quand le 31<sup>e</sup> bit du pool RID global est déverrouillé. Les contrôleurs de domaine Windows Server 2008 R2 *peuvent* <sup>utiliser 31 RID</sup> de bit, *mais uniquement s'* ils ont installé le correctif logiciel [KB 2642658](https://support.microsoft.com/kb/2642658) . Les contrôleurs de domaine non pris en charge et non corrigés traitent le pool RID global comme s'il était épuisé quand il est déverrouillé.  
  
Cette fonctionnalité n'est pas appliquée selon n'importe quel niveau fonctionnel de domaine ; veillez à ce que seuls des contrôleurs de domaine Windows Server 2012 ou Windows Server 2008 R2 mis à jour existent dans le domaine.  
  
#### <a name="implementing-unlocked-global-rid-space"></a>Implémentation de l'espace RID global déverrouillé  
Pour déverrouiller le pool RID au niveau du 31<sup>e</sup> bit après avoir reçu l'alerte de plafond RID (voir ci-dessous), procédez comme suit :  
  
1.  Vérifiez que le rôle de maître RID est exécuté sur un contrôleur de domaine Windows Server 2012. Dans le cas contraire, transférez-le vers un contrôleur de domaine Windows Server 2012.  
  
2.  Exécutez LDP.exe.  
  
3.  Cliquez sur le menu **Connexion**, sur **Se connecter** pour le maître RID Windows Server 2012 sur le port 389, puis sur **Lier** en tant qu'administrateur de domaine.  
  
4.  Cliquez sur le menu **Parcourir**, puis sur **Modifier**.  
  
5.  Vérifiez que **Nom unique** est vide.  
  
6.  Dans **Modifier l'entrée Attribut**, tapez :  
  
    ```  
    SidCompatibilityVersion  
    ```  
  
7.  Dans **Valeurs**, tapez :  
  
    ```  
    1  
    ```  
  
8.  Vérifiez que l'opération **Ajouter** est sélectionnée dans **Opération** et cliquez sur **Entrée**. La **Liste d'entrées** est ainsi mise à jour.  
  
9. Sélectionnez les options **Synchrone** et **Étendu**, puis cliquez sur **Exécuter**.  
  
    ![Émission RID](media/Managing-RID-Issuance/ADDS_RID_TR_LDPModify.png)  
  
10. Si l'opération réussit, la fenêtre de sortie LDP affiche ce qui suit :  
  
    ```  
    ***Call Modify...  
     ldap_modify_ext_s(Id, '(null)',[1] attrs, SvrCtrls, ClntCtrls);  
    modified "".  
  
    ```  
  
    ![Émission RID](media/Managing-RID-Issuance/ADDS_RID_TR_LDPModifySuccess.png)  
  
11. Confirmez que le pool RID global a été augmenté en recherchant dans le journal des événements système sur ce contrôleur de domaine l'événement d'information Directory-Services-SAM 16655.  
  
### <a name="rid-ceiling-enforcement"></a>Application d'un plafond RID  
Pour offrir une mesure de protection et sensibiliser davantage les utilisateurs aux problèmes d'administration, Windows Server 2012 introduit un plafond artificiel sur la plage RID globale à dix (10) pour cent d'identificateurs RID restants dans l'espace global. À moins d'un (1) pour cent du plafond artificiel, les contrôleurs de domaine qui demandent des pools RID écrivent l'événement d'avertissement Directory-Services-SAM 16656 dans le journal des événements système. Quand ils atteignent le plafond de dix pour cent sur les opérations FSMO du maître RID, ils écrivent l'événement Directory-Services-SAM 16657 dans le journal des événements système et n'alloue aucun autre pool RID tant que le plafond n'est pas modifié. Vous devez ainsi évaluer l'état du maître RID dans le domaine et traiter l'éventuelle perte de contrôle de l'allocation RID ; cela protège également les domaines contre l'épuisement de tout l'espace RID.  
  
Ce plafond est codé en dur à dix pour cent de l'espace RID disponible restant. Autrement dit, le plafond est activé quand le maître RID alloue un pool qui inclut le RID correspondant à quatre-vingt-dix (90) pour cent de l'espace RID global.  
  
-   Pour les domaines par défaut, le premier seuil de déclenchement est 2<sup>30</sup>-1 * 0,90 = 966 367 640 (ou 107 374 183 identificateurs RID restants).  
  
-   Pour les domaines avec un espace RID de 31 bits déverrouillé, le seuil de déclenchement est 2<sup>31</sup>-1 * 0,90 = 1 932 735 282 identificateurs RID (ou 214 748 365 identificateurs RID restants).  
  
Une fois déclenché, le maître RID affecte à l'attribut Active Directory **msDS-RIDPoolAllocationEnabled** (nom commun **ms-DS-RID-Pool-Allocation-Enabled**) la valeur FALSE sur l'objet :  
  
CN = RID Manager $, CN = System, DC = *<domain>*  
  
L'événement 16657 est consigné et aucune autre émission de blocs RID n'est permise sur tous les contrôleurs de domaine. Les contrôleurs de domaine continuent de consommer les pools RID en attente déjà émis à leur intention.  
  
Pour supprimer le bloc et autoriser la poursuite de l'allocation de pools RID, choisissez la valeur TRUE. Lors de la prochaine allocation RID effectuée par le maître RID, l'attribut reprendra sa valeur NOT SET par défaut. Après cela, il n'existe pas d'autre plafond et l'espace RID global finit par être insuffisant, ce qui nécessite une migration de domaine ou une récupération de forêt.  
  
#### <a name="removing-the-ceiling-block"></a>Suppression du bloc du plafond  
Pour supprimer le bloc une fois le plafond artificiel atteint, procédez comme suit :  
  
1.  Vérifiez que le rôle de maître RID est exécuté sur un contrôleur de domaine Windows Server 2012. Dans le cas contraire, transférez-le vers un contrôleur de domaine Windows Server 2012.  
  
2.  Exécutez LDP.exe.  
  
3.  Cliquez sur le menu **Connexion**, sur *Se connecter* pour le maître RID Windows Server 2012 sur le port 389, puis sur **Lier** en tant qu'administrateur de domaine.  
  
4.  Cliquez sur le menu **Affichage** et sur **Arborescence**, puis pour **Nom unique de base**, sélectionnez le propre contexte de nommage du domaine du maître RID. Cliquez sur **OK**.  
  
5.  Dans le volet de navigation, descendez dans la hiérarchie jusqu'au conteneur **CN=System** et cliquez sur l'objet **CN=RID Manager$** . Cliquez avec le bouton droit dessus et cliquez sur **Modifier**.  
  
6.  Dans Modifier l'entrée Attribut, tapez :  
  
    ```  
    MsDS-RidPoolAllocationEnabled  
    ```  
  
7.  Dans **Valeurs**, tapez (en majuscules) :  
  
    ```  
    TRUE  
    ```  
  
8.  Sélectionnez **Remplacer** dans **Opération** et cliquez sur **Entrée**. La **Liste d'entrées** est ainsi mise à jour.  
  
9. Activez les options **Synchrone** et **Étendu**, puis cliquez sur **Exécuter**.  
  
    ![Émission RID](media/Managing-RID-Issuance/ADDS_RID_TR_LDPRaiseCeiling.png)  
  
10. Si l'opération réussit, la fenêtre de sortie LDP affiche ce qui suit :  
  
    ```  
    ***Call Modify...  
    ldap_modify_ext_s(ld, 'CN=RID Manager$,CN=System,DC=<domain>',[1] attrs, SvrCtrls, ClntCtrls);  
    Modified "CN=RID Manager$,CN=System,DC=<domain>".  
  
    ```  
  
    ![Émission RID](media/Managing-RID-Issuance/ADDS_RID_TR_LDPRaiseCeilingSuccess.png)  
  
### <a name="other-rid-fixes"></a>Autres correctifs RID  
Les systèmes d'exploitation Windows Server précédents présentaient une fuite du pool RID quand l'attribut rIDSetReferences était manquant. Pour résoudre ce problème sur les contrôleurs de domaine qui exécutent Windows Server 2008 R2, installez le correctif logiciel de la [base de connaissances KB 2618669](https://support.microsoft.com/kb/2618669).  
  
### <a name="unfixed-rid-issues"></a>Problèmes RID non résolus  
Il est habituel d'avoir une fuite RID lors de l'échec de la création de comptes ; pendant la création d'un compte, l'échec épuise toujours un RID. Citons, à titre d'exemple, la création d'un utilisateur avec un mot de passe qui ne répond pas aux exigences de complexité.  
  
### <a name="rid-fixes-for-earlier-versions-of-windows-server"></a>Correctifs RID pour les versions antérieures de Windows Server  
Des correctifs logiciels Windows Server 2008 R2 ont été commercialisés pour l'ensemble des correctifs et modifications ci-dessus. Actuellement, aucun correctif logiciel Windows Server 2008 n'est prévu ni en cours.  
  
## <a name="BKMK_Tshoot"></a>Résolution des problèmes d’émission RID  
  
### <a name="introduction-to-troubleshooting"></a>Introduction à la résolution des problèmes  
La résolution des problèmes associés à l'émission RID nécessite une méthode logique et linéaire. Sauf si vous analysez attentivement vos journaux des événements à la recherche d'erreurs et d'avertissements déclenchés par RID, les premières indications d'un problème risquent d'être des créations de comptes qui ont échoué. La solution pour résoudre les problèmes associés à l'émission RID consiste à comprendre quand le symptôme est prévu ou non ; de nombreux problèmes d'émission RID peuvent affecter uniquement un contrôleur de domaine et n'avoir aucun lien avec les améliorations apportées aux composants. Ce simple diagramme ci-dessous permet de présenter plus clairement ces décisions :  
  
![Émission RID](media/Managing-RID-Issuance/adds_rid_issuance_troubleshooting.png)  
  
### <a name="troubleshooting-options"></a>Options de résolution des problèmes  
  
#### <a name="logging-options"></a>Options de journalisation  
Toute la journalisation de l'émission RID a lieu dans le journal des événements système, sous la source Directory-Services-SAM. La journalisation est activée et configurée pour un maximum de commentaires par défaut. Si aucune entrée n'est enregistrée pour les nouvelles modifications de composants dans Windows Server 2012, traitez le problème comme un problème d'émission RID classique (autrement dit, hérité et antérieur à Windows Server 2012) vu dans Windows 2008 R2 ou des systèmes d'exploitation plus anciens.  
  
#### <a name="utilities-and-commands-for-troubleshooting"></a>Utilitaires et commandes pour la résolution des problèmes  
Pour résoudre les problèmes non décrits par les journaux indiqués ci-dessus, en particulier les problèmes d'émission RID plus anciens, utilisez la liste suivante d'outils comme point de départ :  
  
-   Dcdiag.exe  
  
-   Repadmin.exe  
  
-   Moniteur réseau 3.4  
  
### <a name="general-methodology-for-troubleshooting-domain-controller-configuration"></a>Méthodologie générale pour la résolution des problèmes de configuration des contrôleurs de domaine  
  
1.  Est-ce que l'erreur est causée par un simple problème d'autorisations ou de disponibilité de contrôleur de domaine ?  
  
    1.  Essayez-vous de créer un principal de sécurité sans les autorisations nécessaires ? Examinez la sortie pour les erreurs d'accès refusé.  
  
    2.  Est-ce qu'un contrôleur de domaine est disponible ? Examinez l'erreur retournée ou LDAP ou les messages de disponibilité de contrôleur de domaine.  
  
2.  Est-ce que l'erreur retournée mentionne spécifiquement les identificateurs RID, et est-elle assez caractéristique pour être utilisée comme indication ? Si tel est le cas, suivez l'indication.  
  
3.  Est-ce que l'erreur retournée mentionne spécifiquement les identificateurs RID sans être par ailleurs caractéristique ? Par exemple, « Windows ne peut pas créer l'objet car le service d'annuaire n'a pas pu allouer un identificateur relatif ».  
  
    1.  Examinez le journal des événements système sur le contrôleur de domaine pour les événements RID « hérités » (antérieurs à Windows Server 2012) détaillés dans la [demande de pool RID](https://technet.microsoft.com/library/ee406152(WS.10).aspx) (16642, 16643, 16644, 16645, 16656).  
  
    2.  Recherchez dans le journal des événements système sur le contrôleur de domaine et le maître RID de nouveaux événements indiquant des blocs détaillés ci-dessous dans cette rubrique (16655, 16656, 16657).  
  
    3.  Validez l'intégrité de la réplication Active Directory avec Repadmin.exe et la disponibilité du maître RID avec **Dcdiag.exe /test:ridmanager /v**. Autorisez les captures réseau recto verso entre le contrôleur de domaine et le maître RID si ces tests ne sont pas concluants.  
  
### <a name="troubleshooting-specific-problems"></a>Résolution de problèmes spécifiques  
Les nouveaux messages suivants sont consignés dans le journal des événements système sur les contrôleurs de domaine Windows Server 2012. Les systèmes de suivi d'intégrité Active Directory automatisés, tels que System Center Operations Manager, doivent analyser ces événements ; tous sont importants et certains indiquent des problèmes de domaine sérieux.  
  
|||  
|-|-|  
|ID d’événement|16653|  
|`Source`|Directory-Services-SAM|  
|Sévérité|Warning|  
|`Message`|Une taille de pool pour des identificateurs de comptes (RID) qui a été configurée par un administrateur est supérieure au maximum pris en charge. La valeur maximale %1 est utilisée quand le contrôleur de domaine est le maître RID.<br /><br />Pour plus d'informations, voir [Limite de la taille de bloc RID](../../ad-ds/manage/../../ad-ds/manage/../../ad-ds/manage/../../ad-ds/manage/Managing-RID-Issuance.md#BKMK_RIDBlockMaxSize).|  
|Remarques et résolution|La valeur maximale pour la taille de bloc de RID est maintenant 15 000 en décimal (0x3A98 en hexadécimal). Un contrôleur de domaine ne peut pas demander plus de 15 000 identificateurs RID. Cet événement est consigné à chaque redémarrage jusqu'à ce que la valeur définie soit inférieure ou égale à cette valeur maximale.|  
  
|||  
|-|-|  
|ID d’événement|16654|  
|`Source`|Directory-Services-SAM|  
|Sévérité|Informationnel|  
|`Message`|Un pool d'identificateurs de comptes (RID) a été invalidé Ceci peut se produire dans les cas attendus suivants :<br /><br />1. Un contrôleur de domaine est restauré à partir de la sauvegarde.<br /><br />2. Un contrôleur de domaine exécuté sur un ordinateur virtuel est restauré à partir de la capture instantanée.<br /><br />3. Un administrateur a invalidé le pool manuellement.<br /><br />Pour plus d'informations, voir https://go.microsoft.com/fwlink/?LinkId=226247.|  
|Remarques et résolution|Si cet événement est inattendu, contactez tous les administrateurs de domaine et identifiez celui qui a effectué l'action. Le journal des événements des services d'annuaire contient également d'autres informations sur le moment où l'une de ces étapes a été effectuée.|  
  
|||  
|-|-|  
|ID d’événement|16655|  
|`Source`|Directory-Services-SAM|  
|Sévérité|Informationnel|  
|`Message`|La limite supérieure pour les identificateurs de comptes (RID) est passée à %1.|  
|Remarques et résolution|Si cet événement est inattendu, contactez tous les administrateurs de domaine et identifiez celui qui a effectué l'action. Cet événement indique l'augmentation de la taille du pool RID global au-delà de la valeur par défaut 2<sup>30</sup>, qui n'a pas eu lieu automatiquement, mais résulte d'une action de l'administrateur.|  
  
|||  
|-|-|  
|ID d’événement|16656|  
|`Source`|Directory-Services-SAM|  
|Sévérité|Warning|  
|`Message`|La limite supérieure pour les identificateurs de comptes (RID) est passée à %1.|  
|Remarques et résolution|Action requise ! Un pool d'identificateurs de comptes (RID) a été alloué à ce contrôleur de domaine La valeur du pool indique que ce domaine a consommé une partie importante du total des identificateurs de comptes disponibles.<br /><br />Un mécanisme de protection est activé lorsque le domaine atteint le seuil suivant : nombre total d’identificateurs de comptes disponibles restants :% 1.  Le mécanisme de protection empêche toute création de comptes jusqu'à ce que vous réactiviez manuellement l'allocation des identificateurs de comptes sur le contrôleur de domaine du maître RID.<br /><br />Pour plus d'informations, voir https://go.microsoft.com/fwlink/?LinkId=228610.|  
  
|||  
|-|-|  
|ID d’événement|16657|  
|`Source`|Directory-Services-SAM|  
|Sévérité|Error|  
|`Message`|Action requise ! Ce domaine a consommé une partie importante du total des identificateurs de comptes (RID) disponibles. Un mécanisme de protection a été activé, car le total des identificateurs de comptes disponibles restants est inférieur à : X% [argument de plafond artificiel].<br /><br />Le mécanisme de protection empêche toute création de comptes jusqu'à ce que vous réactiviez manuellement l'allocation des identificateurs de comptes sur le contrôleur de domaine du maître RID.<br /><br />Il est extrêmement important d'effectuer certains diagnostics avant de réactiver la création de comptes pour s'assurer que ce domaine ne consomme pas les identificateurs de comptes à une vitesse anormalement élevée. Tout problème identifié doit être résolu avant de réactiver la création de comptes.<br /><br />L'échec du diagnostic et de la résolution de tout problème sous-jacent provoquant une vitesse anormalement élevée de consommation des identificateurs de compte peut amener à l'épuisement des identificateurs de comptes dans le domaine, après quoi la création de comptes sera définitivement désactivée dans ce domaine.<br /><br />Pour plus d'informations, voir https://go.microsoft.com/fwlink/?LinkId=228610.|  
|Remarques et résolution|Contactez tous les administrateurs de domaine et informez-les qu'aucun autre principal de sécurité ne peut être créé dans ce domaine jusqu'à ce que cette protection soit remplacée. Pour plus d'informations sur la façon de remplacer la protection et d'augmenter éventuellement le pool RID global, voir [Déverrouillage de la taille de l'espace RID global](../../ad-ds/manage/../../ad-ds/manage/../../ad-ds/manage/../../ad-ds/manage/Managing-RID-Issuance.md#BKMK_GlobalRidSpaceUnlock).|  
  
|||  
|-|-|  
|ID d’événement|16658|  
|`Source`|Directory-Services-SAM|  
|Sévérité|Warning|  
|`Message`|Cet événement est une mise à jour périodique de la quantité totale restante d'identificateurs de comptes (RID) disponibles. Le nombre d’identificateurs de compte restants est approximativement :% 1.<br /><br />Les identificateurs de comptes sont utilisés quand des comptes sont créés ; une fois épuisés, aucun compte ne peut être créé dans le domaine.<br /><br />Pour plus d'informations, voir https://go.microsoft.com/fwlink/?LinkId=228745.|  
|Remarques et résolution|Contactez tous les administrateurs de domaine et informez-les que la consommation d'identificateurs RID a franchi une limite cruciale ; déterminez s'il s'agit ou non d'un comportement prévu en examinant les modèles de création de clients approuvés de sécurité. Il est peu probable que vous ne voyiez jamais cet événement, car il signifie qu'au moins 100 millions d'identificateurs RID ont été alloués.|  
  
## <a name="see-also"></a>Voir aussi  
[Gestion de l’émission RID dans Windows Server 2012](http://blogs.technet.com/b/askds/archive/2012/08/10/managing-rid-issuance-in-windows-server-2012.aspx)  
  


