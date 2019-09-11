---
title: Déploiement des AD FS inter-géographiques à haute disponibilité dans Azure avec Azure Traffic Manager | Microsoft Docs
description: Dans ce document, vous allez apprendre à déployer AD FS dans Azure pour une haute disponibilité.
keywords: AD FS avec Azure Traffic Manager, ADFS avec Azure Traffic Manager, géographique, plusieurs centres de donnés, centres de donnés géographiques, centres de donnés géographiques, déployer des AD FS dans Azure, déployer Azure ADFS, Azure ADFS, Azure AD FS, déployer ADFS, déployer AD FS, ADFS dans Azure, déployer ADFS dans Azure, déployer des AD FS dans Azure, ADFS Azure, introduction à AD FS, Azure, AD FS dans Azure, IaaS, ADFS, déplacer ADFS vers Azure
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
ms.openlocfilehash: d98eb126513d707bce7abe3e901c8bf584d2319c
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70868014"
---
# <a name="high-availability-cross-geographic-ad-fs-deployment-in-azure-with-azure-traffic-manager"></a>Déploiement des AD FS inter-géographiques à haute disponibilité dans Azure avec Azure Traffic Manager
[AD FS déploiement dans Azure](how-to-connect-fed-azure-adfs.md) fournit des instructions pas à pas sur la façon dont vous pouvez déployer une infrastructure de AD FS simple pour votre organisation dans Azure. Cet article décrit les étapes suivantes pour créer un déploiement géographique de AD FS dans Azure à l’aide d' [azure Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/). Azure Traffic Manager permet de créer une infrastructure de haute disponibilité et de haute AD FS performance distribuée géographiquement pour votre organisation en utilisant un éventail de méthodes de routage disponibles pour répondre à différents besoins de l’infrastructure.

Une infrastructure de AD FS croisée hautement disponible permet les opérations suivantes :

* **Élimination du point de défaillance unique :** Avec les fonctionnalités de basculement d’Azure Traffic Manager, vous pouvez obtenir une infrastructure AD FS hautement disponible même si l’un des centres de données dans une partie du monde tombe en panne
* **Amélioration des performances :** Vous pouvez utiliser le déploiement suggéré dans cet article pour fournir une infrastructure de AD FS hautes performances qui peut aider les utilisateurs à s’authentifier plus rapidement. 

## <a name="design-principles"></a>Principes de conception
![Conception globale](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/blockdiagram.png)

Les principes de conception de base sont les mêmes que ceux décrits dans les principes de conception de l’article AD FS le déploiement dans Azure. Le diagramme ci-dessus illustre une extension simple du déploiement de base vers une autre région géographique. Voici quelques points à prendre en compte lors de l’extension de votre déploiement vers une nouvelle région géographique

* **Réseau virtuel :** Vous devez créer un réseau virtuel dans la région géographique dans laquelle vous souhaitez déployer des AD FS infrastructure supplémentaires. Dans le diagramme ci-dessus, vous voyez GEO1 VNET et GeO2 VNET comme deux réseaux virtuels dans chaque région géographique.
* **Contrôleurs de domaine et serveurs de AD FS dans un nouveau réseau virtuel géographique :** Il est recommandé de déployer des contrôleurs de domaine dans la nouvelle région géographique afin que les serveurs AD FS de la nouvelle région n’aient pas à contacter un contrôleur de domaine à un autre réseau éloigné pour effectuer une authentification et ainsi améliorer les performances.
* **Comptes de stockage :** Les comptes de stockage sont associés à une région. Étant donné que vous allez déployer des machines dans une nouvelle région géographique, vous devrez créer de nouveaux comptes de stockage à utiliser dans la région.  
* **Groupes de sécurité réseau :** Comme les comptes de stockage, les groupes de sécurité réseau créés dans une région ne peuvent pas être utilisés dans une autre région géographique. Par conséquent, vous devrez créer des groupes de sécurité réseau similaires à ceux de la première région géographique pour les sous-réseaux INT et DMZ dans la nouvelle région géographique.
* **Étiquettes DNS pour les adresses IP publiques :** Azure Traffic Manager peut faire référence à des points de terminaison uniquement par le biais d’étiquettes DNS. Par conséquent, vous devez créer des noms DNS pour les adresses IP publiques des équilibreurs de charge externes.
* **Traffic Manager Azure :** Microsoft Azure Traffic Manager vous permet de contrôler la distribution du trafic utilisateur vers vos points de terminaison de service exécutés dans différents centres de donnes dans le monde entier. Azure Traffic Manager fonctionne au niveau du DNS. Il utilise les réponses DNS pour diriger le trafic de l’utilisateur final vers les points de terminaison distribués globalement. Les clients se connectent ensuite directement à ces points de terminaison. Avec différentes options de routage de performances, pondérées et prioritaires, vous pouvez facilement choisir l’option de routage la mieux adaptée aux besoins de votre organisation. 
* **Connectivité v-net à V-net entre deux régions :** Vous n’avez pas besoin d’une connectivité entre les réseaux virtuels eux-mêmes. Étant donné que chaque réseau virtuel a accès aux contrôleurs de domaine et qu’il dispose de AD FS et d’un serveur WAP en soi, il peut fonctionner sans aucune connectivité entre les réseaux virtuels dans des régions différentes. 

