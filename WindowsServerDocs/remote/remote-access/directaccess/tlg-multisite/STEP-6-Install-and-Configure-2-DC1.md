---
title: ÉTAPE 6 d’installer et configurer 2-DC1
description: 'Cette rubrique fait partie du Guide de laboratoire de Test : illustrer un déploiement Multisite DirectAccess pour Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3d66901a-c40b-474c-9948-f989f399cfea
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4f31c0e1d36ff458fb4807ab6856a56498f449ab
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838180"
---
# <a name="step-6-install-and-configure-2-dc1"></a>ÉTAPE 6 d’installer et configurer 2-DC1

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

2-DC1 fournit les services suivants :  
  
-   Un contrôleur de domaine pour le domaine de Services de domaine Active Directory (AD DS) corp2.corp.contoso.com.  
  
-   Un serveur DNS pour le domaine DNS corp2.corp.contoso.com.  
  
2-DC1 configuration se compose des éléments suivants :  
  
- Installer le système d’exploitation sur 2-DC1
  
- Configurer les propriétés TCP/IP

- Configurer 2-DC1 comme contrôleur de domaine et serveur DNS

- Fournir des autorisations de stratégie de groupe à CORP\User1
  
- Autoriser les ordinateurs CORP2 obtenir des certificats d’ordinateur
  
- Forcer la réplication entre DC1 et 2-DC1
  
## <a name="install-the-operating-system-on-2-dc1"></a>Installer le système d’exploitation sur 2-DC1  
Tout d’abord, installez Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012.  
  
### <a name="to-install-the-operating-system-on-2-dc1"></a>Pour installer le système d’exploitation sur 2-DC1  
  
1.  Démarrez l’installation de Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012.  
  
2.  Suivez les instructions pour terminer l’installation, en spécifiant Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012 (installation complète) et un mot de passe fort pour le compte d’administrateur local. Ouvrez une session à l’aide du compte Administrateur local.  
  
3.  Connecter 2-DC1 à un réseau qui a accès à Internet et exécutez la mise à jour de Windows pour installer les dernières mises à jour pour Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012 et vous déconnecter puis à partir d’Internet.  
  
4.  Se connecter à 2-DC1 au sous-réseau Corpnet de 2.  
  
## <a name="configure-tcpip-properties"></a>Configurer les propriétés TCP/IP  
Configurer le protocole TCP/IP avec des adresses IP statiques.  
  
### <a name="to-configure-tcpip-on-2-dc1"></a>Pour configurer TCP/IP sur DC1-2  
  
1.  Dans la console Gestionnaire de serveur, cliquez sur **serveur Local**, puis, dans le **propriétés** zone, en regard **connexion Ethernet câblée**, cliquez sur le lien.  
  
2.  Dans **Connexions réseau**, cliquez avec le bouton droit sur **Connexion Ethernet câblée** et cliquez sur **Propriétés**.  
  
3.  Cliquez sur **Protocole Internet version 4 (TCP/IPv4)**, puis sur **Propriétés**.  
  
4.  Cliquez sur **Utiliser l’adresse IP suivante**. Dans **adresse IP**, type **10.2.0.1**. Dans la zone **Masque de sous-réseau**, tapez **255.255.255.0**. Dans **passerelle par défaut**, type **10.2.0.254**. Cliquez sur **utiliser l’adresse de serveur DNS suivante**, dans **serveur DNS préféré**, type **10.2.0.1**et dans **serveur DNS auxiliaire**, type **10.0.0.1**.  
  
5.  Cliquez sur **Avancé**, puis sur l’onglet **DNS**.  
  
6.  Dans **suffixe DNS pour cette connexion**, type **corp2.corp.contoso.com**, puis cliquez sur **OK** à deux reprises.  
  
7.  Cliquez sur **Internet Protocol Version 6 (TCP/IPv6)**, puis cliquez sur **Propriétés**.  
  
