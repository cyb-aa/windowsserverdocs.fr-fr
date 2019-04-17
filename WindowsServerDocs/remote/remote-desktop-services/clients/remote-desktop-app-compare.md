---
title: Services Bureau à distance - comparer les applications clientes
description: Découvrez comment les différentes applications de bureau à distance comparer en matière de fonctions et fonctionnalités prises en charge.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 12efe858-6b76-4e08-9f72-b9603aceb0fc
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 06/22/2018
ms.localizationpriority: medium
ms.openlocfilehash: fc20d1a2c51abddb8ae014efc621f4f0b36c3677
ms.sourcegitcommit: d3f73936160505a40633ad8dd5931ac5fe3eccdb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/12/2019
ms.locfileid: "9297408"
---
# Comparer les applications clientes

>S’applique à: Windows 10, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

Nous allons souvent invité à indiquer comment les différentes applications de client Bureau à distance par rapport à l’autre. Font tous la même chose? Voici les réponses à ces questions.

## Prise en charge de la redirection

Les tableaux suivants comparent prise en charge des appareils et des autres redirections sur l’application de connexion Bureau à distance, application universelle, application Android, application iOS, le client d’application et web Mac OS. Les tableaux suivants couvrent les redirections que vous pouvez accéder à la fois dans une session à distance. 

Si vous à distance à votre bureau personnel, il existe des redirections supplémentaires que vous pouvez configurer dans les **Paramètres supplémentaires** pour la session. Si votre Bureau à distance ou les applications sont gérées par votre organisation, votre administrateur peut activer ou désactiver les redirections par le biais des paramètres de stratégie de groupe.

### Redirection d’entrée

