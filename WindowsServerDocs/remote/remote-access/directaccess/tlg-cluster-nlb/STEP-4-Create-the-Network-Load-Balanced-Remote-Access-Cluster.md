---
title: ÉTAPE 4 créer le cluster d’accès à distance avec équilibrage de la charge réseau
description: Cette rubrique fait partie du Guide de laboratoire de test-démonstration de DirectAccess dans un cluster avec Windows NLB pour Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 509eaa08-c49d-448d-a71e-c1c45519ccd5
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f888ebadfaa91b35f0924b23e9818da1c32f26e5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388482"
---
# <a name="step-4-create-the-network-load-balanced-remote-access-cluster"></a>ÉTAPE 4 créer le cluster d’accès à distance avec équilibrage de la charge réseau

>S'applique à : Windows Server (Canal semi-annuel), Windows Server 2016

 Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012 vous permettent de créer des clusters de serveurs d’accès à distance. Un cluster agit comme un serveur logique unique et fournit une configuration et une gestion centralisées pour les serveurs du cluster. Lorsque vous utilisez l’équilibrage de la charge réseau (NLB), vous pouvez prendre en charge jusqu’à 8 membres d’accès à distance dans un seul cluster. Les clusters d’accès à distance offrent une haute disponibilité et un équilibrage de la charge des connexions entre les clients DirectAccess et le réseau interne.  
  
Les procédures suivantes vous permettent de créer et de tester un cluster d’accès à distance :  
  
1. Installez la fonctionnalité d’équilibrage de charge réseau sur EDGE1 et EDGE2. Avant d’activer l’équilibrage de charge, vous devez installer la fonctionnalité d’équilibrage de charge réseau sur EDGE1 et EDGE2.
  
2. Activez l’équilibrage de charge sur EDGE1. EDGE1 a été installé à l’origine en mode serveur unique. Pour activer l’équilibrage de charge, vous configurez de nouvelles adresses IP dédiées externes et internes (DIP) pour EDGE1. Les adresses IP virtuelles précédentes sur EDGE1 sont automatiquement configurées en tant qu’adresses IP virtuelles (VIP) pour le cluster. La nouvelle DIP externe est 131.107.0.10, le nouveau DIP IPv4 interne est 10.0.0.10, la nouvelle DIP IPv6 interne est 2001 : DB8:1 :: 10. Les adresses IP virtuelles du cluster sont 131.107.0.2 et 131.107.0.3 (External), et 10.0.0.2 et 2001 : DB8:1 :: 2 (Internal).
  
3. Ajoutez EDGE2 au cluster à charge équilibrée. Après avoir activé l’équilibrage de charge, vous pouvez maintenant ajouter EDGE2 au cluster pour assurer l’équilibrage de charge et la haute disponibilité pour les connexions des clients DirectAccess.

## <a name="prerequisites"></a>Prérequis

Si vous créez ce laboratoire de test sur des machines virtuelles, vous devez activer l’usurpation d’adresses MAC sur EDGE1 et EDGE2.  
  
### <a name="enable-mac-address-spoofing-on-edge1-and-edge2"></a>Activer l’usurpation d’adresses MAC sur EDGE1 et EDGE2  
  
1.  Effectuez un arrêt approprié sur EDGE1 et EDGE2.  
  
2.  Sur l’ordinateur hébergeant vos machines virtuelles, dans le **Gestionnaire Hyper-V**, cliquez avec le bouton droit sur Edge1, puis cliquez sur **paramètres**.  
  
3.  Dans la boîte de dialogue **paramètres** , dans la liste **matériel** , cliquez sur la carte réseau connectée au réseau Corpnet, puis dans le volet d’informations, activez la case à cocher **activer l’usurpation des adresses Mac** .  
  
4.  Dans la liste **matériel** , cliquez sur la carte réseau connectée au réseau Internet, puis dans le volet d’informations, activez la case à cocher **activer l’usurpation des adresses Mac** .  
  
5.  Dans la boîte de dialogue **paramètres** , cliquez sur **OK**.  
  
6.  Répétez cette procédure à partir de l’étape 2 sur EDGE2.  
  
## <a name="install-the-network-load-balancing-feature-on-edge1-and-edge2"></a>Installer la fonctionnalité d’équilibrage de charge réseau sur EDGE1 et EDGE2  
Pour configurer EDGE1 et EDGE2 dans un cluster, vous devez installer la fonctionnalité d’équilibrage de charge réseau sur EDGE1 et EDGE2.  
  
### <a name="to-install-network-load-balancing"></a>Pour installer l’équilibrage de la charge réseau  
  
1.  Sur EDGE1, dans la console Gestionnaire de serveur, dans le **tableau de bord**, cliquez sur **Ajouter des rôles et des fonctionnalités**.  
  
2.  Cliquez sur **suivant** quatre fois pour accéder à l’écran de sélection des fonctionnalités du serveur.  
  
3.  Dans la boîte de dialogue **Sélectionner des fonctionnalités** , sélectionnez équilibrage de la **charge réseau**, cliquez sur **Ajouter des fonctionnalités**, cliquez sur **suivant**, puis sur **installer**.  
  
4.  Dans la boîte de dialogue **Progression de l’installation** , vérifiez que l’installation s’est correctement déroulée et cliquez sur **Fermer**.  
  
5.  Répétez cette procédure sur EDGE2.  
  
## <a name="enable-load-balancing-on-edge1"></a>Activer l’équilibrage de charge sur EDGE1  
Utilisez cette procédure pour activer l’équilibrage de charge et configurer les nouvelles adresses IP (DIP) sur EDGE1.  
  
