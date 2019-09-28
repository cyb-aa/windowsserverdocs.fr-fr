---
title: Préparer le fichier CAPolicy.inf
description: Le fichier CAPolicy. INF contient différents paramètres utilisés lors de l’installation du service de certification Active Directory (AD CS) ou lors du renouvellement du certificat de l’autorité de certification.
manager: alanth
ms.topic: article
ms.assetid: 65b36794-bb09-4c1b-a2e7-8fc780893d97
ms.prod: windows-server
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 810f6f8ba9e33f1f26f49f542ad6d23819deb463
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406290"
---
# <a name="capolicyinf-syntax"></a>Syntaxe CAPolicy. inf
>   S'applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Le fichier CAPolicy. inf est un fichier de configuration qui définit les extensions, les contraintes et d’autres paramètres de configuration appliqués à un certificat d’autorité de certification racine et à tous les certificats émis par l’autorité de certification racine. Le fichier CAPolicy. inf doit être installé sur un serveur hôte avant le début de la routine d’installation de l’autorité de certification racine. Lorsque les restrictions de sécurité sur une autorité de certification racine doivent être modifiées, le certificat racine doit être renouvelé et un fichier CAPolicy. inf mis à jour doit être installé sur le serveur avant le début du processus de renouvellement.

Le fichier CAPolicy. inf est le suivant :

-   Créé et défini manuellement par un administrateur

-   Utilisé lors de la création de certificats d’autorité de certification racine et subordonnés

-   Défini sur l’autorité de certification de signature où vous signez et émettez le certificat (pas l’autorité de certification dans laquelle la demande est accordée)

Une fois que vous avez créé votre fichier CAPolicy. inf, vous devez le copier dans le dossier **% systemroot%** de votre serveur avant d’installer ADCS ou de renouveler le certificat de l’autorité de certification.

Le fichier CAPolicy. inf permet de spécifier et de configurer un large éventail d’attributs et d’options d’autorité de certification. La section suivante décrit toutes les options permettant de créer un fichier. inf adapté à vos besoins spécifiques.

## <a name="capolicyinf-file-structure"></a>Structure du fichier CAPolicy. inf

Les termes suivants sont utilisés pour décrire la structure du fichier. inf :

-   _Section_ : zone du fichier qui couvre un groupe logique de clés. Les noms de section dans les fichiers. inf sont identifiés par des crochets. De nombreuses sections, mais pas toutes, sont utilisées pour configurer des extensions de certificat.

-   _Key_ : est le nom d’une entrée et apparaît à gauche du signe égal.

-   _Valeur_ : est le paramètre et apparaît à droite du signe égal.

Dans l’exemple ci-dessous, **[version]** est la section, **signature** est la clé, et **« \$Windows NT @ no__t-4 »** est la valeur.

Exemple :

```PowerShell
[Version]                     #section
Signature="$Windows NT$"      #key=value
```

###  <a name="version"></a>Version

Identifie le fichier en tant que fichier. inf. La version est la seule section requise et doit se trouver au début de votre fichier CAPolicy. inf.

###  <a name="policystatementextension"></a>PolicyStatementExtension

Répertorie les stratégies qui ont été définies par l’organisation et si elles sont facultatives ou obligatoires. Plusieurs stratégies sont séparées par des virgules. Les noms ont une signification dans le contexte d’un déploiement spécifique, ou par rapport à des applications personnalisées qui vérifient la présence de ces stratégies.

Pour chaque stratégie définie, il doit y avoir une section qui définit les paramètres de cette stratégie particulière. Pour chaque stratégie, vous devez fournir un identificateur d’objet (OID) défini par l’utilisateur et le texte que vous souhaitez afficher comme instruction de stratégie ou un pointeur d’URL vers l’instruction de stratégie. L’URL peut se présenter sous la forme d’une URL HTTP, FTP ou LDAP.

Si vous prévoyez d’avoir un texte descriptif dans la déclaration de stratégie, les trois lignes suivantes du fichier CAPolicy. inf se présentent comme suit :

