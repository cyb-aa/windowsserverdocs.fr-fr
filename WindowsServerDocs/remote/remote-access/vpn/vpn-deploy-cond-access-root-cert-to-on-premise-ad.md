---
title: Déployer des certificats racine d'accès conditionnel aux services AD locaux
description: ''
services: active-directory
ms.prod: windows-server
ms.technology: networking-ras
ms.workload: identity
ms.topic: article
ms.date: 06/28/2019
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.reviewer: deverette
ms.openlocfilehash: 67d361db7a2dd3f2879e8beb924075dae68d52a3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404319"
---
# <a name="step-74-deploy-conditional-access-root-certificates-to-on-premises-ad"></a>Étape 7.4. Déployer des certificats racine d’accès conditionnel vers AD local

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows 10

Dans cette étape, vous allez déployer le certificat racine d’accès conditionnel en tant que certificat racine approuvé pour l’authentification VPN sur votre annuaire Active Directory local.

- [**Précédent :** Étape 7,3. Configurer la stratégie d’accès conditionnel](vpn-config-conditional-access-policy.md)
- [**Ensuite :** Étape 7,5. Créer des profils VPNv2 basés sur OMA-DM sur des appareils Windows 10](vpn-create-oma-dm-based-vpnv2-profiles.md)

1. Dans la page **connectivité VPN** , sélectionnez **Télécharger le certificat**.

   >[!NOTE]
   >L’option **Télécharger le certificat base64** est disponible pour certaines configurations qui requièrent des certificats Base64 pour le déploiement.

2. Connectez-vous à un ordinateur appartenant à un domaine disposant de droits d’administrateur d’entreprise et exécutez ces commandes à partir d’une invite de commandes d’administrateur pour ajouter le ou les certificats racine du Cloud dans le magasin *NTauth d’entreprise* :

   >[!NOTE]
   >Pour les environnements où le serveur VPN n’est pas joint au domaine Active Directory, les certificats racine du Cloud doivent être ajoutés manuellement au magasin _autorités de certification racines de confiance_ .

   | Commande | Description |
   | --- | --- |
   | `certutil -dspublish -f VpnCert.cer RootCA` | Crée deux conteneurs de l’autorité de certification **racine VPN Microsoft** dans les conteneurs **CN = AIA** et **CN = autorités de certification** , et publie chaque certificat racine en tant que valeur sur l’attribut _caCertificate_ des deux conteneurs de l' **autorité de certification racine VPN Microsoft** . |
   | `certutil -dspublish -f VpnCert.cer NTAuthCA` | Crée un **conteneur CN = NTAuthCertificates** sous les conteneurs **CN = AIA** et **CN = autorités de certification** , et publie chaque certificat racine en tant que valeur sur l’attribut _caCertificate_ du conteneur **CN = NTAuthCertificates** . |
   | `gpupdate /force` | Accélère l’ajout des certificats racine sur les ordinateurs clients et Windows Server. |

3. Vérifiez que les certificats racine sont présents dans le magasin Enterprise NTauth et qu’ils s’affichent comme approuvés :
   1. Connectez-vous à un serveur avec des droits d’administrateur d’entreprise sur lesquels sont installés les outils de gestion de l' **autorité de certification** .

   >[!NOTE]
   >Par défaut, les outils de gestion de l' **autorité de certification** sont des serveurs d’autorité de certification installés. Ils peuvent être installés sur d’autres serveurs membres dans le cadre des **Outils d’administration de rôle** dans Gestionnaire de serveur.

   1. Sur le serveur VPN, dans le menu Démarrer, entrez **PKIView. msc** pour ouvrir la boîte de dialogue PKI d’entreprise.
   1. Dans le menu Démarrer, entrez **PKIView. msc** pour ouvrir la boîte de dialogue PKI d’entreprise.
   1. Cliquez avec le bouton droit sur **PKI d’entreprise** , puis sélectionnez **gérer les conteneurs ad**.
   1. Vérifiez que chaque certificat de l’autorité de certification racine VPN Microsoft est présent sous :
      - NTAuthCertificates
      - Conteneur AIA
      - Conteneur d’autorités de certification

## <a name="next-steps"></a>Étapes suivantes

[Étape 7,5. Créer des profils VPNv2 basés sur OMA-DM sur des appareils Windows 10](vpn-create-oma-dm-based-vpnv2-profiles.md): au cours de cette étape, vous pouvez créer des profils VPNv2 basés sur OMA-DM à l’aide d’Intune pour déployer une stratégie de configuration d’appareil VPN. Si vous souhaitez créer un script SCCM ou PowerShell pour créer des profils VPNv2, consultez [paramètres CSP VPNv2](https://docs.microsoft.com/windows/client-management/mdm/vpnv2-csp) pour plus d’informations.