| Redirection | Bureau à distance<br> Connexion | Universel | Android | iOS | macOS | client Web |
|-------------|-------------------------------|-----------|---------|-----|-------|------------|
| Clavier    | X                             | X         | X       | X   | X     | X          |
| Souris       | X                             | X         | X       | X *    | X     | X          |
| Commandes tactiles       | X                             | X         | X       | X   |       |            |
| Other       | Stylet                           |           |         |     |       |            |
* Permet d’afficher la [liste des périphériques d’entrée pris en charge pour le client de bêta iOS Bureau à distance](remote-desktop-ios.md#supported-input-devices).

### Redirection de port   

| Redirection | Bureau à distance <br>Connexion | Universel | Android | iOS | macOS | Client Web |
|-------------|-------------------------------|-----------|---------|-----|-------|------------|
| Port série | X                             |           |         |     |       |            |
| USB         | X                             |           |         |     |       |            |

Lorsque vous activez la redirection de ports USB, tous les périphériques USB au port USB sont automatiquement reconnus dans la session à distance.

### Autres redirection (appareils, etc.)



| Redirection         | Connexion Bureau à distance | Universel   | Android | iOS         | macOS                                    | Client Web    |
|---------------------|---------------------------|-------------|---------|-------------|------------------------------------------|---------------|
| Appareils photo             | X                         |             |         |             |                                          |               |
| Presse-papiers           | X                         | texte, d’image | texte    | texte, d’image | X                                        | texte          |
| Disque/stockage local | X                         |             | X       |             | x                                        |               |
| Services de localisation            | X                         |             |         |             |                                          |               |
| Microphones         | X                         |X            |         |             | X                                        |               |
| Imprimantes            | X                         |             |         |             | X (tasses uniquement)                            | Impression PDF     |
| Scanneurs            | X                         |             |         |             |                                          |               |
| Cartes à puce         | X                         |             |         |             | X (non pris en charge l’authentification Windows) |               |
| Speakers            | X                         | X           | X       | X           | X                                        | X (à l’exception d’Internet Explorer) |

* La redirection d’imprimante - l’application macOS prend en charge le pilote d’imprimante ci-dessus par défaut. Elles ne gèrent pas la redirection des pilotes d’imprimante native.

### Paramètres RDP pris en charge
Le tableau ci-dessous comprend la liste des paramètres du fichier RDP pris en charge qui peut être utilisé avec les clients Windows et HTML. Un «x» dans la colonne de la plateforme indique que le paramètre est pris en charge. Veuillez noter qu’il ne s’agit pas d’une liste exhaustive des paramètres pris en charge pour les clients Windows et HTML5. Nous continuerons à mettre à jour de ce tableau pour inclure plus des paramètres RDP pris en charge pour les clients Windows et HTML5, ainsi que les Mac OS, iOS et Android clients.

| Paramètre RDP                        | Description            | Valeurs                 | Valeur par défaut          | Bureau virtuel Windows | Windows | HTML5   |
|------------------------------------|------------------------|------------------------|:----------------------:|:-----------------------:|:-------:|:-------:|
| autre adresse de complet: s:value | Spécifie un autre nom ou adresse IP de l’ordinateur distant. | N’importe quel nom valide ou l’adresse IP de l’ordinateur distant, par exemple, «10.10.15.15» | | x | x | x |
| interpréteur de commandes alternatif: s:value        | Détermine si un programme commence automatiquement lorsque vous vous connectez à l’aide du protocole RDP. Ce paramètre correspond à la programme chemin d’accès et de fichier zone Nom sous l’onglet programmes du client Connexion Bureau à distance. Pour spécifier un autre interpréteur de commandes, entrez un chemin d’accès valide à un fichier exécutable pour la valeur, par exemple ««C:\ProgramFiles\Office\word.exe»». Ce paramètre détermine également le chemin d’accès ou l’alias de l’Application distante à démarrer au moment de la connexion si RemoteApplicationMode est activé. | par exemple, «C:\ProgramFiles\Office\word.exe» || x | x | x |
| audiocapturemode:i:value | Indique si la redirection d’entrée/sortie audio est activée. Ce paramètre correspond à la zone de l’Audio à distance du client Connexion Bureau à distance. | (0) désactiver la capture audio à partir de l’appareil local; (1) activer la capture audio à partir de l’appareil local et la redirection vers une application audio dans la session à distance | 0 | x | x | |
| audiomode:i:value | Détermine quel ordinateur (c'est-à-dire, local ou distant) joue du contenu audio. | Lire des sons (0) sur l’ordinateur local (lire sur cet ordinateur); (1) lire des sons sur un ordinateur distant (s’exécute sur un ordinateur distant); (2) ne pas lire les sons (ne pas lire) | 0 | x | x | x |
| niveau d’authentification: i:value | Définit les paramètres au niveau du serveur d’authentification. | (0) en cas d’échec de l’authentification du serveur, connectez-vous à l’ordinateur sans avertissement (Connect et ne m’avertir); (1) en cas d’échec de l’authentification du serveur, ne pas établir une connexion (ne pas se connecter); (2) en cas d’échec de l’authentification du serveur, afficher un message d’avertissement et autoriser me connecter ou refuser la connexion (m’avertir); (3) aucune exigence d’authentification n’est spécifié. | 3 | x | x ||
| reconnexion automatique activée: i:value | Ce paramètre détermine si l’ordinateur client tente automatiquement de se reconnecter à l’ordinateur distant si la connexion est supprimée; par exemple, lorsqu’il existe une interruption de la connectivité réseau. Ce paramètre correspond à la se reconnecter si la connexion est déplacé case à cocher sous l’onglet expérience sous les Options de connexion de bureau à distance (RDC).| Ordinateur client (0) n’essaie pas automatiquement de se reconnecter; (1) ordinateur client tente automatiquement de se reconnecter| 1 | x | x | x |
| bandwidthautodetect:i:value | Détermine si la détection du type de réseau automatique est activée | Détection du type de réseau automatique désactiver (0); (1) activer la détection automatique du réseau type | 1 | x | x | x |
| compression: i:value | Ce paramètre détermine si la compression en bloc est activée quand elle est transmise par RDP sur l’ordinateur local.|Compression de bloc RDP désactiver (0); (1) activer la compression de bloc RDP | 1 | x | x | x |
| taille du bureau id: i:value | Spécifie les dimensions du bureau session à distance à partir d’un ensemble d’options prédéfinies. Ce paramètre est remplacé si desktopheight ou desktopwidth sont spécifié.| (0) 640 x 480; (1) 800 x 600; (2) 1024 x 768; (3) 1280 x 1024; (4) 1600 x 1200 | 0 | x | x | x |
| desktopheight:i:value | Ce paramètre détermine la hauteur de résolution (en pixels) sur l’ordinateur distant lorsque vous vous connectez à l’aide de connexion de bureau à distance (RDC). Ce paramètre correspond à la sélection dans Display configurationslider sur underOptions theDisplaytab dans RDC. | Valeur numérique de 200 à 2048 | La valeur par défaut est définie à la résolution de l’ordinateur local | x | x | x |
| desktopwidth:i:value | Ce paramètre détermine la largeur de résolution (en pixels) sur l’ordinateur distant lorsque vous vous connectez à l’aide de connexion de bureau à distance (RDC). Ce paramètre correspond à la sélection dans Display configurationslider sur underOptions theDisplaytab dans RDC. | Valeur numérique de 200 à 4096 | La valeur par défaut est définie à la résolution de l’ordinateur local | x | x | x |
| disableclipboardredirection:i:value | Ce paramètre détermine si la redirection du Presse-papiers est activée lorsque vous vous connectez à l’ordinateur distant. | La redirection du Presse-papiers (0) est activée; (1) la redirection du Presse-papiers n’est pas activée. | x | x | x |
| disableconnectionsharing:i:value | Détermine si le client Bureau à distance se reconnecte à toutes les connexions existantes ouvertes ou lancer une nouvelle connexion lors du lancée d’un bureau ou RemoteApp | (0) se reconnecter à une session existante; (1) de lancer une nouvelle connexion | 0 | x | x | x |
| disableprinterredirection:i:value | Ce paramètre détermine si la redirection d’imprimante facile d’impression est activée lorsque vous vous connectez à l’ordinateur distant. | La redirection d’imprimante facile d’impression (0) est activée; (1) la redirection d’imprimante facile d’impression est désactivée. | x | x | x |
| domaine: s:value | Ce paramètre spécifie le nom du domaine dans lequel se trouve le compte d’utilisateur qui sera utilisé pour vous connecter à l’ordinateur distant à l’aide de connexion de bureau à distance (RDC). La valeur de ce paramètre, ainsi que la valeur du paramètre nom d’utilisateur, s’affiche dans la zone de nametext droits sur theGeneraltab underOptionsin RDC. | Un nom de domaine valide, par exemple, «CONTOSO» | Aucune valeur par défaut | x | x | x |
| drivestoredirect:s:value | Détermine quels lecteurs de disque locales sur l’ordinateur client sera redirigé et disponible dans la session à distance. | Aucune valeur spécifiée - ne pas rediriger les lecteurs; * - Rediriger tous les lecteurs de disque, y compris les lecteurs connectés ultérieurement; DynamicDrives - rediriger les lecteurs connectés ultérieurement; Le lecteur et des étiquettes pour un ou plusieurs lecteurs - rediriger les lecteurs spécifiés.| Aucune valeur spécifiée - ne pas rediriger les lecteurs | x | x    | |
| enablecredsspsupport:i:value | Ce paramètre détermine si RDP doit utiliser les informations d’identification Security Support Provider (CredSSP) pour l’authentification si elle est disponible. | RDP (0) n’utilisera pas CredSSP, même si le système d’exploitation prend en charge CredSSP; (1) RDP utilisera CredSSP si le système d’exploitation prend en charge CredSSP | 1 | x | x | |
| adresse: s:value complet | Ce paramètre spécifie le nom ou l’adresse IP de l’ordinateur distant que vous souhaitez vous connecter à | Un nom d’ordinateur valide, son adresse IPv4 ou IPv6 adresse - spécifie l’ordinateur distant auquel vous souhaitez vous connecter. | | x | x | x |
| GatewayCredentialsSource:i:value | Spécifie ou récupère la méthode d’authentification de passerelle des services Bureau à distance. | Poser une question (0) pour le mot de passe (NTLM); (1) utiliser une carte à puce; (4) autoriser l’utilisateur de sélectionner plus tard | 0 | x | x | x |
| gatewayhostname:s:value | Spécifie le nom d’hôte passerelle des services Bureau à distance. | Adresse du serveur de passerelle valide. ||x|x|x|
| gatewayprofileusagemethod:i:value | Spécifie si vous souhaitez utiliser les paramètres de la passerelle des services Bureau à distance par défaut | (0) utilisent le mode de profil par défaut, tel que spécifié par l’administrateur. (1) utiliser les paramètres explicite, tel que spécifié par l’utilisateur. | 0 | x | x | x |
| gatewayusagemethod:i:value | Spécifie à quel moment utiliser le serveur de passerelle Bureau à distance | (0) n’utilisez pas un serveur passerelle des services Bureau à distance; (1) de toujours utiliser un serveur passerelle des services Bureau à distance; (2) utiliser un serveur de passerelle des services Bureau à distance si une connexion directe ne peut pas être établie avec l’hôte de Session Bureau à distance; (3) vous utilisez les paramètres de serveur de passerelle Bureau à distance par défaut; (4) ne pas utiliser une passerelle des services Bureau à distance, lesquelles ignorer le serveur pour les adresses locales; Définition de cette valeur de propriété à, 0 ou 4 sont équivaut efficacement, mais la définition de cette propriété à 4 activé l’option d’ignorer les adresses locales. | | x | x | x |
| networkautodetect:i:value | Indique s’il convient d’utiliser la détection de la bande passante réseau automatique ou non. Nécessite l’optionbandwidthautodetectto être définies et met en corrélation type7 withconnection. | (0) n’utilisent pas de détection de la bande passante réseau automatique; (1) utiliser la détection de la bande passante réseau automatique | 1 | x ||x|
| PromptCredentialOnce:i:value | Détermine si les informations d’identification de l’utilisateur sont enregistrées et utilisées pour la passerelle des services Bureau à distance et l’ordinateur distant.|Session à distance (0) n’utilisera pas les mêmes informations d’identification; (1) session à distance utilisera les mêmes informations d’identification|1|x|x||
| redirectclipboard:i:value | Ce paramètre détermine si le Presse-papiers sur l’ordinateur local sera redirigé et disponible dans la session à distance. Ce paramètre correspond à la boîte de theClipboardcheck sur le chemin Resourcestab underOptionsin RDC. | Presse-papiers (0) sur l’ordinateur local n’est pas disponible dans une session à distance; (1) Presse-papiers sur l’ordinateur local n’est disponible dans la session à distance|1|x|x|x|
| redirectdrives:i:value | Ce paramètre détermine si les lecteurs sur l’ordinateur client sera redirigé et disponible dans la session à distance. Ce paramètre correspond à la sélections forDrivesunderMoreon chemin Resourcestab underOptionsin RDC.|(0) les disques sur l’ordinateur local ne sont pas disponibles dans la session à distance; (1) les disques sur l’ordinateur local sont disponibles dans la session à distance|0|x|x| |
| redirectprinters:i:value | Ce paramètre détermine si les imprimantes configurées sur l’ordinateur client sera redirigé et disponible dans la session à distance lorsque vous vous connectez à un ordinateur distant à l’aide de connexion de bureau à distance (RDC). Ce paramètre correspond à la sélection dans la zone de thePrinterscheck sur le chemin Resourcestab underOptionsin RDC. | (0) les imprimantes sur l’ordinateur local ne sont pas disponibles dans la session à distance; (1) les imprimantes sur l’ordinateur local sont disponibles dans la session à distance|1|x|x|x|
| redirectsmartcards:i:value | Ce paramètre détermine si les périphériques de carte à puce sur l’ordinateur client sera redirigé et disponible dans la session à distance lorsque vous vous connectez à un ordinateur distant à l’aide de connexion de bureau à distance (RDC). Ce paramètre correspond à la sélection dans theSmart cardscheck zone underMore sur le chemin Resourcestab underOptionsin RDC.|(0) l’appareil de carte à puce sur l’ordinateur local n’est pas disponible dans la session à distance; (1) le périphérique de carte à puce sur l’ordinateur local est disponible dans la session à distance|1|x|x||
| remoteapplicationcmdline:s:value | Paramètres de ligne de commande facultatifs pour le RemoteApp.||x|x|x|
| remoteapplicationexpandcmdline:i:value| Détermine si les variables d’environnement contenues dans le paramètre de ligne de commande RemoteApp doivent être développés localement ou à distance.|Variables d’environnement (0) doivent être développés pour les valeurs de l’ordinateur local; (1) variables d’environnement doivent être développés sur l’ordinateur distant pour les valeurs de l’ordinateur distant||x|x|x|
| remoteapplicationexpandworkingdir | Détermine si les variables d’environnement contenues dans le paramètre RemoteApp du répertoire de travail doivent être développés localement ou à distance. | Variables d’environnement (0) doivent être développés pour les valeurs de l’ordinateur local; (1) variables d’environnement doivent être développés sur l’ordinateur distant pour les valeurs de l’ordinateur distant. Remarque: Le répertoire de travail RemoteApp est spécifié via le paramètre de répertoire de travail interpréteur de commandes.||x|x|x|
|remoteapplicationfile:s:value | Spécifie un fichier à être ouverts sur l’ordinateur distant par le RemoteApp. Remarque: Pour les fichiers locaux être ouvert, vous devez également activer la redirection de lecteur pour le disque source.||x|x|x|
|remoteapplicationicon:s:value | Spécifie le fichier d’icône s’affichent dans l’interface utilisateur du client lors du lancement d’un RemoteApp. Si aucun nom de fichier n’est spécifié, le client utilisera l’icône de bureau à distance standard. Seuls les fichiers «.ico» sont pris en charge.||x|x|x|
|remoteapplicationmode:i:value | Détermine si une connexion RemoteApp est lancée en tant que session RemoteApp.| (0) ne pas lancer une session RemoteApp; (1) lancement d’une session RemoteApp|1|x|x|x|
|remoteapplicationname:s:value | Spécifie le nom de la RemoteApp dans l’interface du client lors du démarrage de le RemoteApp.| par exemple, «Excel 2016»|x|x|x|
|remoteapplicationprogram:s:value | Spécifie le nom d’alias ou le fichier exécutable de le RemoteApp. | par exemple, «EXCEL» |x|x|x|
|écran mode id: i:value | Ce paramètre détermine si la fenêtre de la session à distance s’affiche en plein écran lorsque vous vous connectez à l’ordinateur distant à l’aide de connexion de bureau à distance (RDC). Ce paramètre correspond à la sélection dans Display configurationslider sur theDisplaytab underOptionsin RDC.|(1) la session à distance s’affichent dans une fenêtre; (2) la session à distance s’affichent en plein écran|2|x|x|x|
|dimensionnement: i:value actives | Ce paramètre détermine si l’ordinateur client peut évoluer le contenu sur l’ordinateur distant pour s’adapter à la taille de fenêtre de l’ordinateur client.|(0) l’affichage de la fenêtre client ne sera pas réduite lors du redimensionnement; (1) l’affichage de la fenêtre client sera réduite lors du redimensionnement|0|x|x||
| utiliser multimon:i:value | Ce paramètre configure la prise en charge de plusieurs moniteurs lorsque vous vous connectez à l’ordinateur distant à l’aide de connexion de bureau à distance (RDC).|(0) ne permettent pas de prise en charge de plusieurs moniteurs; (1) activer la prise en charge de plusieurs moniteurs|0|x|x||
| username:s:value | Ce paramètre spécifie le nom du compte d’utilisateur qui sera utilisé pour vous connecter à l’ordinateur distant à l’aide de connexion de bureau à distance (RDC). La valeur de ce paramètre, ainsi que la valeur du paramètre domaine, s’affichent dans la zone Nom de droits sur theGeneraltab underOptionsin RDC.| N’importe quel nom d’utilisateur valide. ||x|x|x|
| videoplaybackmode:i:value| Ce paramètre détermine si la connexion de bureau à distance (RDC) utilisera RDP efficace multimédia de diffusion en continu pour la lecture vidéo.|(0) n’utilisent pas le protocole RDP efficace multimédia de diffusion en continu pour la lecture vidéo; (1) utiliser le protocole RDP efficace multimédia de diffusion en continu pour la lecture vidéo lorsque cela est possible|1|x|x||
| workspaceid:s:value | Ce paramètre définit le RemoteApp et bureau ID associé avec le fichier RDP qui contient ce paramètre. | Un RemoteApp valide et un ID de connexion de bureau|x|x||