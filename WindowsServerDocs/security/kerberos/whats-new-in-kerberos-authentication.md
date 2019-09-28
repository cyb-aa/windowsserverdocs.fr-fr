---
title: What's New in Kerberos Authentication
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: 7bd17803-6e42-4a3b-803f-e47c74725813
manager: alanth
author: justinha
ms.technology: security-authentication
ms.date: 11/09/2016
ms.openlocfilehash: a0916abf1076b5f791a856f0c85f54ad17f6d64c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403473"
---
# <a name="whats-new-in-kerberos-authentication"></a>What's New in Kerberos Authentication

>S'applique à : Windows Server 2016 et Windows 10

## <a name="kdc-support-for-public-key-trust-based-client-authentication"></a>Prise en charge du KDC pour l’authentification du client basé sur une confiance de clé publique

À partir de Windows Server 2016, les KDC prennent en charge un mode de mappage de clé publique. Si la clé publique est approvisionnée pour un compte, le KDC prend en charge explicitement le protocole PKInit Kerberos à l’aide de cette clé. Étant donné qu’il n’existe aucune validation de certificat, les certificats auto-signés sont pris en charge et l’assurance du mécanisme d’authentification n’est pas prise en charge.

L’approbation de clé est préférable lorsqu’elle est configurée pour un compte quel que soit le paramètre UseSubjectAltName.

## <a name="kerberos-client-and-kdc-support-for-rfc-8070-pkinit-freshness-extension"></a>Prise en charge du client Kerberos et du KDC pour l’extension d’actualisation de PKInit RFC 8070

À compter de Windows 10, version 1607 et Windows Server 2016, les clients Kerberos essaient l' [extension d’actualisation de PKInit RFC 8070](https://datatracker.ietf.org/doc/draft-ietf-kitten-pkinit-freshness/) pour les connexions basées sur des clés publiques. 

À partir de Windows Server 2016, les KDC peuvent prendre en charge l’extension d’actualisation de PKInit. Par défaut, les KDC n’offrent pas l’extension d’actualisation de PKInit. Pour l’activer, utilisez le paramètre de stratégie nouvelle prise en charge du KDC pour l’extension d’actualisation du KDC de l’extension KDC sur tous les contrôleurs de domaine du domaine. Lorsqu’ils sont configurés, les options suivantes sont prises en charge lorsque le domaine est le niveau fonctionnel de domaine Windows Server 2016 (DFL) :

- **Disabled** : Le KDC n’offre jamais l’extension d’actualisation de PKInit et accepte les demandes d’authentification valides sans vérification de l’actualisation. Les utilisateurs ne recevront jamais le SID actualisé de l’identité de la clé publique.
- **Pris en charge**: L’extension d’actualisation de PKInit est prise en charge sur la demande. Les clients Kerberos s’authentifient correctement avec l’extension d’actualisation de PKInit reçoivent le SID d’identité de clé publique actualisé.
- **Obligatoire** : L’extension d’actualisation de PKInit est requise pour une authentification réussie. Les clients Kerberos qui ne prennent pas en charge l’extension d’actualisation de PKInit échouent toujours lors de l’utilisation des informations d’identification de la clé publique.

## <a name="domain-joined-device-support-for-authentication-using-public-key"></a>Prise en charge d’appareils joints à un domaine pour l’authentification à l’aide de la clé publique

À compter de Windows 10 version 1507 et de Windows Server 2016, si un appareil joint à un domaine est en mesure d’inscrire sa clé publique liée auprès d’un contrôleur de domaine Windows Server 2016, l’appareil peut s’authentifier avec la clé publique à l’aide de l’authentification Kerberos. un contrôleur de Windows Server 2016. Pour plus d’informations, consultez [authentification par clé publique d’appareil joint à un domaine](Domain-joined-Device-Public-Key-Authentication.md) .

## <a name="kerberos-clients-allow-ipv4-and-ipv6-address-hostnames-in-service-principal-names-spns"></a>Les clients Kerberos autorisent les noms d’hôte des adresses IPv4 et IPv6 dans les noms de principal du service (SPN)

À compter de Windows 10 version 1507 et Windows Server 2016, les clients Kerberos peuvent être configurés pour prendre en charge les noms d’hôte IPv4 et IPv6 dans les SPN. 

Chemin du Registre :

HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System\Kerberos\Parameters

Pour configurer la prise en charge des noms d’hôtes d’adresse IP dans les noms de principal du service, créez une entrée TryIPSPN. Par défaut, cette entrée n’existe pas dans le Registre. Après avoir créé l’entrée, remplacez la valeur DWORD par 1. S’il n’est pas configuré, les noms d’hôte des adresses IP ne sont pas tentés.

Si le SPN est inscrit dans Active Directory, l’authentification est réussie avec Kerberos. 

Pour plus d’informations, consultez le document [configuration de Kerberos pour les adresses IP](configuring-kerberos-over-ip.md).

## <a name="kdc-support-for-key-trust-account-mapping"></a>Prise en charge du KDC pour le mappage de compte de confiance de clé

À partir de Windows Server 2016, les contrôleurs de domaine prennent en charge le mappage de compte de confiance de clé, ainsi que le recours à AltSecID et au nom d’utilisateur principal (UPN) existants dans le comportement SAN. Lorsque UseSubjectAltName a la valeur :

- 0 : Un mappage explicite est requis. Il doit y avoir :
    - Confiance de clé (nouveauté de Windows Server 2016)
    - ExplicitAltSecID
- 1 : Le mappage implicite est autorisé (par défaut) :
    1. Si l’approbation de clé est configurée pour le compte, elle est utilisée pour le mappage (nouveauté avec Windows Server 2016).
    2. S’il n’existe aucun UPN dans le réseau SAN, AltSecID est tenté pour le mappage.
    3. S’il existe un UPN dans le réseau SAN, un nom d’utilisateur principal est tenté pour le mappage.

## <a name="see-also"></a>Voir aussi

- [Présentation de l’authentification Kerberos](kerberos-authentication-overview.md)
