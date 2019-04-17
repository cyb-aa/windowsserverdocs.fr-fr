---
title: Préparer le fichier CAPolicy.inf
description: Le CAPolicy.inf contient différents paramètres qui sont utilisés lors de l’installation du Service de Certification d’ActiveDirectory (ADCS) ou lors du renouvellement de l’autorité de certification certificat.
manager: alanth
ms.topic: article
ms.assetid: 65b36794-bb09-4c1b-a2e7-8fc780893d97
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9618d4abe512b487f4f22ffde85a052c1c52ef22
ms.sourcegitcommit: fb4e2ace2e0a29e0f6b028f1cb945cab6aa6ee2c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/05/2018
---
# <a name="capolicyinf-syntax"></a>Syntaxe de CAPolicy.inf
>   S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Le CAPolicy.inf est un fichier de configuration qui définit les extensions de contraintes et autres paramètres de configuration qui sont appliqués à un certificat d’autorité de certification racine et tous les certificats émis par l’autorité de certification racine. Le fichier CAPolicy.inf doit être installé sur un serveur hôte avant la routine d’installation pour l’autorité de certification de racine. Lorsque les restrictions de sécurité sur une autorité de certification doivent être modifiées, le certificat racine doit être renouvelé et un fichier CAPolicy.inf mis à jour doit être installé sur le serveur avant le début du processus de renouvellement.

Le CAPolicy.inf est:

-   Créé et défini manuellement par un administrateur

-   Utilisé lors de la création de racine et les certificats d’autorité de certification secondaires

-   Défini sur l’autorité de certification de signature où vous connecterez et émettez le certificat (pas l’autorité de certification où la demande soit accordée)

Une fois que vous avez créé votre fichier CAPolicy.inf, vous devez le copier dans le **% SystemRoot%** dossier de votre serveur avant d’installer ADCS ou renouveler le certificat d’autorité de certification.

Le CAPolicy.inf rend possible spécifier et configurer un large éventail d’options et les attributs de l’autorité de certification. La section suivante décrit toutes les options vous permettant de créer un fichier .inf adapté à vos besoins spécifiques.

## <a name="capolicyinf-file-structure"></a>Structure du fichier CAPolicy.inf

Les termes suivants sont utilisés pour décrire la structure du fichier .inf:

-   _Section_ – est une zone du fichier qui couvre un groupe logique de clés. Les noms de section dans les fichiers .inf sont identifiés par apparaissent entre crochets. Les sections de nombreuses, mais pas tous, sont utilisées pour configurer les extensions de certificat.

-   _Clé_: est le nom d’une entrée et apparaît à gauche du signe égal.

-   _Valeur_: est le paramètre et s’affiche à droite du signe égal.

Dans l’exemple ci-dessous, **[Version]** est la section, **Signature** est la clé, et **«\ $ Windows NT \ $«** est la valeur.

Exemple:

```PowerShell
[Version]                     #section
Signature="$Windows NT$"      #key=value
```

###  <a name="version"></a>Version

Identifie le fichier comme un fichier .inf. Version est la seule section requise doit être au début de votre fichier CAPolicy.inf.

###  <a name="policystatementextension"></a>PolicyStatementExtension

Répertorie les stratégies qui ont été définies par l’organisation, et s’ils sont facultative ou obligatoire. Plusieurs stratégies sont séparés par des virgules. Les noms ont une signification dans le cadre d’un déploiement spécifique, ou en ce qui concerne les applications personnalisées qui vérifient la présence de ces stratégies.

Pour chaque stratégie définie, il doit exister une section qui définit les paramètres de stratégie déterminée. Pour chaque stratégie, vous devez fournir un identificateur d’objet défini par l’utilisateur (OID) et le texte que vous souhaitez afficher en tant que l’instruction de stratégie ou d’un pointeur d’URL à l’instruction de stratégie. L’URL peut être sous la forme d’une HTTP, FTP ou URL LDAP.

Si vous souhaitez que le texte descriptif dans l’instruction de stratégie, les trois lignes de la CAPolicy.inf ressemblerait à ceci:

