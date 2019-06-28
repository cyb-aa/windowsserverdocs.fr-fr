---
title: Installer des certificats de racine de confiance TPM
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 06/27/2019
ms.openlocfilehash: 0d42befcfacfffd302cfcb27f9f3c2c973534398
ms.sourcegitcommit: 2c2c37170c65434179bcf2989d557f97dcbe1b9f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/27/2019
ms.locfileid: "67419229"
---
# <a name="install-trusted-tpm-root-certificates"></a>Installer des certificats de racine de confiance TPM

>S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016

Lorsque vous configurez SGH pour utiliser l’attestation TPM, vous devez également configurer SGH pour approuver les fournisseurs de modules de plateforme sécurisée sur vos serveurs.
Ce processus de vérification supplémentaire ne garantit que les modules TPM authentique et digne de confiance est en mesure d’attester avec votre SGH.
Si vous essayez d’inscrire un module de plateforme sécurisée non approuvé avec `Add-HgsAttestationTpmHost`, vous recevrez une erreur indiquant que le fournisseur de module de plateforme sécurisée n’est pas approuvé.

Pour approuver votre module de plateforme sécurisée, les certificats racines et intermédiaires signatures utilisés pour signer la paire de clés dans le module de plateforme sécurisée vos serveurs doivent être installés sur SGH.
Si vous utilisez plusieurs modèles de module de plateforme sécurisée dans votre centre de données, vous devrez peut-être installer des certificats différents pour chaque modèle.
SGH recherchera dans le « TrustedTPM_RootCA » et les magasins de certificats de « TrustedTPM_IntermediateCA » pour les certificats de fournisseur.

> [!NOTE]
> Les certificats de fournisseur de module de plateforme sécurisée sont différents de ceux installés par défaut dans Windows et représentent la spécifique certificats racines et intermédiaires utilisés par les fournisseurs du module de plateforme sécurisée.

Une collection de certificats approuvés TPM racines et intermédiaires est publiée par Microsoft pour votre commodité.
Vous pouvez utiliser les étapes ci-dessous pour installer ces certificats.
Si vos certificats de module de plateforme sécurisée ne sont pas inclus dans le package ci-dessous, contactez votre fournisseur de module de plateforme sécurisée ou le serveur OEM pour obtenir les certificats racines et intermédiaires pour votre modèle de module de plateforme sécurisée.

Répétez les étapes suivantes sur **chaque serveur SGH**:

1.  Téléchargez le dernier package à partir de [ https://go.microsoft.com/fwlink/?linkid=2097925 ](https://go.microsoft.com/fwlink/?linkid=2097925).

2.  Vérifier la signature du fichier cab pour garantir son authenticité. Ne continuez pas si la signature n’est pas valide.

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

2.  Développez le fichier cab.

    ```
    mkdir .\TrustedTPM
    expand.exe -F:* <Path-To-TrustedTpm.cab> .\TrustedTPM
    ```

3.  Par défaut, le script de configuration va installer des certificats pour chaque fournisseur de module de plateforme sécurisée. Si vous souhaitez importer des certificats pour votre fournisseur de module de plateforme sécurisée spécifique uniquement, supprimez les dossiers pour les fournisseurs TPM ne pas approuvés par votre organisation.

4.  Installez le package de certificat approuvé en exécutant le script d’installation dans le dossier développé.

    ```
    cd .\TrustedTPM
    .\setup.cmd
    ```

Pour ajouter de nouveaux certificats ou celles a été ignorée intentionnellement pendant une installation antérieure, répétez simplement les étapes ci-dessus sur tous les nœuds dans votre cluster SGH.
Les certificats existants resteront approuvés mais trouvées dans le fichier cab développé de nouveaux certificats seront ajoutés au TPM approuvé stocke.

## <a name="next-step"></a>Étape suivante

> [!div class="nextstepaction"]
> [Configurer une structure DNS](guarded-fabric-configuring-fabric-dns-tpm.md)



