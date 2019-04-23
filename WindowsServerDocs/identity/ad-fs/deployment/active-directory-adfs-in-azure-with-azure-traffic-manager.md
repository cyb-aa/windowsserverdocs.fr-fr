---
title: Déploiement des services haute disponibilité par-delà AD FS dans Azure avec Azure Traffic Manager | Microsoft Docs
description: Dans ce document, vous allez apprendre à déployer AD FS dans Azure pour la haute disponibilité.
keywords: Les services AD fs avec Azure traffic manager, adfs avec Azure Traffic Manager, géographique, centre de données multiples, centres de données géographiques, plusieurs centres de données géographiques, déploiement AD FS dans azure, déploiement azure adfs, azure adfs, azure ad fs, déploiement adfs, déploiement ad fs, adfs dans azure, déployer adfs dans azure, déployer AD FS dans azure, adfs azure, présentation d’AD FS, Azure, AD FS dans Azure, iaas, ADFS, déplacer adfs vers azure
services: active-directory
documentationcenter: ''
author: anandyadavmsft
manager: mtillman
editor: ''
ms.assetid: a14bc870-9fad-45ed-acd5-a90ccd432e54
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/01/2016
ms.author: anandy;billmath
ms.openlocfilehash: 4af0386ef97694baa9ba7f1e5e7163554d79734a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874120"
---
# <a name="high-availability-cross-geographic-ad-fs-deployment-in-azure-with-azure-traffic-manager"></a>Déploiement des services haute disponibilité par-delà AD FS dans Azure avec Azure Traffic Manager
[Déploiement d’AD FS dans Azure](how-to-connect-fed-azure-adfs.md) fournit des instructions détaillées sur la façon dont vous pouvez déployer une infrastructure AD FS simple pour votre organisation dans Azure. Cet article fournit les étapes suivantes pour créer un déploiement par-delà d’AD FS dans Azure à l’aide [Azure Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/). Azure Traffic Manager permet de créer une géographiquement spread haute disponibilité et une infrastructure AD FS de hautes performances pour votre organisation en rendant l’utilisation de différentes méthodes de routage disponibles en fonction des besoins de l’infrastructure.

Une infrastructure AD FS par-delà hautement disponible permet :

* **Élimination du point de défaillance unique :** Les fonctionnalités de basculement d’Azure Traffic Manager, vous pouvez obtenir une infrastructure AD FS hautement disponible même lorsqu’un des centres de données dans une partie du globe tombe en panne
* **Amélioration des performances :** Vous pouvez utiliser le déploiement suggéré dans cet article pour fournir une infrastructure AD FS hautes performances qui permettre aider les utilisateurs à s’authentifier plus rapidement. 

## <a name="design-principles"></a>Principes de conception
![Conception générale](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/blockdiagram.png)

Les principes de conception de base seront identiques comme indiqué dans les principes de conception dans l’article de déploiement d’AD FS dans Azure. Le diagramme ci-dessus présente une extension simple du déploiement de base vers une autre région géographique. Voici quelques points à prendre en compte lorsque vous étendez votre déploiement à la nouvelle région géographique

* **Réseau virtuel :** Vous devez créer un nouveau réseau virtuel dans la région géographique que vous souhaitez déployer une infrastructure AD FS supplémentaire. Dans le diagramme ci-dessus, vous voyez Geo1 VNET et Geo2 VNET en tant que les deux réseaux virtuels dans chaque région géographique.
* **Contrôleurs de domaine et serveurs AD FS dans le nouveau réseau virtuel géographique :** Il est recommandé de déployer des contrôleurs dans la nouvelle région géographique afin que les serveurs AD FS dans la nouvelle région n’aient pas à contacter un contrôleur de domaine dans un autre loin du réseau pour effectuer une authentification, ce qui améliore les performances de domaine.
* **Comptes de stockage :** Comptes de stockage sont associés à une région. Étant donné que vous déployez des machines dans une nouvelle région géographique, vous devrez créer de nouveaux comptes de stockage à utiliser dans la région.  
* **Groupes de sécurité réseau :** Comme les comptes de stockage, les groupes de sécurité réseau créés dans une région ne peut pas être utilisés dans une autre région géographique. Par conséquent, vous devrez créer nouveau réseau des groupes de sécurité semblables à celles de la première région géographique pour les sous-réseaux INT et DMZ dans la nouvelle région géographique.
* **Noms DNS pour les adresses IP publiques :** Azure Traffic Manager peut faire référence aux points de terminaison uniquement via des noms DNS. Par conséquent, vous devez créer des noms DNS pour les adresses IP publiques des équilibreurs de charge externe.
* **Azure Traffic Manager :** Microsoft Azure Traffic Manager vous permet de contrôler la distribution du trafic utilisateur vers vos points de terminaison de service en cours d’exécution dans différents centres de données dans le monde entier. Azure Traffic Manager fonctionne au niveau du DNS. Il utilise les réponses DNS pour diriger le trafic de l’utilisateur final aux points de terminaison globalement distribués. Les clients se connectent ensuite à ces points de terminaison directement. Avec les différentes options de routage performances, pondéré et priorité, vous pouvez facilement choisir l’option de routage adaptée aux besoins de votre organisation. 
* **Connectivité de V-net net V entre deux régions :** Il est inutile de disposer d’une connectivité entre les réseaux virtuels proprement dit. Étant donné que chaque réseau virtuel a accès aux contrôleurs de domaine et serveur AD FS et WAP en soi, il peut fonctionner sans aucune connectivité entre les réseaux virtuels dans différentes régions. 

