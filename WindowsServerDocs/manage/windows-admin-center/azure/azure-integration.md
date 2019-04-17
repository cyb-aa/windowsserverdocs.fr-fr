---
title: Configuration de l’intégration Azure
description: Configuration d’Azure intégration Windows Admin Center (projet Honolulu). Connectez votre passerelle Windows Admin Center à Azure.
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 09/19/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 38b973680463cdebc1b3168e447abfcf1caba6b5
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296919"
---
# Configuration de l’intégration Azure

>S’applique à: Windows Admin Center, Windows Admin Center Preview

Windows Admin Center prend en charge plusieurs fonctionnalités facultatives qui s’intègrent avec les services Azure. [Apprenez-en plus sur les options d’intégration Azure disponibles avec Windows Admin Center.](../plan/azure-integration-options.md)

Pour permettre à la passerelle Windows Admin Center communiquer avec Azure pour tirer parti de l’authentification Azure Active Directory pour l’accès de passerelle, ou pour créer des ressources Azure pour votre compte (par exemple, pour protéger les machines virtuelles gérées dans Windows Admin Center à l’aide d’Azure Site Récupération), vous devez d’abord inscrire votre passerelle Windows Admin Center avec Azure. Vous devez uniquement effectuer cette opération une fois pour votre passerelle Windows Admin Center - le paramètre est conservé lorsque vous mettez à jour votre passerelle vers une version plus récente.

## Inscrire votre passerelle avec Azure

La première fois que vous essayez d’utiliser une fonctionnalité d’intégration d’Azure dans Windows Admin Center, vous devrez inscrire la passerelle vers Azure. Vous pouvez également enregistrer la passerelle en accédant à l’onglet **Azure** dans les paramètres de Windows Admin Center.

Les étapes de produit guidés crée une application Azure AD dans votre répertoire, ce qui permet à Windows Admin Center communiquer avec Azure. Pour afficher l’application Azure AD qui est automatiquement créée, accédez à l’onglet **Azure** des paramètres de Windows Admin Center. Le lien hypertexte **vue dans Azure** vous permet d’afficher l’application Azure AD dans le portail Azure. 

L’application Azure AD créée est utilisée pour tous les points d’intégration Azure dans Windows Admin Center, y compris [l’authentification Azure AD à la passerelle](../configure/user-access-control.md#azure-active-directory).