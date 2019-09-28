---
title: 'ÉTAPE 6 : installer et configurer 2-DC1'
description: 'Cette rubrique fait partie du Guide de laboratoire de test : illustrer un déploiement multisite DirectAccess pour Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3d66901a-c40b-474c-9948-f989f399cfea
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4c7a8243922f58f9705a85cd30b2a68cf4d876c6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404773"
---
# <a name="step-6-install-and-configure-2-dc1"></a>ÉTAPE 6 : installer et configurer 2-DC1

>S'applique à : Windows Server (Canal semi-annuel), Windows Server 2016

2-DC1 fournit les services suivants :  
  
-   Un contrôleur de domaine pour le domaine corp2.corp.contoso.com Active Directory Domain Services (AD DS).  
  
-   Un serveur DNS pour le domaine DNS corp2.corp.contoso.com.  
  
la configuration 2-DC1 comprend les éléments suivants :  
  
- Installer le système d’exploitation sur 2-DC1
  
- Configurer les propriétés TCP/IP

- Configurer 2-DC1 en tant que contrôleur de domaine et serveur DNS

- Fournir stratégie de groupe autorisations à Corp\user1.
  
- Autoriser les ordinateurs CORP2 à obtenir des certificats d’ordinateur
  
- Forcer la réplication entre DC1 et 2-DC1
  
## <a name="install-the-operating-system-on-2-dc1"></a>Installer le système d’exploitation sur 2-DC1  
Tout d’abord, installez Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012.  
  
### <a name="to-install-the-operating-system-on-2-dc1"></a>Pour installer le système d’exploitation sur 2-DC1  
  
1.  Démarrez l’installation de Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012.  
  
2.  Suivez les instructions pour terminer l’installation, en spécifiant Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012 (installation complète) et un mot de passe fort pour le compte administrateur local. Ouvrez une session à l’aide du compte Administrateur local.  
  
3.  Connectez 2-DC1 à un réseau disposant d’un accès Internet et exécutez Windows Update pour installer les dernières mises à jour pour Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012, puis vous déconnecter d’Internet.  
  
4.  Connectez 2-DC1 au sous-réseau 2-Corpnet.  
  
## <a name="configure-tcpip-properties"></a>Configurer les propriétés TCP/IP  
Configurez le protocole TCP/IP avec des adresses IP statiques.  
  
### <a name="to-configure-tcpip-on-2-dc1"></a>Pour configurer TCP/IP sur 2-DC1  
  
1.  Dans la console Gestionnaire de serveur, cliquez sur **serveur local**, puis dans la zone **Propriétés** , en regard de **connexion Ethernet câblée**, cliquez sur le lien.  
  
2.  Dans **Connexions réseau**, cliquez avec le bouton droit sur **Connexion Ethernet câblée** et cliquez sur **Propriétés**.  
  
3.  Cliquez sur **Protocole Internet version 4 (TCP/IPv4)** , puis sur **Propriétés**.  
  
4.  Cliquez sur **Utiliser l’adresse IP suivante**. Dans **adresse IP**, tapez **10.2.0.1**. Dans la zone **Masque de sous-réseau**, tapez **255.255.255.0**. Dans **passerelle par défaut**, tapez **10.2.0.254**. Cliquez sur **utiliser l’adresse de serveur DNS suivante**, dans **serveur DNS préféré**, tapez **10.2.0.1**, puis dans **serveur DNS auxiliaire**, tapez **10.0.0.1**.  
  
5.  Cliquez sur **Avancé**, puis sur l’onglet **DNS**.  
  
6.  Dans **suffixe DNS pour cette connexion**, tapez **corp2.Corp.contoso.com**, puis cliquez deux fois sur **OK** .  
  
7.  Cliquez sur **Internet Protocol Version 6 (TCP/IPv6)** , puis cliquez sur **Propriétés**.  
  
