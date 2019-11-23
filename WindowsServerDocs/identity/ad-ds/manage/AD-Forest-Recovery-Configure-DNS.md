---
title: Récupération de la forêt Active Directory-configurer le service serveur DNS
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 2c1f2f68509c9136735fb13e24c86a1da40660eb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71369247"
---
# <a name="ad-forest-recovery---configuring-the-dns-server-service"></a>Récupération de la forêt Active Directory-Configuration du service serveur DNS

>S’applique à : Windows Server 2016, Windows Server 2012 et 2012 R2, Windows Server 2008 et 2008 R2

Si le rôle de serveur DNS n’est pas installé sur le contrôleur de domaine que vous restaurez à partir d’une sauvegarde, vous devez installer et configurer le serveur DNS. 

## <a name="install-and-configure-the-dns-server-service"></a>Installer et configurer le service serveur DNS

Effectuez cette étape pour chaque contrôleur de domaine restauré qui ne s’exécute pas en tant que serveur DNS une fois la restauration terminée. 

> [!NOTE]
> Si le contrôleur de domaine que vous avez restauré à partir d’une sauvegarde exécute Windows Server 2008 R2, vous devez connecter le contrôleur de domaine à un réseau isolé pour installer le serveur DNS. Ensuite, connectez chacun des serveurs DNS restaurés à un réseau isolé mutuellement partagé. Exécutez repadmin/replsum. pour vérifier que la réplication fonctionne entre les serveurs DNS restaurés. Après avoir vérifié la réplication, vous pouvez connecter les contrôleurs de domaine restaurés au réseau de production si le rôle de serveur DNS est déjà installé, vous pouvez appliquer un correctif logiciel qui permet à un serveur DNS de démarrer alors que le serveur n’est connecté à aucun réseau. Vous devez intégrer le correctif dans l’image d’installation du système d’exploitation pendant les processus de génération automatisés. Pour plus d’informations sur le correctif, consultez l' [Article 975654](https://go.microsoft.com/fwlink/?LinkId=184691) de la base de connaissances Microsoft (https://go.microsoft.com/fwlink/?LinkId=184691). 

Effectuez les étapes d’installation et de configuration ci-dessous.

### <a name="to-install-and-the-dns-server-service-using-server-manager"></a>Pour installer et le service serveur DNS à l’aide de Gestionnaire de serveur  

1. Ouvrez Gestionnaire de serveur, puis cliquez sur **Ajouter des rôles et des fonctionnalités**. 
2. Dans l'Assistant Ajout de rôles, si la page **Avant de commencer** s'affiche, cliquez sur **Suivant**. 
3. Dans l’écran **type d’installation** , sélectionnez installation basée sur **un rôle ou une fonctionnalité** , puis cliquez sur **suivant**.
4. Dans l’écran **sélection du serveur** , sélectionnez le serveur, puis cliquez sur **suivant**.
5. Dans l' **écran rôles du serveur** , sélectionnez **serveur DNS**, si vous y êtes invité, cliquez sur **Ajouter des fonctionnalités** , puis sur **suivant**.
6. Dans l’écran **fonctionnalités** , cliquez sur **suivant**.
7. Lisez les informations de la page **serveur DNS** , puis cliquez sur **suivant**.
   ![serveur DNS](media/AD-Forest-Recovery-Configure-DNS/dns1.png)  
8. Dans la page **confirmation** , vérifiez que le rôle serveur DNS sera installé, puis cliquez sur **installer**. 

### <a name="to-configure-the-dns-server-service"></a>Pour configurer le service serveur DNS

1. Ouvrez Gestionnaire de serveur, cliquez sur **Outils** , puis sur **DNS**.
   ![serveur DNS](media/AD-Forest-Recovery-Configure-DNS/dns2.png)
2. Créez des zones DNS pour les mêmes noms de domaine DNS hébergés sur les serveurs DNS avant le dysfonctionnement critique. Pour plus d’informations, consultez Ajouter une zone de recherche directe ([https://go.microsoft.com/fwlink/?LinkId=74574](https://go.microsoft.com/fwlink/?LinkId=74574)).
3. Configurez les données DNS telles qu’elles existaient avant le dysfonctionnement critique. Par exemple :  

   - Configurez les zones DNS à stocker dans AD DS. Pour plus d’informations, consultez modifier le type de zone ([https://go.microsoft.com/fwlink/?LinkId=74579](https://go.microsoft.com/fwlink/?LinkId=74579)).
   - Configurez la zone DNS faisant autorité pour les enregistrements de ressource localisateur de contrôleur de domaine pour autoriser la mise à jour dynamique sécurisée. Pour plus d’informations, consultez autoriser uniquement les mises à jour dynamiques sécurisées ([https://go.microsoft.com/fwlink/?LinkId=74580](https://go.microsoft.com/fwlink/?LinkId=74580)).

4. Vérifiez que la zone DNS parent contient les enregistrements de ressource de délégation (NS) et les enregistrements de ressource d’hôte (A) de la zone enfant hébergée sur ce serveur DNS. Pour plus d’informations, consultez créer une délégation de zone ([https://go.microsoft.com/fwlink/?LinkId=74562](https://go.microsoft.com/fwlink/?LinkId=74562)).
5. Après avoir configuré DNS, vous pouvez accélérer l’inscription des enregistrements NETLOGon.

   > [!NOTE]
   > Les mises à jour dynamiques sécurisées fonctionnent uniquement lorsqu’un serveur de catalogue global est disponible. 

   À l'invite de commandes, tapez la commande suivante et appuyez sur Entrée :  

   **net stop Netlogon**  

6. Tapez la commande suivante et appuyez sur ENTRÉE :  

   **NET START Netlogon**  

   ![Serveur DNS](media/AD-Forest-Recovery-Configure-DNS/dns3.png)  

## <a name="next-steps"></a>Étapes suivantes

- [Guide de récupération de la forêt Active Directory](AD-Forest-Recovery-Guide.md)
- [Récupération de la forêt Active Directory : procédures](AD-Forest-Recovery-Procedures.md)
