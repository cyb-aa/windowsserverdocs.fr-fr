---
title: "Récupération de la forêt ActiveDirectory - service de configurer le serveur DNS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f24570965fd8b3f3e050779c42758865cbee2728
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="ad-forest-recovery---configuring-the-dns-server-service"></a>Récupération de la forêt ActiveDirectory - Configuration du service serveur DNS 

>S’applique à: Windows Server2016, Windows Server2012 et 2012R2, Windows Server2008 et 2008R2
 
Si le rôle de serveur DNS n’est pas installé sur le contrôleur de domaine que vous restaurez à partir de la sauvegarde, vous devez installer et configurer le serveur DNS.  
  

## <a name="install-and-configure-the-dns-server-service"></a>Installer et configurer le service serveur DNS  
Effectuez cette étape pour chaque contrôleur de domaine restauré qui ne fonctionne pas comme un serveur DNS une fois la restauration terminée.  
  
> [!NOTE]
>  Si le contrôleur de domaine que vous avez restauré à partir de la sauvegarde s’exécute Windows Server2008R2, vous devez connecter le contrôleur de domaine à un réseau isolé pour installer le serveur DNS. Connectez ensuite chacun des serveurs DNS restaurés à un réseau partagé s’excluent mutuellement, isolé. Exécutez repadmin /replsum pour vérifier que la réplication fonctionne entre les serveurs DNS restaurés. Après avoir vérifié la réplication, les contrôleurs de domaine restaurés peuvent se connecter au réseau de production si le rôle de serveur DNS est déjà installé, vous pouvez appliquer un correctif qui rend possible pour un serveur DNS de démarrer alors que le serveur n’est pas connecté à un réseau. Vous devez intégrer le correctif logiciel dans l’image d’installation de système d’exploitation pendant votre processus de génération automatique. Pour plus d’informations sur le correctif logiciel, voir [Article975654](https://go.microsoft.com/fwlink/?LinkId=184691) dans la Base de connaissances Microsoft (https://go.microsoft.com/fwlink/?LinkId=184691). 

Effectuez les étapes d’installation et la configuration ci-dessous.
  
### <a name="to-install-and-the-dns-server-service-using-server-manager"></a>Pour installer et le service serveur DNS à l’aide du Gestionnaire de serveur  
  
1.  Ouvrez le Gestionnaire de serveur et cliquez sur **ajouter des rôles et fonctionnalités**.  
2.  Dans l’Assistant Ajout de rôles, si le **avant de commencer** page s’affiche, cliquez sur **suivant**.  
3.  Sur le **type d’Installation** écran Sélectionnez **un rôle ou fonctionnalité installation basée sur un** et cliquez sur **suivant**.
4.  Sur le **sélection du serveur** écran, sélectionnez le serveur et cliquez sur **suivant**.
5.  Sur le **des rôles de serveurs** écran Sélectionnez **serveur DNS**, si vous y êtes invité cliquez sur **ajouter des fonctionnalités** et cliquez sur **suivant**.
6.  Sur le **fonctionnalités** écran, cliquez sur **suivant**.
7.  Lire les informations sur la **serveur DNS** page, puis cliquez sur **suivant**.
![Serveur DNS](media/AD-Forest-Recovery-Configure-DNS/dns1.png)  
8.  Sur le **Confirmation**, vérifiez que le rôle serveur DNS sera installé, puis cliquez sur **installer**.  
  
     
### <a name="to-configure-the-dns-server-service"></a>Pour configurer le service serveur DNS 
1.  Ouvrez le Gestionnaire de serveur, cliquez sur **outils** et cliquez sur **DNS**.
![Serveur DNS](media/AD-Forest-Recovery-Configure-DNS/dns2.png)    
2.  Créer des zones DNS pour les mêmes noms de domaine DNS hébergés sur les serveurs DNS avant la défaillance critique. Pour plus d’informations, voir Ajouter une Zone de recherche directe ([https://go.microsoft.com/fwlink/?LinkId=74574](https://go.microsoft.com/fwlink/?LinkId=74574)).  
3.  Configurer les données DNS, tel qu’il existait avant la défaillance critique. Par exemple:  
  
    -   Configurer des zones DNS à être stockées dans ADDS. Pour plus d’informations, voir modifier le Type de Zone ([https://go.microsoft.com/fwlink/?LinkId=74579](https://go.microsoft.com/fwlink/?LinkId=74579)).  
  
    -   Configurer la zone DNS faisant autoritée pour les enregistrements de ressource de localisateur du contrôleur de domaine de contrôleur de domaine permettre la mise à jour dynamique sécurisée. Pour plus d’informations, voir autoriser uniquement sécuriser les mises à jour dynamiques ([https://go.microsoft.com/fwlink/?LinkId=74580](https://go.microsoft.com/fwlink/?LinkId=74580)).  
  
4. Assurez-vous que la zone DNS parente contienne des enregistrements de ressource délégation (nom de serveur (NS) et collage ressource hôte (A) enregistrements) pour la zone enfant qui est hébergé sur ce serveur DNS. Pour plus d’informations, voir créer une délégation de Zone ([https://go.microsoft.com/fwlink/?LinkId=74562](https://go.microsoft.com/fwlink/?LinkId=74562)).  
5. Une fois que vous configurez DNS, vous pouvez accélérer l’inscription des enregistrements de l’accès réseau.  
  
    > [!NOTE]
    >  Les mises à jour dynamiques sécurisées fonctionnent uniquement lorsqu’un serveur de catalogue global est disponible.  
  
     À l’invite de commandes, tapez la commande suivante et appuyez sur ENTRÉE:  
  
     **Net stop netlogon**  
  
6. Tapez la commande suivante et appuyez sur ENTRÉE:  
  
     **Net start messagerie**  

![Serveur DNS](media/AD-Forest-Recovery-Configure-DNS/dns3.png)  

## <a name="next-steps"></a>Étapes suivantes

- [Guide de récupération de forêt ActiveDirectory](AD-Forest-Recovery-Guide.md)
- [Récupération de la forêt ActiveDirectory - procédures](AD-Forest-Recovery-Procedures.md)
