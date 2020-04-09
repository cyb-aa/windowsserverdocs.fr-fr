---
title: ÉTAPE 5 configurer DC1
description: 'Cette rubrique fait partie du Guide de laboratoire de test : illustrer un déploiement multisite DirectAccess pour Windows Server 2016'
manager: brianlic
ms.prod: windows-server
ms.technology: networking-da
ms.topic: article
ms.assetid: 70357156-fcb0-4346-a61e-4ea963e3ffb0
ms.author: lizross
author: eross-msft
ms.openlocfilehash: d7bf3fff3100f866b16d4932cf9df05d16c50d40
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861572"
---
# <a name="step-5-configure-dc1"></a>ÉTAPE 5 configurer DC1

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

DC1 agit en tant que contrôleur de domaine, serveur DNS et serveur DHCP pour le domaine corp.contoso.com.  
  
Pour configurer l’accès à distance en vue d’utiliser une topologie multisite, vous devez ajouter un site de Active Directory Domain Services (AD DS) supplémentaire pour le deuxième contrôleur de domaine 2-DC1 et configurer le routage entre les sous-réseaux.  
  
1. Pour configurer la passerelle par défaut sur le contrôleur de domaine. Configurez la passerelle par défaut sur DC1.  
  
2. Créez des groupes de sécurité pour les clients DirectAccess Windows 7 sur DC1. Lorsque DirectAccess est configuré, il crée automatiquement des objets de stratégie de groupe (GPO) et des paramètres d’objet de stratégie de groupe qui sont appliqués aux serveurs et clients DirectAccess. L’objet de stratégie de groupe du client DirectAccess est appliqué à des groupes de sécurité Active Directory spécifiques.  
  
3. Pour ajouter un nouveau site de AD DS. Créez un deuxième AD DS site.  
  
## <a name="to-configure-the-default-gateway-on-the-domain-controller"></a>Pour configurer la passerelle par défaut sur le contrôleur de domaine  
  
1.  Dans la console Gestionnaire de serveur, cliquez sur **serveur local**, puis dans la zone **Propriétés** , en regard de **connexion Ethernet câblée**, cliquez sur le lien.  
  
2.  Dans la fenêtre Connexions réseau, cliquez avec le bouton droit sur **connexion Ethernet câblée**, puis cliquez sur **Propriétés**.  
  
3.  Cliquez sur **Protocole Internet version 4 (TCP/IPv4)** , puis sur **Propriétés**.  
  
4.  Dans **passerelle par défaut**, tapez **10.0.0.254**, et dans **serveur DNS auxiliaire**, tapez **10.2.0.1**, puis cliquez sur **OK**.  
  
5.  Cliquez sur **Internet Protocol Version 6 (TCP/IPv6)** , puis cliquez sur **Propriétés**.  
  
6.  Dans **passerelle par défaut**, tapez **2001 : DB8:1 :: Fe**, et dans **serveur DNS auxiliaire**, tapez **2001 : DB8:2 :: 1**, puis cliquez sur **OK**.  
  
7.  Dans la boîte de dialogue **Propriétés de connexion Ethernet câblée** , cliquez sur **Fermer**.  
  
8.  Fermez la fenêtre **Connexions réseau**.  
  
## <a name="create-security-groups-for-windows-7-directaccess-clients-on-dc1"></a>Créer des groupes de sécurité pour les clients DirectAccess Windows 7 sur DC1  
Créez les groupes de sécurité DirectAccess pour Windows 7 à l’aide de la procédure suivante.  
  
 Les ordinateurs clients Windows 7 doivent être membres de groupes de sécurité distincts, car ils sont en mesure de se connecter aux ressources internes par le biais d’un seul point d’entrée. Lors de l’activation de la prise en charge multisite ou de l’ajout de points d’entrée, si la prise en charge de Windows 7 est demandée, un objet de stratégie de groupe distinct est automatiquement créé par DirectAccess pour les clients Windows 7 pour chaque point d’entrée.  
  
### <a name="create-security-groups"></a>Créer des groupes de sécurité  
  
1.  Dans l’écran d' **Accueil** , tapez**DSA. msc**, puis appuyez sur entrée.  
  
2.  Dans le volet gauche, développez **Corp.contoso.com**, cliquez sur **utilisateurs**, cliquez avec le bouton droit sur **utilisateurs**, pointez sur **nouveau**, puis cliquez sur **groupe**.  
  
