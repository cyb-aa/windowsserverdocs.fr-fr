---
ms.date: 01/07/2019
contributor: damaerteMSFT
author: maertendmsft
title: OpenSSH dans Windows
ms.product: windows-server
ms.openlocfilehash: 57126f38245f4547a04ea3a51f58aee865b5eb97
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2020
ms.locfileid: "80852032"
---
# <a name="openssh-in-windows"></a>OpenSSH dans Windows

OpenSSH est la version open source des outils Secure Shell (SSH) utilisée par les administrateurs de Linux et d’autres systèmes hors Windows pour la gestion inter-plateforme de systèmes distants. OpenSSH a été ajouté à Windows à l’automne 2018 et est inclus dans Windows 10 et Windows Server 2019. 

SSH est basé sur une architecture client-serveur dans laquelle le système sur lequel travaille l’utilisateur est le client et le système distant géré est le serveur. OpenSSH comporte différents composants et outils qui offrent une approche sécurisée et directe de l’administration de système à distance, notamment :

* sshd.exe, le composant de serveur SSH qui doit être exécuté sur le système géré à distance 
* ssh.exe, le composant de client SSH qui est exécuté sur le système local de l’utilisateur
* ssh-keygen.exe génère, gère et convertit les clés d’authentification pour SSH 
* ssh-agent.exe stocke les clés privées utilisées pour l’authentification des clés publiques
* ssh-add.exe ajoute des clés privées à la liste autorisée par le serveur
* ssh-keyscan.exe aide à collecter les clés d’hôte SSH publiques à partir de différents ordinateurs hôtes
* sftp.exe est le service qui fournit le protocole Secure File Transfer et est exécuté sur SSH
* scp.exe est un utilitaire de copie de fichiers exécuté sur SSH

La documentation présentée dans cette section traite de l’utilisation de OpenSSH sur Windows, notamment l’installation, et de la configuration et des cas d’utilisation spécifiques à Windows. Voici les rubriques :
* Installation et désinstallation de OpenSSH pour Windows Server 2019 et Windows 10

Une documentation détaillée supplémentaire relative aux fonctionnalités OpenSSH courantes est disponible en ligne sur [OpenSSH.com](https://www.openssh.com/manual.html). 

Le [projet open source OpenSSH](https://www.openssh.com) est géré par les développeurs dans le projet OpenBSD. La fourche Microsoft de ce projet se trouve dans [GitHub](https://github.com/PowerShell/openssh-portable).
Les commentaires sur Windows OpenSSH sont les bienvenus. Ils peuvent être transmis en créant un ticket GitHub dans notre [Référentiel GitHub OpenSSH](https://github.com/PowerShell/openssh-portable). 
