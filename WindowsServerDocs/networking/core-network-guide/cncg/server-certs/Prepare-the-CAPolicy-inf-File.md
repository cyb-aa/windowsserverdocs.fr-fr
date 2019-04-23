---
title: Préparer le fichier CAPolicy.inf
description: Le fichier CAPolicy.inf contient différents paramètres qui sont utilisés lors de l’installation du Service de Certification Active Directory (AD CS) ou lors du renouvellement du certificat d’autorité de certification.
manager: alanth
ms.topic: article
ms.assetid: 65b36794-bb09-4c1b-a2e7-8fc780893d97
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 80c7155224502379e2e9618ceb38709c5051a6b7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857840"
---
# <a name="capolicyinf-syntax"></a>Syntaxe CAPolicy.inf
>   S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Le fichier CAPolicy.inf est un fichier de configuration qui définit les extensions, des contraintes et des autres paramètres de configuration qui sont appliqués à un certificat d’autorité de certification racine et tous les certificats émis par l’autorité de certification racine. Le fichier CAPolicy.inf doit être installé sur un serveur hôte avant de la routine de configuration pour la racine de qu'autorité de certification. Lorsque les restrictions de sécurité sur une autorité de certification racine doivent être modifiées, le certificat racine doit être renouvelé et un fichier CAPolicy.inf mis à jour doit être installé sur le serveur avant que ne commence le processus de renouvellement.

Le fichier CAPolicy.inf est :

-   Créée et définie manuellement par un administrateur

-   Utilisé lors de la création de la racine et les certificats d’autorité de certification

-   Défini sur l’autorité de certification de signature où vous connecterez et émettez le certificat (pas l’autorité de certification où la demande est accordée)

Une fois que vous avez créé votre fichier CAPolicy.inf, vous devez le copier dans le **%SystemRoot%** dossier de votre serveur avant d’installer AD CS ou de renouveler le certificat d’autorité de certification.

Le fichier CAPolicy.inf rend possible spécifier et configurer un large éventail d’options et les attributs de l’autorité de certification. La section suivante décrit toutes les options vous permettant de créer un fichier .inf adapté à vos besoins spécifiques.

## <a name="capolicyinf-file-structure"></a>Structure de fichier CAPolicy.inf

Les termes suivants sont utilisés pour décrire la structure de fichiers .inf :

-   _Section_ – est une zone du fichier qui couvre un groupe logique de clés. Noms de sections dans les fichiers .inf sont identifiés par figurant entre crochets. Les sections de nombreuses, mais pas tous, sont utilisées pour configurer les extensions de certificat.

-   _Clé_ : est le nom d’une entrée et apparaît à gauche du signe égal.

-   _Valeur_ : est le paramètre et apparaît à droite du signe égal.

Dans l’exemple ci-dessous, **[Version]** correspond à la section **Signature** est la clé, et **»\$Windows NT\$»** est la valeur.

Exemple :

```PowerShell
[Version]                     #section
Signature="$Windows NT$"      #key=value
```

###  <a name="version"></a>Version

Identifie le fichier comme un fichier .inf. Version est la seule section requise et doit être au début de votre fichier CAPolicy.inf.

###  <a name="policystatementextension"></a>PolicyStatementExtension

Répertorie les stratégies qui ont été définies par l’organisation, et si elles sont facultatives ou obligatoires. Plusieurs stratégies sont séparés par des virgules. Les noms ont une signification dans le cadre d’un déploiement spécifique, ou par rapport à des applications personnalisées qui vérifient la présence de ces stratégies.

Pour chaque stratégie définie, il doit y avoir une section qui définit les paramètres pour cette stratégie particulière. Pour chaque stratégie, vous devez fournir un identificateur d’objet défini par l’utilisateur (OID) et le texte que vous souhaitez afficher en tant que l’instruction de stratégie ou un pointeur d’URL à l’instruction de stratégie. L’URL peut être sous la forme d’un HTTP, FTP ou URL LDAP.

Si vous vous apprêtez à avoir un texte descriptif dans l’instruction de stratégie, les trois lignes suivantes du fichier CAPolicy.inf ressemblerait à :

```
[InternalPolicy]
OID=1.1.1.1.1.1.1
Notice=”Legal policy statement text”
```

Si vous vous apprêtez à utiliser une URL pour héberger l’instruction de stratégie d’autorité de certification, puis trois lignes suivantes au lieu de cela ressemblerait à :

