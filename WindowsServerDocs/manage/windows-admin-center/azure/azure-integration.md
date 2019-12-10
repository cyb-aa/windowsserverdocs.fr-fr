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
ms.openlocfilehash: d8758342752ac71c5b700682d4c0f4317dc4cb4e
ms.sourcegitcommit: e817a130c2ed9caaddd1def1b2edac0c798a6aa2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/09/2019
ms.locfileid: "74945219"
---
# <a name="configuring-azure-integration"></a>Configuration de l’intégration d’Azure

>S’applique à : Windows Admin Center, Windows Admin Center Preview

Le centre d’administration Windows prend en charge plusieurs fonctionnalités facultatives qui s’intègrent avec les services Azure. [En savoir plus sur les options d’intégration Azure disponibles dans le centre d’administration Windows.](../plan/azure-integration-options.md)

Pour permettre à la passerelle du centre d’administration Windows de communiquer avec Azure pour tirer parti de l’authentification Azure Active Directory pour l’accès à la passerelle, ou pour créer des ressources Azure en votre nom (par exemple, pour protéger des machines virtuelles gérées dans le centre d’administration Windows à l’aide du site Azure Récupération), vous devez d’abord inscrire votre passerelle du centre d’administration Windows auprès d’Azure. Vous ne devez effectuer cette opération qu’une seule fois pour votre passerelle du centre d’administration Windows. le paramètre est conservé lorsque vous mettez à jour votre passerelle vers une version plus récente.

## <a name="register-your-gateway-with-azure"></a>Inscrire votre passerelle auprès d’Azure

La première fois que vous essayez d’utiliser une fonctionnalité d’intégration Azure dans le centre d’administration Windows, vous êtes invité à inscrire la passerelle sur Azure. Vous pouvez également inscrire la passerelle en accédant à l’onglet **Azure** dans les paramètres du centre d’administration Windows. Notez que seuls les administrateurs de la passerelle du centre d’administration Windows peuvent inscrire la passerelle du centre d’administration Windows auprès d’Azure. [En savoir plus sur les autorisations d’administrateur et d’utilisateur du centre d’administration Windows](../configure/user-access-control.md#gateway-access-role-definitions).

Les étapes guidées du produit permettent de créer un Azure AD application dans votre annuaire, ce qui permet au centre d’administration Windows de communiquer avec Azure. Pour afficher l’application de Azure AD qui est automatiquement créée, accédez à l’onglet **Azure** des paramètres du centre d’administration Windows. La **vue dans** le lien hypertexte Azure vous permet d’afficher l’application Azure ad dans le portail Azure. 

L’application Azure AD créée est utilisée pour tous les points de l’intégration d’Azure dans le centre d’administration Windows, y compris l' [authentification Azure ad à la passerelle](../configure/user-access-control.md#azure-active-directory). Le centre d’administration Windows configure automatiquement les autorisations nécessaires pour créer et gérer des ressources Azure en votre nom :

- Azure Active Directory Graph
    - Directory.AccessAsUser.All
    - User.Read
- Gestion des services Azure
    - user_impersonation

### <a name="manual-azure-ad-app-configuration"></a>Configuration manuelle de l’application Azure AD

Si vous souhaitez configurer manuellement une application Azure AD, au lieu d’utiliser l’application Azure AD créée automatiquement par le centre d’administration Windows pendant le processus d’inscription de la passerelle, vous devez effectuer les opérations suivantes.

1. Accordez à l’application Azure AD les autorisations d’API requises mentionnées ci-dessus. Pour ce faire, accédez à votre application Azure AD dans le Portail Azure. Accédez au Portail Azure > **Azure Active Directory** > **inscriptions d’applications** > sélectionnez l’application Azure ad que vous souhaitez utiliser. Puis sur l’onglet **autorisations d’API** et ajoutez les autorisations d’API mentionnées ci-dessus.
2. Ajoutez l’URL de la passerelle du centre d’administration Windows aux URL de réponse (également appelées URI de redirection). Accédez à votre application Azure AD, puis accédez au **manifeste**. Recherchez la clé « replyUrlsWithType » dans le manifeste. Dans la clé, ajoutez un objet contenant deux clés : « URL » et « type ». La clé « URL » doit avoir une valeur de l’URL de la passerelle du centre d’administration Windows, en ajoutant un caractère générique à la fin. La clé « type » de clé doit avoir la valeur « Web ». Exemple :

    ```json
    "replyUrlsWithType": [
            {
                    "url": "http://localhost:6516/*",
                    "type": "Web"
            }
    ],
    ```
