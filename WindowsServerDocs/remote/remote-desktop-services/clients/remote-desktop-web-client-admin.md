---
title: Configurer le client web Bureau à distance pour vos utilisateurs
description: Décrit la procédure à suivre par un administrateur pour configurer le client web Bureau à distance.
ms.prod: windows-server-threshold
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 11/2/2018
ms.topic: article
author: Heidilohr
ms.localizationpriority: medium
ms.openlocfilehash: d167fb5dfdfbb2a302c2b0fca9286dc034b730e3
ms.sourcegitcommit: 0467b8e69de66e3184a42440dd55cccca584ba95
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69546332"
---
# <a name="set-up-the-remote-desktop-web-client-for-your-users"></a>Configurer le client web Bureau à distance pour vos utilisateurs

Le client web Bureau à distance permet aux utilisateurs d'accéder à l'infrastructure Bureau à distance de votre organisation via un navigateur web compatible. Où qu'ils se trouvent, les utilisateurs peuvent alors interagir avec les applications ou appareils de bureau distants comme ils le feraient avec un PC local. Une fois votre client web Bureau à distance configuré, vos utilisateurs ont uniquement besoin de l'URL d'accès au client, de leurs informations d'identification et d'un navigateur web pris en charge.

>[!IMPORTANT]
>Le client web ne prend actuellement pas en charge l'utilisation du Proxy d'application Azure ni le Proxy d'application Web. Pour plus d'informations, consultez [Utilisation des services Bureau à distance avec les services de proxy d'application](../rds-supported-config.md#using-remote-desktop-services-with-application-proxy-services).

## <a name="what-youll-need-to-set-up-the-web-client"></a>Ce dont vous aurez besoin pour configurer le client web

Avant de commencer, gardez les points suivants à l'esprit :

* Assurez-vous que votre [déploiement Bureau à distance](../rds-deploy-infrastructure.md) dispose d'une passerelle de services Bureau à distance, d'un service Broker pour les connexions Bureau à distance et d'un accès aux services Bureau à distance par le web sous Windows Server 2016 ou 2019.
* Assurez-vous que votre déploiement est configuré pour utiliser des [licences d'accès client par utilisateur](../rds-client-access-license.md) et non par appareil, sinon toutes les licences seront utilisées.
* Installez la [mise à jour KB4025334 de Windows 10](https://support.microsoft.com/en-us/help/4025334/windows-10-update-kb4025334) sur la passerelle des services Bureau à distance. Les mises à jour cumulatives ultérieures peuvent déjà contenir cette mise à jour.
* Assurez-vous que les certificats publics approuvés sont configurés pour les rôles Passerelle des services Bureau à distance et Accès aux services Bureau à distance par le web.
* Assurez-vous que tous les ordinateurs auxquels vos utilisateurs se connecteront exécutent l'une des versions suivantes du système d'exploitation :
  * Windows 10
  * Windows Server 2008 R2 ou version ultérieure

Vos utilisateurs bénéficieront de meilleures performances en se connectant à Windows Server 2016 (ou version ultérieure) et à Windows 10 (version 1611 ou ultérieure).

>[!IMPORTANT]
>Si vous avez utilisé le client web pendant la phase de préversion et installé une version antérieure à la version 1.0.0, vous devez d'abord désinstaller l'ancien client avant de passer à la nouvelle version. Si vous recevez le message d'erreur « Le client web a été installé à l'aide d'une version antérieure de RDWebClientManagement et doit d'abord être supprimé avant de déployer la nouvelle version », procédez comme suit :
>
>1. Ouvrez une invite PowerShell avec élévation de privilèges.
>2. Exécutez **Uninstall-Module RDWebClientManagement** pour désinstaller le nouveau module.
>3. Fermez et rouvrez l'invite PowerShell avec élévation de privilèges.
>4. Exécutez **Install-Module RDWebClientManagement -RequiredVersion \<ancienne version> pour installer l'ancien module.**
>5. Exécutez **Uninstall-RDWebClient** pour désinstaller l'ancien client web.
>6. Exécutez **Uninstall-Module RDWebClientManagement** pour désinstaller l'ancien module.
>7. Fermez et rouvrez l'invite PowerShell avec élévation de privilèges.
>8. Passez aux étapes normales d'installation décrites ci-dessous.

## <a name="how-to-publish-the-remote-desktop-web-client"></a>Publier le client web Bureau à distance

Pour installer le client web, procédez comme suit :

1. Sur le serveur du service Broker pour les connexions Bureau à distance, procurez-vous le certificat utilisé pour les connexions Bureau à distance et exportez-le au format .cer. À partir du service Broker pour les connexions Bureau à distance, copiez le fichier .cer sur le serveur exécutant le rôle Accès aux services Bureau à distance par le web.
2. Sur le serveur d'accès aux services Bureau à distance par le web, ouvrez une invite PowerShell avec élévation de privilèges.
3. Sous Windows Server 2016, mettez à jour le module PowerShellGet, car la version de la boîte de réception ne prend pas en charge l'installation du module de gestion du client web. Pour mettre à jour le module PowerShellGet, exécutez l'applet de commande suivante :
    ```PowerShell
    Install-Module -Name PowerShellGet -Force
    ```

    >[!IMPORTANT]
    >Vous devrez redémarrer PowerShell pour que la mise à jour prenne effet, sinon le module risque de ne pas fonctionner.

4. Installez le module PowerShell de gestion du client web Bureau à distance à partir de la galerie PowerShell avec l'applet de commande suivante :
    ```PowerShell
    Install-Module -Name RDWebClientManagement
    ```

5. Puis exécutez l'applet de commande suivante pour télécharger la dernière version du client web Bureau à distance :
    ```PowerShell
    Install-RDWebClientPackage
    ```

6. Exécutez ensuite l'applet de commande suivante en remplaçant la valeur entre crochets par le chemin du fichier .cer que vous avez copié à partir du service Broker Bureau à distance :
    ```PowerShell
    Import-RDWebClientBrokerCert <.cer file path>
    ```

7. Enfin, exécutez l'applet de commande suivante pour publier le client web Bureau à distance :
    ```PowerShell
    Publish-RDWebClientPackage -Type Production -Latest
    ```
    Vérifiez que vous avez accès au client web via l'URL avec le nom de votre serveur, au format <https://server_FQDN/RDWeb/webclient/index.html>. Il est important d'utiliser le nom du serveur qui correspond au certificat public d'accès aux services Bureau à distance par le web dans l'URL (il s'agit généralement du nom de domaine complet du serveur).

    >[!NOTE]
    >Lorsque vous exécutez l'applet de commande **Publish-RDWebClientPackage**, un avertissement peut apparaître pour indiquer que les licences d'accès client par appareil ne sont pas prises en charge, même si votre déploiement est configuré pour les licences d'accès client par utilisateur. Si votre déploiement utilise des licences d'accès client par utilisateur, vous pouvez ignorer cet avertissement. Nous l'affichons pour vous informer des limites de la configuration.
8. Lorsque vous êtes prêt à autoriser les utilisateurs à accéder au client web, il vous suffit de leur envoyer l'URL que vous avez créée pour le client web.

>[!NOTE]
>Pour afficher la liste de toutes les applets de commande prises en charge pour le module RDWebClientManagement, exécutez l'applet de commande suivante dans PowerShell :
>```PowerShell
>Get-Command -Module RDWebClientManagement
>```

## <a name="how-to-update-the-remote-desktop-web-client"></a>Mettre à jour le client web Bureau à distance

Lorsqu'une nouvelle version du client web Bureau à distance est disponible, procédez comme suit pour mettre à jour le déploiement avec le nouveau client :

1. Ouvrez une invite PowerShell avec élévation de privilèges sur le serveur d'accès aux services Bureau à distance par le web et exécutez l'applet de commande suivante pour télécharger la dernière version disponible du client web :
    ```PowerShell
    Install-RDWebClientPackage
    ```

2. Si vous le souhaitez, vous pouvez tester le client avant sa publication officielle en exécutant l'applet de commande suivante :
    ```PowerShell
    Publish-RDWebClientPackage -Type Test -Latest
    ```

    Le client doit apparaître sur l'URL de test correspondant à l'URL de votre client web (par exemple, <https://server_FQDN/RDWeb/webclient-test/index.html>).
3. Publiez le client pour les utilisateurs en exécutant l'applet de commande suivante :
    ```PowerShell
    Publish-RDWebClientPackage -Type Production -Latest
    ```

    Cela remplacera le client pour tous les utilisateurs lorsqu'ils relanceront la page web.

## <a name="how-to-uninstall-the-remote-desktop-web-client"></a>Désinstaller le client web Bureau à distance

Pour supprimer toute trace du client web, procédez comme suit :

1. Sur le serveur d'accès aux services Bureau à distance par le web, ouvrez une invite PowerShell avec élévation de privilèges.
2. Annulez la publication des clients de test et de production, désinstallez tous les packages locaux et supprimez les paramètres du client web :

   ```PowerShell
   Uninstall-RDWebClient
   ```

3. Désinstallez le module PowerShell de gestion du client web Bureau à distance :

   ```PowerShell
   Uninstall-Module -Name RDWebClientManagement
   ```

## <a name="how-to-install-the-remote-desktop-web-client-without-an-internet-connection"></a>Installer le client web Bureau à distance sans connexion Internet

Suivez les étapes ci-dessous pour déployer le client web sur un serveur d'accès aux services Bureau à distance par le web dépourvu de connexion Internet.

> [!NOTE]
> L'installation sans connexion Internet est disponible avec les versions 1.0.1 et ultérieures du module PowerShell RDWebClientManagement.

> [!NOTE]
> Vous devez malgré tout disposer d'un PC administrateur avec accès Internet pour télécharger les fichiers nécessaires avant de les transférer vers le serveur hors ligne.

> [!NOTE]
> Pour le moment, le PC de l'utilisateur final doit disposer d'une connexion Internet. Ce point sera traité dans une prochaine version du client afin de fournir un scénario hors ligne complet.

### <a name="from-a-device-with-internet-access"></a>À partir d'un appareil connecté à Internet

1. Ouvrez une invite PowerShell.

2. Importez le module PowerShell de gestion du client web Bureau à distance à partir de la galerie PowerShell :
    ```PowerShell
    Import-Module -Name RDWebClientManagement
    ```

3. Téléchargez la dernière version du client web Bureau à distance pour l'installer sur un autre appareil :
    ```PowerShell
    Save-RDWebClientPackage "C:\WebClient\"
    ```

4. Téléchargez la dernière version du module PowerShell RDWebClientManagement :
    ```PowerShell
    Find-Module -Name "RDWebClientManagement" -Repository "PSGallery" | Save-Module -Path "C:\WebClient\"
    ```

5. Copiez le contenu de « C:\WebClient\" » sur le serveur d'accès aux services Bureau à distance par le web.

### <a name="from-the-rd-web-access-server"></a>À partir du serveur d'accès aux services Bureau à distance par le web

Suivez les instructions fournies à la section [Publier le client web Bureau à distance](remote-desktop-web-client-admin.md#how-to-publish-the-remote-desktop-web-client) en remplaçant les étapes 4 et 5 par les suivantes.

4. Importez le module PowerShell de gestion du client web Bureau à distance à partir du dossier local :
    ```PowerShell
    Import-Module -Name "C:\WebClient\"
    ```

5. Déployez la dernière version du client web Bureau à distance à partir du dossier local (remplacez le fichier par le zip approprié) :
    ```PowerShell
    Install-RDWebClientPackage -Source "C:\WebClient\rdwebclient-1.0.1.zip"
    ```

## <a name="connecting-to-rd-broker-without-rd-gateway-in-windows-server-2019"></a>Connexion au service Broker Bureau à distance sans passerelle de services Bureau à distance dans Windows Server 2019
Cette section explique comment activer une connexion client web à un service Broker Bureau à distance sans passerelle de services Bureau à distance dans Windows Server 2019.

### <a name="setting-up-the-rd-broker-server"></a>Configuration du serveur du service Broker Bureau à distance

#### <a name="follow-these-steps-if-there-is-no-certificate-bound-to-the-rd-broker-server"></a>Suivez les étapes ci-dessous si aucun certificat n'est lié au serveur du service Broker Bureau à distance.

1. Ouvrez **Gestionnaire de serveur** > **Services Bureau à distance**.

2. Dans la section **Vue d'ensemble du déploiement**, sélectionnez le menu déroulant **Tâches**.

3. Sélectionnez **Modifier les propriétés de déploiement**. Une nouvelle fenêtre intitulée **Propriétés de déploiement** s'ouvre alors.

4. Dans la fenêtre **Propriétés de déploiement**, sélectionnez **Certificats** à partir du menu de gauche.

5. Dans la liste des Niveaux de certification, sélectionnez **Service Broker pour les connexions Bureau à distance - Activer l'authentification unique**. Deux options s'offrent à vous : (1) créer un nouveau certificat ou (2) utiliser un certificat existant.

#### <a name="follow-these-steps-if-there-is-a-certificate-previously-bound-to-the-rd-broker-server"></a>Suivez les étapes ci-dessous si un certificat a précédemment été lié au serveur du service Broker Bureau à distance :

1. Ouvrez le certificat lié au service Broker et copiez la valeur **Thumbprint**.

2. Pour lier ce certificat au port sécurisé 3392, ouvrez une fenêtre PowerShell avec élévation de privilèges et exécutez la commande suivante, en remplaçant **"<thumbprint>"** par la valeur copiée à l'étape précédente :

    ```PowerShell
    netsh http add sslcert ipport=0.0.0.0:3392 certhash="<thumbprint>" certstorename="Remote Desktop" appid="{00000000-0000-0000-0000-000000000000}"
    ```

    > [!NOTE]
    > Pour vérifier que le certificat a été correctement lié, exécutez la commande suivante :
    >
    > ```PowerShell
    > netsh http show sslcert
    > ```
    >
    > Dans la liste des liaisons de certificat SSL, assurez-vous que le certificat qui convient est lié au port 3392.

3. Ouvrez le Registre Windows (regedit), accédez à ```HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp``` et localisez la clé **WebSocketURI**. La valeur doit être définie sur <strong>https://+:3392/rdp/</strong>.

### <a name="setting-up-the-rd-session-host"></a>Configuration de l'Hôte de session Bureau à distance
Suivez les étapes ci-dessous si le serveur Hôte de session Bureau à distance est différent du serveur du service Broker Bureau à distance :

1. Créez un certificat pour l'ordinateur Hôte de session Bureau à distance, ouvrez-le et copiez la valeur **Thumbprint**.

2. Pour lier ce certificat au port sécurisé 3392, ouvrez une fenêtre PowerShell avec élévation de privilèges et exécutez la commande suivante, en remplaçant **"<thumbprint>"** par la valeur copiée à l'étape précédente :

    ```PowerShell
    netsh http add sslcert ipport=0.0.0.0:3392 certhash="<thumbprint>" appid="{00000000-0000-0000-0000-000000000000}"
    ```

    > [!NOTE]
    > Pour vérifier que le certificat a été correctement lié, exécutez la commande suivante :
    >
    > ```PowerShell
    > netsh http show sslcert
    > ```
    >
    > Dans la liste des liaisons de certificat SSL, assurez-vous que le certificat qui convient est lié au port 3392.

3. Ouvrez le Registre Windows (regedit), accédez à ```HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp``` et localisez la clé **WebSocketURI**. La valeur doit être définie sur <https://+:3392/rdp/>.

### <a name="general-observations"></a>Observations générales

* Assurez-vous que l'Hôte de session Bureau à distance et le serveur du service Broker Bureau à distance exécutent Windows Server 2019.

* Assurez-vous que des certificats publics approuvés sont configurés pour l'Hôte de session Bureau à distance et le serveur du service Broker Bureau à distance.
    > [!NOTE]
    > Si l'Hôte de session Bureau à distance et le serveur du service Broker Bureau à distance partagent le même ordinateur, définissez uniquement le certificat du serveur du service Broker Bureau à distance. Si l'Hôte de session Bureau à distance et le serveur du service Broker Bureau à distance utilisent des ordinateurs différents, les deux doivent être configurés avec des certificats uniques.

* L'**autre nom de l'objet (SAN)** de chaque certificat doit être défini sur le **nom de domaine complet (FQDN)** de l'ordinateur. Le **nom commun (CN)** doit correspondre au SAN pour chaque certificat.

## <a name="how-to-pre-configure-settings-for-remote-desktop-web-client-users"></a>Préconfigurer les paramètres pour les utilisateurs du client web Bureau à distance
Cette section explique comment utiliser PowerShell pour configurer les paramètres de déploiement de votre client web Bureau à distance. Ces applets de commande PowerShell déterminent la capacité d'un utilisateur à modifier les paramètres en fonction des préoccupations de sécurité de votre organisation ou du flux de travail prévu. Les paramètres suivants se trouvent tous dans le volet latéral **​Paramètres** du client web.

### <a name="suppress-telemetry"></a>Supprimer des données de télémétrie
Par défaut, les utilisateurs peuvent choisir d'activer ou de désactiver la collecte des données de télémétrie envoyées à Microsoft. Pour plus d'informations sur les données de télémétrie collectées par Microsoft, consultez notre déclaration de confidentialité via le lien situé dans le volet latéral **À propos de**.

En tant qu'administrateur, vous pouvez choisir de supprimer la collecte de données de télémétrie pour votre déploiement à l'aide de l'applet de commande PowerShell suivante :

   ```PowerShell
    Set-RDWebClientDeploymentSetting -Name "SuppressTelemetry" $true
   ```

Par défaut, l'utilisateur peut activer ou désactiver les données de télémétrie. Une valeur booléenne **$false** correspondra au comportement par défaut du client. Une valeur booléenne **$true** désactivera les données de télémétrie et empêchera l'utilisateur de les activer.

### <a name="remote-resource-launch-method"></a>Méthode de lancement de ressources distantes
Par défaut, les utilisateurs peuvent choisir de lancer des ressources distantes (1) dans le navigateur ou (2) en téléchargeant un fichier .rdp à gérer avec un autre client installé sur leur ordinateur. En tant qu'administrateur, vous pouvez choisir de limiter la méthode de lancement de ressources distantes de votre déploiement à l'aide de la commande Powershell suivante :

   ```PowerShell
    Set-RDWebClientDeploymentSetting -Name "LaunchResourceInBrowser" ($true|$false)
   ```
 Par défaut, l'utilisateur peut sélectionner l'une ou l'autre des méthodes de lancement. Une valeur booléenne **$true** obligera l'utilisateur à lancer les ressources dans le navigateur. Une valeur booléenne **$false** obligera l'utilisateur à lancer les ressources en téléchargeant un fichier .rdp à gérer avec un client RDP installé localement.

### <a name="reset-rdwebclientdeploymentsetting-configurations-to-default"></a>Rétablir les configurations RDWebClientDeploymentSetting par défaut

Pour rétablir un paramètre client web de niveau déploiement à la configuration par défaut, exécutez l’applet de commande PowerShell suivante et utilisez le paramètre -name pour spécifier le paramètre que vous souhaitez rétablir :

   ```PowerShell
    Reset-RDWebClientDeploymentSetting -Name "LaunchResourceInBrowser"
    Reset-RDWebClientDeploymentSetting -Name "SuppressTelemetry"
   ```

## <a name="troubleshooting"></a>Résolution des problèmes

Si un utilisateur signale l'un des problèmes suivants lors de l'ouverture du client web, les sections suivantes vous indiqueront comment y remédier.

### <a name="what-to-do-if-the-users-browser-shows-a-security-warning-when-they-try-to-access-the-web-client"></a>Que faire si le navigateur de l'utilisateur affiche un avertissement de sécurité lorsqu'il tente d'accéder au client web ?

Le rôle Accès aux services Bureau à distance par le web n'utilise peut-être pas de certificat approuvé. Assurez-vous que le rôle Accès aux services Bureau à distance par le web est configuré avec un certificat public approuvé.

Si le problème persiste, il se peut que le nom de votre serveur dans l'URL du client web ne corresponde pas au nom fourni par le certificat d'accès aux services Bureau à distance par le web. Assurez-vous que votre URL utilise le nom de domaine complet du serveur qui héberge le rôle Accès aux services Bureau à distance par le web.

### <a name="what-to-do-if-the-user-cant-connect-to-a-resource-with-the-web-client-even-though-they-can-see-the-items-under-all-resources"></a>Que faire si l'utilisateur ne parvient pas à se connecter à une ressource avec le client web alors qu'il voit les éléments sous Toutes les ressources ?

Si l'utilisateur signale qu'il ne parvient pas à se connecter au client web alors qu'il voit les ressources répertoriées, vérifiez les points suivants :

* Le rôle Passerelle des services Bureau à distance est-il correctement configuré pour utiliser un certificat public approuvé ?
* Les mises à jour requises sont-elles installées sur le serveur de passerelle Bureau à distance ? Assurez-vous que la [mise à jour KB4025334](https://support.microsoft.com/en-us/help/4025334/windows-10-update-kb4025334) est installée sur votre serveur.

Si l'utilisateur reçoit le message d'erreur « Un certificat d'authentification serveur inattendu a été reçu » lorsqu'il tente de se connecter, le message contient l'empreinte du certificat. Recherchez le gestionnaire de certificat du serveur du service Broker Bureau à distance à l'aide de cette empreinte pour trouver le bon certificat. Vérifiez que le certificat est configuré pour le rôle Service Broker Bureau à distance sur la page des propriétés du déploiement Bureau à distance. Après vous être assuré que le certificat n'a pas expiré, copiez-le au format .cer sur le serveur d'accès aux services Bureau à distance par le web et exécutez la commande suivante sur le serveur d'accès aux services Bureau à distance par le web, en remplaçant la valeur entre crochets par le chemin du certificat :

```PowerShell
Import-RDWebClientBrokerCert <certificate file path>
```

### <a name="diagnose-issues-with-the-console-log"></a>Diagnostiquer les problèmes liés au journal de la console

Si vous ne parvenez pas à résoudre le problème en suivant les instructions de résolution fournies dans cet article, vous pouvez essayer de diagnostiquer vous-même la source du problème en consultant le journal de la console dans le navigateur. Le client web fournit une méthode pour enregistrer l'activité du journal de la console du navigateur tout en utilisant le client web pour diagnostiquer les problèmes.

* Sélectionnez les points de suspension dans le coin supérieur droit et accédez à la page **À propos de** à partir du menu déroulant.
* Sous **Capturer les informations de support**, sélectionnez le bouton **Démarrer l'enregistrement**.
* Dans le client web, reproduisez les opérations qui ont généré le problème que vous essayez de diagnostiquer.
* Accédez à la page **À propos de** et sélectionnez **Arrêter l'enregistrement**.
* Votre navigateur télécharge automatiquement un fichier .txt intitulé **RD Console Logs.txt**. Ce fichier contient l'activité complète du journal de la console générée lors de la reproduction du problème cible.

Vous pouvez également accéder directement à la console à partir de votre navigateur. La console se trouve généralement sous les outils de développement. Par exemple, dans Microsoft Edge, vous pouvez accéder au journal en appuyant sur la touche **F12** ou en sélectionnant les points de suspension, puis en accédant à **Autres outils** > **Outils de développement**.

## <a name="get-help-with-the-web-client"></a>Obtenir de l’aide concernant le client web

Si vous rencontrez un problème que les informations contenues dans cet article ne permettent pas de résoudre, vous pouvez le signaler sur [Tech Community](https://aka.ms/wvdtc). Vous pouvez également solliciter ou voter en faveur de nouvelles fonctionnalités via notre [boîte à suggestions](https://remotedesktop.uservoice.com/forums/911494-remote-desktop-web-client).
