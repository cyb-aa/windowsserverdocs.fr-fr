---
title: Gérer des certificats pour Sdn
description: Vous pouvez utiliser cette rubrique pour savoir comment gérer des certificats pour Northbound du contrôleur de réseau et communications Southbound lorsque vous déployez la mise en réseau SDN (Software Defined) dans Windows Server 2016 Datacenter.
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: c4e2f6c7-0364-4bf8-bb66-9af59c0bbd74
ms.author: pashort
author: shortpatti
ms.date: 08/22/2018
ms.openlocfilehash: 618c2c4da60decc94f84c2a40cd4d2aa80d5f26b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827570"
---
# <a name="manage-certificates-for-software-defined-networking"></a>Gérer des certificats pour Sdn

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour apprendre à gérer les certificats pour les communications Northbound du contrôleur de réseau et Southbound lorsque vous déployez Sdn \(SDN\) dans Windows Server 2016 Datacenter et que vous utilisez système Center Virtual Machine Manager \(SCVMM\) comme votre client de gestion SDN.

>[!NOTE]
>Pour plus d’informations sur le contrôleur de réseau, consultez [contrôleur de réseau](../technologies/network-controller/Network-Controller.md).

Si vous n’utilisez pas Kerberos pour sécuriser la communication du contrôleur de réseau, vous pouvez utiliser des certificats X.509 pour l’authentification, autorisation et chiffrement.

SDN dans Windows Server 2016 Datacenter prend en charge les deux self\-signé et l’autorité de Certification \(autorité de certification\)-signé de certificats X.509. Cette rubrique fournit des instructions pas à pas pour la création de ces certificats et de les appliquer pour sécuriser les canaux de communication Northbound du contrôleur réseau avec les clients de gestion et communications Southbound avec les périphériques réseau, tels que le logiciel L’équilibreur de charge \(SLB\).
.
Lorsque vous utilisez un certificat\-basé l’authentification, vous devez inscrire un certificat sur les nœuds de contrôleur de réseau qui est utilisé de plusieurs manières.

1. Chiffrement de la Communication Northbound avec le protocole SSL (Secure Sockets Layer) \(SSL\) entre les nœuds de contrôleur de réseau et les clients de gestion, tels que System Center Virtual Machine Manager.
2. L’authentification entre les nœuds du contrôleur de réseau et des services tels que les hôtes Hyper-V et les équilibreurs de charge logiciels et appareils Southbound \(SLBs\).

## <a name="creating-and-enrolling-an-x509-certificate"></a>Création et l’inscription d’un certificat X.509

Vous pouvez créer et inscrire un self\-signé le certificat ou un certificat émis par une autorité de certification.

>[!NOTE]
>Lorsque vous utilisez SCVMM pour déployer le contrôleur de réseau, vous devez spécifier le certificat X.509 qui est utilisé pour chiffrer les communications Northbound lors de la configuration du modèle de Service de contrôleur de réseau.

La configuration de certificat doit inclure les valeurs suivantes.

- La valeur de la **RestEndPoint** zone de texte doit être soit le réseau contrôleur de nom de domaine complet \(FQDN\) ou l’adresse IP. 
- Le **RestEndPoint** valeur doit correspondre au nom de sujet \(nom commun, CN\) du certificat X.509.

### <a name="creating-a-self-signed-x509-certificate"></a>Création d’un libre-service\-certificat X.509 signé

Vous pouvez créer un certificat X.509 auto-signé et exportez-le avec la clé privée \(protégé avec un mot de passe\) en suivant ces étapes pour seule\-nœud et plusieurs\-des déploiements de nœuds de contrôleur de réseau .

Lorsque vous créez self\-signé des certificats, vous pouvez utiliser les instructions suivantes.

- Vous pouvez utiliser l’adresse IP du point de terminaison REST de contrôleur de réseau pour le paramètre DnsName - mais cela n’est pas recommandée car elle requiert que les nœuds de contrôleur de réseau se trouvent toutes dans un sous-réseau de gestion unique \(par exemple, sur un seul rack\)
- Pour les déploiements de plusieurs nœud NC, le nom DNS que vous spécifiez devient le nom de domaine complet du Cluster de contrôleur de réseau \(hôte DNS un enregistrements sont créés automatiquement.\) 
- Pour les déploiements à nœud unique contrôleur de réseau, le nom DNS peut être nom d’hôte du contrôleur de réseau suivi du nom de domaine complet.

#### <a name="multiple-node"></a>Plusieurs nœuds

Vous pouvez utiliser la [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pkiclient/new-selfsignedcertificate) commande de Windows PowerShell pour créer un libre-service\-certificat signé.

