---
title: Azure AD Domain Services et services Bureau à distance
description: Découvrez comment intégrer Azure AD Domain Services à votre déploiement des services Bureau à distance.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: chrimo
ms.date: 10/02/2017
ms.tgt_pltfrm: na
ms.topic: article
author: christianmontoya
ms.localizationpriority: medium
ms.openlocfilehash: 511aee3ea5be7e5c70c75cbc4d1febe1ec58dd97
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871067"
---
# <a name="integrate-azure-ad-domain-services-with-your-rds-deployment"></a>Intégrer Azure AD Domain Services à votre déploiement des services Bureau à distance

Vous pouvez utiliser [Azure AD Domain Services](/azure/active-directory-domain-services/active-directory-ds-overview) (Azure AD DS) dans votre déploiement des services Bureau à distance à la place de Windows Server Active Directory. Azure AD DS vous permet d’utiliser vos identités Azure AD existantes avec des charges de travail Windows classiques.

Avec Azure AD DS, vous pouvez : 
- Créer un environnement Azure avec un domaine local pour les organisations nées dans le cloud 
- Créer un environnement Azure isolé avec les mêmes identités que celles utilisées pour votre environnement local et votre environnement en ligne, sans qu’il soit nécessaire de créer un VPN site à site ou de recourir à ExpressRoute 

Une fois que vous avez fini d’intégrer Azure AD DS à votre déploiement du Bureau à distance, votre architecture ressemble à ceci :

![Diagramme d’architecture illustrant les services Bureau à distance avec Azure AD DS](media/aadds-rds.png)

Pour comparer cette architecture à d’autres scénarios de déploiement des services Bureau à distance, consultez [Architectures des services Bureau à distance](desktop-hosting-logical-architecture.md).

Pour mieux comprendre Azure AD DS, consultez [Vue d’ensemble d’Azure AD DS](/azure/active-directory-domain-services/active-directory-ds-overview) et [Guide pratique pour déterminer si Azure AD DS est adapté à votre cas d’usage](/azure/active-directory-domain-services/active-directory-ds-comparison).

Utilisez les informations suivantes pour déployer Azure AD DS avec les services Bureau à distance.

## <a name="prerequisites"></a>Conditions préalables

Avant de pouvoir utiliser vos identités Azure AD dans un déploiement des services Bureau à distance, [configurez Azure AD pour enregistrer les mots de passe hachés des identités de vos utilisateurs](/azure/active-directory-domain-services/active-directory-ds-getting-started-password-sync). Les organisations nées dans le cloud n’ont pas besoin d’apporter des changements supplémentaires à leur annuaire. Toutefois, les organisations locales doivent autoriser la synchronisation et le stockage des hachages de mots de passe dans Azure AD, ce qui n’est peut-être pas autorisé par certaines d’entre elles. Les utilisateurs doivent réinitialiser leurs mots de passe après ce changement de configuration.

## <a name="deploy-azure-ad-ds-and-rds"></a>Déployer Azure AD DS et les services Bureau à distance 
Suivez les étapes ci-après pour déployer Azure AD DS et les services Bureau à distance.

1. Activez [Azure AD DS](/azure/active-directory-domain-services/active-directory-ds-getting-started). Notez que l’article lié fournit les informations suivantes :
   - Il explique selon une procédure pas à pas comment créer les groupes Azure AD appropriés pour l’administration d’un domaine.
   - Il met l’accent sur le moment où vous devez forcer les utilisateurs à changer leurs mots de passe pour que leurs comptes puissent fonctionner avec Azure AD DS.
   
2. Configurez les services Bureau à distance. Vous pouvez utiliser un modèle Azure ou déployer manuellement les services Bureau à distance.
   - Utilisez le [modèle Active Directory existant](https://azure.microsoft.com/resources/templates/rds-deployment-existing-ad/). Veillez à personnaliser ce qui suit :
   
     - **Paramètres**
       - **Groupe de ressources** : utilisez le groupe de ressources dans lequel vous souhaitez créer les ressources des services Bureau à distance.
         > [!NOTE] 
         > Pour le moment, il doit s’agir du même groupe de ressources que celui du réseau virtuel d’Azure Resource Manager.

       - **Préfixe d’étiquette DNS** : Entrez l’URL dont les utilisateurs doivent se servir pour accéder au Bureau à distance par le web.
       - **Nom de domaine Active Directory** : Entrez le nom complet de votre instance d’Azure AD, par exemple « contoso.onmicrosoft.com » ou « contoso.com ».
       - **Nom du réseau virtuel Active Directory** et **Nom du sous-réseau Active Directory** : Entrez les mêmes valeurs que celles que vous avez utilisées durant la création du réseau virtuel d’Azure Resource Manager. Il s’agit du sous-réseau auquel les ressources des services Bureau à distance vont se connecter.
       - **Nom d’utilisateur de l’administrateur** et **Mot de passe d’administrateur** : Entrez les informations d’identification d’un utilisateur administrateur membre du groupe **Administrateurs AAD DC** dans Azure AD.
   
     - **Modèle**
        - Supprimez toutes les propriétés **dnsServers** : après avoir sélectionné **Modifier le modèle** dans la page de modèles de démarrage rapide Azure, recherchez « dnsServers », puis supprimez la propriété. 

           Par exemple, avant de supprimer la propriété **dnsServers** :
      
           ![Modèle de démarrage rapide Azure avec la propriété dnsSettings](media/rds-remove-dnssettings-before.png)

           Et voici le même fichier après la suppression de la propriété :

           ![Modèle de démarrage rapide Azure où la propriété dnsSettings a été supprimée](media/rds-remove-dnssettings-after.png)
   
   - [Déployez les services Bureau à distance manuellement](rds-deploy-infrastructure.md). 

