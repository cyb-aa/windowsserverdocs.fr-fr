---
title: ÉTAPE 3 installer et configurer EDGE2
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
ms.assetid: f04eb11e-ed5f-42a1-a77b-57a248ba2d10
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0692d47d50d84a66b5c3cc41d2ba2fca1004cafe
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870880"
---
# <a name="step-3-install-and-configure-edge2"></a>ÉTAPE 3 installer et configurer EDGE2

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

EDGE2 est le deuxième membre d’un cluster de l’accès à distance. EDGE2 est installé et configuré avant d’activer la configuration du cluster.

Procédez comme suit pour configurer EDGE2 :

## <a name="installOS"></a>Installer le système d’exploitation sur EDGE2  
  
1.  Sur EDGE2, démarrez l’installation de Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012.  
  
2.  Suivez les instructions pour terminer l’installation, en spécifiant Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012 (installation complète) et un mot de passe fort pour le compte d’administrateur local. Ouvrez une session à l’aide du compte Administrateur local.  
  
3.  Connecter EDGE2 à un réseau qui a accès à Internet et exécutez la mise à jour de Windows pour installer les dernières mises à jour pour Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012 et vous déconnecter puis à partir d’Internet.  
  
4.  Se connecter à une carte réseau au sous-réseau du réseau d’entreprise ou le commutateur virtuel représentant le sous-réseau de réseau d’entreprise et l’autre pour le sous-réseau Internet ou d’un commutateur virtuel représentant le sous-réseau Internet.  
  
## <a name="TCP"></a>Configurez les propriétés TCP/IP  
  
1.  Dans la console Gestionnaire de serveur, cliquez sur **serveur Local**, puis, dans le **propriétés** zone, en regard **connexion Ethernet câblée**, cliquez sur le lien.  
  
2.  Dans le **connexions réseau** fenêtre, avec le bouton droit de la connexion réseau qui est connectée au sous-réseau de réseau d’entreprise ou de commutateur virtuel, puis cliquez sur **renommer**.  
  
3.  Type **Corpnet**, puis appuyez sur ENTRÉE.  
  
4.  Avec le bouton droit **Corpnet**, puis cliquez sur **propriétés**.  
  
5.  Cliquez sur **Protocole Internet version 4 (TCP/IPv4)**, puis sur **Propriétés**.  
  
6.  Cliquez sur **Utiliser l’adresse IP suivante**. Dans **adresse IP**, type **10.0.0.8**. Dans la zone **Masque de sous-réseau**, tapez **255.255.255.0**.  
  
7.  Cliquez sur **Utiliser l’adresse de serveur DNS suivante**. Dans **Serveur DNS préféré**, tapez **10.0.0.1**.  
  
8.  Cliquez sur **Avancé**, puis sur l’onglet **DNS**.  
  
9. Dans **suffixe DNS pour cette connexion**, type **corp.contoso.com**, cliquez sur **OK** à deux reprises.  
  
10. Cliquez sur **Internet Protocol Version 6 (TCP/IPv6)**, puis cliquez sur **Propriétés**.  
  
11. Cliquez sur **utiliser l’adresse IPv6 suivante**. Dans **adresse IPv6**, type **2001:db8:1::8**. Dans **longueur de préfixe de sous-réseau**, type **64**.  
  
12. Cliquez sur **Utiliser l’adresse de serveur DNS suivante**. Dans **serveur DNS préféré**, type **2001:db8:1::1**.  
  
13. Cliquez sur **Avancé**, puis sur l’onglet **DNS**.  
  
14. Dans **suffixe DNS pour cette connexion**, type **corp.contoso.com**, cliquez sur **OK** à deux reprises, puis cliquez sur **fermer**.  
  
15. Dans le **connexions réseau** fenêtre, avec le bouton droit de la connexion réseau qui est connectée au sous-réseau Internet, puis cliquez sur **renommer**.  
  
16. Type **Internet**, puis appuyez sur ENTRÉE.  
  
17. Cliquez avec le bouton droit sur **Internet**, puis cliquez sur **Propriétés**.  
  
18. Cliquez sur **Protocole Internet version 4 (TCP/IPv4)**, puis sur **Propriétés**.  
  
19. Cliquez sur **Utiliser l’adresse IP suivante**. Dans **adresse IP**, entrez **131.107.0.8**. Dans **masque de sous-réseau**, entrez **255.255.255.0**.  
  
20. Cliquez sur le **DNS** onglet  
  
21. Dans **suffixe DNS pour cette connexion**, type **ISP.exemple.com**, puis cliquez sur **OK** à deux reprises, puis cliquez sur **fermer**.  
  
22. Fermez la fenêtre **Connexions réseau**.  
  
23. Pour vérifier la communication réseau entre EDGE2 et DC1, cliquez sur **Démarrer**, type **cmd**, puis appuyez sur ENTRÉE.  
  
24. Dans la fenêtre d’invite de commandes, tapez **ping dc1.corp.contoso.com** et appuyez sur ENTRÉE. Vérifiez qu’il existe quatre réponses depuis 10.0.0.1 ou l’adresse IPv6 adresse 2001:db8:1::1  
  
