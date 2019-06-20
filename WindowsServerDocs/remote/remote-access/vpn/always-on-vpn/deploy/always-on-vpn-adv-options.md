---
title: Fonctionnalités avancées de VPN Toujours actif (AlwaysOn)
description: Au-delà du scénario de déploiement fourni dans ce déploiement, vous pouvez ajouter d’autres fonctionnalités avancées de VPN pour améliorer la sécurité et la disponibilité de votre connexion VPN.
ms.assetid: 51a1ee61-3ffe-4f65-b8de-ff21903e1e74
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.date: 11/05/2018
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.reviewer: deverette
ms.openlocfilehash: 5f43d64dc7642ef67da03fec989909bc4f2f14ae
ms.sourcegitcommit: a3c9a7718502de723e8c156288017de465daaf6b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/19/2019
ms.locfileid: "67263027"
---
# <a name="advanced-features-of-always-on-vpn"></a>Fonctionnalités avancées de VPN Always On

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Précédent :** En savoir plus sur la technologie VPN Always On](../always-on-vpn-technology-overview.md)
- [**prochain :** Commencez à planifier le déploiement VPN Always On](always-on-vpn-deploy-planning.md)

Au-delà des scénarios de déploiement fournis, vous pouvez ajouter d’autres fonctionnalités avancées de VPN pour améliorer la sécurité et la disponibilité de votre connexion VPN. Par exemple, ces composants peuvent aider à vous assurer que le client qui se connecte est sain avant d’autoriser une connexion.

## <a name="high-availability"></a>Haute disponibilité

Les options supplémentaires pour la haute disponibilité sont les suivantes.