```
[InternalPolicy]
OID=1.1.1.1.1.1.1
Notice=”Legal policy statement text”
```

Si vous prévoyez d’utiliser une URL pour héberger la déclaration de stratégie d’autorité de certification, les trois lignes suivantes ressemblent à ceci :

```
[InternalPolicy]
OID=1.1.1.1.1.1.2
URL=https://pki.wingtiptoys.com/policies/legalpolicy.asp
```

En outre :

-   Plusieurs clés d’URL et de notifications sont prises en charge.

-   Les clés de notification et d’URL de la même section de stratégie sont prises en charge.

-   Les URL avec des espaces ou du texte avec des espaces doivent être entourées de guillemets. Cela est vrai pour la clé d' **URL** , quelle que soit la section dans laquelle elle apparaît.

Voici un exemple de plusieurs notifications et URL dans une section de stratégie :

```
[InternalPolicy]
OID=1.1.1.1.1.1.1
URL=https://pki.wingtiptoys.com/policies/legalpolicy.asp
URL=ftp://ftp.wingtiptoys.com/pki/policies/legalpolicy.asp
Notice=”Legal policy statement text”
```

### <a name="crldistributionpoint"></a>CRLDistributionPoint

Vous pouvez spécifier des points de distribution de liste de révocation de certificats (CDP) pour un certificat d’autorité de certification racine dans le fichier CAPolicy. inf.  Après l’installation de l’autorité de certification, vous pouvez configurer les URL CDP incluses dans chaque certificat émis par l’autorité de certification. Le certificat d’autorité de certification racine affiche les URL spécifiées dans cette section du fichier CAPolicy. inf. 

```
[CRLDistributionPoint]
URL=http://pki.wingtiptoys.com/cdp/WingtipToysRootCA.crl
```

Pour plus d’informations sur cette section, procédez comme suit :
-   Permet
    - HTTP 
    - URL de fichier
    - URL LDAP 
    - Plusieurs URL
   
    >[!IMPORTANT]
    >Ne prend pas en charge les URL HTTPs.

-   Les guillemets doivent entourer les URL avec des espaces.

-   Si aucune URL n’est spécifiée (autrement dit, si la section **[CRLDistributionPoint]** existe dans le fichier mais est vide), l’extension d’accès aux informations de l’autorité est omise du certificat d’autorité de certification racine. Cela est généralement préférable lors de la configuration d’une autorité de certification racine. Windows n’effectue pas de vérification de la révocation sur un certificat d’autorité de certification racine, de sorte que l’extension CDP est superflue dans un certificat d’autorité de certification racine.

-    L’autorité de certification peut publier dans un fichier UNC, par exemple, sur un partage qui représente le dossier d’un site Web où un client récupère via HTTP.

-   Utilisez cette section uniquement si vous configurez une autorité de certification racine ou si vous renouvelez le certificat d’autorité de certification racine. L’autorité de certification détermine les extensions CDP de l’autorité de certification secondaire.
   

### <a name="authorityinformationaccess"></a>AuthorityInformationAccess

Vous pouvez spécifier les points d’accès aux informations de l’autorité dans le fichier CAPolicy. inf pour le certificat d’autorité de certification racine.

```
[AuthorityInformationAccess]
URL=http://pki.wingtiptoys.com/Public/myCA.crt
```

Quelques remarques supplémentaires sur la section accès aux informations de l’autorité :

-   Plusieurs URL sont prises en charge.

-   HTTP, FTP, LDAP et les URL de fichier sont pris en charge. Les URL HTTPs ne sont pas prises en charge.

-   Cette section est utilisée uniquement si vous configurez une autorité de certification racine ou si vous renouvelez le certificat d’autorité de certification racine. Les extensions AIA de l’autorité de certification secondaire sont déterminées par l’autorité de certification qui a émis le certificat de l’autorité de certification secondaire.

-   Les URL avec des espaces doivent être entourées de guillemets.

