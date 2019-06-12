---
title: Obtenir des certificats pour SGH
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: f4b4d1a8-bf6d-4881-9150-ddeca8b48038
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 2dc232eb7aeb8b0807a8e9989ae3dc893f925f66
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447366"
---
# <a name="obtain-certificates-for-hgs"></a>Obtenir des certificats pour SGH

>S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016

Lorsque vous déployez SGH, vous devrez fournir des certificats de signature et de chiffrement qui sont utilisés pour protéger les informations sensibles nécessaires pour démarrer une machine virtuelle protégée.
Ces certificats jamais laisser SGH et servent uniquement à des clés de machine virtuelle decrypt protégée lors de l’hôte sur lequel ils s’exécutent s’est avérée, qu'il est intègre.
Locataires (propriétaires d’ordinateurs virtuels) utilisent les public de la moitié des certificats pour autoriser votre centre de données pour exécuter leurs machines virtuelles protégées.
Cette section décrit les étapes nécessaires pour obtenir des certificats de signature et le chiffrement compatibles pour SGH.

## <a name="request-certificates-from-your-certificate-authority"></a>Demander des certificats à partir de votre autorité de certification

Bien que non obligatoire, il est fortement recommandé d’obtenir vos certificats à partir d’une autorité de certification approuvée.
Ce faisant, les propriétaires de machine virtuelle vérifier qu’ils sont autorisation du serveur SGH correct (par exemple, le fournisseur de services ou le centre de données) pour exécuter leurs machines virtuelles protégées.
Dans un scénario d’entreprise, vous pouvez choisir d’utiliser votre propre autorité de certification d’entreprise pour émettre ces certificats.
Les hébergeurs et les fournisseurs de services doivent prendre en compte à l’aide d’une autorité de certification publique, bien connue, à la place.

Les certificats à la fois la signature et le chiffrement doivent être émis avec les propriétés suivantes de certificiate (sauf s’il est marqué « recommandés ») :

Propriété de modèle de certificat | Valeur requise 
------------------------------|----------------
Fournisseur de chiffrement               | N’importe quel fournisseur de stockage de clés (KSP). Hérité fournisseurs de services cryptographiques (CSP) sont **pas** pris en charge.
Algorithme de clé                 | RSA
Taille de clé minimale              | 2 048 bits
Algorithme de signature           | Recommandé : SHA256
Utilisation de la clé                     | Signature numérique *et* chiffrement des données
Utilisation de clé améliorée            | Authentification du serveur
Stratégie de renouvellement de clé            | Renouveler avec la même clé. Renouvellement des certificats de service SGH avec des clés différentes empêche des machines virtuelles protégées de démarrage.
Nom d'objet                  | Recommandé : votre société nom ou l’adresse web. Ces informations seront affichera pour les propriétaires d’ordinateurs virtuels dans l’Assistant fichier de données protection.

Ces exigences s’appliquent si vous utilisez des certificats assorties de matériel ou logiciel.
Pour des raisons de sécurité, il est recommandé de créer vos clés de SGH dans un Module de sécurité matériel (HSM) pour empêcher que les clés privées sont copiées sur le système.
Suivez les instructions à partir de votre fournisseur HSM pour demander des certificats avec les attributs ci-dessus et veillez à installer et autoriser le KSP HSM sur chaque nœud SGH.

Chaque nœud SGH nécessitent un accès à la même signature et les certificats de chiffrement.
Si vous utilisez des certificats reposant sur le logiciel, vous pouvez exporter vos certificats vers un fichier PFX avec un mot de passe et autoriser SGH pour gérer les certificats pour vous.
Vous pouvez également installer les certificats dans le magasin de certificats de l’ordinateur local sur chaque nœud SGH et fournir l’empreinte numérique à SGH.
Les deux options sont expliquées dans le [initialiser le Cluster SGH](guarded-fabric-initialize-hgs.md) rubrique.

## <a name="create-self-signed-certificates-for-test-scenarios"></a>Créer des certificats auto-signés pour les scénarios de test

Si vous créez un environnement de laboratoire SGH et ne pas avoir ou à utiliser une autorité de certification, vous pouvez créer des certificats auto-signés.
Vous recevrez un avertissement lors de l’importation des informations de certificat dans l’Assistant fichier de données protection, mais toutes les fonctionnalités restent les mêmes.

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

Toutes les clés et les informations sensibles sont transmises entre les hôtes Hyper-V et SGH est chiffrés au niveau du message : autrement dit, les informations sont chiffrées avec des clés connues au service SGH ou Hyper-V, empêchant un utilisateur à partir de la détection de votre trafic réseau et de voler des clés à vos machines virtuelles.
Toutefois, si vous avez reqiurements de conformité ou que vous préférez simplement chiffrer toutes les communications entre Hyper-V et SGH, vous pouvez configurer SGH avec un certificat SSL qui chiffre toutes les données au niveau du transport.

Les hôtes Hyper-V et les nœuds de SGH seront doivent approuver le certificat SSL que vous fournissez, il est donc recommandé que vous demandez le certificat SSL à partir de votre autorité de certification d’entreprise. Lorsque vous demandez le certificat, veillez à spécifier les éléments suivants :

Propriété du certificat SSL | Valeur requise
-------------------------|---------------
Nom d'objet             | Nom de votre cluster SGH (nom de réseau distribué). Il s’agit de la concaténation du nom de votre service SGH fourni à `Initialize-HgsServer` et votre nom de domaine SGH.
Autre nom d’objet | Si vous l’utiliserez un autre nom DNS pour atteindre votre cluster SGH (par exemple, s’il se trouve derrière un équilibreur de charge), veillez à inclure les noms DNS dans le champ SAN votre demande de certificat.

Les options pour spécifier ce certificat lors de l’initialisation du serveur SGH couvertes dans [configurer le premier nœud SGH](guarded-fabric-initialize-hgs.md).
Vous pouvez également ajouter ou modifier le certificat SSL à un moment ultérieur à l’aide du [Set-HgsServer](https://docs.microsoft.com/powershell/module/hgsserver/set-hgsserver?view=win10-ps) applet de commande.

## <a name="next-step"></a>Étape suivante

> [!div class="nextstepaction"]
> [Installer SGH](guarded-fabric-choose-where-to-install-hgs.md)