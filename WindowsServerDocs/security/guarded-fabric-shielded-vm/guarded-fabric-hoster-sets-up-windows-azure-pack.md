---
title: 'VM dotées d’une protection maximale : Le fournisseur de services d’hébergement configure Windows Azure Pack'
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: d528c689-58b0-425c-9740-25e2553ed689
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 156832087bc7af0c95a92cab9a0c1501264d47a5
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447501"
---
# <a name="shielded-vms---hosting-service-provider-sets-up-windows-azure-pack"></a>VM dotées d’une protection maximale : Le fournisseur de services d’hébergement configure Windows Azure Pack

Cette rubrique décrit comment un fournisseur de services peut configurer Windows Azure Pack afin que les clients peuvent utiliser pour déployer des machines virtuelles protégées. Windows Azure Pack est un portail web qui étend les fonctionnalités de System Center Virtual Machine Manager pour permettre aux locataires de déployer et gérer leurs propres machines virtuelles via une interface web simple. Windows Azure Pack prend en charge des machines virtuelles protégées entièrement et rend encore plus facile pour vos clients créer et gérer leurs fichiers de données de protection.

Pour comprendre comment cette rubrique s’intègre dans le processus de déploiement de machines virtuelles protégées, consultez [hébergeant des étapes de configuration de fournisseur de service pour les hôtes service Guardian et des machines virtuelles protégées](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md).

## <a name="setting-up-windows-azure-pack"></a>Configuration de Windows Azure Pack

Vous effectuerez les tâches suivantes pour configurer Windows Azure Pack dans votre environnement :