**Syntaxe**

    New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "<YourNCComputerName>" -DnsName @("<NCRESTName>")

**Exemple d’utilisation**

    New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "MultiNodeNC" -DnsName @("NCCluster.Contoso.com")

#### <a name="single-node"></a>Nœud unique

Vous pouvez utiliser la [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pkiclient/new-selfsignedcertificate) commande de Windows PowerShell pour créer un libre-service\-certificat signé.

**Syntaxe**

    New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "<YourNCComputerName>" -DnsName @("<NCFQDN>")

**Exemple d’utilisation**

    New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "SingleNodeNC" -DnsName @("SingleNodeNC.Contoso.com")

### <a name="creating-a-ca-signed-x509-certificate"></a>Création d’une autorité de certification\-certificat X.509 signé

Pour créer un certificat à l’aide d’une autorité de certification, vous devez déjà avoir déployé une Infrastructure à clé publique \(PKI\) avec Active Directory Certificate Services \(AD CS\). 

>[!NOTE]
>Vous pouvez utiliser des autorités de certification de tierce partie ou des outils, tels qu’openssl, pour créer un certificat à utiliser avec le contrôleur de réseau, mais les instructions fournies dans cette rubrique sont spécifiques aux services AD CS. Pour apprendre à utiliser une autorité de certification tierce ou l’outil, consultez la documentation pour le logiciel que vous utilisez.

Création d’un certificat avec une autorité de certification inclut les étapes suivantes.

1. De vous ou votre organisation domaine ou administrateur de sécurité configure le modèle de certificat
2. Vous ou votre administrateur de contrôleur de réseau ou organisation Administrateur SCVMM demande un nouveau certificat à partir de l’autorité de certification.

#### <a name="certificate-configuration-requirements"></a>Exigences de configuration de certificat

Lorsque vous configurez un modèle de certificat à l’étape suivante, vérifiez que le modèle que vous configurez inclut les éléments requis suivants.

1. Le nom de sujet du certificat doit être le nom de domaine complet de l’hôte Hyper-V
2. Le certificat doit être placé dans le magasin personnel local machine (mon – cert : \localmachine\my)
3. Le certificat doit avoir les deux l’authentification du serveur (EKU : 1.3.6.1.5.5.7.3.1) et l’authentification du Client (EKU : 1.3.6.1.5.5.7.3.2) stratégies d’application.

>[!NOTE]
>Si le profil personnel et \(mon – cert : \localmachine\my\) magasin de certificats sur l’Hyper\-hôte V a plusieurs certificats X.509 avec le nom de sujet (CN) en tant que le nom de domaine complet de l’hôte \(FQDN\), Assurez-vous que le certificat qui sera utilisé par SDN a une propriété d’utilisation améliorée de la clé personnalisée supplémentaire avec l’OID 1.3.6.1.4.1.311.95.1.1.1. Sinon, la communication entre le contrôleur de réseau et l’hôte ne peut ne pas fonctionne.

#### <a name="to-configure-the-certificate-template"></a>Pour configurer le modèle de certificat
  
>[!NOTE]
>Avant d’effectuer cette procédure, vous devez examiner la configuration requise des certificats et les modèles de certificats disponibles dans la console Modèles de certificats. Vous pouvez modifier un modèle existant ou créer un doublon d’un modèle existant et ensuite modifier votre copie du modèle. Création d’une copie d’un modèle existant est recommandée.

1. Sur le serveur où les services AD CS est installé, dans le Gestionnaire de serveur, cliquez sur **outils**, puis cliquez sur **autorité de Certification**. La Console Autorité de Certification Microsoft \(MMC\) s’ouvre. 
2. Dans la console MMC, double-cliquez sur le nom de l’autorité de certification, cliquez sur **modèles de certificats**, puis cliquez sur **gérer**.
3. La console Modèles de certificats s’ouvre. Tous les modèles de certificats sont affichés dans le volet détails.
4. Dans le volet de détails, cliquez sur le modèle que vous souhaitez dupliquer.
5.  Cliquez sur le **Action** menu, puis sur **dupliquer le modèle**. Le modèle **propriétés** boîte de dialogue s’ouvre.
6.  Dans le modèle **propriétés** boîte de dialogue le **nom de l’objet** , cliquez sur **fourni dans la demande**. \(Ce paramètre est obligatoire pour les certificats SSL de contrôleur de réseau.\)
7.  Dans le modèle **propriétés** boîte de dialogue le **traitement de la demande** , vérifiez que **autoriser à exporter la clé privée** est sélectionné. Vérifiez également que le **Signature et chiffrement** objectif est sélectionné.
8.  Dans le modèle **propriétés** boîte de dialogue le **Extensions** onglet, sélectionnez **utilisation de la clé**, puis cliquez sur **modifier**.
9.  Dans **Signature**, vérifiez que **Signature numérique** est sélectionné.
10.  Dans le modèle **propriétés** boîte de dialogue le **Extensions** onglet, sélectionnez **stratégies d’Application**, puis cliquez sur **modifier**.
11.  Dans **stratégies d’Application**, vérifiez que **l’authentification du Client** et **l’authentification du serveur** sont répertoriés.
12.  Enregistrer la copie du modèle de certificat avec un nom unique, par exemple **modèle de contrôleur de réseau**.

