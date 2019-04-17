---
title: Configurer le client web de Bureau à distance pour vos utilisateurs
description: Décrit comment un administrateur peut configurer le client Bureau à distance du web.
ms.prod: windows-server-threshold
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 11/2/2018
ms.topic: article
author: Heidilohr
ms.localizationpriority: medium
ms.openlocfilehash: 2cb819a7f91646c61b84c3ee70550af6033ba340
ms.sourcegitcommit: 491c94b25501732c4a4abda533cc62b8bd278ed2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/22/2019
ms.locfileid: "9099182"
---
# Configurer le client web de Bureau à distance pour vos utilisateurs

Le client de web des services Bureau à distance permet aux utilisateurs d’accéder infrastructure de bureau à distance de votre organisation par le biais d’un navigateur web compatible. Ils serez en mesure d’interagir avec des applications à distance ou les ordinateurs de bureau comme ils le feraient avec un PC local où qu’ils se trouvent. Une fois de votre Distante Desktop web client, all your il à get est URL où il accéder client, identification, navigateur compatible.

>[!IMPORTANT]
>Le client web ne prend pas actuellement en charge à l’aide du Proxy d’Application Azure et ne prend pas en charge le Proxy d’Application Web. Pour plus d’informations, reportez-vous [à l’aide de services Bureau à distance avec les services de proxy d’application](../rds-supported-config.md#using-remote-desktop-services-with-application-proxy-services) .

## Ce que vous devez configurer le client web

Avant de commencer, gardez à l’esprit les points suivants:

* Assurez-vous que votre [déploiement des services Bureau à distance](../rds-deploy-infrastructure.md) a une passerelle des services Bureau à distance, un Broker pour les connexions Bureau à distance et l’accès Web Bureau à distance en cours d’exécution sur Windows Server 2016 ou 2019.
* Assurez-vous que votre déploiement est configuré pour [par utilisateur les licences d’accès client](../rds-client-access-license.md) (CAL) au lieu de chaque appareil, sinon, toutes les licences seront utilisés.
* Installer la [mise à jour de Windows 10 KB4025334](https://support.microsoft.com/en-us/help/4025334/windows-10-update-kb4025334) sur la passerelle des services Bureau à distance. Ultérieurement les mises à jour cumulatives peut contient déjà cette base de connaissances.
* Assurez-vous que les certificats approuvés publiques sont configurés pour les rôles de passerelle des services Bureau à distance et l’accès Web Bureau à distance.
* Assurez-vous que vos utilisateurs seront connectent à tous les ordinateurs exécutent une des versions du système d’exploitation suivantes:
  * Windows10
  * Windows Server 2008 R2 ou version ultérieure

Vos utilisateurs verront une meilleure performance se connecter à Windows Server 2016 (ou version ultérieure) et Windows 10 (1611 ou version ultérieure).

>[!IMPORTANT]
>Si vous utilisé le client web pendant la période d’aperçu et installé une version avant 1.0.0, vous devez d’abord désinstaller l’ancien client avant de passer à la nouvelle version. Si vous recevez une erreur indiquant «le client web a été installé à l’aide d’une version antérieure de RDWebClientManagement et doit tout d’abord être supprimé avant le déploiement de la nouvelle version», procédez comme suit:
>
>1. Ouvrez Une PowerShell INVITE.
>2. Exécutez **RDWebClientManagement du Module de désinstallation** pour désinstaller le nouveau module.
>3. Fermez et rouvrez l’invite PowerShell avec élévation de privilèges.
>4. Exécutez **RDWebClientManagement Install-Module paramètres-RequiredVersion \<old version> pour installer le module ancien.**
>5. Exécutez **RDWebClient-désinstallation** pour désinstaller l’ancien client web.
>6. Exécutez **RDWebClientManagement du Module de désinstallation** pour désinstaller l’ancien module.
>7. Fermez et rouvrez l’invite PowerShell avec élévation de privilèges.
>8. Suivez la procédure d’installation normale comme suit.

## Procédure pour publier le client de web des services Bureau à distance

Pour installer le client web pour la première fois, procédez comme suit:

1. Sur le serveur Broker pour les connexions Bureau à distance, obtenir le certificat utilisé pour les connexions Bureau à distance et exporter sous la forme d’un fichier .cer. Copiez le fichier .cer à partir du Broker pour les connexions Bureau à distance sur le serveur qui exécute le rôle Web des services Bureau à distance.
2. Sur le serveur d’accès Web de bureau à distance, ouvrez une invite PowerShell avec élévation de privilèges.
3. Sur Windows Server 2016, mettez à jour le module PowerShellGet dans la mesure où la version de la boîte de réception non pris en charge l’installation du module de gestion du client web. Pour mettre à jour PowerShellGet, exécutez l’applet de commande suivante:
    ```PowerShell
    Install-Module -Name PowerShellGet -Force
    ```

    >[!IMPORTANT]
    >Vous devrez redémarrer PowerShell avant la mise à jour peut prendre effet, sinon, que le module peut ne pas fonctionner.

4. Installer le module PowerShell de gestion des services Bureau à distance web client à partir de la galerie PowerShell avec cette applet de commande:
    ```PowerShell
    Install-Module -Name RDWebClientManagement
    ```

5. Après cela, exécutez l’applet de commande suivante pour télécharger la dernière version du client Bureau à distance web:
    ```PowerShell
    Install-RDWebClientPackage
    ```

6. Ensuite, exécutez cette applet de commande avec la valeur entre crochets remplacée par le chemin d’accès du fichier .cer que vous avez copié à partir du service Broker pour les services Bureau à distance:
    ```PowerShell
    Import-RDWebClientBrokerCert <.cer file path>
    ```

7. Enfin, exécutez la commande suivante pour publier le client de web des services Bureau à distance:
    ```PowerShell
    Publish-RDWebClientPackage -Type Production -Latest
    ```
    Assurez-vous que le client web à l’URL de client web accessibles avec le nom de votre serveur, au format <https://server_FQDN/RDWeb/webclient/index.html>. Il est important d’utiliser le nom du serveur qui correspond au certificat public un accès Web Bureau à distance dans l’URL (en règle générale, le serveur nom de domaine complet).

    >[!NOTE]
    >Lorsque vous exécutez l’applet de commande **Publier-RDWebClientPackage** , vous pouvez voir un message d’avertissement indiquant les licences d’accès client par périphérique ne sont pas prises en charge, même si votre déploiement est configuré pour les licences d’accès client par utilisateur. Si votre déploiement utilise des licences d’accès client par utilisateur, vous pouvez ignorer cet avertissement. Nous l’affichons pour vous assurer que vous en ayez connaissance de la limitation de la configuration.
8. Lorsque vous êtes prêt pour les utilisateurs d’accéder au client web, il suffit d’envoyer les l’URL de client web que vous avez créé.

>[!NOTE]
>Pour afficher une liste de toutes les applets de commande pris en charge pour le module RDWebClientManagement, exécutez l’applet de commande suivante dans PowerShell:
>```PowerShell
>Get-Command -Module RDWebClientManagement
>```

## Comment mettre à jour le client de web des services Bureau à distance

Lorsqu’une nouvelle version du client Bureau à distance web est disponible, procédez comme suit pour mettre à jour le déploiement avec le nouveau client:

1. Ouvrez une invite PowerShell avec élévation de privilèges sur le serveur d’accès Web de bureau à distance et exécutez l’applet de commande suivante pour télécharger la dernière version disponible du client web:
    ```PowerShell
    Install-RDWebClientPackage
    ```

2. Si vous le souhaitez, vous pouvez publier le client avant le lancement officiel de test en exécutant l’applet de commande suivante:
    ```PowerShell
    Publish-RDWebClientPackage -Type Test -Latest
    ```

    Le client doit apparaître sur l’URL de test correspondant à l’URL de votre client web (par exemple, <https://server_FQDN/RDWeb/webclient-test/index.html>).
3. Publier le client pour les utilisateurs en exécutant l’applet de commande suivante:
    ```PowerShell
    Publish-RDWebClientPackage -Type Production -Latest
    ```

    Cela remplace le client pour tous les utilisateurs lorsqu’ils relancer la page web.

## Procédure pour désinstaller le client web services Bureau à distance

Pour supprimer toutes les traces du client web, procédez comme suit:

1. Sur le serveur d’accès Web de bureau à distance, ouvrez une invite PowerShell avec élévation de privilèges.
2. Annuler la publication les clients de Test et de Production, désinstallez tous les packages locales et supprimer les paramètres du client web:

   ```PowerShell
   Uninstall-RDWebClient
   ```

3. Désinstaller le module PowerShell de gestion des services Bureau à distance web client:

   ```PowerShell
   Uninstall-Module -Name RDWebClientManagement
   ```

## Procédure pour installer le client web services Bureau à distance sans connexion internet

Suivez ces étapes pour déployer le client web sur un serveur d’accès Web de bureau à distance qui n’a pas une connexion internet.

> [!NOTE]
> L’installation sans connexion internet est disponible dans la version 1.0.1 et au-dessus du module RDWebClientManagement PowerShell.

> [!NOTE]
> Vous devez toujours un administrateur PC ayant accès à internet pour télécharger les fichiers nécessaires avant de les transférer vers le serveur en mode hors connexion.

> [!NOTE]
> Le PC de l’utilisateur final a besoin d’une connexion internet pour le moment. Cela sera résolu dans une prochaine version du client pour fournir un scénario en mode hors connexion complet.

### À partir d’un appareil doté d’un accès internet

1. Ouvrez une invite de commandes PowerShell.

2. Importez le module PowerShell de gestion des services Bureau à distance web client à partir de la galerie PowerShell:
    ```PowerShell
    Import-Module -Name RDWebClientManagement
    ```

3. Téléchargez la dernière version du client Bureau à distance web pour l’installation sur un autre appareil:
    ```PowerShell
    Save-RDWebClientPackage "C:\WebClient\"
    ```

4. Téléchargez la dernière version du module RDWebClientManagement PowerShell:
    ```PowerShell
    Find-Module -Name "RDWebClientManagement" -Repository "PSGallery" | Save-Module -Path "C:\WebClient\"
    ```

5. Copiez le contenu de «C:\WebClient\» sur le serveur d’accès Web de bureau à distance.

### À partir du serveur d’accès Web de bureau à distance

Suivez les instructions sous [comment publier le client Bureau à distance du web](remote-desktop-web-client-admin.md#how-to-publish-the-remote-desktop-web-client), en remplaçant les étapes 4 et 5 par ce qui suit.

4. Importez le module PowerShell de gestion des services Bureau à distance web client à partir du dossier local:
    ```PowerShell
    Import-Module -Name "C:\WebClient\"
    ```

5. Déployer la dernière version du client Bureau à distance web à partir du dossier local (Remplacez par le fichier zip approprié):
    ```PowerShell
    Install-RDWebClientPackage -Source "C:\WebClient\rdwebclient-1.0.1.zip"
    ```

## Connexion au service Broker sans passerelle des services Bureau à distance dans Windows Server 2019
Cette section décrit comment activer une connexion de client web à un service Broker sans une passerelle des services Bureau à distance dans Windows Server 2019.

### Configuration du serveur du service Broker

#### Suivez ces étapes s’il n’existe aucun certificat liée au serveur du service Broker

1. Ouvrez le **Gestionnaire de serveur** > **des Services Bureau à distance**.

2. Dans la section de la **Vue d’ensemble du déploiement** , sélectionnez le menu déroulant de **tâches** .

3. Sélectionnez **Modifier les propriétés du déploiement**, une nouvelle fenêtre intitulée **Déploiement des propriétés** s’ouvre.

4. Dans la fenêtre de **Propriétés de déploiement** , sélectionnez **les certificats** dans le menu de gauche.

5. Dans la liste des niveaux de certificat, sélectionnez **Broker pour les connexions Bureau à distance - activer Single Sign On**. Vous disposez de deux options: (1) créez un nouveau certificat ou (2) un certificat existant.

#### Suivez ces étapes s’il existe un certificat précédemment lié au serveur du service Broker

1. Ouvrez le certificat lié au Broker et copiez la valeur de **l’empreinte numérique** .

2. Pour lier ce certificat au port sécurisé 3392, ouvrez une fenêtre PowerShell avec élévation de privilèges et exécutez la commande suivante, en remplaçant **«< empreinte numérique >»** avec la valeur copiée à partir de l’étape précédente:

    ```PowerShell
    netsh http add sslcert ipport=0.0.0.0:3392 certhash="<thumbprint>" certstorename="Remote Desktop" appid="{00000000-0000-0000-0000-000000000000}"
    ```

    > [!NOTE]
    > Pour vérifier si le certificat a été lié correctement, exécutez la commande suivante:
    >
    > ```PowerShell
    > netsh http show sslcert
    > ```
    >
    > Dans la liste des liaisons de certificat SSL, assurez-vous que le certificat approprié est lié au port 3392.

3. Ouvrez le Registre Windows (regedit) et nagivate à ```HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp``` et recherchez la clé **WebSocketURI**. La valeur doit être définie sur **https://+:3392/rdp/**.

### Configuration de l’hôte de Session Bureau à distance
Si le serveur hôte de Session Bureau à distance est différent du serveur du service Broker, procédez comme suit:

1. Créer un certificat pour l’ordinateur hôte de Session Bureau à distance, ouvrez-le et copiez la valeur de **l’empreinte numérique** .

2. Pour lier ce certificat au port sécurisé 3392, ouvrez une fenêtre PowerShell avec élévation de privilèges et exécutez la commande suivante, en remplaçant **«< empreinte numérique >»** avec la valeur copiée à partir de l’étape précédente:

    ```PowerShell
    netsh http add sslcert ipport=0.0.0.0:3392 certhash="<thumbprint>" appid="{00000000-0000-0000-0000-000000000000}"
    ```

    > [!NOTE]
    > Pour vérifier si le certificat a été lié correctement, exécutez la commande suivante:
    >
    > ```PowerShell
    > netsh http show sslcert
    > ```
    >
    > Dans la liste des liaisons de certificat SSL, assurez-vous que le certificat approprié est lié au port 3392.

3. Ouvrez le Registre Windows (regedit) et nagivate à ```HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp``` et recherchez la clé **WebSocketURI**. La valeur doit être définie sur **https://+:3392/rdp/**.

### Remarques générales

* Assurez-vous que l’hôte de Session Bureau à distance et du service Broker server exécutent Windows Server 2019.

* Veillez à ce que public sécurisée de certificats sont configurés pour le serveur de l’hôte de Session Bureau à distance et du service Broker.
    > [!NOTE]
    > Si l’hôte de Session Bureau à distance et le serveur du service Broker partagent le même ordinateur, définissez le certificat de serveur du service Broker uniquement. Si le serveur hôte de Session Bureau à distance et du service Broker utiliser différents ordinateurs, les deux doivent être configurés avec des certificats uniques.

* **Subject Alternative Name (SAN)** pour chaque certificat doit être défini sur de l’ordinateur **Nom de domaine complet (FQDN)**. Le **Nom commun (CN)** doit correspondre à la SAN pour chaque certificat.

## Résolution des problèmes

Si un utilisateur signale les éventuels problèmes suivants lors de l’ouverture du client web pour la première fois, les sections suivantes vous indique comment procéder pour les résoudre.

### Que faire si le navigateur de l’utilisateur affiche un avertissement de sécurité lorsqu’ils essaient d’accéder au client web

Le rôle d’accès Web de bureau à distance ne peut pas d’utiliser un certificat approuvé. Assurez-vous que le rôle d’accès Web de bureau à distance est configuré avec un certificat approuvé publiquement.

Si cela ne fonctionne pas, le nom de votre serveur dans le site web URL client ne peut pas correspondre au nom fourni par le certificat Web des services Bureau à distance. Assurez-vous que votre URL utilise le nom de domaine complet du serveur qui héberge le rôle Web des services Bureau à distance.

### Que faire si l’utilisateur Impossible de se connecter à une ressource avec le client web même s’ils peuvent voir les éléments placés sous toutes les ressources

Si l’utilisateur indique qu’ils ne peuvent pas se connecter avec le client web même si elles peuvent voir les ressources répertoriées, vérifiez les éléments suivants:

* Le rôle de passerelle des services Bureau à distance est correctement configuré pour utiliser un certificat public approuvé?
* Le serveur de passerelle des services Bureau à distance a-t-il les mises à jour requises installés? Assurez-vous que votre serveur dispose de [la mise à jour de la KB4025334](https://support.microsoft.com/en-us/help/4025334/windows-10-update-kb4025334) installé.

Si l’utilisateur reçoit une erreur «certificat d’authentification serveur inattendue a été reçue» lorsqu’ils essaient de se connecter, puis le message affiche l’empreinte du certificat du message. Recherchez le Gestionnaire de certificats du serveur du service Broker à l’aide de cette empreinte pour trouver le certificat approprié. Vérifiez que le certificat est configuré pour être utilisé pour le rôle du service Broker dans la page de propriétés de déploiement de services Bureau à distance. Une fois assurant que le certificat n’a pas encore expiré, copiez le certificat au format de fichier .cer sur le serveur d’accès Web de bureau à distance et exécutez la commande suivante sur le serveur d’accès Web de bureau à distance avec la valeur entre crochets remplacée par un chemin d’accès du certificat:

```PowerShell
Import-RDWebClientBrokerCert <certificate file path>
```

### Diagnostiquer les problèmes avec le journal de console

Si vous ne pouvez pas résoudre le problème basé sur les instructions de résolution des problèmes de cet article, vous pouvez tenter de diagnostiquer la source du problème en regardant la console ouvrir une session dans le navigateur. Le client web fournit une méthode pour l’enregistrement de l’activité du journal console navigateur tout en utilisant le client web pour aider à diagnostiquer les problèmes.

* Sélectionnez les points de suspension dans le coin supérieur droit et accédez à la page **à propos** dans le menu déroulant.
* Sous les **informations de prise en charge de Capture** , sélectionnez le bouton **Démarrer l’enregistrement** .
* Effectuer les opérations dans le client web qui a généré le problème que vous essayez de diagnostiquer.
* Accédez à la page **à propos** et sélectionnez **Arrêter l’enregistrement**.
* Votre navigateur télécharge automatiquement un fichier .txt intitulé **Logs.txt Console de bureau à distance**. Ce fichier contient l’activité du journal de console complet générée lors de reproduire le problème cible.

La console est également accessibles directement par le biais de votre navigateur. La console se trouve généralement sous les outils de développement. Par exemple, vous pouvez accéder au journal dans Microsoft Edge en appuyant sur la touche **F12** ou en sélectionnant les points de suspension, puis en accédant à **outils plus** > **Outils de développement**.

## Obtenez de l’aide avec le client web

Si vous avez rencontré un problème qui ne peut pas être résolu par les informations contenues dans cet article, vous pouvez [nous envoyer un e-mail](mailto:rdwbclnt@microsoft.com) pour le signaler. Vous pouvez également demander ou voter pour les nouvelles fonctionnalités à notre [zone de suggestion](https://aka.ms/rdwebfbk).