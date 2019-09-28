---
title: Services ADFS et informations sur les propriétés de la spécification de clé de certificat
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.assetid: a5307da5-02ff-4c31-80f0-47cb17a87272
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 51c9828cfe494c68422f4985e5b17113020c8414
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407425"
---
# <a name="ad-fs-and-certificate-keyspec-property-information"></a>AD FS et certificater les informations de propriété de KeySpec
La spécification de clé ("KeySpec") est une propriété associée à un certificat et une clé. Elle spécifie si une clé privée associée à un certificat peut être utilisée pour la signature, le chiffrement ou les deux.   

Une valeur KeySpec incorrecte peut provoquer des erreurs de AD FS et de proxy d’application Web, par exemple :


- Échec de l’établissement d’une connexion SSL/TLS à AD FS ou au proxy d’application Web, sans AD FS événements consignés (même si les événements SChannel 36888 et 36874 peuvent être enregistrés)
- Échec de la connexion au AD FS ou à la page d’authentification par formulaires WAP, sans message d’erreur sur la page.

Vous pouvez voir les éléments suivants dans le journal des événements :

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

## <a name="what-causes-the-problem"></a>Ce qui est à l’origine du problème
La propriété KeySpec identifie la manière dont une clé générée ou récupérée par Microsoft CryptoAPI (CAPI), à partir d’un fournisseur de stockage de chiffrement (CSP) hérité de Microsoft, peut être utilisée.

La valeur KeySpec **1**ou **AT_KEYEXCHANGE**peut être utilisée pour la signature et le chiffrement.  La valeur **2**, ou **AT_SIGNATURE**, est utilisée uniquement pour la signature.

L’utilisation de la mauvaise configuration KeySpec la plus courante utilise la valeur 2 pour un certificat autre que le certificat de signature de jetons.  

Pour les certificats dont les clés ont été générées à l’aide de fournisseurs CNG (Cryptography Next Generation), il n’existe aucun concept de spécification de clé et la valeur KeySpec est toujours égale à zéro.

Consultez Comment rechercher une valeur KeySpec valide ci-dessous. 

### <a name="example"></a>Exemple
Le fournisseur de services de chiffrement amélioré Microsoft est un exemple de CSP hérité. 

Le format BLOB de clé Microsoft RSA CSP contient un identificateur d’algorithme, **CALG_RSA_KEYX** ou **CALG_RSA_SIGN**, respectivement, pour traiter les demandes pour les clés <strong>AT_KEYEXCHANGE * * ou * * AT_SIGNATURE</strong> .

Les identificateurs d’algorithme de clé RSA sont mappés aux valeurs KeySpec comme suit

| Algorithme pris en charge par le fournisseur| Valeur de la spécification de clé pour les appels CAPI |
| --- | --- |
|CALG_RSA_KEYX : Clé RSA qui peut être utilisée pour la signature et le déchiffrement| AT_KEYEXCHANGE (ou KeySpec = 1)|
CALG_RSA_SIGN : Clé RSA signature only |AT_SIGNATURE (ou KeySpec = 2)|

## <a name="keyspec-values-and-associated-meanings"></a>Valeurs keyspecs et significations associées
Voici la signification des différentes valeurs KeySpec :

|Valeur KeySpec|Cela|Utilisation AD FS recommandée|
| --- | --- | --- |
|0|Le certificat est un certificat CNG|Certificat SSL uniquement|
|1|Pour un certificat CAPI (non CNG) hérité, la clé peut être utilisée pour la signature et le déchiffrement|    SSL, signature de jetons, déchiffrement de jetons, certificats de communication de service|
|2|Pour un certificat CAPI (non CNG) hérité, la clé peut être utilisée uniquement pour la signature|Non recommandé|

## <a name="how-to-check-the-keyspec-value-for-your-certificates--keys"></a>Comment vérifier la valeur KeySpec pour vos certificats/clés
Pour afficher une valeur de certificat, vous pouvez utiliser l’outil en ligne de commande **certutil** .  

Voici un exemple : **certutil – v – Store My**.  Les informations de certificat sont vidées à l’écran.

![KeySpec CERT](media/AD-FS-and-KeySpec-Property/keyspec1.png)

Sous CERT_KEY_PROV_INFO_PROP_ID, recherchez deux choses :


1. **ProviderType :** cela indique si le certificat utilise un fournisseur de stockage de chiffrement (CSP) hérité ou un fournisseur de stockage de clés basé sur des API CNG (Certificate Next Generation) plus récentes.  Toute valeur différente de zéro indique un fournisseur hérité.
2. **KeySpec** Vous trouverez ci-dessous les valeurs KeySpec valides pour un certificat AD FS :

   Fournisseur CSP hérité (ProviderType différent de 0) :

   |AD FS objet du certificat|Valeurs KeySpec valides|
   | --- | --- |
   |Communication du service|1|
   |Déchiffrement de jetons|1|
   |Signature des jetons|1 et 2|
   |SSL|1|

   Fournisseur CNG (ProviderType = 0) :

   |AD FS objet du certificat|Valeurs KeySpec valides|
   | --- | --- |   
   |SSL|0|

## <a name="how-to-change-the-keyspec-for-your-certificate-to-a-supported-value"></a>Comment faire passer la valeur KeySpec pour votre certificat à une valeur prise en charge
La modification de la valeur KeySpec ne requiert pas que le certificat soit régénéré ou émis de nouveau par l’autorité de certification.  Vous pouvez modifier la clé de réinitialisation en réimportant le certificat complet et la clé privée d’un fichier PFX dans le magasin de certificats à l’aide des étapes ci-dessous :


1. Tout d’abord, vérifiez et enregistrez les autorisations de clé privée sur le certificat existant afin qu’elles puissent être reconfigurées si nécessaire après la réimportation.
2. Exportez le certificat incluant la clé privée dans un fichier PFX.
3. Procédez comme suit pour chaque AD FS et serveur WAP
    1. Supprimer le certificat (à partir du AD FS/serveur WAP)
    2. Ouvrez une invite de commandes PowerShell avec élévation de privilèges et importez le fichier PFX sur chaque AD FS et un serveur WAP à l’aide de la syntaxe d’applet de commande ci-dessous, en spécifiant la valeur AT_KEYEXCHANGE (qui fonctionne pour tous les rôles AD FS certifications) :
        1. C : \>certutil – importpfx CertFile. pfx AT_KEYEXCHANGE
        2. Entrer le mot de passe PFX
    3. Une fois l’opération ci-dessus terminée, procédez comme suit :
        1. vérifier les autorisations de la clé privée
        2. redémarrer le service ADFS ou WAP





