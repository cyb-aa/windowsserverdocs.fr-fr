---
title: Accorder un accès Bureau à distance à votre PC
description: En savoir plus sur les options disponibles pour accéder à distance à votre PC
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0f1557ed-53f7-4333-b023-c8e0f4b58bf4
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 06/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: de08101ed1d4d4527242778d657778f1a16b3dad
ms.sourcegitcommit: 5b055fc1d73375f68149c214152f1d63396dd6ca
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/17/2020
ms.locfileid: "76248401"
---
# <a name="remote-desktop---allow-access-to-your-pc"></a>Accorder un accès Bureau à distance à votre PC

>S'applique à : Windows 10, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

Vous pouvez utiliser Bureau à distance pour vous connecter à votre PC et le contrôler à partir d’un appareil distant avec un [client Bureau à distance Microsoft](remote-desktop-clients.md) (disponible pour Windows, iOS, macOS et Android). Lorsque vous autorisez les connexions à distance à votre PC, vous pouvez utiliser cet appareil pour accéder à l’ensemble de vos applications, fichiers et ressources réseau comme si vous étiez assis à votre bureau.  

> [!NOTE]
> Vous pouvez utiliser le Bureau à distance pour vous connecter à Windows 10 Professionnel et Entreprise, Windows 8.1 et 8 versions Enterprise et Pro, Windows 7 Professionnel, Entreprise et Édition intégrale et les versions de Windows Server plus récentes que Windows Server 2008. Vous ne pouvez pas vous connecter aux ordinateurs exécutant une édition familiale (par exemple, Windows 10 Famille). 

Pour vous connecter à un PC distant, cet ordinateur doit être allumé, il doit avoir une connexion réseau, Bureau à distance doit être activé, vous devez avoir un accès réseau à l’ordinateur distant (par exemple via Internet) et vous devez être autorisé à vous connecter. Pour obtenir l’autorisation de vous connecter, vous devez être sur la liste des utilisateurs. Avant de démarrer la connexion, il est judicieux de rechercher le nom de l’ordinateur auquel vous vous connectez et de vous assurer que les connexions Bureau à distance sont autorisées via son pare-feu.

## <a name="how-to-enable-remote-desktop"></a>Pour activer le Bureau à distance

La façon la plus simple d’autoriser l’accès à votre PC à partir d’un appareil distant est avec les options de Bureau à distance dans Paramètres. Étant donné que cette fonctionnalité a été ajoutée dans Windows 10 Fall Creators update (1709), une application téléchargeable est également disponible pour offrir les mêmes fonctionnalités pour les versions antérieures de Windows. Vous pouvez également utiliser l’ancienne procédure d’activation du Bureau à distance, toutefois, cette méthode fournit moins de fonctionnalités et de validation.

### <a name="windows-10-fall-creator-update-1709-or-later"></a>Windows 10 Fall Creators Update (1709) ou version ultérieure

Vous pouvez configurer votre PC pour l’accès à distance en quelques étapes.
1. Sur l’appareil auquel vous souhaitez vous connecter, sélectionnez **Démarrer** puis cliquez sur l’icône des **paramètres** à gauche.
2. Sélectionnez le groupe **Système**, suivi de l’élément [**Bureau à distance**](ms-settings:remotedesktop).
3. Utilisez le curseur pour activer le Bureau à distance.
4. Il est également recommandé de garder le PC allumé et détectable pour faciliter les connexions. Cliquez sur **Afficher les paramètres** pour l’activer.
5. Si nécessaire, ajoutez les utilisateurs qui peuvent se connecter à distance en cliquant sur **Sélectionner des utilisateurs qui peuvent accéder à distance à ce PC**.
   1. Les membres du groupe Administrateurs ont automatiquement accès.
6. Notez le nom de l’ordinateur sous **Comment se connecter à ce PC**. Vous en aurez besoin pour configurer les clients.

### <a name="windows-7-and-early-version-of-windows-10"></a>Windows 7 et version antérieure de Windows 10

Pour configurer votre PC pour l’accès à distance, téléchargez et exécutez l’[Assistant Bureau à distance Microsoft](https://www.microsoft.com/download/details.aspx?id=50042). Cet Assistant met à jour les paramètres de votre système pour activer l’accès à distance, vérifie que votre ordinateur est actif pour les connexions et vérifie que votre pare-feu autorise les connexions Bureau à distance. 

### <a name="all-versions-of-windows-legacy-method"></a>Toutes les versions de Windows (méthode héritée)

Pour activer le Bureau à distance avec les propriétés système héritées, suivez les instructions pour [Se connecter à un autre ordinateur à l’aide de la connexion Bureau à distance](https://windows.microsoft.com/windows/remote-desktop-connection-faq).

## <a name="should-i-enable-remote-desktop"></a>Est-ce que je dois activer le Bureau à distance ?

Si vous souhaitez uniquement accéder à votre PC quand vous l’avez sous la main, vous n’avez pas besoin d’activer le Bureau à distance. L’activation du Bureau à distance ouvre un port sur votre PC qui le rend visible sur votre réseau local. Vous ne devez activer le Bureau à distance que sur les réseaux de confiance, par exemple votre réseau domestique. Il est également déconseillé d’activer le Bureau à distance sur les PC sur lesquels l’accès est étroitement contrôlé.

N’oubliez pas que lorsque vous activez l’accès Bureau à distance, vous accordez à tout les membres du groupe Administrateurs, ainsi qu’aux autres utilisateurs que vous sélectionnez, la possibilité d’accéder à distance à leur compte sur l’ordinateur.

Vous devez vous assurer que chaque compte qui a accès à votre PC est configuré avec un mot de passe fort.

## <a name="why-allow-connections-only-with-network-level-authentication"></a>Pourquoi autoriser les connexions uniquement avec l’authentification au niveau du réseau ? 

Si vous souhaitez restreindre l’accès à votre PC, choisissez d’autoriser l’accès uniquement avec authentification au niveau du réseau. Lorsque vous activez cette option, les utilisateurs doivent s’authentifier auprès du réseau avant de pouvoir se connecter à votre PC. Le fait d’autoriser les connexions uniquement sur les ordinateurs exécutant Bureau à distance avec l’authentification au niveau du réseau est une méthode d’authentification plus sécurisée qui peut aider à protéger votre ordinateur des logiciels et des utilisateurs malveillants. Pour en savoir plus sur l’authentification au niveau du réseau et sur le Bureau à distance, consultez [Configurer l’authentification au niveau du réseau pour les connexions des services Bureau à distance](https://technet.microsoft.com/library/cc732713(v=ws.11).aspx).

Si vous vous connectez à distance à un PC sur votre réseau domestique depuis l’extérieur, ne sélectionnez pas cette option.
