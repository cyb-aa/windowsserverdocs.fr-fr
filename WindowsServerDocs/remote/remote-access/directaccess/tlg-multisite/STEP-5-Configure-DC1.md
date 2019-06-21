---
title: ÉTAPE 5 configurer DC1
description: 'Cette rubrique fait partie du Guide de laboratoire de Test : illustrer un déploiement Multisite DirectAccess pour Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 70357156-fcb0-4346-a61e-4ea963e3ffb0
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 108e517923c75f685d817cdf9fad9b14132e3bb0
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281432"
---
# <a name="step-5-configure-dc1"></a>ÉTAPE 5 configurer DC1

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

DC1 joue un contrôleur de domaine, le serveur DNS et le serveur DHCP pour le domaine corp.contoso.com.  
  
Pour configurer l’accès à distance pour utiliser une topologie multisite, il est nécessaire pour ajouter un site supplémentaire de Services de domaine Active Directory (AD DS) pour le deuxième contrôleur 2 domaine-DC1 et configurer le routage entre les sous-réseaux.  
  
1. Pour configurer la passerelle par défaut sur le contrôleur de domaine. Configurer la passerelle par défaut sur DC1.  
  
2. Créer des groupes de sécurité pour les clients DirectAccess dans Windows 7 sur DC1. Lorsque DirectAccess est configuré, il crée automatiquement des objets de stratégie de groupe (GPO) et les paramètres de stratégie de groupe qui sont appliqués aux clients DirectAccess et serveurs. Le client DirectAccess GPO est appliqué à des groupes de sécurité Active Directory spécifiques.  
  
3. Pour ajouter un nouveau site AD DS. Créer un deuxième site AD DS.  
  
## <a name="to-configure-the-default-gateway-on-the-domain-controller"></a>Pour configurer la passerelle par défaut sur le contrôleur de domaine  
  
1.  Dans la console Gestionnaire de serveur, cliquez sur **serveur Local**, puis, dans le **propriétés** zone, en regard **connexion Ethernet câblée**, cliquez sur le lien.  
  
2.  Dans la fenêtre Connexions réseau, cliquez sur **connexion Ethernet câblée**, puis cliquez sur **propriétés**.  
  
3.  Cliquez sur **Protocole Internet version 4 (TCP/IPv4)** , puis sur **Propriétés**.  
  
4.  Dans **passerelle par défaut**, type **10.0.0.254**et dans **serveur DNS auxiliaire**, type **10.2.0.1**, puis cliquez sur **OK** .  
  
5.  Cliquez sur **Internet Protocol Version 6 (TCP/IPv6)** , puis cliquez sur **Propriétés**.  
  
6.  Dans **passerelle par défaut**, type **2001:db8:1::fe**et dans **serveur DNS auxiliaire**, type **2001:db8:2::1**, puis cliquez sur **OK**.  
  
7.  Sur le **propriétés de connexion Ethernet câblée** boîte de dialogue, cliquez sur **fermer**.  
  
8.  Fermez la fenêtre **Connexions réseau**.  
  
## <a name="create-security-groups-for-windows-7-directaccess-clients-on-dc1"></a>Créer des groupes de sécurité pour les clients DirectAccess dans Windows 7 sur DC1  
Créez les groupes de sécurité de DirectAccess pour Windows 7 avec la procédure suivante.  
  
 Les ordinateurs clients Windows 7 doivent être membres de groupes de sécurité distincts, car ils sont en mesure de se connecter aux ressources internes via un point d’entrée unique. Lors de l’activation de prise en charge Multisite ou l’ajout d’entrée de pointe, si la prise en charge de Windows 7 est demandée, puis un GPO est automatiquement créé par les clients DirectAccess pour Windows 7 pour chaque point d’entrée.  
  
### <a name="create-security-groups"></a>Créer des groupes de sécurité  
  
1.  Sur le **Démarrer** , tapez**DSA.msc**, puis appuyez sur ENTRÉE.  
  
2.  Dans le volet gauche, développez **corp.contoso.com**, cliquez sur **utilisateurs**, puis cliquez sur **utilisateurs**, pointez sur **New**, puis cliquez sur **Groupe**.  
  
