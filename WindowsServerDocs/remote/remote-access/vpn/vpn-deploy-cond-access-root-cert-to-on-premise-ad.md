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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837370"
---
# <a name="step-74-deploy-conditional-access-root-certificates-to-on-premises-ad"></a>Étape 7.4. Déployer des certificats racine de l’accès conditionnel en local AD

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows 10

Dans cette étape, vous déployez le certificat racine de l’accès conditionnel en tant que certificat racine approuvé pour l’authentification VPN sur votre réseau local AD.

&#171;  [**Précédent :** Étape 7.3. Configurer la stratégie d’accès conditionnel](vpn-config-conditional-access-policy.md)<br>
&#187;[ **Suivant :** Étape 7.5. Créer OMA-DM en fonction des profils de VPNv2 pour les appareils Windows 10](vpn-create-oma-dm-based-vpnv2-profiles.md)

1. Sur le **connectivité VPN** , cliquez sur **télécharger le certificat**. 
   
    ![Télécharger le certificat pour l’accès conditionnel](../../media/Always-On-Vpn/06.png)

    >[!NOTE]
    >Le **télécharger le certificat au format base64** option est disponible pour certaines configurations qui nécessitent des certificats de base64 pour le déploiement. 

2. Ouvrez une session sur un ordinateur joint au domaine avec droits d’administrateur d’entreprise et d’exécuter ces commandes à partir d’une invite de commandes administrateur pour ajouter le cloud racine ou les certificats dans le *Enterprise NTauth* stocker :

    >[!NOTE]
    >Pour les environnements où le serveur VPN n’est pas joint au domaine Active Directory, les certificats racines de cloud doivent être ajoutés à la _Trusted Root Certification Authorities_ stocker manuellement.

    |Command  |Description  |  
    |---------|-------------| 
    |`certutil -dspublish -f VpnCert.cer RootCA`     |Crée deux **racine VPN Microsoft génération de l’autorité de certification 1** conteneurs sous le **CN = AIA** et **CN = autorités de Certification** conteneurs et publie chaque certificat racine en tant que valeur sur le _cACertificate_ attribut des deux **racine VPN Microsoft génération de l’autorité de certification 1** conteneurs.|  
    |`certutil -dspublish -f VpnCert.cer NTAuthCA`   |Crée un **CN = NTAuthCertificates** conteneur sous la **CN = AIA** et **CN = autorités de Certification** conteneurs et publie chaque certificat racine en tant que valeur sur le _cACertificate_ attribut de la **CN = NTAuthCertificates** conteneur. |  
    |`gpupdate /force`     |Accélère l’ajout de certificats racines pour les ordinateurs clients et le serveur de Windows.  |

3.  Vérifiez que les certificats racine sont présents dans le magasin NTauth d’entreprise et les affiche comme étant approuvée :

    a.  Ouvrez une session sur un serveur avec des droits d’administrateur d’entreprise qui possède le **outils de gestion d’autorité de certificat** installé.

    >[!NOTE]
    >Par défaut le **outils de gestion d’autorité de certificat** sont des serveurs d’autorité de certification installés. Ils peuvent être installés sur d’autres serveurs membres dans le cadre de la **outils d’Administration de rôles** dans le Gestionnaire de serveur.

    b.  Sur le serveur VPN, dans le menu Démarrer, tapez **pkiview.msc** pour ouvrir la boîte de dialogue PKI d’entreprise.

    c.  Dans le menu Démarrer, tapez **pkiview.msc** pour ouvrir la boîte de dialogue PKI d’entreprise.

    d.  Avec le bouton droit **PKI d’entreprise** et sélectionnez **gérer Active Directory conteneurs**.

    d.  Vérifiez que chaque certificat de génération 1 d’autorité de certification racine Microsoft VPN est présent sous :<ul><li>NTAuthCertificates</li><li>Conteneur AIA</li><li>Conteneur autorités de certificat</li></ul>

    
## <a name="next-step"></a>Étape suivante
[Étape 7.5. Créer OMA-DM en fonction des profils de VPNv2 pour les appareils Windows 10](vpn-create-oma-dm-based-vpnv2-profiles.md): Dans cette étape, vous pouvez créer OMA-DM profils VPNv2 à l’aide d’Intune pour déployer une stratégie de Configuration de l’appareil VPN. Si vous souhaitez SCCM ou un PowerShell Script pour créer des profils de VPNv2, consultez [paramètres VPNv2 CSP](https://docs.microsoft.com/windows/client-management/mdm/vpnv2-csp) pour plus d’informations.

---
