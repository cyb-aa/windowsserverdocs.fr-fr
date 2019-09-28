---
title: ÉTAPE 3 installation et configuration de EDGE2
description: Cette rubrique fait partie du Guide de laboratoire de test-démonstration de DirectAccess dans un cluster avec Windows NLB pour Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f04eb11e-ed5f-42a1-a77b-57a248ba2d10
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3dad1db575bd9b9b4a70a24da44d1d030273f021
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404863"
---
# <a name="step-3-install-and-configure-edge2"></a>ÉTAPE 3 installation et configuration de EDGE2

>S'applique à : Windows Server (Canal semi-annuel), Windows Server 2016

EDGE2 est le deuxième membre d’un cluster d’accès à distance. EDGE2 est installé et configuré avant d’activer la configuration du cluster.

Pour configurer EDGE2, procédez comme suit :

## <a name="installOS"></a>Installer le système d’exploitation sur EDGE2  
  
1.  Sur EDGE2, démarrez l’installation de Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012.  
  
2.  Suivez les instructions pour terminer l’installation, en spécifiant Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012 (installation complète) et un mot de passe fort pour le compte administrateur local. Ouvrez une session à l’aide du compte Administrateur local.  
  
3.  Connectez EDGE2 à un réseau disposant d’un accès Internet et exécutez Windows Update pour installer les dernières mises à jour pour Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012, puis vous déconnecter d’Internet.  
  
4.  Connectez une carte réseau au sous-réseau Corpnet ou au commutateur virtuel représentant le sous-réseau Corpnet et l’autre au sous-réseau Internet ou au commutateur virtuel représentant le sous-réseau Internet.  
  
## <a name="TCP"></a>Configurer les propriétés TCP/IP  
  
1.  Dans la console Gestionnaire de serveur, cliquez sur **serveur local**, puis dans la zone **Propriétés** , en regard de **connexion Ethernet câblée**, cliquez sur le lien.  
  
2.  Dans la fenêtre **connexions réseau** , cliquez avec le bouton droit sur la connexion réseau qui est connectée au sous-réseau Corpnet ou au commutateur virtuel, puis cliquez sur **Renommer**.  
  
3.  Tapez **corpnet**, puis appuyez sur entrée.  
  
4.  Cliquez avec le bouton droit sur **corpnet**, puis cliquez sur **Propriétés**.  
  
5.  Cliquez sur **Protocole Internet version 4 (TCP/IPv4)** , puis sur **Propriétés**.  
  
6.  Cliquez sur **Utiliser l’adresse IP suivante**. Dans **adresse IP**, tapez **10.0.0.8**. Dans la zone **Masque de sous-réseau**, tapez **255.255.255.0**.  
  
7.  Cliquez sur **Utiliser l’adresse de serveur DNS suivante**. Dans **Serveur DNS préféré**, tapez **10.0.0.1**.  
  
8.  Cliquez sur **Avancé**, puis sur l’onglet **DNS**.  
  
9. Dans **suffixe DNS pour cette connexion**, tapez **Corp.contoso.com**, cliquez deux fois sur **OK** .  
  
10. Cliquez sur **Internet Protocol Version 6 (TCP/IPv6)** , puis cliquez sur **Propriétés**.  
  
11. Cliquez sur **utiliser l’adresse IPv6 suivante**. Dans **adresse IPv6**, tapez **2001 : DB8:1 :: 8**. Dans **longueur du préfixe du sous-réseau**, tapez **64**.  
  
12. Cliquez sur **Utiliser l’adresse de serveur DNS suivante**. Dans **serveur DNS préféré**, tapez **2001 : DB8:1 :: 1**.  
  
13. Cliquez sur **Avancé**, puis sur l’onglet **DNS**.  
  
14. Dans **suffixe DNS pour cette connexion**, tapez **Corp.contoso.com**, cliquez deux fois sur **OK** , puis cliquez sur **Fermer**.  
  
15. Dans la fenêtre **connexions réseau** , cliquez avec le bouton droit sur la connexion réseau qui est connectée au sous-réseau Internet, puis cliquez sur **Renommer**.  
  
16. Tapez **Internet**, puis appuyez sur entrée.  
  
17. Cliquez avec le bouton droit sur **Internet**, puis cliquez sur **Propriétés**.  
  
18. Cliquez sur **Protocole Internet version 4 (TCP/IPv4)** , puis sur **Propriétés**.  
  
19. Cliquez sur **Utiliser l’adresse IP suivante**. Dans **adresse IP**, entrez **131.107.0.8**. Dans **masque de sous-réseau**, entrez **255.255.255.0**.  
  
20. Cliquez sur l’onglet **DNS** .  
  
21. Dans **suffixe DNS pour cette connexion**, tapez **ISP.example.com**, puis cliquez sur **OK** à deux reprises, puis sur **Fermer**.  
  
22. Fermez la fenêtre **Connexions réseau**.  
  
23. Pour vérifier la communication réseau entre EDGE2 et DC1, cliquez sur **Démarrer**, tapez **cmd**, puis appuyez sur entrée.  
  