```
[InternalPolicy]
OID=1.1.1.1.1.1.1
Notice=”Legal policy statement text”
```

Si vous envisagez d’utiliser une URL pour héberger l’instruction de stratégie d’autorité de certification, puis trois lignes au lieu de cela ressemblerait à ceci:

```
[InternalPolicy]
OID=1.1.1.1.1.1.2
URL=http://pki.wingtiptoys.com/policies/legalpolicy.asp
```

De plus,:

-   Plusieurs clés d’URL et les avis sont pris en charge.

-   Clés d’avis et l’URL dans la même section de stratégie sont pris en charge.

-   URL avec des espaces ou du texte avec les espaces doit être entouré de guillemets. Cela est vrai pour les **URL** clé, quel que soit la section dans laquelle il s’affiche.

Un exemple de plusieurs notifications et les URL dans la section stratégie ressemblerait à ceci:

```
[InternalPolicy]
OID=1.1.1.1.1.1.1
URL=http://pki.wingtiptoys.com/policies/legalpolicy.asp
URL=ftp://ftp.wingtiptoys.com/pki/policies/legalpolicy.asp
Notice=”Legal policy statement text”
```

### <a name="crldistributionpoint"></a>CRLDistributionPoint

Vous pouvez spécifier des Points de Distribution CRL (CDP) pour un certificat d’autorité de certification racine dans le CAPolicy.inf. Une fois que l’autorité de certification a été installée, vous pouvez configurer les URL CDP qui inclut de l’autorité de certification dans chaque certificat émis. Les URL spécifiées dans cette section du fichier CAPolicy.inf sont inclus dans le certificat d’autorité de certification racine lui-même.

```
[CRLDistributionPoint]
URL=http://pki.wingtiptoys.com/cdp/WingtipToysRootCA.crl
```

Des informations supplémentaires sur cette section:

-   Plusieurs URL sont pris en charge.

-   Prise en charge HTTP, FTP et les URL de LDAP. URL HTTPS ne sont pas pris en charge.

-   Cette section est utilisée uniquement si vous configurez une autorité de certification racine ou renouvellement du certificat d’autorité de certification racine. Les extensions CDP de l’autorité de certification secondaire sont déterminées par l’autorité de certification qui émet le certificat de l’autorité de certification secondaire.

-   URL avec les espaces doivent être entourés de guillemets.

-   Si aucune URL n’est spécifiés: autrement dit, si le **[CRLDistributionPoint]** section existe dans le fichier, mais est vide, l’extension de Point de Distribution CRL est omise du certificat d’autorité de certification racine. Cette méthode est généralement préférable lorsque vous configurez une autorité de certification racine. Windows n’effectue pas la vérification de la sur un certificat d’autorité de certification racine, l’extension CDP est superflue dans un certificat d’autorité de certification racine.

### <a name="authorityinformationaccess"></a>AuthorityInformationAccess

Vous pouvez spécifier les points d’accès autorité des informations dans le CAPolicy.inf pour le certificat d’autorité de certification racine.

```
[AuthorityInformationAccess]
URL=http://pki.wingtiptoys.com/Public/myCA.crt
```

Quelques remarques supplémentaires sur la section d’accès aux informations autorité:

-   Plusieurs URL sont pris en charge.

-   HTTP, FTP, LDAP et URL de fichier sont pris en charge. URL HTTPS ne sont pas pris en charge.

-   Cette section est utilisée uniquement si vous configurez une autorité de certification racine, ou renouvellement du certificat d’autorité de certification racine. Les extensions AIA de l’autorité de certification secondaire sont déterminées par l’autorité de certification qui a émis le certificat de l’autorité de certification secondaire.

-   URL avec les espaces doivent être entourés de guillemets.

-   Si aucune URL n’est spécifiés: autrement dit, si le **[AuthorityInformationAccess]** section existe dans le fichier, mais est vide, l’extension de Point de Distribution CRL est omise du certificat d’autorité de certification racine. Là encore, il s’agit du paramètre recommandé dans le cas d’un certificat d’autorité de certification racine comme il n’existe aucune autorité supérieure à une autorité de certification qui doit être référencées par un lien vers son certificat racine.

