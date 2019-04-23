---
title: ÉTAPE 4 créer le Cluster de l’accès à distance à charge équilibrée de charge réseau
description: Cette rubrique fait partie du Guide de laboratoire de Test - décrire de DirectAccess dans un Cluster avec équilibrage de charge réseau Windows pour Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 509eaa08-c49d-448d-a71e-c1c45519ccd5
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 11f4fe7b68f69b00ec0f8fb9764e0fb4460a2851
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845950"
---
# <a name="step-4-create-the-network-load-balanced-remote-access-cluster"></a>ÉTAPE 4 créer le Cluster de l’accès à distance à charge équilibrée de charge réseau

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

 Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012 permettent de créer des clusters de serveurs d’accès à distance. Un cluster agit comme un seul serveur logique et fournit la configuration centralisée et gestion pour les serveurs du cluster. Lorsque vous utilisez l’équilibrage de charge réseau (NLB) il est prise en charge jusqu'à 8 membres l’accès à distance dans un seul cluster. Les clusters de l’accès à distance fournissant une haute disponibilité et l’équilibrage de connexions entre les clients DirectAccess et le réseau interne.  
  
Les procédures suivantes permettent de créer et tester un cluster de l’accès à distance :  
  
1. Installer la fonctionnalité d’équilibrage de charge réseau sur EDGE1 et EDGE2. Avant d’activer l’équilibrage de charge, vous devez installer la fonctionnalité d’équilibrage de charge réseau sur EDGE1 et EDGE2.
  
2. Activer l’équilibrage de charge sur EDGE1. EDGE1 a été initialement installé en mode de serveur unique. Pour activer l’équilibrage de charge, vous configurez nouvelle internes et externes adresses IP dédiées (DIP) pour EDGE1. Les adresses IP dynamiques précédentes sur EDGE1 sont automatiquement configurés en tant qu’adresses IP virtuelles (VIP) pour le cluster. La nouvelle adresse DIP externe est 131.107.0.10, l’adresse IP dédiée IPv4 interne est 10.0.0.10, la nouvelle adresse IPv6 DIP interne est 2001:db8:1::10. Les adresses IP virtuelles de cluster sont 131.107.0.2 et 131.107.0.3 (externe) et 10.0.0.2 et 2001:db8:1::2 (interne).
  
3. Ajouter EDGE2 pour le cluster d’équilibrage de charge. Après l’activation de l’équilibrage de charge, vous pouvez maintenant ajouter EDGE2 au cluster pour fournir la charge équilibrage et de haute disponibilité pour les connexions de client DirectAccess.

## <a name="prerequisites"></a>Prérequis

Si vous créez ce laboratoire de test sur des machines virtuelles, vous devez activer l’usurpation sur EDGE1 et EDGE2 des adresses MAC.  
  
### <a name="enable-mac-address-spoofing-on-edge1-and-edge2"></a>Activer l’usurpation sur EDGE1 et EDGE2 des adresses MAC  
  
1.  Effectuer un arrêt approprié sur EDGE1 et EDGE2.  
  
2.  Sur la machine hébergeant vos machines virtuelles, dans **Gestionnaire Hyper-V**, EDGE1 d’avec le bouton droit, puis cliquez sur **paramètres**.  
  
3.  Sur le **paramètres** boîte de dialogue le **matériel** liste et cliquez sur la carte réseau connectée au réseau du réseau d’entreprise, puis dans le volet de détails, sélectionnez le **activer l’usurpation des adresses MAC**  case à cocher.  
  
4.  Dans le **matériel** liste et cliquez sur la carte réseau connectée au réseau Internet, puis dans le volet de détails, sélectionnez le **activer l’usurpation des adresses MAC** case à cocher.  
  
5.  Sur le **paramètres** boîte de dialogue, cliquez sur **OK**.  
  
6.  Répétez cette procédure à l’étape 2 sur EDGE2.  
  
## <a name="install-the-network-load-balancing-feature-on-edge1-and-edge2"></a>Installer la fonctionnalité d’équilibrage de charge réseau sur EDGE1 et EDGE2  
Pour configurer EDGE1 et EDGE2 dans un cluster, vous devez installer la fonctionnalité d’équilibrage de charge réseau sur EDGE1 et EDGE2.  
  
### <a name="to-install-network-load-balancing"></a>Pour installer l’équilibrage de charge réseau  
  
1.  Sur EDGE1, dans la console Gestionnaire de serveur, dans le **tableau de bord**, cliquez sur **ajouter des rôles et fonctionnalités**.  
  
2.  Cliquez sur **suivant** quatre fois pour accéder à l’écran de sélection de fonctionnalité de serveur.  
  
3.  Sur le **sélectionner des fonctionnalités** boîte de dialogue, sélectionnez **équilibrage de charge réseau**, cliquez sur **ajouter des fonctionnalités**, cliquez sur **suivant**, puis cliquez sur **Installer**.  
  
4.  Dans la boîte de dialogue **Progression de l’installation** , vérifiez que l’installation s’est correctement déroulée et cliquez sur **Fermer**.  
  
5.  Répétez cette procédure sur EDGE2.  
  
## <a name="enable-load-balancing-on-edge1"></a>Activer l’équilibrage de charge sur EDGE1  
Utilisez cette procédure pour activer l’équilibrage de charge et de configurer les nouvelles adresses IP dynamiques sur EDGE1.  
  
