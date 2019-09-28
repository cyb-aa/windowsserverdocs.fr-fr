---
title: Installer des certificats racines TPM approuvés
ms.custom: na
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 06/27/2019
ms.openlocfilehash: 15614ce1065170bc557fad10a168b3dda6a5b05a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386549"
---
# <a name="install-trusted-tpm-root-certificates"></a>Installer des certificats racines TPM approuvés

>S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016

Quand vous configurez SGH pour utiliser l’attestation TPM, vous devez également configurer SGH pour faire confiance aux fournisseurs des plateforme sécurisée de vos serveurs.
Ce processus de vérification supplémentaire garantit que seuls les plateforme sécurisée authentiques et dignes de confiance sont en mesure d’attester avec votre SGH.
Si vous essayez d’inscrire un module de plateforme sécurisée non approuvé avec `Add-HgsAttestationTpmHost`, vous recevrez une erreur indiquant que le fournisseur du module de plateforme sécurisée n’est pas approuvé.

Pour approuver votre plateforme sécurisée, les certificats de signature intermédiaire et racine utilisés pour signer la clé de type EK dans les plateforme sécurisée de vos serveurs doivent être installés sur le serveur SGH.
Si vous utilisez plusieurs modèles de module de plateforme sécurisée dans votre centre de informations, vous devrez peut-être installer différents certificats pour chaque modèle.
SGH cherchera dans les magasins de certificats « TrustedTPM_RootCA » et « TrustedTPM_IntermediateCA » pour les certificats du fournisseur.

> [!NOTE]
> Les certificats du fournisseur du module de plateforme sécurisée sont différents de ceux installés par défaut dans Windows et représentent la racine et les certificats intermédiaires spécifiques utilisés par les fournisseurs de module de plateforme sécurisée.

Une collection de certificats racines et intermédiaires TPM approuvés est publiée par Microsoft à des fins de commodité.
Vous pouvez utiliser les étapes ci-dessous pour installer ces certificats.
Si vos certificats TPM ne sont pas inclus dans le package ci-dessous, contactez votre fournisseur de module de plateforme sécurisée ou votre serveur OEM pour obtenir les certificats racines et intermédiaires pour votre modèle TPM spécifique.

Répétez les étapes suivantes sur **chaque serveur SGH**:

1.  Téléchargez le package le plus récent à partir de [https://go.microsoft.com/fwlink/?linkid=2097925](https://go.microsoft.com/fwlink/?linkid=2097925).

2.  Vérifiez la signature du fichier CAB pour garantir son authenticité. Ne pas continuer si la signature n’est pas valide.

    ```powershell
    Get-AuthenticodeSignature .\TrustedTpm.cab
    ```
    
    Voici quelques exemples de sortie :
    
    ```
    Directory: C:\Users\Administrator\Downloads
        
    SignerCertificate                         Status                                 Path
    -----------------                         ------                                 ----
    0DD6D4D4F46C0C7C2671962C4D361D607E370940  Valid                                  TrustedTpm.cab
    ```

2.  Développez le fichier CAB.

    ```
    mkdir .\TrustedTPM
    expand.exe -F:* <Path-To-TrustedTpm.cab> .\TrustedTPM
    ```

3.  Par défaut, le script de configuration installe les certificats pour chaque fournisseur de module de plateforme sécurisée. Si vous souhaitez uniquement importer des certificats pour votre fournisseur de module de plateforme sécurisée spécifique, supprimez les dossiers des fournisseurs de module de plateforme sécurisée non approuvés par votre organisation.

4.  Installez le package de certificat approuvé en exécutant le script d’installation dans le dossier développé.

    ```
    cd .\TrustedTPM
    .\setup.cmd
    ```

Pour ajouter de nouveaux certificats ou ceux qui sont ignorés intentionnellement au cours d’une installation antérieure, répétez simplement les étapes ci-dessus sur chaque nœud de votre cluster SGH.
Les certificats existants resteront approuvés, mais les nouveaux certificats se trouvant dans le fichier CAB développé seront ajoutés aux magasins TPM approuvés.

## <a name="next-step"></a>Étape suivante

> [!div class="nextstepaction"]
> [Configurer une structure DNS](guarded-fabric-configuring-fabric-dns-tpm.md)