#### <a name="to-request-a-certificate-from-the-ca"></a>Pour demander un certificat à partir de l’autorité de certification

Vous pouvez utiliser le composant logiciel enfichable Certificats pour demander des certificats. Vous pouvez demander tout type de certificat qui a été préconfigurée et mis à disposition par un administrateur de l’autorité de certification qui traite la demande de certificat.

**Les utilisateurs** ou local **administrateurs** est l’appartenance de groupe minimale requise pour effectuer cette procédure.

1. Ouvrez le composant logiciel enfichable Certificats pour un ordinateur.
2. Dans l’arborescence de la console, cliquez sur **certificats \(ordinateur Local\)**. Sélectionnez le **personnel** magasin de certificats.
3. Sur le **Action** menu, pointez sur ** toutes les tâches **, puis cliquez sur **demander un nouveau certificat** pour démarrer l’Assistant Inscription de certificats. Cliquez sur **Suivant**.
4. Sélectionnez le **configuré par votre administrateur** stratégie d’inscription de certificat et cliquez sur **suivant**.
5. Sélectionnez le **stratégie de l’inscription Active Directory** \(basé sur le modèle de l’autorité de certification que vous avez configuré dans la section précédente\).
6. Développez le **détails** section et configurer les éléments suivants.
    1. Vérifiez que **utilisation de la clé** comprend à la fois ** Signature numérique ** et **chiffrage de clés**.
    2. Vérifiez que **stratégies d’Application** comprend à la fois **l’authentification du serveur** \(1.3.6.1.5.5.7.3.1\) et **l’authentification du Client** \(1.3.6.1.5.5.7.3.2\).
7. Cliquez sur **Propriétés**.
8. Sur le **sujet** sous l’onglet **nom de l’objet**, dans **Type**, sélectionnez **nom commun**. Dans valeur, spécifiez **point de terminaison REST de contrôleur de réseau**.
9. Cliquez sur **Appliquer**, puis sur **OK**.
10. Cliquez sur **S’inscrire**.

Dans la console MMC Certificats, cliquez sur le magasin personnel pour afficher le certificat que vous avez inscrits à partir de l’autorité de certification.

## <a name="exporting-and-copying-the-certificate-to-the-scvmm-library"></a>Exportation et copier le certificat sur la bibliothèque SCVMM

Après avoir créé un self\-signé ou autorité de certification\-signé le certificat, vous devez exporter le certificat avec la clé privée \(au format .pfx\) et sans la clé privée \(Base 64 au format .cer\) à partir du composant logiciel enfichable Certificats. 

Vous devez ensuite copier les deux fichiers exportés à le **ServerCertificate.cr** et **NCCertificate.cr** dossiers que vous avez spécifié au moment lorsque vous avez importé le modèle de Service de contrôleur de réseau.

1. Ouvrez le composant logiciel enfichable Certificats (certlm.msc) et recherchez le certificat dans le magasin de certificats personnel de l’ordinateur local.
2. Droite\-cliquez sur le certificat, cliquez sur **toutes les tâches**, puis cliquez sur **exporter**. L'Assistant Exportation de certificat s'ouvre. Cliquez sur **Suivant**.
3. Sélectionnez **Oui**, exporter l’option de clé privée, cliquez sur **suivant**.
4. Choisissez **échange d’informations personnelles - PKCS #12 (. PFX)** et acceptez la valeur par défaut à **inclure tous les certificats dans le chemin d’accès de certification** si possible.
5. Attribuer les utilisateurs/groupes et un mot de passe pour le certificat que vous exportez, cliquez sur **suivant**.
6. Sur la page fichier à exporter, accédez à l’emplacement où vous souhaitez placer le fichier exporté et donnez-lui un nom.
7. De même, exportez le certificat dans. Format CER. Remarque: Pour exporter vers. Format CER, décochez la case Oui, exporter l’option de clé privée.
8. Copie le. PFX dans le dossier ServerCertificate.cr.
9. Copie le. Fichier CER vers le dossier NCCertificate.cr.