-   Si aucune URL n’est spécifiée (autrement dit, si la section **[AuthorityInformationAccess]** existe dans le fichier mais est vide), l’extension du point de distribution de la liste de révocation de certificats est omise du certificat d’autorité de certification racine. Là encore, il s’agit du paramètre préféré dans le cas d’un certificat d’autorité de certification racine, car il n’existe aucune autorité supérieure à une autorité de certification racine qui doit être référencée par un lien vers son certificat.

### <a name="certsrv_server"></a>certsrv_Server

Une autre section facultative du fichier CAPolicy. inf est [certsrv_server], qui est utilisée pour spécifier la longueur de la clé de renouvellement, la période de validité du renouvellement et la période de validité de la liste de révocation de certificats pour une autorité de certification en cours de renouvellement ou d’installation. Aucune des clés de cette section n’est requise. La plupart de ces paramètres ont des valeurs par défaut qui sont suffisantes pour la plupart des besoins et peuvent simplement être omises dans le fichier CAPolicy. inf. Un grand nombre de ces paramètres peuvent également être modifiés après l’installation de l’autorité de certification.

Voici un exemple :

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

**RenewalKeyLength** définit la taille de clé pour le renouvellement uniquement. Cela est utilisé uniquement lorsqu’une nouvelle paire de clés est générée lors du renouvellement du certificat de l’autorité de certification. La taille de clé du certificat d’autorité de certification initial est définie lors de l’installation de l’autorité de certification.

Lors du renouvellement d’un certificat d’autorité de certification avec une nouvelle paire de clés, la longueur de la clé peut être augmentée ou diminuée. Par exemple, si vous avez défini une clé d’autorité de certification racine de 4096 octets ou plus, puis découvrez que vous disposez d’applications ou de périphériques réseau Java qui ne peuvent prendre en charge que des tailles de clé de 2048 octets. Que vous augmentiez ou diminuiez la taille, vous devez réémettre tous les certificats émis par cette autorité de certification.

**RenewalValidityPeriod** et **RenewalValidityPeriodUnits** établissent la durée de vie du nouveau certificat d’autorité de certification racine lors du renouvellement de l’ancien certificat d’autorité de certification racine. Elle s’applique uniquement à une autorité de certification racine. La durée de vie du certificat d’une autorité de certification secondaire est déterminée par son supérieur. RenewalValidityPeriod peut avoir les valeurs suivantes : Heures, jours, semaines, mois et années.

**CRLPeriod** et **CRLPeriodUnits** établissent la période de validité de la liste de révocation de certificats de base. **CRLPeriod** peut avoir les valeurs suivantes : Heures, jours, semaines, mois et années.

**CRLDeltaPeriod** et **CRLDeltaPeriodUnits** établissent la période de validité de la liste de révocation de certificats delta. **CRLDeltaPeriod** peut avoir les valeurs suivantes : Heures, jours, semaines, mois et années.

Chacun de ces paramètres peut être configuré après l’installation de l’autorité de certification :

```
Certutil -setreg CACRLPeriod Weeks
Certutil -setreg CACRLPeriodUnits 1
Certutil -setreg CACRLDeltaPeriod Days
Certutil -setreg CACRLDeltaPeriodUnits 1
```

N’oubliez pas de redémarrer Active Directory Services de certificats pour que les modifications soient prises en compte.

**LoadDefaultTemplates** s’applique uniquement lors de l’installation d’une autorité de certification d’entreprise. Ce paramètre, true ou false (ou 1 ou 0), détermine si l’autorité de certification est configurée avec l’un des modèles par défaut.

Dans une installation par défaut de l’autorité de certification, un sous-ensemble des modèles de certificat par défaut est ajouté au dossier modèles de certificats dans le composant logiciel enfichable Autorité de certification. Cela signifie que dès que le service AD CS démarre après l’installation du rôle, un utilisateur ou un ordinateur disposant des autorisations suffisantes peut s’inscrire immédiatement pour obtenir un certificat.