25. Fermez la fenêtre d’invite de commandes.  
  
## <a name="rename"></a>Renommer EDGE2 et joignez-le au domaine  
  
1.  Dans la console Gestionnaire de serveur, dans **serveur Local**, dans le **propriétés** zone, en regard **nom de l’ordinateur**, cliquez sur le lien.  
  
2.  Dans la boîte de dialogue **Propriétés système**, sous l'onglet **Nom de l'ordinateur**, cliquez sur **Modifier**.  
  
3.  Sur le **modification du nom ou du domaine d’ordinateur** boîte de dialogue le **nom de l’ordinateur** , tapez **EDGE2**. Dans le **membre** zone, cliquez sur **domaine**et dans la zone de texte, entrez **corp.contoso.com**, puis cliquez sur **OK**.  
  
4.  Lorsque vous êtes invité à entrer un nom d’utilisateur et un mot de passe, tapez **User1** et le mot de passe correspondant, puis cliquez sur **OK**.  
  
5.  Quand une boîte de dialogue vous souhaitant la bienvenue dans le domaine corp.contoso.com s’affiche, cliquez sur **OK**.  
  
6.  Lorsque vous êtes invité à redémarrer l’ordinateur, cliquez sur **OK**.  
  
7.  Dans la boîte de dialogue **Propriétés système**, cliquez sur **Fermer**.  
  
8.  Lorsque vous êtes invité à redémarrer l’ordinateur, cliquez sur **Redémarrer maintenant**.  
  
9. Après avoir redémarré, connectez-vous en tant que CORP\User1.  
  
## <a name="IPHTTPSCert"></a>Installer le certificat IP-HTTPS  
  
1.  Sur le **Démarrer** , tapez**mmc.exe**, puis appuyez sur ENTRÉE. Si la boîte de dialogue **Contrôle de compte d'utilisateur** s'affiche, vérifiez que l'action affichée est celle que vous voulez, puis cliquez sur **Oui**.  
  
2.  Dans la console MMC, dans le menu **Fichier**, cliquez sur **Ajouter/Supprimer un composant logiciel enfichable**.  
  
3.  Sur le **ajouter ou supprimer des composants logiciel enfichables** boîte de dialogue, cliquez sur **certificats**, cliquez sur **ajouter**, cliquez sur **compte d’ordinateur**, cliquez sur  **Suivant**, cliquez sur **Terminer**, puis cliquez sur **OK**.  
  
4.  Dans le volet gauche de la console, accédez à **certificats (ordinateur Local) \Personal\Certificates**. Bouton droit sur le **certificats** nœud, pointez sur **toutes les tâches**, puis cliquez sur **demander un nouveau certificat**.  
  
5.  Dans l’Assistant Inscription de certificat, cliquez sur **suivant** à deux reprises.  
  
6.  Sur le **demander des certificats** page, sélectionnez le **serveur Web** case à cocher, puis cliquez sur **inscription pour obtenir ce certificat nécessite des informations plus**.  
  
7.  Sur le **propriétés du certificat** boîte de dialogue le **sujet** sous l’onglet le **nom de l’objet** zone, dans le **Type** , cliquez sur **Nom commun**.  
  
8.  Dans **valeur**, type **edge1.contoso.com**, puis cliquez sur **ajouter**.  
  
9. Dans le **autre nom** zone, dans le **Type** , cliquez sur **DNS**.  
  
10. Dans **valeur**, type **edge1.contoso.com**, puis cliquez sur **ajouter**.  
  
11. Sur le **général** sous l’onglet **nom convivial**, type **certificat IP-HTTPS**.  
  
12. Cliquez sur **OK**, sur **Inscrire**, puis sur **Terminer**.  
  
13. Dans le volet détails du composant logiciel enfichable Certificats, vérifiez qu’un nouveau certificat avec le nom edge1.contoso.com a été inscrit avec rôles prévus l’à des fins d’authentification de serveur.  
  
14. Fermez la fenêtre de console. Si vous êtes invité à enregistrer les paramètres, cliquez sur **non**.  
  
## <a name="InstallDA"></a>Installer le rôle de l’accès à distance sur EDGE2  
  
1.  Dans la console Gestionnaire de serveur, dans le **tableau de bord**, cliquez sur **ajouter des rôles et fonctionnalités**.  
  
2.  Cliquez sur **Suivant** trois fois pour accéder à l’écran de sélection du rôle de serveur.  
  
3.  Sur le**Sélectionner des rôles de serveur**boîte de dialogue, sélectionnez**accès distant**cliquez sur**Ajouter des fonctionnalités**puis cliquez sur**Suivant**.  
  
4.  Cliquez sur **Suivant** cinq fois.  
  
5.  Dans la boîte de dialogue **Confirmer les sélections d’installation** , cliquez sur **Installer**.  
  
6.  Dans la boîte de dialogue **Progression de l’installation** , vérifiez que l’installation s’est correctement déroulée et cliquez sur **Fermer**.  
  