8.  Cliquez sur **utiliser l’adresse IPv6 suivante**. Dans **adresse IPv6**, type **2001:db8:2::1**. Dans **longueur de préfixe de sous-réseau**, type **64**. Dans **passerelle par défaut**, type **2001:db8:2::fe**. Cliquez sur **utiliser l’adresse de serveur DNS suivante**, dans **serveur DNS préféré**, type **2001:db8:2::1**et dans **serveur DNS auxiliaire**, type **2001:db8:1::1**.  
  
9. Cliquez sur **Avancé**, puis sur l’onglet **DNS**.  
  
10. Dans **suffixe DNS pour cette connexion**, type **corp2.corp.contoso.com**, puis cliquez sur **OK** à deux reprises.  
  
11. Sur le **propriétés de connexion Ethernet câblée** boîte de dialogue, cliquez sur **fermer**.  
  
12. Fermez la fenêtre **Connexions réseau**.  
  
13. Dans la console Gestionnaire de serveur, dans **serveur Local**, dans le **propriétés** zone, en regard **nom de l’ordinateur**, cliquez sur le lien.  
  
14. Dans la boîte de dialogue **Propriétés système**, sous l'onglet **Nom de l'ordinateur**, cliquez sur **Modifier**.  
  
15. Sur le **modification du nom ou du domaine d’ordinateur** boîte de dialogue **nom de l’ordinateur**, type **2-DC1**, puis cliquez sur **OK**.  
  
16. Lorsque vous êtes invité à redémarrer l’ordinateur, cliquez sur **OK**.  
  
17. Dans la boîte de dialogue **Propriétés système**, cliquez sur **Fermer**.  
  
18. Lorsque vous êtes invité à redémarrer l’ordinateur, cliquez sur **Redémarrer maintenant**.  
  
19. Après avoir redémarré, connectez-vous à l’aide de compte d’administrateur local.  
  
## <a name="configure-2-dc1-as-a-domain-controller-and-dns-server"></a>Configurer 2-DC1 comme contrôleur de domaine et serveur DNS  
Configurer 2-DC1 comme contrôleur de domaine pour le domaine corp2.corp.contoso.com et comme un serveur DNS pour le domaine DNS corp2.corp.contoso.com.  
  
### <a name="to-configure-2-dc1-as-a-domain-controller-and-dns-server"></a>Pour configurer 2-DC1 comme contrôleur de domaine et serveur DNS  
  
1.  Dans la console Gestionnaire de serveur, sur le **tableau de bord**, cliquez sur **ajouter des rôles et fonctionnalités**.  
  
2.  Cliquez sur **suivant** trois fois pour accéder à l’écran de sélection de rôle de serveur  
  
3.  Sur le **sélectionner des rôles de serveur** , sélectionnez **Active Directory Domain Services**. Cliquez sur **ajouter des fonctionnalités** lorsque vous y êtes invité, puis cliquez sur **suivant** trois fois.  
  
4.  Dans la page **Confirmer les sélections d’installation** , cliquez sur **Installer**.  
  
5.  Une fois l’installation terminée, cliquez sur **promouvoir ce serveur en contrôleur de domaine**.  
  
6.  Dans l’Assistant de Configuration Active Directory domaine Services, sur le **Configuration de déploiement** , cliquez sur **ajouter un nouveau domaine à une forêt existante**.  
  
7.  Dans **nom du domaine Parent**, type **corp.contoso.com**, dans **nouveau nom de domaine**, type **corp2**.  
  
8.  Sous **fournir les informations d’identification pour effectuer cette opération**, cliquez sur **modification**. Sur le **Windows Security** boîte de dialogue **nom d’utilisateur**, type **corp.contoso.com\Administrator**et dans **mot de passe**, entrez le corp\ Mot de passe administrateur, cliquez sur **OK**, puis cliquez sur **suivant**.  
  