3.  Dans la boîte de dialogue **nouvel objet-groupe** , sous **nom du groupe**, entrez **Win7_Clients_Site1**.  
  
4.  Sous **Étendue du groupe**, cliquez sur **Globale**. Sous **Type de groupe**, cliquez sur **Sécurité**, puis cliquez sur **OK**.  
  
5.  Double-cliquez sur le groupe de sécurité **Win7_Clients_Site1** , puis dans la boîte de dialogue **Propriétés du Win7_Clients_Site1** , cliquez sur l’onglet **membres** .  
  
6.  Sous l'onglet **Membres**, cliquez sur **Ajouter**.  
  
7.  Dans la boîte de dialogue **Sélectionner les utilisateurs, les contacts, les ordinateurs ou les comptes de service** , cliquez sur **types d’objets**. Dans la boîte de dialogue **types d’objets** , sélectionnez **ordinateurs**, puis cliquez sur **OK**.  
  
8.  Dans **Entrez les noms des objets à sélectionner**, tapez **client2**, puis cliquez sur **OK**, puis dans la boîte de dialogue **Propriétés du Win7_Clients_Site1** , cliquez sur **OK**.  
  
9. Dans la console **utilisateurs et ordinateurs Active Directory** , dans le volet gauche, cliquez avec le bouton droit sur **utilisateurs**, pointez sur **nouveau**, puis cliquez sur **groupe**.  
  
10. Dans la boîte de dialogue **nouvel objet-groupe** , sous **nom du groupe**, entrez **Win7_Clients_Site2**.  
  
11. Sous **Étendue du groupe**, cliquez sur **Globale**. Sous **Type de groupe**, cliquez sur **Sécurité**, puis cliquez sur **OK**.  
  
12. Fermez la console **Utilisateurs et ordinateurs Active Directory**.  
  
## <a name="to-add-a-new-ad-ds-site"></a>Pour ajouter un nouveau site AD DS  
  
1.  Dans l’écran d' **Accueil** , tapez**Dssite. msc**, puis appuyez sur entrée.  
  
2.  Dans la console sites et services Active Directory, dans l’arborescence de la console, cliquez avec le bouton droit sur **sites**, puis cliquez sur **nouveau site**.  
  
3.  Dans la boîte de dialogue **nouvel objet-site** , dans la zone **nom** , tapez **second-site**.  
  
4.  Dans la zone de liste, cliquez sur **DEFAULTIPSITELINK**, puis cliquez deux fois sur **OK** .  
  
5.  Dans l’arborescence de la console, développez **sites**, cliquez avec le bouton droit sur **sous-réseaux**, puis cliquez sur **nouveau sous-réseau**.  
  
6.  Dans la boîte de dialogue **nouvel objet-sous-réseau** , sous **préfixe**, tapez **10.0.0.0/24**dans la liste **Sélectionnez un objet de site pour ce préfixe** , cliquez sur **nom-premier-site-par-défaut**, puis cliquez sur **OK**.  
  
7.  Dans l’arborescence de la console, cliquez avec le bouton droit sur **sous-réseaux**, puis cliquez sur **nouveau sous-réseau**.  
  
8.  Dans la boîte de dialogue **nouvel objet-sous-réseau** , sous **préfixe**, tapez **2001 : DB8:1 ::/64**, dans la liste **Sélectionnez un objet de site pour ce préfixe** , cliquez sur **nom-premier-site-par-défaut**, puis cliquez sur **OK**.  
  
9. Dans l’arborescence de la console, cliquez avec le bouton droit sur **sous-réseaux**, puis cliquez sur **nouveau sous-réseau**.  
  
10. Dans la boîte de dialogue **nouvel objet-sous-réseau** , sous **préfixe**, tapez **10.2.0.0/24**dans la liste **Sélectionnez un objet de site pour ce préfixe** , cliquez sur **second site**, puis cliquez sur **OK**.  
  
11. Dans l’arborescence de la console, cliquez avec le bouton droit sur **sous-réseaux**, puis cliquez sur **nouveau sous-réseau**.  
  
12. Dans la boîte de dialogue **nouvel objet-sous-réseau** , sous **préfixe**, tapez **2001 : DB8:2 ::/64**, dans la liste **Sélectionnez un objet de site pour ce préfixe** , cliquez sur **second-site**, puis cliquez sur **OK**.  
  
13. Fermez Active Directory sites et services.  
  


