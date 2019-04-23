---
title: Configuration requise pour le déploiement de contrôleur de réseau
description: Préparer votre centre de données pour le déploiement de contrôleur de réseau, ce qui nécessite un ou plusieurs ordinateurs ou machines virtuelles et un seul ordinateur ou machine virtuelle. Avant de déployer le contrôleur de réseau, vous devez configurer les groupes de sécurité, emplacements des fichiers journaux (si nécessaire) et l’inscription DNS dynamique.
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: get-started-article
ms.assetid: 7f899e62-6e5b-4fca-9a59-130d4766ee2f
ms.author: pashort
author: shortpatti
ms.date: 08/10/2018
ms.openlocfilehash: 9db7609f6f1273c46cba1dd29f81c297bb26f94b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829860"
---
# <a name="requirements-for-deploying-network-controller"></a>Configuration requise pour le déploiement de contrôleur de réseau

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Préparer votre centre de données pour le déploiement de contrôleur de réseau, ce qui nécessite un ou plusieurs ordinateurs ou machines virtuelles et un seul ordinateur ou machine virtuelle. Avant de déployer le contrôleur de réseau, vous devez configurer les groupes de sécurité, emplacements des fichiers journaux (si nécessaire) et l’inscription DNS dynamique.


## <a name="network-controller-requirements"></a>Configuration requise du contrôleur réseau

Déploiement du contrôleur de réseau nécessite un ou plusieurs ordinateurs ou machines virtuelles qui ont comme contrôleur de réseau et un seul ordinateur ou machine virtuelle comme un client de gestion pour le contrôleur de réseau. 

- Toutes les machines virtuelles et ordinateurs planifiées en tant que nœuds de contrôleur de réseau doivent exécuter Windows Server 2016 Datacenter edition. 
- L’Édition Datacenter de Windows Server 2016 doit être en cours d’exécution n’importe quel ordinateur ou une machine virtuelle (VM) sur lequel vous installez le contrôleur de réseau. 
- L’ordinateur client de gestion ou de la machine virtuelle pour le contrôleur de réseau doit exécuter Windows 10. 

  
## <a name="configuration-requirements"></a>Configuration requise

Avant de déployer un contrôleur de réseau, vous devez configurer les groupes de sécurité, emplacements des fichiers journaux (si nécessaire) et l’inscription DNS dynamique.
  
### <a name="step-1-configure-your-security-groups"></a>Étape 1. Configurer vos groupes de sécurité
  
La première chose que vous voulez faire est créer deux groupes de sécurité pour l’authentification Kerberos. 

Vous créez des groupes pour les utilisateurs habilités à : 

1. Configurer le contrôleur de réseau<p>Vous pouvez nommer ce groupe Administrateurs du contrôleur de réseau, par exemple. 
2.  Configurer et gérer le réseau à l’aide du contrôleur de réseau<p>Vous pouvez nommer ce contrôleur de réseau de groupe utilisateurs, par exemple. Utilisez Representational State Transfer (REST) pour configurer et gérer le contrôleur de réseau.

>[!NOTE]
>Tous les utilisateurs que vous ajoutez doivent être membres du groupe utilisateurs du domaine dans Active Directory Users and Computers.

### <a name="step-2-configure-log-file-locations-if-needed"></a>Étape 2. Configurer les emplacements des fichiers journaux si nécessaire

La prochaine chose que vous voulez faire est de configurer les emplacements des fichiers pour stocker les journaux de débogage de contrôleur de réseau sur l’ordinateur de contrôleur de réseau ou d’une machine virtuelle ou sur un partage de fichiers distant. 

>[!NOTE]
>Si vous stockez les journaux dans un partage de fichiers distant, assurez-vous que le partage est accessible à partir du contrôleur de réseau.


### <a name="step-3-configure-dynamic-dns-registration-for-network-controller"></a>Étape 3. Configurer l’inscription DNS dynamique pour le contrôleur de réseau
  
Enfin, la prochaine chose que vous voulez faire est de déployer des nœuds de cluster de contrôleur de réseau sur le même sous-réseau ou des sous-réseaux différents. 

|Si…  |Alors…  |
|---------|---------|
|Sur le même sous-réseau, |Vous devez fournir l’adresse IP REST du contrôleur réseau. |
|Sur des sous-réseaux différents, |Vous devez fournir le nom DNS REST du contrôleur réseau, que vous créez pendant le processus de déploiement. Vous devez également effectuer ce qui suit :<ul><li>Configurer les mises à jour dynamiques DNS pour le nom DNS du contrôleur réseau sur le serveur DNS.</li><li>Restreindre les mises à jour dynamiques DNS pour les nœuds de contrôleur de réseau uniquement.</li></ul> |
---

> [!NOTE]
> L’appartenance au **Admins du domaine**, ou équivalente, est la condition minimale requise pour effectuer ces procédures.
  
1. Autoriser les mises à jour dynamiques DNS pour une zone.

   a. Ouvrez le Gestionnaire DNS et dans l’arborescence de la console, cliquez sur la zone concernée, puis cliquez sur **propriétés**. 
      
   b. Sur le **général** , vérifiez que le type de zone est soit **principal** ou **intégré à Active Directory**.

   c. Dans **mises à jour dynamiques**, vérifiez que **Secure uniquement** est sélectionnée, puis cliquez sur **OK**.

