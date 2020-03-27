---
title: Utiliser stratégie de groupe pour configurer les ordinateurs clients membres du domaine
description: Cette rubrique fait partie du Guide de déploiement BranchCache pour Windows Server 2016, qui montre comment déployer BranchCache en mode de cache distribué et hébergé pour optimiser l’utilisation de la bande passante WAN dans les filiales.
manager: dougkim
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 911c1538-f79d-42e9-ba38-f4618f87b008
ms.author: lizross
author: eross-msft
ms.date: 06/02/2018
ms.openlocfilehash: c6ca1ff8fabb559628afd2dd1abafc56a908909a
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80319194"
---
# <a name="use-group-policy-to-configure-domain-member-client-computers"></a>Utiliser stratégie de groupe pour configurer les ordinateurs clients membres du domaine

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Dans cette section, vous créez un objet de stratégie de groupe pour tous les ordinateurs de votre organisation, vous configurez des ordinateurs clients membres du domaine avec le mode de cache distribué ou le mode de cache hébergé, puis vous configurez le pare-feu Windows avec fonctions avancées de sécurité pour autoriser BranchCache le trafic.  
  
Cette section contient les procédures suivantes.  
  
1.  [Pour créer un objet stratégie de groupe et configurer les modes BranchCache](#bkmk_gp)  
  
2.  [Pour configurer les règles de trafic entrant du pare-feu Windows avec fonctions avancées de sécurité](#bkmk_inbound)  
  
3.  [Pour configurer les règles de trafic sortant du pare-feu Windows avec fonctions avancées de sécurité](#bkmk_outbound)  
  
> [!TIP]  
> Dans la procédure suivante, vous êtes invité à créer un objet stratégie de groupe dans la stratégie de domaine par défaut. Toutefois, vous pouvez créer l’objet dans une unité d’organisation ou un autre conteneur approprié pour votre déploiement.  
  
Vous devez être membre du **groupe Admins du domaine**ou avoir l’équivalent d’effectuer ces procédures.  
  
## <a name="to-create-a-group-policy-object-and-configure-branchcache-modes"></a><a name="bkmk_gp"></a>Pour créer un objet stratégie de groupe et configurer les modes BranchCache  
  
1.  Sur un ordinateur sur lequel le rôle de serveur Active Directory Domain Services est installé, dans Gestionnaire de serveur, cliquez sur **Outils**, puis sur **gestion des stratégie de groupe**. La console de gestion stratégie de groupe s’ouvre.  
  
2.  Dans la console de gestion de stratégie de groupe, développez le chemin suivant : **Forest :** *example.com*, **Domains**, *example.com*, **stratégie de groupe Objects**, où *example.com* est le nom du domaine dans lequel se trouvent les comptes d’ordinateur client BranchCache que vous souhaitez configurer.  
  
3.  Cliquez avec le bouton droit de la souris sur **Objets de stratégie de groupe** et puis cliquez sur **Nouveau**. La boîte de dialogue **Nouvel objet GPO** s'ouvre. Dans **nom**, tapez un nom pour le nouvel objet de stratégie de groupe (GPO). Par exemple, si vous voulez nommer l'objet Ordinateurs clients BranchCache, tapez **Ordinateurs clients BranchCache**. Cliquez sur **OK**.  
  
4.  Dans la console de gestion des stratégies de groupe, vérifiez que **Objets de stratégie de groupe** est sélectionné, et dans le volet d'informations, cliquez avec le bouton droit sur l'objet de stratégie de groupe que vous venez de créer. Par exemple, si vous avez nommé votre objet de stratégie de groupe Ordinateurs clients BranchCache, cliquez avec le bouton droit sur **Ordinateurs clients BranchCache**. Cliquez sur **Modifier**. La console Éditeur de gestion des stratégies de groupe s’ouvre.  
  
5.  Dans la console Éditeur de gestion des stratégies de groupe, développez le chemin suivant : **Configuration ordinateur**, **stratégies**, **modèles d’administration : définitions de stratégie (fichiers ADMX) récupérées à partir de l’ordinateur local**, **réseau**, **BranchCache**.  
  
6.  Cliquez sur **BranchCache**, puis dans le volet d'informations, double-cliquez sur **Activer BranchCache**. La boîte de dialogue paramètre de stratégie s’ouvre.  
  
7.  Dans la boîte de dialogue **Activer BranchCache**, cliquez sur **Activé**, puis sur **OK**.  
  
8.  Pour activer le mode de cache distribué de BranchCache, dans le volet d’informations, double-cliquez sur **définir le mode de cache distribué de BranchCache**. La boîte de dialogue paramètre de stratégie s’ouvre.  
  
9. Dans la boîte de dialogue **Définir le mode de cache distribué de BranchCache**, cliquez sur **Activé**, puis sur **OK**.  
  
10. Si vous avez un ou plusieurs succursales où vous déployez BranchCache en mode de cache hébergé et que vous avez déployé des serveurs de cache hébergé dans ces bureaux, double-cliquez sur **activer la découverte automatique du cache hébergé par le point de connexion de service**. La boîte de dialogue paramètre de stratégie s’ouvre.  
  
11. Dans la boîte de dialogue **activer la découverte automatique du cache hébergé par le point de connexion de service** , cliquez sur **activé**, puis sur **OK**.  
  
    > [!NOTE]  
    > Lorsque vous activez les paramètres de stratégie **définir le mode de cache distribué de BranchCache** et activer la **découverte automatique du cache hébergé par le point de connexion de service** , les ordinateurs clients fonctionnent en mode de cache distribué BranchCache, sauf s’ils trouvent un serveur de cache hébergé dans la filiale, à partir duquel ils opèrent en mode de cache hébergé.  
  
12. Utilisez les procédures ci-dessous pour configurer les paramètres de pare-feu sur les ordinateurs clients à l’aide de stratégie de groupe.  
  
## <a name="to-configure-windows-firewall-with-advanced-security-inbound-traffic-rules"></a><a name="bkmk_inbound"></a>Pour configurer les règles de trafic entrant du pare-feu Windows avec fonctions avancées de sécurité  
  
1.  Dans la console de gestion de stratégie de groupe, développez le chemin suivant : **Forest :** *example.com*, **Domains**, *example.com*, **stratégie de groupe Objects**, où *example.com* est le nom du domaine dans lequel se trouvent les comptes d’ordinateur client BranchCache que vous souhaitez configurer.  
  
2.  Dans la console de gestion des stratégies de groupe, vérifiez que **Objets de stratégie de groupe** est sélectionné, et dans le volet d'informations, cliquez avec le bouton droit sur l'objet de stratégie de groupe Ordinateurs clients BranchCache précédemment créé. Par exemple, si vous avez nommé votre objet de stratégie de groupe Ordinateurs clients BranchCache, cliquez avec le bouton droit sur **Ordinateurs clients BranchCache**. Cliquez sur **Modifier**. La console Éditeur de gestion des stratégies de groupe s’ouvre.  
  
3.  Dans la console Éditeur de gestion des stratégies de groupe, développez le chemin suivant **: Configuration ordinateur**, **stratégies**, **Paramètres Windows**, **paramètres de sécurité**, **pare-feu Windows avec fonctions avancées de sécurité**, **pare-feu Windows avec fonctions avancées de sécurité-LDAP**, **règles de trafic entrant**.  
  
4.  Cliquez avec le bouton droit sur **Règles de trafic entrant**, puis cliquez sur **Nouvelle règle**. L’Assistant Nouvelle règle de trafic entrant s’ouvre.  
  
5.  Dans **type de règle**, cliquez sur **prédéfini**, développez la liste de choix, puis cliquez sur **BranchCache-récupération de contenu (utilise http)** . Cliquez sur **Suivant**.  
  
6.  Dans **Règles prédéfinies**, cliquez sur **Suivant**.  
  
7.  Dans **Action**, vérifiez que **Autoriser la connexion** est sélectionné, puis cliquez sur **Terminer**.  
  
    > [!IMPORTANT]  
    > Vous devez sélectionner **Autoriser la connexion** pour le client BranchCache afin d'être en mesure de recevoir le trafic sur ce port.  
  
8.  Pour créer l'exception de pare-feu WS-Discovery, cliquez avec le bouton droit sur **Exceptions entrantes**, puis cliquez sur **Nouvelle règle**. L’Assistant Nouvelle règle de trafic entrant s’ouvre.  
  
9. Dans **type de règle**, cliquez sur **prédéfini**, développez la liste de choix, puis cliquez sur **BranchCache-Découverte d’homologue (utilise WSD)** . Cliquez sur **Suivant**.  
  
10. Dans **Règles prédéfinies**, cliquez sur **Suivant**.  
  
11. Dans **Action**, vérifiez que **Autoriser la connexion** est sélectionné, puis cliquez sur **Terminer**.  
  
    > [!IMPORTANT]  
    > Vous devez sélectionner **Autoriser la connexion** pour le client BranchCache afin d'être en mesure de recevoir le trafic sur ce port.  
  
## <a name="to-configure-windows-firewall-with-advanced-security-outbound-traffic-rules"></a><a name="bkmk_outbound"></a>Pour configurer les règles de trafic sortant du pare-feu Windows avec fonctions avancées de sécurité  
  
1.  Dans la console de l'Éditeur de gestion de stratégie de groupe, cliquez avec le bouton droit sur **Règles sortantes**, puis cliquez sur **Nouvelle règle**. L’Assistant Nouvelle règle de trafic sortant s’ouvre.  
  
2.  Dans **type de règle**, cliquez sur **prédéfini**, développez la liste de choix, puis cliquez sur **BranchCache-récupération de contenu (utilise http)** . Cliquez sur **Suivant**.  
  
3.  Dans **Règles prédéfinies**, cliquez sur **Suivant**.  
  
4.  Dans **Action**, vérifiez que **Autoriser la connexion** est sélectionné, puis cliquez sur **Terminer**.  
  
    > [!IMPORTANT]  
    > Vous devez sélectionner **Autoriser la connexion** pour le client BranchCache afin d'être en mesure d'envoyer le trafic sur ce port.  
  
5.  Pour créer l'exception de pare-feu WS-Discovery, cliquez avec le bouton droit sur **Exceptions sortantes**, puis cliquez sur **Nouvelle règle**. L’Assistant Nouvelle règle de trafic sortant s’ouvre.  
  
6.  Dans **type de règle**, cliquez sur **prédéfini**, développez la liste de choix, puis cliquez sur **BranchCache-Découverte d’homologue (utilise WSD)** . Cliquez sur **Suivant**.  
  
7.  Dans **Règles prédéfinies**, cliquez sur **Suivant**.  
  
8.  Dans **Action**, vérifiez que **Autoriser la connexion** est sélectionné, puis cliquez sur **Terminer**.  
  
    > [!IMPORTANT]  
    > Vous devez sélectionner **Autoriser la connexion** pour le client BranchCache afin d'être en mesure d'envoyer le trafic sur ce port.  
  


