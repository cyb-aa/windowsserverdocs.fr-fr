---
title: Authentification par clé publique d'appareil joint au domaine
ms.prod: windows-server
ms.topic: article
ms.assetid: 7bd17803-6e42-4a3b-803f-e47c74725813
manager: alanth
author: michikos
ms.technology: security-authentication
ms.date: 08/18/2017
ms.openlocfilehash: 450d3e64ff753a718c2e72e69cb60d51c8c18f78
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856332"
---
# <a name="domain-joined-device-public-key-authentication"></a>Authentification par clé publique d'appareil joint au domaine

>S’applique à : Windows Server 2016, Windows 10

Kerberos a ajouté la prise en charge des appareils joints à un domaine pour se connecter à l’aide d’un certificat à partir de Windows Server 2012 et Windows 8. Cette modification permet aux fournisseurs tiers de créer des solutions pour approvisionner et initialiser des certificats pour les appareils joints à un domaine à utiliser pour l’authentification de domaine. 

## <a name="automatic-public-key-provisioning"></a>Approvisionnement automatique des clés publiques

À compter de Windows 10 version 1507 et de Windows Server 2016, les appareils joints à un domaine approvisionnent automatiquement une clé publique liée à un contrôleur de domaine Windows Server 2016 (DC). Une fois qu’une clé est approvisionnée, Windows peut utiliser l’authentification par clé publique dans le domaine.

### <a name="key-generation"></a>Génération de clé
Si l’appareil exécute Credential Guard, une paire de clés publique/privée est créée protégée par Credential Guard. 

Si Credential Guard n’est pas disponible et qu’un module de plateforme sécurisée est, une paire de clés publique/privée est créée protégée par le module de plateforme sécurisée. 

Si aucun n’est disponible, une paire de clés n’est pas générée et l’appareil peut uniquement s’authentifier à l’aide du mot de passe.

### <a name="provisioning-computer-account-public-key"></a>Configuration de la clé publique du compte d’ordinateur
Lorsque Windows démarre, il vérifie si une clé publique est approvisionnée pour son compte d’ordinateur. Si ce n’est pas le cas, il génère une clé publique liée et le configure pour son compte dans Active Directory à l’aide d’un contrôleur de domaine Windows Server 2016 ou version ultérieure. Si tous les contrôleurs de service sont de niveau inférieure, aucune clé n’est approvisionnée.

### <a name="configuring-device-to-only-use-public-key"></a>Configuration de l’appareil pour qu’il utilise uniquement la clé publique
Si le paramètre stratégie de groupe **prise en charge de l’authentification des appareils à l’aide du certificat** est défini sur **force**, alors l’appareil doit trouver un contrôleur de périphérique qui exécute Windows Server 2016 ou une version ultérieure pour s’authentifier. Le paramètre se trouve sous Modèles d’administration système > > Kerberos.

### <a name="configuring-device-to-only-use-password"></a>Configuration de l’appareil pour utiliser uniquement le mot de passe
Si le paramètre stratégie de groupe **prise en charge de l’authentification des appareils à l’aide du certificat** est désactivé, le mot de passe est toujours utilisé. Le paramètre se trouve sous Modèles d’administration système > > Kerberos.

## <a name="domain-joined-device-authentication-using-public-key"></a>Authentification d’appareil joint à un domaine à l’aide d’une clé publique
Lorsque Windows dispose d’un certificat pour l’appareil joint à un domaine, Kerberos s’authentifie d’abord à l’aide du certificat et en cas d’échec de nouvelles tentatives avec un mot de passe. Cela permet à l’appareil de s’authentifier auprès des contrôleurs de gamme de niveau supérieur.

Étant donné que les clés publiques automatiquement approvisionnées ont un certificat auto-signé, la validation du certificat échoue sur les contrôleurs de domaine qui ne prennent pas en charge le mappage de compte de confiance de clé. Par défaut, Windows effectue une nouvelle tentative d’authentification à l’aide du mot de passe de domaine de l’appareil.


