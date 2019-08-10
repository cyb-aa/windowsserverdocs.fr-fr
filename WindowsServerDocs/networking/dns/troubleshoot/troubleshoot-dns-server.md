---
title: Dépannage des serveurs DNS
description: Cet article explique comment résoudre les problèmes DNS du côté serveur.
manager: willchen
ms.prod: ''
ms.technology: networking-dns
ms.topic: article
ms.author: delhan
ms.date: 8/8/2019
author: Deland-Han
ms.openlocfilehash: b0547436cfa0f07ba9cbc4e3dd1825f8d33bc093
ms.sourcegitcommit: 0e3c2473a54f915d35687d30d1b4b1ac2bae4068
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68917766"
---
# <a name="troubleshooting-dns-servers"></a>Dépannage des serveurs DNS

Cet article explique comment résoudre les problèmes sur les serveurs DNS.

## <a name="check-ip-configuration"></a>Vérifier la configuration IP

1. Exécutez `ipconfig /all` à partir d’une invite de commandes et vérifiez l’adresse IP, le masque de sous-réseau et la passerelle par défaut.

2. Vérifiez si le serveur DNS fait autorité pour le nom recherché. Dans ce cas, consultez [vérification des problèmes avec les données faisant autorité](#checking-for-problems-with-authoritative-data).

3. Exécutez la commande suivante :

   ```cmd
   nslookup <name> <IP address of the DNS server>
   ```
   Exemple : 
   ```cmd
   nslookup app1 10.0.0.1
   ```
   Si vous obtenez une réponse d’échec ou de délai d’attente, consultez [vérification des problèmes de récurrence](#checking-for-recursion-problems).

4. Videz le cache du programme de résolution. Pour ce faire, exécutez la commande suivante dans une fenêtre d’invite de commandes d’administration:

   ```cmd
   dnscmd /clearcache
   ```
   Ou, dans une fenêtre PowerShell d’administration, exécutez l’applet de commande suivante:
   ```powershell
   Clear-DnsServerCache
   ```

5. Répétez l’étape 3. 

## <a name="check-dns-server-problems"></a>Vérifier les problèmes de serveur DNS

### <a name="event-log"></a>Journal des événements

Vérifiez les journaux suivants pour voir s’il existe des erreurs enregistrées:

- Application

- System

- Serveur DNS

### <a name="test-by-using-nslookup-query"></a>Tester à l’aide de la requête nslookup

Exécutez la commande suivante et vérifiez si le serveur DNS est accessible à partir des ordinateurs clients.

```cmd
nslookup <client name> <server IP address>
```

- Si le programme de résolution renvoie l’adresse IP du client, le serveur n’a aucun problème.

- Si le programme de résolution renvoie une réponse «défaillance du serveur» ou «requête refusée», la zone est probablement suspendue, ou le serveur est probablement surchargé. Vous pouvez savoir si elle est suspendue en consultant l’onglet général des propriétés de la zone dans la console DNS.

Si le programme de résolution renvoie une réponse «requête envoyée au serveur expirée» ou «aucune réponse du serveur», cela signifie que le service DNS n’est probablement pas en cours d’exécution. Essayez de redémarrer le service serveur DNS en entrant la commande suivante à l’invite de commandes sur le serveur:

```cmd
net start DNS
```

Si le problème se produit lorsque le service est en cours d’exécution, il est possible que le serveur n’écoute pas sur l’adresse IP que vous avez utilisée dans votre requête nslookup. Sous l’onglet **interfaces** de la page Propriétés du serveur dans la console DNS, les administrateurs peuvent limiter un serveur DNS à écouter uniquement les adresses sélectionnées. Si le serveur DNS a été configuré pour limiter le service à une liste spécifique de ses adresses IP configurées, il est possible que l’adresse IP utilisée pour contacter le serveur DNS ne figure pas dans la liste. Vous pouvez essayer une autre adresse IP dans la liste ou ajouter l’adresse IP à la liste.

Dans de rares cas, le serveur DNS peut avoir une configuration de pare-feu ou de sécurité avancée. Si le serveur se trouve sur un autre réseau accessible uniquement via un hôte intermédiaire (tel qu’un routeur de filtrage de paquets ou un serveur proxy), le serveur DNS peut utiliser un port non standard pour écouter et recevoir les demandes des clients. Par défaut, nslookup envoie des requêtes aux serveurs DNS sur le port UDP 53. Par conséquent, si le serveur DNS utilise un autre port, les requêtes nslookup échouent. Si vous pensez que cela peut être le problème, vérifiez si un filtre intermédiaire est utilisé intentionnellement pour bloquer le trafic sur les ports DNS connus. Si ce n’est pas le cas, essayez de modifier les filtres de paquets ou les règles de port sur le pare-feu pour autoriser le trafic sur le port UDP/TCP 53.

## <a name="checking-for-problems-with-authoritative-data"></a>Vérification des problèmes avec les données faisant autorité

Vérifiez si le serveur qui retourne la réponse incorrecte est un serveur principal pour la zone (serveur principal standard pour la zone ou un serveur qui utilise Active Directory intégration pour charger la zone) ou un serveur qui héberge une copie secondaire de la zone. 

### <a name="if-the-server-is-a-primary-server"></a>Si le serveur est un serveur principal

Le problème peut être causé par une erreur de l’utilisateur lorsque les utilisateurs entrent des données dans la zone. Il peut également s’agir d’un problème affectant la réplication de Active Directory ou la mise à jour dynamique.

### <a name="if-the-server-is-hosting-a-secondary-copy-of-the-zone"></a>Si le serveur héberge une copie secondaire de la zone

1. Examinez la zone sur le serveur maître (le serveur à partir duquel ce serveur extrait les transferts de zone).

   > [!NOTE]
   >Vous pouvez déterminer quel serveur est le serveur maître en examinant les propriétés de la zone secondaire dans la console DNS.

   Si le nom n’est pas correct sur le serveur maître, passez à l’étape 4.

2. Si le nom est correct sur le serveur maître, vérifiez si le numéro de série sur le serveur maître est inférieur ou égal au numéro de série sur le serveur secondaire. Si c’est le cas, modifiez le serveur maître ou le serveur secondaire de sorte que le numéro de série sur le serveur maître soit supérieur au numéro de série sur le serveur secondaire. 
  
3. Sur le serveur secondaire, forcez un transfert de zone à partir de la console DNS ou en exécutant la commande suivante:
  
   ```cmd
   dnscmd /zonerefresh <zone name>
   ```
  
   Par exemple, si la zone est corp.contoso.com, entrez: `dnscmd /zonerefresh corp.contoso.com`.
  
4. Examinez de nouveau le serveur secondaire pour voir si la zone a été correctement transférée. Si ce n’est pas le cas, vous avez probablement un problème de transfert de zone. Pour plus d’informations, consultez [problèmes de transfert de zone](#zone-transfer-problems).

5. Si la zone a été correctement transférée, vérifiez si les données sont à présent correctes. Si ce n’est pas le cas, les données sont incorrectes dans la zone principale. Le problème peut être causé par une erreur de l’utilisateur lorsque les utilisateurs entrent des données dans la zone. Il peut également s’agir d’un problème affectant la réplication de Active Directory ou la mise à jour dynamique.

## <a name="checking-for-recursion-problems"></a>Vérification des problèmes de récursivité

Pour que la récursivité fonctionne correctement, tous les serveurs DNS utilisés dans le chemin d’accès d’une requête récursive doivent être en mesure de répondre et de transférer les données correctes. Si ce n’est pas le cas, une requête récursive peut échouer pour l’une des raisons suivantes:

- La requête expire avant de pouvoir être terminée.

- Un serveur qui est utilisé pendant la requête ne répond pas.

- Un serveur qui est utilisé pendant la requête fournit des données incorrectes.

Commencez la résolution des problèmes sur le serveur qui a été utilisé dans votre requête d’origine. Vérifiez si ce serveur transfère les requêtes vers un autre serveur en examinant l’onglet redirecteurs dans les propriétés du serveur dans la console DNS. Si la case à cocher **activer** les redirecteurs est activée et qu’un ou plusieurs serveurs sont répertoriés, ce serveur transfère les requêtes.

Si ce serveur transfère les requêtes vers un autre serveur, recherchez les problèmes qui affectent le serveur vers lequel ce serveur transfère les requêtes. Pour vérifier la résolution des problèmes, consultez [vérifier les problèmes du serveur DNS](#check-dns-server-problems). Lorsque cette section vous indique comment effectuer une tâche sur le client, exécutez-la sur le serveur à la place.

Si le serveur est sain et peut transférer des requêtes, répétez cette étape, puis examinez le serveur sur lequel ce serveur transfère les requêtes.

Si ce serveur ne transfère pas de requêtes vers un autre serveur, testez si ce serveur peut interroger un serveur racine. Pour ce faire, exécutez la commande suivante :

```cmd
nslookup
server <IP address of server being examined>
set q=NS
 ```

- Si le programme de résolution retourne l’adresse IP d’un serveur racine, vous avez probablement une délégation rompue entre le serveur racine et le nom ou l’adresse IP que vous essayez de résoudre. Suivez la procédure [tester une délégation rompue](#test-a-broken-delegation) pour déterminer si vous avez une délégation rompue.

- Si le programme de résolution renvoie une réponse «demande au serveur expiré», vérifiez si les indications de racine pointent vers des serveurs racines en fonctionnement. Pour ce faire, utilisez le [pour afficher la procédure d’indications de racine actuelle](#to-view-the-current-root-hints) . Si les indications de racine pointent vers des serveurs racine opérationnels, vous pouvez rencontrer un problème réseau ou le serveur peut utiliser une configuration de pare-feu avancée qui empêche le programme de résolution d’interroger le serveur, comme décrit dans la section [vérifier les problèmes de serveur DNS](#check-dns-server-problems) . Il est également possible que la valeur par défaut du délai d’expiration récursif soit trop petite.

### <a name="test-a-broken-delegation"></a>Tester une délégation rompue

Commencez les tests de la procédure suivante en interrogeant un serveur racine valide. Le test vous guide tout au long du processus d’interrogation de tous les serveurs DNS à partir de la racine jusqu’au serveur que vous testez pour une délégation rompue.

1. À l’invite de commandes sur le serveur que vous testez, entrez ce qui suit:

   ```cmd
   nslookup
   server <server IP address>
   set norecursion
   set querytype= <resource record type>
   <FQDN>
   ```
   > [!NOTE]
   >Le type d’enregistrement de ressource est le type d’enregistrement de ressource pour lequel vous interrogez dans votre requête d’origine, et le nom de domaine complet est le nom de domaine complet pour lequel vous interrogez (terminé par un point).
 
2. Si la réponse comprend une liste d’enregistrements de ressources «NS» et «A» pour les serveurs délégués, répétez l’étape 1 pour chaque serveur et utilisez l’adresse IP des enregistrements de ressource «A» comme adresse IP du serveur.

   - Si la réponse ne contient pas d’enregistrement de ressource «NS», vous avez une délégation rompue.
   
   - Si la réponse contient des enregistrements de ressources «NS», mais pas d’enregistrements de ressources «A», entrez **Set récursivité**, puis interrogez individuellement pour obtenir les enregistrements de ressource «a» des serveurs répertoriés dans les enregistrements «NS». Si vous ne trouvez pas au moins une adresse IP valide d’un enregistrement de ressource «A» pour chaque enregistrement de ressource NS dans une zone, vous avez une délégation rompue.

3. Si vous déterminez que vous avez une délégation rompue, corrigez-la en ajoutant ou en mettant à jour un enregistrement de ressource «A» dans la zone parente à l’aide d’une adresse IP valide pour un serveur DNS correct pour la zone déléguée.

### <a name="to-view-the-current-root-hints"></a>Pour afficher les indications de racine actuelles

1. Démarrez la console DNS.

2. Ajoutez ou connectez-vous au serveur DNS qui a échoué à une requête récursive.

3. Cliquez avec le bouton droit sur le serveur, puis sélectionnez **Propriétés**.

4. Cliquez sur indications de racine.

Vérifiez la connectivité de base aux serveurs racine.

- Si les indications de racine semblent être correctement configurées, vérifiez que le serveur DNS utilisé dans une résolution de noms défaillante peut envoyer une requête ping aux serveurs racines par adresse IP.

- Si les serveurs racine ne répondent pas au test ping par adresse IP, les adresses IP des serveurs racine peuvent avoir changé. Toutefois, il est rare de voir une reconfiguration des serveurs racine.

## <a name="zone-transfer-problems"></a>Problèmes de transfert de zone

Exécutez les vérifications suivantes:

- Vérifiez observateur d’événements pour le serveur DNS principal et le serveur DNS secondaire.

- Vérifiez le serveur maître pour voir s’il refuse d’envoyer le transfert pour la sécurité. 

- Vérifiez l’onglet **transferts de zone** des propriétés de la zone dans la console DNS. Si le serveur limite les transferts de zone à une liste de serveurs, tels que ceux répertoriés sous l’onglet **serveurs de noms** des propriétés de la zone, assurez-vous que le serveur secondaire se trouve dans cette liste. Assurez-vous que le serveur est configuré pour envoyer des transferts de zone.

- Pour en savoir plus sur le serveur maître, suivez les étapes décrites dans la section [vérifier les problèmes de serveur DNS](#check-dns-server-problems) . Lorsque vous êtes invité à effectuer une tâche sur le client, effectuez la tâche sur le serveur secondaire à la place.

- Vérifiez si le serveur secondaire exécute une autre implémentation de serveur DNS, telle que la liaison. Si c’est le cas, le problème peut être dû à l’une des causes suivantes:

  - Le serveur maître Windows peut être configuré pour envoyer des transferts de zone rapides, mais il est possible que le serveur secondaire tiers ne prenne pas en charge les transferts de zone rapide. Dans ce cas, désactivez les transferts de zone rapide sur le serveur maître à partir de la console DNS en activant la case à cocher **activer les liaisons secondaires** dans l’onglet **avancé** des propriétés de votre serveur.

  - Si une zone de recherche directe sur le serveur Windows contient un type d’enregistrement (par exemple, un enregistrement SRV) qui n’est pas pris en charge par le serveur secondaire, le serveur secondaire risque de rencontrer des problèmes pour extraire la zone.

Vérifiez si le serveur maître exécute une autre implémentation de serveur DNS, telle que la liaison. Dans ce cas, il est possible que la zone sur le serveur maître inclue des enregistrements de ressource incompatibles que Windows ne reconnaît pas.
 
Si le serveur principal ou secondaire exécute une autre implémentation de serveur DNS, vérifiez les deux serveurs pour vous assurer qu’ils prennent en charge les mêmes fonctionnalités. Vous pouvez vérifier le serveur Windows dans la console DNS sous l’onglet **avancé** de la page Propriétés du serveur. Outre la zone activer les liaisons secondaires, cette page comprend la liste déroulante **vérification du nom** . Cela vous permet de sélectionner la mise en œuvre de la conformité RFC stricte pour les caractères dans les noms DNS.

