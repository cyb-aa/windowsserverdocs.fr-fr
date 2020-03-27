---
title: Processus de déploiement de l’accès sans fil
description: Cette rubrique fait partie du Guide de mise en réseau de Windows Server 2016 « déployer l’accès sans fil authentifié 802.1 X basé sur un mot de passe »
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 2555f238-926e-4b20-9bfb-9774831062da
author: eross-msft
ms.author: lizross
ms.openlocfilehash: 30e3da7e1365585bf9dc5ff34a72a367e1ed28f7
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80318082"
---
# <a name="wireless-access-deployment-process"></a>Processus de déploiement de l’accès sans fil

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Le processus que vous utilisez pour déployer l’accès sans fil se produit dans les étapes suivantes :

## <a name="stage-1--ap-deployment"></a>Étape 1 : déploiement d’AP

Planifiez, déployez et configurez vos APs pour la connectivité des clients sans fil et pour une utilisation avec NPS. Selon vos préférences et vos dépendances réseau, vous pouvez pré\-configurer les paramètres sur vos points d’accès sans fil avant de les installer sur votre réseau, ou vous pouvez les configurer à distance après l’installation.

## <a name="stage-2--adds-group-configuration"></a>Étape 2 : configuration du groupe de AD DS

Dans AD DS, vous devez créer un ou plusieurs groupes de sécurité utilisateurs sans fil.

Ensuite, identifiez les utilisateurs autorisés à accéder au réseau sans fil.

Enfin, ajoutez les utilisateurs aux groupes de sécurité des utilisateurs sans fil appropriés que vous avez créés.

>[!NOTE]
>Par défaut, le paramètre d' **autorisation d’accès réseau** dans les propriétés d’accès à distance du compte d’utilisateur est configuré avec le paramètre **contrôler l’accès via la stratégie de réseau NPS**. À moins que vous n’ayez des raisons spécifiques de modifier ce paramètre, il est recommandé de conserver la valeur par défaut. Cela vous permet de contrôler l’accès réseau via les stratégies réseau que vous configurez dans NPS.

## <a name="stage-3--group-policy-configuration"></a>Étape 3 : configuration de la stratégie de groupe

Configurez le réseau sans fil \(l’extension des stratégies de\) IEEE 802,11 de stratégie de groupe à l’aide de la Éditeur de gestion des stratégies de groupe \(MMC\)de la console de gestion Microsoft.

Pour configurer des ordinateurs membres du domaine\-à l’aide des paramètres des stratégies de réseau sans fil, vous devez appliquer les stratégie de groupe. Lorsqu’un ordinateur est d’abord joint au domaine, stratégie de groupe est appliqué automatiquement. Si des modifications sont apportées à stratégie de groupe, les nouveaux paramètres sont appliqués automatiquement :

- Par stratégie de groupe à des intervalles déterminés au préalable\-

- Si un utilisateur de domaine se déconnecte puis revient au réseau

- En redémarrant l’ordinateur client et en ouvrant une session sur le domaine

Vous pouvez également forcer l’actualisation de stratégie de groupe lors de la connexion à un ordinateur en exécutant la commande **gpupdate** à l’invite de commandes.

## <a name="stage-4--nps-configuration"></a>Étape 4 : configuration de NPS

Utilisez un assistant de configuration dans NPS pour ajouter des points d’accès sans fil en tant que clients RADIUS et pour créer les stratégies réseau que NPS utilise lors du traitement des demandes de connexion.

Lorsque vous utilisez l’Assistant pour créer les stratégies réseau, spécifiez PEAP comme type EAP et le groupe de sécurité utilisateurs sans fil créé lors de la deuxième étape.

## <a name="stage-5--deploy-wireless-clients"></a>Étape 5 : déployer des clients sans fil

Utilisez les ordinateurs clients pour se connecter au réseau.

Pour les ordinateurs membres du domaine qui peuvent se connecter au réseau local câblé, les paramètres de configuration sans fil nécessaires sont automatiquement appliqués lorsque stratégie de groupe est actualisé.

Si vous avez activé le paramètre des stratégies de réseau sans fil \(IEEE 802,11\) pour se connecter automatiquement lorsque l’ordinateur se trouve dans la plage de diffusion du réseau sans fil, vos ordinateurs sans fil\-joints à un domaine sont automatiquement tentés de se connecter au réseau local sans fil.

Pour vous connecter au réseau sans fil, les utilisateurs doivent uniquement fournir leurs informations d’identification de nom d’utilisateur et de mot de passe de domaine lorsqu’ils y sont invités par Windows.

Pour planifier votre déploiement de l’accès sans fil, consultez [planification du déploiement de l’accès sans fil](d-wireless-access-planning.md).
