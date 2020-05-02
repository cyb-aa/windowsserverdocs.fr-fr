---
title: wecutil
description: Rubrique de référence pour wecutil, qui vous permet de créer et de gérer des abonnements aux événements qui sont transférés à partir d’ordinateurs distants.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0c82a6cb-d652-429c-9c3d-0f568c78d54b
author: coreyp-at-msft
ms.author: coreyp
manager: dansimps
ms.openlocfilehash: 6c62d3ce24f539b4176acd6193cbd956ecbf5471
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725859"
---
# <a name="wecutil"></a>wecutil



Vous permet de créer et de gérer des abonnements aux événements qui sont transférés à partir d’ordinateurs distants. L’ordinateur distant doit prendre en charge le protocole WS-Management. 


## <a name="syntax"></a>Syntaxe

```
wecutil  [{es | enum-subscription}] 
[{gs | get-subscription} <Subid> [/f:<Format>] [/uni:<Unicode>]] 
[{gr | get-subscriptionruntimestatus} <Subid> [<Eventsource> …]] 
[{ss | set-subscription} [<Subid> [/e:[<Subenabled>]] [/esa:<Address>] [/ese:[<Srcenabled>]] [/aes] [/res] [/un:<Username>] [/up:<Password>] [/d:<Desc>] [/uri:<Uri>] [/cm:<Configmode>] [/ex:<Expires>] [/q:<Query>] [/dia:<Dialect>] [/tn:<Transportname>] [/tp:<Transportport>] [/dm:<Deliverymode>] [/dmi:<Deliverymax>] [/dmlt:<Deliverytime>] [/hi:<Heartbeat>] [/cf:<Content>] [/l:<Locale>] [/ree:[<Readexist>]] [/lf:<Logfile>] [/pn:<Publishername>] [/essp:<Enableport>] [/hn:<Hostname>] [/ct:<Type>]] [/c:<Configfile> [/cun:<Username> /cup:<Password>]]] 
[{cs | create-subscription} <Configfile> [/cun:<Username> /cup:<Password>]] 
[{ds | delete-subscription} <Subid>] 
[{rs | retry-subscription} <Subid> [<Eventsource>…]] 
[{qc | quick-config} [/q:[<Quiet>]]].
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|{es \| enum-abonnement}|Affiche les noms de tous les abonnements aux événements distants qui existent.|
|{GS \| obten-subscription} \<Subid> [/F :\<format>] [/Uni :\<Unicode>]|Affiche les informations de configuration de l’abonnement à distance. \<Subid> est une chaîne qui identifie de façon unique un abonnement. \<Subid> est identique à la chaîne spécifiée dans la \<balise SubscriptionId> du fichier de configuration XML, utilisé pour créer l’abonnement.|
|{GR \| -subscriptionruntimestatus} \<Subid> [\<EventSource>...]|Affiche l’état d’exécution d’un abonnement. \<Subid> est une chaîne qui identifie de façon unique un abonnement. \<Subid> est identique à la chaîne spécifiée dans la \<balise SubscriptionId> du fichier de configuration XML, utilisé pour créer l’abonnement. \<EventSource> est une chaîne qui identifie un ordinateur qui sert de source d’événements. \<La> EventSource doit être un nom de domaine complet, un nom NetBIOS ou une adresse IP.|
|{SS \| Set-subscription} \<Subid> [/e : [\<sous-activée>]] [/ESA :\<adresse>] [/ESE : [\<Srcenabled>]] [/AES] [/res] [/un :\<username>] [/up :\<Password>] [/d :\<DESC>] [/URI :\<URI>] [/cm :\<Configmode>] [/ex :\<Expires>] [/Q\<: Query>] [/dia\<: Dialect>] [/TN\<: Transportname>] [/TP\<: Transportport>] [/DM\<: DeliveryMode>] [/DMI\<: Deliverymax>] [/DMLT\<: deliveryTime>] [/HI\<: Heartbeat>] [/CF\<: content>] [/l\<: locale>] [/REE : [\<Readexist>]] [/LF :\<logfile>] [/PN :\<PublisherName>] [/ESSP :\<Enableport>] [/HN :\<hostname>] [/CT :\<type>]</br>or</br>{SS \| Set-subscription\</c : ConfigFile> [\</cun : comnom_utilisateur>\</Cup : compassword>]|Modifie la configuration de l’abonnement. Vous pouvez spécifier l’ID d’abonnement et les options appropriées pour modifier les paramètres d’abonnement, ou vous pouvez spécifier un fichier de configuration XML pour modifier les paramètres d’abonnement.|
|{CS \| Create-Subscription} \<ConfigFile> [/cun :\<username>/Cup :\<Password>]|Crée un abonnement à distance. \<ConfigFile> spécifie le chemin d’accès au fichier XML qui contient la configuration de l’abonnement. Le chemin d’accès peut être absolu ou relatif au répertoire actif.|
|{DS \| Delete-subscription} \<Subid>|Supprime un abonnement et annule l’abonnement de toutes les sources d’événements qui délivrent des événements dans le journal des événements pour l’abonnement. Les événements déjà reçus et journalisés ne sont pas supprimés. \<Subid> est une chaîne qui identifie de façon unique un abonnement. \<Subid> est identique à la chaîne spécifiée dans la \<balise SubscriptionId> du fichier de configuration XML, utilisé pour créer l’abonnement.|
|{RS \| Retry-subscription} \<Subid> [\<EventSource>...]|Effectue une nouvelle tentative pour établir une connexion et envoyer une demande d’abonnement à distance à un abonnement inactif. Tente de réactiver toutes les sources d’événements ou les sources d’événement spécifiées. Les sources désactivées ne sont pas retentées. \<Subid> est une chaîne qui identifie de façon unique un abonnement. \<Subid> est identique à la chaîne spécifiée dans la \<balise SubscriptionId> du fichier de configuration XML, utilisé pour créer l’abonnement. \<EventSource> est une chaîne qui identifie un ordinateur qui sert de source d’événements. \<La> EventSource doit être un nom de domaine complet, un nom NetBIOS ou une adresse IP.|
|{configuration \| rapide QC} [/q : [\<> quiet]]|Configure le service collecteur d’événements Windows pour s’assurer qu’un abonnement peut être créé et soutenu par le biais de redémarrages. Cela comprend les étapes suivantes :</br>1. activez le canal ForwardedEvents s’il est désactivé.</br>2. Définissez le service collecteur d’événements Windows pour qu’il retarde le démarrage.</br>3. Démarrez le service collecteur d’événements Windows s’il n’est pas en cours d’exécution.|

## <a name="options"></a>Options

|Option|Description|
|------|-----------|
|/f :\<format>|Spécifie le format des informations affichées. \<Les> de format peuvent être XML ou succinctes. Si <Format> est du code XML, la sortie est affichée au format XML. Si \<le> de format est succinct, la sortie est affichée dans les paires nom-valeur. La valeur par défaut est laconique.|
|/c :\<ConfigFile>|Spécifie le chemin d’accès au fichier XML qui contient une configuration d’abonnement. Le chemin d’accès peut être absolu ou relatif au répertoire actif. Cette option ne peut être utilisée qu’avec les options **/cun** et **/Cup** et s’exclut mutuellement avec toutes les autres options.|
|/e : [\<> sous-activé]|Active ou désactive un abonnement. \<Les> sous-activés peuvent avoir la valeur true ou false. La valeur par défaut de cette option est true.|
|/ESA :\<adresse>|Spécifie l’adresse d’une source d’événement. \<Adresse> est une chaîne qui contient un nom de domaine complet, un nom NetBIOS ou une adresse IP, qui identifie un ordinateur qui sert de source d’événements. Cette option doit être utilisée avec les options **/ESE**, **/AES**, **/res**, ou **/un** et **/up** .|
|/ESE : [\<Srcenabled>]|Active ou désactive une source d’événement. \<Srcenabled> peut avoir la valeur true ou false. Cette option est autorisée uniquement si l’option **/ESA** est spécifiée. La valeur par défaut de cette option est true.|
|/aes|Ajoute la source d’événements spécifiée par l’option **/ESA** si elle ne fait pas déjà partie de l’abonnement. Si l’adresse spécifiée par l’option **/ESA** fait déjà partie de l’abonnement, une erreur est signalée. Cette option est autorisée uniquement si l’option **/ESA** est spécifiée.|
|/res|Supprime la source d’événements spécifiée par l’option **/ESA** si elle fait déjà partie de l’abonnement. Si l’adresse spécifiée par l’option **/ESA** ne fait pas partie de l’abonnement, une erreur est signalée. Cette option est autorisée uniquement si l’option **/ESA** est spécifiée.|
|/un :\<nom d’utilisateur>|Spécifie les informations d’identification de l’utilisateur à utiliser avec la source d’événements spécifiée par l’option **/ESA** . Cette option est autorisée uniquement si l’option **/ESA** est spécifiée.|
|/Up :\<mot de passe>|Spécifie le mot de passe qui correspond aux informations d’identification de l’utilisateur. Cette option est autorisée uniquement si l’option **/un** est spécifiée.|
|/d :\<DESC>|Fournit une description de l’abonnement.|
|/URI :\<URI>|Spécifie le type des événements consommés par l’abonnement. \<URI> contient une chaîne d’URI associée à l’adresse de l’ordinateur source de l’événement pour identifier de manière unique la source des événements. La chaîne d’URI est utilisée pour toutes les adresses de la source d’événements dans l’abonnement.|
|/cm :\<Configmode>|Définit le mode de configuration. \<Configmode> peut être l’une des chaînes suivantes : normal, Custom, MinLatency ou MinBandwidth. Les modes normal, MinLatency et MinBandwidth définissent le mode de remise, le nombre maximal d’éléments de remise, l’intervalle de pulsation et le temps de latence maximal de remise. Les options **/DM**, **/DMI**, **/Hi** ou **/DMLT** peuvent être spécifiées uniquement si le mode de configuration est défini sur personnalisé.|
|/ex :\<expire>|Définit l’heure d’expiration de l’abonnement. \<Expire> doit être défini au format de date et d’heure standard XML ou ISO8601 : YYYY-MM-ddThh : mm : SS [. sss] [Z], où T est le séparateur d’heure et Z indique l’heure UTC.|
|/q :\<> de requête|Spécifie la chaîne de requête pour l’abonnement. Le format des \<> de requête peut être différent pour différentes valeurs d’URI et s’applique à toutes les sources de l’abonnement.|
|/Dia :\<dialecte>|Définit le dialecte utilisé par la chaîne de requête.|
|/TN :\<Transportname>|Spécifie le nom du transport utilisé pour se connecter à une source d’événements distante.|
|/TP :\<Transportport>|Définit le numéro de port utilisé par le transport lors de la connexion à une source d’événements distante.|
|/DM :\<DeliveryMode>|Spécifie le mode de remise. \<DeliveryMode> peut avoir la valeur pull ou Push. Cette option est valide uniquement si l’option **/cm** est définie sur personnalisé.|
|/DMI :\<Deliverymax>|Définit le nombre maximal d’éléments pour la remise par lot. Cette option est valide uniquement si **/cm** est défini sur Custom.|
|/DMLT :\<deliveryTime>|Définit la latence maximale pour la transmission d’un lot d’événements. \<Deliverytime> est le nombre de millisecondes. Cette option est valide uniquement si **/cm** est défini sur Custom.|
|/HI :\<pulsation>|Définit l’intervalle de pulsation. \<Le> de pulsation est le nombre de millisecondes. Cette option est valide uniquement si **/cm** est défini sur Custom.|
|/CF :\<> de contenu|Spécifie le format des événements retournés. \<Les> de contenu peuvent être des événements ou RenderedText. Lorsque la valeur est RenderedText, les événements sont retournés avec les chaînes localisées (telles que la description de l’événement) jointes à l’événement. La valeur par défaut est RenderedText.|
|/l :\<paramètres régionaux>|Spécifie les paramètres régionaux pour la remise des chaînes localisées au format RenderedText. \<Paramètres régionaux> est un identificateur de langue et de pays/région, par exemple, en-US. Cette option est valide uniquement si l’option **/CF** est définie sur RenderedText.|
|/REE : [\<Readexist>]|Identifie les événements remis pour l’abonnement. \<Readexist> peut avoir la valeur true ou false. Lorsque la <Readexist> propriété a la valeur true, tous les événements existants sont lus à partir des sources d’événements de l’abonnement. Lorsque la <Readexist> valeur de est false, seuls les événements futurs (arrivant) sont remis. La valeur par défaut est true pour une option **/REE** sans valeur. Si aucune option **/REE** n’est spécifiée, la valeur par défaut est false.|
|/LF :\<logfile>|Spécifie le journal des événements local utilisé pour stocker les événements reçus des sources d’événements.|
|/PN :\<PublisherName>|Spécifie le nom de l’éditeur. Il doit s’agir d’un serveur de publication qui possède ou importe le journal spécifié par l’option **/LF** .|
|/ESSP :\<Enableport>|Spécifie que le numéro de port doit être ajouté au nom de principal du service du service distant. \<Enableport> peut avoir la valeur true ou false. Le numéro de port est ajouté lorsque <Enableport> la propriété a la valeur true. Lorsque le numéro de port est ajouté, une configuration peut être nécessaire pour empêcher le refus de l’accès aux sources d’événements.|
|/HN :\<nom d’hôte>|Spécifie le nom DNS de l’ordinateur local. Ce nom est utilisé par la source d’événements à distance pour envoyer des événements et doit être utilisé uniquement pour un abonnement par émission de type push.|
|/CT :\<type>|Définit le type d’informations d’identification pour l’accès à la source distante. \<Le type> doit être l’une des valeurs suivantes : default, Negotiate, Digest, Basic ou LocalMachine. La valeur par défaut est default.|
|/CUN :\<comnom_utilisateur>|Définit les informations d’identification de l’utilisateur partagé à utiliser pour les sources d’événements qui n’ont pas leurs propres informations d’identification de l’utilisateur. Si cette option est spécifiée avec l’option **/c** , les paramètres username et UserPassword des sources d’événements individuels du fichier de configuration sont ignorés. Si vous souhaitez utiliser des informations d’identification différentes pour une source d’événement spécifique, vous devez remplacer cette valeur en spécifiant les options **/un** et **/up** pour une source d’événement spécifique sur la ligne de commande d’une autre commande **SS** .|
|/Cup :\<compassword>|Définit le mot de passe de l’utilisateur pour les informations d’identification de l’utilisateur partagé. Lorsque \<compassword> a la valeur * (astérisque), le mot de passe est lu à partir de la console. Cette option est valide uniquement lorsque l’option **/cun** est spécifiée.|
|/q : [\<> quiet]|Spécifie si la procédure de configuration vous invite à confirmer l’opération. \<Le> quiet peut avoir la valeur true ou false. Si <Quiet> a la valeur true, la procédure de configuration ne demande pas de confirmation. La valeur par défaut de cette option est false.|

## <a name="remarks"></a>Notes 

> [!IMPORTANT]
> Si vous recevez le message «le serveur RPC n’est pas disponible ? Lorsque vous essayez d’exécuter wecutil, vous devez démarrer le service collecteur d’événements Windows (wecsvc). Pour démarrer wecsvc, à partir d’une invite de commandes avec élévation de privilèges, tapez net start wecsvc.

- Pour afficher le contenu d’un fichier de configuration :  
  ```
  <Subscription xmlns=https://schemas.microsoft.com/2006/03/windows/events/subscription>
  <Uri>https://schemas.microsoft.com/wbem/wsman/1/windows/EventLog</Uri>
  <!-- Use Normal (default), Custom, MinLatency, MinBandwidth -->
  <ConfigurationMode>Normal</ConfigurationMode>
  <Description>Forward Sample Subscription</Description>
  <SubscriptionId>SampleSubscription</SubscriptionId>
  <Query><![CDATA[
  <QueryList>
  <Query Path=Application>
  <Select>*</Select>
  </Query>
  </QueryList>
  ]]></Query>
  <EventSources>
  <EventSource Enabled=true>
  <Address>mySource.myDomain.com</Address>
  <UserName>myUserName</UserName>
  <Password>*</Password>
  </EventSource>
  </EventSources>
  <CredentialsType>Default</CredentialsType>
  <Locale Language=EN-US></Locale>
  </Subscription>
  ```

## <a name="examples"></a>Exemples

Informations de configuration de sortie pour un abonnement nommé Sub1 :
```
wecutil gs sub1
```
Exemple de sortie :
```
EventSource[0]:
Address: localhost
Enabled: true
Description: Subscription 1
Uri: wsman:microsoft/logrecord/sel
DeliveryMode: pull
DeliveryMaxSize: 16000
DeliveryMaxItems: 15
DeliveryMaxLatencyTime: 1000
HeartbeatInterval: 10000
Locale:
ContentFormat: renderedtext
LogFile: HardwareEvents
```
Affichez l’état d’exécution d’un abonnement nommé Sub1 :
```
wecutil gr sub1
```
Mettez à jour la configuration de l’abonnement nommée Sub1 à partir d’un nouveau fichier XML appelé WsSelRg2. xml :
```
wecutil ss sub1 /c:%Windir%\system32\WsSelRg2.xml
```
Mettez à jour la configuration d’abonnement nommée Sub2 avec plusieurs paramètres :
```
wecutil ss sub2 /esa:myComputer /ese /un:uname /up:* /cm:Normal
```
Créez un abonnement pour transférer les événements d’un journal des événements des applications Windows Vista d’un ordinateur distant sur mySource.myDomain.com au Journal ForwardedEvents (pour obtenir un exemple de fichier de configuration, consultez la section Notes) :
```
wecutil cs subscription.xml
```
Supprimez un abonnement nommé Sub1 :
```
wecutil ds sub1
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
