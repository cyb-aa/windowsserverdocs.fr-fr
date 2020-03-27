---
title: Configuration requise pour le déploiement du contrôleur de réseau
description: Préparez votre centre de informations pour le déploiement de contrôleur de réseau, qui nécessite un ou plusieurs ordinateurs ou machines virtuelles et un ordinateur ou une machine virtuelle. Avant de pouvoir déployer le contrôleur de réseau, vous devez configurer les groupes de sécurité, les emplacements des fichiers journaux (si nécessaire) et l’inscription DNS dynamique.
manager: dougkim
ms.prod: windows-server
ms.technology: networking-sdn
ms.topic: get-started-article
ms.assetid: 7f899e62-6e5b-4fca-9a59-130d4766ee2f
ms.author: lizross
author: eross-msft
ms.date: 08/10/2018
ms.openlocfilehash: a16d82e4db1e92a5dd20f6b4feb88f0619d50cc4
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80317512"
---
# <a name="requirements-for-deploying-network-controller"></a>Configuration requise pour le déploiement du contrôleur de réseau

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Préparez votre centre de informations pour le déploiement de contrôleur de réseau, qui nécessite un ou plusieurs ordinateurs ou machines virtuelles et un ordinateur ou une machine virtuelle. Avant de pouvoir déployer le contrôleur de réseau, vous devez configurer les groupes de sécurité, les emplacements des fichiers journaux (si nécessaire) et l’inscription DNS dynamique.


## <a name="network-controller-requirements"></a>Configuration requise pour le contrôleur de réseau

Le déploiement du contrôleur de réseau nécessite un ou plusieurs ordinateurs ou machines virtuelles qui servent de contrôleur de réseau, et un ordinateur ou une machine virtuelle pour servir de client de gestion pour le contrôleur de réseau. 

- Toutes les machines virtuelles et tous les ordinateurs planifiés en tant que nœuds de contrôleur de réseau doivent exécuter Windows Server 2016 Datacenter Edition. 
- Tout ordinateur ou ordinateur virtuel sur lequel vous installez le contrôleur de réseau doit exécuter l’édition Datacenter de Windows Server 2016. 
- L’ordinateur client de gestion ou la machine virtuelle pour le contrôleur de réseau doit exécuter Windows 10. 


## <a name="configuration-requirements"></a>Configuration requise

Avant de déployer le contrôleur de réseau, vous devez configurer les groupes de sécurité, les emplacements des fichiers journaux (si nécessaire) et l’inscription DNS dynamique.

### <a name="step-1-configure-your-security-groups"></a>Étape 1. Configurer vos groupes de sécurité

La première chose à faire est de créer deux groupes de sécurité pour l’authentification Kerberos. 

Vous créez des groupes pour les utilisateurs qui ont l’autorisation d’effectuer les opérations suivantes : 

1. Configurer le contrôleur de réseau<p>Vous pouvez nommer ce groupe administrateurs du contrôleur de réseau, par exemple. 
2.  Configurer et gérer le réseau à l’aide du contrôleur de réseau<p>Vous pouvez nommer ce groupe utilisateurs du contrôleur de réseau, par exemple. Utilisez REST pour configurer et gérer le contrôleur de réseau.

>[!NOTE]
>Tous les utilisateurs que vous ajoutez doivent être membres du groupe utilisateurs du domaine dans Active Directory utilisateurs et ordinateurs.

### <a name="step-2-configure-log-file-locations-if-needed"></a>Étape 2. Configurer des emplacements de fichiers journaux si nécessaire

La prochaine chose à faire est de configurer les emplacements de fichiers pour stocker les journaux de débogage du contrôleur de réseau, soit sur l’ordinateur du contrôleur de réseau, soit sur une machine virtuelle ou sur un partage de fichiers distant. 

>[!NOTE]
>Si vous stockez les journaux dans un partage de fichiers distant, assurez-vous que le partage est accessible à partir du contrôleur de réseau.


### <a name="step-3-configure-dynamic-dns-registration-for-network-controller"></a>Étape 3. Configurer l’inscription DNS dynamique pour le contrôleur de réseau

Enfin, la prochaine chose à faire est de déployer des nœuds de cluster de contrôleur de réseau sur le même sous-réseau ou sur des sous-réseaux différents. 


|         Condition         |                                                                                                                                                         Conséquence                                                                                                                                                         |
|-----------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  Sur le même sous-réseau,  |                                                                                                                                Vous devez fournir l’adresse IP REST du contrôleur de réseau.                                                                                                                                 |
| Sur des sous-réseaux différents, | Vous devez fournir le nom DNS REST du contrôleur de réseau, que vous créez pendant le processus de déploiement. Vous devez également effectuer les opérations suivantes :<ul><li>Configurez des mises à jour dynamiques DNS pour le nom DNS du contrôleur de réseau sur le serveur DNS.</li><li>Limitez les mises à jour dynamiques DNS aux nœuds de contrôleur de réseau uniquement.</li></ul> |

---

> [!NOTE]
> Pour exécuter ces procédures, il est nécessaire d’appartenir au minimum au **groupe Admins du domaine**ou à un groupe équivalent.

