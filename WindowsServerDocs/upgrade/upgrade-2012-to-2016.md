---
title: Mise à niveau de Windows Server 2012 vers Windows Server 2016 | Microsoft Docs
description: Découvrez comment effectuer une mise à niveau sur place à partir de Windows Server 2012 vers Windows Server 2016.
ms.prod: windows server
ms.technology: server-general
ms.topic: upgrade
author: RobHindman
ms.author: robhind
ms.date: 09/16/2019
ms.openlocfilehash: 09c5a95e2ccd065f3ebbe551404064c39f803f2a
ms.sourcegitcommit: 27f0caf74e88781054250455c3c1adf06deb6234
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/19/2019
ms.locfileid: "71124719"
---
# <a name="upgrade-windows-server-2012-to-windows-server-2016"></a>Mise à niveau de Windows Server 2012 vers Windows Server 2016

Si vous souhaitez conserver le même matériel et tous les rôles de serveur que vous avez déjà configurés sans aplatir le serveur, vous devez effectuer une mise à niveau sur place. Une mise à niveau sur place vous permet de passer d’un système d’exploitation plus ancien à un système plus récent tout en conservant les paramètres, les rôles de serveur et les données intacts. Cet article vous aide à passer de Windows Server 2012 à Windows Server 2016.

Pour effectuer une mise à niveau vers Windows Server 2019, utilisez d’abord cette rubrique pour effectuer une mise à niveau vers Windows Server 2016, puis [effectuez une mise à niveau de Windows server 2016 vers Windows server 2019](upgrade-2016-to-2019.md).

## <a name="before-you-begin-your-in-place-upgrade"></a>Avant de commencer la mise à niveau sur place

Avant de commencer la mise à niveau de Windows Server, nous vous recommandons de collecter des informations à partir de vos appareils, à des fins de diagnostic et de dépannage. Étant donné que ces informations sont destinées à être utilisées uniquement en cas d’échec de la mise à niveau, vous devez vous assurer que vous stockez les informations dans un emplacement que vous pouvez extraire de votre appareil.

### <a name="to-collect-your-info"></a>Pour collecter vos informations

1. Ouvrez une invite de commandes, accédez `c:\Windows\system32`à, puis tapez **systeminfo. exe**.

2. Copiez, collez et stockez les informations système résultantes à partir de votre appareil.

3. Tapez **ipconfig/all** dans l’invite de commandes, puis copiez et collez les informations de configuration obtenues dans le même emplacement que ci-dessus.

4. Ouvrez l’éditeur du Registre, accédez à la ruche HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsNT\CurrentVersion, puis copiez et collez les **BuildLabEx** Windows Server (version) et **EditionID** (édition) dans le même emplacement que ci-dessus.

Une fois que vous avez collecté toutes les informations relatives à Windows Server, nous vous recommandons vivement de sauvegarder le système d’exploitation, les applications et les machines virtuelles. Vous devez également **arrêter**, **migrer rapidement**ou **migrer dynamiquement** les machines virtuelles en cours d’exécution sur le serveur. Vous ne pouvez pas avoir de machines virtuelles en cours d’exécution pendant la mise à niveau sur place.

## <a name="to-perform-the-upgrade"></a>Pour effectuer la mise à niveau

1. Assurez-vous que la valeur **BuildLabEx** indique que vous exécutez Windows Server 2012.

2. Localisez le support d’installation de Windows Server 2016, puis sélectionnez **Setup. exe**.

    ![Explorateur Windows avec le fichier Setup. exe](media/upgrade-2012-2016/setup-2016.png)

3. Sélectionnez **Oui** pour démarrer le processus d’installation.

    ![Contrôle de compte d’utilisateur qui demande l’autorisation de démarrer l’installation](media/upgrade-2012-2016/start-setup-uac-box.png)

4. Sur l’écran Windows Server 2016, sélectionnez **Installer maintenant**.

5. Pour les appareils connectés à Internet, sélectionnez **Télécharger et installer les mises à jour (recommandé)** .

    ![Pour obtenir des mises à jour Windows importantes](media/upgrade-2012-2016/imp-updates-win-setup.png)

