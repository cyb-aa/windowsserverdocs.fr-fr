---
title: "ActiveDirectory Federation Services et propriété de clé spécification d’informations de certificat"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: a5307da5-02ff-4c31-80f0-47cb17a87272
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: db58fcce054f34c4b0a3f6725456badae9fd0468
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="ad-fs-and-certificate-keyspec-property-information"></a>ADFS et propriété KeySpec informations de certificat
Spécification de clé («KeySpec») est une propriété associée à un certificat et une clé. Il spécifie si une clé privée associée à un certificat peut être utilisée pour la signature, le chiffrement ou les deux.   

Une valeur incorrecte de KeySpec peut provoquer des erreurs ADFS et le Proxy d’Application Web telles que:


- Échec d’établir une connexion SSL/TLS pour ADFS ou le Proxy d’Application Web, avec aucune événements ADFS connectés (bien que les événements SChannel36888 et 36874 peuvent être enregistrés)
- Échec pour vous connecter au ADFS ou WAP constitue la page de l’authentification, avec aucun message d’erreur indiqué dans la page.

Vous pouvez voir les éléments suivants dans le journal des événements:

    Log Name:      AD FS Tracing/Debug
    Source:        AD FS Tracing
    Date:          2/12/2015 9:03:08 AM
    Event ID:      67
    Task Category: None
    Level:         Error
    Keywords:      ADFSProtocol
    User:          S-1-5-21-3723329422-3858836549-556620232-1580884
    Computer:      ADFS1.contoso.com
    Description:
    Ignore corrupted SSO cookie.

## <a name="what-causes-the-problem"></a>Quelle est la cause du problème
La propriété KeySpec identifie comment une clé générée ou récupérées par MicrosoftCryptoAPI (CAPI), à partir d’un Microsoft hérités stockage fournisseur chiffrement (CSP), peut être utilisée.

La valeur KeySpec **1**, ou **AT_KEYEXCHANGE**, peut être utilisé pour la signature et chiffrement.  La valeur **2**, ou **AT_SIGNATURE**, est utilisé uniquement pour la signature.

Les plus courants KeySpec mauvaise configuration est à l’aide de la valeur 2 pour un certificat autre que le certificat de signature de jetons.  

Pour les certificats dont les clés ont été générées à l’aide de la prochaine génération fournisseurs CNG (Cryptography), il n’existe pas de concept de spécification de clé, et la valeur KeySpec sera toujours zéro.

Découvrez comment rechercher une valeur KeySpec valide ci-dessous. 

### <a name="example"></a>Exemple
Un exemple d’un fournisseur de services cryptographiques héritée est MicrosoftEnhanced Cryptographic Provider. 

Format d’objet blob de clé RSA CSP Microsoft inclut un identificateur d’algorithme, soit **CALG_RSA_KEYX** ou **CALG_RSA_SIGN**, respectivement, pour traiter les demandes pour soit ** AT_KEYEXCHANGE ** ou **AT_SIGNATURE** clés.
  
Les identificateurs d’algorithme de clé RSA correspondent aux valeurs KeySpec ci-dessous

| Algorithme de fournisseur pris en charge| Valeur de spécification de clés pour les appels CAPI |
| --- | --- |
|CALG_RSA_KEYX: Clé RSA qui peut être utilisé pour la signature et le déchiffrement| AT_KEYEXCHANGE (ou KeySpec = 1)|
CALG_RSA_SIGN: Uniquement la clé de signature de RSA |AT_SIGNATURE (ou KeySpec = 2)|

## <a name="keyspec-values-and-associated-meanings"></a>Valeurs KeySpec et significations associées
La signification des différentes valeurs KeySpec sont les suivantes:

|Valeur KeySpec|Moyen|Utilisation d’ADFS recommandée|
| --- | --- | --- |
|0|Le certificat est un certificat CNG|Certificat SSL uniquement|
|1|Pour un certificat CAPI (non CNG) hérité, la clé peut être utilisée pour la signature et le déchiffrement|    SSL, jeton déchiffrement, certificats de communication du service, de signature de jetons|
|2|Pour un certificat CAPI (non CNG) hérité, la clé peut être utilisée uniquement pour la signature|Non recommandé|

## <a name="how-to-check-the-keyspec-value-for-your-certificates--keys"></a>Comment vérifier la valeur KeySpec pour vos certificats / clés
Pour afficher une valeur de certificats que vous pouvez utiliser le **certutil** outil de ligne de commande.  

Voici un exemple: **certutil – v – stocker mes**.  Cela provoque le vidage les informations de certificat à l’écran.

![KeySpec cert](media/AD-FS-and-KeySpec-Property/keyspec1.png)

Sous options Rechercher CERT_KEY_PROV_INFO_PROP_ID deux choses:


1. **ProviderType:** cela indique si le certificat utilise un hérités fournisseur de chiffrement stockage (CSP) ou un fournisseur de stockage de clé basé sur les plus récents certificat Next Generation (CNG) API.  N’importe quelle valeur non nulle indique un fournisseur hérité.
2.  **KeySpec:** les éléments suivants sont valides KeySpec pour un certificat ADFS:

    (ProviderType pas égal à 0) du fournisseur CSP hérité:
    
    |ADFS certificat objectif|Valeurs valides KeySpec|
    | --- | --- |
    |Communication du service|1|
    |Déchiffrement de jeton|1|
    |Signature de jetons|1 et 2|
    |SSL|1|

    Fournisseur CNG (ProviderType = 0):
    |ADFS certificat objectif|Valeurs valides KeySpec|
    | --- | --- |   
    |SSL|0|

## <a name="how-to-change-the-keyspec-for-your-certificate-to-a-supported-value"></a>Comment modifier le keyspec pour votre certificat sur une valeur pris en charge
Modification de la valeur KeySpec ne nécessite pas le certificat à être régénéré ou nouveau émis par l’autorité de certification.  Le KeySpec peut être modifié à nouveau l’importation du certificat terminée et la clé privée à partir d’un fichier PFX dans le magasin de certificats à l’aide de la procédure ci-dessous:


1. Tout d’abord, vérifiez et enregistrez les autorisations de clé privées sur le certificat existant afin qu’ils peuvent être modifiées si nécessaire après le réimporter.
2. Exporter le certificat, y compris la clé privée dans un fichier PFX.
3. Procédez comme suit pour chaque serveur ADFS et WAP
    1. Supprimer le certificat (à partir de l’ADFS / serveur WAP)
    2. Ouvrez une invite de commandes PowerShell avec élévation de privilèges et importer le fichier PFX sur chaque serveur ADFS et WAP, à l’aide de la syntaxe de l’applet de commande ci-dessous, en spécifiant la valeur AT_KEYEXCHANGE (qui fonctionne pour tous les rôles du certificat ADFS):
        1. C:\ > certutil – importpfx certfile.pfx AT_KEYEXCHANGE
        2. Entrez le mot de passe PFX
    3. Une fois terminée le ci-dessus, procédez comme suit
        1. Vérifiez les autorisations de la clé privées
        2. Redémarrez le service ADFS ou wap