3.  Sur le **nouvel objet - groupe** boîte de dialogue **nom_groupe**, entrez **Win7_Clients_Site1**.  
  
4.  Sous **Étendue du groupe**, cliquez sur **Globale**. Sous **Type de groupe**, cliquez sur **Sécurité**, puis cliquez sur **OK**.  
  
5.  Double-cliquez sur le **Win7_Clients_Site1** groupe de sécurité, puis, dans le **Win7_Clients_Site1 propriétés** boîte de dialogue, cliquez sur le **membres** onglet.  
  
6.  Sous l'onglet **Membres** , cliquez sur **Ajouter**.  
  
7.  Sur le **sélectionner des utilisateurs, Contacts, ordinateurs ou comptes de Service** boîte de dialogue, cliquez sur **Types d’objets**. Sur le **Types d’objets** boîte de dialogue, sélectionnez **ordinateurs**, puis cliquez sur **OK**.  
  
8.  Dans **Entrez les noms des objets à sélectionner**, type **client2**, puis cliquez sur **OK**, puis, dans le **Win7_Clients_Site1 propriétés** boîte de dialogue Cliquez sur boîte **OK**.  
  
9. Dans le **Active Directory Users and Computers** avec le bouton droit de la console, dans le volet gauche, **utilisateurs**, pointez sur **New**, puis cliquez sur **groupe** .  
  
10. Sur le **nouvel objet - groupe** boîte de dialogue **nom_groupe**, entrez **Win7_Clients_Site2**.  
  
11. Sous **Étendue du groupe**, cliquez sur **Globale**. Sous **Type de groupe**, cliquez sur **Sécurité**, puis cliquez sur **OK**.  
  
12. Fermez la console **Utilisateurs et ordinateurs Active Directory** .  
  
## <a name="to-add-a-new-ad-ds-site"></a>Pour ajouter un nouveau site AD DS  
  
1.  Sur le **Démarrer** , tapez**dssite.msc**, puis appuyez sur ENTRÉE.  
  
2.  Dans la console Services et Sites Active Directory, dans l’arborescence de la console, cliquez sur **Sites**, puis cliquez sur **nouveau Site**.  
  
3.  Sur le **nouvel objet - Site** boîte de dialogue le **nom** , tapez **seconde-Site**.  
  
4.  Dans la zone de liste, cliquez sur **DEFAULTIPSITELINK**, puis cliquez sur **OK** à deux reprises.  
  
5.  Dans l’arborescence de la console, développez **Sites**, avec le bouton droit **sous-réseaux**, puis cliquez sur **nouveau sous-réseau**.  
  
6.  Sur le **nouvel objet - sous-réseau** boîte de dialogue **préfixe**, type **10.0.0.0/24**, dans le **sélectionnez un objet de site pour ce préfixe** , cliquez sur **Default-First-Site-Name**, puis cliquez sur **OK**.  
  
7.  Dans l’arborescence de la console, cliquez sur **sous-réseaux**, puis cliquez sur **nouveau sous-réseau**.  
  
8.  Sur le **nouvel objet - sous-réseau** boîte de dialogue **préfixe**, type **2001:db8:1 :: / 64**, dans le **sélectionnez un objet de site pour ce préfixe** liste, Cliquez sur **Default-First-Site-Name**, puis cliquez sur **OK**.  
  
9. Dans l’arborescence de la console, cliquez sur **sous-réseaux**, puis cliquez sur **nouveau sous-réseau**.  
  
10. Sur le **nouvel objet - sous-réseau** boîte de dialogue **préfixe**, type **10.2.0.0/24**, dans le **sélectionnez un objet de site pour ce préfixe** , cliquez sur **Seconde intersites**, puis cliquez sur **OK**.  
  
11. Dans l’arborescence de la console, cliquez sur **sous-réseaux**, puis cliquez sur **nouveau sous-réseau**.  
  
12. Sur le **nouvel objet - sous-réseau** boîte de dialogue **préfixe**, type **2001:db8:2 :: / 64**, dans le **sélectionnez un objet de site pour ce préfixe** liste, Cliquez sur **seconde intersites**, puis cliquez sur **OK**.  
  
13. Fermez les Sites et Services Active Directory.  
  