Lorsque vous avez terminé, actualisez ces dossiers dans la bibliothèque SCVMM et assurez-vous d’avoir ces certificats copiés. Poursuivre la Configuration de modèle de Service de contrôleur réseau et le déploiement.

## <a name="authenticating-southbound-devices-and-services"></a>L’authentification des services et appareils Southbound 

Communication de contrôleur de réseau avec les hôtes et les périphériques de SLB MUX utilise des certificats pour l’authentification. Communication avec les ordinateurs hôtes est via le protocole OVSDB tandis que la communication avec les appareils SLB MUX est sur le protocole WCF.

### <a name="hyper-v-host-communication-with-network-controller"></a>Communication d’hôte Hyper-V avec le contrôleur de réseau

Pour la communication avec les hôtes Hyper-V sur OVSDB, contrôleur de réseau doit présenter un certificat sur les ordinateurs hôtes. Par défaut, SCVMM récupère le certificat SSL configuré sur le contrôleur de réseau et l’utilise pour la communication southbound avec les ordinateurs hôtes.

C’est pourquoi le certificat SSL doit avoir l’EKU d’authentification Client configuré. Ce certificat est configuré sur « Serveurs » REST ressources \(hôtes Hyper-V sont représentées dans le contrôleur de réseau en tant que serveur ressource\)et peut être affichée en exécutant la commande Windows PowerShell  **Get-NetworkControllerServer**.

Voici un exemple partiel de la ressource REST du serveur.

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

Pour l’authentification mutuelle, l’hôte Hyper-V doit également posséder un certificat pour communiquer avec le contrôleur de réseau. 

Vous pouvez inscrire le certificat à partir d’une autorité de Certification \(autorité de certification\). Si un certificat d’autorité de certification est introuvable sur l’ordinateur hôte, SCVMM crée un certificat auto-signé et met en service sur l’ordinateur hôte.

Contrôleur de réseau et les certificats d’ordinateur hôte Hyper-V doivent être approuvés par eux. Certificat de racine du certificat d’ordinateur hôte Hyper-V doit être présent dans le réseau contrôleur Trusted Root Certification Authorities stocker pour l’ordinateur Local et vice versa. 

Lorsque vous utilisez self\-signé des certificats, SCVMM garantit que les certificats requis sont présents dans le magasin d’autorités de Certification racine de confiance pour l’ordinateur Local. 

Si vous utilisez des certificats d’autorité de certification pour les hôtes Hyper-V, vous devez vous assurer que le certificat racine est présent sur le magasin d’autorités de Certification racine de confiance du contrôleur de réseau pour l’ordinateur Local.

### <a name="software-load-balancer-mux-communication-with-network-controller"></a>Logiciel charge équilibrage MUX Communication avec le contrôleur de réseau

MULTIPLEXEUR équilibreur de charge logiciel \(MUX\) et contrôleur de réseau communiquent via le protocole WCF, à l’aide de certificats pour l’authentification.

Par défaut, SCVMM récupère le certificat SSL configuré sur le contrôleur de réseau et l’utilise pour la communication southbound avec les appareils Mux. Ce certificat est configuré sur le « NetworkControllerLoadBalancerMux » REST ressources et peuvent être affichés en exécutant l’applet de commande Powershell **Get-NetworkControllerLoadBalancerMux**.

Exemple de ressource MUX REST \(partielle\):

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

Pour l’authentification mutuelle, vous devez également posséder un certificat sur les appareils SLB MUX. Ce certificat est automatiquement configuré par SCVMM quand vous déployez l’équilibreur de charge logiciel à l’aide de SCVMM.

>[!IMPORTANT]
>Sur l’hôte et les nœuds SLB, il est essentiel que le magasin de certificats Autorités de Certification racine de confiance n’inclut pas de n’importe quel certificat où « Délivré à » n’est pas identique à « Délivré par ». Si cela se produit, la communication entre le contrôleur de réseau et de l’appareil southbound échoue.

Contrôleur de réseau et les certificats de SLB MUX doivent être approuvés par eux \(certificat de racine du certificat SLB MUX doit être présent dans l’ordinateur contrôleur de réseau de stocker des autorités de Certification racine et vice versa\). Lorsque vous utilisez self\-signé des certificats, SCVMM garantit que les certificats requis sont présents dans le dans Trusted Root Certification Authorities stocker pour l’ordinateur Local.



