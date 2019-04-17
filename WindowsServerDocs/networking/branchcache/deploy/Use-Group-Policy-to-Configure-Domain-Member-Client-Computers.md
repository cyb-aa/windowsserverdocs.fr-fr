---
title: Utilisez la stratégie de groupe pour configurer les ordinateurs clients membres du domaine
description: Cette rubrique fait partie de la BranchCache déploiement Guide pour Windows Server2016, qui montre comment déployer BranchCache en mode de cache distribué et hébergé d’optimiser l’utilisation de la bande passante réseau étendu dans les filiales
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 911c1538-f79d-42e9-ba38-f4618f87b008
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 96bf84df14eac8016a898e92eb96f90435c53c98
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="use-group-policy-to-configure-domain-member-client-computers"></a>Utilisez la stratégie de groupe pour configurer les ordinateurs clients membres du domaine

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser ces procédures pour créer un objet de stratégie de groupe pour tous les ordinateurs de votre organisation, pour configurer des ordinateurs clients membres du domaine avec le mode de cache distribué ou en mode de cache hébergé et pour configurer le pare-feu Windows avec fonctions avancées de sécurité pour autoriser le trafic BranchCache.  
  
Cette section contient les procédures suivantes.  
  
1.  [Pour créer un objet de stratégie de groupe et de configurer les modes BranchCache](#bkmk_gp)  
  
2.  [Pour configurer le pare-feu Windows avec sécurité avancée entrant de trafic règles](#bkmk_inbound)  
  
3.  [Pour configurer le pare-feu Windows avec les règles de trafic sortant sécurité avancée](#bkmk_outbound)  
  
> [!TIP]  
> Dans la procédure suivante, vous êtes invité à créer un objet de stratégie de groupe dans la stratégie de domaine par défaut, cependant vous pouvez créer l’objet dans une unité d’organisation (UO) ou un autre conteneur qui est appropriée pour votre déploiement.  
  
Vous devez être membre du **Admins du domaine**, ou équivalent pour effectuer ces procédures.  
  
## <a name="bkmk_gp"></a>Pour créer un objet de stratégie de groupe et de configurer les modes BranchCache  
  
1.  Sur un ordinateur sur lequel est installé le rôle de serveur Services de domaine Active Directory, dans le Gestionnaire de serveur, cliquez sur **outils**, puis cliquez sur **gestion des stratégies de groupe**. La console de gestion de stratégie de groupe s’ouvre.  
  
2.  Dans la console de gestion des stratégies de groupe, développez le chemin suivant: **forêt:** *example.com*, **domaines**, *example.com*, **objets de stratégie de groupe**, où *example.com* est le nom du domaine où se trouvent les comptes d’ordinateur client BranchCache que vous souhaitez configurer.  
  
3.  Avec le bouton droit **objets de stratégie de groupe**, puis cliquez sur **New**. Le **nouvel objet GPO** boîte de dialogue s’ouvre. Dans **nom**, tapez un nom pour l’objet de stratégie de groupe (GPO) la nouvelle. Par exemple, si vous souhaitez nommer l’objet les ordinateurs clients BranchCache, tapez **les ordinateurs clients BranchCache**. Cliquez sur **OK**.  
  
4.  Dans la console Gestion de stratégie de groupe, vérifiez que **objets de stratégie de groupe** est sélectionné et dans le volet d’informations, cliquez sur l’objet de stratégie de groupe que vous venez de créer. Par exemple, si vous avez nommé votre objet stratégie de groupe les ordinateurs clients BranchCache, avec le bouton droit **les ordinateurs clients BranchCache**. Cliquez sur **modifier**. La console de l’éditeur de gestion de stratégie de groupe s’ouvre.  
  
5.  Dans la console Éditeur de gestion de stratégie de groupe, développez le chemin suivant: **Configuration ordinateur**, **stratégies**, **modèles d’administration: définitions de stratégies (fichiers ADMX) récupérées à partir de l’ordinateur local**, **réseau**, **BranchCache**.  
  
6.  Cliquez sur **BranchCache**, puis, dans le volet d’informations, double-cliquez sur **activer BranchCache**. La boîte de dialogue de paramètre de stratégie s’ouvre.  
  
7.  Dans le **activer BranchCache** boîte de dialogue, cliquez sur **activé**, puis cliquez sur **OK**.  
  
8.  Pour activer le mode de cache distribué de BranchCache, dans le volet d’informations, double-cliquez sur **mode de Cache distribué de BranchCache définir**. La boîte de dialogue de paramètre de stratégie s’ouvre.  
  
9. Dans le **en mode de Cache distribué de BranchCache définir** boîte de dialogue, cliquez sur **activé**, puis cliquez sur **OK**.  
  
10. Si vous disposez d’un ou plusieurs sites distants où vous déployez BranchCache en mode de cache hébergé, et que vous avez déployé hébergé dans les filiales, les serveurs de cache, double-cliquez sur **activer hébergé Cache de la découverte automatique par le Point de connexion de Service**. La boîte de dialogue de paramètre de stratégie s’ouvre.  
  
11. Dans le **activer hébergé Cache de la découverte automatique par le Point de connexion de Service** boîte de dialogue, cliquez sur **activé**, puis cliquez sur **OK**.  
  
    > [!NOTE]  
    > Lorsque vous activez les deux la **mode de Cache distribué de BranchCache définir** et **activer hébergé Cache de la découverte automatique par le Point de connexion de Service** paramètres de stratégie, les ordinateurs clients fonctionnent en mode de cache distribué de BranchCache, sauf si elles trouver un serveur de cache hébergé dans la filiale, à partir de laquelle elles fonctionnent en mode de cache hébergé.  
  
12. Utilisez les procédures ci-dessous pour configurer les paramètres de pare-feu sur les ordinateurs clients à l’aide de stratégie de groupe.  
  
## <a name="bkmk_inbound"></a>Pour configurer le pare-feu Windows avec sécurité avancée entrant de trafic règles  
  
1.  Dans la console de gestion des stratégies de groupe, développez le chemin suivant: **forêt:** *example.com*, **domaines**, *example.com*, **objets de stratégie de groupe**, où *example.com* est le nom du domaine où se trouvent les comptes d’ordinateur client BranchCache que vous souhaitez configurer.  
  
2.  Dans la console Gestion de stratégie de groupe, vérifiez que **objets de stratégie de groupe** est sélectionné et dans le volet d’informations, cliquez sur les ordinateurs clients BranchCache GPO que vous avez créé précédemment. Par exemple, si vous avez nommé votre objet stratégie de groupe les ordinateurs clients BranchCache, avec le bouton droit **les ordinateurs clients BranchCache**. Cliquez sur **modifier**. La console de l’éditeur de gestion de stratégie de groupe s’ouvre.  
  
3.  Dans la console Éditeur de gestion de stratégie de groupe, développez le chemin suivant: **Configuration ordinateur**, **stratégies**, **paramètres Windows**, **paramètres de sécurité**, **pare-feu Windows avec fonctions avancées de sécurité**, **pare-feu Windows avec fonctions avancées de sécurité - LDAP**, **règles de trafic entrant**.  
  
4.  Avec le bouton droit **règles de trafic entrant**, puis cliquez sur **nouvelle règle**. L’Assistant Nouvelle règle de trafic entrant s’ouvre.  
  
5.  Dans **Type de règle**, cliquez sur **prédéfini**, développez la liste de choix, puis cliquez sur **BranchCache - extraction du contenu (utilise HTTP)**. Cliquez sur **suivant**.  
  
6.  Dans **règles prédéfinies**, cliquez sur **suivant**.  
  
7.  Dans **Action**, assurez-vous que **autoriser la connexion** est sélectionné, puis cliquez sur **Terminer**.  
  
    > [!IMPORTANT]  
    > Vous devez sélectionner **autoriser la connexion** pour le client BranchCache afin d’être en mesure de recevoir le trafic sur ce port.  
  
8.  Pour créer l’exception de pare-feu WS-Discovery, cliquez à nouveau sur **règles de trafic entrant**, puis cliquez sur **nouvelle règle**. L’Assistant Nouvelle règle de trafic entrant s’ouvre.  
  
9. Dans **Type de règle**, cliquez sur **prédéfini**, développez la liste de choix, puis cliquez sur **BranchCache - découverte d’homologue (utilise WSD)**. Cliquez sur **suivant**.  
  
10. Dans **règles prédéfinies**, cliquez sur **suivant**.  
  
11. Dans **Action**, assurez-vous que **autoriser la connexion** est sélectionné, puis cliquez sur **Terminer**.  
  
    > [!IMPORTANT]  
    > Vous devez sélectionner **autoriser la connexion** pour le client BranchCache afin d’être en mesure de recevoir le trafic sur ce port.  
  
## <a name="bkmk_outbound"></a>Pour configurer le pare-feu Windows avec les règles de trafic sortant sécurité avancée  
  
1.  Dans la console Éditeur de gestion de stratégie de groupe, cliquez sur **règles de trafic sortant**, puis cliquez sur **nouvelle règle**. L’Assistant Nouvelle règle de trafic sortant s’ouvre.  
  
2.  Dans **Type de règle**, cliquez sur **prédéfini**, développez la liste de choix, puis cliquez sur **BranchCache - extraction du contenu (utilise HTTP)**. Cliquez sur **suivant**.  
  
3.  Dans **règles prédéfinies**, cliquez sur **suivant**.  
  
4.  Dans **Action**, assurez-vous que **autoriser la connexion** est sélectionné, puis cliquez sur **Terminer**.  
  
    > [!IMPORTANT]  
    > Vous devez sélectionner **autoriser la connexion** pour le client BranchCache afin d’être en mesure d’envoyer le trafic sur ce port.  
  
5.  Pour créer l’exception de pare-feu WS-Discovery, cliquez à nouveau sur **règles de trafic sortant**, puis cliquez sur **nouvelle règle**. L’Assistant Nouvelle règle de trafic sortant s’ouvre.  
  
6.  Dans **Type de règle**, cliquez sur **prédéfini**, développez la liste de choix, puis cliquez sur **BranchCache - découverte d’homologue (utilise WSD)**. Cliquez sur **suivant**.  
  
7.  Dans **règles prédéfinies**, cliquez sur **suivant**.  
  
8.  Dans **Action**, assurez-vous que **autoriser la connexion** est sélectionné, puis cliquez sur **Terminer**.  
  
    > [!IMPORTANT]  
    > Vous devez sélectionner **autoriser la connexion** pour le client BranchCache afin d’être en mesure d’envoyer le trafic sur ce port.  
  


