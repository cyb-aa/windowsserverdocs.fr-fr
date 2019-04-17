---
title: Processus de déploiement d’accès sans fil
description: Cette rubrique fait partie du guide de mise en réseau de Windows Server2016 «Accès sans fil authentifié 802. 1-mot de passe de déployer X»
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 2555f238-926e-4b20-9bfb-9774831062da
author: shortpatti
ms.author: pashort
ms.openlocfilehash: fcdbe796e4d530604dfec448e817eab877d34c4f
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="wireless-access-deployment-process"></a>Processus de déploiement d’accès sans fil

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Le processus que vous utilisez pour déployer l’accès sans fil se produit dans ces étapes:

## <a name="stage-1--ap-deployment"></a>Étape 1: déploiement de point d’accès

Planifier, déployer et configurer vos points d’accès pour la connectivité des clients sans fil et pour une utilisation avec le serveur NPS. Selon votre préférence et les dépendances réseau, vous pouvez soit pre\-configurer les paramètres sur vos points d’accès sans fil avant leur installation sur votre réseau, ou vous pouvez les configurer à distance après l’installation.

## <a name="stage-2--ad-ds-group-configuration"></a>Étape 2: Configuration d’AD DS groupe

Dans AD DS, vous devez créer des groupes de sécurité un ou plusieurs utilisateurs sans fil.

Ensuite, identifier les utilisateurs qui sont autorisés à accéder au réseau sans fil.

Enfin, ajoutez les utilisateurs aux groupes de sécurité appropriés aux utilisateurs sans fil que vous avez créé.

>[!NOTE]
>Par défaut, le **autorisation d’accès réseau** paramètre dans les propriétés d’accès du compte d’utilisateur est configuré avec le paramètre **contrôler l’accès via la stratégie de réseau NPS**. Sauf si vous avez des raisons spécifiques pour modifier ce paramètre, il est recommandé de conserver la valeur par défaut. Cela vous permet de contrôler l’accès au réseau par les stratégies de réseau que vous configurez dans NPS.

## <a name="stage-3--group-policy-configuration"></a>Étape 3: Configuration de stratégie de groupe

Configurer le réseau sans fil \ IEEE (802.11\) extension des stratégies de stratégie de groupe à l’aide de l’éditeur de gestion de stratégie de groupe Microsoft Management Console \(MMC\).

Pour configurer les ordinateurs membres du domain\ en utilisant les paramètres dans les stratégies de réseau sans fil, vous devez appliquer la stratégie de groupe. Lorsqu’un ordinateur est tout d’abord joint au domaine, stratégie de groupe est automatiquement appliquée. Si des modifications sont apportées à la stratégie de groupe, les nouveaux paramètres sont automatiquement appliquées:

- Par la stratégie de groupe à des intervalles pre\-déterminé

- Si un utilisateur de domaine se déconnecte, puis de nouveau une session sur le réseau

- En redémarrant l’ordinateur client et en ouvrant une session sur le domaine

Vous pouvez également forcer la stratégie de groupe pour actualiser lorsque vous êtes connecté à un ordinateur en exécutant la commande **gpupdate** à l’invite de commandes.

## <a name="stage-4--nps-server-configuration"></a>Étape 4: configuration du serveur NPS

Utilisez un Assistant de configuration dans NPS pour ajouter des points d’accès sans fil en tant que clients RADIUS et pour créer les stratégies de réseau que NPS utilise lors du traitement des demandes de connexion.

Lorsque vous utilisez l’Assistant pour créer les stratégies de réseau, spécifiez PEAP, ainsi que le type EAP, le groupe de sécurité utilisateurs sans fil qui a été créé dans la deuxième étape.

## <a name="stage-5--deploy-wireless-clients"></a>Étape 5: déploiement de clients sans fil

Utilisez les ordinateurs clients pour se connecter au réseau.

Pour les ordinateurs membres du domaine qui peuvent se connecter au réseau local câblé, les paramètres de configuration sans fil nécessaires sont automatiquement appliqués lors de la stratégie de groupe est actualisée.

Si vous avez activé le paramètre de réseau sans fil \ IEEE (802.11\) stratégies pour se connecter automatiquement lorsque l’ordinateur est automatiquement à portée de diffusion du réseau sans fil, votre sera ordinateurs appartenant à un domain\, sans fil, puis essayez de vous connecter au réseau local sans fil.

Pour vous connecter au réseau sans fil, les utilisateurs doivent fournir uniquement de leurs domaine nom et mot de passe informations d’identification utilisateur lorsque vous y êtes invité par Windows.

Pour planifier votre déploiement de l’accès sans fil, consultez [planification du déploiement de l’accès sans fil](d-wireless-access-planning.md).
