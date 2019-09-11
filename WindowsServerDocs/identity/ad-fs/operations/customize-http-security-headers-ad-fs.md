---
title: Personnaliser les en-têtes de réponse de sécurité HTTP avec AD FS
description: Ce document descirbes comment personnaliser les en-têtes de sécurité pour vous protéger contre les failles de sécurité.
author: billmath
ms.author: billmath
manager: daveba
ms.reviewer: akgoel23
ms.date: 02/19/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4fd1e62e67f66a217a1d4f3a26933723a4645a31
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70865564"
---
# <a name="customize-http-security-response-headers-with-ad-fs-2019"></a>Personnaliser les en-têtes de réponse de sécurité HTTP avec AD FS 2019 
 
Pour vous protéger contre les failles de sécurité courantes et fournir aux administrateurs la possibilité de tirer parti des dernières avancées dans les mécanismes de protection basés sur un navigateur, AD FS 2019 a ajouté la fonctionnalité de personnalisation des en-têtes de réponse de sécurité HTTP. envoyé par AD FS. Pour ce faire, vous devez introduire deux nouvelles applets de `Get-AdfsResponseHeaders` commande `Set-AdfsResponseHeaders`: et.  

>[!NOTE]
>La fonctionnalité de personnalisation des en-têtes de réponse de sécurité http (à l’exception des en- `Get-AdfsResponseHeaders` têtes `Set-AdfsResponseHeaders` cors) à l’aide des applets de commande : et a été reportée à AD FS 2016. Vous pouvez ajouter la fonctionnalité à votre AD FS 2016 en installant [KB4493473](https://support.microsoft.com/en-us/help/4493473/windows-10-update-kb4493473) et [KB4507459](https://support.microsoft.com/en-us/help/4507459/windows-10-update-kb4507459). 

Dans ce document, nous aborderons les en-têtes de réponse de sécurité couramment utilisés pour montrer comment personnaliser les en-têtes envoyés par AD FS 2019.   
 
>[!NOTE]
>Le document suppose que AD FS 2019 a été installé.  

 
Avant de discuter des en-têtes, examinons quelques scénarios qui créent la nécessité pour les administrateurs de personnaliser les en-têtes de sécurité 
 
## <a name="scenarios"></a>Scénarios 
1. L’administrateur a activé [**http strict-transport-Security (HSTS)** ](#http-strict-transport-security-hsts) (force toutes les connexions sur le CHIFFREment https) pour protéger les utilisateurs qui peuvent accéder à l’application Web à l’aide de http à partir d’un point d’accès Wi-Fi public qui peut être piraté. Elle souhaite renforcer la sécurité en activant HSTS pour les sous-domaines.  
2. L’administrateur a configuré l’en-tête de réponse [**X-Frame-options**](#x-frame-options) (empêche le rendu d’une page Web dans un IFRAME) pour empêcher les pages Web d’être clickjacked. Toutefois, elle doit personnaliser la valeur d’en-tête en raison d’une nouvelle exigence commerciale pour afficher les données (dans un iFrame) à partir d’une application avec une origine différente (domaine).
3. L’administrateur a activé la [**protection X-XSS**](#x-xss-protection) (empêchant les attaques entre scripts) d’assainir et de bloquer la page si le navigateur détecte des attaques de script croisé. Toutefois, elle doit personnaliser l’en-tête pour permettre au chargement de la page d’être nettoyée.  
4. L’administrateur doit activer le [**partage des ressources Cross-Origin (cors)** ](#cross-origin-resource-sharing-cors-headers) et définir l’origine (domaine) sur AD FS pour autoriser une application à page unique à accéder à une API Web avec un autre domaine.  
5. L’administrateur a activé l’en-tête de [**stratégie de sécurité de contenu (CSP)** ](#content-security-policy-csp) pour empêcher les attaques par injection de données et de script entre sites en interdisant les demandes inter-domaines. Toutefois, en raison d’une nouvelle exigence commerciale, elle doit personnaliser l’en-tête pour permettre à la page Web de charger des images à partir de n’importe quelle origine et de restreindre le média aux fournisseurs approuvés.  

 
## <a name="http-security-response-headers"></a>En-têtes de réponse de sécurité HTTP 
Les en-têtes de réponse sont inclus dans la réponse HTTP sortante envoyée par AD FS à un navigateur Web. Les en-têtes peuvent être répertoriés `Get-AdfsResponseHeaders` à l’aide de l’applet de commande, comme indiqué ci-dessous.  

![Réponse d’en-tête](media/customize-http-security-headers-ad-fs/header1.png)

L' `ResponseHeaders` attribut dans la capture d’écran ci-dessus identifie les en-têtes de sécurité qui seront inclus par AD FS dans chaque réponse http. Les en-têtes de réponse sont envoyés uniquement `ResponseHeadersEnabled` si a `True` la valeur (valeur par défaut). La valeur peut être définie sur `False` pour empêcher AD FS y compris les en-têtes de sécurité dans la réponse http. Toutefois, cela n’est pas recommandé.  Pour ce faire, utilisez ce qui suit :

```PowerShell
Set-AdfsResponseHeaders -EnableResponseHeaders $false
```
 
### <a name="http-strict-transport-security-hsts"></a>HTTP strict-transport-Security (HSTS) 
HSTS est un mécanisme de stratégie de sécurité Web qui permet d’atténuer les attaques par rétrogradation de protocole et le piratage de cookies pour les services qui ont à la fois des points de terminaison HTTP et HTTPs. Il permet aux serveurs Web de déclarer que les navigateurs Web (ou d’autres agents utilisateur conformes) doivent uniquement interagir avec lui à l’aide de HTTPs et jamais via le protocole HTTP.  
 
Tous les points de terminaison de AD FS pour le trafic d’authentification Web sont ouverts exclusivement via HTTPs. Par conséquent, AD FS atténue efficacement les menaces fournies par le mécanisme de stratégie de sécurité de transport strict (par défaut, il n’y a pas de passage à HTTP, car il n’y a aucun écouteur dans HTTP). L’en-tête peut être personnalisé en définissant les paramètres suivants 
 
- **max-age =&lt;expiration-Time&gt;**  – le délai d’expiration (en secondes) spécifie la durée pendant laquelle le site ne doit être accessible qu’à l’aide du protocole HTTPS. La valeur par défaut et la valeur recommandée est de 31536000 secondes (1 an).  
- **includeSubDomains** : il s’agit d’un paramètre facultatif. S’il est spécifié, la règle HSTS s’applique également à tous les sous-domaines.  
 
#### <a name="hsts-customization"></a>Personnalisation de HSTS 
Par défaut, l’en-tête est `max-age` activé et défini sur 1 an ; Toutefois, les administrateurs `max-age` peuvent modifier la valeur (la diminution de l’âge maximal n’est pas recommandée) ou activer HSTS `Set-AdfsResponseHeaders` pour les sous-domaines via l’applet de commande.  
 
```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "Strict-Transport-Security" -SetHeaderValue "max-age=<seconds>; includeSubDomains" 
``` 

Exemple : 

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "Strict-Transport-Security" -SetHeaderValue "max-age=31536000; includeSubDomains" 
 ```

Par défaut, l’en-tête est inclus `ResponseHeaders` dans l’attribut ; Toutefois, les administrateurs peuvent supprimer l' `Set-AdfsResponseHeaders` en-tête par le biais de l’applet de commande.  
 
```PowerShell
Set-AdfsResponseHeaders -RemoveHeaders "Strict-Transport-Security" 
```

### <a name="x-frame-options"></a>X-Frame-options 
AD FS par défaut n’autorise pas les applications externes à utiliser des iFrames lors de l’exécution de connexions interactives. Cela permet d’éviter certains styles d’attaques par hameçonnage. Notez que les connexions non interactives peuvent être effectuées via un iFrame en raison de la sécurité au niveau de la session antérieure qui a été établie.  
 
Toutefois, dans certains cas rares, vous pouvez faire confiance à une application spécifique qui nécessite une page de connexion AD FS interactive prenant en charge iFrame. L’en-tête « X-Frame-options » est utilisé à cet effet.  
 
Cet en-tête de réponse de sécurité http est utilisé pour communiquer avec le navigateur, qu’il puisse &lt;afficher&gt;une&gt;page dans un IFRAME de cadre/&lt;. L’en-tête peut être défini sur l’une des valeurs suivantes : 
 
- **Deny** : la page dans un frame ne s’affiche pas. Il s’agit de la valeur par défaut et recommandée.  
- **sameorigin** : la page s’affiche uniquement dans le frame si l’origine est identique à l’origine de la page Web. L’option n’est pas très utile, sauf si tous les ancêtres sont également dans la même origine.  
- **autoriser : à <specified origin> partir** de la page s’affiche uniquement dans le frame si l’origine (par exemple https://www,.». com) correspond à l’origine spécifique dans l’en-tête. 

#### <a name="x-frame-options-customization"></a>Personnalisation des options X-Frame  
Par défaut, l’en-tête est défini sur Deny ; Toutefois, les administrateurs peuvent modifier la valeur par `Set-AdfsResponseHeaders` le biais de l’applet de commande.  
```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "X-Frame-Options" -SetHeaderValue "<deny/sameorigin/allow-from<specified origin>>" 
 ```

Exemple : 

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "X-Frame-Options" -SetHeaderValue "allow-from https://www.example.com" 
 ```

Par défaut, l’en-tête est inclus `ResponseHeaders` dans l’attribut ; Toutefois, les administrateurs peuvent supprimer l' `Set-AdfsResponseHeaders` en-tête par le biais de l’applet de commande.  

```PowerShell
Set-AdfsResponseHeaders -RemoveHeaders "X-Frame-Options" 
```

### <a name="x-xss-protection"></a>X-XSS-protection 
Cet en-tête de réponse de sécurité HTTP est utilisé pour empêcher le chargement des pages Web lorsque des attaques de script entre sites (XSS) sont détectées par les navigateurs. C’est ce que l’on appelle le filtrage XSS. L’en-tête peut être défini sur l’une des valeurs suivantes. 
 
- **0** – désactive le filtrage XSS. Non recommandé.  
- **1** – active le filtrage XSS. Si une attaque XSS est détectée, le navigateur nettoie la page.   
- **1 ; mode = bloc** : active le filtrage XSS. Si une attaque XSS est détectée, le navigateur empêchera le rendu de la page. Il s’agit de la valeur par défaut et recommandée.  

#### <a name="x-xss-protection-customization"></a>Personnalisation X-XSS-protection 
Par défaut, l’en-tête est défini sur 1 ; mode = bloc ; Toutefois, les administrateurs peuvent modifier la valeur par `Set-AdfsResponseHeaders` le biais de l’applet de commande.  

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "X-XSS-Protection" -SetHeaderValue "<0/1/1; mode=block/1; report=<reporting-uri>>" 
``` 

Exemple : 

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "X-XSS-Protection" -SetHeaderValue "1" 
 ```

Par défaut, l’en-tête est inclus `ResponseHeaders` dans l’attribut ; Toutefois, les administrateurs peuvent supprimer l' `Set-AdfsResponseHeaders` en-tête par le biais de l’applet de commande. 

```PowerShell
Set-AdfsResponseHeaders -RemoveHeaders "X-XSS-Protection" 
```

### <a name="cross-origin-resource-sharing-cors-headers"></a>En-têtes de partage des ressources Cross-Origin (CORS) 
La sécurité du navigateur Web empêche une page Web de créer des requêtes Cross-Origin lancées à partir de scripts. Toutefois, il peut arriver que vous souhaitiez accéder à des ressources dans d’autres origines (domaines). CORS est une norme W3C qui permet à un serveur d’assouplir la stratégie de même origine. À l’aide de CORS, un serveur peut autoriser explicitement certaines demandes cross-origin lors du refus d’autres.  
 
Pour mieux comprendre la demande CORS, voyons un scénario dans lequel une application à page unique (SPA) doit appeler une API Web avec un domaine différent. En outre, considérons que SPA et l’API sont configurés sur ADFS 2019 AD FS et que CORS est activé, c.-à-d. AD FS pouvez identifier les en-têtes CORS dans la requête HTTP, valider les valeurs d’en-tête et inclure les en-têtes CORS appropriés dans la réponse (détails sur l’activation et la configurer CORS sur AD FS 2019 dans la section de personnalisation CORS ci-dessous). Exemple de flow : 

1. L’utilisateur accède au SPA par le biais du navigateur client et est redirigé vers AD FS point de terminaison d’authentification pour l’authentification. Étant donné que SPA est configuré pour le workflow d’octroi implicite, la requête retourne un jeton d’accès + ID au navigateur après une authentification réussie.  
2. Après l’authentification de l’utilisateur, le code JavaScript frontal inclus dans le SPA fait une demande d’accès à l’API Web. La demande est redirigée vers AD FS avec les en-têtes suivants
    - Options : décrit les options de communication pour la ressource cible. 
    - Origin : comprend l’origine de l’API Web.
    - Access-Control-Request-Method : identifie la méthode HTTP (par exemple, DELETE) à utiliser lorsque la demande réelle est effectuée. 
    - Access-Control-request-headers-identifie les en-têtes HTTP à utiliser lorsque la demande réelle est effectuée 
    
   >[!NOTE]
   >La demande CORS ressemble à une requête HTTP standard. Toutefois, la présence d’un en-tête Origin indique que la demande entrante est liée à CORS. 
3. AD FS vérifie que l’origine de l’API Web incluse dans l’en-tête est répertoriée dans les origines approuvées configurées dans AD FS (détails sur la modification des origines approuvées dans la section de personnalisation CORS ci-dessous). AD FS répond ensuite avec les en-têtes suivants.  
    - Access-Control-allow-Origin – valeur identique à celle de l’en-tête Origin 
    - Access-Control-allow-Method – valeur identique à celle de l’en-tête Access-Control-Request-Method 
    - Access-Control-allow-en-têtes-valeur identique à celle de l’en-tête Access-Control-request-headers 
4. Le navigateur envoie la demande réelle, y compris les en-têtes suivants 
    - Méthode HTTP (par exemple, DELETE) 
    - Origin : comprend l’origine de l’API Web. 
    - Tous les en-têtes inclus dans l’en-tête de réponse Access-Control-allow-headers 
5. Une fois la vérification effectuée, AD FS approuve la requête en incluant le domaine de l’API Web (Origin) dans l’en-tête de réponse Access-Control-allow-Origin.  
6. L’inclusion de l’en-tête Access-Control-allow-Origin permettra au navigateur d’accéder à l’API demandée.

#### <a name="cors-customization"></a>Personnalisation CORS 
Par défaut, les fonctionnalités CORS ne sont pas activées ; Toutefois, les administrateurs peuvent activer la fonctionnalité par le biais de l’applet de commande Set-AdfsResponseHeaders.  

```PowerShell 
Set-AdfsResponseHeaders -EnableCORS $true 
 ```

Une fois activée, les administrateurs peuvent énumérer une liste d’origines approuvées à l’aide de la même applet de commande. Par exemple, la commande suivante autorise les requêtes CORS à partir des origines **https&#58;//example1.com** et **https&#58;//example1.com**. 
 
```PowerShell
Set-AdfsResponseHeaders -CORSTrustedOrigins https://example1.com,https://example2.com 
 ```

> [!NOTE]
> Les administrateurs peuvent autoriser les requêtes CORS à partir de n’importe quelle origine en incluant « * » dans la liste des origines approuvées, bien que cette approche ne soit pas recommandée en raison des failles de sécurité et qu’un message d’avertissement est fourni s’ils choisissent de le faire. 

### <a name="content-security-policy-csp"></a>Stratégie de sécurité de contenu (CSP) 
Cet en-tête de réponse de sécurité HTTP est utilisé pour empêcher les attaques de script entre sites, le détournement et autres attaques par injection de données en empêchant les navigateurs d’exécuter par inadvertance du contenu malveillant. Les navigateurs qui ne prennent pas en charge CSP ignorent simplement les en-têtes de réponse CSP.  
 
#### <a name="csp-customization"></a>Personnalisation du CSP 
La personnalisation de l’en-tête CSP implique la modification de la stratégie de sécurité qui définit que le navigateur des ressources est autorisé à charger pour la page Web. La stratégie de sécurité par défaut est  
 
`Content-Security-Policy: default-src ‘self' ‘unsafe-inline' ‘'unsafe-eval'; img-src ‘self' data:;` 
 
La directive **par défaut de SRC** est utilisée pour modifier [des directives-SRC](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/default-src) sans répertorier chaque directive explicitement. Par exemple, dans l’exemple ci-dessous, la stratégie 1 est la même que la stratégie 2.  

Stratégie 1 
```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "Content-Security-Policy" -SetHeaderValue "default-src 'self'" 
```
 
Stratégie 2
```PowerShell 
Set-AdfsResponseHeaders -SetHeaderName "Content-Security-Policy" -SetHeaderValue "script-src ‘self'; img-src ‘self'; font-src 'self';  
frame-src 'self'; manifest-src 'self'; media-src 'self';" 
```

Si une directive est explicitement listée, la valeur spécifiée remplace la valeur donnée pour default-SRC. Dans l’exemple ci-dessous, img-src prend la valeur « * » (autorisant le chargement des images à partir de n’importe quelle origine), tandis que les autres directives-SRC prennent la valeur comme « Self » (ce qui limite à la même origine que la page Web).  

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "Content-Security-Policy" -SetHeaderValue "default-src ‘self'; img-src *" 
```
Les sources suivantes peuvent être définies pour la stratégie par défaut de SRC 
 
- 'Self' : spécifier cela restreint l’origine du contenu à charger à l’origine de la page Web 
- 'unsafe-inline' – la spécification de ce paramètre dans la stratégie permet l’utilisation de JavaScript inline et de CSS 
- 'unsafe-eval' – la spécification de ce paramètre dans la stratégie permet l’utilisation de mécanismes de texte à JavaScript comme eval 
- 'none' – la spécification de ce paramètre limite le contenu d’une origine à une charge 
- Data :-spécification des données : Les URI permettent aux créateurs de contenu d’incorporer des fichiers de petite taille dans des documents. Utilisation non recommandée.  
 
>[!NOTE]
>AD FS utilise JavaScript dans le processus d’authentification et, par conséquent, active JavaScript en incluant des sources « unsafe-inline » et « unsafe-eval » dans la stratégie par défaut.  

### <a name="custom-headers"></a>En-têtes personnalisés 
En plus des en-têtes de réponse de sécurité indiqués ci-dessus (HSTS, CSP, X-Frame-options, X-XSS-protection et CORS), AD FS 2019 permet de définir de nouveaux en-têtes.  
 
Exemple : Pour définir un nouvel en-tête « TestHeader » avec la valeur « TestHeaderValue » 

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "TestHeader" -SetHeaderValue "TestHeaderValue" 
 ```

Une fois défini, le nouvel en-tête est envoyé dans la réponse AD FS (extrait de code Fiddler ci-dessous).  
 
![Fiddler](media/customize-http-security-headers-ad-fs/header2.png)

## <a name="web-browswer-compatibility"></a>Compatibilité Web navigateur attribuant alors
Utilisez le tableau et les liens suivants pour déterminer les navigateurs Web compatibles avec chacun des en-têtes de réponse de sécurité.

|En-têtes de réponse de sécurité HTTP|Compatibilité du navigateur|
|-----|-----|
|HTTP strict-transport-Security (HSTS)|[Compatibilité du navigateur HSTS](https://developer.mozilla.org/docs/Web/HTTP/Headers/Strict-Transport-Security#Browser_compatibility)|
|X-Frame-options|[Compatibilité du navigateur X-Frame-options](https://developer.mozilla.org/docs/Web/HTTP/Headers/X-Frame-Options#Browser_compatibility)| 
|X-XSS-protection|[Compatibilité du navigateur X-XSS-protection](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-XSS-Protection#Browser_compatibility)| 
|Partage des ressources Cross-Origin (CORS)|[Compatibilité des navigateurs CORS](https://developer.mozilla.org/docs/Web/HTTP/CORS#Browser_compatibility) 
|Stratégie de sécurité de contenu (CSP)|[Compatibilité du navigateur CSP](https://developer.mozilla.org/docs/Web/HTTP/CSP#Browser_compatibility) 

## <a name="next"></a>Suivant

- [Utiliser AD FS guides troublehshooting Help](https://aka.ms/adfshelp/troubleshooting )
- [Résolution des problèmes AD FS](../../ad-fs/troubleshooting/ad-fs-tshoot-overview.md)
