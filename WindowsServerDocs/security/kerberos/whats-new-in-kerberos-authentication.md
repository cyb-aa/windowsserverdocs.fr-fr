---
title: Quelles sont les nouveautés dans l’authentification Kerberos
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 7bd17803-6e42-4a3b-803f-e47c74725813
manager: alanth
author: justinha
ms.technology: security-authentication
ms.date: 11/09/2016
ms.openlocfilehash: 90107bd49268f232fd6d532c304c2fdd050bcbf5
ms.sourcegitcommit: c6acac3622e5d34714ca5c569805931681f98779
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66391503"
---
# <a name="whats-new-in-kerberos-authentication"></a>What's New in Kerberos Authentication

>S'applique à : Windows Server 2016 et Windows 10

## <a name="kdc-support-for-public-key-trust-based-client-authentication"></a>Prise en charge du contrôleur de domaine Kerberos pour l’authentification client basée sur la confiance de clé publique

À compter de Windows Server 2016, KDC prennent en charge un moyen de mappage de clé publique. Si la clé publique est configurée pour un compte, le contrôleur de domaine Kerberos prend en charge Kerberos PKInit explicitement à l’aide de cette clé. Dans la mesure où il n’existe aucune validation de certificat, les certificats auto-signés sont pris en charge et assurance du mécanisme d’authentification n’est pas pris en charge.

Il est préférable de confiance de clé quand configuré pour un compte, indépendamment du paramètre UseSubjectAltName.

## <a name="kerberos-client-and-kdc-support-for-rfc-8070-pkinit-freshness-extension"></a>Client Kerberos et KDC prennent en charge pour RFC 8070 PKInit fraîcheur Extension

Depuis Windows 10, version 1607 et Windows Server 2016, Kerberos tentent du [extension de fraîcheur RFC 8070 PKInit](https://datatracker.ietf.org/doc/draft-ietf-kitten-pkinit-freshness/) pour clé publique basés sur des ouvertures de session. 

À compter de Windows Server 2016, KDC peut prendre en charge l’extension de fraîcheur PKInit. Par défaut, le KDC n’offre pas l’extension de fraîcheur PKInit. Pour l’activer, utilisez la nouvelle prise en charge du contrôleur de domaine Kerberos pour le paramètre de stratégie de modèle d’administration PKInit fraîcheur Extension KDC sur tous les contrôleurs de domaine dans le domaine. Configuré, les options suivantes sont prises en charge lorsque le domaine est le niveau fonctionnel du domaine Windows Server 2016 (DFL) :

- **Disabled** : Le centre KDC offre l’Extension de fraîcheur PKInit jamais et accepte les demandes d’authentification valide sans vérifier d’actualisation. Les utilisateurs ne recevront jamais de l’identité de clé publique fraîche SID.
- **Prise en charge**: Extension de fraîcheur PKInit est prise en charge sur demande. Les clients Kerberos avec succès l’authentification avec l’Extension de fraîcheur PKInit recevoir l’identité de clé publique fraîche SID.
- **Obligatoire** : Extension de fraîcheur PKInit est requise pour l’authentification réussie. Les clients Kerberos qui ne prennent pas en charge l’Extension de fraîcheur PKInit continue d’échouer lors de l’utilisation des informations d’identification de clé publiques.

## <a name="domain-joined-device-support-for-authentication-using-public-key"></a>Prise en charge des appareils joints au domaine pour l’authentification à l’aide de la clé publique

Depuis Windows 10 version 1507 et Windows Server 2016, si un appareil joint au domaine est en mesure d’enregistrer sa clé publique lié avec un contrôleur de domaine (DC) Windows Server 2016, puis l’appareil peut s’authentifier avec la clé publique à l’aide de l’authentification Kerberos un contrôleur de domaine Windows Server 2016. Pour plus d’informations, consultez [joints au domaine appareil authentification par clé publique](Domain-joined-Device-Public-Key-Authentication.md)

## <a name="kerberos-clients-allow-ipv4-and-ipv6-address-hostnames-in-service-principal-names-spns"></a>Les clients Kerberos autorisent des noms d’hôte des adresses IPv4 et IPv6 dans les noms de principal du Service (SPN)

Depuis Windows 10 version 1507 et Windows Server 2016, les clients Kerberos peuvent être configurés pour prendre en charge des noms d’hôtes IPv4 et IPv6 dans les noms principaux de service. 

Chemin d’accès du Registre :

HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System\Kerberos\Parameters

Pour configurer la prise en charge des noms d’hôte des adresses IP dans les noms principaux de service, créez une entrée TryIPSPN. Par défaut, cette entrée n’existe pas dans le Registre. Une fois que vous avez créé l’entrée, remplacez la valeur DWORD par 1. Si ne pas configuré, les noms d’hôte des adresses IP ne sont pas tentées.

Si le SPN est inscrit dans Active Directory, l’authentification réussit avec Kerberos. 

Pour plus d’informations, consultez le document [configuration de Kerberos pour les adresses IP](configuring-kerberos-over-ip.md).

## <a name="kdc-support-for-key-trust-account-mapping"></a>Prise en charge pour le mappage de compte de confiance de clé

À compter de Windows Server 2016, les contrôleurs de domaine ont prise en charge pour le mappage de confiance de clé de compte, ainsi que comme secours AltSecID et nom d’utilisateur Principal (UPN) existants dans le comportement de SAN. Lorsque UseSubjectAltName a la valeur :

- 0: Mappage explicite est requis. Puis il doit être :
    - Confiance de clé (nouveauté de Windows Server 2016)
    - ExplicitAltSecID
- 1 : Mise en correspondance implicite est autorisé (valeur par défaut) :
    1. Si la confiance de clé est configuré pour le compte, puis il est utilisé pour le mappage (nouveauté de Windows Server 2016).
    2. S’il n’existe pas de nom UPN dans le réseau SAN, AltSecID est tentée pour le mappage.
    3. S’il existe un UPN dans le réseau SAN, UPN est tentée pour le mappage.

## <a name="see-also"></a>Voir aussi

- [Vue d’ensemble de l’authentification Kerberos](kerberos-authentication-overview.md)
