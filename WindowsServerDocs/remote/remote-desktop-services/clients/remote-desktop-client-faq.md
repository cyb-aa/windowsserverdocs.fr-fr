---
title: FAQ sur les clients Bureau à distance
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
ms.openlocfilehash: e6f91aa02cd0f19d480c24309be5797c273b0f2e
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/17/2019
ms.locfileid: "66804952"
---
# <a name="frequently-asked-questions-about-the-remote-desktop-clients"></a>Forum aux questions sur les clients Bureau à distance

>S’applique à : Windows 10, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

Une fois que vous avez configuré le client Bureau à distance sur votre appareil (Android, Mac, iOS ou Windows), il peut vous rester quelques questions. Vous trouverez ici les réponses aux questions les plus fréquemment posées sur les clients Bureau à distance. 

- [Configuration](#setting-up)
- [Connexions, passerelle et réseaux](#connection-gateway-and-networks)
- [Client web](#web-client)
- [Moniteurs, audio et souris](#monitors-audio-and-mouse)
- [Matériel Mac](#mac-client---hardware-questions)
- [Messages d’erreur spécifiques](#specific-errors)

La majorité de ces questions s'appliquent à tous les clients, mais certaines d’entre-elles sont propres à certains clients.

Si vous voulez nous poser d’autres questions, ajoutez-les dans les commentaires de cet article.

## <a name="setting-up"></a>Configuration

### <a name="which-pcs-can-i-connect-to"></a>À quels PC puis-je me connecter ?

Merci de consulter l’article [Client Bureau à distance - configuration prise en charge](remote-desktop-supported-config.md) : il contient toutes les informations nécessaires pour savoir à quels PC vous pouvez vous connecter.

### <a name="how-do-i-set-up-a-pc-for-remote-desktop"></a>Comment configurer un PC pour le Bureau à distance ?

Mon appareil est configuré, mais je pense que le PC n’est pas encore prêt. Pourriez-vous m’aider ?

Tout d’abord, avez-vous consulté l’Assistant Configuration du Bureau à distance ? Il vous guide durant la préparation de votre PC pour l’accès à distance. Téléchargez et exécutez cet utilitaire sur votre PC : il se charge de tout régler. 

Cependant, si vous préférez procéder manuellement, n’hésitez pas à lire ce qui suit.

Pour Windows 10, procédez comme suit :

1. Dans l’appareil auquel vous souhaitez vous connecter, ouvrez **Paramètres**.
2. Sélectionnez **Système**, puis **Bureau à distance**.
3. Utilisez le curseur pour activer le Bureau à distance.
4. Généralement, il est recommandé de garder le PC allumé et détectable pour faciliter les connexions. Cliquez sur **Afficher les paramètres** pour accéder aux paramètres d’alimentation de votre PC, où vous pouvez modifier ce paramètre.
   > [!NOTE]
   > Vous ne pouvez pas vous connecter à un PC qui est en veille ou en veille prolongée. Par conséquent, vérifiez que les paramètres de mise en veille et veille prolongée sur un PC distant sont définis sur **Jamais**. Remarque : la mise en veille prolongée n’est pas disponible sur tous les PC.


Veuillez noter le nom de ce PC sous **Comment se connecter à ce PC**. Vous en aurez besoin pour configurer les clients.

Vous pouvez autoriser certains utilisateurs à accéder à ce PC : pour ce faire, cliquez sur **Sélectionner les utilisateurs qui peuvent accéder à distance à ce PC**.
Les membres du groupe Administrateurs ont automatiquement accès.

Pour Windows 8.1, suivez les instructions pour autoriser les connexions à distance dans [Se connecter à un autre ordinateur à l’aide de Connexion Bureau à distance](https://support.microsoft.com/en-us/help/17463/windows-7-connect-to-another-computer-remote-desktop-connection#1TC=windows-8).



## <a name="connection-gateway-and-networks"></a>Connexion, passerelle et réseaux

### <a name="why-cant-i-connect-using-remote-desktop"></a>Pourquoi ne puis-je pas me connecter à l’aide du Bureau à distance ?

Voici quelques solutions possibles aux problèmes courants que vous pouvez rencontrer lorsque vous tentez de vous connecter à un PC distant. Si ces solutions ne fonctionnent pas, vous trouverez plus d’informations sur le [site web de Microsoft Community](https://go.microsoft.com/fwlink/p/?LinkId=242079).

- **Impossible de trouver un PC distant.** Assurez-vous d’avoir le nom de PC adéquat, puis vérifiez si vous avez entré ce nom correctement. Si vous ne pouvez toujours pas vous connecter, essayez d’utiliser l’adresse IP du PC distant au lieu de son nom.
- **Un problème de réseau est survenu.** Vérifiez que vous disposez d’une connexion à Internet. 
- **Le port du Bureau à distance peut être bloqué par un pare-feu.** Si vous utilisez un pare-feu Windows, procédez comme suit :

  1. Ouvrez le pare-feu Windows. 
  2. Cliquez sur **Autoriser une application ou une fonctionnalité via le Pare-feu Windows**. 
  3. Cliquez sur **Modifier les paramètres**. Vous devrez peut-être saisir votre mot de passe administrateur ou confirmer votre choix.
  4. Sous **Applications et fonctionnalités autorisées**, sélectionnez **Bureau à distance**, puis appuyez ou cliquez sur **OK**.

     Si vous utilisez un autre pare-feu, assurez-vous que le port Bureau à distance (généralement 3389) est ouvert.
- **La fonctionnalité Connexion à distance ne peut pas être configurée sur le PC distant.** Pour résoudre ce problème, revenez à la question [Comment configurer un PC pour le Bureau à distance ?](#how-do-i-set-up-a-pc-for-remote-desktop) de cette rubrique.
- **Le PC à distance peut autoriser uniquement la connexion des PC dont l’authentification au niveau du réseau est configurée.** 
- **Le PC à distance est peut-être désactivé.** Vous ne pouvez pas vous connecter à un PC qui est désactivé, en veille, ou en veille prolongée. Par conséquent, vérifiez que les paramètres de mise en veille et veille prolongée sur un PC distant sont définis sur **Jamais**. Remarque : la mise en veille prolongée n’est pas disponible pour tous les PC.

### <a name="why-cant-i-find-or-connect-to-my-pc"></a>Pourquoi ne puis-je pas trouver mon PC ou m’y connecter ?

Vérifiez les points suivants :

- Le PC est-il activé et en cours d’utilisation ?
- Avez-vous entré son nom ou son adresse IP correctement ?

   > [!IMPORTANT]
   > Si vous indiquez un nom de PC pour établir une connexion, cela implique que votre réseau puisse le résoudre correctement via le DNS. Dans la plupart des réseaux domestiques, vous devez utiliser l’adresse IP au lieu du nom d’hôte pour vous connecter.
- Le PC est-il sur un réseau différent ? Avez-vous configuré le PC pour permettre les connexions externes ?  Si vous souhaitez obtenir plus d’informations, veuillez consulter l’article [Bureau à distance - autoriser l’accès à votre PC hors de votre réseau](remote-desktop-allow-outside-access.md).
- Êtes-vous connecté à une version de Windows prise en charge ? 

   > [!NOTE]
   > La prise en charge de Windows XP Famille, Windows Media Center Edition, Windows Vista Famille et Windows 7 Famille ou Starter requiert un logiciel tiers.

### <a name="why-cant-i-sign-in-to-a-remote-pc"></a>Pourquoi ne puis-je pas me connecter à un PC distant ?

Si vous voyez l’écran de connexion du PC distant, mais que vous ne pouvez pas vous connecter, cela signifie peut-être que vous n’avez pas été ajouté au groupe d’utilisateurs de Bureau à distance ou que vous ne faites pas partie d’un groupe disposant des droits d’administrateur sur un PC distant. Veuillez demander à votre administrateur système d’effectuer cette opération pour vous.

### <a name="which-connection-methods-are-supported-for-company-networks"></a>Quelles méthodes de connexion sont prises en charge par les réseaux d’entreprises ?

Si vous souhaitez accéder à un poste de travail situé dans les locaux de votre entreprise alors que vous vous trouvez à l’extérieur du réseau de votre d’entreprise, cette dernière doit vous fournir un moyen d’accès à distance. Le client Bureau à distance prend actuellement en charge les éléments suivants :

- Passerelle des services Terminal Server ou passerelle des services Bureau à distance
- Accès web au Bureau à distance
- Réseau privé virtuel (via les options de VPN intégrées d’iOS)

### <a name="vpn-doesnt-work"></a>Le VPN ne fonctionne pas

Les problèmes de VPN peuvent avoir plusieurs causes. Dans ce cas, vous devez commencer par vérifier que la connexion VPN fonctionne sur le même réseau que votre PC ou votre Mac. Si vous ne pouvez pas procéder à un test avec un PC ou un Mac, vous pouvez tenter d’accéder à une page web de l’intranet de votre entreprise avec le navigateur de votre appareil.

Autres points à vérifier :
- **Le réseau 3G bloque ou endommage le VPN.** Certains fournisseurs de 3G dans le monde semblent bloquer ou endommager le trafic 3G. Vérifiez que la connectivité VPN fonctionne correctement pendant au moins une minute.
- **VPN L2TP ou PPTP.** Si vous utilisez L2TP ou PPTP dans votre réseau privé virtuel, lors de la configuration du VPN, veuillez définir le paramètre Envoyer tout le trafic sur Activé.
- **Le VPN est incorrectement configuré.** Une erreur de configuration d’un serveur VPN peut être la raison pour laquelle les connexions VPN ne fonctionnent pas ou cessent de fonctionner après un certain temps. Si cela se produit, veuillez procéder à un test avec le navigateur web du périphérique iOS ou un PC ou un Mac appartenant au même réseau.

### <a name="how-can-i-test-if-vpn-is-working-properly"></a>Comment puis-je tester le bon fonctionnement de mon VPN ?

Vérifiez que le VPN est activé sur votre appareil. Vous pouvez tester votre connexion VPN en accédant à une page web sur votre réseau interne ou à l’aide d’un service web qui est uniquement disponible via le VPN.

### <a name="how-do-i-configure-l2tp-or-pptp-vpn-connections"></a>Comment configurer les connexions VPN L2TP ou PPTP ?

Si vous utilisez une connexion L2TP ou PPTP dans votre réseau privé virtuel, veillez à définir **Envoyer tout le trafic** sur **Activé** dans la configuration du VPN.

## <a name="web-client"></a>Client web

### <a name="which-browsers-can-i-use"></a>Quels navigateurs puis-je utiliser ?

Le client web prend en charge Microsoft Edge, Internet Explorer 11, Mozilla Firefox (55.0 et versions ultérieures), Safari et Google Chrome.

### <a name="what-pcs-can-i-use-to-access-the-web-client"></a>Quels PC puis-je utiliser pour accéder au client web ?

Le client web prend en charge Windows, macOS, Linux et ChromeOS. Il ne prend pas en charge les appareils mobiles pour l’instant.

### <a name="can-i-use-the-web-client-in-a-remote-desktop-deployment-without-a-gateway"></a>Puis-je utiliser le client web dans un déploiement de bureau à distance sans une passerelle ?

Non. Le client nécessite une passerelle des services Bureau à distance pour se connecter. Vous ne savez pas ce que cela signifie ? Si vous avez besoin de plus d’informations à ce sujet, veuillez contacter votre administrateur.

### <a name="does-the-remote-desktop-web-client-replace-the-remote-desktop-web-access-page"></a>Le client web du service Bureau à distance remplace-t-il la page Accès web au Bureau à distance ?

Non. Le client web du service Bureau à distance est hébergé sur une URL différente de la page Accès web au Bureau à distance. Vous pouvez utiliser le client web ou la page Accès web pour afficher les ressources à distance dans un navigateur.

### <a name="can-i-embed-the-web-client-in-another-web-page"></a>Puis-je intégrer le client web dans une autre page web ?

Cette fonctionnalité n’est pas prise en charge pour le moment.

## <a name="monitors-audio-and-mouse"></a>Moniteurs, audio et souris

### <a name="how-do-i-use-all-of-my-monitors"></a>Comment puis-je utiliser tous les mes moniteurs ?
Pour utiliser plus de deux écrans, procédez comme suit :

1. Faites un clic droit sur le Bureau à distance pour lequel vous souhaitez activer plusieurs écrans, puis cliquez sur **Modifier**.
2. Activez **Utiliser tous les moniteurs** et **Plein écran**.

### <a name="is-bi-directional-sound-supported"></a>Le signal sonore bidirectionnel est-il pris en charge ?
Le traitement du son dans le sens ascendant (de client à serveur, pour des microphones) n’est pas pris en charge par le client Bureau à distance.

### <a name="what-can-i-do-if-the-sound-wont-play"></a>Que puis-je faire si le son n’est pas lu ?
Déconnectez-vous de votre session (procédez à une déconnexion complète), puis reconnectez-vous.

## <a name="mac-client---hardware-questions"></a>Questions sur le matériel requis pour un client Mac
### <a name="is-retina-resolution-supported"></a>La résolution d’écran Retina est-elle prise en charge ?
Oui.

### <a name="how-do-i-enable-secondary-right-click"></a>Comment activer le clic droit (ou « clic secondaire ») ?
Pour utiliser le clic droit à l’intérieur d’une session ouverte, vous avez trois possibilités :

- Utiliser une souris USB standard à deux boutons
- Utiliser la Magic Mouse d’Apple : dans ce cas, pour activer le clic droit, cliquez sur **Préférences système** dans le Dock, cliquez sur **Souris**, puis activez **Clic secondaire**.
- Utiliser le Magic Trackpad ou le MacBook Trackpad d’Apple : dans ce cas, pour activer le clic droit, cliquez sur **Préférences système** dans le Dock, cliquez sur **Souris**, puis activez **Clic secondaire**.

### <a name="is-airprint-supported"></a>AirPrint est pris en charge ?
Non. Cette limitation s’applique aux clients Mac et iOS.

### <a name="why-do-incorrect-characters-appear-in-the-session"></a>Pourquoi des caractères incorrects n’affichent-ils pendant la session ?
Si vous utilisez un clavier international, il est possible que le problème suivant survienne : les caractères qui s’affichent durant votre session ne correspondent pas aux caractères que vous avez tapés sur le clavier Mac.

Cela peut se produire dans les scénarios suivants :

- Vous utilisez un clavier que la session à distance ne reconnaît pas. Lorsque le Bureau à distance ne reconnaît pas le clavier, il utilise comme langue de saisie par défaut la dernière langue définie pour le PC distant.
- Vous vous connectez à une session déconnectée précédemment sur un PC distant et ce PC à distance utilise une langue de saisie différente de celle que vous tentez d’utiliser.

Vous pouvez résoudre ce problème en définissant manuellement la langue de saisie pour la session à distance. Si vous souhaitez en savoir plus à ce sujet, veuillez consulter la procédure décrite dans la section suivante.

### <a name="how-do-language-settings-affect-keyboards-in-a-remote-session"></a>Comment les paramètres de langue affectent-ils les claviers d’une session distante ?
Il existe de nombreux types de dispositions de clavier Mac. Certaines d'entre elles sont des dispositions propres aux Mac ou des dispositions personnalisées, pour lesquelles il n’est pas toujours possible d’établir une correspondance exacte sur la version de Windows dans laquelle vous effectuez la communication à distance. La session à distance mappe votre clavier vers la langue du clavier la plus adaptée sur le PC distant. 

Si la disposition de votre clavier Mac est définie sur une version de clavier PC d’une même langue de saisie (par exemple, Français – PC) toutes vos touches seront probablement mappées correctement et votre clavier devrait fonctionner.

Si la disposition de votre clavier Mac est définie sur une version de clavier Mac d’une même langue de saisie (par exemple, Français) la session à distance vous mappera vers la version de PC de la langue de saisie (Français). Certains raccourcis clavier Mac que vous avez l'habitude d'utiliser sous OSX ne fonctionneront pas dans la session Windows distante.

Si la disposition de votre clavier est définie sur une variante d’une langue (par exemple, le français canadien) et si la session distante ne peut pas vous associer exactement à cette variante, la session distante vous affectera à la langue la plus proche (par exemple, le français). Certains raccourcis clavier Mac que vous avez l'habitude d'utiliser sous OSX ne fonctionneront pas dans la session Windows distante.

Si la disposition de votre clavier est définie sur une disposition à laquelle la session distante ne peut pas du tout correspondre, cette dernière vous attribuera par défaut la langue que vous avez utilisée en dernier sur ce PC. Dans ce cas, ou dans les cas où vous devez changer la langue de votre session distante pour qu'elle corresponde à celle de votre clavier Mac, vous pouvez régler manuellement la langue du clavier de la session distante dans la langue qui correspond le mieux à celle que vous souhaitez utiliser, comme suit.

Suivez cette procédure pour modifier la disposition du clavier de la session Bureau à distance :

**Sur Windows 10 ou Windows 8 :**

1. Dans le PC à distance, ouvrez Région et langue. Cliquez sur **Démarrer > Paramètres > Heure et langue**. Ouvrez **Région et langue**.
2. Ajoutez la langue que vous souhaitez utiliser. Puis fermez la fenêtre Région et langue.
3. Maintenant, dans la session à distance, vous pourrez basculer d’une langue à l’autre. Cette option est disponible dans la partie droite de la session à distance, près de l’horloge. Cliquez sur la langue vers laquelle vous souhaitez basculer (par exemple : **Eng**).

Vous devrez peut-être fermer et redémarrer l’application que vous utilisez actuellement pour que la modification du clavier entre en vigueur.


## <a name="specific-errors"></a>Erreurs spécifiques

### <a name="why-do-i-get-an-insufficient-privileges-error"></a>Pourquoi est-ce que je reçois une erreur « Privilèges insuffisants » ?
Vous n’êtes pas autorisé à accéder à la session à laquelle vous souhaitez vous connecter. La cause la plus probable est que vous essayez de vous connecter à une session d’administrateur. Seuls les administrateurs sont autorisés à se connecter à la console. Vérifiez que le commutateur de console est désactivé dans les paramètres avancés du Bureau à distance. Si le problème est ailleurs, veuillez contacter votre administrateur système pour lui demander de l’aide.

### <a name="why-does-the-client-say-that-there-is-no-cal"></a>Pourquoi le client indique qu’il n’existe aucune licence d’accès client (CAL) ?
Lorsqu’un client Bureau à distance se connecte à un serveur Bureau à distance, le serveur émet une licence d’accès client aux services Bureau à distance (RDS CAL) stockée par le client. À chaque nouvelle connexion du client, il utilise sa licence d’accès et le serveur n’émet pas d’autre licence. Le serveur émet une autre licence si la licence d’accès client aux services Bureau à distance n’est plus détectée sur l’appareil ou est endommagée. Lorsque le nombre maximal d’appareils sous licence est atteint, le serveur ne génère pas de nouvelles licences. Si vous avez besoin d’aide supplémentaire, veuillez contacter votre administrateur réseau.

### <a name="why-did-i-get-an-access-denied-error"></a>Pourquoi ai-je reçu une erreur « Accès refusé » ?
L’erreur « Accès refusé » est générée par la passerelle des services Bureau à distance. Elle est due à la saisie d’informations identification incorrectes lors de la tentative de connexion. Veuillez vérifier votre nom d’utilisateur et votre mot de passe. Si connexion a déjà fonctionné et que l’erreur s’est produite récemment, il est possible que vous ayez modifié le mot de passe de votre compte d’utilisateur Windows, mais que vous ne l’ayez pas encore mis à jour dans les paramètres du Bureau à distance.

### <a name="what-does-rpc-error-23014-or-error-0x59e6-mean"></a>Que signifient les codes « RPC Error 23014 » ou « Error 0x59e6 » ?
Si une erreur de type **RPC 23014** ou **0x59E6** survient, veuillez attendre quelques minutes, puis réessayez de vous connecter. Ces erreurs signifient généralement que le nombre maximal de connexions actives prises en charge par le serveur de la passerelle des services Bureau à distance est atteint. Cette limite varie selon la version de Windows en cours d’exécution sur la passerelle des services Bureau à distance : Pour l’implémentation Windows Server 2008 R2 Standard, le nombre maximal de connexions actives est de 250. Pour l’implémentation Windows Server 2008 R2 Foundation, le nombre maximal de connexions actives est de 50. Toutes les autres implémentations de Windows permettent un nombre illimité de connexions.

### <a name="what-does-the-failed-to-parse-ntlm-challenge-error-mean"></a>Que signifie l’erreur « Impossible d’analyser le test NTLM » ?
Cette erreur est due à une erreur de configuration sur un PC distant. Assurez-vous que le paramètre de niveau de sécurité RDP du PC distant est défini sur « Client compatible ». Si vous avez besoin d’aide supplémentaire, veuillez contacter votre administrateur système.

### <a name="what-does-tsrap-you-are-not-allowed-to-connect-to-the-given-host-mean"></a>Que signifie « TS_RAP Vous n'êtes pas autorisé à vous connecter à l'hôte donné » ?
Cette erreur se produit lorsqu’une stratégie d’autorisation d’accès aux ressources sur le serveur de la passerelle arrête la connexion de votre nom d’utilisateur au PC distant. Cela peut se produire dans les cas suivants :

- Le nom du PC à distance est le même que le nom de la passerelle. Puis, lorsque vous essayez de vous connecter au PC distant, la connexion s’établit plutôt vers la passerelle, et vous ne disposez probablement pas des autorisations requises pour une telle connexion. Si vous devez pour vous connecter à la passerelle, n’utilisez pas le nom de la passerelle externe en tant que nom de PC. Veuillez plutôt utiliser « localhost » ou l’adresse IP (127.0.0.1) ou le nom du serveur interne.
- Votre compte d’utilisateur n’est pas membre du groupe d’utilisateurs autorisés à utiliser l’accès à distance.