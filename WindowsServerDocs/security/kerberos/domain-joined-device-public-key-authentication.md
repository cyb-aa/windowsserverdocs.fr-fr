---
title: "Authentification par clé publique appareil joint au domaine"
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 7bd17803-6e42-4a3b-803f-e47c74725813
manager: alanth
author: michikos
ms.technology: security-authentication
ms.date: 8/18/2017
ms.openlocfilehash: 65f39f4900fcd99ef90b160d3db7099edb73bc1c
ms.sourcegitcommit: e94838df702ff0135ebe7c179b228aa67b772d35
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/28/2017
---
# <a name="domain-joined-device-public-key-authentication"></a>Authentification par clé publique appareil joint au domaine

>S’applique à: Windows Server2016, Windows10

Kerberos prise en charge pour les appareils joints au domaine de connexion à l’aide d’un début certificat avec Windows Server2012 et Windows8. Cette modification permet 3efournisseurs tiers créer des solutions pour configurer et d’initialiser des certificats pour les appareils joints au domaine à utiliser pour l’authentification de domaine. 

## <a name="automatic-public-key-provisioning"></a>Configuration clé publique automatique

À partir de Windows10 version1507 et Windows Server2016, les appareils joints au domaine configurer automatiquement une clé publique liée à un contrôleur de domaine Windows Server2016 (DC). Une fois qu’une clé est configurée, Windows peut utiliser l’authentification de clé publique au domaine.

### <a name="public-key-generation"></a>Génération de clés publiques
Si l’appareil est en cours d’exécution Credential Guard, une clé publique est créée protégée par Credential Guard. 

Si Credential Guard n’est pas disponible et un module de plateforme sécurisée, une clé publique est créée protégée par le module de plateforme sécurisée. 

Si aucun n’est disponible, puis une clé n’est pas générée et l’appareil s’authentifier à l’aide du mot de passe.

### <a name="provisioning-computer-account-public-key"></a>Mise en service la clé publique du compte ordinateur
Lorsque Windows démarre, il vérifie si une clé publique a été configurée pour son compte d’ordinateur. Si ce n’est pas le cas, puis il génère une clé publique liée et le configure pour son compte dans ActiveDirectory à l’aide d’un Windows Server2016 ou un contrôleur de domaine supérieur. Si tous les contrôleurs de domaine sont de niveau inférieur, aucune clé n’a été configuré.

### <a name="configuring-device-to-only-use-public-key"></a>La configuration de périphérique à utiliser uniquement la clé publique
Si le paramètre de stratégie de groupe **prise en charge pour l’authentification des appareils à l’aide du certificat** est défini sur **Force**, puis le périphérique a besoin pour trouver un contrôleur de domaine qui exécute Windows Server2016 ou version ultérieure pour s’authentifier. Le paramètre est sous modèles d’administration > système > Kerberos.

### <a name="configuring-device-to-only-use-password"></a>La configuration de périphérique à utiliser uniquement le mot de passe
Si le paramètre de stratégie de groupe **prise en charge pour l’authentification des appareils à l’aide du certificat** est désactivé, mot de passe est toujours utilisé. Le paramètre est sous modèles d’administration > système > Kerberos.

## <a name="domain-joined-device-authentication-using-public-key"></a>Authentification d’appareil joints au domaine à l’aide de la clé publique
Lorsque Windows dispose d’un certificat pour l’appareil joint au domaine, Kerberos authentifie tout d’abord l’utilisation du certificat et de tentatives d’échec avec mot de passe. Cela permet à l’appareil pour s’authentifier auprès des contrôleurs de domaine de niveau inférieur.

Dans la mesure où les clés publiques configurés automatiquement disposent d’un certificat auto-signé, la validation du certificat échoue sur les contrôleurs de domaine qui ne prennent pas en charge approbation de la clé de mappage de compte. Par défaut, Windows tente de renvoyer l’authentification par mot de passe de domaine de l’appareil.