|Option  |Description  |
|---------|---------|
|Résilience des serveurs et équilibrage de charge     |Dans les environnements qui requièrent une haute disponibilité ou la prise en charge grand nombre de demandes, vous pouvez augmenter les performances et la résilience d’accès à distance à l’aide d’équilibrage de charge entre plusieurs serveurs qui exécutent le serveur NPS (Network Policy Server) et activation à distance Serveur d’accès au clustering.<p>Documents associés :<ul><li>[Équilibrage de charge de serveur de Proxy de serveur NPS](../../../../../networking/technologies/nps/nps-manage-proxy-lb.md)</li><li>[Déployer l’accès à distance dans un cluster](https://docs.microsoft.com/windows-server/remote/remote-access/ras/cluster/deploy-remote-access-in-cluster)</li></ul>        |
|Résilience de site géographique     |Pour la géolocalisation basé sur IP, vous pouvez utiliser Global Traffic Manager avec DNS dans Windows Server 2016. Pour l’équilibrage de charge géographique plus robuste, vous pouvez utiliser des solutions globales Server l’équilibrage de charge, tels que Microsoft Azure Traffic Manager.<p>Documents associés :<ul><li>[Vue d’ensemble de Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview)</li><li>[Microsoft Azure Traffic Manager](https://azure.microsoft.com/services/traffic-manager)</li></ul>         |

## <a name="advanced-authentication"></a>Authentification avancée

Les éléments suivants sont des options supplémentaires pour l’authentification.

|Option  |Description  |
|---------|---------|
|Windows Hello Entreprise     |Dans Windows 10, Windows Hello Entreprise remplace les mots de passe par l’authentification forte à 2 facteurs sur les PC et appareils mobiles. Cette authentification se compose d’un nouveau type d’informations d’identification qui sont liée à un appareil et utilise un biométrique ou numéro d’Identification personnel (PIN).<p>Le client VPN Windows 10 est compatible avec Windows Hello entreprise. Une fois que l’utilisateur se connecte avec un mouvement, la connexion VPN utilise le Windows Hello pour le certificat d’entreprise pour l’authentification basée sur le certificat.<p>Documents associés :<ul><li>[Windows Hello entreprise](https://docs.microsoft.com/windows/access-protection/hello-for-business/hello-identity-verification)</li><li>Étude de cas technique : [L’activation de l’accès à distance avec Windows Hello for Business dans Windows 10](https://msdn.microsoft.com/library/mt728163.aspx)</li></ul>         |
|Authentification multifacteur Azure (MFA)     |Azure MFA a cloud locales et dans les versions que vous pouvez intégrer avec le mécanisme d’authentification VPN de Windows.<p>Pour plus d’informations sur le fonctionne de ce mécanisme, consultez [l’authentification RADIUS s’intègrent avec le serveur Azure multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-server-radius).         |

## <a name="advanced-vpn-features"></a>Fonctionnalités VPN avancées

Les options supplémentaires pour les fonctionnalités avancées sont les suivantes.

|Option  |Description  |
|---------|---------|
|Filtrage du trafic     |Si vous avez besoin appliquer les applications VPN clients peuvent accéder à, vous pouvez activer les filtres de trafic VPN.<p>Pour plus d’informations, consultez [les fonctionnalités de sécurité VPN](https://docs.microsoft.com/windows/access-protection/vpn/vpn-security-features).         |
|VPN déclenché par les applications     |Vous pouvez configurer des profils VPN pour se connecter automatiquement lorsque vous démarrent de certaines applications ou certains types d’applications.<p>Pour plus d’informations sur cela et d’autres options de déclenchement, consultez [options déclenché automatiquement le profil VPN](https://docs.microsoft.com/windows/access-protection/vpn/vpn-auto-trigger-profile).         |
|Accès conditionnel de VPN   |Conformité de dispositif et d’accès conditionnelle peut nécessiter des appareils gérés afin de répondre aux normes avant de se connecter au VPN. L’une des fonctionnalités avancées pour l’accès conditionnel VPN vous permet de restreindre les connexions VPN à ceux où le certificat d’authentification client contient « Des AAD l’accès conditionnel OID de « 1.3.6.1.4.1.311.87 ».<p>Pour restreindre les connexions VPN, vous devez :<ol><li>Sur le serveur NPS, ouvrez le **Network Policy Server** enfichable.</li><li>Développez **stratégies** > **stratégies réseau**.</li><li>Avec le bouton droit le **des connexions de réseau privé virtuel (VPN, Virtual Private Network)** stratégie de réseau, puis sélectionnez **propriétés**.</li><li>Sélectionnez le **paramètres** onglet.</li><li>Sélectionnez **fournisseur spécifique** et sélectionnez **ajouter**.</li><li>Sélectionnez le **OID de certificat autorisées** option, puis sélectionnez **ajouter**.</li><li>Collez l’OID d’accès conditionnel AAD de **1.3.6.1.4.1.311.87** comme la valeur d’attribut, puis sélectionnez **OK** à deux reprises.</li><li>Sélectionnez **fermer** , puis **appliquer**.<p>Désormais, lorsque les clients VPN tentent de vous connecter à l’aide de n’importe quel certificat autre que le certificat de cloud de courte durée, la connexion échoue.</li></ol>Pour plus d’informations sur l’accès conditionnel, consultez [VPN et l’accès conditionnel](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access).   |


---
## <a name="blocking-vpn-clients-that-use-revoked-certificates"></a>Blocage des Clients VPN qui utilisent des certificats révoqués
  
Après avoir installé les mises à jour, le serveur RRAS peut appliquer la révocation de certificats pour les VPN qui utilisent le protocole IKEv2 et certificats d’ordinateur pour l’authentification, tels que le périphérique du tunnel VPN Always on. Cela signifie que pour ce type de VPN, le serveur RRAS peut refuser les connexions VPN pour les clients qui tentent d’utiliser un certificat révoqué.

**Disponibilité**

Le tableau suivant répertorie les dates de lancement approximative des correctifs pour chaque version de Windows.

|Version du système d'exploitation |Date de version * |
|---------|---------|
|Windows Server, version 1903  |Q2, 2019  |
|Windows Server 2019<br />Windows Server, version 1809  |Q3, 2019  |
|Windows Server, version 1803  |Q3, 2019  |
|Windows Server, version 1709  |Q3, 2019  |
|Windows Server 2016, version 1607  |Q2, 2019  |
  
\* Toutes les dates de version sont répertoriées dans les trimestres calendaires. Dates sont approximatifs et peuvent changer sans préavis.

**Comment configurer les composants requis** 

1. Installer les mises à jour de Windows lorsqu’elles sont disponibles.
1. Assurez-vous que tous les certificats de serveur RRAS que vous utilisez et de client VPN ont des entrées CDP et que le serveur RRAS peut atteindre les CRL respectifs.
1. Sur le serveur RRAS, utilisez le **Set-VpnAuthProtocol** applet de commande PowerShell pour configurer le **RootCertificateNameToAccept** paramètre.<br /><br />
   L’exemple suivant répertorie les commandes pour ce faire. Dans l’exemple, **CN = Autorité de Certification racine Contoso** représente le nom unique de l’autorité de Certification racine. 
   ``` powershell
   $cert1 = ( Get-ChildItem -Path cert:LocalMachine\root | Where-Object -FilterScript { $_.Subject -Like "*CN=Contoso Root Certification Authority,*" } )
   Set-VpnAuthProtocol -RootCertificateNameToAccept $cert1 -PassThru
   ```
**Comment configurer le serveur RRAS pour appliquer la révocation de certificats pour les connexions VPN qui sont basées sur les certificats d’ordinateur IKEv2**

1. Dans une fenêtre d’invite de commandes, exécutez la commande suivante : 
   ```
   reg add HKLM\SYSTEM\CurrentControlSet\Services\RemoteAccess\Parameters\Ikev2 /f /v CertAuthFlags /t REG_DWORD /d "4"
   ```

1. Redémarrez le **routage et accès distant** service.
  
Pour désactiver la révocation de certificats pour les connexions VPN, définissez **CertAuthFlags = 2** ou supprimer la **CertAuthFlags** valeur, puis redémarrez le **routage et accès distant**service. 

**Pour révoquer un certificat de client VPN pour une connexion VPN basée sur un certificat d’ordinateur IKEv2**
1. Révoquer le certificat de client VPN à partir de l’autorité de Certification.
1. Publier une nouvelle liste de révocation de l’autorité de Certification.
1. Sur le serveur RRAS, ouvrez une fenêtre d’invite de commandes d’administration, puis exécutez les commandes suivantes :
   ```
   certutil -urlcache * delete
   certutil -setreg chain\ChainCacheResyncFiletime @now
   ```

**Comment vérifier que la révocation de certificats pour les connexions VPN basée sur certificat d’ordinateur IKEv2 fonctionne**  
>[!Note]  
> Avant d’utiliser cette procédure, assurez-vous que vous activez le journal des événements opérationnel CAPI2.
1. Suivez les étapes précédentes pour révoquer un certificat de client VPN.
1. Essayez de vous connecter au VPN à l’aide d’un client qui possède le certificat révoqué. Le serveur RRAS doit refuser la connexion et affiche un message comme « informations d’identification de l’authentification IKE sont inacceptables ».
1. Sur le serveur RRAS, ouvrez l’Observateur d’événements et accédez à **Applications et les journaux de Services/Microsoft/Windows/CAPI2**. 
1. Recherchez un événement qui comporte les informations suivantes :
   * Nom du journal : **Microsoft-Windows-CAPI2/Operational Microsoft-Windows-CAPI2/Operational**
   * ID d'événement : **41** 
   * L’événement contient le texte suivant : **sujet = «*Client FQDN*»** (*Client FQDN* représente le nom de domaine complet du client qui a le révoqués certificat.) 

   Le **<Result>** champ des données d’événement doit inclure **le certificat est révoqué**. Par exemple, consultez les extraits suivants à partir d’un événement :
   ```xml
   Log Name:      Microsoft-Windows-CAPI2/Operational Microsoft-Windows-CAPI2/Operational  
   Source:        Microsoft-Windows-CAPI2  
   Date:          5/20/2019 1:33:24 PM  
   Event ID:      41  
   ...  
   Event Xml:
   <Event xmlns="http://schemas.microsoft.com/win/2004/08/events/event">
    <UserData>  
     <CertVerifyRevocation>  
      <Certificate fileRef="C97AE73E9823E8179903E81107E089497C77A720.cer" subjectName="client01.corp.contoso.com" />  
      <IssuerCertificate fileRef="34B1AE2BD868FE4F8BFDCA96E47C87C12BC01E3A.cer" subjectName="Contoso Root Certification Authority" />
      ...
      <Result value="80092010">The certificate is revoked.</Result>
     </CertVerifyRevocation>
    </UserData>
   </Event>
   ```

---
## <a name="additional-protection"></a>Protection supplémentaire

### <a name="trusted-platform-module-tpm-key-attestation"></a>Attestation de clé Trusted Platform Module (TPM)

Un certificat de l’utilisateur avec une clé attesté par le module de plateforme sécurisée fournit la garantie de sécurité plus élevé, sauvegardé par non-exportabilité anti-hameçonnage, isolation et de clés fournies par le module de plateforme sécurisée.

Pour plus d’informations sur l’attestation de clé TPM dans Windows 10, consultez [Attestation de clé TPM](https://docs.microsoft.com/windows-server/identity/ad-ds/manage/component-updates/tpm-key-attestation).

## <a name="next-step"></a>Étape suivante

[Commencez à planifier le déploiement VPN Always On](always-on-vpn-deploy-planning.md): Avant d’installer le rôle de serveur d’accès à distance sur l’ordinateur que vous avez l’intention d’utiliser comme un serveur VPN, procédez comme suit. Après une planification appropriée, vous pouvez déployer VPN Toujours actif (AlwaysOn) et éventuellement configurer l’accès conditionnel pour une connectivité VPN à l’aide d’Azure AD.  

## <a name="related-topics"></a>Rubriques connexes
- [L’équilibrage de charge du serveur NPS Proxy](../../../../../networking/technologies/nps/nps-manage-proxy-lb.md): Clients distants d’authentification Dial-In Service RADIUS (User), qui sont des serveurs d’accès réseau comme les serveurs de réseau privé virtuel (VPN) et des points d’accès sans fil, créer des demandes de connexion et les envoient aux serveurs RADIUS tels que NPS. Dans certains cas, un serveur NPS peut recevoir trop de demandes de connexion en même temps, ce qui entraîne une dégradation des performances ou une surcharge.

- [Vue d’ensemble de Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview): Cette rubrique fournit une vue d’ensemble d’Azure Traffic Manager, ce qui vous permet de contrôler la distribution du trafic utilisateur pour les points de terminaison de service. Traffic Manager utilise le système DNS (Domain Name) pour diriger les demandes des clients vers le point de terminaison approprié selon une méthode de routage du trafic et l’intégrité des points de terminaison. 

- [Windows Hello entreprise](https://docs.microsoft.com/windows/access-protection/hello-for-business/hello-identity-verification): Cette rubrique fournit les conditions préalables, telles que les déploiements cloud uniquement et les déploiements hybrides.  Cette rubrique répertorie également les questions fréquemment posées sur Windows Hello entreprise.

- [Étude de cas technique : L’activation de l’accès à distance avec Windows Hello for Business dans Windows 10](https://msdn.microsoft.com/library/mt728163.aspx): Dans cette étude de cas technique, vous découvrez comment Microsoft a implémenté l’accès à distance avec Windows Hello for Business.  Windows Hello entreprise est une clé publique/privée ou une approche d’authentification par certificat pour les organisations et les consommateurs qui vont au-delà des mots de passe. Cette forme d’authentification s’appuie sur les informations d’identification de la paire de clés qui peuvent remplacer des mots de passe et sont résistantes aux failles, aux vols et au phishing. 

- [Intégrer l’authentification RADIUS avec le serveur Azure multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-server-radius): Cette rubrique vous présente Ajout et configuration d’une authentification de client RADIUS avec le serveur Azure multi-Factor Authentication. RADIUS est un protocole standard qui permet d’accepter les demandes d’authentification et de traiter ces demandes. Le serveur Azure multi-Factor Authentication peut agir comme un serveur RADIUS. 

- [Fonctionnalités de sécurité VPN](https://docs.microsoft.com/windows/access-protection/vpn/vpn-security-features): Cette rubrique fournit des instructions de sécurité VPN de vous pour LockDown VPN, l’intégration de Protection des informations Windows (WIP) avec un VPN et les filtres de trafic. 

- [Options de VPN déclenché automatiquement le profil](https://docs.microsoft.com/windows/access-protection/vpn/vpn-auto-trigger-profile): Cette rubrique vous fournit des options de profil à déclenchement automatique VPN, comme déclencheur d’application, en fonction du nom de déclencheur et Always On.

- [VPN et l’accès conditionnel](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access): Cette rubrique fournit une vue d’ensemble de la plateforme d’accès conditionnel basé sur le cloud pour fournir une option de conformité d’appareil pour les clients distants. L’accès conditionnel est un moteur d’évaluation basé sur les stratégies qui vous permet de créer des règles d’accès pour n’importe quelle application connectée Azure Active Directory (Azure AD). 

- [Attestation de clé TPM](https://docs.microsoft.com/windows-server/identity/ad-ds/manage/component-updates/tpm-key-attestation): Cette rubrique fournit une vue d’ensemble du Module de plateforme sécurisée (TPM) et les étapes pour déployer l’attestation de clé TPM. Vous trouverez également des informations et les étapes pour résoudre les problèmes de dépannage.