```
[InternalPolicy]
OID=1.1.1.1.1.1.2
URL=https://pki.wingtiptoys.com/policies/legalpolicy.asp
```

En outre :

-   Plusieurs clés d’URL et les avis sont pris en charge.

-   Avis et URL dans la même section de stratégie sont prises en charge.

-   URL avec des espaces ou du texte avec espaces doit être entourée de guillemets. Cela est vrai pour les **URL** clé, quelle que soit la section dans laquelle elle apparaît.

Un exemple de plusieurs URL dans une section de stratégie et les mentions ressemblerait à :

```
[InternalPolicy]
OID=1.1.1.1.1.1.1
URL=https://pki.wingtiptoys.com/policies/legalpolicy.asp
URL=ftp://ftp.wingtiptoys.com/pki/policies/legalpolicy.asp
Notice=”Legal policy statement text”
```

### <a name="crldistributionpoint"></a>CRLDistributionPoint

Vous pouvez spécifier des Points de Distribution CRL (CDP) d’un certificat d’autorité de certification racine dans le fichier CAPolicy.inf.  Après l’installation de l’autorité de certification, vous pouvez configurer les URL CDP qui inclut de l’autorité de certification dans chaque certificat émis. Le certificat d’autorité de certification racine affiche l’URL spécifiées dans cette section du fichier CAPolicy.inf. 

```
[CRLDistributionPoint]
URL=http://pki.wingtiptoys.com/cdp/WingtipToysRootCA.crl
```

Des informations supplémentaires sur cette section :
-   prend en charge :
    - HTTP 
    - URL de fichier
    - URL LDAP 
    - Plusieurs URL
   
    >[!IMPORTANT]
    >Ne prend pas en charge les URL HTTPS.

-   Guillemets doivent entourer des URL avec des espaces.

-   Si aucune URL n’est spécifiée : autrement dit, si le **[CRLDistributionPoint]** section existe dans le fichier mais est vide, l’extension d’informations d’accès est omise à partir du certificat d’autorité de certification racine. Cette méthode est généralement préférable lorsque vous configurez une autorité de certification racine. Windows n’effectue pas de contrôle de révocation sur un certificat d’autorité de certification racine, par conséquent, l’extension CDP est superflue dans un certificat d’autorité de certification racine.

-    Autorité de certification peut publier au fichier UNC, par exemple, dans un partage qui représente le dossier d’un site Web où un client récupère via HTTP.

-   Utilisez uniquement cette section si vous êtes de configuration d’une autorité de certification racine ou de renouvellement du certificat d’autorité de certification racine. L’autorité de certification détermine les extensions CDP de l’autorité de certification subordonnées.
   

### <a name="authorityinformationaccess"></a>AuthorityInformationAccess

Vous pouvez spécifier les points d’accès autorité d’informations dans le fichier CAPolicy.inf pour le certificat d’autorité de certification racine.

```
[AuthorityInformationAccess]
URL=http://pki.wingtiptoys.com/Public/myCA.crt
```

Quelques remarques supplémentaires sur les informations de l’autorité accéder à la section :

-   Plusieurs URL sont prises en charge.

-   HTTP, FTP, LDAP et URL de fichier sont pris en charge. URL HTTPS ne sont pas pris en charge.

-   Cette section est utilisée uniquement si vous définissez d’une autorité de certification racine ou renouvellement du certificat d’autorité de certification racine. Les extensions AIA de l’autorité de certification secondaire sont déterminées par l’autorité de certification qui a émis le certificat de l’autorité de certification subordonnées.

-   URL avec des espaces doivent être entourées de guillemets.

-   Si aucune URL n’est spécifiée : autrement dit, si le **[AuthorityInformationAccess]** section existe dans le fichier mais est vide, l’extension de Point de Distribution CRL est omise à partir du certificat d’autorité de certification racine. Là encore, il s’agit du paramètre recommandé dans le cas d’un certificat d’autorité de certification racine car il n’existe aucune autorité supérieure à une autorité de certification qui devra être référencé par un lien vers son certificat racine.

### <a name="certsrvserver"></a>certsrv_Server

