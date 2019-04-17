---
title: Gérer les certificats pour les logiciels définis mise en réseau
description: Vous pouvez utiliser cette rubrique pour savoir comment gérer des certificats pour Northbound du contrôleur de réseau et les communications Southbound lorsque vous déployez le logiciel défini de mise en réseau (SDN) dans Windows Server2016 Datacenter.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: c4e2f6c7-0364-4bf8-bb66-9af59c0bbd74
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3036de9161cabc2f3a85a1d3b2ce7739f0ff6bd3
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="manage-certificates-for-software-defined-networking"></a>Gérer les certificats pour les logiciels définis mise en réseau

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette rubrique pour savoir comment gérer des certificats pour Northbound du contrôleur de réseau et les communications Southbound lorsque vous déployez le logiciel défini de mise en réseau de \(SDN\) dans Windows Server2016 Datacenter et vous utilisez SystemCenter Virtual Machine Manager \(SCVMM\) en tant que votre client de gestion SDN.

>[!NOTE]
>Pour plus d’informations sur le contrôleur de réseau, voir [contrôleur de réseau](../technologies/network-controller/Network-Controller.md).

Si vous n’utilisez pas Kerberos pour sécuriser la communication du contrôleur de réseau, vous pouvez utiliser des certificats X.509 pour l’authentification, d’autorisation et le chiffrement.

SDN dans Windows Server2016 Datacenter prend en charge les deux signé intégrée et l’autorité de Certification \ (CA\)-signé de certificats X.509. Cette rubrique fournit des instructions détaillées pour la création de ces certificats et en les appliquant pour sécuriser les canaux de communication Northbound du contrôleur réseau avec des clients de gestion et communications Southbound avec les périphériques réseau, tels que l’équilibrage de charge logicielle \(SLB\).
.
Lorsque vous utilisez l’authentification basée sur les certificate\, vous devez inscrire un certificat sur des nœuds de contrôleur de réseau qui est utilisé de plusieurs façons.

1. Le chiffrement des communications Northbound avec \(SSL\) (Secure Sockets Layer) entre les nœuds de contrôleur de réseau et des clients de gestion, tels que SystemCenter Virtual Machine Manager.
2. Authentification entre les nœuds de contrôleur de réseau et les appareils Southbound et services, tels que les ordinateurs hôtes Hyper-V et les équilibreurs de charge logicielle \(SLBs\).

## <a name="creating-and-enrolling-an-x509-certificate"></a>Création et l’inscription d’un certificat X.509

Vous pouvez créer et inscrire un certificat signé intégrée ou un certificat émis par une autorité de certification.

>[!NOTE]
>Lorsque vous utilisez SCVMM pour déployer le contrôleur de réseau, vous devez spécifier le certificat X.509 utilisé pour chiffrer les communications Northbound lors de la configuration du modèle de Service de contrôleur de réseau.

La configuration de certificat doit inclure les valeurs suivantes.

- La valeur de la **RestEndPoint** zone de texte doit être le \(FQDN\) réseau contrôleur nom de domaine complet ou l’adresse IP. 
- Le **RestEndPoint** valeur doit correspondre au nom de sujet \ (nom commun, CN\) du certificat X.509.

### <a name="creating-a-self-signed-x509-certificate"></a>Création d’un certificat X.509 signé intégrée

Vous pouvez créer un certificat X.509 auto-signé et l’exporter avec la clé privée \ (protégé avec un password\) en suivant ces étapes pour les déploiements single\ et multiple\ noeuds du contrôleur de réseau.

Lorsque vous créez des certificats signés par intégrée, vous pouvez utiliser les instructions suivantes.

- Vous pouvez utiliser l’adresse IP du point de terminaison REST contrôleur de réseau pour le paramètre DnsName - mais ce n’est pas recommandé, car elle requiert que les nœuds de contrôleur de réseau sont tous situés dans un sous-réseau de gestion unique \ (par exemple, sur un seul rack\)
- Pour les déploiements de nœud CN multiples, le nom DNS que vous spécifiez devient le nom de domaine complet du Cluster de contrôleur de réseau \ (enregistrements d’hôte DNS A sont créés automatiquement. \) 
- Pour les déploiements de contrôleur de réseau de nœud unique, le nom DNS peut être nom d’hôte du contrôleur de réseau suivi du nom de domaine complet.

#### <a name="multiple-node"></a>Plusieurs nœuds

