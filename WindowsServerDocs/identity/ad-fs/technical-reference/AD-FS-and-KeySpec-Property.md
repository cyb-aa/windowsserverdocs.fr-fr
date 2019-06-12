---
title: Active Directory Federation Services et une propriété de la spécification de la clé d’informations de certificat
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: a5307da5-02ff-4c31-80f0-47cb17a87272
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 32b0d08f678e9e612bb0ce9cc38d254564bd9b2f
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66444092"
---
# <a name="ad-fs-and-certificate-keyspec-property-information"></a>AD FS et la propriété KeySpec informations de certificat
Spécification de clé (« KeySpec ») est une propriété associée à un certificat et une clé. Il spécifie si une clé privée associée à un certificat peut être utilisée pour la signature, chiffrement ou les deux.   

Une valeur KeySpec incorrecte peut provoquer des erreurs de AD FS et Proxy d’Application Web tel que :


- Échec d’établissement d’une connexion SSL/TLS pour AD FS ou le Proxy d’Application Web, sans événements AD FS connecté (bien que les événements SChannel 36888 et 36874 peuvent être enregistrés)
- Échec de l’ouverture de session sur les services AD FS ou WAP constitue la page de l’authentification basée sur, avec aucun message d’erreur indiqué dans la page.

Vous pouvez voir ce qui suit dans le journal des événements :

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

## <a name="what-causes-the-problem"></a>Ce qui provoque le problème
La propriété KeySpec identifie la façon dont une clé générée ou récupérées par Microsoft CryptoAPI (CAPI), à partir d’un Microsoft hérité stockage fournisseur chiffrement (CSP), peut être utilisée.

La valeur KeySpec **1**, ou **AT_KEYEXCHANGE**, peut être utilisé pour la signature et chiffrement.  La valeur **2**, ou **AT_SIGNATURE**, est utilisé uniquement pour la signature.

La plus courante KeySpec mauvaise configuration est à l’aide de la valeur 2 pour un certificat autre que le certificat de signature de jetons.  

Pour les certificats dont les clés ont été générées à l’aide de fournisseurs d’infrastructure de Diagnostics Windows (CNG, Cryptography Next Generation), il n’existe aucun concept de spécification de clé, et la valeur KeySpec sera toujours égal à zéro.

Découvrez comment rechercher une valeur valide de KeySpec ci-dessous. 

### <a name="example"></a>Exemple
Un exemple d’un CSP hérité est Microsoft Enhanced Cryptographic Provider. 

Format d’objet blob de clé RSA CSP Microsoft inclut un identificateur d’algorithme, soit **CALG_RSA_KEYX** ou **CALG_RSA_SIGN**, respectivement, pour traiter les demandes pour soit <strong>AT_KEYEXCHANGE ** ou ** AT_ SIGNATURE</strong> clés.

Les identificateurs d’algorithme de clé RSA mappent aux valeurs de KeySpec comme suit

| Algorithme de fournisseur pris en charge| Valeur de spécification de clé pour les appels CAPI |
| --- | --- |
|CALG_RSA_KEYX : Clé RSA qui peut être utilisé pour la signature et le déchiffrement| AT_KEYEXCHANGE (ou KeySpec = 1)|
CALG_RSA_SIGN : Signature RSA clé uniquement |AT_SIGNATURE (ou KeySpec = 2)|

## <a name="keyspec-values-and-associated-meanings"></a>Les valeurs KeySpec et significations associées
La signification des différentes valeurs KeySpec sont les suivantes :

|Valeur de KeySpec|Moyen|Utilisation d’AD FS recommandée|
| --- | --- | --- |
|0|Le certificat est un certificat CNG|Certificat SSL uniquement|
|1|Pour un certificat CAPI (non CNG) hérité, la clé peut être utilisée pour la signature et le déchiffrement|    SSL, certificats de communication de service de déchiffrement, jeton, de signature de jetons|
|2|Pour un certificat CAPI (non CNG) hérité, la clé peut être utilisée uniquement pour la signature|Non recommandé|

## <a name="how-to-check-the-keyspec-value-for-your-certificates--keys"></a>Comment vérifier la valeur KeySpec pour vos certificats / clés
Pour afficher une valeur de certificats que vous pouvez utiliser la **certutil** outil de ligne de commande.  

Voici un exemple : **certutil – v – stocker mon**.  Cela vide les informations de certificat à l’écran.

![KeySpec cert](media/AD-FS-and-KeySpec-Property/keyspec1.png)

Sous CERT_KEY_PROV_INFO_PROP_ID, recherchez deux choses :


1. **ProviderType :** cela indique si le certificat utilise un fournisseur hérité de stockage chiffrement (CSP) ou un fournisseur de stockage de clé basée sur plus récente certificat Next Generation (CNG) API.  Toute valeur différente de zéro indique un fournisseur hérité.
2. **KeySpec :** Les éléments suivants sont valides KeySpec pour un certificat AD FS :

   Fournisseur CSP hérité (ProviderType pas égal à 0) :

   |AD FS certificat objectif|Valeurs valides KeySpec|
   | --- | --- |
   |Communication de service|1|
   |Déchiffrement de jetons|1|
   |Signature de jetons|1 et 2|
   |SSL|1|

   Fournisseur de CNG (ProviderType = 0) :

   |AD FS certificat objectif|Valeurs valides KeySpec|
   | --- | --- |   
   |SSL|0|

## <a name="how-to-change-the-keyspec-for-your-certificate-to-a-supported-value"></a>Comment modifier le keyspec pour votre certificat à une valeur prise en charge
Modification de la valeur KeySpec ne nécessite pas le certificat à être régénérés ou ré-émis par l’autorité de certification.  KeySpec peut être modifié en réimportant le complète des certificats et la clé privée à partir d’un fichier PFX dans le magasin de certificats à l’aide de la procédure ci-dessous :


1. Tout d’abord, vérifiez et enregistrez les autorisations de clé privées sur le certificat existant afin qu’ils peuvent être reconfigurées si nécessaire après le réimporter.
2. Exportez le certificat, y compris la clé privée dans un fichier PFX.
3. Procédez comme suit pour chaque serveur AD FS et WAP
    1. Supprimer le certificat (à partir d’AD FS / serveur WAP)
    2. Ouvrez une invite de commandes PowerShell avec élévation de privilèges et importez le fichier PFX sur chaque serveur AD FS et WAP à l’aide de la syntaxe de l’applet de commande ci-dessous, en spécifiant la valeur AT_KEYEXCHANGE (qui fonctionne pour toutes les opérations de certificat AD FS) :
        1. C:\>certutil – importpfx certfile.pfx AT_KEYEXCHANGE
        2. Entrez le mot de passe PFX
    3. Une fois terminée la méthode ci-dessus, procédez comme suit
        1. Vérifiez les autorisations de clé privées
        2. Redémarrez le service AD FS ou wap