## <a name="steps-to-integrate-azure-traffic-manager"></a>Procédure d’intégration d’Azure Traffic Manager
### <a name="deploy-ad-fs-in-the-new-geographical-region"></a>Déployer AD FS dans la nouvelle région géographique
Suivez les étapes et les instructions dans [déploiement AD FS dans Azure](how-to-connect-fed-azure-adfs.md) pour déployer la même topologie dans la nouvelle région géographique.

### <a name="dns-labels-for-public-ip-addresses-of-the-internet-facing-public-load-balancers"></a>Noms DNS des adresses IP publiques des équilibreurs de charge (public) accessible sur Internet
Comme mentionné ci-dessus, Azure Traffic Manager ne peut faire référence à des noms DNS en tant que points de terminaison et par conséquent, il est important de créer des noms DNS pour les adresses IP publiques des équilibreurs de charge externe. Capture d’écran ci-dessous montre comment vous pouvez configurer votre nom DNS pour l’adresse IP publique. 

![Étiquette DNS](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/eastfabstsdnslabel.png)

### <a name="deploying-azure-traffic-manager"></a>Déploiement d’Azure Traffic Manager
Suivez les étapes ci-dessous pour créer un profil traffic manager. Pour plus d’informations, vous pouvez également faire référence à [gestion d’un profil Azure Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-manage-profiles).

1. **Créer un profil Traffic Manager :** Donnez un nom unique à votre profil traffic manager. Ce nom du profil fait partie du nom DNS et sert de préfixe pour l’étiquette de nom de domaine Traffic Manager. Le nom / préfixe est ajouté à. trafficmanager.net pour créer un nom DNS pour le trafic de votre gestionnaire. La capture d’écran ci-dessous montre le Gestionnaire de trafic préfixe DNS défini comme mysts, et qui en résulte étiquette DNS sera mysts.trafficmanager.net. 
   
    ![Création du profil Traffic Manager](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/trafficmanager01.png)
2. **Méthode de routage du trafic :** Il existe trois options de routage disponibles dans traffic manager :
   
   * Priority 
   * Performances
   * Pondérée
     
     **Performances** est l’option recommandée pour atteindre des infrastructure AD FS hautement réactive. Toutefois, vous pouvez choisir n’importe quelle méthode de routage plus adaptée à vos besoins de déploiement. La fonctionnalité AD FS n’est pas affectée par l’option de routage sélectionnée. Consultez [méthodes de routage du trafic de Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-routing-methods) pour plus d’informations. Dans l’exemple de capture d’écran ci-dessus, vous pouvez voir le **performances** méthode sélectionnée.