2. Configurer les autorisations de sécurité de zone DNS pour les nœuds de contrôleur de réseau

   a.  Cliquez sur l'onglet **Sécurité**, puis sur **Avancé**. 

   b. Dans **paramètres de sécurité avancés**, cliquez sur **ajouter**. 
  
   c. Cliquez sur **Sélectionnez un principal**. 

   d. Dans le **sélectionner un utilisateur, ordinateur, compte de Service ou groupe** boîte de dialogue, cliquez sur **Types d’objets**. 

   e. Dans **Types d’objets**, sélectionnez **ordinateurs**, puis cliquez sur **OK**.

   f. Dans le **sélectionner un utilisateur, ordinateur, compte de Service ou groupe** boîte de dialogue, tapez le nom NetBIOS d’un des nœuds du contrôleur de réseau dans votre déploiement, puis cliquez sur **OK**.

   g. Dans **entrée d’autorisation**, vérifiez les valeurs suivantes :

      - **Type** = Allow
      - **S’applique à** = cet objet et tous ceux descendants
  
   h. Dans **autorisations**, sélectionnez **écrire toutes les propriétés** et **supprimer**, puis cliquez sur **OK**.

3. Répétez pour tous les ordinateurs et les machines virtuelles dans le cluster de contrôleur de réseau.

### <a name="step-4-configure-service-principal-name-if-using-kerberos-based-authentication"></a>Étape 4. Configurer le nom Principal de Service si à l’aide de Kerberos, l’authentification basée sur

Si le contrôleur de réseau est à l’aide de l’authentification Kerberos pour la communication avec les clients de gestion, vous devez configurer un nom de Principal du Service (SPN) pour le contrôleur de réseau dans Active Directory. Le contrôleur de réseau configure automatiquement le SPN. Il vous suffit de faire consiste à fournir des autorisations pour les machines de contrôleur de réseau pour vous inscrire et de modifier le SPN. Pour plus d’informations, consultez [configurer Service noms principaux (SPN)](https://docs.microsoft.com/windows-server/networking/sdn/security/kerberos-with-spn#configure-service-principal-names-spn).

## <a name="deployment-options"></a>Options de déploiement

### <a name="network-controller-deployment"></a>Déploiement du contrôleur de réseau

Le programme d’installation est hautement disponible avec trois nœuds de contrôleur de réseau configurés sur des machines virtuelles. Est également affiché de deux clients avec un réseau virtuel du locataire 2 divisé en deux sous-réseaux virtuels pour simuler un niveau web et un niveau de base de données.  

![Planification de contrôleur de réseau SDN](../../media/Plan-a-Software-Defined-Network-Infrastructure/SDN-NC-Planning.png)

### <a name="network-controller-and-software-load-balancer-deployment"></a>Déploiement d’un équilibrage de charge logiciel et le contrôleur de réseau

Pour une haute disponibilité, il existe deux ou plusieurs nœuds SLB/MUX.
   
![Planification de contrôleur de réseau SDN](../../media/Plan-a-Software-Defined-Network-Infrastructure/SDN-SLB-Deployment.png)
  
### <a name="network-controller-software-load-balancer-and-ras-gateway-deployment"></a>Déploiement de contrôleur de réseau, équilibrage de charge logicielle et passerelle RAS

Il existe trois machines virtuelles de passerelle ; deux sont actifs, et une est redondante.

![Planification de contrôleur de réseau SDN](../../media/Plan-a-Software-Defined-Network-Infrastructure/SDN-GW-Deployment.png)  
  
  
  
Pour l’automatisation du déploiement basé sur TP5, Active Directory doit être disponible et accessible à partir de ces sous-réseaux. Pour plus d’informations sur Active Directory, consultez [présentation des Services de domaine Active Directory](https://docs.microsoft.com/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview).  
  
>[!IMPORTANT] 
>Si vous déployez à l’aide de VMM, vérifiez vos machines virtuelles d’infrastructure (serveur VMM, AD/DNS, SQL Server, etc.) ne sont pas hébergés sur un des quatre hôtes affichés dans les diagrammes.  


## <a name="next-steps"></a>Étapes suivantes
[Plan d’un logiciel défini par l’Infrastructure réseau](https://technet.microsoft.com/windows-server-docs/networking/sdn/plan/plan-a-software-defined-network-infrastructure).

## <a name="related-topics"></a>Rubriques connexes
- [Contrôleur de réseau](../technologies/network-controller/Network-Controller.md) 
- [Haute disponibilité du contrôleur de réseau](../technologies/network-controller/network-controller-high-availability.md) 
- [Déployer le contrôleur de réseau à l’aide de Windows PowerShell](../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)   
- [Installer le rôle de serveur de contrôleur de réseau à l’aide du Gestionnaire de serveur](../technologies/network-controller/Install-the-Network-Controller-server-role-using-Server-Manager.md)   
