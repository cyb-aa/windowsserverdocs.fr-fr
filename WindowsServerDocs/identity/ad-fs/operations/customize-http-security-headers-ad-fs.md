---
title: Personnaliser des en-têtes de réponse de sécurité HTTP avec AD FS
description: Ce document décrit comment personnaliser les en-têtes de sécurité pour vous protéger contre des failles de sécurité.
author: billmath
ms.author: billmath
manager: daveba
ms.reviewer: akgoel23
ms.date: 02/19/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: cd3ad4e6547194a971d8a51ecb95ee56f5e4e8c2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822720"
---
# <a name="customize-http-security-response-headers-with-ad-fs-2019"></a>Personnaliser des en-têtes de réponse de sécurité HTTP avec AD FS 2019 
S'applique à : Windows Server 2019 
 
Pour protéger contre les vulnérabilités de sécurité courantes et fournissent aux administrateurs la possibilité de tirer parti des derniers progrès de mécanismes de protection basée sur navigateur, AD FS 2019 ajouté la fonctionnalité permettant de personnaliser les en-têtes de réponse de sécurité HTTP envoyé par AD FS. Cela est réalisé via l’introduction de deux nouvelles applets de commande : `Get-AdfsResponseHeaders` et `Set-AdfsResponseHeaders`.  
 
Dans ce document, que nous aborderons couramment utilisé les en-têtes de réponse de sécurité pour montrer comment personnaliser les en-têtes envoyés par AD FS 2019.   
 
>[!NOTE]
>Le document suppose que AD FS 2019 a été installé.  

 
Avant d’aborder les en-têtes, jetons un œil à quelques scénarios de création de la nécessité pour les administrateurs de personnaliser les en-têtes de sécurité 
 