Une autre section facultative du fichier CAPolicy.inf est [certsrv_server], qui est utilisé pour spécifier la longueur de clé de renouvellement, la période de validité de renouvellement et période de validité liste de révocation de révocation de certificat pour une autorité de certification qui est en cours de renouvellement ou installé. Aucune des touches dans cette section sont nécessaires. La plupart de ces paramètres ont des valeurs par défaut sont suffisantes pour la plupart des besoins et peuvent simplement être omis dans le fichier CAPolicy.inf. Vous pouvez également plusieurs de ces paramètres peuvent être modifiés après l’installation de l’autorité de certification.

Un exemple ressemblerait à :

```
[certsrv_server]
RenewalKeyLength=2048
RenewalValidityPeriod=Years
RenewalValidityPeriodUnits=5
CRLPeriod=Days
CRLPeriodUnits=2
CRLDeltaPeriod=Hours
CRLDeltaPeriodUnits=4
ClockSkewMinutes=20
LoadDefaultTemplates=True
AlternateSignatureAlgorithm=0
ForceUTF8=0
EnableKeyCounting=0
```

**RenewalKeyLength** définit la taille de clé pour le renouvellement uniquement. Cela est utilisé uniquement lorsqu’une nouvelle paire de clés est générée lors du renouvellement de certificat d’autorité de certification. La taille de clé pour le certificat d’autorité de certification initiale est définie lors de l’autorité de certification est installée.

Lors du renouvellement d’un certificat d’autorité de certification avec une paire de clés, la longueur de clé peut être augmentée ou diminuée. Par exemple, si vous avez défini une racine Autorité de certification taille de la clé de 4 096 octets ou une version ultérieure et que vous réalisez que vous avez des applications Java ou des périphériques réseau qui peuvent prendre uniquement en charge les tailles de clé de 2 048 octets. Si vous augmentez ou diminuez la taille, vous devez émettre de nouveau tous les certificats émis par cette autorité de certification.

**RenewalValidityPeriod** et **RenewalValidityPeriodUnits** établir la durée de vie du nouveau certificat d’autorité de certification racine lors du renouvellement de l’ancien certificat d’autorité de certification racine. Elle s’applique uniquement à une autorité de certification racine. La durée de vie du certificat d’autorité de certification subordonnée est déterminée par son supérieur. RenewalValidityPeriod peut avoir les valeurs suivantes : Heures, jours, semaines, mois et années.

**Période_crl** et **CRLPeriodUnits** établir la période de validité pour la liste CRL de base. **Période_crl** peut avoir les valeurs suivantes : Heures, jours, semaines, mois et années.

**Période_delta_crl** et **CRLDeltaPeriodUnits** établir la période de validité de la liste CRL delta. **Période_delta_crl** peut avoir les valeurs suivantes : Heures, jours, semaines, mois et années.

Chacun de ces paramètres peut être configuré après l’installation de l’autorité de certification :

```
Certutil -setreg CACRLPeriod Weeks
Certutil -setreg CACRLPeriodUnits 1
Certutil -setreg CACRLDeltaPeriod Days
Certutil -setreg CACRLDeltaPeriodUnits 1
```

N’oubliez pas de redémarrer Active Directory Certificate Services pour toutes les modifications prennent effet.

**LoadDefaultTemplates** s’applique uniquement lors de l’installation d’une autorité de certification d’entreprise. Ce paramètre, soit True ou False (ou 1 ou 0), détermine si l’autorité de certification est configurée avec un des modèles par défaut.

Dans une installation par défaut de l’autorité de certification, un sous-ensemble des modèles de certificats par défaut est ajouté au dossier de modèles de certificat dans le composant logiciel enfichable Autorité de Certification. Cela signifie que dès que le service AD CS démarre après avoir installé le rôle d’un utilisateur ou un ordinateur disposant d’autorisations suffisantes peut immédiatement inscrire pour un certificat.

Vous souhaiterez pas émettre de certificats immédiatement après l’installation d’une autorité de certification, vous pouvez utiliser le paramètre LoadDefaultTemplates pour empêcher les modèles par défaut d’être ajouté à l’autorité de certification d’entreprise. Si aucun modèle configuré sur l’autorité de certification il ne peut émettre aucun certificat.

**AlternateSignatureAlgorithm** configure l’autorité de certification pour prendre en charge le PKCS\#format de signature V2.1 1 pour le certificat d’autorité de certification et les demandes de certificat. Lorsque la valeur 1 sur une autorité de certification racine sur le certificat d’autorité de certification inclut le PKCS\#format de signature V2.1 1. Lorsqu’il est défini sur une autorité de certification secondaire, l’autorité de certification subordonnée créera une demande de certificat qui inclut le PKCS\#format de signature V2.1 1.

