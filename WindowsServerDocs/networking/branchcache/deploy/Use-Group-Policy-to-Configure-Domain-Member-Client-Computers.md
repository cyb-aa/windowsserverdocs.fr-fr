---
title: Utilisez la stratégie de groupe pour configurer les ordinateurs clients membres du domaine
description: Cette rubrique fait partie de BranchCache déploiement Guide pour Windows Server 2016, qui montre comment déployer BranchCache en mode cache distribué et hébergé pour optimiser l’utilisation de la bande passante WAN dans les succursales
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 911c1538-f79d-42e9-ba38-f4618f87b008
ms.author: pashort
author: shortpatti
ms.date: 06/02/2018
ms.openlocfilehash: 8e82d3e0ee7a84fbd6e2916d0f22472a8c117688
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855080"
---
# <a name="use-group-policy-to-configure-domain-member-client-computers"></a>Utilisez la stratégie de groupe pour configurer les ordinateurs clients membres du domaine

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Dans cette section, vous créez un objet de stratégie de groupe pour tous les ordinateurs de votre organisation, configurez des ordinateurs clients membres du domaine avec le mode de cache distribué ou mode de cache hébergé et configurez les pare-feu Windows avec fonctions avancées de sécurité pour permettre de BranchCache trafic.  
  
Cette section contient les procédures suivantes.  
  
