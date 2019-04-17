---
title: Déploiement du VPN Toujours actif (AlwaysOn) pour WindowsServer et Windows10
description: Vous pouvez utiliser ce déploiement pour déployer des connexions toujours sur réseau privé virtuel (VPN) pour les employés à distance à l’aide d’un accès à distance dans Windows Server 2016 ou version ultérieure et profils toujours actif (AlwaysOn) pour les ordinateurs clients de Windows 10.
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: 5ae1a40b-4f10-4ace-8aaf-13f7ab581f4f
ms.localizationpriority: medium
ms.date: 12/20/2018
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7cb60bdc6d6f3ff074f04827aa95c9e8e8abf35b
ms.sourcegitcommit: c435f91ef6f3ff5ffd2661291264b939d5ce4e2a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/21/2018
ms.locfileid: "8981129"
---
# Déploiement de toujours actif (AlwaysOn) pour Windows Server et Windows 10

>S’applique à: Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows 10

& #171;  [ **Précédente:** accès à distance](../../../Remote-Access.md)<br>
& #187; [ **Suivant:** obtenir des informations sur les fonctionnalités toujours actif (AlwaysOn)](../../vpn-map-da.md)


Toujours actif (AlwaysOn) fournit une solution unique et cohérente pour l’accès à distance et prend en charge joints au domaine, joints à un domaine (groupe de travail) ou Azure AD appareils joints, même personnels n’appareils.  Toujours actif, le type de connexion ne doit pas être exclusivement utilisateur ou du périphérique, mais peut être une combinaison des deux. Par exemple, vous pourriez activer l’authentification d’appareil pour la gestion des périphériques à distance et activez ensuite l’authentification utilisateur pour la connectivité aux sites internes de l’entreprise et services.



## Conditions préalables

Vous avez probablement accès les technologies de déploiement que vous pouvez utiliser pour déployer toujours actif (AlwaysOn). Autres que vos serveurs DNS/contrôleur de domaine, le déploiement toujours actif (AlwaysOn) requiert un serveur NPS (rayon), un serveur d’autorité de Certification (CA) et un serveur d’accès à distance (routage/VPN). Une fois que l’infrastructure est configuré, vous devez inscrire des clients et puis connecter les clients à vos locaux en toute sécurité par le biais de plusieurs modifications de réseau.

- Infrastructure Active Directory domaine, y compris un ou plusieurs serveurs de nom de domaine (DNS). Zones de nom de domaine (DNS) internes et externes ne sont requises, ce qui suppose que la zone interne est un sous-domaine délégué de la zone externe (par exemple, corp.contoso.com et contoso.com).
- Active Directory basé infrastructure à clé publique (PKI) et les Services de certificats Active Directory (AD CS).
- Serveur, virtuelle ou physique, existante ou nouvelle, pour installer le serveur NPS (Network Policy Server). Si vous avez déjà des serveurs NPS sur votre réseau, vous pouvez modifier une configuration de serveur NPS existante plutôt qu’ajouter un nouveau serveur.
- Accès à distance un serveur RAS passerelle VPN avec un petit sous-ensemble de fonctionnalités prenant en charge les connexions VPN IKEv2 et le routage de réseau local.
- Réseau de périmètre qui inclut les deux pare-feu.  Assurez-vous que votre pare-feu autorise le trafic qui est nécessaire pour les communications VPN et rayon de fonctionner correctement. Pour plus d’informations, voir [Toujours sur VPN présentation de la technologie](../always-on-vpn-technology-overview.md).
- Serveur physique ou machine virtuelle (VM) sur votre réseau de périmètre avec deux cartes réseau Ethernet physiques pour installer l’accès à distance en tant qu’un serveur RAS passerelle VPN. Machines virtuelles nécessitent un réseau local virtuel (VLAN) pour l’hôte. 
- L’appartenance à des administrateurs, ou l’équivalent, est au minimum requis.
- Lisez la section Planification de ce guide pour vous assurer que vous êtes prêt pour ce déploiement avant d’effectuer le déploiement.
- Passez en revue les guides de conception et de déploiement pour chacune des technologies utilisées. Ces guides peuvent vous aider à déterminer si les scénarios de déploiement fournissent des services et une configuration dont vous avez besoin pour le réseau de votre organisation. Pour plus d’informations, voir [Toujours sur VPN présentation de la technologie](../always-on-vpn-technology-overview.md).
- Plateforme de gestion de votre choix pour le déploiement de la configuration toujours actif (AlwaysOn), car le fournisseur CSP n’est pas spécifique au fournisseur.