## <a name="steps-to-integrate-azure-traffic-manager"></a>Étapes d’intégration d’Azure Traffic Manager
### <a name="deploy-ad-fs-in-the-new-geographical-region"></a>Déployer AD FS dans la nouvelle région géographique
Suivez les étapes et les instructions de [AD FS déploiement dans Azure](how-to-connect-fed-azure-adfs.md) pour déployer la même topologie dans la nouvelle région géographique.

### <a name="dns-labels-for-public-ip-addresses-of-the-internet-facing-public-load-balancers"></a>Étiquettes DNS pour les adresses IP publiques des équilibreurs de charge accessibles sur Internet (publics)
Comme indiqué ci-dessus, les Traffic Manager Azure peuvent uniquement faire référence à des étiquettes DNS en tant que points de terminaison. par conséquent, il est important de créer des noms DNS pour les adresses IP publiques des équilibreurs de charge externes. La capture d’écran ci-dessous montre comment vous pouvez configurer votre nom DNS pour l’adresse IP publique. 

![Nom DNS](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/eastfabstsdnslabel.png)

### <a name="deploying-azure-traffic-manager"></a>Déploiement d’Azure Traffic Manager
Suivez les étapes ci-dessous pour créer un profil Traffic Manager. Pour plus d’informations, vous pouvez également vous reporter à la rubrique [gestion d’un profil de Traffic Manager Azure](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-manage-profiles).

1. **Créez un profil de Traffic Manager :** Donnez un nom unique à votre profil Traffic Manager. Ce nom du profil fait partie du nom DNS et joue le rôle de préfixe pour l’étiquette de nom de domaine Traffic Manager. Le nom/préfixe est ajouté à. trafficmanager.net pour créer un nom DNS pour votre Traffic Manager. La capture d’écran ci-dessous montre le préfixe DNS Traffic Manager défini en tant que Mysts et l’étiquette DNS qui en résulte est mysts.trafficmanager.net. 
   
    ![Création d’un profil de Traffic Manager](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/trafficmanager01.png)
2. **Méthode de routage du trafic :** Il existe trois options de routage disponibles dans Traffic Manager :
   
   * Priority 
   * Performances
   * Pondérée
     
     Les **performances** sont l’option recommandée pour obtenir une infrastructure AD FS hautement réactive. Toutefois, vous pouvez choisir n’importe quelle méthode de routage la mieux adaptée à vos besoins de déploiement. La fonctionnalité de AD FS n’est pas affectée par l’option de routage sélectionnée. Pour plus d’informations, consultez [Traffic Manager des méthodes de routage du trafic](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-routing-methods) . Dans l’exemple de capture d’écran ci-dessus, vous pouvez voir la méthode de **performance** sélectionnée.