9. Sur le **Options du contrôleur de domaine** page, assurez-vous que le **nom du Site** est **seconde-Site**. Sous **tapez le mot de passe du Mode de restauration des Services annuaire (DSRM)**, dans **mot de passe** et **confirmer le mot de passe**, tapez un mot de passe fort à deux reprises, puis cliquez sur  **Suivant** cinq fois.  
  
10. Sur le **vérification des prérequis** page, une fois les conditions préalables sont validés, cliquez sur **installer**.  
  
11. Attendez que l’Assistant a terminé la configuration des services Active Directory et DNS, puis cliquez sur **fermer**.  
  
12. Une fois l’ordinateur redémarré, connectez-vous au domaine de CORP2 en utilisant le compte d’administrateur.  
  
## <a name="provide-group-policy-permissions-to-corpuser1"></a>Fournir des autorisations de stratégie de groupe à CORP\User1  
Cette procédure permet de fournir à l’utilisateur CORP\User1 avec toutes les autorisations pour créer et modifier des objets de stratégie de groupe corp2.  
  
### <a name="to-provide-group-policy-permissions"></a>Pour fournir des autorisations de stratégie de groupe  
  
1.  Sur le **Démarrer** , tapez**gpmc.msc**, puis appuyez sur ENTRÉE.  
  
2.  Dans la console de gestion de stratégie de groupe, ouvrez **forêt : corp.contoso.com/Domains/corp2.corp.contoso.com**.  
  
3.  Dans le volet détails, cliquez sur le **délégation** onglet. Dans le **autorisation** la liste déroulante, cliquez sur **lier les objets GPO**.  
  
4.  Cliquez sur **ajouter**et sur le nouveau **sélectionner un utilisateur, ordinateur ou groupe** boîte de dialogue, cliquez sur **emplacements**.  
  
5.  Sur le **emplacements** boîte de dialogue le **emplacement** arborescence, cliquez sur **corp.contoso.com**, puis cliquez sur **OK**.  
  
6.  Dans **Entrez le nom de l’objet à sélectionner** type **User1**, cliquez sur **OK**, puis, dans le **ajouter un groupe ou utilisateur** boîte de dialogue, cliquez sur **OK** .  
  
7.  Dans la console de gestion de stratégie de groupe, dans l’arborescence, cliquez sur **les objets de stratégie de groupe**, dans les détails du volet sur le **délégation** onglet.  
  
8.  Cliquez sur **ajouter**et sur le nouveau **sélectionner un utilisateur, ordinateur ou groupe** boîte de dialogue, cliquez sur **emplacements**.  
  
9. Sur le **emplacements** boîte de dialogue le **emplacement** arborescence, cliquez sur **corp.contoso.com**, puis cliquez sur **OK**.  
  
10. Dans **Entrez le nom de l’objet à sélectionner** type **User1**, cliquez sur **OK**.  
  
11. Dans la console de gestion de stratégie de groupe, dans l’arborescence, cliquez sur **filtres WMI**, dans les détails du volet sur le **délégation** onglet.  
  
12. Cliquez sur **ajouter**et sur le nouveau **sélectionner un utilisateur, ordinateur ou groupe** boîte de dialogue, cliquez sur **emplacements**.  
  
13. Sur le **emplacements** boîte de dialogue le **emplacement** arborescence, cliquez sur **corp.contoso.com**, puis cliquez sur **OK**.  
  
14. Dans **Entrez le nom de l’objet à sélectionner** type **User1**, cliquez sur **OK**. Sur le **ajouter un groupe ou utilisateur** boîte de dialogue zone, assurez-vous que l’option **autorisations** sont définies sur **contrôle total**, puis cliquez sur **OK**.  
  
15. Fermez la console de gestion des stratégies de groupe.  
  
## <a name="allow-corp2-computers-to-obtain-computer-certificates"></a>Autoriser les ordinateurs CORP2 obtenir des certificats d’ordinateur 

Les ordinateurs dans le domaine CORP2 doivent obtenir des certificats d’ordinateur à partir de l’autorité de certification sur APP1. Effectuez cette procédure sur APP1.  
  