3. **Configurer des points de terminaison :** Dans la page traffic manager, cliquez sur les points de terminaison et sélectionnez Ajouter. Ceci ouvrira une page Ajouter un point de terminaison similaire à la capture d’écran ci-dessous
   
   ![Configurer les points de terminaison](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/eastfsendpoint.png)
   
   Pour les différentes entrées, suivez les recommandations ci-dessous :
   
   **Type :** Sélectionnez le point de terminaison Azure que nous allons pointer vers une adresse IP publique Azure.
   
   **Nom :** Créer un nom que vous souhaitez associer le point de terminaison. Cela n’est pas le nom DNS et n’a aucune incidence sur les enregistrements DNS.
   
   **Type de ressource cible :** Sélectionnez l’adresse IP publique comme valeur à cette propriété. 
   
   **Ressource cible :** Cela vous donnera la possibilité de choisir entre les différentes étiquettes DNS, que vous disposez dans votre abonnement. Choisissez le nom DNS correspondant au point de terminaison que vous configurez.
   
   Ajouter un point de terminaison pour chaque région géographique que vous souhaitez que le Azure Traffic Manager pour acheminer le trafic vers.
   Pour plus d’informations et des instructions détaillées sur la façon d’ajouter / configurer des points de terminaison dans traffic manager, reportez-vous à [ajouter, désactiver, activer ou supprimer des points de terminaison](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-manage-endpoints)
4. **Configuration de la sonde :** Dans la page traffic manager, cliquez sur la Configuration. Dans la page de configuration, vous devez modifier les paramètres d’analyse pour détecter au port HTTP 80 et le chemin d’accès relatif /adfs/probe
   
    ![Configuration de la sonde](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/mystsconfig.png) 
   
   > [!NOTE]
   > **Vérifiez que l’état des points de terminaison est en ligne une fois que la configuration est terminée**. Si tous les points de terminaison sont dans un état « détérioré », Azure Traffic Manager effectuera une meilleure tentative pour acheminer le trafic en supposant que le diagnostic est incorrect et que tous les points de terminaison sont accessibles.
   > 
   > 
5. **Modification des enregistrements DNS pour Azure Traffic Manager :** Votre service de fédération doit être un enregistrement CNAME pour le nom DNS de Traffic Manager de Azure. Créer un enregistrement CNAME dans les enregistrements DNS publics afin que toute personne qui tente de contacter le service de fédération ait réellement atteint Azure Traffic Manager.
   
    Par exemple, pour faire pointer le fs.fabidentity.com de service de fédération à l’aide de Traffic Manager, vous devez mettre à jour votre enregistrement de ressource DNS comme suit :
   
    <code>fs.fabidentity.com IN CNAME mysts.trafficmanager.net</code>

## <a name="test-the-routing-and-ad-fs-sign-in"></a>Tester le routage et l’authentification dans AD FS
### <a name="routing-test"></a>Test de routage
Un test très basique pour le routage consisterait à essayer un test ping sur le nom DNS de service de fédération à partir d’un ordinateur dans chaque région géographique. Selon la méthode de routage choisie, le point de terminaison qu'il réellement effectue un test ping apparaîtront dans l’affichage ping. Par exemple, si vous avez sélectionné le routage de performances, le point de terminaison plus proche de la région du client est atteint. Voici la capture instantanée de deux tests ping à partir de deux ordinateurs clients de région différents, une dans la région Asie et une région ouest des États-Unis. 

![Test de routage](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/pingtest.png)

### <a name="ad-fs-sign-in-test"></a>AD FS connexion test
Le moyen le plus simple de tester AD FS est à l’aide de la page IdpInitiatedSignon.aspx. Afin d’être en mesure d’effectuer, il est nécessaire pour activer l’authentification IdpInitiatedSignOn sur les propriétés AD FS. Suivez les étapes ci-dessous pour vérifier votre configuration AD FS

1. Exécutez l’applet de commande sur le serveur AD FS, à l’aide de PowerShell, pour définir sur activé ci-dessous. 
   Set-AdfsProperties -EnableIdPInitiatedSignonPage $true
2. À partir de n’importe quel ordinateur externe accès https://<yourfederationservicedns>/adfs/ls/IdpInitiatedSignon.aspx
3. Vous devez voir la page AD FS comme ci-dessous :
   
    ![Test ADFS : demande d’authentification](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/adfstest1.png)
   
    et, lors de l’authentification réussie, il vous fournira un message de réussite comme indiqué ci-dessous :
   
    ![Test ADFS : authentification réussie](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/adfstest2.png)

## <a name="related-links"></a>Liens connexes
* [Base déploiement AD FS dans Azure](how-to-connect-fed-azure-adfs.md)
* [Microsoft Azure Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/)
* [Méthodes de routage du trafic Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-routing-methods)

## <a name="next-steps"></a>Étapes suivantes
* [Gestion d’un profil Azure Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-manage-profiles)
* [Ajouter, désactiver, activer ou supprimer des points de terminaison](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-manage-endpoints) 

