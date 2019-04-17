---
title: "Nouveautés de l’authentification Kerberos"
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 7bd17803-6e42-4a3b-803f-e47c74725813
manager: alanth
author: justinha
ms.technology: security-authentication
ms.date: 11/09/2016
ms.openlocfilehash: 7b046490c606cdf9e1436f503bf46a9cd4280ea9
ms.sourcegitcommit: 2782a80a916f8432c030af76e860a72f08481899
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2018
---
# <a name="whats-new-in-kerberos-authentication"></a>Nouveautés de l’authentification Kerberos

>S’applique à: Windows Server2016 et Windows10

## <a name="kdc-support-for-public-key-trust-based-client-authentication"></a>Prise en charge pour l’authentification client basée sur la confiance de clé publique

À compter de Windows Server 2016, KDC prennent en charge un moyen de mappage de clé publique. Si la clé publique est configurée pour un compte, le contrôleur de domaine Kerberos prend en charge Kerberos PKInit explicitement à l’aide de cette clé. Dans la mesure où il n’existe aucune validation de certificat, les certificats auto-signés sont pris en charge et l’assurance du mécanisme d’authentification n’est pas pris en charge.

Approbation de la clé est conseillée lorsque configuré pour un compte, indépendamment du paramètre UseSubjectAltName.

## <a name="kerberos-client-and-kdc-support-for-rfc-8070-pkinit-freshness-extension"></a>Client Kerberos et KDC prennent en charge pour l’Extension de rafraîchissement du document RFC 8070 PKInit

À partir de Windows10, version1607 et Windows Server2016, les clients Kerberos essaient le [extension de rafraîchissement du document RFC8070 PKInit](https://datatracker.ietf.org/doc/draft-ietf-kitten-pkinit-freshness/) de-les ouvertures de session à partir de clé publique. 

À compter de Windows Server 2016, KDC peut prendre en charge l’extension de rafraîchissement du PKInit. Par défaut, le KDC n’offre pas l’extension de rafraîchissement du PKInit. Pour l’activer, utilisez la nouvelle prise en charge pour le paramètre de stratégie de modèle d’administration PKInit nouveauté Extension KDC sur tous les contrôleurs de domaine dans le domaine. Configuré, les options suivantes sont prises en charge lorsque le domaine est le niveau fonctionnel du domaine Windows Server2016 (DFL):

- **Désactivé**: le KDC offre l’Extension de rafraîchissement du PKInit jamais et accepte les demandes d’authentification valide sans vérification de l’actualisation. Les utilisateurs ne recevront jamais l’identité de clé publique nouvelle SID.
- **Prise en charge**: PKInit nouveauté Extension est pris en charge sur demande. Les clients Kerberos avec succès l’authentification avec l’Extension de rafraîchissement du PKInit recevoir l’identité de clé publique nouvelle SID.
- **Requis**: PKInit nouveauté Extension est requise pour réussir l’authentification. Les clients Kerberos qui ne prennent pas en charge l’Extension de rafraîchissement du PKInit échouera toujours lors de l’utilisation des informations d’identification de clé publiques.

## <a name="domain-joined-device-support-for-authentication-using-public-key"></a>Prise en charge des appareils joints au domaine pour l’authentification à l’aide de la clé publique

Compter de Windows 10 version 1507 et Windows Server 2016, si un appareil joint au domaine est en mesure d’enregistrer sa clé publique lié avec un contrôleur de domaine Windows Server 2016 (DC), puis l’appareil peut s’authentifier avec la clé publique à l’aide de l’authentification Kerberos pour un contrôleur de domaine Windows Server 2016. Pour plus d’informations, voir [authentification de clé publique appareil joint au domaine](Domain-joined-Device-Public-Key-Authentication.md)

## <a name="kerberos-clients-allow-ipv4-and-ipv6-address-hostnames-in-service-principal-names-spns"></a>Les clients Kerberos autorisent des noms d’hôte des adresses IPv4 et IPv6 dans les noms de Principal du Service (SPN)

À partir de Windows 10 version 1507 et Windows Server 2016, les clients Kerberos peuvent être configurés pour prendre en charge des noms d’hôtes IPv4 et IPv6 dans les noms principaux de service. 

Chemin d’accès du Registre:

HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System\Kerberos\Parameters

Pour configurer la prise en charge des noms d’hôte des adresses IP dans les noms principaux de service, créez une entrée TryIPSPN. Par défaut, cette entrée n’existe pas dans le Registre. Après avoir créé l’entrée, remplacez la valeur DWORD par 1. Si non configuré, les noms d’hôte des adresses IP sont a tenté pas.

Si le SPN est enregistré dans ActiveDirectory, l’authentification réussit avec le protocole Kerberos. 

## <a name="kdc-support-for-key-trust-account-mapping"></a>Prise en charge pour l’approbation de la clé de mappage de compte

À compter de Windows Server2016, les contrôleurs de domaine ont prise en charge de l’approbation de la clé de mappage de compte ainsi que de secours AltSecID et nom d’utilisateur Principal (UPN) existants dans le comportement de SAN. Lorsque UseSubjectAltName est défini sur:

- 0: mappage explicite est requis. Puis il doit être:
    - Clé de confiance (nouveauté de Windows Server 2016)
    - ExplicitAltSecID
- 1: le mappage implicite est autorisé (par défaut):
    1. Si la clé de confiance est configuré pour le compte, puis il est utilisé pour le mappage (nouveauté de Windows Server 2016).
    2. S’il n’existe aucun nom d’utilisateur principal dans le réseau SAN, AltSecID est tentée pour le mappage.
    3. S’il existe un nom UPN dans le réseau SAN, UPN est tentée pour le mappage.

## <a name="see-also"></a>Voir aussi

- [Vue d’ensemble de l’authentification Kerberos](kerberos-authentication-overview.md)
