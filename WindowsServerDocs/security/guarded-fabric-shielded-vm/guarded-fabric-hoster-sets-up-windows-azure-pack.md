---
title: 'VM dotées d’une protection maximale : Le fournisseur de services d’hébergement configure Windows Azure Pack'
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: d528c689-58b0-425c-9740-25e2553ed689
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: d388da2b7416543c307bd931636902b4a7543e1e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403652"
---
# <a name="shielded-vms---hosting-service-provider-sets-up-windows-azure-pack"></a>VM dotées d’une protection maximale : Le fournisseur de services d’hébergement configure Windows Azure Pack

Cette rubrique décrit comment un fournisseur de services d’hébergement peut configurer Windows Azure Pack afin que les locataires puissent l’utiliser pour déployer des machines virtuelles protégées. Windows Azure Pack est un portail Web qui étend les fonctionnalités de System Center Virtual Machine Manager pour permettre aux locataires de déployer et de gérer leurs propres machines virtuelles via une interface Web simple. Windows Azure Pack prend entièrement en charge les machines virtuelles protégées et facilite encore la création et la gestion des fichiers de données de protection de vos locataires.

Pour comprendre le fonctionnement de cette rubrique dans le processus global de déploiement de machines virtuelles protégées, consultez [étapes de configuration du fournisseur de services d’hébergement pour les hôtes service Guardian et les machines virtuelles](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)protégées.

## <a name="setting-up-windows-azure-pack"></a>Configuration de Windows Azure Pack

Vous allez effectuer les tâches suivantes pour configurer Windows Azure Pack dans votre environnement :

