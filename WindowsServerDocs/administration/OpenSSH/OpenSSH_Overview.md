---
ms.date: 01/07/2019
contributor: damaerteMSFT
author: maertendMSFT
keywords: OpenSSH, SSH, Win32-OpenSSH
title: OpenSSH dans Windows
ms.product: w10
ms.openlocfilehash: c6563fbe4fe69acad4d295a3f7fe166e92d38444
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280049"
---
# <a name="openssh-in-windows"></a>OpenSSH dans Windows

OpenSSH est la version open source des outils SSH (Secure Shell) utilisés par les administrateurs de Linux et d’autres non Windows-pour une gestion multiplateforme de systèmes distants. OpenSSH a été ajouté à Windows à compter de l’automne 2018 et est inclus dans Windows 10 et Windows Server 2019. 

SSH est basé sur une architecture client-serveur dans lequel le système que fonctionne sur l’utilisateur est le client et le système distant géré est le serveur. OpenSSH inclut une gamme de composants et outils destinés à fournir une approche sécurisée et simple à l’administration du système distant, y compris :

* sshd.exe, qui est le composant de serveur SSH doit être en cours d’exécution sur le système géré à distance 
* SSH.exe, qui est le composant de client SSH qui s’exécute sur le système local de l’utilisateur
* SSH keygen.exe génère, gère et convertit les clés d’authentification pour le protocole SSH 
* stocke les clés privées utilisées pour l’authentification par clé publique SSH agent.exe
* SSH add.exe ajoute des clés privées à la liste autorisée par le serveur
* SSH keyscan.exe contribue à la collecte les clés d’hôte SSH publiques à partir d’un nombre d’hôtes
* SFTP.exe est le service qui fournit la Secure File Transfer Protocol et s’exécute via SSH
* SCP.exe est un utilitaire de copie de fichier qui s’exécute sur SSH

Dans cette section se concentre sur l’utilisation d’OpenSSH sur Windows, y compris l’installation et les cas de configuration et l’utilisation de Windows spécifique. Voici les rubriques :
* Installation et désinstallation OpenSSH pour Windows Server 2019 et Windows 10

Une documentation détaillée supplémentaire pour les fonctionnalités communes de OpenSSH est disponible en ligne [OpenSSH.com](https://www.openssh.com/manual.html). 

Le maître [OpenSSH projet open source](https://www.openssh.com) est géré par les développeurs au projet OpenBSD. La duplication de Microsoft de ce projet est dans [GitHub](https://github.com/PowerShell/openssh-portable).
Commentaires sur Windows OpenSSH sont bienvenues et peut être fourni en créant des problèmes GitHub dans notre [référentiel GitHub d’OpenSSH](https://github.com/PowerShell/openssh-portable). 