## <a name="scenarios"></a>Scénarios 
1. L’administrateur a activé [ **HTTP Strict-Transport-Security (HSTS)** ](#http-strict-transport-security-hsts) (force toutes les connexions sur le chiffrement HTTPS) pour protéger les utilisateurs qui peuvent accéder à l’application web à l’aide de HTTP à partir d’un accès Wi-Fi public point qui peut être piraté. Elle souhaite renforcer davantage la sécurité en activant HSTS des sous-domaines.  
2. L’administrateur a configuré le [ **X-Frame-Options** ](#x-frame-options) en-tête de réponse (empêche le rendu de n’importe quelle page web dans un iFrame) pour protéger les pages web ne soient pas clickjacked. Toutefois, elle est chargée personnaliser la valeur d’en-tête en raison d’une nouvelle spécification de l’entreprise pour afficher des données (dans un iFrame) à partir d’une application avec une autre origine (domaine).
3. L’administrateur a activé [ **X-XSS-Protection** ](#x-xss-protection) (empêche entre les attaques de script) pour nettoyer et de bloquer la page si le navigateur détecte entre les attaques de script. Toutefois, elle doit personnaliser l’en-tête pour autoriser le chargement de la page purgé qu’une seule fois.  
4. L’administrateur doit activer [ **Cross-Origin CORS (Resource Sharing)** ](#cross-origin-resource-sharing-cors-headers) et définir l’origine (domaine) sur AD FS pour autoriser une Application à Page unique accéder à une API web avec un autre domaine.  
5. L’administrateur a activé [ **stratégie de sécurité de contenu (CSP)** ](#content-security-policy-csp) en-tête pour empêcher entre l’injection de script et de données de site les attaques en interdisant les demandes inter-domaines. Toutefois, en raison d’une nouvelle exigence d’entreprise dont elle a besoin personnaliser l’en-tête pour autoriser la page web à charger des images à partir de toute origine et de restreindre les médias aux fournisseurs approuvés.  

 
## <a name="http-security-response-headers"></a>En-têtes de réponse de sécurité HTTP 
Les en-têtes de réponse sont inclus dans la réponse HTTP sortante envoyée par AD FS dans un navigateur web. Les en-têtes peuvent être répertoriés à l’aide de la `Get-AdfsResponseHeaders` applet de commande comme indiqué ci-dessous.  

![Réponse de l’en-tête](media\customize-http-security-headers-ad-fs\header1.png)

Le `ResponseHeaders` attribut dans la capture d’écran ci-dessus identifie les en-têtes de sécurité qui seront inclus par AD FS dans chaque réponse HTTP. Les en-têtes de réponse seront envoyés uniquement si `ResponseHeadersEnabled` a la valeur `True` (valeur par défaut). La valeur peut être définie `False` afin d’éviter d’AD FS, y compris ceux des en-têtes de sécurité dans la réponse HTTP. Cela n’est toutefois pas recommandé.  Pour cela, utilisez les éléments suivants :

```PowerShell
Set-AdfsResponseHeaders -EnableResponseHeaders $false
```
 
### <a name="http-strict-transport-security-hsts"></a>HTTP Strict-Transport-Security (HSTS) 
HSTS est un mécanisme de stratégie de sécurité web qui vous aide à atténuer les attaques de protocole vers une version antérieure et détournement de cookie pour les services qui ont des points de terminaison HTTP et HTTPS. Il permet de serveurs web déclarer que les navigateurs web (ou autres agents utilisateurs conforme) doivent uniquement interagir avec lui à l’aide de HTTPS et jamais via le protocole HTTP.  
 
Tous les points de terminaison AD FS pour le trafic d’authentification web sont ouverts exclusivement via le protocole HTTPS. Par conséquent, ADFS permet d’atténuer efficacement les menaces qui fournit du mécanisme de stratégie de sécurité du Transport HTTP Strict (par défaut il n’est aucun vers une version antérieure à HTTP, car il n’existe aucun écouteur HTTP). L’en-tête peut être personnalisé en définissant les paramètres suivants 
 
- **max-age =&lt;temps expirer&gt;**  : le délai d’expiration (en secondes) spécifie la durée pendant laquelle le site doit uniquement être accessible à l’aide de HTTPS. Valeur par défaut et recommandée est 31536000 secondes (1 an).  
- **includeSubDomains** – il s’agit d’un paramètre facultatif. Si spécifié, la règle HSTS s’applique à tous les sous-domaines également.  
 
#### <a name="hsts-customization"></a>Personnalisation de HSTS 
Par défaut, l’en-tête est activée et `max-age` défini sur 1 an ; Toutefois, les administrateurs peuvent modifier le `max-age` (l’en réduisant la valeur d’âge maximal n’est pas recommandé) ou activer HSTS pour les sous-domaines via le `Set-AdfsResponseHeaders` applet de commande.  
 
```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "Strict-Transport-Security" -SetHeaderValue "max-age=<seconds>; includeSubDomains" 
``` 

Exemple : 

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "Strict-Transport-Security" -SetHeaderValue "max-age=31536000; includeSubDomains" 
 ```

Par défaut, l’en-tête est inclus dans le `ResponseHeaders` attribut ; Toutefois, les administrateurs peuvent supprimer l’en-tête via le `Set-AdfsResponseHeaders` applet de commande.  
 
```PowerShell
Set-AdfsResponseHeaders -RemoveHeaders "Strict-Transport-Security" 
```

### <a name="x-frame-options"></a>X-Frame-Options 
AD FS par défaut n’autorise pas les applications externes à utiliser des iFrames lorsque vous effectuez des connexions interactives. Cela vise à empêcher un style spécifique d’attaques de phishing. Notez que les connexions non interactives peuvent être effectuées via iFrame en raison de la sécurité de niveau session antérieure qui a été établie.  
 
Toutefois, dans certains cas rares, vous pourrez approuver une application spécifique qui nécessite la page de connexion FS iFrame capable AD interactif. L’en-tête 'X-Frame-Options' est utilisé à cet effet.  
 
Cet en-tête de réponse de sécurité HTTP est utilisé pour communiquer avec le navigateur si elle peut restituer une page dans un &lt;frame&gt;/&lt;iframe&gt;. L’en-tête peut être défini à une des valeurs suivantes : 
 
- **refuser** : la page dans un frame ne s’affichera pas. Ceci est la valeur par défaut et recommandée.  
- **sameorigin** – la page sera uniquement être affichée dans le cadre de si l’origine est identique à l’origine de la page web. L’option n’est pas très utile, sauf si tous les ancêtres sont également dans la même origine.  
- **autoriser de <specified origin>**  -la page sera uniquement être affichée dans le frame se l’origine (par exemple, https://www. ». com) correspond à l’origine spécifique dans l’en-tête. 

#### <a name="x-frame-options-customization"></a>Personnalisation de X-Frame-Options  
Par défaut, en-tête est défini à refuser ; Toutefois, les administrateurs peuvent modifier la valeur via le `Set-AdfsResponseHeaders` applet de commande.  
```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "X-Frame-Options" -SetHeaderValue "<deny/sameorigin/allow-from<specified origin>>" 
 ```

Exemple : 

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "X-Frame-Options" -SetHeaderValue "allow-from https://www.example.com" 
 ```

Par défaut, l’en-tête est inclus dans le `ResponseHeaders` attribut ; Toutefois, les administrateurs peuvent supprimer l’en-tête via le `Set-AdfsResponseHeaders` applet de commande.  

```PowerShell
Set-AdfsResponseHeaders -RemoveHeaders "X-Frame-Options" 
```

### <a name="x-xss-protection"></a>X-XSS-Protection 
Cet en-tête de réponse de sécurité HTTP est utilisé pour arrêter les pages web à partir de chargement lorsque des attaques cross site scripting (XSS) sont détectées par les navigateurs. Cela est appelé XSS filtrage. L’en-tête peut être défini à une des valeurs suivantes 
 
- **0** – XSS désactive le filtrage. Non recommandé.  
- **1** – XSS permet le filtrage. Si l’attaque XSS est détectée, le navigateur Assainit la page.   
- **1 ; mode = bloc** – XSS permet le filtrage. Si l’attaque XSS est détectée, navigateur empêche le rendu de la page. Ceci est la valeur par défaut et recommandée.  

#### <a name="x-xss-protection-customization"></a>Personnalisation de X-XSS-Protection 
Par défaut, l’en-tête sera être défini sur 1 ; mode = bloc ; Toutefois, les administrateurs peuvent modifier la valeur via le `Set-AdfsResponseHeaders` applet de commande.  

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "X-XSS-Protection" -SetHeaderValue "<0/1/1; mode=block/1; report=<reporting-uri>>" 
``` 

Exemple : 

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "X-XSS-Protection" -SetHeaderValue "1" 
 ```

Par défaut, l’en-tête est inclus dans le `ResponseHeaders` attribut ; Toutefois, les administrateurs peuvent supprimer l’en-tête via le `Set-AdfsResponseHeaders` applet de commande. 

```PowerShell
Set-AdfsResponseHeaders -RemoveHeaders "X-XSS-Protection" 
```

### <a name="cross-origin-resource-sharing-cors-headers"></a>Entre les en-têtes de partage des ressources d’origine (CORS) 
Sécurité des navigateurs Web empêche une page web d’effectuer des demandes de cross-origin initialisées à partir de scripts. Toutefois, vous pouvez parfois d’accéder aux ressources dans d’autres origines (domaines). CORS est une norme W3C qui permet à un serveur d’assouplir la stratégie de même origine. À l’aide de CORS, un serveur peut autoriser explicitement certaines demandes cross-origin lors du refus d’autres.  
 
Pour mieux comprendre demande CORS, nous allons procédure pas à pas un scénario où une seule page (SPA) d’application doit appeler une API web avec un autre domaine. En outre, prenons l’exemple que SPA et API sont configurées sur ADFS 2019 et AD FS a CORS est activé par exemple, AD FS peut identifier les en-têtes CORS dans la requête HTTP, valider les valeurs d’en-tête et inclure des en-têtes CORS appropriés dans la réponse (plus d’informations sur l’activation et configurer CORS sur AD FS des 2019 dans CORS de personnalisation, section ci-dessous). Exemple de flux : 

1. Utilisateur accède à SPA via le navigateur client et est redirigé vers le point de terminaison AD FS d’authentification pour l’authentification. SPA n’est configuré pour le flux d’octroi implicite, demander le jeton retourne un accès + l’ID dans le navigateur après une authentification réussie.  
2. Une fois l’authentification utilisateur, le front-end JavaScript inclus dans SPA effectue une demande à accéder à l’API web. La requête est redirigée vers AD FS avec les en-têtes suivants
    - Options : décrit les options de communication pour la ressource cible 
    - Origine – inclut l’origine de l’API web
    - Access-Control-Request-Method – identifie la méthode HTTP (par exemple, supprimer) à utiliser lors de la demande réelle est effectuée. 
    - Access-Control-Request-Headers - identifie les en-têtes HTTP à utiliser lors de la demande réelle est effectuée. 
    
   >[!NOTE]
   >Demande CORS ressemble à une requête HTTP standard, toutefois, la présence d’un en-tête d’origine signale que la demande entrante est CORS liés. 
3. AD FS vérifie que le web origine API inclus dans l’en-tête est répertorié dans les origines approuvées configurés dans AD FS (plus d’informations sur comment modifier les origines approuvées dans CORS de personnalisation, section ci-dessous). AD FS répond ensuite avec les en-têtes suivants.  
    - Access-Control-Allow-Origin – même valeur, comme dans l’en-tête d’origine 
    - Access-Control-autoriser-Method – même valeur, comme dans l’en-tête Access-Control-Request-Method 
    - Access-Control-Allow-Headers - même valeur, comme dans l’en-tête Access-Control-Request-Headers 
4. Navigateur envoie la demande réelle, y compris les en-têtes suivants 
    - Méthode HTTP (par exemple, supprimer) 
    - Origine – inclut l’origine de l’API web 
    - Tous les en-têtes inclus dans l’en-tête de réponse Access-Control-Allow-Headers 
5. Une fois vérifiée, AD FS approuve la demande en incluant le domaine d’API web (origine) dans l’en-tête de réponse Access-Control-Allow-Origin.  
6. L’inclusion de l’en-tête Access-Control-Allow-Origin permettra le navigateur accéder à l’avance avec l’appel de l’API demandée.

#### <a name="cors-customization"></a>Personnalisation de CORS 
Par défaut, les fonctionnalités CORS ne seront pas activée ; Toutefois, les administrateurs peuvent activer les fonctionnalités via l’applet de commande Set-AdfsResponseHeaders.  

```PowerShell 
Set-AdfsResponseHeaders -EnableCORS $true 
 ```

Une option est activée, les administrateurs seront en mesure d’énumérer une liste d’origines approuvées à l’aide de l’applet de commande. Par exemple, la commande suivante permettrait de demandes CORS des origines **https&#58;//example1.com** et **https&#58;//example1.com**. 
 
```PowerShell
Set-AdfsResponseHeaders -CORSTrustedOrigins https://example1.com,https://example2.com 
 ```

> [!NOTE]
> Les administrateurs peuvent autoriser les demandes CORS à partir de toute origine en incluant « * » dans la liste des origines approuvées, bien que cette approche n’est pas recommandée en raison de vulnérabilités de sécurité et un message d’avertissement est fournie si dont ils ont besoin. 

### <a name="content-security-policy-csp"></a>Stratégie de sécurité du contenu (CSP) 
Cet en-tête de réponse de sécurité HTTP est utilisé pour empêcher les scripts intersites, le détournement de clics et autres attaques par injection de données en empêchant les navigateurs de l’exécution par inadvertance de contenu malveillant. Les navigateurs qui ne prennent pas en charge le CSP ignore simplement les en-têtes de réponse CSP.  
 
#### <a name="csp-customization"></a>Personnalisation du CSP 
Personnalisation de l’en-tête CSP implique la modification de la stratégie de sécurité qui définit le navigateur de ressources est autorisée à charger pour la page web. La stratégie de sécurité par défaut est  
 
`Content-Security-Policy: default-src ‘self’ ‘unsafe-inline’ ‘’unsafe-eval’; img-src ‘self’ data:;` 
 
Le **par défaut-src** directive est utilisée pour modifier [- src directives](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/default-src) sans avoir à mentionner explicitement de chaque directive. Par exemple, dans l’exemple ci-dessous, la stratégie 1 est identique à la stratégie 2.  

Stratégie 1 
```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "Content-Security-Policy" -SetHeaderValue "default-src 'self'" 
```
 
Stratégie 2
```PowerShell 
Set-AdfsResponseHeaders -SetHeaderName "Content-Security-Policy" -SetHeaderValue "script-src ‘self’; img-src ‘self’; font-src 'self';  
frame-src 'self'; manifest-src 'self'; media-src 'self';" 
```

Si une directive est répertoriée explicitement, la valeur spécifiée substitue à la valeur donnée pour la valeur par défaut-src. Dans l’exemple ci-dessous, img-src prendra la valeur en tant que « * » (ce qui permet d’images à charger à partir de toute origine) tandis que les autres directives - src prendra la valeur comme « personnel » (limitant à la même origine que la page web).  

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "Content-Security-Policy" -SetHeaderValue "default-src ‘self’; img-src *" 
```
Les sources suivantes peuvent être définies pour la stratégie par défaut-src 
 
- 'self' – spécifiant cette limite à l’origine du contenu à charger à l’origine de la page web 
- 'unsafe-inline' – Cela spécifiant dans la stratégie autorise l’utilisation de code JavaScript intégré et CSS 
- 'unsafe eval' – Cela spécifiant dans la stratégie autorise l’utilisation de texte à JavaScript des mécanismes tels qu’eval 
- 'none' – spécifiant cette limite le contenu à partir de toute origine à charger 
- données :-spécification des données : URI permet aux créateurs de contenu incorporer des petits fichiers inline dans les documents. Utilisation déconseillée.  
 
>[!NOTE]
>AD FS utilise du code JavaScript dans le processus d’authentification et par conséquent, JavaScript en incluant 'unsafe inline' et 'unsafe eval' sources par défaut stratégie.  

### <a name="custom-headers"></a>En-têtes personnalisés 
Outre les précautions ci-dessus répertorié les en-têtes de réponse de sécurité (HSTS, CSP, X-Frame-Options, X-XSS-Protection et CORS), AD FS 2019 offre la possibilité de définir de nouveaux en-têtes.  
 
Exemple : Pour définir un nouvel en-tête « TestHeader » avec la valeur en tant que « TestHeaderValue » 

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "TestHeader" -SetHeaderValue "TestHeaderValue" 
 ```

Une fois définie, le nouvel en-tête est envoyé dans la réponse d’AD FS (fiddler extrait de code ci-dessous).  
 
![Fiddler](media\customize-http-security-headers-ad-fs\header2.png)

## <a name="web-browswer-compatibility"></a>Compatibilité de navigateur attribuant alors Web
Utilisez le tableau et les liens suivants pour déterminer quels sont les navigateurs web sont compatibles avec chacun des en-tête de réponse de sécurité.

|En-têtes de réponse de sécurité HTTP|Compatibilité du navigateur|
|-----|-----|
|HTTP Strict-Transport-Security (HSTS)|[Compatibilité des navigateurs HSTS](https://developer.mozilla.org/docs/Web/HTTP/Headers/Strict-Transport-Security#Browser_compatibility)|
|X-Frame-Options|[Compatibilité des navigateurs de X-Frame-Options](https://developer.mozilla.org/docs/Web/HTTP/Headers/X-Frame-Options#Browser_compatibility)| 
|X-XSS-Protection|[Compatibilité des navigateurs de X-XSS-Protection](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-XSS-Protection#Browser_compatibility)| 
|Cross-Origin Resource Sharing (CORS)|[Compatibilité des navigateurs CORS](https://developer.mozilla.org/docs/Web/HTTP/CORS#Browser_compatibility) 
|Stratégie de sécurité du contenu (CSP)|[Compatibilité des navigateurs CSP](https://developer.mozilla.org/docs/Web/HTTP/CSP#Browser_compatibility) 

## <a name="next"></a>Suivant

- [Utiliser AD FS Help troublehshooting guides](https://aka.ms/adfshelp/troubleshooting )
- [Résolution des problèmes de AD FS](../../ad-fs/troubleshooting/ad-fs-tshoot-overview.md)