1. Procédez à la configuration de System Center 2016-Virtual Machine Manager (VMM) pour votre infrastructure d’hébergement. Cela comprend la configuration des modèles de machine virtuelle et un Cloud d’ordinateurs virtuels, qui seront exposés via Windows Azure Pack :

    [Scénario : déployer des hôtes service Guardian et des machines virtuelles protégées dans VMM](https://technet.microsoft.com/system-center-docs/vmm/scenario/guarded-overview)

2. Installez et configurez System Center 2016-Service Provider Foundation (SPF). Ce logiciel permet à Windows Azure Pack de communiquer avec vos serveurs VMM :

    [Déploiement de Service Provider Foundation-SPF](https://technet.microsoft.com/system-center-docs/spf/deploy/deploy-spf)

3. Installez Windows Azure Pack et configurez-le pour qu’il communique avec SPF :

    - [Windows Azure Pack d’installation](#install-windows-azure-pack) (dans cette rubrique)
    - [Configurer Windows Azure Pack](#configure-windows-azure-pack) (dans cette rubrique)

4. Créez un ou plusieurs plans d’hébergement dans Windows Azure Pack pour permettre aux locataires d’accéder à vos Clouds de machines virtuelles :

    [Créer un plan dans Windows Azure Pack](#create-a-plan-in-windows-azure-pack) (dans cette rubrique)

## <a name="install-windows-azure-pack"></a>Installer Windows Azure Pack

Installez et configurez Windows Azure Pack (WAP) sur l’ordinateur où vous souhaitez héberger le portail Web pour vos locataires. Cet ordinateur devra être en mesure d’atteindre le serveur SPF et d’être accessible à vos locataires.

1.  Examen de la [Configuration requise du système WAP](https://technet.microsoft.com/library/dn296442.aspx) et installation des [logiciels requis](https://technet.microsoft.com/library/dn469335.aspx).

2.  Téléchargez et installez le [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx). Si l’ordinateur n’est pas connecté à Internet, suivez les [instructions d’installation hors connexion](http://www.iis.net/learn/install/web-platform-installer/web-platform-installer-v4-command-line-webpicmdexe-rtw-release).

3.  Ouvrez la Web Platform Installer et recherchez **Windows Azure Pack : portail et API Express** sous l’onglet **produits** . cliquez sur **Ajouter**, puis sur **installer** en bas de la fenêtre.

4.  Procédez à l’installation. Une fois l’installation terminée, le site de configuration (*https://&lt;wapserver&gt;: 30101/* ) s’ouvre dans votre navigateur Web. Sur ce site Web, fournissez des informations sur votre serveur SQL Server et terminez la configuration du WAP.

Pour obtenir de l’aide sur la configuration des Windows Azure Pack, consultez [installer un déploiement Express de Windows Azure Pack](https://technet.microsoft.com/dn296439.aspx).

> [!NOTE]
> Si vous avez déjà exécuté Windows Azure Pack dans votre environnement, vous pouvez utiliser votre installation existante. Toutefois, pour utiliser les dernières fonctionnalités de la machine virtuelle protégée, vous devrez mettre à niveau votre installation vers au moins le correctif cumulatif 10.

### <a name="configure-windows-azure-pack"></a>Configurer Windows Azure Pack

Avant d’utiliser Windows Azure Pack, vous devez avoir déjà installé et configuré pour votre infrastructure.

1.  Accédez au portail d’administration Windows Azure Pack à l’adresse *https://&lt;wapserver&gt;: 30091*, puis connectez-vous à l’aide de vos informations d’identification d’administrateur.

2.  Dans le volet gauche, cliquez sur **Clouds d’ordinateurs virtuels**.

3.  Connectez Windows Azure Pack à l’instance de Service Provider Foundation en cliquant sur **inscrire System Center Service Provider Foundation**. Vous devez spécifier l’URL de Service Provider Foundation, ainsi qu’un nom d’utilisateur et un mot de passe.

    ![Inscrire System Center Service Provider Foundation](../media/Guarded-Fabric-Shielded-VM/guarded-host-azure-pack-01-register-spf.png)

4.  Une fois que vous avez terminé, vous devez être en mesure de voir les clouds de machines virtuelles configurés dans votre environnement VMM. Avant de poursuivre, assurez-vous de disposer d’au moins un Cloud de machines virtuelles prenant en charge les machines virtuelles protégées.

### <a name="create-a-plan-in-windows-azure-pack"></a>Créer un plan dans Windows Azure Pack

Pour permettre aux locataires de créer des machines virtuelles dans WAP, vous devez d’abord créer un plan d’hébergement auquel les locataires peuvent s’abonner. Les plans définissent les clouds de machines virtuelles, les modèles, les réseaux et les entités de facturation autorisés pour vos locataires.

1. Dans le volet inférieur du portail, cliquez sur **+ nouveau** &gt; **plan** &gt; **créer un plan**.

2. Dans la première étape de l’Assistant, choisissez un nom pour votre plan. Il s’agit du nom que vos locataires verront lors de l’abonnement.

3. Dans la deuxième étape, sélectionnez **Clouds de machines virtuelles** comme l’un des services à proposer dans le plan.

4. Ignorez l’étape relative à la sélection des modules complémentaires pour le plan.

5. Cliquez sur **OK** (coche) pour créer le plan. Bien que cela crée le plan, il n’est pas encore dans un état configuré.

   ![Plans dans Windows Azure Pack](../media/Guarded-Fabric-Shielded-VM/guarded-host-azure-pack-02-create-plan.png)

6. Pour commencer à configurer le plan, cliquez sur son nom.

7. Sur la page suivante, sous **services du plan**, cliquez sur **Clouds de machines virtuelles**. Cela ouvre la page dans laquelle vous pouvez configurer des quotas pour ce plan.

8. Sous de **base**, sélectionnez le serveur d’administration VMM et le Cloud d’ordinateurs virtuels que vous souhaitez proposer à vos locataires. Les clouds qui peuvent offrir des machines virtuelles protégées seront affichés avec **(protection prise en charge)** en regard de leur nom.

9. Sélectionnez les quotas que vous souhaitez appliquer dans ce plan. (Par exemple, limites sur l’utilisation du processeur et de la RAM). Veillez à laisser la case à cocher **autoriser les machines virtuelles à être protégées** sélectionnée.

   ![Paramètres pour les clouds de machines virtuelles dans Windows Azure Pack](../media/Guarded-Fabric-Shielded-VM/guarded-host-azure-pack-03-virtual-machine-clouds.png)
    
10. Faites défiler jusqu’à la section intitulée **modèles**, puis sélectionnez un ou plusieurs modèles à proposer à vos locataires. Vous pouvez proposer des modèles protégés et non protégés aux locataires, mais un modèle protégé doit être proposé pour donner aux locataires des garanties de bout en bout sur l’intégrité de la machine virtuelle et de leurs secrets.

11. Dans la section **réseaux** , ajoutez un ou plusieurs réseaux pour vos locataires.

12. Après avoir défini d’autres paramètres ou quotas pour le plan, cliquez sur **Enregistrer** en bas.

13. En haut à gauche de l’écran, cliquez sur la flèche pour revenir à la page **plan** .

14. En bas de l’écran, définissez le niveau de **confidentialité** sur **public** afin que les locataires puissent s’abonner au plan.

    ![Modifier l’accès d’un plan dans Windows Azure Pack](../media/Guarded-Fabric-Shielded-VM/guarded-host-azure-pack-04-change-access.png)

    À ce stade, Windows Azure Pack est configurée et les locataires peuvent s’abonner au plan que vous venez de créer et déployer des machines virtuelles protégées. Pour connaître les étapes supplémentaires que les locataires doivent effectuer, consultez [machines virtuelles protégées pour les locataires-déploiement d’une machine virtuelle protégée à l’aide de Windows Azure Pack](guarded-fabric-shielded-vm-windows-azure-pack.md).

## <a name="see-also"></a>Voir également

- [Étapes de configuration du fournisseur de services d’hébergement pour les hôtes service Guardian et les machines virtuelles protégées](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [Structure protégée et machines virtuelles dotées d’une protection maximale](guarded-fabric-and-shielded-vms-top-node.md)
