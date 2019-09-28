---
title: Configuration de l’intégration d’Azure
description: Configuration du centre d’administration Windows Azure Integration (projet Honolulu). Connexion de votre passerelle du centre d’administration Windows à Azure.
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 09/19/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 448a8fb3e4340752b673b06f86d5d49211b6b147
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357357"
---
# <a name="configuring-azure-integration"></a>Configuration de l’intégration d’Azure

>S'applique à : Windows Admin Center, Windows Admin Center Preview

Le centre d’administration Windows prend en charge plusieurs fonctionnalités facultatives qui s’intègrent avec les services Azure. [En savoir plus sur les options d’intégration Azure disponibles dans le centre d’administration Windows.](../plan/azure-integration-options.md)

Pour permettre à la passerelle du centre d’administration Windows de communiquer avec Azure pour tirer parti de l’authentification Azure Active Directory pour l’accès à la passerelle, ou pour créer des ressources Azure en votre nom (par exemple, pour protéger des machines virtuelles gérées dans le centre d’administration Windows à l’aide du site Azure Récupération), vous devez d’abord inscrire votre passerelle du centre d’administration Windows auprès d’Azure. Vous ne devez effectuer cette opération qu’une seule fois pour votre passerelle du centre d’administration Windows. le paramètre est conservé lorsque vous mettez à jour votre passerelle vers une version plus récente.

## <a name="register-your-gateway-with-azure"></a>Inscrire votre passerelle auprès d’Azure

La première fois que vous essayez d’utiliser une fonctionnalité d’intégration Azure dans le centre d’administration Windows, vous êtes invité à inscrire la passerelle sur Azure. Vous pouvez également inscrire la passerelle en accédant à l’onglet **Azure** dans les paramètres du centre d’administration Windows.

Les étapes guidées du produit permettent de créer un Azure AD application dans votre annuaire, ce qui permet au centre d’administration Windows de communiquer avec Azure. Pour afficher l’application de Azure AD qui est automatiquement créée, accédez à l’onglet **Azure** des paramètres du centre d’administration Windows. La **vue dans** le lien hypertexte Azure vous permet d’afficher l’application Azure ad dans le portail Azure. 

L’application Azure AD créée est utilisée pour tous les points de l’intégration d’Azure dans le centre d’administration Windows, y compris l' [authentification Azure ad à la passerelle](../configure/user-access-control.md#azure-active-directory).