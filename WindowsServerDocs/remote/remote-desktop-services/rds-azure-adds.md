---
title: Azure AD Domain Services et des Services Bureau à distance
description: Découvrez comment intégrer Azure AD Domain Services dans votre déploiement des services Bureau à distance.
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59860510"
---
# <a name="integrate-azure-ad-domain-services-with-your-rds-deployment"></a>Intégrer Azure AD Domain Services avec votre déploiement des Services Bureau à distance

Vous pouvez utiliser [Azure AD Domain Services](/azure/active-directory-domain-services/active-directory-ds-overview) (Azure AD DS) dans votre déploiement des Services Bureau à distance à la place de Windows Server Active Directory. Azure AD DS vous permet d’utiliser vos identités Azure AD existantes avec des charges de travail Windows classiques.

Avec Azure AD DS, vous pouvez : 
- Créer un environnement Azure avec un domaine local pour les organisations né-in-the-cloud. 
- Créer un environnement isolé Azure avec les mêmes identités utilisées pour votre en local et votre environnement en ligne, sans avoir à créer un VPN de site à site ou ExpressRoute. 

Lorsque vous avez terminé l’intégration d’Azure AD DS dans votre déploiement de bureau à distance, votre architecture se présente comme suit :

![Un diagramme d’architecture montrant des services Bureau à distance avec Azure AD DS](media/aadds-rds.png)

Pour voir comment cette architecture compare avec les autres scénarios de déploiement des services Bureau à distance, consultez [architectures des Services Bureau à distance](desktop-hosting-logical-architecture.md).

Pour obtenir une meilleure compréhension d’Azure AD DS, consultez le [vue d’ensemble d’Azure AD DS](/azure/active-directory-domain-services/active-directory-ds-overview) et [comment déterminer si Azure AD DS est adapté à votre cas d’usage](/azure/active-directory-domain-services/active-directory-ds-comparison).

Utilisez les informations suivantes pour déployer Azure AD DS avec RDS.

## <a name="prerequisites"></a>Prérequis

Avant de pouvoir mettre vos identités d’Azure AD à utiliser dans un déploiement services Bureau à distance, [configurer Azure AD pour enregistrer les mots de passe hachés pour les identités des utilisateurs](/azure/active-directory-domain-services/active-directory-ds-getting-started-password-sync). Les organisations né-in-the-cloud n’avez pas besoin d’apporter des modifications supplémentaires dans son répertoire ; Toutefois, les organisations de local doivent autoriser les hachages de mot de passe être synchronisés et stockés dans Azure AD, qui ne peut pas être autorisée pour certaines organisations. Les utilisateurs devront réinitialiser leurs mots de passe après avoir apporté cette modification de configuration.

## <a name="deploy-azure-ad-ds-and-rds"></a>Déployer des services Bureau à distance et Azure AD DS 
Utilisez les étapes suivantes pour déployer Azure AD DS et RDS.

1. Activer [Azure AD DS](/azure/active-directory-domain-services/active-directory-ds-getting-started). Notez que l’article lié effectue les opérations suivantes :
   - Explique comment créer approprié groupes Azure AD pour l’administration de domaine.
   - Mettez en surbrillance lorsque vous devrez peut-être forcer les utilisateurs à modifier leur mot de passe pour leurs comptes puissent travailler avec Azure AD DS.
   
2. Configuration de RDS. Vous pouvez utiliser un modèle Azure ou déployer manuellement des services Bureau à distance.
   - Utilisez le [Existing AD modèle](https://azure.microsoft.com/resources/templates/rds-deployment-existing-ad/). Veillez à personnaliser les éléments suivants :
   
      - **Paramètres**
         - **Groupe de ressources**: Utilisez le groupe de ressources dans lequel vous souhaitez créer les ressources de services Bureau à distance.
         > [!NOTE] 
         > Actuellement, cette valeur doit être le même groupe de ressources où se trouve le réseau virtuel Azure resource manager.

         - **Préfixe d’étiquette DNS**: Entrez l’URL que vous souhaitez que les utilisateurs à utiliser pour accéder à Web du Bureau à distance.
         - **Nom de domaine AD**: Entrez le nom complet de votre instance Azure AD, par exemple, « contoso.onmicrosoft.com » ou « contoso.com ».
         - **Nom du réseau virtuel AD** et **nom du sous-réseau Ad**: Entrez les mêmes valeurs que vous avez utilisé lorsque vous avez créé le réseau virtuel Azure resource manager. Il s’agit du sous-réseau auquel les ressources des services Bureau à distance se connectera.
         - **Nom d’utilisateur administrateur** et **mot de passe administrateur**: Entrez les informations d’identification pour un utilisateur d’administrateur est un membre de la **AAD DC Administrators** groupe dans Azure AD.
   
      - **modèle**
         - Supprimer toutes les propriétés de **dnsServers**: après avoir sélectionné **modifier un modèle de** à partir de la page de modèle de démarrage rapide Azure, recherchez « dnsServers » et supprimez la propriété. 

            Par exemple, avant de supprimer le **dnsServers** propriété :
      
            ![Modèle de démarrage rapide Azure avec une propriété dnsSettings](media/rds-remove-dnssettings-before.png)

            Et Voici le fichier même après la suppression de la propriété :

            ![Modèle de démarrage rapide Azure avec une propriété dnsSettings supprimée](media/rds-remove-dnssettings-after.png)
   
   - [Déployer manuellement des services Bureau à distance](rds-deploy-infrastructure.md). 