### <a name="to-allow-corp2-computers-to-automatically-obtain-computer-certificates"></a>Pour permettre aux ordinateurs CORP2 obtenir automatiquement les certificats d’ordinateur  
  
1.  Sur APP1, cliquez sur **Démarrer**, type **certtmpl.msc**, puis appuyez sur ENTRÉE.  
  
2.  Dans le **Console Modèles de certificats**, dans le volet central, double-cliquez sur **Client-Server Authentication**.  
  
3.  Sur le **propriétés d’authentification Client-serveur** boîte de dialogue, cliquez sur le **sécurité** onglet.  
  
4.  Cliquez sur **ajouter**, puis, dans le **sélectionner des utilisateurs, les ordinateurs, les comptes de Service ou les groupes** boîte de dialogue, cliquez sur **emplacements**.  
  
5.  Sur le **emplacements** boîte de dialogue **emplacement**, développez **corp.contoso.com**, cliquez sur **corp2.corp.contoso.com**, puis cliquez sur  **OK**.  
  
6.  Dans **Entrez les noms des objets à sélectionner**, type **Admins du domaine ; Ordinateurs du domaine** puis cliquez sur **OK**.  
  
7.  Sur le **propriétés d’authentification Client-serveur** boîte de dialogue **noms d’utilisateur ou groupe**, cliquez sur **Admins du domaine (CORP2\Domain du)** et dans  **Autorisations pour les administrateurs de domaine**, dans le **autoriser** colonne, sélectionnez **écrire** et **inscription**.  
  
8.  Dans **noms d’utilisateur ou groupe**, cliquez sur **domaine ordinateurs (CORP2\Domain)** et dans **autorisations pour les ordinateurs du domaine**, dans le **autoriser**colonne, sélectionnez **inscription** et **inscription automatique**, puis cliquez sur **OK**.  
  
9. Fermez la Console Modèles de certificat.  
  
## <a name="replication"></a>Forcer la réplication entre DC1 et 2-DC1  
Avant de pouvoir inscrire des certificats sur EDGE1-2, vous devez forcer la réplication des paramètres de DC1 à 2-DC1. Cette opération doit être effectuée sur DC1.  
  
### <a name="to-force-replication"></a>Pour forcer la réplication  
  
1.  Sur DC1, cliquez sur **Démarrer**, puis cliquez sur **Sites Active Directory et Services**.  
  
2.  Dans la console Services et Sites Active Directory, dans l’arborescence, développez **Transports inter-sites**, puis cliquez sur **IP**.  
  
3.  Dans le volet détails, double-cliquez sur **DEFAULTIPSITELINK**.  
  
4.  Sur le **DEFAULTIPSITELINK propriétés** boîte de dialogue **coût**, type **1**, dans **répliquer chaque**, type **15**, puis cliquez sur **OK**. Attendez 15 minutes pour la réplication soit terminée.  
  
5.  Pour forcer la réplication maintenant dans l’arborescence de la console, développez **sites\site-Premier-Site-name\Servers\DC1\NTDS paramètres**, dans le volet détails, cliquez sur **<automatically generated>**, cliquez sur  **Répliquer maintenant**, puis, dans le **Répliquer maintenant** boîte de dialogue, cliquez sur **OK**.  
  
6.  Pour garantir la réplication est terminée avec succès procédez comme suit :  
  
    1.  Sur le **Démarrer** , tapez**cmd.exe**, puis appuyez sur ENTRÉE.  
  
    2.  Tapez la commande suivante et appuyez sur ENTRÉE.  
  
        ```  
        repadmin /syncall /e /A /P /d /q  
        ```  
  
    3.  Assurez-vous que toutes les partitions sont synchronisées sans erreurs. Si ce n’est pas le cas, puis réexécutez la commande jusqu'à ce qu’aucune erreur n’est signalés avant de continuer.  
  
7.  Fermez la fenêtre d’invite de commandes.  
  


