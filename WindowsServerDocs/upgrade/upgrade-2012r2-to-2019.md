---
title: Mettre à niveau Windows Server 2012 R2 vers Windows Server 2019 | Microsoft Docs
description: Découvrez comment effectuer une mise à niveau sur place pour passer de Windows Server 2012 R2 à Windows Server 2019.
ms.prod: windows-server
ms.technology: server-general
ms.topic: upgrade
author: RobHindman
ms.author: robhind
ms.date: 09/16/2019
ms.openlocfilehash: 02d6dd21b798346245f209174902b6f4fdf24ff8
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853342"
---
# <a name="upgrade-windows-server-2012-r2-to-windows-server-2019"></a>Mettre à niveau Windows Server 2012 R2 vers Windows Server 2019

Si vous voulez conserver le même matériel et tous les rôles serveur que vous avez déjà configurés sans remettre à plat le serveur, vous allez effectuer une mise à niveau sur place. Une mise à niveau sur place vous permet de passer d’un ancien système d’exploitation à un plus récent, tout en conservant inchangés vos paramètres, vos rôles serveur et vos données. Cet article vous aide à passer de Windows Server 2012 R2 à Windows Server 2019.

## <a name="before-you-begin-your-in-place-upgrade"></a>Avant de commencer votre mise à niveau sur place

Avant de commencer votre mise à niveau de Windows Server, nous vous recommandons de collecter quelques informations auprès de vos appareils, à des fins de diagnostic et de résolution des problèmes. Comme ces informations sont destinées à être utilisées seulement en cas d’échec de la mise à niveau, veillez à stocker les informations à un autre endroit que sur votre appareil pour pouvoir les récupérer.

### <a name="to-collect-your-info"></a>Pour collecter vos informations

1. Ouvrez une invite de commandes, accédez à `c:\Windows\system32`, puis tapez **systeminfo.exe**.

2. Copiez, collez et stockez les informations système résultantes ailleurs que sur votre appareil.

3. Tapez **ipconfig /all** à l’invite de commandes, puis copiez et collez les informations de configuration obtenues au même emplacement que ci-dessus.

4. Ouvrez l’Éditeur du Registre, accédez à la ruche HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsNT\CurrentVersion, puis copiez et collez les informations **BuildLabEx** (version) et **EditionID** (édition) de Windows Server au même emplacement que ci-dessus.

Une fois que vous avez collecté toutes les informations relatives à Windows Server, nous vous recommandons vivement de sauvegarder votre système d’exploitation, vos applications et vos machines virtuelles. Vous devez aussi effectuer les opérations **Arrêter**, **Effectuer une migration rapide** ou **Migrer dynamiquement** pour les machines virtuelles en cours d’exécution sur le serveur. Vous ne pouvez pas avoir de machines virtuelles en cours d’exécution pendant la mise à niveau sur place.

## <a name="to-perform-the-upgrade"></a>Pour effectuer la mise à niveau

1. Vérifiez que la valeur de **BuildLabEx** indique que vous exécutez Windows Server 2012 R2.

2. Recherchez le support d’installation de Windows Server 2019, puis sélectionnez **setup.exe**.

    ![Explorateur Windows montrant le fichier setup.exe](media/upgrade-2012r2-2019/setup-2019.png)

3. Sélectionnez **Oui** pour démarrer le processus d’installation.

    ![Contrôle de compte d’utilisateur demandant l’autorisation de démarrer l’installation](media/upgrade-2012r2-2019/start-setup-uac-box.png)

4. Pour les appareils connectés à Internet, sélectionnez l’option **Télécharger les mises à jour, les pilotes et les fonctionnalités facultatives (recommandé)** , puis sélectionnez **Suivant**.

    ![Écran permettant de choisir de se connecter pour obtenir des mises à jour importantes de Windows](media/upgrade-2012r2-2019/online-updates-win-setup.png)

5. Le programme d’installation vérifie la configuration de votre appareil : attendez qu’il termine, puis sélectionnez **Suivant**.

6. Selon le canal de distribution dont vous avez reçu le support de Windows Server (Vente, Licence en volume, OEM, ODM, etc.) et la licence du serveur, vous pouvez être invité à entrer une clé de licence pour continuer.

7. Sélectionnez l’édition de Windows Server 2019 que vous voulez installer, puis sélectionnez **Suivant**.

    ![Écran permettant de choisir l’édition de Windows Server 2012 R2 à installer](media/upgrade-2012r2-2019/select-os-edition.png)

8. Sélectionnez **Accepter** pour accepter les termes de votre contrat de licence, en fonction de votre canal de distribution (comme Vente, Licence en volume, OEM, ODM, etc.).

    ![Écran permettant d’accepter votre contrat de licence](media/upgrade-2012r2-2019/license-terms.png)

9. Le programme d’installation vous recommande de supprimer Microsoft Endpoint Protection en utilisant **Ajout/Suppression de programmes**.

    Cette fonctionnalité n’est pas compatible avec Windows Server 2019.

10. Sélectionnez **Conserver les fichiers personnels et applications** pour choisir d’effectuer une mise à niveau sur place, puis sélectionnez **Suivant**.

    ![Écran permettant de choisir votre type d’installation](media/upgrade-2012r2-2019/choose-install-upgrade.png)

11. Une fois que le programme d’installation a analysé votre appareil, il vous invite à procéder à la mise à niveau en sélectionnant **Installer**.

    ![Écran montrant que vous êtes prêt à démarrer la mise à niveau](media/upgrade-2012r2-2019/ready-to-install.png)

    La mise à niveau sur place démarre, vous montrant l’écran **Mise à niveau de Windows** avec sa progression. Une fois la mise à niveau terminée, votre serveur redémarre.

    ![Écran montrant la progression de la mise à niveau](media/upgrade-2012r2-2019/upgrading-windows-with-progress.png)

## <a name="after-your-upgrade-is-done"></a>Une fois la mise à niveau terminée

Une fois la mise à niveau terminée, vous devez vérifier que la mise à niveau vers Windows Server 2019 a réussi.

### <a name="to-make-sure-your-upgrade-was-successful"></a>Pour vérifier que votre mise à niveau a réussi

1. Ouvrez l’Éditeur du Registre, accédez à la ruche HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsNT\CurrentVersion et regardez **ProductName**. Vous devez voir votre édition de Windows Server 2019, par exemple **Windows Server 2019 Datacenter**.

2. Vérifiez que toutes vos applications sont en cours d’exécution et que les connexions de vos clients aux applications sont réussies.

Si vous pensez qu’une erreur s’est produite lors de la mise à niveau, copiez et compressez le répertoire `%SystemRoot%\Panther` (généralement `C:\Windows\Panther`), puis contactez le support technique Microsoft.

## <a name="related-articles"></a>Articles connexes

- Pour plus de détails et d’informations sur Windows Server 2019, consultez [Bien démarrer avec Windows Server 2019](https://docs.microsoft.com/windows-server/get-started-19/get-started-19).