1. Autorisez les mises à jour dynamiques DNS pour une zone.

   a. Ouvrez le Gestionnaire DNS et, dans l’arborescence de la console, cliquez avec le bouton droit sur la zone correspondante, puis cliquez sur **Propriétés**. 

   b. Sous l’onglet **général** , vérifiez que le type de zone est **principal** ou **intégré à Active Directory**.

   c. Dans **mises à jour dynamiques**, vérifiez que **sécurisé uniquement** est sélectionné, puis cliquez sur **OK**.

2. Configurer les autorisations de sécurité de zone DNS pour les nœuds de contrôleur de réseau

   a.  Cliquez sur l'onglet **Sécurité**, puis sur **Avancé**. 

   b. Dans **paramètres de sécurité avancés**, cliquez sur **Ajouter**. 

   c. Cliquez sur **Sélectionnez un principal**. 

   d. Dans la boîte de dialogue **Sélectionner un utilisateur, un ordinateur, un compte de service ou un groupe** , cliquez sur **types d’objets**. 

   e. Dans **types d’objets**, sélectionnez **ordinateurs**, puis cliquez sur **OK**.

   f. Dans la boîte de dialogue **Sélectionner un utilisateur, un ordinateur, un compte de service ou un groupe** , tapez le nom NetBIOS de l’un des nœuds du contrôleur de réseau dans votre déploiement, puis cliquez sur **OK**.

   g. Dans **entrée d’autorisation**, vérifiez les valeurs suivantes :

      - **Type** = autoriser
      - **S’applique à** = cet objet et tous les objets descendants

   h. Dans **autorisations**, sélectionnez **écrire toutes les propriétés** et **supprimer**, puis cliquez sur **OK**.

3. Répétez cette opération pour tous les ordinateurs et toutes les machines virtuelles du cluster de contrôleur de réseau.

### <a name="step-4-configure-service-principal-name-if-using-kerberos-based-authentication"></a>Étape 4. Configurer le nom de principal du service si vous utilisez l’authentification Kerberos

Si le contrôleur de réseau utilise l’authentification Kerberos pour la communication avec les clients de gestion, vous devez configurer un nom de principal du service (SPN) pour le contrôleur de réseau dans Active Directory. Le contrôleur de réseau configure automatiquement le SPN. Il vous suffit de fournir des autorisations pour que les ordinateurs du contrôleur de réseau inscrivent et modifient le SPN. Pour plus d’informations, consultez [configurer des noms de principal du service (SPN)](https://docs.microsoft.com/windows-server/networking/sdn/security/kerberos-with-spn#configure-service-principal-names-spn).

## <a name="deployment-options"></a>Options de déploiement

### <a name="network-controller-deployment"></a>Déploiement du contrôleur de réseau

La configuration est hautement disponible avec trois nœuds de contrôleur de réseau configurés sur des machines virtuelles. Il s’agit également de deux locataires avec le réseau virtuel du locataire 2 divisés en deux sous-réseaux virtuels pour simuler un niveau Web et un niveau base de données.  

![Planification SDN NC](../../media/Plan-a-Software-Defined-Network-Infrastructure/SDN-NC-Planning.png)

### <a name="network-controller-and-software-load-balancer-deployment"></a>Déploiement du contrôleur de réseau et de l’équilibrage de charge logiciel

Pour une haute disponibilité, il existe deux ou plusieurs nœuds SLB/MUX.

![Planification SDN NC](../../media/Plan-a-Software-Defined-Network-Infrastructure/SDN-SLB-Deployment.png)

### <a name="network-controller-software-load-balancer-and-ras-gateway-deployment"></a>Déploiement du contrôleur de réseau, du Load Balancer logiciel et de la passerelle RAS

Il existe trois machines virtuelles de passerelle ; deux sont actifs et l’un est redondant.

![Planification SDN NC](../../media/Plan-a-Software-Defined-Network-Infrastructure/SDN-GW-Deployment.png)  



Pour l’automatisation du déploiement basé sur TP5, Active Directory doit être disponible et accessible à partir de ces sous-réseaux. Pour plus d’informations sur Active Directory, consultez [Active Directory Domain Services vue d’ensemble](https://docs.microsoft.com/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview).  

>[!IMPORTANT] 
>Si vous déployez à l’aide de VMM, assurez-vous que vos machines virtuelles d’infrastructure (serveur VMM, AD/DNS, SQL Server, etc.) ne sont pas hébergées sur l’un des quatre hôtes affichés dans les diagrammes.  


## <a name="next-steps"></a>Étapes suivantes :
[Planifier une infrastructure réseau à définition logicielle](https://technet.microsoft.com/windows-server-docs/networking/sdn/plan/plan-a-software-defined-network-infrastructure).

## <a name="related-topics"></a>Rubriques connexes
- [Contrôleur de réseau](../technologies/network-controller/Network-Controller.md) 
- [Haute disponibilité du contrôleur de réseau](../technologies/network-controller/network-controller-high-availability.md) 
- [Déployer le contrôleur de réseau à l’aide de Windows PowerShell](../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)   
- [Installer le rôle serveur Contrôleur de réseau en utilisant le Gestionnaire de serveur](../technologies/network-controller/Install-the-Network-Controller-server-role-using-Server-Manager.md)   
