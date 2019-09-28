---
title: Déploiement du VPN Toujours actif (AlwaysOn) pour Windows Server et Windows 10
description: Vous pouvez utiliser ce déploiement pour déployer Always On connexions de réseau privé virtuel (VPN) pour les employés distants à l’aide de l’accès à distance dans Windows Server 2016 ou version ultérieure et Always On profils VPN pour les ordinateurs clients Windows 10.
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: 5ae1a40b-4f10-4ace-8aaf-13f7ab581f4f
ms.localizationpriority: medium
ms.date: 12/20/2018
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5eba89cf61354627b63bcdf2420c25e7a44e3d9a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388143"
---
# <a name="always-on-vpn-deployment-for-windows-server-and-windows-10"></a>Always On le déploiement VPN pour Windows Server et Windows 10

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Premier** Accès à distance @ no__t-0<br>
- [**Situé** En savoir plus sur les fonctionnalités et fonctionnalités VPN de Always On @ no__t-0

Always On VPN offre une solution unique et cohésive pour l’accès à distance et prend en charge les appareils joints à un domaine, non joints à un domaine (groupe de travail) ou Azure AD, même les appareils personnels. Avec VPN Toujours actif (AlwaysOn), le type de connexion ne doit pas nécessairement être exclusivement utilisateur ou appareil, mais peut être une combinaison des deux. Par exemple, vous pouvez activer l'authentification de l’appareil pour la gestion d’appareils à distance, puis activer l'authentification utilisateur pour la connectivité aux sites et services internes de la société.

## <a name="prerequisites"></a>Prérequis

Vous disposez probablement des technologies déployées que vous pouvez utiliser pour déployer Always On VPN. À l’exception de vos serveurs DC/DNS, le déploiement VPN Always On nécessite un serveur NPS (RADIUS), un serveur d’autorité de certification (CA) et un serveur d’accès à distance (routage/VPN). Une fois l’infrastructure configurée, vous devez inscrire les clients, puis connecter les clients à votre site local en toute sécurité par le biais de plusieurs modifications du réseau.

- Active Directory l’infrastructure de domaine, y compris un ou plusieurs serveurs DNS (Domain Name System). Les zones DNS (Domain Name System) internes et externes sont requises, ce qui suppose que la zone interne est un sous-domaine délégué de la zone externe (par exemple, corp.contoso.com et contoso.com).
- Infrastructure à clé publique (PKI) Active Directory et les services de certificats Active Directory (AD CS).
- Serveur (virtuel ou physique, existant ou nouveau) pour installer le serveur NPS (Network Policy Server). Si vous avez déjà des serveurs NPS sur votre réseau, vous pouvez modifier une configuration de serveur NPS existante au lieu d’ajouter un nouveau serveur.
- Accès à distance en tant que serveur VPN de passerelle RAS avec un petit sous-ensemble de fonctionnalités prenant en charge les connexions VPN IKEv2 et le routage LAN.
- Réseau de périmètre qui comprend deux pare-feu.  Assurez-vous que vos pare-feu autorisent le trafic nécessaire pour que les communications VPN et RADIUS fonctionnent correctement. Pour plus d’informations, consultez [Always on vue d’ensemble de la technologie VPN](../always-on-vpn-technology-overview.md).
- Serveur physique ou machine virtuelle (VM) sur votre réseau de périmètre avec deux cartes réseau Ethernet physiques pour installer l’accès à distance en tant que serveur VPN de passerelle RAS. Les machines virtuelles nécessitent un réseau local virtuel (VLAN) pour l’ordinateur hôte. 
- L’appartenance au groupe administrateurs, ou équivalent, est la condition minimale requise.
- Lisez la section relative à la planification de ce guide pour vous assurer que vous êtes prêt pour ce déploiement avant d’effectuer le déploiement.
- Passez en revue les guides de conception et de déploiement pour chacune des technologies utilisées. Ces guides peuvent vous aider à déterminer si les scénarios de déploiement fournissent les services et la configuration dont vous avez besoin pour le réseau de votre organisation. Pour plus d’informations, consultez [Always on vue d’ensemble de la technologie VPN](../always-on-vpn-technology-overview.md).
- Plateforme de gestion de votre choix pour le déploiement de la configuration VPN Always On, car le CSP n’est pas spécifique au fournisseur.

>[!IMPORTANT]
>Pour ce déploiement, il n’est pas nécessaire que vos serveurs d’infrastructure, tels que les ordinateurs qui exécutent Active Directory Domain Services, Active Directory les services de certificats et le serveur de stratégie réseau, exécutent Windows Server 2016. Vous pouvez utiliser des versions antérieures de Windows Server, telles que Windows Server 2012 R2, pour les serveurs d’infrastructure et pour le serveur qui exécute l’accès à distance.
>
>N’essayez pas de déployer l’accès à distance sur une machine virtuelle (VM) dans Microsoft Azure. L’utilisation de l’accès à distance dans Microsoft Azure n’est pas prise en charge, y compris le VPN d’accès à distance et DirectAccess. Pour plus d’informations, consultez [prise en charge des logiciels serveur Microsoft pour les machines virtuelles Microsoft Azure](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines).

## <a name="about-this-deployment"></a>À propos de ce déploiement

Les instructions fournies vous guident tout au long du déploiement de l’accès à distance en tant que passerelle RAS VPN à client unique pour les connexions VPN de point à site, à l’aide de l’un des scénarios mentionnés ci-dessous, pour les ordinateurs clients distants qui exécutent Windows 10. Vous trouverez également des instructions pour la modification de l’infrastructure existante pour le déploiement. En outre, tout au long de ce déploiement, vous trouverez des liens qui vous aideront à en savoir plus sur le processus de connexion VPN, les serveurs à configurer, le nœud CSP ProfileXML VPNv2 et d’autres technologies pour déployer Always On VPN.

**Scénarios de déploiement Always On VPN :**

1. Déployez Always On VPN uniquement.
2. Déployez Always On VPN avec un accès conditionnel pour la connectivité VPN à l’aide de Azure AD.

Pour plus d’informations et le flux de travail des scénarios présentés, consultez [déployer Always on VPN](always-on-vpn-deploy-deployment.md).

## <a name="what-isnt-provided-in-this-deployment"></a>Ce qui n’est pas fourni dans ce déploiement

Ce déploiement ne fournit pas d’instructions pour :

- Active Directory Domain Services (AD DS).
- Les services de certificats Active Directory (AD CS) et une infrastructure à clé publique (PKI).
- Protocole DHCP (Dynamic Host Configuration Protocol).
- Matériel réseau, tel que le câblage Ethernet, les pare-feu, les commutateurs et les concentrateurs.
- Ressources réseau supplémentaires, telles que les serveurs d’applications et de fichiers, auxquelles les utilisateurs distants peuvent accéder via une connexion VPN Always On.
- Connectivité Internet ou accès conditionnel pour la connectivité Internet à l’aide de Azure AD. Pour plus d’informations, consultez [accès conditionnel dans Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal).

## <a name="next-steps"></a>Étapes suivantes

- [En savoir plus sur les fonctionnalités et fonctionnalités VPN Always On](../../vpn-map-da.md)

- [En savoir plus sur les améliorations apportées au VPN Always On](../always-on-vpn-enhancements.md)

- [En savoir plus sur les fonctionnalités avancées Always On VPN](always-on-vpn-adv-options.md)

- [En savoir plus sur la technologie VPN Always On](../always-on-vpn-technology-overview.md)

- [Démarrer la planification de votre déploiement Always On VPN](always-on-vpn-deploy-deployment.md)