Il se peut que vous ne souhaitiez pas émettre de certificats immédiatement après l’installation d’une autorité de certification. vous pouvez donc utiliser le paramètre LoadDefaultTemplates pour empêcher l’ajout des modèles par défaut à l’autorité de certification d’entreprise. Si aucun modèle n’est configuré sur l’autorité de certification, il ne peut émettre aucun certificat.

**AlternateSignatureAlgorithm** configure l’autorité de certification pour prendre en\#charge le format de signature PKCS 1 v 2.1 pour le certificat d’autorité de certification et les demandes de certificat. Lorsque la valeur 1 est affectée à une autorité de certification racine, le certificat\#de l’autorité de certification inclut le format de signature PKCS 1 v 2.1. Lorsqu’elle est définie sur une autorité de certification secondaire, elle crée une demande de certificat qui comprend\#le format de signature PKCS 1 v 2.1.

**ForceUTF8** remplace l’encodage par défaut des noms uniques relatifs (RDN) dans les noms distinctifs de l’objet et de l’émetteur par UTF-8. Seuls les RDN qui prennent en charge UTF-8, tels que ceux qui sont définis en tant que types de chaîne d’annuaire par une RFC, sont affectés. Par exemple, le RDN du composant de domaine (DC) prend en charge l’encodage en IA5 ou UTF-8, tandis que le pays RDN (C) prend uniquement en charge l’encodage comme chaîne imprimable. La directive ForceUTF8 affecte donc un RDN DC, mais n’affecte pas un RDN C.

**EnableKeyCounting** configure l’autorité de certification pour incrémenter un compteur chaque fois que la clé de signature de l’autorité de certification est utilisée. N’activez pas ce paramètre, sauf si vous disposez d’un module de sécurité matériel (HSM) et du fournisseur de services de chiffrement (CSP) associé qui prend en charge le décompte de clés. Ni le CSP fort Microsoft ni le fournisseur de stockage de clés (KSP) Microsoft ne prennent en charge le comptage de clés.


## <a name="create-the-capolicyinf-file"></a>Créer le fichier CAPolicy. inf

Avant d’installer les services AD CS, vous configurez le fichier CAPolicy. inf avec des paramètres spécifiques pour votre déploiement.

**Requis** Vous devez être membre du groupe administrateurs.

1. Sur l’ordinateur sur lequel vous prévoyez d’installer les services AD CS, ouvrez Windows PowerShell, tapez **notepad c:\CAPolicy.inf** et appuyez sur entrée.

2. À l’invite de création de fichier, cliquez sur **Oui**.

3. Entrez le code suivant comme contenu du fichier :
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
4. Cliquez sur **fichier**, puis sur **Enregistrer sous**.

5. Accédez au dossier% systemroot%.

6. Vérifiez les éléments suivants :

   -   **Nom de fichier** a la valeur **CAPolicy.inf**

   -   **Type** a la valeur **Tous les fichiers**

   -   **Codage** a la valeur **ANSI**

7. Cliquez sur **Enregistrer**.

8. Lorsque vous êtes invité à remplacer le fichier, cliquez sur **Oui**.

   ![Enregistrer en tant qu’emplacement pour le fichier CAPolicy. inf](../../../media/Prepare-the-CAPolicy-inf-File/001-SaveCAPolicyORCA1.gif)

   > [!CAUTION]
   >   Veillez à enregistrer le fichier CAPolicy.inf avec l’extension inf. Si vous ne tapez pas spécifiquement **.inf** à la fin du nom de fichier et que vous ne sélectionnez pas les options comme décrit, le fichier est enregistré au format texte et n’est pas utilisé durant l’installation de l’autorité de certification.

9. Fermez le Bloc-notes.

> [!IMPORTANT]
>   Dans le fichier CAPolicy. inf, vous pouvez voir qu’une ligne spécifie l' https://pki.corp.contoso.com/pki/cps.txt URL. La section de stratégie interne du fichier CAPolicy.inf est juste affichée en tant qu’exemple de spécification de l’emplacement d’une déclaration de mise en œuvre des certificats. Dans ce guide, vous n’êtes pas invité à créer la déclaration CPS (Certificate Practice Statement).