>[!IMPORTANT]
>Pour ce déploiement, il n’est pas obligatoire que vos serveurs d’infrastructure, tels que les ordinateurs exécutant les Services de domaine Active Directory, Services de certificats Active Directory et serveur NPS, exécutent Windows Server 2016. Vous pouvez utiliser les versions antérieures de Windows Server, par exemple, Windows Server 2012 R2, pour les serveurs d’infrastructure et le serveur qui exécute l’accès à distance.
>
>N’essayez pas de déployer l’accès à distance sur une machine virtuelle \(VM\) dans Microsoft Azure. À l’aide de l’accès à distance dans Microsoft Azure n'est pas pris en charge, y compris les accès à distance VPN et DirectAccess. Pour plus d’informations, voir [le logiciel serveur Microsoft prend en charge pour les machines virtuelles Microsoft Azure](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines).


## <a name="bkmk_about"></a>À propos de ce déploiement

Les instructions fournies vous guident dans le processus de déploiement d’accès à distance en tant qu’un seul locataire VPN RAS passerelle pour les connexions VPN point\-à\-site, à l’aide d’un des scénarios mentionnés ci-après, pour les ordinateurs clients qui exécutent Windows 10. Vous trouverez également des instructions permettant de modifier certaines de votre infrastructure existante pour le déploiement. Tout au long de ce déploiement, vous trouverez des liens pour vous aider à en savoir plus sur le processus de connexion VPN, les serveurs à configurer, nœud ProfileXML VPNv2 CSP et autres technologies de déploiement toujours actif (AlwaysOn).

**Scénarios de déploiement toujours actif (AlwaysOn):**

1. Déployez toujours sur VPN uniquement.
2. Déployez toujours actif (AlwaysOn) l’accès conditionnel pour une connexion VPN à l’aide d’Azure AD.


Pour plus d’informations et des flux de travail des scénarios présentés, voir [Déployer toujours actif (AlwaysOn)](always-on-vpn-deploy-deployment.md).


## <a name="bkmk_not"></a>Ce qui n’est pas fourni dans ce déploiement

Ce déploiement ne fournit pas d’instructions pour:

- \(AD DS\) des Services de domaine Active Directory.
- \(AD CS\) et une Infrastructure à clé publique de Services de certificats Active Directory \(PKI\).
- Dynamic Host Configuration Protocol \(DHCP\). 
- Le matériel, par exemple, les câbles Ethernet, pare-feu, commutateurs et concentrateurs du réseau.
- Ressources de réseau supplémentaires, telles que les serveurs de fichiers et d’application, que les utilisateurs distants peuvent accéder via une connexion toujours actif (AlwaysOn).
- Connectivité Internet ou l’accès conditionnel pour la connectivité Internet à l’aide d’Azure AD. Pour plus d’informations, voir [conditionnel accès à Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal).




## Étapes suivantes

- [En savoir plus sur les fonctionnalités toujours actif (AlwaysOn)](../../vpn-map-da.md)

- [En savoir plus sur les améliorations toujours actif (AlwaysOn)](../always-on-vpn-enhancements.md)

- [En savoir plus sur certaines des fonctionnalités avancées toujours actif (AlwaysOn)](always-on-vpn-adv-options.md)

- [En savoir plus sur la technologie VPN Toujours actif (AlwaysOn)](../always-on-vpn-technology-overview.md)

- [Commencer à planifier votre déploiement de VPN Toujours actif (AlwaysOn)](always-on-vpn-deploy-deployment.md)


---