Vous pouvez utiliser le [New-SelfSignedCertificate](https://technet.microsoft.com/en-us/itpro/powershell/windows/pkiclient/new-selfsignedcertificate) commande Windows PowerShell pour créer un certificat signé intégrée.

**Syntaxe**

    New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "<YourNCComputerName>" -DnsName @("<NCRESTName>")

**Exemple d’utilisation**

    New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "MultiNodeNC" -DnsName @("NCCluster.Contoso.com")

#### <a name="single-node"></a>Nœud unique

Vous pouvez utiliser le [New-SelfSignedCertificate](https://technet.microsoft.com/en-us/itpro/powershell/windows/pkiclient/new-selfsignedcertificate) commande Windows PowerShell pour créer un certificat signé intégrée.

**Syntaxe**

    New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "<YourNCComputerName>" -DnsName @("<NCFQDN>")

**Exemple d’utilisation**

    New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "SingleNodeNC" -DnsName @("SingleNodeNC.Contoso.com")

### <a name="creating-a-ca-signed-x509-certificate"></a>Création d’un certificat X.509 signé CA\

Pour créer un certificat à l’aide d’une autorité de certification, vous devez avoir déployé un \(PKI\) Infrastructure à clé publique avec les Services de certificats ActiveDirectory \(ADCS\). 

>[!NOTE]
>Vous pouvez utiliser un tiers autorités de certification ou les outils, tels qu’openssl, pour créer un certificat à utiliser avec le contrôleur de réseau, mais les instructions fournies dans cette rubrique sont spécifiques aux services ADCS. Pour savoir comment utiliser une autorité de certification tierce ou l’outil, consultez la documentation du logiciel que vous utilisez.

Création d’un certificat avec une autorité de certification inclut les étapes suivantes.

1. Vous ou votre organisation administrateur de domaine ou sécurité configure le modèle de certificat
2. Vous ou votre organisation réseau contrôleur administrateur ou Administrateur SCVMM demande un nouveau certificat à partir de l’autorité de certification.

#### <a name="certificate-configuration-requirements"></a>Configuration requise du certificat

Quand vous configurez un modèle de certificat à l’étape suivante, assurez-vous que le modèle que vous configurez inclut les éléments requis suivants.

1. Le nom de sujet du certificat doit être le nom de domaine complet de l’ordinateur hôte Hyper-V
2. Le certificat doit être placé dans le magasin personnel d’ordinateur local (mon – cert: \localmachine\my)
3. Le certificat doit avoir les deux l’authentification du serveur (EKU: 1.3.6.1.5.5.7.3.1) et l’authentification du Client (EKU: 1.3.6.1.5.5.7.3.2) stratégies d’Application.

>[!NOTE]
>Si le personnel \ (mon – cert: \localmachine\my\) certificat Windows store sur l’hôte Hyper\-V a plusieurs X.509 de certificat avec le nom de sujet (CN) comme hôte de nom de domaine complet \(FQDN\), assurez-vous que le certificat qui sera utilisé par SDN dispose d’une propriété d’utilisation améliorée de la clé personnalisée supplémentaire avec l’OID 1.3.6.1.4.1.311.95.1.1.1. Dans le cas contraire, la communication entre le contrôleur de réseau et l’ordinateur hôte peut ne pas fonctionne.

#### <a name="to-configure-the-certificate-template"></a>Pour configurer le modèle de certificat
  
>[!NOTE]
>Avant d’effectuer cette procédure, vous devez passer en revue les conditions requises et les modèles de certificat dans la console Modèles de certificats. Vous pouvez modifier un modèle existant ou créer un doublon d’un modèle existant et ensuite modifier votre copie du modèle. Création d’une copie d’un modèle existant est recommandée.

1. Sur le serveur où les services ADCS est installé, dans le Gestionnaire de serveur, cliquez sur **outils**, puis cliquez sur **autorité de Certification**. L’autorité de Certification MicrosoftManagement Console \(MMC\) s’ouvre. 
2. Dans la console MMC, double-cliquez sur le nom de l’autorité de certification, faites un clic droit **modèles de certificats**, puis cliquez sur **gérer**.
3. La console Modèles de certificats s’ouvre. Tous les modèles de certificats sont affichés dans le volet d’informations.
4. Dans le volet d’informations, cliquez sur le modèle que vous souhaitez dupliquer.
5.  Cliquez sur le **Action** menu, puis cliquez sur **dupliquer le modèle**. Le modèle **propriétés** boîte de dialogue s’ouvre.
6.  Dans le modèle **propriétés** boîte de dialogue le **nom de l’objet**, cliquez sur **fourni dans la demande**. \ (Ce paramètre est requis pour les certificats SSL de contrôleur de réseau. \)
7.  Dans le modèle **propriétés** boîte de dialogue le **traitement de la demande**, vérifiez que **autoriser à exporter la clé privée** est sélectionné. Vérifiez également que le **Signature et chiffrement** objectif est sélectionné.
8.  Dans le modèle **propriétés** boîte de dialogue le **Extensions** onglet, sélectionnez **utilisation de la clé**, puis cliquez sur **modifier**.
9.  Dans **Signature**, assurez-vous que **Signature numérique** est sélectionné.
10.  Dans le modèle **propriétés** boîte de dialogue le **Extensions** onglet, sélectionnez **stratégies d’Application**, puis cliquez sur **modifier**.
11.  Dans **stratégies d’Application**, assurez-vous que **l’authentification du Client** et **l’authentification du serveur** sont répertoriés.
12.  Enregistrer la copie du modèle de certificat avec un nom unique, par exemple **modèle de contrôleur de réseau**.

#### <a name="to-request-a-certificate-from-the-ca"></a>Pour demander un certificat à partir de l’autorité de certification

Vous pouvez utiliser le composant logiciel enfichable Certificats pour demander des certificats. Vous pouvez demander tout type de certificat qui a été préconfiguré et mis à disposition par un administrateur de l’autorité de certification qui traite la demande de certificat.

**Les utilisateurs** ou local **administrateurs** est l’appartenance au groupe minimale requise pour effectuer cette procédure.

1. Ouvrez le composant logiciel enfichable Certificats pour un ordinateur.
2. Dans l’arborescence de la console, cliquez sur **certificats \(Local Computer\)**. Sélectionnez le **personnel** magasin de certificats.
3. Sur le **Action** menu, pointez sur ** toutes les tâches **, puis cliquez sur **demander un nouveau certificat** pour démarrer l’Assistant Inscription de certificats. Cliquez sur **suivant**.
4. Sélectionnez le **configurés par votre administrateur** stratégie d’inscription de certificat et cliquez sur **suivant**.
5. Sélectionnez le **stratégie d’inscription d’ActiveDirectory** \ (basé sur le modèle d’autorité de certification que vous avez configuré dans la précédente section\).
6. Développez le **détails** section et configurer les éléments suivants.
    1. Vérifiez que **utilisation de la clé** inclut à la fois ** Signature numérique ** et **chiffrement de clé**.
    2. Vérifiez que **stratégies d’Application** inclut à la fois **l’authentification du serveur** \(1.3.6.1.5.5.7.3.1\) et **l’authentification du Client** \(1.3.6.1.5.5.7.3.2\).
7. Cliquez sur **propriétés**.
8. Sur le **objet** onglet **nom de l’objet**, dans **Type**, sélectionnez **nom commun**. Dans la valeur, spécifiez **point de terminaison REST de contrôleur de réseau**.
9. Cliquez sur **appliquer**, puis cliquez sur **OK**.
10. Cliquez sur **inscrire**.

Dans la console MMC Certificats, cliquez sur le magasin personnel pour afficher le certificat que vous avez inscrit à partir de l’autorité de certification.

## <a name="exporting-and-copying-the-certificate-to-the-scvmm-library"></a>Exporter et copier le certificat à la bibliothèque SCVMM

Après avoir créé un certificat signé intégrée ou la signature CA\, vous devez exporter le certificat avec la clé privée \(in.pfx format\) et sans la clé privée \ (dans l’applet de commande format .cer Base-64) à partir du composant logiciel enfichable Certificats. 

Vous devez ensuite copier les deux fichiers exportés à le **ServerCertificate.cr** et **NCCertificate.cr** dossiers que vous avez spécifié à l’heure lorsque vous avez importé le modèle de Service CN.

1. Ouvrez le (certlm.msc) composant logiciel enfichable Certificats et recherchez le certificat dans le magasin de certificats personnel de l’ordinateur local.
2. Cliquez sur le certificat, clic droit **toutes les tâches**, puis cliquez sur **exporter**. L’Assistant Exportation de certificat s’ouvre. Cliquez sur **suivant**.
3. Sélectionnez **Oui**, exporter l’option de clé privée, cliquez sur **suivant**.
4. Choisissez **échange d’informations personnelles - PKCS #12 (.PFX)** et accepter la valeur par défaut à **inclure tous les certificats dans le chemin d’accès de certification** si possible.
5. Affecter les utilisateurs/groupes et un mot de passe pour le certificat que vous exportez, cliquez sur **suivant**.
6. Sur la page fichier à exporter, naviguez jusqu'à l’emplacement où vous souhaitez placer le fichier exporté et lui donner un nom.
7. De même, exportez le certificat dans. Format CER. Remarque: Pour exporter vers. Format CER, décochez la case Oui, exporter l’option de clé privée.
8. Copie. PFX dans le dossier ServerCertificate.cr.
9. Copie. Fichier CER dans le dossier NCCertificate.cr.

Lorsque vous avez terminé, actualiser ces dossiers dans la bibliothèque SCVMM et vérifiez que vous disposez de ces certificats copiés. Poursuivez la Configuration du modèle de Service contrôleur réseau et le déploiement.

## <a name="authenticating-southbound-devices-and-services"></a>L’authentification des services et périphériques Southbound 

Communication de contrôleur de réseau avec des ordinateurs hôtes et les périphériques SLB MUX utilise des certificats pour l’authentification. Communication avec les ordinateurs hôtes est via le protocole OVSDB tandis que la communication avec les périphériques SLB MUX est via le protocole WCF.

### <a name="hyper-v-host-communication-with-network-controller"></a>Communication avec le contrôleur réseau hôte Hyper-V

Pour la communication avec les ordinateurs hôtes Hyper-V sur OVSDB, le contrôleur de réseau doit présenter un certificat pour les ordinateurs hôtes. Par défaut, SCVMM sélectionne le certificat SSL configuré sur le contrôleur de réseau et l’utilise pour la communication southbound avec les ordinateurs hôtes.

C’est pourquoi le certificat SSL doit avoir l’EKU d’authentification Client configuré. Ce certificat est configuré sur «Serveurs» reste ressource \ (les hôtes Hyper-V sont représentés dans le contrôleur de réseau comme un resource\ Server) et peut être affiché en exécutant la commande Windows PowerShell **Get-NetworkControllerServer**.

Voici un exemple de la ressource reste serveur partiel.

      "resourceId": "host31.fabrikam.com",
      "properties": {
        "connections": [
          {
            "managementAddresses": [
               "host31.fabrikam.com"
            ],
            "credential": {
              "resourceRef": "/credentials/a738762f-f727-43b5-9c50-cf82a70221fa"
            },
            "credentialType": "X509Certificate"
          }
        ],

Pour l’authentification mutuelle, l’ordinateur hôte Hyper-V doit également disposer un certificat pour communiquer avec le contrôleur de réseau. 

Vous pouvez inscrire le certificat à partir d’une autorité de Certification de \(CA\). Si un certificat d’autorité de certification basée est introuvable sur l’ordinateur hôte, SCVMM crée un certificat auto-signé et il configure sur l’ordinateur hôte.

Contrôleur de réseau et les certificats d’ordinateur hôte Hyper-V doivent être approuvés par les autres. Certificat de racine du certificat d’ordinateur hôte Hyper-V doit être présent dans le réseau contrôleur Trusted Root Certification Authorities stocker pour l’ordinateur Local et vice versa. 

Lorsque vous utilisez des certificats signés par intégrée, SCVMM garantit que les certificats requis sont présents dans le magasin Autorités de Certification racine de confiance pour l’ordinateur Local. 

Si vous utilisez des certificats d’autorité de certification en fonction des ordinateurs hôtes Hyper-V, vous devez vous assurer que le certificat d’autorité de certification racine est présent sur le magasin d’autorités de Certification racine de confiance du contrôleur de réseau de l’ordinateur Local.

### <a name="software-load-balancer-mux-communication-with-network-controller"></a>Équilibrage de la charge logiciel MUX la Communication avec le contrôleur de réseau

Le \(MUX\) multiplexeur d’équilibrage de charge logicielle et le contrôleur de réseau communiquent via le protocole WCF, à l’aide de certificats pour l’authentification.

Par défaut, SCVMM sélectionne le certificat SSL configuré sur le contrôleur de réseau et l’utilise pour la communication southbound avec les périphériques Mux. Ce certificat est configuré sur le «NetworkControllerLoadBalancerMux» reste des ressources et peuvent être affichés en exécutant l’applet de commande Powershell **Get-NetworkControllerLoadBalancerMux**.

Exemple de MUX reste ressource \(partial\):

      "resourceId": "slbmux1.fabrikam.com",
      "properties": {
        "connections": [
          {
            "managementAddresses": [
               "slbmux1.fabrikam.com"
            ],
            "credential": {
              "resourceRef": "/credentials/a738762f-f727-43b5-9c50-cf82a70221fa"
            },
            "credentialType": "X509Certificate"
          }
        ],

Pour l’authentification mutuelle, vous devez également posséder un certificat sur les appareils SLB MUX. Ce certificat est automatiquement configuré par SCVMM lorsque vous déployez l’équilibreur de charge logicielle à l’aide de SCVMM.

>[!IMPORTANT]
>Sur l’hôte et les nœuds SLB, il est essentiel que le magasin de certificats Autorités de Certification racine de confiance n’inclut pas de n’importe quel certificat dans le cas de «pour» n’est pas identique à «Délivré par». Si cela se produit, la communication entre le contrôleur de réseau et de l’appareil southbound échoue.

Contrôleur de réseau et les certificats SLB MUX doivent être approuvés par les autres \ (certificat de racine du certificat SLB MUX doit être présent dans l’ordinateur du contrôleur de réseau des autorités de Certification racine approuvée, stocker et inversement versa\). Lorsque vous utilisez des certificats signés par intégrée, SCVMM permet de s’assurer que les certificats requis sont présents dans le dans les autorités de Certification racine de confiance stocker pour l’ordinateur Local.