**ForceUTF8** modifie la valeur par défaut des noms uniques relatifs (RDN) dans le sujet et l’émetteur d’encodage des noms au format UTF-8 uniques. Uniquement les noms uniques relatifs qui prennent en charge UTF-8, telles que celles qui sont définies comme des types de chaîne de répertoire par une commande RFC, sont affectés. Par exemple, le nom unique relatif pour le composant de domaine (DC) prend en charge le codage IA5 ou UTF-8, tandis que le Country RDN (C) prend uniquement en charge le codage sous forme de chaîne imprimable. La directive ForceUTF8 affecte donc un nom unique relatif de contrôleur de domaine mais n’affecte pas un nom unique relatif de C.

**EnableKeyCounting** configure l’autorité de certification pour incrémenter un compteur chaque fois que la clé de signature de l’autorité de certification est utilisée. N’activez pas ce paramètre, sauf si vous avez un Module de sécurité matériel (HSM) et le fournisseur de services de chiffrement associé (CSP) qui prend en charge les clés de comptage. Le CSP fort Microsoft ni le comptage clé du fournisseur de stockage de clés (KSP) de Microsoft Software prise en charge.


## <a name="create-the-capolicyinf-file"></a>Créer le fichier CAPolicy.inf

Avant d’installer les services AD CS, vous configurez le fichier CAPolicy.inf avec des paramètres spécifiques pour votre déploiement.

**Condition préalable :** Vous devez être membre du groupe Administrateurs.

1.  Sur l’ordinateur où vous envisagez d’installer les services AD CS, ouvrez Windows PowerShell, tapez **le bloc-notes c:\CAPolicy.inf** et appuyez sur ENTRÉE.

2.  À l’invite de création de fichier, cliquez sur **Oui**.

3.  Entrez le code suivant comme contenu du fichier :
   ```
   [Version]  
    Signature="$Windows NT$"  
    [PolicyStatementExtension]  
    Policies=InternalPolicy  
    [InternalPolicy]  
    OID=1.2.3.4.1455.67.89.5  
    Notice="Legal Policy Statement"  
    URL=https://pki.corp.contoso.com/pki/cps.txt  
    [Certsrv_Server]  
    RenewalKeyLength=2048  
    RenewalValidityPeriod=Years  
    RenewalValidityPeriodUnits=5  
    CRLPeriod=weeks  
    CRLPeriodUnits=1  
    LoadDefaultTemplates=0  
    AlternateSignatureAlgorithm=1  
    [CRLDistributionPoint]  
    [AuthorityInformationAccess]
   ```
1.  Cliquez sur **fichier**, puis cliquez sur **Enregistrer sous**.

2.  Accédez au dossier % SystemRoot%.

3.  Vérifiez les éléments suivants :

    -   **Nom de fichier** a la valeur **CAPolicy.inf**

    -   **Type** a la valeur **Tous les fichiers**

    -   **Codage** a la valeur **ANSI**

4.  Cliquez sur **Enregistrer**.

5.  Lorsque vous êtes invité à remplacer le fichier, cliquez sur **Oui**.

    ![Enregistrer en tant qu’emplacement pour le fichier CAPolicy.inf](../../../media/Prepare-the-CAPolicy-inf-File/001-SaveCAPolicyORCA1.gif)

    >   [!CAUTION]  
    >   Veillez à enregistrer le fichier CAPolicy.inf avec l’extension inf. Si vous ne tapez pas spécifiquement **.inf** à la fin du nom de fichier et que vous ne sélectionnez pas les options comme décrit, le fichier est enregistré au format texte et n’est pas utilisé durant l’installation de l’autorité de certification.

6.  Fermez le Bloc-notes.

>   [!IMPORTANT]  
>   Dans le fichier CAPolicy.inf, vous pouvez voir une ligne spécifiant l’URL https://pki.corp.contoso.com/pki/cps.txt. La section de stratégie interne du fichier CAPolicy.inf est juste affichée en tant qu’exemple de spécification de l’emplacement d’une déclaration de mise en œuvre des certificats. Dans ce guide, vous n’êtes pas invité à créer l’instruction de pratique de certificat (CPS).