8.  Cliquez sur **utiliser l’adresse IPv6 suivante**. Dans **adresse IPv6**, tapez **2001 : DB8:2 :: 1**. Dans **longueur du préfixe du sous-réseau**, tapez **64**. Dans **passerelle par défaut**, tapez **2001 : DB8:2 :: Fe**. Cliquez sur **utiliser l’adresse de serveur DNS suivante**, dans **serveur DNS préféré**, tapez **2001 : DB8:2 :: 1**, et dans **serveur DNS auxiliaire**, tapez **2001 : DB8:1 :: 1**.  
  
9. Cliquez sur **Avancé**, puis sur l’onglet **DNS**.  
  
10. Dans **suffixe DNS pour cette connexion**, tapez **corp2.Corp.contoso.com**, puis cliquez deux fois sur **OK** .  
  
11. Dans la boîte de dialogue **Propriétés de connexion Ethernet câblée** , cliquez sur **Fermer**.  
  
12. Fermez la fenêtre **Connexions réseau**.  
  
13. Dans la console Gestionnaire de serveur, dans **serveur local**, dans la zone **Propriétés** , en regard de nom de l' **ordinateur**, cliquez sur le lien.  
  
14. Dans la boîte de dialogue **Propriétés système**, sous l'onglet **Nom de l'ordinateur**, cliquez sur **Modifier**.  
  
15. Dans la boîte de dialogue **modification du nom ou du domaine** de l’ordinateur, dans nom de l' **ordinateur**, tapez **2-DC1**, puis cliquez sur **OK**.  
  
16. Lorsque vous êtes invité à redémarrer l’ordinateur, cliquez sur **OK**.  
  
17. Dans la boîte de dialogue **Propriétés système**, cliquez sur **Fermer**.  
  
18. Lorsque vous êtes invité à redémarrer l’ordinateur, cliquez sur **Redémarrer maintenant**.  
  
19. Après le redémarrage, connectez-vous à l’aide du compte d’administrateur local.  
  
## <a name="configure-2-dc1-as-a-domain-controller-and-dns-server"></a>Configurer 2-DC1 en tant que contrôleur de domaine et serveur DNS  
Configurez 2-DC1 en tant que contrôleur de domaine pour le domaine corp2.corp.contoso.com et en tant que serveur DNS pour le domaine DNS corp2.corp.contoso.com.  
  
### <a name="to-configure-2-dc1-as-a-domain-controller-and-dns-server"></a>Pour configurer 2-DC1 en tant que contrôleur de domaine et serveur DNS  
  
1.  Dans la console Gestionnaire de serveur, dans le **tableau de bord**, cliquez sur **Ajouter des rôles et des fonctionnalités**.  
  
2.  Cliquez sur **suivant** trois fois pour accéder à l’écran de sélection du rôle de serveur  
  
3.  Dans la page **Sélectionner des rôles de serveurs** , sélectionnez **Active Directory Domain Services**. Cliquez sur **Ajouter des fonctionnalités** lorsque vous y êtes invité, puis cliquez sur **suivant** trois fois.  
  
4.  Dans la page **Confirmer les sélections d’installation** , cliquez sur **Installer**.  
  
5.  Une fois l’installation terminée, cliquez sur **promouvoir ce serveur en contrôleur de domaine**.  
  
6.  Dans l’Assistant Configuration de Active Directory Domain Services, dans la page **configuration du déploiement** , cliquez sur **Ajouter un nouveau domaine à une forêt existante**.  
  
7.  Dans **nom du domaine parent**, tapez **Corp.contoso.com**, dans **nouveau nom de domaine**, tapez **corp2**.  
  
8.  Sous **fournir les informations d’identification pour effectuer cette opération**, cliquez sur **modifier**. Dans la boîte de dialogue **sécurité Windows** , **dans nom d’utilisateur**, tapez **Corp. contoso. Com\Administrator**et dans **mot de passe**, entrez le mot de passe Corp\administrateur., cliquez sur **OK**, puis sur **suivant**.  
  
9. Dans la page **Options du contrôleur de domaine** , assurez-vous que le nom du **site** est **second-site**. Sous **tapez le mot de passe du mode de restauration des services d’annuaire (DSRM)** , dans **mot** de passe et **confirmer le mot de passe**, tapez un mot de passe fort à deux reprises, puis cliquez sur **suivant** cinq fois.  
  