### <a name="enable-load-balancing"></a>Activer l’équilibrage de charge  
  
1.  Dans EDGE1, cliquez sur **Démarrer**, type **RAMgmtUI.exe**, puis appuyez sur ENTRÉE. Si la boîte de dialogue **Contrôle de compte d'utilisateur** s'affiche, vérifiez que l'action affichée est celle que vous voulez, puis cliquez sur **Oui**.  
  
2.  Dans la console de gestion de l’accès à distance, dans le volet gauche, cliquez sur **Configuration**, puis, dans le **tâches** volet, cliquez sur **activer l’équilibrage de charge**.  
  
3.  Dans l’Assistant Activation de charge équilibrage, cliquez sur **suivant**.  
  
4.  Sur le **méthode d’équilibrage de charge** , cliquez sur **utilisez Windows NLB Network Load Balancing ()**, puis cliquez sur **suivant**.  
  
5.  Sur le **externes des adresses IP dédiées** page, dans le **adresse IPv4** , tapez **131.107.0.10**, dans le **masque de sous-réseau** , vérifiez la préfixe de sous-réseau est **255.255.255.0**, puis cliquez sur **suivant**.  
  
6.  Sur le **interne des adresses IP dédiées** page, procédez comme suit, puis cliquez sur **suivant**:  
  
    1.  Dans le **adresse IPv4** , tapez **10.0.0.10** et dans le **masque de sous-réseau** , vérifiez le préfixe de sous-réseau est **255.255.255.0**.  
  
    2.  Dans le **adresse IPv6** , tapez **2001:db8:1::10** puis, dans la longueur de préfixe de sous-réseau, vérifiez la valeur est **64**.  
  
7.  Sur le **Résumé** , cliquez sur **valider**.  
  
8.  Sur le **activer l’équilibrage de charge** boîte de dialogue, cliquez sur **fermer**.  
  
9. Dans l’Assistant Activation de charge équilibrage, cliquez sur **fermer**.  
  
## <a name="add-edge2-to-the-load-balanced-cluster"></a>Ajouter EDGE2 pour le cluster d’équilibrage de charge  
Utilisez cette procédure pour ajouter des EDGE2 au cluster NLB.  
  
> [!NOTE]  
> Vous devez patienter deux minutes après avoir effectué les étapes précédentes avant de continuer. Après l’activation de NLB, la RAConfigTask s’exécute et configure l’ordinateur avec les paramètres d’équilibrage de charge réseau. Cette opération peut prendre quelques minutes pour terminer, et si l’administrateur exécute une autre configuration connexe NLB avant la fin de la tâche, que la configuration échoue.  
  
### <a name="add-edge2-to-the-cluster"></a>Ajouter EDGE2 au cluster  
  
1.  Sur l’ordinateur de EDGE1 ou d’un ordinateur virtuel, dans la Console de gestion de l’accès à distance, dans le **tâches** volet, sous **Cluster avec équilibrage de charge**, cliquez sur **ajouter ou supprimer des serveurs**.  
  
2.  Sur le **ajouter ou supprimer des serveurs** boîte de dialogue, cliquez sur **ajouter un serveur**.  
  
3.  Sur le **ajouter un serveur** Assistant, sur le **sélectionner un serveur** , tapez **EDGE2**, puis cliquez sur **suivant**.  
  
4.  Sur le **cartes réseau** page, dans **adaptateur externe**, assurez-vous que l’option **Internet** est sélectionnée, puis, dans **carte réseau interne**, assurez-vous que qui **Corpnet** est sélectionné. Cliquez sur **Parcourir**, dans le **Windows Security** boîte de dialogue zone, assurez-vous que l’option **certificat IP-HTTPS** est sélectionnée, cliquez sur **OK**, puis cliquez sur **Suivant**.  
  
5.  Sur le **Résumé** , cliquez sur **ajouter**.  
  
6.  Sur la page **Fin**, cliquez sur **Fermer**.  
  
7.  Sur le **ajouter ou supprimer des serveurs** boîte de dialogue, cliquez sur **valider**.  
  
8.  Sur le **Ajout et suppression de serveurs** boîte de dialogue, cliquez sur **fermer**.  
  
9. Sur le **Démarrer** , tapez**nlbmgr.exe**, puis appuyez sur ENTRÉE. Si la boîte de dialogue **Contrôle de compte d'utilisateur** s'affiche, vérifiez que l'action affichée est celle que vous voulez, puis cliquez sur **Oui**.  
  
10. Dans le **le Gestionnaire d’équilibrage de charge réseau**, cliquez sur **DA interne cluster**. Dans le volet détails, vérifiez que les deux **EDGE1(Corpnet)** et **EDGE2(Corpnet)** ont l’état **convergé**.  
  
11. Si un serveur n’est pas **convergé**, dans l’arborescence de la console, cliquez sur le serveur, pointez sur **contrôle hôte**, puis cliquez sur **Démarrer**.  
  
12. Dans le **le Gestionnaire d’équilibrage de charge réseau**, cliquez sur **Internet DA cluster**. Assurez-vous que dans le volet de détails, les deux **EDGE1(Internet)** et **EDGE2(Internet)** ont l’état **convergé**.  
  
13. Si un serveur n’est pas **convergé**, dans l’arborescence de la console, cliquez sur le serveur, pointez sur **contrôle hôte**, puis cliquez sur **Démarrer**.
