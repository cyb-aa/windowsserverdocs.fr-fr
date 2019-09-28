---
title: Gérer les certificats pour la mise en réseau définie par logiciel
description: Vous pouvez utiliser cette rubrique pour apprendre à gérer les certificats pour les communications Northbound et Southbound du contrôleur de réseau lorsque vous déployez la mise en réseau à définition logicielle (SDN) dans Windows Server 2016 Datacenter.
manager: dougkim
ms.prod: windows-server
ms.technology: networking-sdn
ms.topic: article
ms.assetid: c4e2f6c7-0364-4bf8-bb66-9af59c0bbd74
ms.author: pashort
author: shortpatti
ms.date: 08/22/2018
ms.openlocfilehash: b1cff080630c68ee8c4b7f0904f8fd0978330edc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405988"
---
# <a name="manage-certificates-for-software-defined-networking"></a>Gérer les certificats pour la mise en réseau définie par logiciel

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour apprendre à gérer les certificats pour les communications Northbound et Southbound du contrôleur de réseau lorsque vous déployez\) des SDN de mise en réseau \(à configuration logicielle dans Windows Server 2016 datacenter et que vous utilisez le système Center Virtual Machine Manager \(SCVMM\) en tant que client de gestion Sdn.

>[!NOTE]
>Pour obtenir des informations générales sur le contrôleur de réseau, consultez [contrôleur de réseau](../technologies/network-controller/Network-Controller.md).

Si vous n’utilisez pas Kerberos pour sécuriser la communication du contrôleur de réseau, vous pouvez utiliser des certificats X. 509 pour l’authentification, l’autorisation et le chiffrement.

SDN dans Windows Server 2016 Datacenter prend en charge\-les certificats X. \(509\)signés auto-signés et les autorités de certification. Cette rubrique fournit des instructions pas à pas pour créer ces certificats et les appliquer aux canaux de communication Northbound du contrôleur de réseau sécurisé avec les clients de gestion et les communications Southbound avec les périphériques réseau, tels que le logiciel Load balancer \(SLB\).
.
Lorsque vous utilisez l’authentification\-par certificat, vous devez inscrire un certificat sur les nœuds du contrôleur de réseau qui est utilisé des manières suivantes.

1. Chiffrement de la communication Northbound \(avec\) protocole SSL SSL entre les nœuds de contrôleur de réseau et les clients de gestion, tels que les System Center Virtual Machine Manager.
2. Authentification entre les nœuds de contrôleur de réseau et les appareils et services Southbound, tels que les hôtes Hyper- \(V\)et les équilibrages de charge logicielle SLBs.

## <a name="creating-and-enrolling-an-x509-certificate"></a>Création et inscription d’un certificat X. 509

Vous pouvez créer et inscrire un certificat auto\--signé ou un certificat émis par une autorité de certification.

>[!NOTE]
>Lorsque vous utilisez SCVMM pour déployer le contrôleur de réseau, vous devez spécifier le certificat X. 509 utilisé pour chiffrer les communications Northbound lors de la configuration du modèle de service de contrôleur de réseau.

La configuration du certificat doit inclure les valeurs suivantes.

- La valeur de la zone de texte **RestEndPoint** doit être l’adresse IP \(\) ou le nom de domaine complet du nom de domaine complet du contrôleur de réseau. 
- La valeur **RestEndPoint** doit correspondre au nom commun \(du nom d’objet\) , CN du certificat X. 509.

### <a name="creating-a-self-signed-x509-certificate"></a>Création d’un\-certificat X. 509 auto-signé

Vous pouvez créer un certificat X. 509 auto-signé et l’exporter avec la \(clé privée protégée par un mot de passe\) en procédant comme suit pour les\-déploiements à nœud unique\-et à plusieurs nœuds du contrôleur de réseau. .

Lorsque vous créez des\-certificats auto-signés, vous pouvez utiliser les instructions suivantes.

- Vous pouvez utiliser l’adresse IP du point de terminaison REST du contrôleur de réseau pour le paramètre dNSName, mais cela n’est pas recommandé, car cela nécessite que les nœuds du contrôleur de réseau \(soient tous situés dans un sous-réseau de gestion unique, par exemple, sur un seul rack.\)
- Pour les déploiements NC à plusieurs nœuds, le nom DNS que vous spécifiez devient le nom de domaine \(complet de l’hôte DNS du cluster de contrôleur de réseau. un enregistrement A est créé automatiquement.\) 
- Pour les déploiements de contrôleur de réseau à nœud unique, le nom DNS peut être le nom d’hôte du contrôleur de réseau, suivi du nom de domaine complet.

#### <a name="multiple-node"></a>Nœuds multiples

Vous pouvez utiliser la commande Windows PowerShell [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pkiclient/new-selfsignedcertificate) pour créer un certificat\-auto-signé.

**Syntaxe**

    New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "<YourNCComputerName>" -DnsName @("<NCRESTName>")

**Exemple d’utilisation**

    New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "MultiNodeNC" -DnsName @("NCCluster.Contoso.com")

#### <a name="single-node"></a>Nœud unique

Vous pouvez utiliser la commande Windows PowerShell [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pkiclient/new-selfsignedcertificate) pour créer un certificat\-auto-signé.

**Syntaxe**

    New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "<YourNCComputerName>" -DnsName @("<NCFQDN>")

**Exemple d’utilisation**

    New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "SingleNodeNC" -DnsName @("SingleNodeNC.Contoso.com")

### <a name="creating-a-ca-signed-x509-certificate"></a>Création d’un\-certificat X. 509 signé par une autorité de certification

Pour créer un certificat à l’aide d’une autorité de certification, vous devez déjà avoir déployé \(une\) infrastructure à clé publique \(PKI avec\)Active Directory Services de certificats AD CS. 

>[!NOTE]
>Vous pouvez utiliser des autorités de certification ou des outils tiers, tels que OpenSSL, pour créer un certificat à utiliser avec le contrôleur de réseau. Toutefois, les instructions de cette rubrique sont spécifiques aux services AD CS. Pour savoir comment utiliser une autorité de certification ou un outil tiers, consultez la documentation du logiciel que vous utilisez.

La création d’un certificat avec une autorité de certification comprend les étapes suivantes.

1. Vous ou l’administrateur de la sécurité ou du domaine de votre organisation configure le modèle de certificat
2. Vous ou l’Administrateur SCVMM ou l’administrateur du contrôleur de réseau de votre organisation demande un nouveau certificat auprès de l’autorité de certification.

#### <a name="certificate-configuration-requirements"></a>Configuration requise des certificats

Pendant que vous configurez un modèle de certificat à l’étape suivante, assurez-vous que le modèle que vous configurez comprend les éléments requis suivants.

1. Le nom d’objet du certificat doit être le nom de domaine complet de l’hôte Hyper-V
2. Le certificat doit être placé dans le magasin personnel de l’ordinateur local (My – CERT : \ LocalMachine\MY)
3. Le certificat doit avoir une authentification serveur (EKU : 1.3.6.1.5.5.7.3.1) et l’authentification du client (EKU : stratégies d’application 1.3.6.1.5.5.7.3.2).

>[!NOTE]
>Si le magasin \(de certificats My-CERT :\) \ LocalMachine\MY personnel sur l'\-hôte Hyper-V a plusieurs certificats X. 509 avec le nom d’objet (CN)\)comme nom \(de domaine complet de l’hôte, Assurez-vous que le certificat qui sera utilisé par SDN a une propriété d’utilisation améliorée de la clé personnalisée supplémentaire avec l’OID 1.3.6.1.4.1.311.95.1.1.1. Dans le cas contraire, la communication entre le contrôleur de réseau et l’hôte peut ne pas fonctionner.

#### <a name="to-configure-the-certificate-template"></a>Pour configurer le modèle de certificat
  
>[!NOTE]
>Avant d’effectuer cette procédure, vous devez passer en revue les certificats requis et les modèles de certificats disponibles dans la console modèles de certificats. Vous pouvez soit modifier un modèle existant, soit créer un doublon d’un modèle existant, puis modifier votre copie du modèle. Il est recommandé de créer une copie d’un modèle existant.

1. Sur le serveur où les services AD CS sont installés, dans Gestionnaire de serveur, cliquez sur **Outils**, puis sur **autorité de certification**. La console \(MMC\) de l’autorité de certification s’ouvre. 
2. Dans la console MMC, double-cliquez sur le nom de l’autorité de certification, cliquez avec le bouton droit sur **modèles de certificats**, puis cliquez sur **gérer**.
3. La console modèles de certificats s’ouvre. Tous les modèles de certificat s’affichent dans le volet d’informations.
4. Dans le volet d’informations, cliquez sur le modèle que vous souhaitez dupliquer.
5.  Cliquez sur le menu **action** , puis sur **dupliquer le modèle**. La boîte de dialogue **Propriétés** du modèle s’ouvre.
6.  Dans la boîte de dialogue **Propriétés** du modèle, sous l’onglet **nom du sujet** , cliquez sur **fournir dans la demande**. \(Ce paramètre est requis pour les certificats SSL du contrôleur de réseau.\)
7.  Dans la boîte de dialogue **Propriétés** du modèle, sous l’onglet **traitement** de la demande, assurez-vous que l’option **autoriser l’exportation de la clé privée** est sélectionnée. Assurez-vous également que la signature et l’objectif de **chiffrement** sont sélectionnés.
8.  Dans la boîte de dialogue **Propriétés** du modèle, sous l’onglet **Extensions** , sélectionnez utilisation de la **clé**, puis cliquez sur **modifier**.
9.  Dans **signature**, assurez-vous que la **signature numérique** est sélectionnée.
10.  Dans la boîte de dialogue **Propriétés** du modèle, sous l’onglet **Extensions** , sélectionnez **stratégies d’application**, puis cliquez sur **modifier**.
11.  Dans **stratégies d’application**, assurez-vous que **authentification du client** et **authentification du serveur** sont répertoriées.
12.  Enregistrez la copie du modèle de certificat avec un nom unique, tel que le **modèle de contrôleur de réseau**.

#### <a name="to-request-a-certificate-from-the-ca"></a>Pour demander un certificat auprès de l’autorité de certification

Vous pouvez utiliser le composant logiciel enfichable Certificats pour demander des certificats. Vous pouvez demander tout type de certificat qui a été préconfiguré et mis à disposition par un administrateur de l’autorité de certification qui traite la demande de certificat.

Les **utilisateurs** ou les **administrateurs** locaux sont les groupes minimum requis pour effectuer cette procédure.

1. Ouvrez le composant logiciel enfichable Certificats pour un ordinateur.
2. Dans l’arborescence de la console, cliquez sur  **\(certificats ordinateur\)local**. Sélectionnez le magasin de certificats **personnel** .
3. Dans le menu **action** , pointez sur * * toutes les tâches<strong>, puis cliquez sur * * demander un nouveau certificat</strong> pour démarrer l’Assistant inscription de certificats. Cliquez sur **Suivant**.
4. Sélectionnez le **configuré par votre stratégie d'** inscription de certificat administrateur, puis cliquez sur **suivant**.
5. Sélectionnez la **stratégie** \(d’inscription Active Directory basée sur le modèle d’autorité de certification que vous avez configuré dans\)la section précédente.
6. Développez la section **Détails** et configurez les éléments suivants.
   1. Assurez-vous que **l’utilisation** de la clé comprend à la fois la <strong>signature numérique * * et * * le chiffrement de la clé</strong>.
   2. Assurez-vous que les **stratégies d’application** incluent à la fois\) **l’authentification** \(du serveur 1.3.6.1.5.5.7.3.1\) et **l’authentification** \(du client 1.3.6.1.5.5.7.3.2.
7. Cliquez sur **Propriétés**.
8. Sous l’onglet **objet** , dans **nom du sujet**, dans **type**, sélectionnez **nom commun**. Dans valeur, spécifiez le **point de terminaison REST du contrôleur de réseau**.
9. Cliquez sur **Appliquer**, puis sur **OK**.
10. Cliquez sur **S’inscrire**.

Dans la console MMC certificats, cliquez sur le magasin personnel pour afficher le certificat que vous avez inscrit auprès de l’autorité de certification.

## <a name="exporting-and-copying-the-certificate-to-the-scvmm-library"></a>Exportation et copie du certificat dans la bibliothèque SCVMM

Après la création d’un\-certificat signé auto\-ou d’une autorité de certification, vous devez exporter le certificat \(avec la clé privée\) au format. pfx et \(sans la clé privée dans le formatdebase-64.cer\) à partir du composant logiciel enfichable Certificats. 

Vous devez ensuite copier les deux fichiers exportés dans les dossiers **serverCertificate.CR** et **NCCertificate.CR** que vous avez spécifiés au moment où vous avez importé le modèle de service NC.

1. Ouvrez le composant logiciel enfichable Certificats (certlm. msc) et recherchez le certificat dans le magasin de certificats personnel de l’ordinateur local.
2. Cliquez\-avec le bouton droit sur le certificat, cliquez sur **toutes les tâches**, puis sur **Exporter**. L'Assistant Exportation de certificat s'ouvre. Cliquez sur **Suivant**.
3. Sélectionnez l’option **Oui**, exporter la clé privée, puis cliquez sur **suivant**.
4. Sélectionnez **échange d’informations personnelles-PKCS #12 (. PFX)** et acceptez la valeur par défaut pour **inclure tous les certificats dans le chemin d’accès de certification** , si possible.
5. Affectez les utilisateurs/groupes et un mot de passe pour le certificat que vous exportez, puis cliquez sur **suivant**.
6. Sur la page fichier à exporter, recherchez l’emplacement où vous souhaitez placer le fichier exporté et donnez-lui un nom.
7. De même, exportez le certificat dans. Format CER. Remarque : À exporter vers. CER format, décochez l’option Oui, exporter la clé privée.
8. Copiez. PFX dans le dossier ServerCertificate.cr.
9. Copiez. Fichier CER dans le dossier NCCertificate.cr

Lorsque vous avez terminé, actualisez ces dossiers dans la bibliothèque SCVMM et assurez-vous que vous avez copié ces certificats. Passez à la configuration et au déploiement du modèle de service de contrôleur de réseau.

## <a name="authenticating-southbound-devices-and-services"></a>Authentification des appareils et services Southbound 

La communication entre le contrôleur de réseau et les hôtes et les périphériques SLB MUX utilise des certificats pour l’authentification. La communication avec les hôtes se fait via le protocole OVSDB, tandis que la communication avec les périphériques SLB MUX se fait via le protocole WCF.

### <a name="hyper-v-host-communication-with-network-controller"></a>Communication de l’hôte Hyper-V avec le contrôleur de réseau

Pour la communication avec les hôtes Hyper-V sur OVSDB, le contrôleur de réseau doit présenter un certificat aux ordinateurs hôtes. Par défaut, SCVMM récupère le certificat SSL configuré sur le contrôleur de réseau et l’utilise pour la communication Southbound avec les hôtes.

C’est la raison pour laquelle le certificat SSL doit avoir la EKU d’authentification du client configurée. Ce certificat est configuré sur la ressource \(Rest « serveurs ». les ordinateurs hôtes Hyper-V sont représentés dans le contrôleur de réseau en tant que ressource\)de serveur et peuvent être affichés en exécutant la commande Windows PowerShell **NetworkControllerServer** .

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

Pour l’authentification mutuelle, l’hôte Hyper-V doit également disposer d’un certificat pour communiquer avec le contrôleur de réseau. 

Vous pouvez inscrire le certificat auprès d’une autorité \(de\)certification. Si un certificat basé sur une autorité de certification est introuvable sur l’ordinateur hôte, SCVMM crée un certificat auto-signé et le met en service sur l’ordinateur hôte.

Le contrôleur de réseau et les certificats hôtes Hyper-V doivent être approuvés l’un par l’autre. Le certificat racine du certificat hôte Hyper-V doit être présent dans le magasin d’autorités de certification racines de confiance du contrôleur de réseau pour l’ordinateur local, et vice versa. 

Lorsque vous utilisez des certificats\-auto-signés, SCVMM garantit que les certificats requis sont présents dans le magasin d’autorités de certification racines de confiance pour l’ordinateur local. 

Si vous utilisez des certificats d’autorité de certification pour les hôtes Hyper-V, vous devez vous assurer que le certificat racine de l’autorité de certification est présent dans le magasin des autorités de certification racines de confiance du contrôleur de réseau pour l’ordinateur local.

### <a name="software-load-balancer-mux-communication-with-network-controller"></a>Communication Load Balancer MUX du logiciel avec le contrôleur de réseau

Le logiciel load balancer multiplexeur \(MUX\) et le contrôleur de réseau communiquent via le protocole WCF, à l’aide de certificats pour l’authentification.

Par défaut, SCVMM récupère le certificat SSL configuré sur le contrôleur de réseau et l’utilise pour la communication Southbound avec les périphériques Mux. Ce certificat est configuré sur la ressource REST « NetworkControllerLoadBalancerMux » et peut être affiché en exécutant l’applet de commande PowerShell **-NetworkControllerLoadBalancerMux**.

Exemple de ressource \(Rest MUX partielle\):

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

Pour l’authentification mutuelle, vous devez également disposer d’un certificat sur les périphériques SLB MUX. Ce certificat est configuré automatiquement par SCVMM quand vous déployez l’équilibreur de charge logiciel à l’aide de SCVMM.

>[!IMPORTANT]
>Sur les nœuds hôte et SLB, il est essentiel que le magasin de certificats des autorités de certification racines de confiance n’inclue pas de certificat où « délivré à » n’est pas le même que « délivré par ». Si cela se produit, la communication entre le contrôleur de réseau et l’appareil Southbound échoue.

Les certificats de contrôleur de réseau et de multiplexeur SLB doivent être \(approuvés par les autres certificats la racine du certificat de multiplexeur SLB doit être présent dans le magasin d’autorités de certification racines\)de confiance de l’ordinateur du contrôleur de réseau, et vice versa. Lorsque vous utilisez des certificats\-auto-signés, SCVMM garantit que les certificats requis sont présents dans le dans le magasin autorités de certification racines de confiance pour l’ordinateur local.



