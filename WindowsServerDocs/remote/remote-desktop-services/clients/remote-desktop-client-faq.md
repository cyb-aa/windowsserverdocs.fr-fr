---
title: Clients Bureau à distance Forum aux questions
description: Forum aux questions sur les clients Bureau à distance
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 785a18cf-a5d0-4bc2-95e4-9ef53ee8f65a
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 07/16/2018
ms.localizationpriority: medium
ms.openlocfilehash: ec1b0a17c578f2d8ac55d1704af6b267b6bb8e5c
ms.sourcegitcommit: d5f10c0c98a9976a86be9f4fa8866650c7fcfc1a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/12/2019
ms.locfileid: "9065946"
---
# Forum aux questions sur les clients Bureau à distance

>S’applique à: Windows10, Windows8.1, Windows Server2012R2, WindowsServer2016

Maintenant que vous avez configuré le client Bureau à distance sur votre appareil (Android, Mac, iOS ou Windows), vous pouvez avoir des questions. Vous trouverez ici les réponses aux questions plus fréquemment posées sur les clients Bureau à distance. 

- [Configuration de](#Setting-up)
- [Connexions, la passerelle et réseaux](#connection-gateway-and-networks)
- [Client Web](#web-client)
- [Moniteur, audio, ainsi que la souris](#monitors-audio-and-mouse)
- [Matériel de Mac](#mac-client---hardware-questions)
- [Messages d’erreur spécifiques](#specific-errors)

La plupart de ces questions s’appliquent à tous les clients, mais il existe quelques éléments spécifiques de client.

Si vous avez des questions supplémentaires que vous souhaitez que nous répondre, conservez les commentaires sur cet article.

## Configuration de

### Les PC puis-je me connecter à?

Consultez l’article pour plus d’informations sur les PC que vous pouvez vous connecter à [configuration prise en charge](remote-desktop-supported-config.md) .

### Comment configurer un PC de bureau à distance?

J’ai mon appareil configuré, mais je ne pensez pas que prêt de l’ordinateur. Aide?

Tout d’abord, vous avez vu l’Assistant Installation de bureau à distance? Il vous explique de préparation de votre PC pour l’accès à distance. Téléchargez et exécutez cet utilitaire sur votre PC pour obtenir tous les éléments définissent. 

Dans le cas contraire, si vous préférez effectuer les opérations manuellement, lisez la suite.

Pour Windows 10, procédez comme suit:

1. Sur l’appareil que vous souhaitez vous connecter, ouvrir les **paramètres**.
2. Sélectionnez **système** , puis **Bureau à distance**.
3. Utilisez le curseur pour activer le Bureau à distance.
4. En règle générale, il est préférable de conserver le PC allumé et détectable pour faciliter les connexions. Cliquez sur **Afficher les paramètres** pour accéder aux paramètres d’alimentation pour votre PC, dans lequel vous pouvez modifier ce paramètre.
   > [!NOTE]
   > Vous ne peut pas se connecter à un ordinateur qui est en veille ou veille prolongée, par conséquent, assurez-vous que les paramètres de mise en veille et de mise en veille prolongée sur l’ordinateur distant sont définies sur **jamais**. (Mise en veille prolongée n’est pas disponible sur tous les PC.)


Notez le nom de ce PC sous **comment se connecter à ce PC**. Vous aurez besoin pour configurer les clients.

Vous pouvez accorder l’autorisation des utilisateurs spécifiques à accéder à ce PC - pour ce faire, cliquez sur **Sélectionner les utilisateurs qui peuvent accéder à distance à cet ordinateur**.
Les membres du groupe Administrateurs ont automatiquement accès.

Pour Windows 8.1, suivez les instructions pour autoriser les connexions à distance de [se connecter à un autre bureau à l’aide de connexions Bureau à distance](https://support.microsoft.com/en-us/help/17463/windows-7-connect-to-another-computer-remote-desktop-connection#1TC=windows-8).



## Connexion, la passerelle et réseaux

### Pourquoi ne puis-je pas me connecter à l’aide des services Bureau à distance?

Voici quelques solutions possibles aux problèmes courants que vous pouvez rencontrer lors de la tentative de connexion à un PC distant. Si ces solutions ne fonctionnent pas, vous trouverez plus d’informations sur le [site Web de la Communauté Microsoft](https://go.microsoft.com/fwlink/p/?LinkId=242079).

- **Impossible de trouver l’ordinateur distant.** Vérifiez que vous le nom du PC droit et vérifiez si vous avez entré correctement ce nom. Si vous ne pouvez pas à vous connecter, essayez d’utiliser l’adresse IP de l’ordinateur distant au lieu du nom du PC.
- **Il existe un problème avec le réseau.** Vérifiez que vous disposez de connexion internet. 
- **Le port du Bureau à distance peut-être être bloqué par un pare-feu.** Si vous utilisez le pare-feu Windows, procédez comme suit:

   1. Ouvrez le pare-feu Windows. 
   2. Cliquez sur **Autoriser une application ou une fonctionnalité via le pare-feu Windows**. 
   3. Cliquez sur **Modifier les paramètres**. Vous pouvez être invité pour un mot de passe administrateur ou confirmer votre choix.
   4. Sous **les fonctionnalités et les applications autorisées**, sélectionnez **Bureau à distance**, puis appuyez ou cliquez sur **OK**.

   Si vous utilisez un autre pare-feu, assurez-vous que le port pour les services Bureau à distance (généralement 3389) est ouvert.
- **Les connexions à distance ne peuvent pas être configurées sur le PC distant.** Pour résoudre ce problème, faites défiler sauvegarder vers [comment j’ai configurer un PC de bureau à distance?](#how-do-i-set-up-a-pc-for-remote-desktop) question dans cette rubrique.
- **L’ordinateur distant peut autoriser uniquement les PC pour se connecter qui ont authentification NLA configurer.** 
- **L’ordinateur distant peut être désactivée.** Vous ne peut pas se connecter à un ordinateur qui est éteint, en veille ou veille prolongée, par conséquent, vérifiez que les paramètres pour mettre en veille et mise en veille prolongée sur l’ordinateur distant sont définies sur **jamais** (mise en veille prolongée n’est pas disponible sur tous les PC.).

### Pourquoi ne puis-je pas rechercher ou vous connecter à mon ordinateur?
Vérifiez les points suivants:
- Est le PC et en éveil?
- Vous entrez le nom ou l’adresse IP?

   > [!IMPORTANT]
   > En utilisant le nom de PC nécessite votre réseau afin de résoudre le nom correctement via DNS. Dans de nombreux réseaux domestiques, vous devez utiliser l’adresse IP plutôt que le nom d’hôte pour se connecter.
- Est le PC sur un réseau différent? Vous n’avez configuré le PC pour permettre à l’extérieur de connexions par le biais de?  Regardez [Autoriser l’accès à votre PC à partir de l’extérieur du réseau](remote-desktop-allow-outside-access.md) de l’aide.
- Vous connectez-vous à une version de Windows prises en charge? 

   > [!NOTE]
   > Windows XP Édition familiale, Édition de Windows Media Center, Windows Vista Édition familiale et Windows 7 Édition familiale ou Starter ne sont pas prises en charge sans 3e logiciels tiers.

### Pourquoi ne puis-je pas se connecter à un PC distant?
Si vous voyez l’écran de connexion de l’ordinateur distant, mais vous ne pouvez pas vous connecter, vous ne peut-être pas ont été ajoutées au groupe d’utilisateurs du Bureau à distance ou à un groupe avec des droits d’administrateur sur l’ordinateur distant. Demandez à votre administrateur système pour effectuer cette opération pour vous.

### Les méthodes de connexion sont pris en charge pour les réseaux d’entreprise?
Si vous souhaitez accéder à votre bureau office à partir d’en dehors de votre réseau d’entreprise, votre entreprise doit vous fournir un moyen d’accès à distance. Le Client Bureau à distance prend actuellement en charge les éléments suivants:

- Passerelle des services Terminal Server ou la passerelle des services Bureau à distance
- Accès Web Bureau à distance
- VPN (par le biais des options de VPN intégrées iOS)

### VPN ne fonctionne pas

Plusieurs raisons peuvent rencontrer des problèmes VPN. La première étape consiste à vérifier que la connexion VPN fonctionne sur le même réseau que votre ordinateur PC ou un Mac. Si vous ne pouvez pas tester avec un PC ou un Mac, vous pouvez tenter d’accéder à une page web d’intranet société avec le navigateur de votre appareil.

Autres éléments à vérifier:
- **Le réseau 3G bloque ou altère VPN.** Il existe plusieurs 3G des fournisseurs dans le monde entier qui semblent se bloquer ou endommagées 3G le trafic. Vérifier la connectivité VPN fonctionne correctement pour par minute.
- **L2TP ou PPTP VPN.** Si vous utilisez PPTP ou L2TP dans votre réseau privé virtuel, définissez envoyer tout le trafic du on dans la configuration VPN.
- **VPN est mal configuré.** Un serveur VPN mal configuré peut être la raison pour laquelle les connexions VPN travaillé ou a cessé de fonctionner après un certain temps de jamais. Vérifiez si tel est le cas de test avec l’iOS Mac ou un PC ou un navigateur web de l’appareil sur le même réseau.

### Comment puis-je tester si VPN fonctionne correctement?
Vérifiez que le VPN est activé sur votre appareil. Vous pouvez tester votre connexion VPN en accédant à une page Web sur votre réseau interne ou en utilisant un service web qui est uniquement disponible via le réseau privé virtuel.

### Comment configurer les connexions L2TP ou PPTP VPN?
Si vous utilisez PPTP ou L2TP dans votre réseau privé virtuel, veillez à définir **Envoyer tout le trafic** **on** dans la configuration VPN.

## Client Web

### Quels sont les navigateurs puis-je utiliser?

Prend en charge le client web Microsoft Edge, Internet Explorer 11, Mozilla Firefox (v55.0 et versions ultérieures), Safari et Google Chrome.

### Les PC puis-je utiliser pour accéder au client web?

Le client web prend en charge Windows, Mac OS, Linux et ChromeOS. Les appareils mobiles ne sont pas prises en charge pour l’instant.

### Puis-je utiliser le client web dans un déploiement des services Bureau à distance sans une passerelle?

Non. Le client a besoin d’une passerelle des services Bureau à distance pour se connecter. Ne savez pas ce que cela signifie? Demandez à votre administrateur à ce sujet.

### Le client Bureau à distance du web remplace-t-il la page accès Bureau à distance par le Web?

Non. Le client Bureau à distance du web est hébergé sur une URL différente de la page accès Bureau à distance par le Web. Vous pouvez utiliser le client web ou la page Web Access pour afficher les ressources distantes dans un navigateur.

### Puis-je incorporer le client web dans une autre page web?

Cette fonctionnalité n’est pas pris en charge pour le moment.

## Moniteur, audio, ainsi que la souris

### Comment utiliser tous les moniteurs?
Pour utiliser les deux ou plusieurs écrans, procédez comme suit:

1. Cliquez sur le Bureau à distance que vous souhaitez activer plusieurs écrans pour, puis cliquez sur **Modifier**.
2. Activez **utiliser tous les écrans** et **plein écran**.

### Son bidirectionnel est pris en charge?
Son en amont (du client au serveur, pour des microphones) n’est pas pris en charge par le Client Bureau à distance.

### Que puis-je faire si le son ne lit pas?
Déconnectez-vous de la session (ne simplement se déconnecter, déconnectez-vous extrême), puis connectez-vous à nouveau.

## Client Mac - questions sur le matériel
### Résolution de la rétine est pris en charge?
Oui, le client Bureau à distance prend en charge la résolution de la rétine.

### Comment puis-je activer avec le bouton droit secondaire?
Pour faire en sorte utiliser sur le bouton droit à l’intérieur d’une session ouverte, vous disposez de trois options:

- Souris USB de standard PC deux boutons
- Apple Magic souris: Pour activer avec le bouton droit, cliquez sur **Préférences système** dans la station d’accueil, cliquez sur **la souris**et activer puis **cliquez sur secondaire**.
- Apple Magic Trackpad ou MacBook Trackpad: activer avec le bouton droit, cliquez sur **Préférences système** dans la station d’accueil, cliquez sur **la souris**, et activer puis **cliquez sur secondaire**.

### AirPrint est pris en charge?
Non, le client Bureau à distance ne prend pas en charge AirPrint. (Cela est vrai pour les clients Mac et iOS).

### Pourquoi des caractères incorrects n’apparaissent pas dans la session?
Si vous utilisez un clavier international, vous constaterez peut-être un problème dans lequel les caractères qui apparaissent dans la session correspondent les caractères saisis sur le clavier de Mac.

Cela peut se produire dans les scénarios suivants:

- Vous utilisez un clavier qui ne prend pas en compte la session à distance. Lorsque les services Bureau à distance ne reconnaît pas le clavier, il est par défaut le langage dernier utilisé avec l’ordinateur distant.
- Vous vous connectez à une session précédemment déconnectée sur un PC distant et que le PC distant utilise une langue du clavier autre que la langue vous essayez actuellement à utiliser.

Vous pouvez résoudre ce problème en définissant manuellement la langue du clavier pour la session à distance. Consultez les étapes décrites dans la section suivante.

### Comment les paramètres de langue affectent les claviers dans une session à distance?
Il existe de nombreux types de dispositions de clavier Mac. Certaines d'entre elles sont des dispositions spécifiques Mac ou pour laquelle une correspondance exacte ne peut-être pas être disponible sur la version de Windows que vous utilisez la communication à distance dans des dispositions personnalisées. La session à distance mappe votre clavier à la meilleure correspondance de langue du clavier disponible sur l’ordinateur distant. 

Si votre clavier Mac est défini sur la version PC de votre clavier et le clavier de langue (par exemple, Français – PC) toutes vos clés doivent être mappés correctement doit fonctionner.

Si votre clavier Mac est défini sur la version Mac d’un clavier (par exemple, Français) la session à distance vous correspondra à la version PC de la langue Français. Certains des raccourcis clavier Mac que vous servent à l’aide de OSX ne fonctionnera pas dans la session à distance de Windows.

Si votre clavier est défini sur une variante d’une langue (par exemple, Français canadien) et si la session à distance ne peut pas vous mappez à cette variante exacte, la session à distance vous correspondra à la plus proche langue (par exemple, Français). Certains des raccourcis clavier Mac que vous servent à l’aide de OSX ne fonctionnera pas dans la session à distance de Windows.

Si votre clavier est définie sur une disposition de que la session à distance ne correspondre pas du tout, votre session à distance par défaut pour vous donner la langue que vous avez utilisé avec ce PC. Dans ce cas, ou dans les cas où vous devez modifier la langue de votre session à distance pour faire correspondre votre clavier Mac, vous pouvez définir manuellement la langue du clavier dans la session à distance à la langue qui correspond à celui que vous souhaitez utiliser comme suit le plus proche.

Utilisez les instructions suivantes pour modifier la disposition du clavier à l’intérieur de la session Bureau à distance:

**Sur Windows 10 ou Windows 8:**

1. À partir de la session à distance, ouvrir région et langue. Cliquez sur **Démarrer > paramètres > heure et langue**. Ouvrez la **région et langue**.
2. Ajoutez la langue que vous souhaitez utiliser. Puis fermez la fenêtre de langue et de région.
3. Maintenant, dans la session à distance, vous verrez la possibilité de basculer entre les langues. (Dans le côté droit de la session à distance, près de l’horloge.) Cliquez sur la langue que vous souhaitez faire basculer vers (par exemple, **Eng**).

Vous devrez peut-être fermer et redémarrer l’application que vous utilisez actuellement pour que les modifications de clavier prennent effet.


## Erreurs spécifiques

### Je reçois une erreur «Privilèges insuffisants»?
Vous n’êtes pas autorisé à accéder à la session que vous souhaitez vous connecter. La plus probable est que vous essayez de vous connecter à une session d’administration. Seuls les administrateurs sont autorisés à se connecter à la console. Vérifiez que le commutateur est désactivé dans les paramètres avancés du Bureau à distance. Si ce n’est pas la source du problème, contactez votre administrateur système pour obtenir une assistance supplémentaire.

### Pourquoi le client indique qu’il n’existe aucune licence d’accès client?
Lorsqu’un client Bureau à distance se connecte à un serveur des services Bureau à distance, le serveur émet un distant Desktop Services licence d’accès Client (RDS) stockées par le client. Chaque fois que le client se connecte à nouveau, il utilise ses CAL RDS et le serveur n’émettra pas une autre licence. Le serveur émet une autre licence si la CAL services Bureau à distance sur l’appareil est manquant ou endommagé. Lorsque le nombre maximal de périphériques sous licence est atteint le serveur n’émettra pas nouvelles licences. Contactez votre administrateur réseau.

### Pourquoi ai-je reçu une erreur «Accès refusé»?
L’erreur «Accès refusé» est un généré par la passerelle des services Bureau à distance et le résultat des informations d’identification incorrectes lors de la tentative de connexion. Vérifiez votre nom d’utilisateur et mot de passe. Si la connexion travaillé et que l’erreur s’est produite récemment, vous éventuellement modifiez votre mot de passe du compte utilisateur Windows et ne l’avez pas encore mis à jour son dans les paramètres du Bureau à distance.

### Que signifie «Erreur RPC 23014» ou moyenne de «Erreur 0x59e6»?
En cas d’une **erreur RPC 23014** ou **erreur 0x59E6 essayez à nouveau après attendre quelques minutes**, le serveur de passerelle des services Bureau à distance a atteint le nombre maximal de connexions actives. En fonction de la version de Windows en cours d’exécution sur la passerelle des services Bureau à distance le nombre maximal de connexions est différent: implémentation The Windows Server 2008 R2 Standard limite le nombre de connexions à 250. L’implémentation de Windows Server 2008 R2 Foundation limite le nombre de connexions à 50. Toutes les autres implémentations de Windows permettent un nombre illimité de connexions.

### Que signifie l’erreur «Impossible d’analyser le défi NTLM»?
Cette erreur est due à une configuration incorrecte sur l’ordinateur distant. Assurez-vous que le paramètre de niveau de sécurité RDP sur le PC distant est défini sur «Compatible avec le Client». (Contactez votre administrateur système si vous avez besoin d’effectuer cette opération.)

### Ce que fait «TS_RAP vous n’êtes pas autorisé à se connecter à l’hôte donné» signifie?
Cette erreur se produit lorsqu’une stratégie d’autorisation de ressource sur le serveur de passerelle s’arrête votre nom d’utilisateur de se connecter au PC distant. Cela peut se produire dans les cas suivants:

- Le nom du PC à distance est identique à celui de la passerelle. Ensuite, lorsque vous essayez de vous connecter au PC distant, la connexion accède à la passerelle à la place, ce qui vous n’avez probablement pas autorisée à accéder à. Si vous avez besoin pour vous connecter à la passerelle, n’utilisez pas le nom de la passerelle externe comme nom d’ordinateur. Utilisez plutôt le nom du serveur interne ou l’adresse IP (127.0.0.1) ou «localhost».
- Votre compte d’utilisateur n’est pas un membre du groupe d’utilisateurs pour l’accès à distance.