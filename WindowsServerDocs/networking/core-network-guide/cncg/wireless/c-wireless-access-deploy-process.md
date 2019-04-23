---
title: Processus de déploiement de l’accès sans fil
description: Cette rubrique fait partie de la « Accès sans fil authentifié 802. 1 mot de passe de déployer X » du guide de mise en réseau de Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 2555f238-926e-4b20-9bfb-9774831062da
author: shortpatti
ms.author: pashort
ms.openlocfilehash: 6a286cf10e066043ee6f514bbf468bfb2b13f162
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879840"
---
# <a name="wireless-access-deployment-process"></a>Processus de déploiement de l’accès sans fil

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Le processus que vous utilisez pour déployer l’accès sans fil se produit dans ces étapes :

## <a name="stage-1--ap-deployment"></a>Étape 1 : déploiement de l’Asie-Pacifique

Planifier, déployer et configurer vos points d’accès pour la connectivité du client sans fil et pour une utilisation avec le serveur NPS. Selon votre préférence et les dépendances réseau, vous pouvez pré-créez\-configurer les paramètres sur vos points d’accès sans fil avant de les installer sur votre réseau, ou vous pouvez les configurer à distance après l’installation.

## <a name="stage-2--adds-group-configuration"></a>Étape 2 : Configuration d’AD DS groupe

Dans AD DS, vous devez créer des groupes de sécurité d’un ou plusieurs utilisateurs sans fil.

Ensuite, identifiez les utilisateurs qui ont un accès sans fil au réseau.

Enfin, ajouter des utilisateurs aux groupes de sécurité sans fil aux utilisateurs que vous avez créé.

>[!NOTE]
>Par défaut, le **l’autorisation d’accès réseau** dans les propriétés d’accès à distance du compte d’utilisateur est défini avec le paramètre **contrôler l’accès via la stratégie de réseau de NPS**. Sauf si vous avez des raisons particulières de modifier ce paramètre, il est recommandé de conserver la valeur par défaut. Cela vous permet de contrôler l’accès réseau via les stratégies de réseau que vous configurez dans NPS.

## <a name="stage-3--group-policy-configuration"></a>Étape 3 : Configuration de stratégie de groupe

Configurer le réseau sans fil \(IEEE 802.11\) extension des stratégies de stratégie de groupe à l’aide de l’éditeur de gestion de stratégie de groupe Microsoft Management Console \(MMC\).

Pour configurer le domaine\-ordinateurs membres en utilisant les paramètres dans les stratégies de réseau sans fil, vous devez appliquer stratégie de groupe. Lorsqu’un ordinateur est tout d’abord joint au domaine, stratégie de groupe est automatiquement appliquée. Si des modifications sont apportées à la stratégie de groupe, les nouveaux paramètres sont automatiquement appliqués :

- Par la stratégie de groupe au préalable\-déterminé d’intervalles

- Si un utilisateur de domaine se déconnecte, puis de nouveau une session sur le réseau

- En redémarrant l’ordinateur client et en ouvrir une session sur le domaine

Vous pouvez également forcer la stratégie de groupe pour actualiser lorsque vous êtes connecté à un ordinateur en exécutant la commande **gpupdate** à l’invite de commandes.

## <a name="stage-4--nps-configuration"></a>Étape 4 : configuration de serveur NPS

Utilisez un Assistant de configuration dans NPS pour ajouter des points d’accès sans fil en tant que clients RADIUS et pour créer les stratégies réseau que NPS utilise lors du traitement des demandes de connexion.

Lorsque vous utilisez l’Assistant pour créer les stratégies de réseau, spécifiez PEAP, ainsi que le type EAP le groupe de sécurité utilisateurs sans fil qui a été créé dans la deuxième étape.

## <a name="stage-5--deploy-wireless-clients"></a>Étape 5 : déployer des clients sans fil

Utilisez les ordinateurs clients pour se connecter au réseau.

Pour les ordinateurs membres du domaine qui peuvent se connecter au réseau local câblé, les paramètres de configuration sans fil nécessaires sont automatiquement appliquées lors de la stratégie de groupe est actualisée.

Si vous avez activé le paramètre de réseau sans fil \(IEEE 802.11\) stratégies pour se connecter automatiquement lorsque l’ordinateur est dans la plage de réseau sans fil, votre sans fil, le domaine de diffusion\-enverra ensuite par les ordinateurs joints tente automatiquement de se connecter au réseau local sans fil.

Pour vous connecter au réseau sans fil, les utilisateurs doivent fournir uniquement de leurs domaine nom et mot de passe informations d’identification utilisateur lorsque vous y êtes invité par Windows.

Pour planifier votre déploiement de l’accès sans fil, consultez [planification du déploiement de l’accès sans fil](d-wireless-access-planning.md).