1. Terminer la configuration de System Center 2016 - Virtual Machine Manager (VMM) pour votre infrastructure d’hébergement. Cela inclut la configuration des modèles de machine virtuelle et un cloud de machine virtuelle, ce qui doit être exposé dans Windows Azure Pack :

    [Scénario : déployer des hôtes service Guardian et des machines virtuelles protégées dans VMM](https://technet.microsoft.com/system-center-docs/vmm/scenario/guarded-overview)

2. Installer et configurer System Center 2016 - Service Provider Foundation (SPF). Ce logiciel permet à Windows Azure Pack communiquer avec vos serveurs VMM :

    [Déploiement de Service Provider Foundation - SPF](https://technet.microsoft.com/system-center-docs/spf/deploy/deploy-spf)

3. Installer Windows Azure Pack et configurez-le pour communiquer avec SPF :

    - [Installer Windows Azure Pack](#install-windows-azure-pack) (dans cette rubrique)
    - [Configurer Windows Azure Pack](#configure-windows-azure-pack) (dans cette rubrique)

4. Créer un ou plusieurs plans d’hébergement dans Windows Azure Pack pour autoriser l’accès à vos clouds de machines virtuelles locataires :

    [Créer un plan dans Windows Azure Pack](#create-a-plan-in-windows-azure-pack) (dans cette rubrique)

## <a name="install-windows-azure-pack"></a>Installer Windows Azure Pack

Installez et configurez Windows Azure Pack (WAP) sur l’ordinateur où vous voulez héberger le portail web pour vos clients. Cet ordinateur doit être en mesure d’atteindre le serveur SPF et être accessible par vos clients.

1.  Examen des [requise WAP](https://technet.microsoft.com/library/dn296442.aspx) et installer le [logiciels requis](https://technet.microsoft.com/library/dn469335.aspx).

2.  Téléchargez et installez le [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx). Si l’ordinateur n’est pas connecté à Internet, suivez le [instructions d’installation hors connexion](http://www.iis.net/learn/install/web-platform-installer/web-platform-installer-v4-command-line-webpicmdexe-rtw-release).

3.  Ouvrez le programme Web Platform Installer, puis recherchez **Windows Azure Pack : Portail et API Express** sous le **produits** onglet. Cliquez sur **ajouter**, puis **installer** en bas de la fenêtre.

4.  Procédez à l’installation. Une fois l’installation terminée, le site de configuration (*https://&lt;wapserver&gt;: 30101 /* ) s’ouvre dans votre navigateur web. Sur ce site Web, fournissent des informations sur votre serveur SQL server et terminer la configuration de proxy d’application Web.

Pour plus d’aide pour configurer Windows Azure Pack, consultez [installer un déploiement express de Windows Azure Pack](https://technet.microsoft.com/dn296439.aspx).

> [!NOTE]
> Si vous exécutez déjà Windows Azure Pack dans votre environnement, vous pouvez utiliser votre installation existante. Pour pouvoir fonctionner avec la dernière version protégée des fonctionnalités de machine virtuelle, toutefois, vous devez mettre à niveau votre installation au moins 10 de correctif cumulatif de mise à jour.

### <a name="configure-windows-azure-pack"></a>Configurer Windows Azure Pack

Avant d’utiliser Windows Azure Pack, il doit déjà être installé et configuré pour votre infrastructure.

1.  Accédez au portail d’administration Windows Azure Pack à *https://&lt;wapserver&gt;: 30091*, puis ouvrez une session à l’aide de vos informations d’identification d’administrateur.

2.  Dans le volet gauche, cliquez sur **Clouds de machines virtuelles**.

3.  Se connecter Windows Azure Pack à l’instance de Service Provider Foundation en cliquant sur **inscrire System Center Service Provider Foundation**. Vous devez spécifier l’URL de Service Provider Foundation, ainsi que d’un nom d’utilisateur et mot de passe.

    ![Inscrire System Center Service Provider Foundation](../media/Guarded-Fabric-Shielded-VM/guarded-host-azure-pack-01-register-spf.png)

4.  Une fois terminé, vous devez être en mesure de voir les clouds d’ordinateurs virtuels à configurer dans votre environnement VMM. Assurez-vous de qu'avoir au moins un cloud d’ordinateurs virtuels que prend en charge des machines virtuelles protégées disponibles pour le proxy d’application Web avant de continuer.

### <a name="create-a-plan-in-windows-azure-pack"></a>Créer un plan dans Windows Azure Pack

Afin de permettre aux clients de créer des machines virtuelles dans WAP, vous devez d’abord créer un plan d’hébergement auquel les clients peuvent s’abonner. Plans définissent les clouds de machines virtuelles autorisées, des modèles, des réseaux et des entités de facturation pour vos clients.

1. Dans le volet inférieur du portail, cliquez sur **+ nouveau** &gt; **planifier** &gt; **créer un PLAN de**.

2. Dans la première étape de l’Assistant, choisissez un nom pour votre Plan. C’est le nom de que vos clients verront plus apparaître lors de l’abonnement.

3. Dans la deuxième étape, sélectionnez **CLOUDS de machines virtuelles** comme l’un des services à proposer dans le plan.

4. Ignorez l’étape sur la sélection de modules complémentaires pour le plan.

5. Cliquez sur **OK** (coche) pour créer le plan. Bien que cela crée le plan, il n’est pas encore dans un état configuré.

   ![Plans dans Windows Azure Pack](../media/Guarded-Fabric-Shielded-VM/guarded-host-azure-pack-02-create-plan.png)

6. Pour configurer le Plan, cliquez sur son nom.

7. Sur la page suivante, sous **services du plan**, cliquez sur **Clouds de machines virtuelles**. Cette opération ouvre la page où vous pouvez configurer des quotas pour ce plan.

8. Sous **base**, sélectionnez le serveur d’administration VMM et le Cloud de machines virtuelles que vous souhaitez proposer à vos clients. Les clouds que peuvent offrir des machines virtuelles protégées seront affichés avec **(prise en charge de protection)** en regard de leur nom.

9. Sélectionnez les quotas à appliquer dans ce Plan. (Par exemple, les limites relatives à cœur de processeur et l’utilisation de mémoire RAM). Veillez à laisser le **autoriser les Machines virtuelles pour être protégées** case à cocher activée.

   ![Paramètres de clouds de machines virtuelles dans Windows Azure Pack](../media/Guarded-Fabric-Shielded-VM/guarded-host-azure-pack-03-virtual-machine-clouds.png)
    
10. Faites défiler jusqu'à la section intitulée **modèles**, puis sélectionnez un ou plusieurs modèles à proposer à vos clients. Vous pouvez proposer les deux protégées et les modèles aux locataires, mais un modèle protégé doit être proposée pour donner des garanties de bout en bout concernant l’intégrité de la machine virtuelle et leurs secrets de locataires.

11. Dans le **réseaux** section, ajoutez un ou plusieurs réseaux pour vos clients.

12. Après avoir défini les paramètres ou les quotas pour le Plan, cliquez sur **enregistrer** en bas.

13. En haut à gauche de l’écran, cliquez sur la flèche pour accéder à nouveau à la **planifier** page.

14. En bas de l’écran, modifier le Plan ne soient pas **privé** à **Public** afin que les clients peuvent s’abonner au Plan.

    ![Modifier l’accès pour un plan dans Windows Azure Pack](../media/Guarded-Fabric-Shielded-VM/guarded-host-azure-pack-04-change-access.png)

    À ce stade, Windows Azure Pack est configuré et les locataires ne pourront pas s’abonner au plan que vous venez de créer et déployer des machines virtuelles protégées. Pour connaître les étapes supplémentaires que les locataires ont besoin pour terminer, consultez [machines virtuelles protégées pour les locataires - déploiement d’une machine virtuelle protégée à l’aide de Windows Azure Pack](guarded-fabric-shielded-vm-windows-azure-pack.md).

## <a name="see-also"></a>Voir aussi

- [Hébergement des étapes de configuration de fournisseur de service pour les hôtes service Guardian et des machines virtuelles protégées](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [Structure protégée et machines virtuelles dotées d’une protection maximale](guarded-fabric-and-shielded-vms-top-node.md)