10. Dans la page vérification de la **Configuration requise** , une fois les conditions préalables validées, cliquez sur **installer**.  
  
11. Attendez que l’Assistant termine la configuration des services Active Directory et DNS, puis cliquez sur **Fermer**.  
  
12. Une fois l’ordinateur redémarré, connectez-vous au domaine CORP2 à l’aide du compte administrateur.  
  
## <a name="provide-group-policy-permissions-to-corpuser1"></a>Fournir stratégie de groupe autorisations à Corp\user1.  
Utilisez cette procédure pour fournir à l’utilisateur Corp\user1. des autorisations complètes pour créer et modifier des objets corp2 stratégie de groupe.  
  
### <a name="to-provide-group-policy-permissions"></a>Pour fournir des autorisations de stratégie de groupe  
  
1.  Dans l’écran **Démarrer** , tapez**GPMC. msc**, puis appuyez sur entrée.  
  
2.  Dans la console de gestion stratégie de groupe, ouvrez la **forêt : Corp.contoso.com/domains/corp2.Corp.contoso.com**.  
  
3.  Dans le volet d’informations, cliquez sur l’onglet **délégation** . Dans la liste déroulante **autorisation** , cliquez sur **lier les objets de stratégie de groupe**.  
  
4.  Cliquez sur **Ajouter**, puis dans la boîte de dialogue Nouveau **Sélectionner un utilisateur, un ordinateur ou un groupe** , cliquez sur **emplacements**.  
  
5.  Dans la boîte de dialogue **emplacements** , dans l’arborescence **emplacement** , cliquez sur **Corp.contoso.com**, puis sur **OK**.  
  
6.  Dans **Entrez le nom de l’objet pour sélectionner** type **User1**, cliquez sur **OK**, puis dans la boîte de dialogue **Ajouter un groupe ou un utilisateur** , cliquez sur **OK**.  
  
7.  Dans l’arborescence de la console de gestion de stratégie de groupe, cliquez sur **stratégie de groupe objets**, puis dans le volet d’informations, cliquez sur l’onglet **délégation** .  
  
8.  Cliquez sur **Ajouter**, puis dans la boîte de dialogue Nouveau **Sélectionner un utilisateur, un ordinateur ou un groupe** , cliquez sur **emplacements**.  
  
9. Dans la boîte de dialogue **emplacements** , dans l’arborescence **emplacement** , cliquez sur **Corp.contoso.com**, puis sur **OK**.  
  
10. Dans **Entrez le nom de l’objet pour sélectionner le** type **utilisateur1**, cliquez sur **OK**.  
  
11. Dans l’arborescence de la console de gestion de stratégie de groupe, cliquez sur **filtres WMI**, puis dans le volet d’informations, cliquez sur l’onglet **délégation** .  
  
12. Cliquez sur **Ajouter**, puis dans la boîte de dialogue Nouveau **Sélectionner un utilisateur, un ordinateur ou un groupe** , cliquez sur **emplacements**.  
  
13. Dans la boîte de dialogue **emplacements** , dans l’arborescence **emplacement** , cliquez sur **Corp.contoso.com**, puis sur **OK**.  
  
14. Dans **Entrez le nom de l’objet pour sélectionner le** type **utilisateur1**, cliquez sur **OK**. Dans la boîte de dialogue **Ajouter un groupe ou un utilisateur** , assurez-vous que les **autorisations** sont définies sur **contrôle total**, puis cliquez sur **OK**.  
  
15. Fermez la console de gestion des stratégies de groupe.  
  
## <a name="allow-corp2-computers-to-obtain-computer-certificates"></a>Autoriser les ordinateurs CORP2 à obtenir des certificats d’ordinateur 

Les ordinateurs du domaine CORP2 doivent obtenir des certificats d’ordinateur auprès de l’autorité de certification sur APP1. Effectuez cette procédure sur APP1.  
  
