---
title: "Comment configurer des comptes protégés"
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.service: na
ms.suite: na
ms.technology: security-auditing
ms.tgt_pltfrm: na
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: f2effdb7a82a25c810bc041475e760145de492d8
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="how-to-configure-protected-accounts"></a>Comment configurer des comptes protégés

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Par le biais d’attaques Pass-the-hash (PtH), une personne malveillante peut s’authentifier sur un serveur distant ou un service à l’aide du hachage NTLM sous-jacent du mot de passe d’un utilisateur (ou autres dérivés d’informations d’identification). Microsoft a précédemment [publié des conseils](https://www.microsoft.com/download/details.aspx?id=36036) à atténuer les attaques pass-the-hash.  Windows Server 2012 R2 inclut de nouvelles fonctionnalités pour réduire ce type d’attaques. Pour plus d’informations sur d’autres fonctionnalités de sécurité permettant de protéger contre le vol d’informations d’identification, voir [gestion et Protection des informations d’identification](https://technet.microsoft.com/library/dn408190.aspx). Cette rubrique explique comment configurer les nouvelles fonctionnalités suivantes:  
  
-   [Utilisateurs protégés](how-to-configure-protected-accounts.md#BKMK_AddtoProtectedUsers)  
  
-   [Stratégies d’authentification](how-to-configure-protected-accounts.md#BKMK_CreateAuthNPolicies)  
  
-   [Silos de stratégies d’authentification](how-to-configure-protected-accounts.md#BKMK_CreateAuthNPolicySilos)  
  
Il existe des préventions supplémentaires intégrées à Windows 8.1 et Windows Server 2012 R2 pour vous protéger contre le vol d’informations d’identification, qui sont traités dans les rubriques suivantes:  
  
-   [Mode d’administration restreinte pour Bureau à distance](http://blogs.technet.com/b/kfalde/archive/20../restricted-admin-mode-for-rdp-in-windows-8-1-2012-r2.aspx)  
  
-   [Protection LSA](https://technet.microsoft.com/library/dn408187)  
  
## <a name="BKMK_AddtoProtectedUsers"></a>Utilisateurs protégés  
Les utilisateurs protégés est un nouveau groupe de sécurité global auquel vous pouvez ajouter des utilisateurs nouveaux ou existants. Périphériques Windows 8.1 et Windows Server 2012 R2 hôtes se comportent différemment avec les membres de ce groupe pour fournir une meilleure protection contre le vol d’informations d’identification. Pour un membre du groupe, un périphérique Windows 8.1 ou un ordinateur hôte Windows Server 2012 R2 ne pas met en cache les informations d’identification qui ne sont pas pris en charge pour les utilisateurs protégés. Les membres de ce groupe n’ont aucune protection supplémentaire s’ils sont connectés à un périphérique qui exécute une version de Windows antérieure à Windows 8.1.  
  
Regroupement les membres d’utilisateurs protégés qui sont authentifiés sur des périphériques Windows 8.1 et les ordinateurs hôtes Windows Server 2012 R2 peuvent *n’est plus* utiliser:  
  
-   Par défaut de la délégation d’informations d’identification (CredSSP): informations d’identification ne sont pas mises en cache en texte en clair même lorsque le **autoriser la délégation des informations d’identification par défaut** stratégie est activée  
  
-   Windows Digest - en texte en clair, informations d’identification ne sont pas mis en cache même si elles sont activées  
  
-   NTLM: NTOWF n’est pas mis en cache.  
  
-   Les clés à long terme Kerberos - Kerberos ticket-granting ticket (TGT ticket) s’obtient à l’ouverture de session et ne peut pas être à nouveau obtenu automatiquement  
  
-   Ouverture de session hors ligne: le vérificateur d’ouverture de session mis en cache n’est pas créé  
  
Si le niveau fonctionnel du domaine est Windows Server 2012 R2, les membres du groupe ne peuvent plus:  
  
-   S’authentifier à l’aide de l’authentification NTLM  
  
-   Utiliser les suites de chiffrement des (Data Encryption Standard) ou RC4 dans la pré-authentification Kerberos  
  
-   Être délégués en utilisant la délégation non contrainte ou contrainte  
  
-   Renouveler les tickets utilisateur (TGT) au-delà de la durée de vie initiale de 4 heures  
  
Pour ajouter des utilisateurs au groupe, vous pouvez utiliser [outils d’interface utilisateur](https://technet.microsoft.com/library/cc753515.aspx) comme Active Directory Administrative Center (ADAC) ou utilisateurs Active Directory et les ordinateurs ou un outil de ligne de commande comme [groupe Dsmod](https://technet.microsoft.com/library/cc732423.aspx), ou Windows PowerShell[Add-ADGroupMember](https://technet.microsoft.com/library/ee617210.aspx) applet de commande. Comptes de services et les ordinateurs *ne doivent pas* être membres du groupe utilisateurs protégés. L’appartenance pour ces comptes ne fournit aucuns protection locale car le mot de passe ou le certificat est toujours disponible sur l’ordinateur hôte.  
  
> [!WARNING]  
> Les restrictions d’authentification n’ont aucune solution de contournement, ce qui signifie que les membres de groupes dotés de privilèges élevés tels que le groupe Administrateurs de l’entreprise ou Admins du domaine sont soumis aux mêmes restrictions que les autres membres du groupe utilisateurs protégés. Si tous les membres de ces groupes sont ajoutés au groupe utilisateurs protégés, il est possible pour l’ensemble de ces comptes peuvent être verrouillés. Vous ne devez jamais ajouter tous les comptes dotés de privilèges élevés au groupe utilisateurs protégés jusqu'à ce que vous avez soigneusement testé l’impact potentiel.  
  
Membres du groupe utilisateurs protégés doivent être en mesure de s’authentifier à l’aide de Kerberos avec les normes AES (Advanced Encryption). Cette méthode nécessite des clés AES pour le compte dans Active Directory. Le compte administrateur intégré ne dispose pas de clé AES sauf si le mot de passe a été modifié sur un contrôleur de domaine qui exécute Windows Server 2008 ou version ultérieure. En outre, n’importe quel compte qui dispose d’un mot de passe qui a été modifié sur un contrôleur de domaine qui exécute une version antérieure de Windows Server, est verrouillé. Par conséquent, suivez ces meilleures pratiques:  
  
-   Ne pas effectuer de test dans des domaines sauf si **tous les contrôleurs de domaine exécutent Windows Server 2008 ou version ultérieure**.  
  
-   **Mot de passe de modification** pour tous les comptes de domaine qui ont été créés *avant* le domaine a été créé. Dans le cas contraire, ces comptes ne peut pas être authentifiés.  
  
-   **Mot de passe de modification** de groupe pour chaque utilisateur avant d’ajouter le compte pour les utilisateurs protégés ou vérifier que le mot de passe a été modifié récemment sur un contrôleur de domaine qui exécute Windows Server 2008 ou version ultérieure.  
  
### <a name="BKMK_Prereq"></a>Configuration requise pour utiliser des comptes protégés  
Ces derniers doivent respecter les exigences de déploiement suivantes:  
  
-   Pour fournir des restrictions côté client pour les utilisateurs protégés, les hôtes doivent exécuter Windows 8.1 ou Windows Server 2012 R2. Un utilisateur a uniquement à l’ouverture de session avec un compte qui est membre d’un groupe utilisateurs protégés. Dans ce cas, le groupe utilisateurs protégés peut être créé par [transférant le rôle d’émulateur de contrôleur principal de domaine](https://technet.microsoft.com/library/cc816944(v=ws.10).aspx) à un contrôleur de domaine qui exécute Windows Server 2012 R2. Une fois l’objet de groupe est répliqué vers d’autres contrôleurs de domaine, le rôle d’émulateur PDC peut être hébergé sur un contrôleur de domaine qui exécute une version antérieure de Windows Server.  
  
-   Pour fournir des restrictions côté contrôleur de domaine pour les utilisateurs protégés, c'est-à-dire pour limiter l’utilisation de l’authentification NTLM, et d’autres restrictions, le niveau fonctionnel du domaine doit être Windows Server 2012 R2. Pour plus d’informations sur les niveaux fonctionnels, voir [niveaux fonctionnels des Services de domaine d’Active Directory présentation (AD DS)](../../identity/ad-ds/active-directory-functional-levels.md).  
  
### <a name="BKMK_TrubleshootingEvents"></a>Résoudre les problèmes des événements associés aux utilisateurs protégés  
Cette section traite des nouveaux journaux pour résoudre les événements liés aux utilisateurs protégés et comment les utilisateurs protégés peuvent répercuter les modifications pour le dépanner soit ticket-granting tickets TGT d’expiration ou délégation.  
  
#### <a name="new-logs-for-protected-users"></a>Nouveaux journaux pour les utilisateurs protégés  
Deux nouveaux journaux d’administration opérationnels sont disponibles pour aider à résoudre les événements qui sont associés aux utilisateurs protégés: utilisateur protégé - journal Client et échecs d’utilisateur protégé - journal du contrôleur de domaine. Ces nouveaux journaux se trouvent dans l’Observateur d’événements et est désactivées par défaut. Pour activer un journal, cliquez sur **journaux des Applications et Services**, cliquez sur **Microsoft**, cliquez sur **Windows**, cliquez sur **authentification**, puis cliquez sur le nom du journal et cliquez sur **Action** (ou cliquez sur le journal) et cliquez sur **activer le journal**.  
  
Pour plus d’informations sur les événements dans ces journaux, consultez [stratégies d’authentification et Silos de stratégies d’authentification](https://technet.microsoft.com/library/dn486813.aspx).  
  
#### <a name="troubleshoot-tgt-expiration"></a>Résoudre les problèmes d’expiration du ticket TGT  
Normalement, le contrôleur de domaine définit la durée de vie TGT et le renouvellement basé sur la stratégie de domaine comme indiqué dans la fenêtre Éditeur de gestion de stratégie de groupe suivant.  
  
![comptes protégés](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_TGTExpiration.png)  
  
Pour **utilisateurs protégés**, les paramètres suivants sont codés en dur:  
  
-   Durée de vie maximale du ticket utilisateur: 240 minutes  
  
-   Durée de vie maximale pour le renouvellement du ticket utilisateur: 240 minutes  
  
#### <a name="troubleshoot-delegation-issues"></a>Résoudre les problèmes de délégation  
Auparavant, si une technologie qui utilise la délégation Kerberos a échoué, le compte client était vérifié pour voir si **compte est sensible et ne peut pas être délégué** a été définie. Toutefois, si le compte est membre du **utilisateurs protégés**, il n’ait pas ce paramètre configuré dans Active Directory Administrative Center (ADAC). Par conséquent, vérifiez le paramètre et l’appartenance au groupe lors de la résolution des problèmes de délégation.  
  
![comptes protégés](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_TshootDelegation.gif)  
  
### <a name="BKMK_AuditAuthNattempts"></a>Auditer les tentatives d’authentification  
Pour auditer les tentatives d’authentification explicitement pour les membres de la **utilisateurs protégés** groupe, vous pouvez continuer à collecter les événements de sécurité journal d’audit ou de collecter les données dans les nouveaux journaux d’administration opérationnels. Pour plus d’informations sur ces événements, voir [stratégies d’authentification et Silos de stratégies d’authentification](https://technet.microsoft.com/library/dn486813.aspx)  
  
### <a name="BKMK_ProvidePUdcProtections"></a>Fournir les protections côté contrôleur de domaine pour les services et les ordinateurs  
Comptes de services et les ordinateurs ne peuvent pas être membres du **utilisateurs protégés**. Cette section décrit les protections basé sur le contrôleur de domaine peuvent être offertes pour ces comptes:  
  
-   Rejet de l’authentification NTLM: configurable uniquement via [stratégies du bloc NTLM](https://technet.microsoft.com/library/jj865674(v=ws.10).aspx)  
  
-   Rejet des (Data Encryption Standard) dans la pré-authentification Kerberos: contrôleurs de domaine Windows Server 2012 R2 n’acceptent pas la norme pour les comptes d’ordinateur sauf s’ils sont configurés pour DES uniquement, car chaque version de Windows publiée avec Kerberos prend également en charge RC4.  
  
-   Rejet de RC4 dans la pré-authentification Kerberos: non configurable.  
  
    > [!NOTE]  
    > Bien qu’il soit possible de [modifier la configuration des types de chiffrement pris en charge](http://blogs.msdn.com/b/openspecification/archive/20../windows-configurations-for-kerberos-supported-encryption-type.aspx), il n’est pas recommandé de changer ces paramètres pour les comptes d’ordinateur sans les tester dans l’environnement cible.  
  
-   Restreindre les tickets utilisateur (TGT) à une durée de vie initiale de 4 heures: utiliser des stratégies d’authentification.  
  
-   Refuser la délégation avec délégation non contrainte ou contrainte: pour restreindre un compte, ouvrez Active Directory Administrative Center (ADAC) et sélectionnez le **compte est sensible et ne peut pas être délégué** case à cocher.  
  
    ![comptes protégés](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_TshootDelegation.gif)  
  
## <a name="BKMK_CreateAuthNPolicies"></a>Stratégies d’authentification  
Stratégies d’authentification est un nouveau conteneur dans AD DS qui contient les objets de stratégie d’authentification. Stratégies d’authentification peuvent spécifier les paramètres qui permettent de prévenir l’exposition au vol d’informations d’identification, telles que la restriction de durée de vie TGT des comptes ou l’ajout d’autres conditions associées aux revendications.  
  
Dans Windows Server 2012, le contrôle d’accès dynamique a introduit une classe d’objet étendue à la forêt Active Directory appelée stratégie d’accès centralisée pour fournir un moyen simple de configurer des serveurs de fichiers dans une organisation. Dans Windows Server 2012 R2, une nouvelle classe d’objet appelée stratégie d’authentification (objectClass msDS-AuthNPolicies) peut être utilisée pour appliquer la configuration de l’authentification aux classes de compte dans les domaines Windows Server 2012 R2. Classes de compte actives Directory sont:  
  
-   Utilisateur  
  
-   Ordinateur  
  
-   Compte de Service administré et le groupe (GMSA) du compte de Service administré  
  
### <a name="quick-kerberos-refresher"></a>Actualisateur Kerberos rapide  
Le protocole d’authentification Kerberos se compose de trois types d’échanges, également connus en tant que sous-protocoles:  
  
![comptes protégés](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_KerbRefresher.gif)  
  
-   L’échange de Service (AS) d’authentification (KRB_AS_ *)  
  
-   L’échange de Service (TGS) Ticket-Granting (KRB_TGS_ *)  
  
-   L’échange Client/serveur (PA) (KRB_AP_ *)  
  
L’échange AS est, le client utilise un mot de passe du compte ou la clé privée pour créer un pré-authentificateur afin de demander un ticket-granting ticket (TGT ticket). Cela se produit à l’utilisateur de session ou la première fois qu’un ticket de service est nécessaire.  
  
L’échange TGS est où le ticket TGT du compte est utilisé pour créer un authentificateur pour demander un ticket de service. Cela se produit lorsqu’une connexion authentifiée est nécessaire.  
  
L’échange AP se produit généralement en tant que données à l’intérieur du protocole d’application et n’est pas affecté par des stratégies d’authentification.  
  
Pour plus d’informations, voir [Kerberos Version5 authentification protocole fonctionnement] (https://technet.microsoft.com/library/cc772815(v=WS.10.aspx.  
  
### <a name="overview"></a>Vue d’ensemble  
Stratégies d’authentification complètent les utilisateurs protégés en fournissant un moyen d’appliquer des restrictions configurables aux comptes et en fournissant des restrictions pour les comptes de services et les ordinateurs. Stratégies d’authentification sont appliquées pendant l’échange AS ou TGS exchange.  
  
Vous pouvez restreindre l’authentification initiale ou l’échange AS en configurant:  
  
-   Une durée de vie du ticket TGT  
  
-   Conditions de contrôle d’accès pour restreindre l’utilisateur de session, qui doit être remplie par les périphériques à partir duquel provient l’échange AS  
  
![comptes protégés](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_RestrictAS.gif)  
  
Vous pouvez restreindre les demandes de ticket de service par le biais d’un échange de service (TGS) ticket-granting en configurant:  
  
-   Conditions de contrôle d’accès qui doivent être remplies par le client (utilisateur, service, ordinateur) ou un périphérique à partir duquel provient l’échange TGS  
  
### <a name="BKMK_ReqForAuthnPolicies"></a>Configuration requise pour l’utilisation de stratégies d’authentification  
  
|Stratégie|Configuration requise|  
|-----|--------|  
|Fournir des durées de vie TGT personnalisées| Domaines de compte de niveau fonctionnel du domaine Windows Server 2012 R2|  
|Restreindre l’authentification|-Domaines de compte de niveau fonctionnel du domaine Windows Server 2012 R2 avec prise en charge du contrôle d’accès dynamique<br />-Prise en charge Windows 8, Windows 8.1, Windows Server 2012 ou les appareils de Windows Server 2012 R2 avec le contrôle d’accès dynamique|  
|Restreindre l’émission de tickets de service qui est basée sur les groupes de sécurité et de compte d’utilisateur| Domaines de ressources de niveau fonctionnel du domaine Windows Server 2012 R2|  
|Restreindre l’émission de tickets de service basée sur les revendications d’utilisateur ou de compte d’appareil, de groupes de sécurité ou de revendications| Domaines de ressources de niveau fonctionnel du domaine Windows Server 2012 R2 avec prise en charge du contrôle d’accès dynamique|  
  
### <a name="restrict-a-user-account-to-specific-devices-and-hosts"></a>Restreindre un compte d’utilisateur aux ordinateurs hôtes et des périphériques spécifiques  
Un compte à valeur élevée avec des privilèges d’administration doit être un membre de la **utilisateurs protégés** groupe. Par défaut, aucun compte n’est membres de la **utilisateurs protégés** groupe. Avant d’ajouter des comptes au groupe, configurez la prise en charge du contrôleur de domaine et créer une stratégie d’audit pour vous assurer qu’il n’existe aucun problème majeur.  
  
#### <a name="configure-domain-controller-support"></a>Configurer la prise en charge du contrôleur de domaine  
Domaine de compte de l’utilisateur doit être au niveau fonctionnel du domaine Windows Server 2012 R2 (DFL). Vérifiez tous les contrôleurs de domaine sont Windows Server 2012 R2 et ensuite utilisent Active Directory Domains and Trusts à [augmenter le niveau fonctionnel du domaine](https://technet.microsoft.com/library/cc753104.aspx) vers Windows Server 2012 R2.  
  
**Pour configurer la prise en charge de contrôle d’accès dynamique**  
  
1.  Dans la stratégie par défaut de contrôleurs de domaine, cliquez sur **activé** pour activer **prise en charge du client de centre de Distribution de clés (KDC) pour les revendications, l’authentification composée et le blindage Kerberos** dans Configuration ordinateur | Modèles d’administration | Système | KDC.  
  
    ![comptes protégés](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_EnableKDCClaims.gif)  
  
2.  Sous **Options**, dans la zone de liste déroulante, sélectionnez **toujours fournir des revendications**.  
  
    > [!NOTE]  
    > **Prise en charge** peut également être configuré, mais étant donné que le domaine est à Windows Server 2012 R2 niveau fonctionnel du domaine, des contrôleurs de domaine ont toujours fournir des revendications permettra accès basée sur les revendications d’utilisateur vérifications lors de l’utilisation des appareils prenant en charge des revendications et des hôtes pour se connecter aux services prenant en charge les revendications.  
  
    ![comptes protégés](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_AlwaysProvideClaims.png)  
  
    > [!WARNING]  
    > La configuration **rejeter les demandes d’authentification non blindées** entraîne des échecs d’authentification à partir de n’importe quel système d’exploitation qui ne prend pas en charge le blindage Kerberos, tels que Windows 7 et les systèmes d’exploitation précédents, ou les systèmes d’exploitation à partir de Windows 8, qui n’ont pas été explicitement configurés pour prendre en charge.  
  
#### <a name="create-a-user-account-audit-for-authentication-policy-with-adac"></a>Créer un audit de compte d’utilisateur pour la stratégie d’authentification avec ADAC  
  
1.  Ouvrez le centre d’administration Active Directory (ADAC).  
  
    ![comptes protégés](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_OpenADAC.gif)  
  
    > [!NOTE]  
    > Le texte sélectionné **authentification** nœud est visible pour des domaines qui sont au niveau fonctionnel du domaine de Windows Server 2012 R2. Si le nœud n’apparaît pas, puis essayez à nouveau à l’aide d’un compte d’administrateur de domaine à partir d’un domaine qui se trouve au niveau fonctionnel du domaine de Windows Server 2012 R2.  
  
2.  Cliquez sur **stratégies d’authentification**, puis cliquez sur **New** pour créer une nouvelle stratégie.  
  
    ![comptes protégés](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_NewAuthNPolicy.gif)  
  
    Les stratégies d’authentification doivent avoir un nom d’affichage et sont appliquées par défaut.  
  
3.  Pour créer une stratégie d’audit uniquement, cliquez sur **auditer uniquement les restrictions de stratégie**.  
  
    ![comptes protégés](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_NewAuthNPolicyAuditOnly.gif)  
  
    Stratégies d’authentification sont appliquées en fonction du type de compte Active Directory. Une stratégie unique peut s’appliquent à tous les trois types de compte en configurant les paramètres pour chaque type. Types de compte sont:  
  
    -   Utilisateur  
  
    -   Ordinateur  
  
    -   Compte de Service administré de groupe et compte de Service administré  
  
    Si vous avez étendu le schéma avec de nouveaux principaux qui peuvent être utilisés par le centre de Distribution de clés (KDC), le nouveau type de compte est classé à partir du type de compte dérivé le plus proche.  
  
4.  Pour configurer une durée de vie TGT des comptes d’utilisateur, sélectionnez le **une durée de vie de Ticket-Granting Ticket pour les comptes d’utilisateur** case à cocher et entrez la durée en minutes.  
  
    ![comptes protégés](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_TGTLifetime.gif)  
  
    Par exemple, si vous voulez une durée de vie de 10 heures maximum TGT, entrez **600** comme indiqué. Si aucune durée de vie TGT n’est configurée et si le compte est membre de la **utilisateurs protégés** de groupe, la durée de vie du ticket TGT et renouvellement est de 4 heures. Dans le cas contraire, le renouvellement et durée de vie TGT sont basés sur la stratégie de domaine comme illustré dans la fenêtre Éditeur de gestion de stratégie de groupe suivante pour un domaine avec des paramètres par défaut.  
  
    ![comptes protégés](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_TGTExpiration.png)  
  
5.  Pour restreindre le compte d’utilisateur pour sélectionner des périphériques, cliquez sur **modifier** pour définir les conditions requises pour l’appareil.  
  
    ![comptes protégés](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_EditAuthNPolicy.gif)  
  
6.  Dans le **modifier les Conditions de contrôle d’accès** fenêtre, cliquez sur **ajouter une condition**.  
  
    ![comptes protégés](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_AddCondition.png)  
  
##### <a name="add-computer-account-or-group-conditions"></a>Ajouter des conditions de groupe ou compte d’ordinateur  
  
1.  Pour configurer des comptes d’ordinateurs ou les groupes, dans la liste déroulante, sélectionnez la zone de liste déroulante **membre de chaque** et remplacez **membre de n’importe lequel**.  
  
    ![comptes protégés](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_AddCompMember.png)  
  
    > [!NOTE]  
    > Ce contrôle d’accès définit les conditions de l’appareil ou l’ordinateur hôte à partir duquel l’utilisateur se connecte sur. Dans la terminologie du contrôle d’accès, le compte d’ordinateur pour le périphérique ou l’ordinateur hôte est l’utilisateur, c’est pourquoi **utilisateur** est la seule option.  
  
2.  Cliquez sur **ajouter des éléments**.  
  
    ![comptes protégés](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_AddCompAddItems.png)  
  
3.  Pour modifier les types d’objets, cliquez sur **Types d’objet**.  
  
    ![comptes protégés](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_ChangeObjects.gif)  
  
4.  Pour sélectionner des objets ordinateur dans Active Directory, cliquez sur **ordinateurs**, puis cliquez sur **OK**.  
  
    ![comptes protégés](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_ChangeObjectsComputers.gif)  
  
5.  Tapez le nom des ordinateurs pour restreindre l’utilisateur, puis cliquez sur **vérifier les noms**.  
  
    ![comptes protégés](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_ChangeObjectsCompName.gif)  
  
6.  Cliquez sur OK et créez une autre condition pour le compte d’ordinateur.  
  
    ![comptes protégés](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_AddCompAddConditions.png)  
  
7.  Lorsque vous avez terminé, cliquez sur **OK** et les conditions définies apparaîtront pour le compte d’ordinateur.  
  
    ![comptes protégés](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_AddCompDone.png)  
  
##### <a name="add-computer-claim-conditions"></a>Ajouter des conditions de revendication d’ordinateur  
  
1.  Pour configurer des revendications d’ordinateur, déroulez le groupe pour sélectionner la revendication.  
  
    ![comptes protégés](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_CompClaim.gif)  
  
    Les revendications sont uniquement disponibles si elles sont déjà déployées dans la forêt.  
  
2.  Tapez le nom de l’unité d’organisation, le compte d’utilisateur doit être limité à l’ouverture de session.  
  
    ![comptes protégés](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_CompClaimOUName.gif)  
  
3.  Lorsque vous avez terminé, puis cliquez sur OK et la zone affichera les conditions définies.  
  
    ![comptes protégés](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_CompClaimComplete.gif)  
  
##### <a name="troubleshoot-missing-computer-claims"></a>Résoudre les problèmes de revendications d’ordinateur manquant  
Si la revendication a été configurée, mais n’est pas disponible, il peut être configuré uniquement pour **ordinateur** classes.  
  
Supposons que vous souhaitez restreindre l’authentification basée sur l’unité d’organisation (UO) de l’ordinateur, ce qui a été déjà configurée, mais uniquement **ordinateur** classes.  
  
![comptes protégés](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_RestrictComputers.gif)  
  
Pour que la revendication soit disponible pour restreindre l’authentification à l’appareil, sélectionnez le **utilisateur** case à cocher.  
  
![comptes protégés](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_RestrictUsersComputers.gif)  
  
#### <a name="provision-a-user-account-with-an-authentication-policy-with-adac"></a>Configurer un compte d’utilisateur avec une stratégie d’authentification avec ADAC  
  
1.  À partir de la **utilisateur** compte, cliquez sur **stratégie**.  
  
    ![comptes protégés](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_UserPolicy.gif)  
  
2.  Sélectionnez le **affecter une stratégie d’authentification à ce compte** case à cocher.  
  
    ![comptes protégés](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_UserPolicyAssign.gif)  
  
3.  Sélectionnez ensuite la stratégie d’authentification à appliquer à l’utilisateur.  
  
    ![comptes protégés](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_UserPolicySelect.png)  
  
#### <a name="configure-dynamic-access-control-support-on-devices-and-hosts"></a>Configurer la prise en charge du contrôle d’accès dynamique sur les appareils et ordinateurs hôtes  
Vous pouvez configurer les durées de vie TGT sans configurer le contrôle d’accès dynamique (DAC). DAC est uniquement nécessaire pour la vérification AllowedToAuthenticateFrom et AllowedToAuthenticateTo.  
  
À l’aide de stratégie de groupe ou éditeur de stratégie de groupe locale, activez **prise en charge du client Kerberos des revendications, l’authentification composée et le blindage Kerberos** dans Configuration ordinateur | Modèles d’administration | Système | Kerberos:  
  
![comptes protégés](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_KerbClientDACSupport.gif)  
  
### <a name="BKMK_TroubleshootAuthnPolicies"></a>Résoudre les problèmes de stratégies d’authentification  
  
#### <a name="determine-the-accounts-that-are-directly-assigned-an-authentication-policy"></a>Déterminer les comptes auxquels sont directement affectées une stratégie d’authentification  
La section comptes dans la stratégie d’authentification illustre les comptes qui ont directement appliqué la stratégie.  
  
![comptes protégés](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_AccountsAssigned.gif)  
  
#### <a name="use-the-authentication-policy-failures---domain-controller-administrative-log"></a>Utilisez les échecs de stratégie d’authentification - journal d’administration du contrôleur de domaine  
Un nouveau **échecs de stratégie d’authentification - contrôleur de domaine** journal d’administration sous **journaux des Applications et Services** > **Microsoft** > **Windows** > **authentification** a été créé pour faciliter la détection d’échecs liés aux stratégies d’authentification. Le journal est désactivé par défaut. Pour l’activer, cliquez sur le nom du journal, cliquez sur **activer le journal**. Les nouveaux événements sont très similaires dans le contenu existant TGT Kerberos les événements d’audit du ticket de service. Pour plus d’informations sur ces événements, voir [stratégies d’authentification et Silos de stratégies d’authentification](https://technet.microsoft.com/library/dn486813.aspx).  
  
### <a name="BKMK_ManageAuthnPoliciesUsingPSH"></a>Gérer les stratégies d’authentification à l’aide de Windows PowerShell  
Cette commande crée une stratégie d’authentification nommée **TestAuthenticationPolicy**. Le **UserAllowedToAuthenticateFrom** paramètre spécifie les appareils à partir de laquelle les utilisateurs peuvent s’authentifier par une chaîne SDDL dans le fichier nommé someFile.txt.  
  
```  
PS C:\> New-ADAuthenticationPolicy testAuthenticationPolicy -UserAllowedToAuthenticateFrom (Get-Acl .\someFile.txt).sddl  
```  
  
Cette commande Obtient toutes les stratégies d’authentification qui correspondent au filtre qui le **filtre** paramètre spécifie.  
  
```  
PS C:\> Get-ADAuthenticationPolicy -Filter "Name -like 'testADAuthenticationPolicy*'" -Server Server02.Contoso.com  
  
```  
  
Cette commande modifie la description et la **UserTGTLifetimeMins** propriétés de la stratégie d’authentification spécifiée.  
  
```  
PS C:\> Set-ADAuthenticationPolicy -Identity ADAuthenticationPolicy1 -Description "Description" -UserTGTLifetimeMins 45  
```  
  
Cette commande supprime la stratégie d’authentification qui les **identité** paramètre spécifie.  
  
```  
PS C:\> Remove-ADAuthenticationPolicy -Identity ADAuthenticationPolicy1  
```  
  
Cette commande utilise le **Get-ADAuthenticationPolicy** applet de commande avec le **filtre** paramètre pour obtenir toutes les stratégies d’authentification qui ne sont pas appliquées. Le jeu de résultats est dirigé vers la **Remove-ADAuthenticationPolicy** applet de commande.  
  
```  
PS C:\> Get-ADAuthenticationPolicy -Filter 'Enforce -eq $false' | Remove-ADAuthenticationPolicy  
```  
  
## <a name="BKMK_CreateAuthNPolicySilos"></a>Silos de stratégies d’authentification  
Silos de stratégies d’authentification est un nouveau conteneur (objectClass msDS-AuthNPolicySilos) dans AD DS pour l’utilisateur, ordinateur et les comptes de service. Ils permettent de protéger les comptes à valeur élevée. Toutes les organisations doivent protéger les membres des groupes Administrateurs de l’entreprise, Admins du domaine et administrateurs du schéma, car ces comptes peuvent être utilisés par une personne malveillante pour accéder à n’importe où dans la forêt, autres comptes peuvent également besoin d’une protection.  
  
Certaines organisations isolent les charges de travail en créant des comptes qui leur sont uniques et en appliquant des paramètres de stratégie de groupe pour limiter l’ouverture de session interactive locale et distante et des privilèges d’administration. Silos de stratégies d’authentification complètent ce travail en créant un moyen de définir une relation entre l’utilisateur, d’ordinateur et les comptes de Service gérés. Comptes ne peuvent appartenir qu’à un seul silo. Vous pouvez configurer la stratégie d’authentification pour chaque type de compte afin de contrôler:  
  
1.  Durée de vie TGT non renouvelable  
  
2.  Accéder aux conditions de contrôle pour renvoyer le ticket TGT (Remarque: ne peut pas s’appliquent aux systèmes car le blindage Kerberos est nécessaire)  
  
3.  Conditions de contrôle d’accès pour retourner le ticket de service  
  
En outre, les comptes dans un silo de stratégies d’authentification ont une revendication de silo, qui peut être utilisée par les ressources prenant en charge les revendications telles que les serveurs de fichiers pour contrôler l’accès.  
  
Un nouveau descripteur de sécurité peut être configuré pour contrôler l’émission du ticket de service basée sur:  
  
-   Utilisateur, les groupes de sécurité de l’utilisateur et/ou les revendications de l’utilisateur  
  
-   APPAREIL, groupe de sécurité de l’appareil et/ou les revendications de l’appareil  
  
Obtention de ces informations pour les contrôleurs de domaine de la ressource nécessite le contrôle d’accès dynamique:  
  
-   Revendications d’utilisateur:  
  
    -   Windows 8 et versions ultérieures clients prenant en charge de contrôle d’accès dynamique  
  
    -   Compte de domaine prend en charge les revendications et contrôle d’accès dynamique  
  
-   Appareil et/ou groupe de sécurité de périphérique:  
  
    -   Windows 8 et versions ultérieures clients prenant en charge de contrôle d’accès dynamique  
  
    -   Ressource configurée pour l’authentification composée  
  
-   Revendications de périphérique:  
  
    -   Windows 8 et versions ultérieures clients prenant en charge de contrôle d’accès dynamique  
  
    -   Domaine de l’appareil prend en charge les revendications et contrôle d’accès dynamique  
  
    -   Ressource configurée pour l’authentification composée  
  
Stratégies d’authentification peuvent être appliquées à tous les membres d’un silo plutôt qu’à des comptes individuels, ou les stratégies d’authentification distinctes peuvent être appliquées à différents types de comptes dans un silo. Par exemple, une stratégie d’authentification peut être appliquée aux comptes d’utilisateurs dotés de privilèges élevés et une autre stratégie peut être appliquée aux comptes de services. Au moins une stratégie d’authentification doit être créée avant la création d’un silo de stratégies d’authentification.  
  
> [!NOTE]  
> Une stratégie d’authentification peut être appliquée aux membres d’un silo de stratégies d’authentification, ou elle peut être appliquée indépendamment des silos pour restreindre l’étendue du compte spécifique. Par exemple, pour protéger un compte unique ou un petit ensemble de comptes, une stratégie peut être définie sur ces comptes sans ajouter les comptes à un silo.  
  
Vous pouvez créer un silo de stratégies d’authentification en utilisant le centre d’administration Active Directory ou Windows PowerShell. Par défaut, un silo de stratégies d’authentification uniquement les audits des stratégies de silos, ce qui revient à spécifier le **WhatIf** paramètre dans les applets de commande Windows PowerShell. Dans ce cas, les restrictions du silo de stratégie ne s’appliquent pas, mais les audits sont générés pour indiquer si des échecs se produisent si les restrictions sont appliquées.  
  
#### <a name="to-create-an-authentication-policy-silo-by-using-active-directory-administrative-center"></a>Pour créer un silo de stratégies d’authentification à l’aide du centre d’administration Active Directory  
  
1.  Ouvrez **centre d’administration Active Directory**, cliquez sur **authentification**, avec le bouton droit **Silos de stratégies d’authentification**, cliquez sur **New**, puis cliquez sur **Silo de stratégies d’authentification**.  
  
    ![comptes protégés](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_CreateNewAuthNPolicySilo.gif)  
  
2.  Dans **nom d’affichage**, tapez un nom pour le silo. Dans **comptes autorisés**, cliquez sur **ajouter**, tapez les noms des comptes, puis cliquez sur **OK**. Vous pouvez spécifier les utilisateurs, ordinateurs ou comptes de service. Ensuite, spécifiez si vous souhaitez utiliser une stratégie unique pour tous les principaux ou une stratégie distincte pour chaque type de principal et le nom de la stratégie ou les stratégies.  
  
    ![comptes protégés](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_NewAuthNPolicySiloDisplayName.gif)  
  
### <a name="BKMK_ManageAuthnSilosUsingPSH"></a>Gérer les silos de stratégies d’authentification à l’aide de Windows PowerShell  
Cette commande crée un objet de silo de stratégies d’authentification et l’applique.  
  
```  
PS C:\>New-ADAuthenticationPolicySilo -Name newSilo -Enforce  
```  
  
Cette commande Obtient toutes les authentifications les silos de stratégies qui correspondent au filtre spécifié par le **filtre** paramètre. La sortie est alors transmise à la **Format-Table** applet de commande pour afficher le nom de la stratégie et la valeur de **appliquer** à chaque stratégie.  
  
```  
PS C:\>Get-ADAuthenticationPolicySilo -Filter 'Name -like "*silo*"' | Format-Table Name, Enforce -AutoSize  
  
Name  Enforce  
--  ----  
silo     True  
silos   False  
  
```  
  
Cette commande utilise le **Get-ADAuthenticationPolicySilo** applet de commande avec le **filtre** paramètre pour obtenir tous les silos de stratégies d’authentification qui ne sont pas appliqués et diriger le résultat du filtre vers le **Remove-ADAuthenticationPolicySilo** applet de commande.  
  
```  
PS C:\>Get-ADAuthenticationPolicySilo -Filter 'Enforce -eq $False' | Remove-ADAuthenticationPolicySilo  
```  
  
Cette commande accorde l’accès pour le silo de stratégies d’authentification nommé *Silo* pour le compte d’utilisateur nommé *User01*.  
  
```  
PS C:\>Grant-ADAuthenticationPolicySiloAccess -Identity Silo -Account User01  
```  
  
Cette commande révoque l’accès pour le silo de stratégies d’authentification nommé *Silo* pour le compte d’utilisateur nommé *User01*. Dans la mesure où le **confirmer** paramètre est défini sur **$False**, aucun message de confirmation s’affiche.  
  
```  
PS C:\>Revoke-ADAuthenticationPolicySiloAccess -Identity Silo -Account User01 -Confirm:$False  
```  
  
Cet exemple utilise d’abord le **Get-ADComputer** applet de commande pour obtenir tous les comptes d’ordinateurs qui correspondent au filtre qui le **filtre** paramètre spécifie. La sortie de cette commande est transmise à **Set-ADAccountAuthenticatinPolicySilo** pour affecter le silo de stratégies d’authentification nommé *Silo* et la stratégie d’authentification nommé *AuthenticationPolicy02* à ces derniers.  
  
```  
PS C:\>Get-ADComputer -Filter 'Name -like "newComputer*"' | Set-ADAccountAuthenticationPolicySilo -AuthenticationPolicySilo Silo -AuthenticationPolicy AuthenticationPolicy02  
```  
  

