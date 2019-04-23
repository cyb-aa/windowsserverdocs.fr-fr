---
title: Récupération de forêt AD - service de configurer le serveur DNS
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: b2c37428a0fb685e6a7fa4875366f3cd13401bd9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842960"
---
# <a name="ad-forest-recovery---configuring-the-dns-server-service"></a>Récupération de forêt AD - configuration du service serveur DNS

>S'applique à : Windows Server 2016, Windows Server 2012 et 2012 R2, Windows Server 2008 et 2008 R2

Si le rôle de serveur DNS n’est pas installé sur le contrôleur de domaine que vous restaurez à partir de la sauvegarde, vous devez installer et configurer le serveur DNS. 

## <a name="install-and-configure-the-dns-server-service"></a>Installer et configurer le service serveur DNS

Effectuez cette étape pour chaque contrôleur de domaine restauré ne fonctionne pas comme un serveur DNS une fois la restauration terminée. 

> [!NOTE]
> Si le contrôleur de domaine que vous avez restauré à partir de la sauvegarde est en cours d’exécution Windows Server 2008 R2, vous devez connecter le contrôleur de domaine à un réseau isolé pour installer le serveur DNS. Puis connectez chacune des serveurs DNS restaurées à un réseau isolé partagé s’excluent mutuellement. Exécutez/replsum repadmin pour vérifier que la réplication fonctionne entre les serveurs DNS restaurées. Après avoir vérifié la réplication, vous pouvez vous connecter les contrôleurs de domaine restaurés vers le réseau de production si le rôle de serveur DNS est déjà installé, vous pouvez appliquer un correctif logiciel qui permet à un serveur DNS pour démarrer alors que le serveur n’est pas connecté à aucun réseau. Vous devez intégrer le correctif logiciel dans l’image d’installation de système d’exploitation pendant votre processus de génération automatisé. Pour plus d’informations sur le correctif logiciel, consultez [Article 975654](https://go.microsoft.com/fwlink/?LinkId=184691) dans la Base de connaissances Microsoft (https://go.microsoft.com/fwlink/?LinkId=184691). 

Effectuez les étapes d’installation et la configuration ci-dessous.

### <a name="to-install-and-the-dns-server-service-using-server-manager"></a>Installation et le service serveur DNS à l’aide du Gestionnaire de serveur  

1. Ouvrez le Gestionnaire de serveur et cliquez sur **ajouter des rôles et fonctionnalités**. 
2. Dans l'Assistant Ajout de rôles, si la page **Avant de commencer** s'affiche, cliquez sur **Suivant**. 
3. Sur le **type d’Installation** écran Sélectionnez **en fonction du rôle ou fonctionnalité une installation basée sur** et cliquez sur **suivant**.
4. Sur le **sélection du serveur** écran Sélectionnez le serveur et cliquez sur **suivant**.
5. Sur le **rôles serveur** écran Sélectionnez **serveur DNS**, si vous y êtes invité cliquez **ajouter des fonctionnalités** et cliquez sur **suivant**.
6. Sur le **fonctionnalités** écran, cliquez sur **suivant**.
7. Lisez les informations sur le **serveur DNS** page, puis cliquez sur **suivant**.
   ![Serveur DNS](media/AD-Forest-Recovery-Configure-DNS/dns1.png)  
8. Sur le **Confirmation** , vérifiez que le rôle serveur DNS sera installé, puis cliquez sur **installer**. 

### <a name="to-configure-the-dns-server-service"></a>Pour configurer le service serveur DNS

1. Ouvrez le Gestionnaire de serveur, cliquez sur **outils** et cliquez sur **DNS**.
   ![Serveur DNS](media/AD-Forest-Recovery-Configure-DNS/dns2.png)
2. Créer des zones DNS pour les mêmes noms de domaine DNS qui étaient hébergés sur les serveurs DNS avant la défaillance critique. Pour plus d’informations, voir Ajouter une Zone de recherche directe ([https://go.microsoft.com/fwlink/?LinkId=74574](https://go.microsoft.com/fwlink/?LinkId=74574)).
3. Configurer les données DNS telle qu’elle existait avant la défaillance critique. Exemple :  

   - Configurer des zones DNS à être stockées dans AD DS. Pour plus d’informations, consultez le Type de Zone de modification ([https://go.microsoft.com/fwlink/?LinkId=74579](https://go.microsoft.com/fwlink/?LinkId=74579)).
   - Configurer la zone DNS faisant autoritée pour les enregistrements de ressource de (localisateur de contrôleur de domaine) de contrôleur de domaine permettre la mise à jour dynamique sécurisée. Pour plus d’informations, consultez Autoriser uniquement sécuriser les mises à jour dynamiques ([https://go.microsoft.com/fwlink/?LinkId=74580](https://go.microsoft.com/fwlink/?LinkId=74580)).

4. Assurez-vous que la zone DNS parente contienne des enregistrements de ressources délégation (nom de serveur de (noms NS) et de type glue ressource hôte (A) enregistrements) pour la zone enfant qui est hébergé sur ce serveur DNS. Pour plus d’informations, voir créer une délégation de Zone ([https://go.microsoft.com/fwlink/?LinkId=74562](https://go.microsoft.com/fwlink/?LinkId=74562)).
5. Après avoir configuré le DNS, vous pouvez accélérer l’inscription des enregistrements de l’accès réseau.

   > [!NOTE]
   > Les mises à jour dynamiques sécurisées fonctionnent uniquement lorsqu’un serveur de catalogue global est disponible. 

   À l'invite de commandes, tapez la commande suivante et appuyez sur Entrée :  

   **net stop netlogon**  

6. Tapez la commande suivante et appuyez sur ENTRÉE :  

   **Net start netlogon**  

   ![Serveur DNS](media/AD-Forest-Recovery-Configure-DNS/dns3.png)  

## <a name="next-steps"></a>Étapes suivantes

- [Guide de la forêt Active Directory](AD-Forest-Recovery-Guide.md)
- [Récupération de forêt AD - procédures](AD-Forest-Recovery-Procedures.md)
