---
title: Authentification par clé publique d'appareil joint au domaine
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 7bd17803-6e42-4a3b-803f-e47c74725813
manager: alanth
author: michikos
ms.technology: security-authentication
ms.date: 08/18/2017
ms.openlocfilehash: 80906e7cfe3740200938704a4b4eaf0759af303a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59885030"
---
# <a name="domain-joined-device-public-key-authentication"></a>Authentification par clé publique d'appareil joint au domaine

>S’applique à : Windows Server 2016, Windows 10

Kerberos prise en charge pour les appareils joints au domaine à se connecter en utilisant un début de certificat avec Windows Server 2012 et Windows 8. Cette modification permet à des fournisseurs tiers 3e créer des solutions pour approvisionner et initialiser des certificats pour les appareils joints au domaine à utiliser pour l’authentification de domaine. 

## <a name="automatic-public-key-provisioning"></a>Approvisionnement clé publique automatique

Appareils joints au domaine depuis Windows 10 version 1507 et Windows Server 2016, déployez automatiquement une clé publique liée à un contrôleur de domaine (DC) Windows Server 2016. Une fois une clé est configurée, Windows peut utiliser l’authentification par clé publique au domaine.

### <a name="public-key-generation"></a>Génération de clés publiques
Si l’appareil est en cours d’exécution de Credential Guard, puis créer une clé publique est protégée par Credential Guard. 

Si Credential Guard n’est pas disponible et est un module de plateforme sécurisée, puis créer une clé publique est protégée par le TPM. 

Si aucun n’est disponible, puis une clé n’est pas générée et l’appareil s’authentifier à l’aide du mot de passe.

### <a name="provisioning-computer-account-public-key"></a>Clé publique du compte ordinateur d’approvisionnement
Lorsque Windows démarre, il vérifie si une clé publique a été configurée pour son compte d’ordinateur. Si ce n’est pas le cas, puis il génère une clé publique liée et le configure pour son compte dans Active Directory à l’aide d’un Windows Server 2016 ou un contrôleur de domaine plus élevée. Si tous les contrôleurs de domaine sont bas niveau, aucune clé n’est configurée.

### <a name="configuring-device-to-only-use-public-key"></a>Configuration de périphérique à utiliser uniquement la clé publique
Si le paramètre de stratégie de groupe **prise en charge pour l’authentification des appareils à l’aide du certificat** a la valeur **Force**, puis l’appareil doit trouver un contrôleur de domaine qui exécute Windows Server 2016 ou version ultérieure pour s’authentifier. Le paramètre se trouve sous modèles d’administration > système > Kerberos.

### <a name="configuring-device-to-only-use-password"></a>Configuration de périphérique à utiliser uniquement le mot de passe
Si le paramètre de stratégie de groupe **prise en charge pour l’authentification des appareils à l’aide du certificat** est désactivé, mot de passe est toujours utilisé. Le paramètre se trouve sous modèles d’administration > système > Kerberos.

## <a name="domain-joined-device-authentication-using-public-key"></a>Authentification des appareils joints au domaine à l’aide de la clé publique
Lorsque Windows dispose d’un certificat pour l’appareil joint au domaine, Kerberos s’authentifie d’abord en utilisant le certificat et sur les nouvelles tentatives d’échec avec mot de passe. Cela permet à l’appareil pour s’authentifier auprès des contrôleurs de domaine de bas niveau.

Étant donné que les clés publiques approvisionnés automatiquement un certificat auto-signé, la validation du certificat échoue sur les contrôleurs de domaine qui ne prennent pas en charge le mappage de confiance de clé de compte. Par défaut, Windows effectue une nouvelle tentative d’authentification à l’aide du mot de passe de domaine de l’appareil.