### <a name="to-allow-corp2-computers-to-automatically-obtain-computer-certificates"></a>Pour permettre aux ordinateurs CORP2 d’obtenir automatiquement des certificats d’ordinateur  
  
1.  Sur APP1, cliquez sur **Démarrer**, tapez **certtmpl. msc**, puis appuyez sur entrée.  
  
2.  Dans la **console du modèle de certificats**, dans le volet central, double-cliquez sur **authentification client-serveur**.  
  
3.  Dans la boîte de dialogue Propriétés de l' **authentification client-serveur** , cliquez sur l’onglet **sécurité** .  
  
4.  Cliquez sur **Ajouter**, puis dans la boîte de dialogue **Sélectionner les utilisateurs, les ordinateurs, les comptes de service ou les groupes** , cliquez sur **emplacements**.  
  
5.  Dans la boîte de dialogue **emplacements** , dans **emplacement**, développez **Corp.contoso.com**, cliquez sur **corp2.Corp.contoso.com**, puis sur **OK**.  
  
6.  Dans **Entrez les noms des objets à sélectionner**, tapez **Admins du domaine ; Ordinateurs du domaine** , puis cliquez sur **OK**.  
  
7.  Dans la boîte de dialogue Propriétés de l' **authentification client-serveur** , dans **noms de groupes ou d’utilisateurs**, cliquez sur **Admins du domaine (CORP2\Domain admins)** , et dans **autorisations pour Admins du domaine**, dans la colonne **autoriser** , sélectionnez **écrire** . et **Inscrivez**-vous.  
  
8.  Dans **noms de groupes ou d’utilisateurs**, cliquez sur **ordinateurs du domaine (ordinateurs CORP2\Domain)** , et dans **autorisations pour ordinateurs du domaine**, dans la colonne **autoriser** , sélectionnez **inscrire** et **inscription**automatique, puis cliquez sur **OK**.  
  
9. Fermez la Console Modèles de certificat.  
  
## <a name="replication"></a>Forcer la réplication entre DC1 et 2-DC1  
Avant de pouvoir vous inscrire aux certificats sur 2 EDGE1, vous devez forcer la réplication des paramètres de DC1 à 2-DC1. Cette opération doit être effectuée sur DC1.  
  
### <a name="to-force-replication"></a>Pour forcer la réplication  
  
1.  Sur DC1, cliquez sur **Démarrer**, puis sur **Sites et services Active Directory**.  
  
2.  Dans l’arborescence de la console sites et services Active Directory, développez **Transports inter-sites**, puis cliquez sur **IP**.  
  
3.  Dans le volet d’informations, double-cliquez sur **DEFAULTIPSITELINK**.  
  
4.  Dans la boîte de dialogue **Propriétés de DEFAULTIPSITELINK** , dans **coût**, tapez **1**, dans **répliquer toutes**les, tapez **15**, puis cliquez sur **OK**. Attendez 15 minutes que la réplication se termine.  
  
5.  Pour forcer la réplication maintenant dans l’arborescence de la console, développez **paramètres Sites\Default-First-Site-name\Servers\DC1\NTDS**, dans le volet d’informations, cliquez avec le bouton droit sur **<automatically generated>** , cliquez sur **répliquer maintenant**, puis dans la boîte de dialogue **répliquer maintenant** , Cliquez sur **OK**.  
  
6.  Pour garantir la réussite de la réplication, procédez comme suit :  
  
    1.  Dans l’écran d' **Accueil** , tapez**cmd. exe**, puis appuyez sur entrée.  
  
    2.  Tapez la commande suivante et appuyez sur ENTRÉE.  
  
        ```  
        repadmin /syncall /e /A /P /d /q  
        ```  
  
    3.  Assurez-vous que toutes les partitions sont synchronisées sans erreurs. Si ce n’est pas le cas, réexécutez la commande jusqu’à ce qu’aucune erreur ne soit signalée avant de continuer.  
  
7.  Fermez la fenêtre d’invite de commandes.  
  