3. **Configurer des points de terminaison :** Dans la page Traffic Manager, cliquez sur points de terminaison, puis sélectionnez Ajouter. Une page Ajouter un point de terminaison similaire à la capture d’écran ci-dessous s’ouvre.
   
   ![Configuration des points de terminaison](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/eastfsendpoint.png)
   
   Pour les différentes entrées, suivez les instructions ci-dessous :
   
   **Type :** Sélectionnez point de terminaison Azure, car nous allons pointer vers une adresse IP publique Azure.
   
   **Nom :** Créez un nom que vous souhaitez associer au point de terminaison. Il ne s’agit pas du nom DNS et il n’a aucun impact sur les enregistrements DNS.
   
   **Type de ressource cible :** Sélectionnez adresse IP publique comme valeur pour cette propriété. 
   
   **Ressource cible :** Cela vous donne la possibilité de choisir parmi les différents noms DNS disponibles dans votre abonnement. Choisissez l’étiquette DNS correspondant au point de terminaison que vous configurez.
   
   Ajoutez un point de terminaison pour chaque région géographique pour laquelle vous souhaitez que le Traffic Manager Azure achemine le trafic.
   Pour plus d’informations et pour obtenir des instructions détaillées sur la façon d’ajouter/configurer des points de terminaison dans Traffic Manager, consultez [Ajouter, désactiver, activer ou supprimer des points de terminaison](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-manage-endpoints) .
4. **Configurer la sonde :** Dans la page Traffic Manager, cliquez sur Configuration. Dans la page Configuration, vous devez modifier les paramètres d’analyse pour sonder sur le port HTTP 80 et le chemin d’accès relatif/ADFS/Probe
   
    ![Configurer la sonde](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/mystsconfig.png) 
   
   > [!NOTE]
   > **Assurez-vous que l’état des points de terminaison est en ligne une fois la configuration terminée**. Si tous les points de terminaison sont dans l’État « détérioré », Azure Traffic Manager effectue une meilleure tentative pour acheminer le trafic en supposant que les diagnostics sont incorrects et que tous les points de terminaison sont accessibles.
   > 
   > 
5. **Modification des enregistrements DNS pour Azure Traffic Manager :** Votre service de Fédération doit être un enregistrement CNAME pour le nom DNS Azure Traffic Manager. Créez un enregistrement CNAME dans les enregistrements DNS publics afin que quiconque tente d’accéder au service de Fédération atteigne réellement le Traffic Manager Azure.
   
    Par exemple, pour faire pointer le service de Fédération fs.fabidentity.com vers le Traffic Manager, vous devez mettre à jour votre enregistrement de ressource DNS comme suit :
   
    <code>fs.fabidentity.com IN CNAME mysts.trafficmanager.net</code>

## <a name="test-the-routing-and-ad-fs-sign-in"></a>Tester le routage et la connexion AD FS
### <a name="routing-test"></a>Test de routage
Un test de base pour le routage consiste à essayer d’exécuter une commande ping sur le nom DNS du service de Fédération à partir d’un ordinateur de chaque région géographique. Selon la méthode de routage choisie, le point de terminaison en fait les pings est reflété dans l’affichage du ping. Par exemple, si vous avez sélectionné le routage des performances, le point de terminaison le plus proche de la région du client sera atteint. Voici la capture instantanée de deux pings de deux machines clientes de région, une dans la région EastAsia et une autre dans l’ouest des États-Unis. 

![Test de routage](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/pingtest.png)

### <a name="ad-fs-sign-in-test"></a>Test de connexion AD FS
Le moyen le plus simple de tester AD FS consiste à utiliser la page IdpInitiatedSignon. aspx. Pour pouvoir effectuer cette opération, vous devez activer IdpInitiatedSignOn sur les propriétés de AD FS. Suivez les étapes ci-dessous pour vérifier la configuration de votre AD FS

1. Exécutez l’applet de commande ci-dessous sur le serveur AD FS, à l’aide de PowerShell, pour lui affecter la valeur activé. 
   Set-AdfsProperties-EnableIdPInitiatedSignonPage $true
2. À partir de n’importe quel<yourfederationservicedns>https://d’accès à l’ordinateur externe/ADFS/LS/IdpInitiatedSignon.aspx
3. Vous devez voir la page AD FS comme ci-dessous :
   
    ![Test ADFS-demande d’authentification](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/adfstest1.png)
   
    en cas de réussite de la connexion, un message de réussite s’affiche, comme indiqué ci-dessous :
   
    ![Test ADFS-réussite de l’authentification](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/adfstest2.png)

## <a name="related-links"></a>Liens connexes
* [Déploiement de base AD FS dans Azure](how-to-connect-fed-azure-adfs.md)
* [Microsoft Azure Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/)
* [Traffic Manager les méthodes de routage du trafic](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-routing-methods)

## <a name="next-steps"></a>Étapes suivantes
* [Gérer un profil de Traffic Manager Azure](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-manage-profiles)
* [Ajouter, désactiver, activer ou supprimer des points de terminaison](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-manage-endpoints) 

