---
title: Configurer le client web de Bureau à distance pour vos utilisateurs
description: Décrit comment un administrateur peut configurer le client web de bureau à distance.
ms.prod: windows-server-threshold
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 11/2/2018
ms.topic: article
author: Heidilohr
ms.localizationpriority: medium
ms.openlocfilehash: 45164e9eca0873c82148aa3b7baa179a3f626dd7
ms.sourcegitcommit: d888e35f71801c1935620f38699dda11db7f7aad
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66804973"
---
# <a name="set-up-the-remote-desktop-web-client-for-your-users"></a>Configurer le client web de Bureau à distance pour vos utilisateurs

Le client web de bureau à distance permet aux utilisateurs d’accéder à infrastructure de bureau à distance de votre organisation via un navigateur web compatible. Ils seront en mesure d’interagir avec des applications à distance ou de postes de travail comme ils le feraient avec un ordinateur local où qu’elles soient. Une fois que vous configurez votre client web de bureau à distance, tous vos utilisateurs ont besoin pour commencer est l’URL où ils peuvent accéder le client, leurs informations d’identification et un navigateur web pris en charge.

>[!IMPORTANT]
>Le client web ne prend pas en charge à l’aide du Proxy d’Application Azure et ne pas en charge le Proxy d’Application Web. Consultez [à l’aide de services Bureau à distance avec les services de proxy d’application](../rds-supported-config.md#using-remote-desktop-services-with-application-proxy-services) pour plus d’informations.

## <a name="what-youll-need-to-set-up-the-web-client"></a>Ce dont vous aurez besoin configurer le client web

Avant de commencer, gardez les points suivants à l’esprit :

* Assurez-vous que votre [déploiement de bureau à distance](../rds-deploy-infrastructure.md) est une passerelle Bureau à distance, service Broker pour les connexions Bureau à distance et que l’accès Web Bureau à distance sur Windows Server 2016 ou 2019.
* Assurez-vous que votre déploiement est configuré pour [licences d’accès client par utilisateur](../rds-client-access-license.md) (CAL) plutôt que par périphérique, sinon toutes les licences seront consommées.
* Installer le [mise à jour Windows 10 KB4025334](https://support.microsoft.com/en-us/help/4025334/windows-10-update-kb4025334) sur la passerelle Bureau à distance. Plus tard les mises à jour cumulatives peut contient déjà cette base de connaissances.
* Assurez-vous que des certificats publics approuvés sont configurés pour les rôles de passerelle Bureau à distance et l’accès Web Bureau à distance.
* Assurez-vous que tous vos utilisateurs se connectent les ordinateurs exécutent une des versions du système d’exploitation suivantes :
  * Windows 10
  * Windows Server 2008 R2 ou version ultérieure

Vos utilisateurs verront les meilleures performances se connectant à Windows Server 2016 (ou version ultérieure) et Windows 10 (version 1611 ou version ultérieure).

>[!IMPORTANT]
>Si vous utiliser le client web pendant la période d’évaluation et installé une version antérieure à 1.0.0, vous devez d’abord désinstaller l’ancien client avant de passer à la nouvelle version. Si vous recevez une erreur indiquant « le client web a été installé à l’aide d’une version antérieure de RDWebClientManagement et doit être retiré avant de déployer la nouvelle version », procédez comme suit :
>
>1. Ouvrez une invite de PowerShell avec élévation de privilèges.
>2. Exécutez **Uninstall-Module RDWebClientManagement** pour désinstaller le nouveau module.
>3. Fermez et rouvrez l’invite PowerShell avec élévation de privilèges.
>4. Exécutez **RDWebClientManagement d’Install-Module - RequiredVersion \<ancienne version > pour installer le module ancien.**
>5. Exécutez **RDWebClient de désinstallation** pour désinstaller l’ancien client web.
>6. Exécutez **Uninstall-Module RDWebClientManagement** pour désinstaller l’ancien module.
>7. Fermez et rouvrez l’invite PowerShell avec élévation de privilèges.
>8. Procédez comme suit les étapes d’installation normale.

## <a name="how-to-publish-the-remote-desktop-web-client"></a>Comment publier le client web de bureau à distance

Pour installer le client web pour la première fois, procédez comme suit :

1. Sur le serveur Service Broker pour les connexions Bureau à distance, obtenir le certificat utilisé pour les connexions Bureau à distance et l’exporter sous forme de fichier .cer. Copiez le fichier .cer à partir de l’agent de connexion Bureau à distance au serveur exécutant le rôle Web de bureau à distance.
2. Sur le serveur d’accès Web de bureau à distance, ouvrez une invite de PowerShell avec élévation de privilèges.
3. Sur Windows Server 2016, mettez à jour le module PowerShellGet, étant donné que la version de la boîte de réception ne prend pas en charge l’installation du module de gestion de client web. Pour mettre à jour PowerShellGet, exécutez l’applet de commande suivante :
    ```PowerShell
    Install-Module -Name PowerShellGet -Force
    ```

    >[!IMPORTANT]
    >Vous devrez redémarrer PowerShell avant la mise à jour peut prendre effet, sinon que le module peut ne pas fonctionne.

4. Installer le module PowerShell de gestion du client de web de bureau à distance à partir de PowerShell gallery avec cette applet de commande :
    ```PowerShell
    Install-Module -Name RDWebClientManagement
    ```

5. Après cela, exécutez l’applet de commande suivante pour télécharger la dernière version du client web Bureau à distance :
    ```PowerShell
    Install-RDWebClientPackage
    ```

6. Ensuite, exécutez cette applet de commande avec la valeur entre crochets est remplacée par le chemin d’accès du fichier .cer que vous avez copiée à partir du courtier de bureau à distance :
    ```PowerShell
    Import-RDWebClientBrokerCert <.cer file path>
    ```

7. Enfin, exécutez cette applet de commande pour publier le client web de bureau à distance :
    ```PowerShell
    Publish-RDWebClientPackage -Type Production -Latest
    ```
    Assurez-vous que vous pouvez accéder le client web à l’URL de client web avec le nom de votre serveur, sous la forme <https://server_FQDN/RDWeb/webclient/index.html>. Il est important d’utiliser le nom du serveur qui correspond au certificat public de l’accès Web Bureau à distance dans l’URL (généralement le serveur nom de domaine complet).

    >[!NOTE]
    >Lorsque vous exécutez le **RDWebClientPackage de publication** applet de commande, vous pouvez voir un avertissement indiquant une CAL par périphérique ne sont pas pris en charge, même si votre déploiement est configuré pour les licences d’accès client par utilisateur. Si votre déploiement utilise des CAL par utilisateur, vous pouvez ignorer cet avertissement. Nous l’affichons s’assurer que vous êtes informé de la limitation de la configuration.
8. Lorsque vous êtes prêt pour les utilisateurs à accéder au client web, simplement les envoyer l’URL de client web que vous avez créé.

>[!NOTE]
>Pour afficher une liste de toutes les applets de commande pris en charge pour le module RDWebClientManagement, exécutez l’applet de commande suivante dans PowerShell :
>```PowerShell
>Get-Command -Module RDWebClientManagement
>```

## <a name="how-to-update-the-remote-desktop-web-client"></a>Comment mettre à jour le client web de bureau à distance

Lorsqu’une nouvelle version du client web Bureau à distance est disponible, procédez comme suit pour mettre à jour le déploiement avec le nouveau client :

1. Ouvrez une invite de PowerShell avec élévation de privilèges sur le serveur d’accès Web de bureau à distance et exécutez l’applet de commande suivante pour télécharger la dernière version disponible du client web :
    ```PowerShell
    Install-RDWebClientPackage
    ```

2. Si vous le souhaitez, vous pouvez publier le client pour le test avant la sortie officielle en exécutant cette applet de commande :
    ```PowerShell
    Publish-RDWebClientPackage -Type Test -Latest
    ```

    Le client doit apparaître sur l’URL de test qui correspond à l’URL de votre client web (par exemple, <https://server_FQDN/RDWeb/webclient-test/index.html>).
3. Publier le client pour les utilisateurs en exécutant l’applet de commande suivante :
    ```PowerShell
    Publish-RDWebClientPackage -Type Production -Latest
    ```

    Cela remplacera le client pour tous les utilisateurs lorsqu’ils relancer la page web.

## <a name="how-to-uninstall-the-remote-desktop-web-client"></a>Comment désinstaller le client web de bureau à distance

Pour supprimer toutes les traces du client web, procédez comme suit :

1. Sur le serveur d’accès Web de bureau à distance, ouvrez une invite de PowerShell avec élévation de privilèges.
2. Annuler la publication les clients de Test et de Production, désinstallez tous les packages locales et supprimer les paramètres de client web :

   ```PowerShell
   Uninstall-RDWebClient
   ```

3. Désinstaller le module PowerShell de gestion du client de web de bureau à distance :

   ```PowerShell
   Uninstall-Module -Name RDWebClientManagement
   ```

## <a name="how-to-install-the-remote-desktop-web-client-without-an-internet-connection"></a>Comment installer le client web Bureau à distance sans connexion internet

Suivez ces étapes pour déployer le client web sur un serveur d’accès Web de bureau à distance n’ayant pas d’une connexion internet.

> [!NOTE]
> Installation sans une connexion internet n’est pas disponible dans la version 1.0.1 et versions ultérieures du module PowerShell de RDWebClientManagement.

> [!NOTE]
> Vous devez toujours un administrateur PC connecté à internet pour télécharger les fichiers nécessaires avant de les transférer vers le serveur en mode hors connexion.

> [!NOTE]
> Le PC de l’utilisateur final a besoin d’une connexion internet pour l’instant. Ce problème sera résolu dans une prochaine version du client pour fournir un scénario hors connexion complet.

### <a name="from-a-device-with-internet-access"></a>À partir d’un appareil connecté à internet

1. Ouvrez une invite de PowerShell.

2. Importer le module PowerShell de gestion du client de web de bureau à distance à partir de PowerShell gallery :
    ```PowerShell
    Import-Module -Name RDWebClientManagement
    ```

3. Téléchargez la dernière version du client web Bureau à distance pour l’installation sur un autre appareil :
    ```PowerShell
    Save-RDWebClientPackage "C:\WebClient\"
    ```

4. Téléchargez la dernière version du module PowerShell de RDWebClientManagement :
    ```PowerShell
    Find-Module -Name "RDWebClientManagement" -Repository "PSGallery" | Save-Module -Path "C:\WebClient\"
    ```

5. Copiez le contenu de « C:\WebClient\" au serveur d’accès Web de bureau à distance.

### <a name="from-the-rd-web-access-server"></a>À partir du serveur d’accès Web de bureau à distance

Suivez les instructions sous [comment publier le client web de bureau à distance](remote-desktop-web-client-admin.md#how-to-publish-the-remote-desktop-web-client), en remplaçant les étapes 4 et 5 par le code suivant.

4. Importez le module PowerShell de gestion du client de web de bureau à distance à partir du dossier local :
    ```PowerShell
    Import-Module -Name "C:\WebClient\"
    ```

5. Déployer la dernière version du client web Bureau à distance à partir du dossier local (à remplacer par le fichier zip approprié) :
    ```PowerShell
    Install-RDWebClientPackage -Source "C:\WebClient\rdwebclient-1.0.1.zip"
    ```

## <a name="connecting-to-rd-broker-without-rd-gateway-in-windows-server-2019"></a>Connexion à Service Broker pour les services Bureau à distance sans passerelle Bureau à distance dans Windows Server 2019
Cette section décrit comment activer une connexion de client web à un répartiteur de bureau à distance sans une passerelle Bureau à distance dans Windows Server 2019.

### <a name="setting-up-the-rd-broker-server"></a>Configuration du serveur Bureau à distance

#### <a name="follow-these-steps-if-there-is-no-certificate-bound-to-the-rd-broker-server"></a>Suivez ces étapes si aucun certificat n’est lié au serveur Service Broker pour les services Bureau à distance

1. Ouvrez **le Gestionnaire de serveur** > **des Services Bureau à distance**.

2. Dans **vue d’ensemble du déploiement** section, sélectionnez le **tâches** menu déroulant.

3. Sélectionnez **modifier les propriétés de déploiement**, une nouvelle fenêtre intitulée **propriétés de déploiement** s’ouvre.

4. Dans le **propriétés de déploiement** fenêtre, sélectionnez **certificats** dans le menu de gauche.

5. Dans la liste des niveaux de certificat, sélectionnez **Broker pour les connexions Bureau à distance - activer l’authentification unique**. Vous avez deux options : (1) créer un nouveau certificat ou (2) un certificat existant.

#### <a name="follow-these-steps-if-there-is-a-certificate-previously-bound-to-the-rd-broker-server"></a>Suivez ces étapes s’il existe un certificat précédemment lié au serveur Service Broker pour les services Bureau à distance

1. Ouvrez le certificat lié au service Broker et copie le **empreinte** valeur.

2. Pour lier ce certificat au port sécurisé 3392, ouvrez une fenêtre PowerShell avec élévation de privilèges et exécutez la commande suivante en remplaçant de la commande **« < thumbprint > »** avec la valeur copiée à partir de l’étape précédente :

    ```PowerShell
    netsh http add sslcert ipport=0.0.0.0:3392 certhash="<thumbprint>" certstorename="Remote Desktop" appid="{00000000-0000-0000-0000-000000000000}"
    ```

    > [!NOTE]
    > Pour vérifier si le certificat a été lié correctement, exécutez la commande suivante :
    >
    > ```PowerShell
    > netsh http show sslcert
    > ```
    >
    > Dans la liste des liaisons de certificat SSL, vérifiez que le certificat correct est lié au port 3392.

3. Ouvrez le Registre Windows (regedit) et le nagivate à ```HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp``` et recherchez la clé **WebSocketURI**. La valeur doit être définie sur <strong>https://+:3392/rdp/</strong>.

### <a name="setting-up-the-rd-session-host"></a>Configuration de l’hôte de Session Bureau à distance
Si le serveur hôte de Session Bureau à distance est différent du serveur Bureau à distance, procédez comme suit :

1. Créer un certificat pour l’ordinateur hôte de Session Bureau à distance, ouvrez-le et copiez le **empreinte** valeur.

2. Pour lier ce certificat au port sécurisé 3392, ouvrez une fenêtre PowerShell avec élévation de privilèges et exécutez la commande suivante en remplaçant de la commande **« < thumbprint > »** avec la valeur copiée à partir de l’étape précédente :

    ```PowerShell
    netsh http add sslcert ipport=0.0.0.0:3392 certhash="<thumbprint>" appid="{00000000-0000-0000-0000-000000000000}"
    ```

    > [!NOTE]
    > Pour vérifier si le certificat a été lié correctement, exécutez la commande suivante :
    >
    > ```PowerShell
    > netsh http show sslcert
    > ```
    >
    > Dans la liste des liaisons de certificat SSL, vérifiez que le certificat correct est lié au port 3392.

3. Ouvrez le Registre Windows (regedit) et le nagivate à ```HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp``` et recherchez la clé **WebSocketURI**. La valeur doit être définie sur <https://+:3392/rdp/>.

### <a name="general-observations"></a>Observations générales

* Assurez-vous que l’hôte de Session Bureau à distance et le Bureau à distance Broker server exécutent Windows Server 2019.

* Assurez-vous que public approuvé de certificats sont configurés pour le serveur de l’hôte de Session Bureau à distance et le Bureau à distance.
    > [!NOTE]
    > Si l’hôte de Session Bureau à distance et le serveur du service Broker de bureau à distance partagent le même ordinateur, définissez uniquement le certificat de serveur Bureau à distance. Si le serveur hôte de Session Bureau à distance et Bureau à distance Broker utilise des ordinateurs différents, les deux doivent être configurés avec les certificats uniques.

* Le **nom SAN (Subject Alternative)** pour chaque certificat doit être définie sur l’ordinateur **nom de domaine complet (FQDN)** . Le **nom commun (CN)** doit correspondre le réseau SAN pour chaque certificat.

## <a name="how-to-pre-configure-settings-for-remote-desktop-web-client-users"></a>Comment préconfigurer les paramètres pour les utilisateurs de clients web Bureau à distance
Cette section vous indique comment utiliser PowerShell pour configurer les paramètres pour votre déploiement de client web Bureau à distance. Ces applets de commande PowerShell contrôlent la capacité d’un utilisateur pour modifier les paramètres selon les problèmes de sécurité de votre organisation ou prévu de flux de travail. Les paramètres suivants se trouvent toutes dans le **paramètres** panneau latéral du client web. 

### <a name="suppress-telemetry"></a>Supprimer les données de télémétrie
Par défaut, les utilisateurs peuvent choisir d’activer ou désactiver la collecte des données de télémétrie sont envoyées à Microsoft. Pour plus d’informations sur les données de télémétrie Microsoft collecte, reportez-vous à notre déclaration de confidentialité via le lien dans le **sur** panneau latéral.

En tant qu’administrateur, vous pouvez choisir de supprimer la collection de données de télémétrie pour votre déploiement à l’aide de l’applet de commande PowerShell suivante :

   ```PowerShell
    Set-RDWebClientDeploymentSetting -SuppressTelemetry $true
   ```

Par défaut, l’utilisateur peut sélectionner pour activer ou désactiver la télémétrie. Valeur booléenne **$false** correspondra le comportement du client par défaut. Valeur booléenne **$true** désactive la télémétrie et limite l’utilisateur à partir de l’activation de la télémétrie.

### <a name="remote-resource-launch-method"></a>Méthode de lancement de ressource distante
Par défaut, les utilisateurs peuvent choisir lancer des ressources distantes (1) dans le navigateur ou (2) en téléchargeant un fichier .rdp à gérer avec un autre client installé sur leur ordinateur. En tant qu’administrateur, vous pouvez choisir de restreindre la méthode de lancement des ressources à distance pour votre déploiement avec la commande Powershell suivante :

   ```PowerShell
    Set-RDWebClientDeploymentSetting -LaunchResourceInBrowser ($true|$false)
   ```
 Par défaut, l’utilisateur peut sélectionner une méthode de lancement. Valeur booléenne **$true** forcera l’utilisateur de lancer des ressources dans le navigateur. Valeur booléenne **$false** forcera l’utilisateur de lancer des ressources en téléchargeant un fichier .rdp à gérer avec un client RDP installé localement.

### <a name="reset-rdwebclientdeploymentsetting-configurations-to-default"></a>Réinitialiser les configurations RDWebClientDeploymentSetting par défaut
Pour réinitialiser tous les paramètres du client au niveau du déploiement web pour les configurations par défaut, exécutez l’applet de commande PowerShell suivante :

   ```PowerShell
    Reset-RDWebClientDeploymentSetting 
   ```

## <a name="troubleshooting"></a>Résolution des problèmes

Si un utilisateur signale tous les problèmes suivants lors de l’ouverture du client web pour la première fois, les sections suivantes vous indiquera que faire pour les corriger.

### <a name="what-to-do-if-the-users-browser-shows-a-security-warning-when-they-try-to-access-the-web-client"></a>Que faire si le navigateur affiche un avertissement de sécurité quand ils tentent d’accéder au client web

Le rôle accès Web de bureau à distance n’utilisez ne peut-être pas un certificat approuvé. Assurez-vous que le rôle accès Bureau à distance par le Web est configuré avec un certificat approuvé publiquement.

Si cela ne fonctionne pas, le nom de votre serveur dans le site web client URL ne peut pas correspondre au nom fourni par le certificat Web Bureau à distance. Assurez-vous que votre URL utilise le nom de domaine complet du serveur qui héberge le rôle Web de bureau à distance.

### <a name="what-to-do-if-the-user-cant-connect-to-a-resource-with-the-web-client-even-though-they-can-see-the-items-under-all-resources"></a>Que faire si l’utilisateur Impossible de se connecter à une ressource avec le client web même s’ils peuvent voir les éléments sous toutes les ressources

Si l’utilisateur signale qu’elles ne peuvent pas se connecter avec le client web même s’ils peuvent voir les ressources répertoriées, vérifiez les points suivants :

* Le rôle de passerelle Bureau à distance est correctement configuré pour utiliser un certificat public approuvé ?
* Le serveur de passerelle Bureau à distance a-t-il les mises à jour requises installées ? Assurez-vous que votre serveur [la mise à jour KB4025334](https://support.microsoft.com/en-us/help/4025334/windows-10-update-kb4025334) installé.

Si l’utilisateur obtient une erreur « certificat d’authentification serveur inattendue a été reçu » lorsqu’ils tentent de se connecter, puis le message affiche l’empreinte du certificat du message. Rechercher le Gestionnaire de certificat du serveur Bureau à distance à l’aide de cette empreinte numérique pour rechercher le certificat approprié. Vérifiez que le certificat est configuré pour être utilisé pour le rôle Service Broker pour les services Bureau à distance dans la page de propriétés de déploiement de bureau à distance. Une fois s’assurer que le certificat n’a pas expiré, copiez le certificat au format de fichier .cer pour le serveur d’accès Web de bureau à distance et exécutez la commande suivante sur le serveur d’accès Web de bureau à distance avec la valeur entre crochets est remplacée par le chemin d’accès de fichier du certificat :

```PowerShell
Import-RDWebClientBrokerCert <certificate file path>
```

### <a name="diagnose-issues-with-the-console-log"></a>Diagnostiquez les problèmes avec le journal de console

Si vous ne pouvez pas résoudre le problème selon les instructions de dépannage dans cet article, vous pouvez essayer de diagnostiquer la source du problème en regardant la console se connecter dans le navigateur. Le client web fournit une méthode d’enregistrement de l’activité de journal de console de navigateur tout en utilisant le client web pour aider à diagnostiquer les problèmes.

* Sélectionnez les points de suspension dans le coin supérieur droit et accédez à la **sur** page dans le menu déroulant.
* Sous **Capture les informations de support** sélectionner le **démarrer l’enregistrement** bouton.
* Effectuer les opérations dans le client web qui a produit le problème que vous tentez de diagnostiquer.
* Accédez à la **sur** page et sélectionnez **arrêter l’enregistrement**.
* Votre navigateur télécharge automatiquement un fichier .txt intitulé **Logs.txt de Console de bureau à distance**. Ce fichier contiendra l’activité du journal de console complète générée lors de la reproduction du problème cible.

La console peut également être accessible directement par le biais de votre navigateur. La console se trouve généralement sous les outils de développement. Par exemple, vous pouvez accéder au journal dans Microsoft Edge en appuyant sur la **F12** clé, ou en sélectionnant les points de suspension, puis navigué **davantage d’outils** > **lesoutilsdedéveloppement**.

## <a name="get-help-with-the-web-client"></a>Obtenir de l’aide avec le client web

Si vous avez rencontré un problème qui ne peut pas être résolu par les informations contenues dans cet article, vous pouvez [envoyez-nous un courrier électronique](mailto:rdwbclnt@microsoft.com) à signaler. Vous pouvez également demander ou voter pour les nouvelles fonctionnalités à notre [boîte à suggestions](https://aka.ms/rdwebfbk).
