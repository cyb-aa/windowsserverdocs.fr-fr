---
title: wecutil
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0c82a6cb-d652-429c-9c3d-0f568c78d54b
author: coreyp-at-msft
ms.author: coreyp
manager: dansimps
ms.openlocfilehash: 04fb86d813049dc5f0aa6d4fba51e45dccbd1b80
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440181"
---
# <a name="wecutil"></a>wecutil



Vous permet de créer et gérer des abonnements aux événements qui sont transférés à partir d’ordinateurs distants. L’ordinateur distant doit prendre en charge le protocole WS-Management. Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).


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

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|{es \| enum-subscription}|Affiche les noms de tous les abonnements d’événements distant qui existent.|
|{gs \| get-subscription} \<Subid > [/ f:\<Format >] [/ uni :\<Unicode >]|Affiche des informations de configuration d’abonnement à distance. \<Subid > est une chaîne qui identifie de façon unique un abonnement. \<Subid > est le même que la chaîne a été spécifiée dans le \<SubscriptionId > balise du fichier de configuration XML qui a été utilisé pour créer l’abonnement.|
|{gr \| get-subscriptionruntimestatus} \<Subid > [\<Eventsource >...]|Affiche l’état d’exécution d’un abonnement. \<Subid > est une chaîne qui identifie de façon unique un abonnement. \<Subid > est le même que la chaîne a été spécifiée dans le \<SubscriptionId > balise du fichier de configuration XML qui a été utilisé pour créer l’abonnement. \<EventSource > est une chaîne qui identifie un ordinateur qui sert de source d’événements. \<EventSource > doit être un nom de domaine complet, un nom NetBIOS ou une adresse IP.|
|{ss \| set-subscription} \<Subid > [/ e: [\<Subenabled >]] [/ SEC :\<adresse >] [/ ese : [\<Srcenabled >]] [/aes] [/ res] [/ Annuler :\<nom d’utilisateur >] [/ jusqu'à :\< Mot de passe >] [/ d:\<Desc >] [/ uri :\<Uri >] [/ cm :\<Configmode >] [/ ex :\<Expires >] [/ q :\<requête >] [/ dia :\<dialecte >] [/ tn :\< TransportName >] [/ tp :\<Transportport >] [/ dm :\<Deliverymode >] [/ dmi :\<Deliverymax >] [/ dmlt :\<Deliverytime >] [/ Bonjour :\<pulsation >] [/ cf :\< Contenu >] [/ l:\<paramètres régionaux >] [/ ré : [\<Readexist >]] [/ lf :\<Logfile >] [/ pn :\<Publishername >] [/ essp :\<Enableport >] [/ hn :\< Nom d’hôte >] [/ ct :\<Type >]</br>ou</br>{ss \| /c: abonnements ensemble\<Configfile > [/ cun :\<Comusername >/GOBELET :\<Compassword >]|Modifie la configuration de l’abonnement. Vous pouvez spécifier l’ID d’abonnement et les options appropriées pour modifier les paramètres d’abonnement, ou vous pouvez spécifier un fichier de configuration XML pour modifier les paramètres d’abonnement.|
|{cs \| -créer un abonnement} \<Configfile > [/ cun :\<nom d’utilisateur >/GOBELET :\<mot de passe >]|Crée un abonnement à distance. \<ConfigFile > Spécifie le chemin d’accès au fichier XML qui contient la configuration de l’abonnement. Le chemin d’accès peut être absolu ou relatif au répertoire actif.|
|{ds \| supprimer-abonnement} \<Subid >|Supprime un abonnement et annule l’abonnement à partir de toutes les sources d’événements qui remettent des événements dans le journal des événements pour l’abonnement. Tous les événements déjà reçus et connecté ne sont pas supprimés. \<Subid > est une chaîne qui identifie de façon unique un abonnement. \<Subid > est le même que la chaîne a été spécifiée dans le \<SubscriptionId > balise du fichier de configuration XML qui a été utilisé pour créer l’abonnement.|
|{rs \| nouvelle tentative-subscription} \<Subid > [\<Eventsource >...]|Effectue une nouvelle tentative pour établir une connexion et envoyer une demande d’abonnement à distance à un abonnement inactif. Tente de se réactiver toutes les sources d’événements ou sources d’événement spécifiées. Sources désactivées ne sont pas retentées. \<Subid > est une chaîne qui identifie de façon unique un abonnement. \<Subid > est le même que la chaîne a été spécifiée dans le \<SubscriptionId > balise du fichier de configuration XML qui a été utilisé pour créer l’abonnement. \<EventSource > est une chaîne qui identifie un ordinateur qui sert de source d’événements. \<EventSource > doit être un nom de domaine complet, un nom NetBIOS ou une adresse IP.|
|{qc \| rapide-config} [/ q : [\<silencieux >]]|Configure le service collecteur d’événements Windows pour garantir un abonnement peut être créé et maintenu à travers les redémarrages. Cela comprend les étapes suivantes :</br>1.  Activer le canal ÉvénementsTransférés si elle est désactivée.</br>2.  Définissez le service collecteur d’événements Windows pour retarder le début.</br>3.  Démarrer le service collecteur d’événements Windows s’il n’est pas exécuté.|

## <a name="options"></a>Options

|Option|Description|
|------|-----------|
|/f :\<format >|Spécifie le format des informations qui s’affiche. \<Format > peut être XML ou « Terse ». Si <Format> est XML, la sortie s’affiche au format XML. Si \<Format > est « Terse », la sortie s’affiche dans les paires nom-valeur. La valeur par défaut est « Terse ».|
|/ c:\<Configfile >|Spécifie le chemin d’accès au fichier XML qui contient une configuration de l’abonnement. Le chemin d’accès peut être absolu ou relatif au répertoire actif. Cette option peut uniquement être utilisée avec le **/cun** et **/GOBELET** options et s’exclut mutuellement avec toutes les autres options.|
|/ e: [\<Subenabled >]|Active ou désactive un abonnement. \<Subenabled > peut être true ou false. La valeur par défaut de cette option a la valeur true.|
|/ESA :\<adresse >|Spécifie l’adresse d’une source d’événement. \<Adresse > est une chaîne qui contient un nom de domaine complet, un nom NetBIOS ou une adresse IP, qui identifie un ordinateur qui sert de source d’événements. Cette option doit être utilisée avec le **/ese**, **/aes**, **/res**, ou **/un** et **/up** options.|
|/ESE : [\<Srcenabled >]|Active ou désactive une source d’événement. \<Srcenabled > peut être true ou false. Cette option est autorisée uniquement si le **/esa** option est spécifiée. La valeur par défaut de cette option a la valeur true.|
|/AES|Ajoute la source d’événements spécifié par le **/esa** option si elle n’est pas déjà une partie de l’abonnement. Si l’adresse spécifiée par le **/esa** option fait déjà partie de l’abonnement, une erreur est signalée. Cette option n’est autorisée que si le **/esa** option est spécifiée.|
|/res|Supprime la source d’événements spécifié par le **/esa** option s’il fait déjà partie de l’abonnement. Si l’adresse spécifiée par le **/esa** option n’est pas une partie de l’abonnement, une erreur est signalée. Cette option est uniquement autorisée si **/esa** option est spécifiée.|
|/un :\<nom d’utilisateur >|Spécifie les informations d’identification de l’utilisateur à utiliser avec la source d’événements spécifiée par le **/esa** option. Cette option n’est autorisée que si le **/esa** option est spécifiée.|
|et le haut :\<mot de passe >|Spécifie le mot de passe qui correspond à l’information d’identification de l’utilisateur. Cette option n’est autorisée que si le **/un** option est spécifiée.|
|/d:\<Desc>|Fournit une description pour l’abonnement.|
|/URI :\<Uri >|Spécifie le type des événements qui sont consommés par l’abonnement. \<URI > contient une chaîne d’URI qui est associée à l’adresse de l’ordinateur de source d’événement pour identifier la source des événements. La chaîne d’URI est utilisée pour toutes les adresses de source d’événement dans l’abonnement.|
|/cm :\<Configmode >|Définit le mode de configuration. \<Configmode > peut prendre l’une des chaînes suivantes : Normal, personnalisé, MinLatency ou MinBandwidth. Les modes Normal, MinLatency et MinBandwidth définir le mode de remise, éléments max de remise, intervalle de pulsation et les temps de latence maximale de remise. Le **/dm**, **/dmi**, **/hi** ou **/dmlt** options peuvent uniquement être spécifiés si le mode de configuration est défini sur personnalisé.|
|/ ex :\<expire >|Définit l’heure d’expiration de l’abonnement. \<Expire > doit être défini dans le format de date et d’heure standard XML ou ISO8601 : AAAA-MM-JJThh [.sss] [Z], où T est le séparateur d’heure et Z indique l’heure UTC.|
|/q :\<requête >|Spécifie la chaîne de requête pour l’abonnement. Le format de \<requête > peut être différente pour différentes valeurs d’URI et s’applique à toutes les sources dans l’abonnement.|
|/DIA :\<dialecte >|Définit le dialecte qui utilise la chaîne de requête.|
|/TN :\<Transportname >|Spécifie le nom du transport qui est utilisé pour se connecter à une source d’événements à distance.|
|/TP :\<Transportport >|Définit le numéro de port qui est utilisé par le transport lors de la connexion à une source d’événements à distance.|
|/DM :\<Deliverymode >|Spécifie le mode de remise. \<DeliveryMode > peut être push ou pull. Cette option est valide uniquement si le **/cm** option est définie sur personnalisé.|
|/DMI :\<Deliverymax >|Définit le nombre maximal d’éléments pour la remise par lot. Cette option est valide uniquement si **/cm** est défini sur personnalisé.|
|/dmlt :\<Deliverytime >|Définit la latence maximale dans la fourniture d’un lot d’événements. \<Valeur de Deliverytime > est le nombre de millisecondes. Cette option est valide uniquement si **/cm** est défini sur personnalisé.|
|/ Bonjour :\<pulsation >|Définit l’intervalle de pulsation. \<Pulsation > est le nombre de millisecondes. Cette option est valide uniquement si **/cm** est défini sur personnalisé.|
|/ CF :\<contenu >|Spécifie le format des événements qui sont retournées. \<Contenu > peuvent être des événements ou RenderedText. Lorsque la valeur est RenderedText, les événements sont retournés avec les chaînes localisées (par exemple, la description de l’événement) attachés à l’événement. La valeur par défaut est RenderedText.|
|/ l:\<paramètres régionaux >|Spécifie les paramètres régionaux pour la remise des chaînes localisées au format RenderedText. \<Paramètres régionaux > est un identificateur de langue et de pays/région, par exemple, « EN-us ». Cette option est valide uniquement si le **/CF** option est définie sur RenderedText.|
|/ree:[\<Readexist>]|Identifie les événements qui sont fournies pour l’abonnement. \<Readexist > peut true ou false. Lorsque le <Readexist> est true, tous les événements existants sont lus à partir de sources d’événements de l’abonnement. Lorsque le <Readexist> est false, seuls les événements futurs (entrants) sont remis. La valeur par défaut est true pour un **/ree** option sans valeur. Si aucun **/ree** est spécifiée, la valeur par défaut est false.|
|/lf:\<Logfile>|Spécifie le journal des événements local qui est utilisé pour stocker les événements envoyés par les sources d’événements.|
|pn :\<Publishername >|Spécifie le nom du serveur de publication. Il doit être un serveur de publication qui est propriétaire ou importe le journal spécifié par le **/lf** option.|
|/essp :\<Enableport >|Spécifie que le numéro de port doit être ajouté au nom principal de service du service distant. \<Enableport > peut être true ou false. Le numéro de port est ajouté lorsque <Enableport> a la valeur true. Lorsque le numéro de port est ajouté, une configuration peut être nécessaire pour empêcher l’accès aux sources d’événements à partir de refusée.|
|/HN :\<nom d’hôte >|Spécifie le nom DNS de l’ordinateur local. Ce nom est utilisé par la source d’événements à distance pour envoyer des événements et doit être utilisé uniquement pour un abonnement envoyé.|
|/CT :\<type >|Définit le type d’informations d’identification pour l’accès à distance source. \<Type > doit être une des valeurs suivantes : par défaut, negotiate, digest, basic ou localmachine. La valeur par défaut est la valeur par défaut.|
|/cun :\<Comusername >|Définit les informations d’identification de l’utilisateur partagées à utiliser pour les sources d’événements qui n’ont pas leurs propres informations d’identification de l’utilisateur. Si cette option est spécifiée avec la **/c** option, les paramètres de nom d’utilisateur et UserPassword pour les sources d’événements individuels à partir du fichier de configuration sont ignorés. Si vous souhaitez utiliser les informations d’identification différentes pour une source d’événement spécifique, vous devez substituer cette valeur en spécifiant le **/un** et **/up** options pour une source d’événements spécifique sur la ligne de commande d’un autre **ss** commande.|
|/ GOBELET :\<Compassword >|Définit le mot de passe utilisateur pour les informations d’identification de l’utilisateur partagées. Lorsque \<Compassword > a la valeur * (astérisque), le mot de passe est lu à partir de la console. Cette option est valide uniquement lorsque le **/cun** option est spécifiée.|
|/q:[\<Quiet>]|Spécifie si la procédure de configuration demande une confirmation. \<Quiet > peut être true ou false. Si <Quiet> a la valeur true, la procédure de configuration n’affiche aucune invite à confirmer l’opération. La valeur par défaut de cette option a la valeur false.|

## <a name="remarks"></a>Notes

> [!IMPORTANT]
> Si vous recevez le message « le serveur RPC n’est pas disponible ? Lorsque vous essayez d’exécuter wecutil, vous devez démarrer le service collecteur d’événements Windows (wecsvc). Pour démarrer wecsvc, à une invite de commandes avec élévation de privilèges, tapez net démarrez wecsvc.

- L’exemple suivant illustre le contenu d’un fichier de configuration :  
  ```
  <Subscription xmlns="https://schemas.microsoft.com/2006/03/windows/events/subscription">
  <Uri>https://schemas.microsoft.com/wbem/wsman/1/windows/EventLog</Uri>
  <!-- Use Normal (default), Custom, MinLatency, MinBandwidth -->
  <ConfigurationMode>Normal</ConfigurationMode>
  <Description>Forward Sample Subscription</Description>
  <SubscriptionId>SampleSubscription</SubscriptionId>
  <Query><![CDATA[
  <QueryList>
  <Query Path="Application">
  <Select>*</Select>
  </Query>
  </QueryList>
  ]]></Query>
  <EventSources>
  <EventSource Enabled="true">
  <Address>mySource.myDomain.com</Address>
  <UserName>myUserName</UserName>
  <Password>*</Password>
  </EventSource>
  </EventSources>
  <CredentialsType>Default</CredentialsType>
  <Locale Language="EN-US"></Locale>
  </Subscription>
  ```

## <a name="BKMK_examples"></a>Exemples

Informations de configuration de sortie pour un abonnement nommé sub1 :
```
wecutil gs sub1
```
Exemple de sortie :
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
Afficher l’état d’exécution d’un abonnement nommé sub1 :
```
wecutil gr sub1
```
Mettre à jour la configuration de l’abonnement nommée sub1 à partir d’un nouveau fichier XML appelé WsSelRg2.xml :
```
wecutil ss sub1 /c:%Windir%\system32\WsSelRg2.xml
```
Mettre à jour la configuration de l’abonnement nommée sub2 avec plusieurs paramètres :
```
wecutil ss sub2 /esa:myComputer /ese /un:uname /up:* /cm:Normal
```
Créer un abonnement pour transférer les événements à partir d’un journal des événements Windows Vista Application d’un ordinateur distant à mySource.myDomain.com dans le journal ÉvénementsTransférés (consultez la section Notes pour obtenir un exemple de fichier de configuration) :
```
wecutil cs subscription.xml
```
Supprimer un abonnement nommé sub1 :
```
wecutil ds sub1
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