1.  [Pour créer un objet de stratégie de groupe et configurer des modes de BranchCache](#bkmk_gp)  
  
2.  [Pour configurer le pare-feu de Windows avec les avancées de sécurité entrante du règles de trafic](#bkmk_inbound)  
  
3.  [Pour configurer le pare-feu de Windows avec des règles de trafic sortant sécurité avancée](#bkmk_outbound)  
  
> [!TIP]  
> Dans la procédure suivante, vous êtes invité à créer un objet de stratégie de groupe dans la stratégie de domaine par défaut, toutefois, vous pouvez créer l’objet dans une unité d’organisation (UO) ou autre conteneur qui est approprié pour votre déploiement.  
  
Vous devez être membre du **Admins du domaine**, ou équivalent pour effectuer ces procédures.  
  
## <a name="bkmk_gp"></a>Pour créer un objet de stratégie de groupe et configurer des modes de BranchCache  
  
1.  Sur un ordinateur sur lequel est installé le rôle de serveur Services de domaine Active Directory, dans le Gestionnaire de serveur, cliquez sur **outils**, puis cliquez sur **Group Policy Management**. La console Gestion de stratégie de groupe s’ouvre.  
  
2.  Dans la console de gestion des stratégies de groupe, développez le chemin suivant : **Forêt :** *example.com*, **domaines**, *example.com*, **les objets de stratégie de groupe**, où  *example.com* est le nom du domaine où se trouvent les comptes d’ordinateur client BranchCache que vous souhaitez configurer.  
  
3.  Cliquez avec le bouton droit de la souris sur **Objets de stratégie de groupe** et puis cliquez sur **Nouveau**. La boîte de dialogue **Nouvel objet GPO** s'ouvre. Dans **nom**, tapez un nom pour l’objet de stratégie de groupe (GPO) la nouvelle. Par exemple, si vous voulez nommer l'objet Ordinateurs clients BranchCache, tapez **Ordinateurs clients BranchCache**. Cliquez sur **OK**.  
  
4.  Dans la console de gestion des stratégies de groupe, vérifiez que **Objets de stratégie de groupe** est sélectionné, et dans le volet d'informations, cliquez avec le bouton droit sur l'objet de stratégie de groupe que vous venez de créer. Par exemple, si vous avez nommé votre objet de stratégie de groupe Ordinateurs clients BranchCache, cliquez avec le bouton droit sur **Ordinateurs clients BranchCache**. Cliquez sur **Modifier**. La console de l’éditeur de gestion de stratégie de groupe s’ouvre.  
  
5.  Dans la console Éditeur de gestion de stratégie de groupe, développez le chemin suivant : **Configuration ordinateur**, **Stratégies**, **Modèles d'administration : Définitions de stratégies (fichiers ADMX) récupérées à partir de l’ordinateur local**, **réseau**, **BranchCache**.  
  
6.  Cliquez sur **BranchCache**, puis dans le volet d'informations, double-cliquez sur **Activer BranchCache**. La boîte de dialogue de paramètre de stratégie s’ouvre.  
  
7.  Dans la boîte de dialogue **Activer BranchCache**, cliquez sur **Activé**, puis sur **OK**.  
  
8.  Pour activer le mode de cache distribué de BranchCache, dans le volet de détails, double-cliquez sur **mode de Cache distribué de BranchCache définir**. La boîte de dialogue de paramètre de stratégie s’ouvre.  
  
9. Dans la boîte de dialogue **Définir le mode de cache distribué de BranchCache**, cliquez sur **Activé**, puis sur **OK**.  
  
10. Si vous avez un ou plusieurs sites distants où vous déployez BranchCache en mode cache hébergé, et que vous avez déployé hébergé dans ces bureaux, les serveurs de cache, double-cliquez sur **activer hébergé Cache la découverte automatique par Point de connexion de Service**. La boîte de dialogue de paramètre de stratégie s’ouvre.  
  
11. Dans le **activer hébergé Cache la découverte automatique par Point de connexion de Service** boîte de dialogue, cliquez sur **activé**, puis cliquez sur **OK**.  
  
    > [!NOTE]  
    > Lorsque vous activez les deux le **mode Cache distribué de BranchCache définir** et **activer hébergé Cache la découverte automatique par Point de connexion de Service** paramètres de stratégie, les ordinateurs clients fonctionnent dans BranchCache mode de cache distribué, sauf si elles trouver un serveur de cache hébergé de la succursale, auquel cas elles fonctionnent en mode cache hébergé.  
  
12. Utilisez les procédures ci-dessous pour configurer les paramètres de pare-feu sur les ordinateurs clients à l’aide de stratégie de groupe.  
  
## <a name="bkmk_inbound"></a>Pour configurer le pare-feu de Windows avec les avancées de sécurité entrante du règles de trafic  
  
1.  Dans la console de gestion des stratégies de groupe, développez le chemin suivant : **Forêt :** *example.com*, **domaines**, *example.com*, **les objets de stratégie de groupe**, où  *example.com* est le nom du domaine où se trouvent les comptes d’ordinateur client BranchCache que vous souhaitez configurer.  
  
2.  Dans la console de gestion des stratégies de groupe, vérifiez que **Objets de stratégie de groupe** est sélectionné, et dans le volet d'informations, cliquez avec le bouton droit sur l'objet de stratégie de groupe Ordinateurs clients BranchCache précédemment créé. Par exemple, si vous avez nommé votre objet de stratégie de groupe Ordinateurs clients BranchCache, cliquez avec le bouton droit sur **Ordinateurs clients BranchCache**. Cliquez sur **Modifier**. La console de l’éditeur de gestion de stratégie de groupe s’ouvre.  
  
3.  Dans la console Éditeur de gestion de stratégie de groupe, développez le chemin suivant : **Configuration de l’ordinateur**, **stratégies**, **Windows paramètres**, **paramètres de sécurité**, **pare-feu Windows avec fonctions avancées de sécurité**, **Pare-feu Windows avec fonctions avancées de sécurité - LDAP**, **règles de trafic entrant**.  
  
4.  Cliquez avec le bouton droit sur **Règles de trafic entrant**, puis cliquez sur **Nouvelle règle**. L’Assistant Nouvelle règle de trafic entrant s’ouvre.  
  
5.  Dans **Type de règle**, cliquez sur **prédéfini**, développez la liste de choix, puis cliquez sur **BranchCache - extraction du contenu (utilise HTTP)**. Cliquez sur **Suivant**.  
  
6.  Dans **Règles prédéfinies**, cliquez sur **Suivant**.  
  
7.  Dans **Action**, vérifiez que **Autoriser la connexion** est sélectionné, puis cliquez sur **Terminer**.  
  
    > [!IMPORTANT]  
    > Vous devez sélectionner **Autoriser la connexion** pour le client BranchCache afin d'être en mesure de recevoir le trafic sur ce port.  
  
8.  Pour créer l'exception de pare-feu WS-Discovery, cliquez avec le bouton droit sur **Exceptions entrantes**, puis cliquez sur **Nouvelle règle**. L’Assistant Nouvelle règle de trafic entrant s’ouvre.  
  
9. Dans **Type de règle**, cliquez sur **prédéfini**, développez la liste de choix, puis cliquez sur **BranchCache - découverte d’homologue (utilise WSD)**. Cliquez sur **Suivant**.  
  
10. Dans **Règles prédéfinies**, cliquez sur **Suivant**.  
  
11. Dans **Action**, vérifiez que **Autoriser la connexion** est sélectionné, puis cliquez sur **Terminer**.  
  
    > [!IMPORTANT]  
    > Vous devez sélectionner **Autoriser la connexion** pour le client BranchCache afin d'être en mesure de recevoir le trafic sur ce port.  
  
## <a name="bkmk_outbound"></a>Pour configurer le pare-feu de Windows avec des règles de trafic sortant sécurité avancée  
  
1.  Dans la console de l'Éditeur de gestion de stratégie de groupe, cliquez avec le bouton droit sur **Règles sortantes**, puis cliquez sur **Nouvelle règle**. L’Assistant Nouvelle règle de sortant s’ouvre.  
  
2.  Dans **Type de règle**, cliquez sur **prédéfini**, développez la liste de choix, puis cliquez sur **BranchCache - extraction du contenu (utilise HTTP)**. Cliquez sur **Suivant**.  
  
3.  Dans **Règles prédéfinies**, cliquez sur **Suivant**.  
  
4.  Dans **Action**, vérifiez que **Autoriser la connexion** est sélectionné, puis cliquez sur **Terminer**.  
  
    > [!IMPORTANT]  
    > Vous devez sélectionner **Autoriser la connexion** pour le client BranchCache afin d'être en mesure d'envoyer le trafic sur ce port.  
  
5.  Pour créer l'exception de pare-feu WS-Discovery, cliquez avec le bouton droit sur **Exceptions sortantes**, puis cliquez sur **Nouvelle règle**. L’Assistant Nouvelle règle de sortant s’ouvre.  
  
6.  Dans **Type de règle**, cliquez sur **prédéfini**, développez la liste de choix, puis cliquez sur **BranchCache - découverte d’homologue (utilise WSD)**. Cliquez sur **Suivant**.  
  
7.  Dans **Règles prédéfinies**, cliquez sur **Suivant**.  
  
8.  Dans **Action**, vérifiez que **Autoriser la connexion** est sélectionné, puis cliquez sur **Terminer**.  
  
    > [!IMPORTANT]  
    > Vous devez sélectionner **Autoriser la connexion** pour le client BranchCache afin d'être en mesure d'envoyer le trafic sur ce port.  
  


