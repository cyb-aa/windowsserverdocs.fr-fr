---
title: Obtenir des certificats pour SGH
ms.prod: windows-server
ms.topic: article
ms.assetid: f4b4d1a8-bf6d-4881-9150-ddeca8b48038
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 09/25/2019
ms.openlocfilehash: da1ae4bacd5a6b2e38b22930aacf06f65b16bb29
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856532"
---
# <a name="obtain-certificates-for-hgs"></a>Obtenir des certificats pour SGH

>S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016

Lorsque vous déployez SGH, vous êtes invité à fournir des certificats de signature et de chiffrement qui sont utilisés pour protéger les informations sensibles nécessaires au démarrage d’une machine virtuelle protégée.
Ces certificats ne laissent jamais SGH et sont utilisés uniquement pour déchiffrer les clés de machine virtuelle protégées lorsque l’ordinateur hôte sur lequel ils s’exécutent a prouvé qu’il est sain.
Les locataires (propriétaires de machines virtuelles) utilisent la moitié publique des certificats pour autoriser votre centre de contenus à exécuter leurs machines virtuelles protégées.
Cette section décrit les étapes requises pour obtenir des certificats de signature et de chiffrement compatibles pour SGH.

## <a name="request-certificates-from-your-certificate-authority"></a>Demander des certificats auprès de votre autorité de certification

Bien que cela ne soit pas obligatoire, il est fortement recommandé d’obtenir vos certificats auprès d’une autorité de certification approuvée.
Cela aide les propriétaires de machines virtuelles à vérifier qu’ils autorisent le serveur SGH approprié (par exemple, le fournisseur de services ou le centre de donné) à exécuter leurs machines virtuelles protégées.
Dans un scénario d’entreprise, vous pouvez choisir d’utiliser votre propre autorité de certification d’entreprise pour émettre ces certificats.
Les hébergeurs et les fournisseurs de services doivent envisager d’utiliser une autorité de certification publique connue à la place.

Les certificats de signature et de chiffrement doivent être émis avec les propriétés Certificiate suivantes (sauf si elles sont marquées « recommandé ») :

Propriété du modèle de certificat | Valeur requise 
------------------------------|----------------
Fournisseur de chiffrement               | N’importe quel fournisseur de stockage de clés (KSP). Les fournisseurs de services de chiffrement (CSP) hérités ne sont **pas** pris en charge.
Algorithme de clé                 | RSA
Taille de clé minimale              | 2 048 bits
Algorithme de signature           | Recommandé : SHA256
Utilisation de la clé                     | Signature numérique *et* chiffrement des données
Utilisation améliorée de la clé            | Authentification du serveur
Stratégie de renouvellement de clé            | Renouvelez avec la même clé. Le renouvellement des certificats SGH avec des clés différentes empêchera le démarrage des machines virtuelles protégées.
Nom d'objet                  | Recommandé : le nom ou l’adresse Web de votre société. Ces informations seront présentées aux propriétaires de machines virtuelles dans l’Assistant fichier de données de protection.

Ces exigences s’appliquent si vous utilisez des certificats sauvegardés par le matériel ou les logiciels.
Pour des raisons de sécurité, il est recommandé de créer vos clés SGH dans un module de sécurité matériel (HSM) pour empêcher la copie des clés privées sur le système.
Suivez les instructions de votre fournisseur HSM pour demander des certificats avec les attributs ci-dessus et veillez à installer et à autoriser le KSP HSM sur chaque nœud SGH.

Chaque nœud SGH doit avoir accès aux mêmes certificats de signature et de chiffrement.
Si vous utilisez des certificats logiciels, vous pouvez exporter vos certificats vers un fichier PFX avec un mot de passe et autoriser SGH à gérer les certificats pour vous.
Vous pouvez également choisir d’installer les certificats dans le magasin de certificats de l’ordinateur local sur chaque nœud SGH et de fournir l’empreinte numérique au SGH.
Les deux options sont expliquées dans la rubrique [initialiser le cluster SGH](guarded-fabric-initialize-hgs.md) .

## <a name="create-self-signed-certificates-for-test-scenarios"></a>Créer des certificats auto-signés pour les scénarios de test

Si vous créez un environnement de laboratoire SGH et que vous n’avez pas besoin d’utiliser une autorité de certification, vous pouvez créer des certificats auto-signés.
Vous recevrez un avertissement lors de l’importation des informations de certificat dans l’Assistant fichier de données de protection, mais toutes les fonctionnalités resteront identiques.

Pour créer des certificats auto-signés et les exporter vers un fichier PFX, exécutez les commandes suivantes dans PowerShell :

```powershell
$certificatePassword = Read-Host -AsSecureString -Prompt "Enter a password for the PFX file"

$signCert = New-SelfSignedCertificate -Subject "CN=HGS Signing Certificate"
Export-PfxCertificate -FilePath .\signCert.pfx -Password $certificatePassword -Cert $signCert
Remove-Item $signCert.PSPath

$encCert = New-SelfSignedCertificate -Subject "CN=HGS Encryption Certificate"
Export-PfxCertificate -FilePath .\encCert.pfx -Password $certificatePassword -Cert $encCert
Remove-Item $encCert.PSPath
```

## <a name="request-an-ssl-certificate"></a>Demander un certificat SSL

Toutes les clés et les informations sensibles transmises entre les hôtes Hyper-V et SGH sont chiffrées au niveau du message, c’est-à-dire que les informations sont chiffrées à l’aide de clés connues en tant que SGH ou Hyper-V, ce qui empêche un utilisateur de détecter votre trafic réseau et de voler des clés à vos machines virtuelles.
Toutefois, si vous avez des reqiurements de conformité ou préférez simplement le chiffrement de toutes les communications entre Hyper-V et SGH, vous pouvez configurer SGH avec un certificat SSL qui chiffrera toutes les données au niveau du transport.

Les hôtes Hyper-V et les nœuds SGH doivent approuver le certificat SSL que vous fournissez. il est donc recommandé de demander le certificat SSL auprès de votre autorité de certification d’entreprise. Lorsque vous demandez le certificat, veillez à spécifier les éléments suivants :

Propriété du certificat SSL | Valeur requise
-------------------------|---------------
Nom d'objet             | Nom de votre cluster SGH (nom de domaine complet du nom de réseau distribué ou objet ordinateur virtuel). Il s’agit de la concaténation de votre nom de service SGH fourni pour `Initialize-HgsServer` et votre nom de domaine SGH.
Autre nom de l’objet | Si vous comptez utiliser un autre nom DNS pour atteindre votre cluster SGH (par exemple, s’il se trouve derrière un équilibreur de charge), veillez à inclure ces noms DNS dans le champ SAN de votre demande de certificat.

Les options permettant de spécifier ce certificat lors de l’initialisation du serveur SGH sont décrites dans [configurer le premier nœud SGH](guarded-fabric-initialize-hgs.md).
Vous pouvez également ajouter ou modifier le certificat SSL ultérieurement à l’aide de l’applet de commande [Set-HgsServer](https://docs.microsoft.com/powershell/module/hgsserver/set-hgsserver?view=win10-ps) .

## <a name="next-step"></a>Étape suivante

> [!div class="nextstepaction"]
> [Installer SGH](guarded-fabric-choose-where-to-install-hgs.md)