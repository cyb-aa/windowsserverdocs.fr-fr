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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865930"
---
# <a name="frequently-asked-questions-about-the-remote-desktop-clients"></a>Forum aux questions sur les clients Bureau à distance

>S'applique à : Windows 10, Windows 8.1, Windows Server 2012 R2, Windows Server 2016

Maintenant que vous avez configuré le client Bureau à distance sur votre appareil (Android, Mac, iOS ou Windows), vous pouvez avoir des questions. Vous trouverez ici les réponses aux questions plus fréquemment posées sur les clients Bureau à distance. 

- [Configuration de](#Setting-up)
- [Connexions, la passerelle et les réseaux](#connection-gateway-and-networks)
- [client Web](#web-client)
- [Moniteurs, audio et la souris](#monitors-audio-and-mouse)
- [Matériel Mac](#mac-client---hardware-questions)
- [Messages d’erreur spécifiques](#specific-errors)

La majorité de ces questions s’appliquent à tous les clients, mais il existe quelques éléments spécifiques de client.

Si vous avez des questions supplémentaires que vous aimeriez nous poser, conservez les commentaires sur cet article.

## <a name="setting-up"></a>Configuration de

### <a name="which-pcs-can-i-connect-to"></a>Les PC puis-je me connecter ?

Découvrez le [pris en charge de la configuration](remote-desktop-supported-config.md) article pour plus d’informations sur les PC que vous pouvez vous connecter à.

### <a name="how-do-i-set-up-a-pc-for-remote-desktop"></a>Comment configurer un PC de bureau à distance ?

Dois-je configurer mon appareil, mais je ne pense pas que les prêts du PC. Aide ?

Tout d’abord, vous avez vu l’Assistant Installation de bureau à distance ? Il vous guide de préparation de votre PC pour l’accès à distance. Téléchargez et exécutez cet utilitaire sur votre PC de tout définissent. 

Sinon, si vous préférez effectuer manuellement les choses, lisez la suite.

Pour Windows 10, procédez comme suit :

1. Sur l’appareil que vous souhaitez vous connecter, ouvrez **paramètres**.
2. Sélectionnez **système** , puis **Bureau à distance**.
3. Utilisez le curseur pour activer le Bureau à distance.
4. En règle générale, il est préférable de conserver le PC éveillés et détectable pour faciliter les connexions. Cliquez sur **afficher les paramètres** pour accéder aux paramètres d’alimentation de votre PC, où vous pouvez modifier ce paramètre.
   > [!NOTE]
   > Vous ne peut pas se connecter à un ordinateur qui est en veille ou en veille prolongée, par conséquent, vérifiez que les paramètres de mise en veille et veille prolongée sur un ordinateur distant sont la valeur **jamais**. (Mise en veille prolongée n’est pas disponible sur tous les PC.)


Prenez note du nom de ce PC sous **comment se connecter à ce PC**. Vous en aurez besoin pour configurer les clients.

Vous pouvez accorder des autorisations pour des utilisateurs spécifiques pour accéder à ce PC - pour ce faire, cliquez sur **sélectionner les utilisateurs qui peuvent accéder à distance à ce PC**.
Les membres du groupe Administrateurs ont automatiquement accès.

Pour Windows 8.1, suivez les instructions pour autoriser les connexions à distance dans [se connecter à un autre bureau à l’aide de connexions Bureau à distance](https://support.microsoft.com/en-us/help/17463/windows-7-connect-to-another-computer-remote-desktop-connection#1TC=windows-8).



## <a name="connection-gateway-and-networks"></a>Connexion, la passerelle et les réseaux

### <a name="why-cant-i-connect-using-remote-desktop"></a>Pourquoi ne puis-je pas me connecter à l’aide du Bureau à distance ?

Voici quelques solutions possibles aux problèmes courants que vous pouvez rencontrer lorsque vous tentez de vous connecter à un ordinateur distant. Si ces solutions ne fonctionnent pas, vous trouverez plus d’informations sur la [site Web de Microsoft Community](https://go.microsoft.com/fwlink/p/?LinkId=242079).

- **Impossible de trouver un ordinateur distant.** Assurez-vous que votre portent le nom d’ordinateur approprié, puis vérifier a posteriori si vous avez correctement entré ce nom. Si vous ne pouvez toujours pas vous connecter, essayez d’utiliser l’adresse IP de l’ordinateur distant au lieu du nom de PC.
- **Il existe un problème avec le réseau.** Assurez-vous que vous disposez de connexion internet. 
- **Le port du Bureau à distance peut être bloqué par un pare-feu.** Si vous utilisez des pare-feu de Windows, procédez comme suit :

   1. Ouvrez le pare-feu Windows. 
   2. Cliquez sur **autoriser une application ou une fonctionnalité via le pare-feu de Windows**. 
   3. Cliquez sur **modifier les paramètres**. Vous pouvez être invité pour un mot de passe administrateur ou à confirmer votre choix.
   4. Sous **applications et fonctionnalités autorisées**, sélectionnez **Bureau à distance**, puis appuyez ou cliquez sur **OK**.

   Si vous utilisez un autre pare-feu, assurez-vous que le port Bureau à distance (généralement 3389) est ouvert.
- **Connexions à distance ne peuvent pas être configurées sur un ordinateur distant.** Pour résoudre ce problème, faites défiler vers le sauvegarder vers [comment configurer un PC de bureau à distance ?](#how-do-i-set-up-a-pc-for-remote-desktop) question dans cette rubrique.
- **Le PC à distance peut autoriser uniquement les PC qui ont configuré l’authentification au niveau du réseau pour vous connecter.** 
- **Le PC à distance peut être désactivé.** Vous ne peut pas se connecter à un PC est éteint, en veille ou en veille prolongée, par conséquent, vérifiez que les paramètres de mise en veille et veille prolongée sur un ordinateur distant sont la valeur **jamais** (mise en veille prolongée n’est pas disponible sur tous les PC.).

### <a name="why-cant-i-find-or-connect-to-my-pc"></a>Pourquoi ne puis-je pas trouver ou se connecter à mon PC ?
Vérifiez les points suivants :
- Est l’ordinateur et en éveil ?
- N’a entré l’adresse IP ou le nom correct ?

   > [!IMPORTANT]
   > En utilisant le nom de PC nécessite votre réseau pour résoudre le nom correctement par le biais de DNS. Dans les réseaux domestiques, vous devez utiliser l’adresse IP au lieu du nom d’hôte pour vous connecter.
- Est le PC sur un réseau différent ? Vous n’avez configuré le PC pour permettre à l’extérieur des connexions via ?  Découvrez [autoriser l’accès à votre PC à partir de votre réseau](remote-desktop-allow-outside-access.md) de l’aide.
- Vous connectez à une version de Windows prise en charge ? 

   > [!NOTE]
   > Windows XP Édition familiale, Windows Media Center Edition, Windows Vista Édition familiale et Windows 7 Édition familiale ou Starter ne sont pas pris en charge sans 3e logiciels tiers.

### <a name="why-cant-i-sign-in-to-a-remote-pc"></a>Pourquoi ne puis-je pas me connecter à un ordinateur distant ?
Si vous voyez l’écran de connexion de l’ordinateur distant, mais vous ne pouvez pas vous connecter, vous ne pouvez pas ont été ajoutées pour le groupe utilisateurs du Bureau à distance ou à un groupe disposant des droits d’administrateur sur un ordinateur distant. Demandez à votre administrateur système pour effectuer cette opération pour vous.

### <a name="which-connection-methods-are-supported-for-company-networks"></a>Les méthodes de connexion sont prises en charge pour les réseaux d’entreprise ?
Si vous souhaitez accéder à votre bureau d’office à partir de votre réseau d’entreprise, votre entreprise doit vous fournir un moyen d’accès à distance. Le Client Bureau à distance prend actuellement en charge les éléments suivants :

- Passerelle de serveur Terminal Server ou de la passerelle des services Bureau à distance
- Accès Web Bureau à distance
- Réseau privé virtuel (par les options intégrées de VPN iOS)

### <a name="vpn-doesnt-work"></a>VPN ne fonctionne pas

Problèmes de VPN peuvent avoir plusieurs causes. La première étape consiste à vérifier que la connexion VPN fonctionne sur le même réseau que votre ordinateur PC ou Mac. Si vous ne pouvez pas tester avec un PC ou un Mac, vous pouvez tenter d’accéder à une page web d’intranet entreprise avec le navigateur de votre appareil.

Autres points à vérifier :
- **Le réseau 3G bloque ou la corruption de VPN.** Il existe plusieurs fournisseurs de 3G dans le monde qui semblent bloc ou endommagé 3G le trafic. Vérifiez la connectivité VPN fonctionne correctement pour par minute.
- **L2TP ou PPTP VPN.** Si vous utilisez L2TP ou PPTP dans votre réseau privé virtuel, définissez envoyer tout trafic on dans la configuration du VPN.
- **VPN est mal configuré.** Un serveur VPN mal configuré peut être la raison pour laquelle les connexions VPN a fonctionné ou a cessé de fonctionner après un certain temps de jamais. Vérifiez que test avec iOS navigateur web de l’appareil ou d’un PC ou Mac sur le réseau même si cela se produit.

### <a name="how-can-i-test-if-vpn-is-working-properly"></a>Comment puis-je tester si VPN fonctionne correctement ?
Vérifiez que le VPN est activé sur votre appareil. Vous pouvez tester votre connexion VPN en accédant à une page Web sur votre réseau interne ou à l’aide d’un service web qui est uniquement disponible via le VPN.

### <a name="how-do-i-configure-l2tp-or-pptp-vpn-connections"></a>Comment configurer les connexions L2TP ou PPTP VPN ?
Si vous utilisez L2TP ou PPTP dans votre réseau privé virtuel, veillez à définir **envoyer tout le trafic** à **ON** dans la configuration du VPN.

## <a name="web-client"></a>client Web

### <a name="which-browsers-can-i-use"></a>Quels navigateurs puis-je utiliser ?

Le client web prend en charge Microsoft Edge, Internet Explorer 11, Mozilla Firefox (v55.0 et versions ultérieures), Safari et Google Chrome.

### <a name="what-pcs-can-i-use-to-access-the-web-client"></a>Quels ordinateurs puis-je utiliser pour accéder au client web ?

Le client web prend en charge Windows, macOS, Linux et ChromeOS. Les appareils mobiles ne sont pas pris en charge pour l’instant.

### <a name="can-i-use-the-web-client-in-a-remote-desktop-deployment-without-a-gateway"></a>Puis-je utiliser le client web dans un déploiement de bureau à distance sans une passerelle ?

Non. Le client nécessite une passerelle des services Bureau à distance pour se connecter. Ignorez ce que cela signifie ? À ce sujet, contactez votre administrateur.

### <a name="does-the-remote-desktop-web-client-replace-the-remote-desktop-web-access-page"></a>Le client web de bureau à distance remplace-t-il la page d’accès Bureau à distance par le Web ?

Non. Le client web de bureau à distance est hébergé à une URL différente de la page d’accès Bureau à distance par le Web. Vous pouvez utiliser le client web ou la page Web Access pour afficher les ressources à distance dans un navigateur.

### <a name="can-i-embed-the-web-client-in-another-web-page"></a>Puis-je intégrer le client web dans une autre page web ?

Cette fonctionnalité n’est pas pris en charge pour le moment.

## <a name="monitors-audio-and-mouse"></a>Moniteurs, audio et la souris

### <a name="how-do-i-use-all-of-my-monitors"></a>Comment utiliser tous les moniteurs ?
Pour utiliser deux ou plusieurs écrans, procédez comme suit :

1. Cliquez sur le Bureau à distance que vous souhaitez activer plusieurs écrans pour, puis cliquez sur **modifier**.
2. Activer **utiliser toutes les analyses** et **plein écran**.

### <a name="is-bi-directional-sound-supported"></a>Signal sonore bidirectionnel est pris en charge ?
Son en amont (à partir de client à serveur, pour des microphones) n’est pas pris en charge par le Client Bureau à distance.

### <a name="what-can-i-do-if-the-sound-wont-play"></a>Que puis-je faire si ne lit pas le son ?
Déconnectez-vous de la session (ne simplement déconnecter, déconnecter complètement), puis connectez-vous à nouveau.

## <a name="mac-client---hardware-questions"></a>Client Mac - questions sur le matériel
### <a name="is-retina-resolution-supported"></a>Résolution de la rétine est pris en charge ?
Oui, le client Bureau à distance prend en charge la résolution retina.

### <a name="how-do-i-enable-secondary-right-click"></a>Comment activer secondaire avec le bouton droit ?
Pour utiliser de clic droit à l’intérieur d’une session ouverte, vous avez trois possibilités :

- Souris USB de bouton standard PC deux
- Souris magique Apple : Pour activer le clic droit, cliquez sur **Préférences système** dans la station d’accueil, cliquez sur **souris**, puis activez **cliquez secondaire**.
- Pavé tactile magique Apple ou le pavé tactile MacBook : Pour activer le clic droit, cliquez sur **Préférences système** dans la station d’accueil, cliquez sur **souris**, puis activez **cliquez secondaire**.

### <a name="is-airprint-supported"></a>AirPrint est pris en charge ?
Non, le client Bureau à distance ne prend en charge AirPrint. (Cela est vrai pour les clients Mac et iOS).

### <a name="why-do-incorrect-characters-appear-in-the-session"></a>Pourquoi des caractères incorrects n’apparaissent pas dans la session ?
Si vous utilisez un clavier, vous pouvez voir un problème où les caractères qui apparaissent dans la session de correspondre les caractères que vous avez tapé sur le clavier Mac.

Cela peut se produire dans les scénarios suivants :

- Vous utilisez un clavier de la session à distance ne reconnaît pas. Lorsque le Bureau à distance ne reconnaît pas le clavier, la valeur par défaut est la langue utilisée en dernier avec l’ordinateur distant.
- Vous vous connectez à une session déconnectée précédemment sur un PC distant et que PC à distance utilise un langage de clavier autre que la langue actuellement souhaitez-vous utiliser.

Vous pouvez résoudre ce problème en définissant manuellement la langue du clavier pour la session à distance. Consultez les étapes décrites dans la section suivante.

### <a name="how-do-language-settings-affect-keyboards-in-a-remote-session"></a>Paramètres de langue répercussions des claviers dans une session à distance ?
Il existe de nombreux types de dispositions de clavier Mac. Certaines d'entre elles sont les dispositions spécifiques Mac ou pour lesquels une correspondance exacte peut être pas disponible sur la version de Windows sont de communication à distance dans des dispositions personnalisées. La session à distance mappe votre clavier pour la langue du clavier disponible sur un ordinateur distant le plus adapté. 

Si la disposition du clavier Mac est définie sur la version PC du clavier en langue (par exemple, Français – PC) toutes vos clés doivent être mappés correctement et de votre clavier devrait fonctionner.

Si la disposition du clavier Mac est définie sur la version Mac de clavier (par exemple, Français) la session à distance vous mappera vers la version de PC de la langue Français. Certains des raccourcis clavier Mac que vous êtes habitué à l’utilisation sur OSX ne fonctionnera pas dans la session à distance de Windows.

Si la disposition du clavier est définie sur une variation d’une langue (par exemple, Français (Canada)) et si la session à distance ne peut pas vous mappez à cette variante exacte, la session à distance mappera vous pour le langage le plus proche (par exemple, Français). Certains des raccourcis clavier Mac que vous êtes habitué à l’utilisation sur OSX ne fonctionnera pas dans la session à distance de Windows.

Si la disposition du clavier est définie sur une disposition de que la session à distance ne peut pas correspondre à tout, votre session à distance par défaut afin de vous donner la langue que ceux utilisés avec ce PC. Dans ce cas, ou dans les cas où vous devez modifier la langue de votre session à distance pour faire correspondre votre clavier Mac, vous pouvez définir manuellement la langue du clavier dans la session à distance à la langue qui correspond le mieux à celui que vous souhaitez utiliser comme suit.

Utilisez les instructions suivantes pour modifier la disposition du clavier à l’intérieur de la session Bureau à distance :

**Sur Windows 10 ou Windows 8 :**

1. À partir d’à l’intérieur de la session à distance, ouvrez région et langue. Cliquez sur **Démarrer > Paramètres > heure et langue**. Ouvrez **région et langue**.
2. Ajoutez la langue que vous souhaitez utiliser. Puis fermez la fenêtre de la région et langue.
3. Maintenant, dans la session à distance, vous verrez la possibilité de basculer entre les langages. (Dans la partie droite de la session à distance, près de l’horloge.) Cliquez sur le langage que vous souhaitez basculer vers (tel que **Eng**).

Vous devrez peut-être fermer et redémarrer l’application que vous utilisez actuellement pour le clavier modifie entrent en vigueur.


## <a name="specific-errors"></a>Erreurs spécifiques

### <a name="why-do-i-get-an-insufficient-privileges-error"></a>Je reçois une erreur « Privilèges insuffisants » ?
Vous n’êtes pas autorisé à accéder à la session que vous souhaitez vous connecter. La cause la plus probable est que vous essayez de vous connecter à une session d’administrateur. Seuls les administrateurs sont autorisés à se connecter à la console. Vérifiez que le commutateur de console est désactivée dans les paramètres avancés du Bureau à distance. S’il ne s’agit pas de la source du problème, contactez votre administrateur système pour obtenir une assistance supplémentaire.

### <a name="why-does-the-client-say-that-there-is-no-cal"></a>Pourquoi le client indique qu’il n’existe aucune licence d’accès client ?
Lorsqu’un client Bureau à distance se connecte à un serveur Bureau à distance, le serveur émet un distant Desktop Services de licence d’accès Client (RDS) stockées par le client. Chaque fois que le client se connecte à nouveau, il utilisera sa CAL RDS et le serveur ne sera pas émettre une autre licence. Le serveur émet une autre licence si la CAL de services Bureau à distance sur l’appareil est manquant ou endommagé. Lorsque le nombre maximal d’appareils sous licence est atteint le serveur ne génère pas de nouvelles licences. Pour obtenir une assistance, contactez votre administrateur réseau.

### <a name="why-did-i-get-an-access-denied-error"></a>Pourquoi ai-je reçu une erreur « Accès refusé » ?
L’erreur « Accès refusé » est un généré par la passerelle des services Bureau à distance et le résultat des informations d’identification incorrectes lors de la tentative de connexion. Vérifiez votre nom d’utilisateur et le mot de passe. Si la connexion a fonctionné avant et l’erreur s’est produite récemment, vous éventuellement modifié votre mot de passe du compte utilisateur Windows et n’avez pas encore mis à jour il dans les paramètres du Bureau à distance.

### <a name="what-does-rpc-error-23014-or-error-0x59e6-mean"></a>Quel est le « Erreur RPC 23014 » ou la moyenne de « Erreur 0x59e6 » ?
Dans le cas d’un **erreur RPC 23014** ou **erreur 0x59E6, puis réessayez attendre quelques minutes**, le serveur de passerelle Bureau à distance a atteint le nombre maximal de connexions actives. Selon la version de Windows en cours d’exécution sur la passerelle Bureau à distance par le nombre maximal de connexions est différent : L’implémentation de Windows Server 2008 R2 Standard limite le nombre de connexions à 250. L’implémentation de Windows Server 2008 R2 Foundation limite le nombre de connexions à 50. Toutes les autres implémentations de Windows permettent à un nombre illimité de connexions.

### <a name="what-does-the-failed-to-parse-ntlm-challenge-error-mean"></a>Que signifie l’erreur « Impossible d’analyser la stimulation NTLM » ?
Cette erreur est due à une mauvaise configuration sur un ordinateur distant. Assurez-vous que le paramètre de niveau de sécurité RDP sur un ordinateur distant est défini « Compatible Client ». (Contactez votre administrateur système si vous avez besoin d’aide pour cette opération.)

### <a name="what-does-tsrap-you-are-not-allowed-to-connect-to-the-given-host-mean"></a>Ce que fait « TS_RAP vous n’êtes pas autorisé à se connecter à l’hôte donné » signifie ?
Cette erreur se produit lorsqu’une stratégie d’autorisation de ressource sur le serveur de passerelle arrête votre nom d’utilisateur de se connecter à l’ordinateur distant. Cela peut se produire dans les cas suivants :

- Le nom de PC à distance est le même que le nom de la passerelle. Puis, lorsque vous essayez de vous connecter à l’ordinateur distant, la connexion va à la passerelle à la place, qui ne présentent pas autorisé à y accéder. Si vous avez besoin pour vous connecter à la passerelle, n’utilisez pas le nom de passerelle externe en tant que nom du PC. À la place utiliser « localhost » ou l’adresse IP (127.0.0.1) ou le nom du serveur interne.
- Votre compte d’utilisateur n’est pas un membre du groupe d’utilisateurs pour l’accès à distance.