24. Dans la fenêtre d’invite de commandes, tapez **ping dc1.Corp.contoso.com** et appuyez sur entrée. Vérifiez qu’il y a quatre réponses de 10.0.0.1 ou l’adresse IPv6 2001 : DB8:1 :: 1  
  
25. Fermez la fenêtre d’invite de commandes.  
  
## <a name="rename"></a>Renommer EDGE2 et le joindre au domaine  
  
1.  Dans la console Gestionnaire de serveur, dans **serveur local**, dans la zone **Propriétés** , en regard de nom de l' **ordinateur**, cliquez sur le lien.  
  
2.  Dans la boîte de dialogue **Propriétés système**, sous l'onglet **Nom de l'ordinateur**, cliquez sur **Modifier**.  
  
3.  Dans la boîte de dialogue **modification du nom ou du domaine** de l’ordinateur, dans la zone nom de l' **ordinateur** , tapez **EDGE2**. Dans la zone **membre de** , cliquez sur **domaine**, puis dans la zone de texte, entrez **Corp.contoso.com**, puis cliquez sur **OK**.  
  
4.  Lorsque vous êtes invité à entrer un nom d’utilisateur et un mot de passe, tapez **User1** et le mot de passe correspondant, puis cliquez sur **OK**.  
  
5.  Quand une boîte de dialogue vous souhaitant la bienvenue dans le domaine corp.contoso.com s’affiche, cliquez sur **OK**.  
  
6.  Lorsque vous êtes invité à redémarrer l’ordinateur, cliquez sur **OK**.  
  
7.  Dans la boîte de dialogue **Propriétés système**, cliquez sur **Fermer**.  
  
8.  Lorsque vous êtes invité à redémarrer l’ordinateur, cliquez sur **Redémarrer maintenant**.  
  
9. Après le redémarrage, connectez-vous en tant que CORP\User1.  
  
## <a name="IPHTTPSCert"></a>Installer le certificat IP-HTTPs  
  
1.  Dans l’écran **Démarrer** , tapez**MMC. exe**, puis appuyez sur entrée. Si la boîte de dialogue **Contrôle de compte d'utilisateur** s'affiche, vérifiez que l'action affichée est celle que vous voulez, puis cliquez sur **Oui**.  
  
2.  Dans la console MMC, dans le menu **Fichier**, cliquez sur **Ajouter/Supprimer un composant logiciel enfichable**.  
  
3.  Dans la boîte de dialogue **Ajouter ou supprimer des composants logiciels enfichables** , cliquez sur **certificats**, sur **Ajouter**, sur **compte d’ordinateur**, sur **suivant**, sur **Terminer**, puis sur **OK**.  
  
4.  Dans le volet gauche de la console, accédez à **certificats (ordinateur local) \Personal\Certificates**. Cliquez avec le bouton droit sur le nœud **certificats** , pointez sur **toutes les tâches**, puis cliquez sur **demander un nouveau certificat**.  
  
5.  Dans l’Assistant inscription de certificats, cliquez sur **suivant** à deux reprises.  
  
6.  Dans la page **demander des certificats** , activez la case à cocher **serveur Web** , puis cliquez sur l' **inscription pour obtenir ce certificat nécessite des informations supplémentaires**.  
  
7.  Dans la boîte de dialogue **Propriétés du certificat** , sous l’onglet **sujet** , dans la zone nom de l' **objet** , dans la liste **type** , cliquez sur **nom commun**.  
  
8.  Dans **valeur**, tapez **Edge1.contoso.com**, puis cliquez sur **Ajouter**.  
  
9. Dans la zone **autre nom** , dans la liste **type** , cliquez sur **DNS**.  
  
10. Dans **valeur**, tapez **Edge1.contoso.com**, puis cliquez sur **Ajouter**.  
  
11. Sous l’onglet **général** , dans **nom convivial**, tapez **certificat IP-HTTPS**.  
  
12. Cliquez sur **OK**, sur **Inscrire**, puis sur **Terminer**.  
  
13. Dans le volet d’informations du composant logiciel enfichable Certificats, vérifiez qu’un nouveau certificat portant le nom edge1.contoso.com a été inscrit dans le cadre de l’authentification du serveur.  
  
14. Fermez la fenêtre de console. Si vous êtes invité à enregistrer les paramètres, cliquez sur **non**.  
  
## <a name="InstallDA"></a>Installer le rôle accès à distance sur EDGE2  
  
1.  Dans la console Gestionnaire de serveur, dans le **tableau de bord**, cliquez sur **Ajouter des rôles et des fonctionnalités**.  
  
2.  Cliquez sur **Suivant** trois fois pour accéder à l’écran de sélection du rôle de serveur.  
  
3.  Sur le**Sélectionner des rôles de serveur**boîte de dialogue, sélectionnez**accès distant**cliquez sur**Ajouter des fonctionnalités**puis cliquez sur**Suivant**.  
  
4.  Cliquez sur **Suivant** cinq fois.  
  
5.  Dans la boîte de dialogue **Confirmer les sélections d’installation** , cliquez sur **Installer**.  
  
6.  Dans la boîte de dialogue **Progression de l’installation** , vérifiez que l’installation s’est correctement déroulée et cliquez sur **Fermer**.  
  


