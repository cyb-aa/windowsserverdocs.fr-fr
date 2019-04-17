---
title: Services de domaine Active Directory Azure et des Services Bureau à distance
description: Découvrez comment intégrer des Services de domaine Active Directory de Azure dans votre déploiement RDS.
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
ms.openlocfilehash: e60cf70f1f91ad87046bedf024fe9afc459075b6
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/01/2018
ms.locfileid: "1614515"
---
# <a name="integrate-azure-ad-domain-services-with-your-rds-deployment"></a>Intégrer des Services de domaine Active Directory de Azure avec votre déploiement RDS

Vous pouvez utiliser [Azure Active Directory Domain Services](/azure/active-directory-domain-services/active-directory-ds-overview) Azure AD DS dans votre déploiement des Services Bureau à distance à la place de Windows Server Active Directory. Azure AD DS vous permet d’utiliser vos identités Azure AD existantes avec des charges de travail Windows classique.

Avec Azure AD DS, vous pouvez: 
- Créer un environnement Azure avec un domaine local pour les organisations de naissance dans-le nuage. 
- Créer un environnement isolé Azure avec la même identité utilisée pour l’organisation locale et votre environnement en ligne, sans devoir créer un site à distance ou VPN ExpressRoute. 

Lorsque vous avez terminé l’intégration Azure AD DS dans votre déploiement de bureau à distance, votre architecture devrait ressembler à ceci:

![Un diagramme d’architecture affichant RDS avec Azure AD DS](media/aadds-rds.png)

Pour voir comment cette architecture compare avec d’autres scénarios de déploiement RDS, consultez la rubrique [architectures des Services Bureau à distance](desktop-hosting-logical-architecture.md).

Pour obtenir une meilleure compréhension d’Azure AD DS, consultez la [vue d’ensemble d’Azure AD DS](/azure/active-directory-domain-services/active-directory-ds-overview) et [comment déterminer si Azure AD DS à votre cas d’utilisation](/azure/active-directory-domain-services/active-directory-ds-comparison).

Utilisez les informations suivantes pour déployer Azure AD DS avec RDS.

## <a name="prerequisites"></a>Prérequis

Avant que vous pouvez mettre vos identités d’Azure Active Directory à utiliser dans un déploiement RDS, [configurer Azure AD pour enregistrer les mots de passe découpés pour les identités de vos utilisateurs](/azure/active-directory-domain-services/active-directory-ds-getting-started-password-sync). Organisations de naissance dans-nuage n’avez pas besoin à apporter des modifications supplémentaires dans leur directory; Toutefois, les organisations locale doivent autoriser les hachages de mot de passe seront synchronisés et stockées dans Azure AD, qui ne peut être admis pour certaines organisations. Les utilisateurs devront réinitialiser leur mot de passe après avoir effectué cette modification de configuration.

## <a name="deploy-azure-ad-ds-and-rds"></a>Déployer RDS et Azure AD DS 
Utilisez les étapes suivantes pour déployer Azure AD DS et RDS.

1. Activer [Azure AD DS](/azure/active-directory-domain-services/active-directory-ds-getting-started). Notez que l’article lié effectue les opérations suivantes:
   - Étapes permettant de créer les groupes d’Azure AD pour l’administration de domaine.
   - Mettez en surbrillance lorsque vous devrez peut-être forcer les utilisateurs à modifier leur mot de passe pour que leurs comptes puissent travailler avec Azure AD DS.
   
2. Configurer RDS. Vous pouvez utiliser un modèle Azure ou déployer RDS manuellement.
   - Utiliser le [modèle existant AD](https://azure.microsoft.com/resources/templates/rds-deployment-existing-ad/). Veillez à personnaliser les éléments suivants:
   
      - **Paramètres**
         - **Groupe de ressources**: utiliser le groupe de ressources où vous souhaitez créer les ressources RDS.
         > [!NOTE] 
         > Maintenant, il doit être le même groupe de ressources où se trouve le réseau virtuel de gestionnaire de ressources Azure.

         - **Préfixe d’étiquette DNS**: entrez l’URL que vous souhaitez que les utilisateurs à utiliser pour accéder aux services Bureau à distance Web.
         - **Nom de domaine Active Directory**: entrez le nom complet de votre instance Azure AD, par exemple, «contoso.onmicrosoft.com» ou «contoso.com».
         - **Ad Vnet nom** et **Nom du sous-réseau Ad**: entrez les mêmes valeurs que vous avez utilisé lorsque vous avez créé le réseau virtuel de gestionnaire de ressources Azure. Il s’agit du sous-réseau à laquelle les ressources RDS seront connectera.
         - **Nom d’utilisateur Admin** et **Le mot de passe d’administration**: entrez les informations d’identification pour un utilisateur d’administration qui est un membre du groupe **Administrateurs du contrôleur de domaine DAS** dans Azure AD.
   
      - **Modèle**
         - Supprimer toutes les propriétés de **dnsServers**: après avoir sélectionné l’option **Modifier le modèle** à partir de la page de modèle de démarrage rapide Azure, recherchez «dnsServers» et supprimer la propriété. 

            Par exemple, avant de supprimer la propriété **dnsServers** :
      
            ![Modèle de démarrage rapide Azure avec la propriété dnsSettings](media/rds-remove-dnssettings-before.png)

            Et Voici le même fichier après la suppression de la propriété:

            ![Modèle de démarrage rapide Azure avec la propriété dnsSettings supprimée](media/rds-remove-dnssettings-after.png)
   
   - [RDS déployer manuellement](rds-deploy-infrastructure.md). 