### <a name="enable-load-balancing"></a>Activer l’équilibrage de charge  
  
1.  Sur EDGE1, cliquez sur **Démarrer**, tapez **RAMgmtUI. exe**, puis appuyez sur entrée. Si la boîte de dialogue **Contrôle de compte d'utilisateur** s'affiche, vérifiez que l'action affichée est celle que vous voulez, puis cliquez sur **Oui**.  
  
2.  Dans la console Gestion de l’accès à distance, dans le volet gauche, cliquez sur **configuration**, puis dans le volet **tâches** , cliquez sur **activer l’équilibrage de charge**.  
  
3.  Dans l’Assistant Activation de l’équilibrage de charge, cliquez sur **suivant**.  
  
4.  Sur la page **méthode d’équilibrage de charge** , cliquez sur utiliser l’équilibrage de **charge réseau Windows (NLB)** , puis cliquez sur **suivant**.  
  
5.  Dans la page **adresses IP dédiées externes** , dans la zone **adresse IPv4** , tapez **131.107.0.10**, dans la zone **masque de sous-réseau** , vérifiez que le préfixe de sous-réseau est **255.255.255.0**, puis cliquez sur **suivant**.  
  
6.  Dans la page **adresses IP dédiées internes** , effectuez les opérations suivantes, puis cliquez sur **suivant**:  
  
    1.  Dans la zone **adresse IPv4** , tapez **10.0.0.10** , puis dans la zone **masque de sous-réseau** , vérifiez que le préfixe de sous-réseau est **255.255.255.0**.  
  
    2.  Dans la zone **adresse IPv6** , tapez **2001 : DB8:1 :: 10** et dans la longueur du préfixe du sous-réseau, vérifiez que la valeur est **64**.  
  
7.  Sur la page **Résumé** , cliquez sur **valider**.  
  
8.  Dans la boîte de dialogue **activer l’équilibrage de charge** , cliquez sur **Fermer**.  
  
9. Dans l’Assistant Activation de l’équilibrage de charge, cliquez sur **Fermer**.  
  
## <a name="add-edge2-to-the-load-balanced-cluster"></a>Ajouter EDGE2 au cluster à charge équilibrée  
Utilisez cette procédure pour ajouter EDGE2 au cluster NLB.  
  
> [!NOTE]  
> Vous devez attendre deux minutes après avoir effectué les étapes précédentes avant de continuer. Une fois l’équilibrage de la charge réseau activé, le RAConfigTask exécute et configure l’ordinateur avec les paramètres NLB. Cette opération peut prendre quelques minutes, et si l’administrateur exécute une autre configuration liée à l’équilibrage de la charge réseau avant la fin de la tâche, cette configuration échoue.  
  
### <a name="add-edge2-to-the-cluster"></a>Ajouter EDGE2 au cluster  
  
1.  Sur l’ordinateur EDGE1 ou la machine virtuelle, dans la console Gestion de l’accès à distance, dans le volet **tâches** , sous **cluster à charge équilibrée**, cliquez sur **Ajouter ou supprimer des serveurs**.  
  
2.  Dans la boîte de dialogue **Ajouter ou supprimer des serveurs** , cliquez sur **Ajouter un serveur**.  
  
3.  Dans l’Assistant **Ajouter un serveur** , dans la page **Sélectionner un serveur** , tapez **EDGE2**, puis cliquez sur **suivant**.  
  
4.  Dans la page **cartes réseau** , dans **carte externe**, assurez-vous qu' **Internet** est sélectionné et, dans **carte interne**, assurez-vous que **corpnet** est sélectionné. Cliquez sur **Parcourir**, dans la boîte de dialogue **sécurité de Windows** , assurez-vous que **certificat IP-HTTPS** est sélectionné, cliquez sur **OK**, puis sur **suivant**.  
  
5.  Sur la page **Résumé** , cliquez sur **Ajouter**.  
  
6.  Sur la page **Fin**, cliquez sur **Fermer**.  
  
7.  Dans la boîte de dialogue **Ajouter ou supprimer des serveurs** , cliquez sur **valider**.  
  
8.  Dans la boîte de dialogue **Ajout et suppression de serveurs** , cliquez sur **Fermer**.  
  
9. Dans l’écran d' **Accueil** , tapez**Nlbmgr. exe**, puis appuyez sur entrée. Si la boîte de dialogue **Contrôle de compte d'utilisateur** s'affiche, vérifiez que l'action affichée est celle que vous voulez, puis cliquez sur **Oui**.  
  
10. Dans le **Gestionnaire d’équilibrage de la charge réseau**, cliquez sur **cluster da interne**. Dans le volet d’informations, assurez-vous que **Edge1 (corpnet)** et **EDGE2 (corpnet)** ont l’état **convergé**.  
  
11. Si un serveur n’est pas **convergé**, dans l’arborescence de la console, cliquez avec le bouton droit sur le serveur, pointez sur **hôte de contrôle**, puis cliquez sur **Démarrer**.  
  
12. Dans le **Gestionnaire d’équilibrage de la charge réseau**, cliquez sur **cluster da Internet**. Assurez-vous que dans le volet d’informations, **Edge1 (Internet)** et **EDGE2 (Internet)** ont l’état **convergé**.  
  
13. Si un serveur n’est pas **convergé**, dans l’arborescence de la console, cliquez avec le bouton droit sur le serveur, pointez sur **hôte de contrôle**, puis cliquez sur **Démarrer**.