### <a name="certsrvserver"></a>certsrv_Server

Une autre section facultative de la CAPolicy.inf est [certsrv_server], qui est utilisée pour spécifier la longueur de clé de renouvellement, la période de validité de renouvellement et la période de validité de liste de révocation de révocation des certificats pour une autorité de certification est est renouvelé ou installé. Aucun des clés dans cette section sont requises. La plupart de ces paramètres ont des valeurs par défaut qui sont suffisantes pour la plupart des besoins et peuvent simplement être omis à partir du fichier CAPolicy.inf. Sinon, la plupart de ces paramètres peuvent être modifiées après l’installation de l’autorité de certification.

Un exemple ressemblerait à ceci:

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

**RenewalKeyLength** définit la taille de clé pour le renouvellement uniquement. Il est utilisé uniquement lorsqu’une nouvelle paire de clés est générée lors du renouvellement de certificat d’autorité de certification. La taille de clé pour le certificat d’autorité de certification initiale est définie lors de l’autorité de certification est installée.

Renouveler un certificat d’autorité de certification avec une nouvelle paire de clés, la longueur de clé peut être augmentée ou diminuée. Par exemple, si vous avez défini une racine d’autorité de certification taille de la clé de 4096octets ou plus, puis découvrez que vous avez applications Java ou les périphériques réseau qui peuvent prendre uniquement en charge des tailles de clé de 2048octets. Si vous augmentez ou réduisez la taille, vous devez émettre de nouveau tous les certificats émis par cette autorité de certification.

**RenewalValidityPeriod** et **RenewalValidityPeriodUnits** établir la durée de vie du nouveau certificat d’autorité de certification racine lors du renouvellement de l’ancien certificat d’autorité de certification racine. Il s’applique uniquement à une autorité de certification racine. La durée de vie du certificat d’autorité de certification subordonnée est déterminée par son supérieur. RenewalValidityPeriod peut avoir les valeurs suivantes: heures, jours, semaines, mois et années.

**Période_crl** et **CRLPeriodUnits** établir la période de validité pour la liste CRL de base. **Période_crl** peut avoir les valeurs suivantes: heures, jours, semaines, mois et années.

**Période_delta_crl** et **CRLDeltaPeriodUnits** établir la période de validité de la révocation de certificats delta. **Période_delta_crl** peut avoir les valeurs suivantes: heures, jours, semaines, mois et années.

Chacun de ces paramètres peut être configuré après l’installation de l’autorité de certification:

```
Certutil -setreg CACRLPeriod Weeks
Certutil -setreg CACRLPeriodUnits 1
Certutil -setreg CACRLDeltaPeriod Days
Certutil -setreg CACRLDeltaPeriodUnits 1
```

N’oubliez pas de redémarrer les Services de certificats ActiveDirectory pour toutes les modifications prennent effet.

**LoadDefaultTemplates** s’applique uniquement lors de l’installation d’une autorité de certification d’entreprise. Ce paramètre, soit True ou False (ou 1 ou 0), détermine si l’autorité de certification est configurée avec tous les modèles par défaut.

Dans une installation par défaut de l’autorité de certification, un sous-ensemble des modèles de certificats par défaut est ajouté au dossier Modèles de certificats dans le composant logiciel enfichable Autorité de Certification. Cela signifie que dès que le service ADCS démarre après avoir installé le rôle d’un utilisateur ou un ordinateur disposant des autorisations suffisantes peut immédiatement inscrire un certificat.

Vous voudrez peut-être pas émettre de certificats immédiatement après avoir installé une autorité de certification, vous pouvez utiliser le paramètre LoadDefaultTemplates afin d’éviter les modèles par défaut d’être ajouté à l’autorité de certification d’entreprise. Si aucun modèle configuré sur l’autorité de certification qu’il ne peut pas émettre pas de certificats.

