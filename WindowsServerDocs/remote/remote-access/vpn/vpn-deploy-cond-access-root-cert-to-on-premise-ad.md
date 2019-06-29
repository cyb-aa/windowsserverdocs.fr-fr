---
title: Déployer des certificats racine d'accès conditionnel aux services AD locaux
description: ''
services: active-directory
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.workload: identity
ms.topic: article
ms.date: 06/28/2019
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.reviewer: deverette
ms.openlocfilehash: 200d3b96ee24b5e1264b4bf2e42d636f9e07fbef
ms.sourcegitcommit: 63926404009f9e1330a4a0aa8cb9821a2dd7187e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67469681"
---
# <a name="step-74-deploy-conditional-access-root-certificates-to-on-premises-ad"></a>Étape 7.4. Déployer des certificats racine de l’accès conditionnel en local AD

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows 10

Dans cette étape, vous déployez le certificat racine de l’accès conditionnel en tant que certificat racine approuvé pour l’authentification VPN sur votre réseau local AD.

- [**Précédent :** Étape 7.3. Configurer la stratégie d’accès conditionnel](vpn-config-conditional-access-policy.md)
- [**prochain :** Étape 7.5. Créer des profils VPNv2 basés sur OMA-DM sur les appareils Windows 10](vpn-create-oma-dm-based-vpnv2-profiles.md)

1. Sur le **connectivité VPN** page, sélectionnez **télécharger le certificat**.

   >[!NOTE]
   >Le **télécharger le certificat au format base64** option est disponible pour certaines configurations qui nécessitent des certificats de base64 pour le déploiement.

2. Ouvrez une session sur un ordinateur joint au domaine avec droits d’administrateur d’entreprise et d’exécuter ces commandes à partir d’une invite de commandes administrateur pour ajouter le cloud racine ou les certificats dans le *Enterprise NTauth* stocker :

   >[!NOTE]
   >Pour les environnements où le serveur VPN n’est pas joint au domaine Active Directory, les certificats racines de cloud doivent être ajoutés à la _Trusted Root Certification Authorities_ stocker manuellement.

   | Command | Description |
   | --- | --- |
   | `certutil -dspublish -f VpnCert.cer RootCA` | Crée deux **racine VPN Microsoft génération de l’autorité de certification 1** conteneurs sous le **CN = AIA** et **CN = autorités de Certification** conteneurs et publie chaque certificat racine en tant que valeur sur le _cACertificate_ attribut des deux **racine VPN Microsoft génération de l’autorité de certification 1** conteneurs. |
   | `certutil -dspublish -f VpnCert.cer NTAuthCA` | Crée un **CN = NTAuthCertificates** conteneur sous la **CN = AIA** et **CN = autorités de Certification** conteneurs et publie chaque certificat racine en tant que valeur sur le _cACertificate_ attribut de la **CN = NTAuthCertificates** conteneur. |
   | `gpupdate /force` | Accélère l’ajout de certificats racines pour les ordinateurs clients et le serveur de Windows. |

3. Vérifiez que les certificats racine sont présents dans le magasin NTauth d’entreprise et les affiche comme étant approuvée :
   1. Ouvrez une session sur un serveur avec des droits d’administrateur d’entreprise qui possède le **outils de gestion d’autorité de certificat** installé.

   >[!NOTE]
   >Par défaut le **outils de gestion d’autorité de certificat** sont des serveurs d’autorité de certification installés. Ils peuvent être installés sur d’autres serveurs membres dans le cadre de la **outils d’Administration de rôles** dans le Gestionnaire de serveur.

   1. Sur le serveur VPN, dans le menu Démarrer, entrez **pkiview.msc** pour ouvrir la boîte de dialogue PKI d’entreprise.
   1. Dans le menu Démarrer, entrez **pkiview.msc** pour ouvrir la boîte de dialogue PKI d’entreprise.
   1. Avec le bouton droit **PKI d’entreprise** et sélectionnez **gérer Active Directory conteneurs**.
   1. Vérifiez que chaque certificat de génération 1 d’autorité de certification racine Microsoft VPN est présent sous :
      - NTAuthCertificates
      - Conteneur AIA
      - Conteneur autorités de certificat

## <a name="next-steps"></a>Étapes suivantes

[Étape 7.5. Créer OMA-DM en fonction des profils de VPNv2 pour les appareils Windows 10](vpn-create-oma-dm-based-vpnv2-profiles.md): Dans cette étape, vous pouvez créer OMA-DM profils VPNv2 à l’aide d’Intune pour déployer une stratégie de Configuration de l’appareil VPN. Si vous souhaitez SCCM ou un PowerShell Script pour créer des profils de VPNv2, consultez [paramètres VPNv2 CSP](https://docs.microsoft.com/windows/client-management/mdm/vpnv2-csp) pour plus d’informations.