6. Le programme d’installation vérifie la configuration de votre appareil, vous devez attendre qu’il se termine, puis sélectionner **suivant**.

7. Selon le canal de distribution sur lequel vous avez reçu le support Windows Server (version commerciale, licence en volume, OEM, ODM, etc.) et la licence du serveur, vous pouvez être invité à entrer une clé de licence pour continuer.

    ![Écran dans lequel vous pouvez entrer votre clé de produit](media/upgrade-2012-2016/enter-product-key.png)

8. Sélectionnez l’édition Windows Server 2016 que vous souhaitez installer, puis sélectionnez **suivant**.

    ![Écran de sélection de l’édition Windows Server 2016 à installer](media/upgrade-2012-2016/select-os-edition.png)

9. Sélectionnez **accepter** pour accepter les termes de votre contrat de licence, en fonction de votre canal de distribution (par exemple, vente au détail, licence en volume, OEM, ODM, etc.).

    ![Écran pour accepter votre contrat de licence](media/upgrade-2012-2016/license-terms.png)

10. Sélectionnez **conserver les fichiers et les applications personnels** pour choisir de procéder à une mise à niveau sur place, puis sélectionnez **suivant**.

    ![Écran pour choisir votre type d’installation](media/upgrade-2012-2016/choose-install-upgrade.png)

11. Si vous voyez une page indiquant que la mise à niveau n’est pas recommandée, vous pouvez l’ignorer et sélectionner **confirmer**. Elle a été mise en place pour demander une nouvelle installation, mais cela n’est pas nécessaire.

    ![Écran présentant le bouton confirmer pour s’assurer que vous voulez effectuer la mise à niveau](media/upgrade-2012-2016/confirm-upgrade-process.png)

12. Le programme d’installation vous demande de supprimer Microsoft Endpoint Protection à l’aide de **Ajout/suppression de programmes**.

    Cette fonctionnalité n’est pas compatible avec Windows 10.

13. Une fois que le programme d’installation a analysé votre appareil, il vous invite à procéder à la mise à niveau en sélectionnant **installer**.

    ![Écran vous permettant de commencer la mise à niveau](media/upgrade-2012-2016/ready-to-install.png)

    La mise à niveau sur place démarre et vous montre l’écran de **mise à niveau de Windows** avec sa progression. Une fois la mise à niveau terminée, votre serveur redémarre.

    ![Écran présentant la progression de la mise à niveau](media/upgrade-2012-2016/upgrading-windows-with-progress.png)

## <a name="after-your-upgrade-is-done"></a>Une fois la mise à niveau terminée

Une fois la mise à niveau terminée, vous devez vous assurer que la mise à niveau vers Windows Server 2016 a réussi.

### <a name="to-make-sure-your-upgrade-was-successful"></a>Pour vous assurer que la mise à niveau a réussi

1. Ouvrez l’éditeur du Registre, accédez à la ruche HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsNT\CurrentVersion et affichez le **ProductName**. Vous devez voir votre édition de Windows Server 2016, par exemple **Windows server 2016 Datacenter**.

2. Vérifiez que toutes vos applications sont en cours d’exécution et que vos connexions client aux applications sont réussies.

Si vous pensez qu’une erreur s’est produite lors de la mise à niveau, `%SystemRoot%\Panther` Copiez `C:\Windows\Panther`et compressez le répertoire (généralement) et contactez le support Microsoft.

## <a name="next-steps"></a>Étapes suivantes

Vous pouvez effectuer une autre mise à niveau pour passer de Windows Server 2016 à Windows Server 2019. Pour obtenir des instructions détaillées, consultez [mise à niveau de Windows server 2016 vers Windows server 2019](upgrade-2016-to-2019.md).

## <a name="related-articles"></a>Articles connexes

- Pour plus de détails et d’informations sur Windows Server 2016, consultez [prise en main de Windows server 2016](https://docs.microsoft.com/windows-server/get-started/server-basics).