---
title: Déployer des certificats racine d'accès conditionnel aux services AD locaux
description: ''
services: active-directory
ms.prod: windows-server-threshold
ms.technology: networking-ras
documentationcenter: ''
ms.assetid: ''
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2018
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.reviewer: deverette
ms.openlocfilehash: 210540846f5d62dfc74a2e629a6b7675ccf9894d
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6067123"
---
# Étape7.4. Déployer des certificats racine de l’accès conditionnel sur site AD

>S’applique à: Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows 10

Dans cette étape, vous déployez le certificat racine de l’accès conditionnel comme certificat racine de confiance pour l’authentification VPN vers votre site sur AD.

& #171;  [ **Précédente:** étape 7.3. Configurer la stratégie d’accès conditionnel](vpn-config-conditional-access-policy.md)<br>
& #187; [ **Suivant:** étape 7.5. Créer des OMA-DM en fonction des profils de VPNv2 pour les appareils Windows 10](vpn-create-oma-dm-based-vpnv2-profiles.md)

1. Sur la page de **connexion VPN** , cliquez sur **Télécharger le certificat**. 
   
    ![Téléchargez le certificat pour l’accès conditionnel](../../media/Always-On-Vpn/06.png)

    >[!NOTE]
    >La possibilité de **Télécharger le certificat au format base64** est disponible pour certaines configurations qui nécessitent des certificats de base64 pour le déploiement. 

2. Ouvrez une session sur un ordinateur joint au domaine avec des droits d’administrateur d’entreprise et exécutez les commandes suivantes à partir d’une invite de commandes administrateur pour ajouter l’ou les certificats racine cloud dans le magasin *NTauth de l’entreprise* :

    >[!NOTE]
    >Pour les environnements où le serveur VPN n’est pas joint au domaine Active Directory, les certificats racines de cloud doivent être ajoutés manuellement dans le magasin _d’Autorités de Certification racine de confiance_ .

    |Commande  |Description  |  
    |---------|-------------| 
    |`certutil -dspublish -f VpnCert.cer RootCA`     |Crée deux conteneurs **VPN Microsoft racine génération d’autorité de certification 1** sous le **CN = AIA** et **CN = autorités de Certification** conteneurs et publie le certificat racine de chaque en tant que valeur de l’attribut _cACertificate_ de ces deux racine **VPN Microsoft Génération de l’autorité de certification 1** conteneurs.|  
    |`certutil -dspublish -f VpnCert.cer NTAuthCA`   |Crée un **CN = NTAuthCertificates** conteneur sous la **CN = AIA** et **CN = autorités de Certification** conteneurs et publie le certificat racine de chaque en tant que valeur de l’attribut _cACertificate_ de la **CN = NTAuthCertificates** conteneur. |  
    |`gpupdate /force`     |Accélère l’ajout de certificats racines à Windows server et les ordinateurs clients.  |

3.  Vérifiez que les certificats racines sont présentes dans le magasin NTauth d’entreprise et l’afficher comme fiable:

    a.  Connectez-vous à un serveur avec des droits d’administrateur d’entreprise qui a installé les **Outils de gestion d’autorité de certificat** .

    >[!NOTE]
    >Par défaut les **Outils de gestion d’autorité de certificat** sont installés serveurs d’autorité de certification. Ils peuvent être installées sur d’autres serveurs membres dans le cadre des **Outils d’Administration de rôles** dans le Gestionnaire de serveur.

    b.  Sur le serveur VPN, dans le menu Démarrer, tapez **pkiview.msc** pour ouvrir la boîte de dialogue PKI d’entreprise.

    c.  Dans le menu Démarrer, tapez **pkiview.msc** pour ouvrir la boîte de dialogue PKI d’entreprise.

    d.  Avec le bouton droit **PKI d’entreprise** et sélectionnez **Gérer les conteneurs Active Directory**.

    d.  Vérifiez que chaque certificat de génération 1 VPN Microsoft autorité de certification racine est présent sous:<ul><li>NTAuthCertificates</li><li>Conteneur AIA</li><li>Conteneur autorités de certificat</li></ul>

    
## Étape suivante
Étape [7.5. Créer des OMA-DM en fonction des profils de VPNv2 pour les appareils Windows 10](vpn-create-oma-dm-based-vpnv2-profiles.md): dans cette étape, vous pouvez créer des OMA-DM profils VPNv2 à l’aide d’Intune pour déployer une stratégie de Configuration de l’appareil VPN. Si vous souhaitez SCCM ou un PowerShell Script pour créer des profils de VPNv2, reportez-vous à la section [paramètres CSP VPNv2](https://docs.microsoft.com/windows/client-management/mdm/vpnv2-csp) pour plus d’informations.

---
