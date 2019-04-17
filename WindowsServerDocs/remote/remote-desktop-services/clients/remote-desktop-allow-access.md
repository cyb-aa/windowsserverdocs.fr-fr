---
title: Services Bureau à distance - autoriser l’accès à votre PC
description: En savoir plus sur les options d’accès à distance à votre PC
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
ms.openlocfilehash: d3c85c1c765583eb26cef60ecd245708e0e02772
ms.sourcegitcommit: d3f73936160505a40633ad8dd5931ac5fe3eccdb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/12/2019
ms.locfileid: "9297218"
---
# Services Bureau à distance - autoriser l’accès à votre PC

>S’applique à: Windows 10, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

Vous pouvez utiliser des services Bureau à distance pour vous connecter à et contrôler votre PC à partir d’un appareil distant à l’aide d’un [client Bureau à distance Microsoft](remote-desktop-clients.md) (disponible pour Windows, iOS, Mac OS et Android). Lorsque vous autorisez les connexions à distance à votre PC, vous pouvez utiliser un autre périphérique pour vous connecter à votre PC et avoir accès à l’ensemble de vos applications, les fichiers et ressources réseau comme si vous étiez assis à votre bureau.  

> [!NOTE]
> Vous pouvez utiliser les services Bureau à distance pour se connecter à Windows 10 Professionnel et Enterprise, Windows 8.1 et 8 versions Professionnel, entreprise et Windows 7 Professionnel, entreprise et Édition intégrale et Windows Server plus récentes que celle de Windows Server 2008. Vous ne pouvez pas vous connecter sur un ordinateur exécutant l’Édition familiale (comme Windows 10 famille). 

Pour vous connecter à un ordinateur distant, qu’ordinateur doit être activé, il doit avoir une connexion réseau, Bureau à distance doit être activé, vous devez disposer d’un accès réseau à l’ordinateur distant (il peut s’agir par le biais d’Internet), et vous devez disposer d’autorisations pour se connecter. Pour obtenir l’autorisation pour se connecter, vous devez être sur la liste des utilisateurs. Avant de commencer une connexion, il est judicieux pour rechercher le nom de l’ordinateur que vous vous connectez et pour vous assurer que les connexions Bureau à distance sont autorisées par le biais de son pare-feu.

## Comment activer le Bureau à distance

Le moyen le plus simple pour autoriser l’accès à votre PC à partir d’un appareil distant utilise les options de bureau à distance sous paramètres. Dans la mesure où cette fonctionnalité a été ajoutée dans Windows 10 Fall Creators update (1709), distincte téléchargeable application est également disponible qui fournit une fonctionnalité similaire pour les versions antérieures de Windows. Vous pouvez également utiliser la manière héritée de l’activation des services Bureau à distance, toutefois, cette méthode assure moins fonctionnalités et validation.

### Windows 10 Fall Creators Update (1709) ou une version ultérieure

Vous pouvez configurer votre PC pour l’accès à distance avec quelques étapes simples.
1. Sur l’appareil que vous souhaitez vous connecter, puis **Démarrer** , puis cliquez sur l’icône **paramètres** de gauche.
2. Sélectionnez le groupe de **système de** suivi de l’élément de [**Bureau à distance**](ms-settings:remotedesktop) .
3. Utilisez le curseur pour activer le Bureau à distance.
4. Il est également recommandé de conserver le PC allumé et détectable pour faciliter les connexions. Cliquez sur **Afficher les paramètres** pour activer.
5. Si nécessaire, ajoutez des utilisateurs qui peuvent se connecter à distance en cliquant sur **Sélectionner les utilisateurs qui peuvent accéder à distance à cet ordinateur**.
   1. Les membres du groupe Administrateurs ont automatiquement accès.
6. Notez le nom de ce PC sous **comment se connecter à ce PC**. Vous aurez besoin pour configurer les clients.

### Windows 7 et l’ancienne version de Windows 10

Pour configurer votre PC pour l’accès à distance, téléchargez et exécutez [l’Assistant de bureau à distance Microsoft](https://www.microsoft.com/download/details.aspx?id=50042). Cet assistant met à jour les paramètres de votre système pour activer l’accès à distance, garantit que votre ordinateur est allumé pour les connexions et vérifie que votre pare-feu autorise les connexions Bureau à distance. 

### Toutes les versions de Windows (méthode hérité)

Pour activer le Bureau à distance en utilisant les propriétés de l’ancien système, suivez les instructions pour [se connecter à un autre ordinateur à l’aide de la connexion Bureau à distance](https://windows.microsoft.com/windows/remote-desktop-connection-faq).

## Dois-je activer des services Bureau à distance?

Si vous souhaitez uniquement accéder à votre PC lorsque vous êtes assis physiquement sur le devant de cela, vous n’avez pas besoin d’activer le Bureau à distance. L’activation du Bureau à distance s’ouvre un port sur votre PC qui n’est visible à votre réseau local. Vous devez uniquement activer des services Bureau à distance dans les réseaux approuvés, par exemple, votre maison. Vous souhaitez également activer le Bureau à distance sur n’importe quel ordinateur où l’accès est étroitement contrôlé.

N’oubliez pas que si vous permettez l’accès aux services Bureau à distance, vous permettez à tout le monde dans le groupe d’administrateurs, ainsi que d’autres utilisateurs vous sélectionnez, la possibilité d’accéder à distance à leurs comptes sur l’ordinateur.

Vous devez vous assurer que chaque compte qui a accès à votre PC est configuré avec un mot de passe.

## Pourquoi autoriser les connexions uniquement avec authentification NLA? 
 
Si vous souhaitez restreindre l’accès à votre PC, choisir d’autoriser l’accès uniquement avec réseau au niveau d’authentification (NLA). Lorsque vous activez cette option, les utilisateurs doivent s’authentifier auprès du réseau avant de se connecter à votre PC. Autoriser les connexions des ordinateurs exécutant Bureau à distance avec authentification NLA est une méthode d’authentification plus sécurisée qui peut aider à protéger votre ordinateur contre les logiciels et les utilisateurs malveillants. Pour en savoir plus sur l’authentification NLA et des services Bureau à distance, consultez [Configurer NLA pour les connexions des services Bureau à distance](https://technet.microsoft.com/library/cc732713(v=ws.11).aspx). 

Si à distance, vous vous connectez à un PC sur votre réseau domestique contre en dehors de ce réseau, ne sélectionnez pas cette option.