**AlternateSignatureAlgorithm** configure l’autorité de certification pour prendre en charge le format de signature PKCS\ #1 V2.1 pour le certificat d’autorité de certification et les demandes de certificat. Lorsque la valeur 1 sur une racine d’autorité de certification le certificat d’autorité de certification inclut le format de signature PKCS\ #1 V2.1. Lorsque la valeur sur un subordonné autorité de certification, l’autorité de certification secondaire crée une demande de certificat qui inclut le format de signature PKCS\ #1 V2.1.

**ForceUTF8** modifie la valeur par défaut le codage des noms uniques relatifs (RDN) dans l’objet et de l’émetteur des noms au format UTF-8uniques. Uniquement les noms uniques relatifs qui prennent en charge le format UTF-8, telles que celles qui sont définies comme des types de chaîne de répertoire par RFC, sont affectés. Par exemple, le nom unique relatif de composant de domaine (DC) prend en charge du codage, comme IA5 ou UTF-8, tandis que le Country RDN (C) prend uniquement en charge le codage sous forme de chaîne imprimable. La directive ForceUTF8 affecte par conséquent, un nom unique relatif du contrôleur de domaine mais n’affectera pas un nom unique relatif C.

**EnableKeyCounting** configure l’autorité de certification pour incrémenter un compteur chaque fois que la clé de signature de l’autorité de certification est utilisée. N’activez pas ce paramètre, sauf si vous disposez d’un Module de sécurité matériel (HSM) et le fournisseur de services de chiffrement associée (CSP) qui prend en charge le comptage des clés. Fournisseur CSP fort Microsoft ni comptage clé Microsoft fournisseur de stockage de clés (KSP) logiciel prise en charge.


## <a name="create-the-capolicyinf-file"></a>Créer le fichier CAPolicy.inf

Avant d’installer les services ADCS, vous configurez le fichier CAPolicy.inf avec des paramètres spécifiques pour votre déploiement.

**La configuration requise:** vous devez être membre du groupe Administrateurs.

1.  Sur l’ordinateur où vous prévoyez d’installer les services ADCS, ouvrez Windows PowerShell, tapez **le bloc-notes c:.inf** et appuyez sur ENTRÉE.

2.  Lorsque vous êtes invité à créer un nouveau fichier, cliquez sur **Oui **.

3.  Entrez les éléments suivants en tant que le contenu du fichier:
   ```
   [Version]  
    Signature="$Windows NT$"  
    [PolicyStatementExtension]  
    Policies=InternalPolicy  
    [InternalPolicy]  
    OID=1.2.3.4.1455.67.89.5  
    Notice="Legal Policy Statement"  
    URL=http://pki.corp.contoso.com/pki/cps.txt  
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
1.  Cliquez sur **fichier**, puis cliquez sur **Enregistrer sous **.

2.  Naviguez vers le dossier % SystemRoot%.

3.  Vérifiez les points suivants:

    -   **Nom de fichier** est défini sur **CAPolicy.inf**

    -   **Enregistrer en tant que type** est défini sur **tous les fichiers**

    -   **Codage** est **ANSI**

4.  Cliquez sur **enregistrer**.

5.  Lorsque vous êtes invité à remplacer le fichier, cliquez sur **Oui**.

    ![Enregistrer en tant qu’emplacement pour le fichier CAPolicy.inf](../../../media/Prepare-the-CAPolicy-inf-File/001-SaveCAPolicyORCA1.gif)

    >   [!CAUTION]  
    >   Veillez à enregistrer le CAPolicy.inf avec l’extension inf. Si vous ne tapez pas spécifiquement **.inf** à la fin du nom de fichier et sélectionnez les options comme décrit, le fichier sera enregistré comme un fichier texte et ne doit pas servir lors de l’installation de l’autorité de certification.

6.  Fermez le bloc-notes.

>   [!IMPORTANT]  
>   Dans le CAPolicy.inf, vous pouvez voir une ligne qui spécifie l’URL http://pki.corp.contoso.com/pki/cps.txt. La section stratégie interne du CAPolicy.inf est juste affichée en tant qu’un exemple de spécification de l’emplacement d’une instruction pratique de certificat (CPS). Dans ce guide, vous n’êtes pas invité à créer l’instruction pratique de certificat (CPS).
