---
title: Bureau à distance - autoriser l’accès à votre PC
description: En savoir plus sur les options disponibles pour accéder à distance à votre PC
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: d03dcd307696aea55ab6a1569ab907635994772a
ms.sourcegitcommit: d888e35f71801c1935620f38699dda11db7f7aad
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66804991"
---
# <a name="remote-desktop---allow-access-to-your-pc"></a>Bureau à distance - autoriser l’accès à votre PC

>S’applique à : Windows 10, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

Vous pouvez utiliser le Bureau à distance pour vous connecter à et contrôler votre PC à partir d’un périphérique distant en utilisant un [client Bureau à distance Microsoft](remote-desktop-clients.md) (disponible pour Windows, iOS, macOS et Android). Lorsque vous autorisez les connexions à distance à votre PC, vous pouvez utiliser un autre appareil pour vous connecter à votre PC et avoir accès à l’ensemble de vos applications, les fichiers et les ressources réseau comme si vous étiez assis à votre bureau.  

> [!NOTE]
> Vous pouvez utiliser le Bureau à distance pour se connecter à Windows 10 Professionnel et entreprise, Windows 8.1 et 8 versions Enterprise et Pro, Windows 7 Professionnel, entreprise et Édition intégrale et Windows Server plus récentes que Windows Server 2008. Impossible de se connecter aux ordinateurs exécutant l’Édition familiale (par exemple, Windows 10 Édition familiale). 

Pour vous connecter à un ordinateur distant, qu’ordinateur doit être allumé, il doit avoir une connexion réseau, le Bureau à distance doit être activé, vous devez avoir accès de réseau à l’ordinateur distant (il peut s’agir via Internet), et vous devez être autorisé à se connecter. Pour l’autorisation de se connecter, vous devez être sur la liste des utilisateurs. Avant de commencer une connexion, il est judicieux pour rechercher le nom de l’ordinateur qu'auquel vous vous connectez et s’assurer que les connexions Bureau à distance sont autorisées via son pare-feu.

## <a name="how-to-enable-remote-desktop"></a>Comment activer le Bureau à distance

La façon la plus simple pour autoriser l’accès à votre PC à partir d’un appareil distant est en utilisant les options de bureau à distance sous paramètres. Étant donné que cette fonctionnalité a été ajoutée dans Windows 10 Fall Creators update (1709), téléchargeable distinct application est également disponible qui fournit des fonctionnalités similaires pour les versions antérieures de Windows. Vous pouvez également utiliser l’ancienne procédure d’activation du Bureau à distance, toutefois, cette méthode fournit moins de fonctionnalités et la validation.

### <a name="windows-10-fall-creator-update-1709-or-later"></a>Windows 10 Fall Creator Update (1709) ou version ultérieure

Vous pouvez configurer votre ordinateur pour l’accès à distance en quelques étapes simples.
1. Sur l’appareil que vous souhaitez vous connecter, sélectionnez **Démarrer** puis cliquez sur le **paramètres** icône sur la gauche.
2. Sélectionnez le **système** groupe suivi par le [ **Bureau à distance** ](ms-settings:remotedesktop) élément.
3. Utilisez le curseur pour activer le Bureau à distance.
4. Il est également recommandé de garder le PC éveillés et détectable pour faciliter les connexions. Cliquez sur **afficher les paramètres** à activer.
5. Si nécessaire, ajouter des utilisateurs qui peuvent se connecter à distance en cliquant sur **sélectionner les utilisateurs qui peuvent accéder à distance à ce PC**.
   1. Les membres du groupe Administrateurs ont automatiquement accès.
6. Prenez note du nom de ce PC sous **comment se connecter à ce PC**. Vous en aurez besoin pour configurer les clients.

### <a name="windows-7-and-early-version-of-windows-10"></a>Windows 7 et version antérieure de Windows 10

Pour configurer votre ordinateur pour l’accès à distance, téléchargez et exécutez le [Assistant de bureau à distance Microsoft](https://www.microsoft.com/download/details.aspx?id=50042). Cet assistant met à jour les paramètres de votre système pour activer l’accès à distance, garantit que votre ordinateur est éveillé pour les connexions et vérifie que votre pare-feu autorise les connexions Bureau à distance. 

### <a name="all-versions-of-windows-legacy-method"></a>Toutes les versions de Windows (méthode hérité)

Pour activer le Bureau à distance en utilisant les propriétés système hérité, suivez les instructions pour [se connecter à un autre ordinateur à l’aide de la connexion Bureau à distance](https://windows.microsoft.com/windows/remote-desktop-connection-faq).

## <a name="should-i-enable-remote-desktop"></a>Dois-je activer Bureau à distance ?

Si vous souhaitez uniquement pour accéder à votre PC lorsque vous êtes assis physiquement devant elle, vous n’avez pas besoin d’activer le Bureau à distance. Activation du Bureau à distance s’ouvre un port sur votre PC est visible pour votre réseau local. Vous devez uniquement activer Bureau à distance dans des réseaux approuvés, tels que votre page d’accueil. Vous ne souhaitez activer le Bureau à distance sur n’importe quel ordinateur où l’accès est étroitement contrôlé.

N’oubliez pas que lorsque vous activez l’accès Bureau à distance, vous accordez à tout le monde dans le groupe Administrateurs, ainsi que d’autres utilisateurs vous sélectionnez, la possibilité d’accéder à distance à leurs comptes sur l’ordinateur.

Vous devez vous assurer que chaque compte qui a accès à votre PC est configuré avec un mot de passe fort.

## <a name="why-allow-connections-only-with-network-level-authentication"></a>Pourquoi autoriser les connexions uniquement avec l’authentification au niveau du réseau ? 

Si vous souhaitez restreindre l’accès à votre PC, choisissez d’autoriser l’accès uniquement avec réseau au niveau de l’authentification (NLA). Lorsque vous activez cette option, les utilisateurs doivent s’authentifier auprès du réseau avant de se connecter à votre PC. Autoriser les connexions uniquement sur les ordinateurs exécutant Bureau à distance avec authentification NLA est une méthode d’authentification plus sécurisée qui peut aider à protéger votre ordinateur contre les logiciels et les utilisateurs malveillants. Pour en savoir plus sur l’authentification NLA et Bureau à distance, consultez [configurer l’authentification pour les connexions des services Bureau à distance](https://technet.microsoft.com/library/cc732713(v=ws.11).aspx).

Si à distance, vous vous connectez à un PC sur votre réseau domestique à partir en dehors de ce réseau, ne sélectionnez pas cette option.
