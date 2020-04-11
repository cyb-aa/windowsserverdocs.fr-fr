---
title: Mettre à niveau Windows Server 2008 R2 vers Windows Server 2012 R2 | Microsoft Docs
description: Découvrez comment effectuer une mise à niveau sur place pour passer de Windows Server 2008 R2 à Windows Server 2012 R2.
ms.prod: windows-server
ms.technology: server-general
ms.topic: upgrade
author: RobHindman
ms.author: robhind
ms.date: 09/16/2019
ms.openlocfilehash: 5e4436bb6e4db19e015056b67730619a93396f9e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854262"
---
# <a name="upgrade-windows-server-2008-r2-to-windows-server-2012-r2"></a>Mettre à niveau Windows Server 2008 R2 vers Windows Server 2012 R2

Si vous voulez conserver le même matériel et tous les rôles serveur que vous avez déjà configurés sans remettre à plat le serveur, vous allez effectuer une mise à niveau sur place. Une mise à niveau sur place vous permet de passer d’un ancien système d’exploitation à un plus récent, tout en conservant inchangés vos paramètres, vos rôles serveur et vos données. Cet article vous aide à passer de Windows Server 2008 R2 à Windows Server 2012 R2.

Pour effectuer une mise à niveau vers Windows Server 2019, utilisez cette rubrique pour d’abord effectuer une mise à niveau vers Windows Server 2012 R2, puis [effectuez la mise à niveau de Windows Server 2012 R2 vers Windows Server 2019](upgrade-2012r2-to-2019.md).

## <a name="before-you-begin-your-in-place-upgrade"></a>Avant de commencer votre mise à niveau sur place

Avant de commencer votre mise à niveau de Windows Server, nous vous recommandons de collecter quelques informations auprès de vos appareils, à des fins de diagnostic et de résolution des problèmes. Comme ces informations sont destinées à être utilisées seulement en cas d’échec de la mise à niveau, veillez à stocker les informations à un autre endroit que sur votre appareil pour pouvoir les récupérer.

### <a name="to-collect-your-info"></a>Pour collecter vos informations

1. Ouvrez une invite de commandes, accédez à `c:\Windows\system32`, puis tapez **systeminfo.exe**.

2. Copiez, collez et stockez les informations système résultantes ailleurs que sur votre appareil.

3. Tapez **ipconfig /all** à l’invite de commandes, puis copiez et collez les informations de configuration obtenues au même emplacement que ci-dessus.

4. Ouvrez l’Éditeur du Registre, accédez à la ruche HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsNT\CurrentVersion, puis copiez et collez les informations **BuildLabEx** (version) et **EditionID** (édition) de Windows Server au même emplacement que ci-dessus.

Une fois que vous avez collecté toutes les informations relatives à Windows Server, nous vous recommandons vivement de sauvegarder votre système d’exploitation, vos applications et vos machines virtuelles. Vous devez aussi effectuer les opérations **Arrêter**, **Effectuer une migration rapide** ou **Migrer dynamiquement** pour les machines virtuelles en cours d’exécution sur le serveur. Vous ne pouvez pas avoir de machines virtuelles en cours d’exécution pendant la mise à niveau sur place.

## <a name="to-perform-the-upgrade"></a>Pour effectuer la mise à niveau

1. Vérifiez que la valeur de **BuildLabEx** indique que vous exécutez Windows Server 2008 R2.

2. Recherchez le support d’installation de Windows Server 2012 R2, puis sélectionnez **setup.exe**.

    ![Explorateur Windows montrant le fichier setup.exe](media/upgrade-2008r2-2012r2/setup-2012r2.png)

3. Sélectionnez **Oui** pour démarrer le processus d’installation.

    ![Contrôle de compte d’utilisateur demandant l’autorisation de démarrer l’installation](media/upgrade-2008r2-2012r2/start-setup-uac-box.png)

4. Dans l’écran de Windows Server 2012 R2, sélectionnez **Installer maintenant**.

5. Pour les appareils connectés à Internet, sélectionnez **Se connecter pour installer les mises à jour maintenant (recommandé)** .

    ![Écran permettant de choisir de se connecter pour obtenir des mises à jour importantes de Windows](media/upgrade-2008r2-2012r2/imp-updates-win-setup.png)

6. Sélectionnez l’édition de Windows Server 2012 R2 que vous voulez installer, puis sélectionnez **Suivant**.

    ![Écran permettant de choisir l’édition de Windows Server 2012 R2 à installer](media/upgrade-2008r2-2012r2/select-os-edition.png)

7. Sélectionnez **J’accepte les termes du contrat de licence** pour accepter les termes de votre contrat de licence, en fonction de votre canal de distribution (par exemple, Vente, Licence en volume, OEM, ODM, etc.), puis sélectionnez **Suivant**.

    ![Écran permettant d’accepter votre contrat de licence](media/upgrade-2008r2-2012r2/license-terms.png)

8. Sélectionnez **Mise à jour : installer Windows et conserver les fichiers, les paramètres et les applications** pour choisir d’effectuer une mise à niveau sur place.

    ![Écran permettant de choisir votre type d’installation](media/upgrade-2008r2-2012r2/choose-install-upgrade.png)

9. Le programme d’installation vous rappelle de vérifier que vos applications sont compatibles avec Windows Server 2012 R2, en utilisant les informations contenues dans l’article [Installation et mise à niveau de Windows Server](https://docs.microsoft.com/windows-server/get-started/installation-and-upgrade), puis sélectionnez **Suivant**.

    ![Écran vous rappelant de vérifier la compatibilité de vos applications](media/upgrade-2008r2-2012r2/compatibility-report.png)

10. Si vous voyez une page indiquant que la mise à niveau n’est pas recommandée, vous pouvez l’ignorer et sélectionner **Confirmer**. Elle a été mise en place pour demander une nouvelle installation, mais ce n’est pas nécessaire.

    ![Écran montrant la progression de la mise à niveau](media/upgrade-2008r2-2012r2/upgrading-windows-with-progress.png)

    La mise à niveau sur place démarre, vous montrant l’écran **Mise à niveau de Windows** avec sa progression. Une fois la mise à niveau terminée, votre serveur redémarre.

## <a name="after-your-upgrade-is-done"></a>Une fois la mise à niveau terminée

Une fois la mise à niveau terminée, vous devez vérifier que la mise à niveau vers Windows Server 2012 R2 a réussi.

### <a name="to-make-sure-your-upgrade-was-successful"></a>Pour vérifier que votre mise à niveau a réussi

1. Ouvrez l’Éditeur du Registre, accédez à la ruche HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsNT\CurrentVersion et regardez **ProductName**. Vous devez voir votre édition de Windows Server 2012 R2, par exemple **Windows Server 2012 R2 Datacenter**.

2. Vérifiez que toutes vos applications sont en cours d’exécution et que les connexions de vos clients aux applications sont réussies.

Si vous pensez qu’une erreur s’est produite lors de la mise à niveau, copiez et compressez le répertoire `%SystemRoot%\Panther` (généralement `C:\Windows\Panther`), puis contactez le support technique Microsoft.

## <a name="next-steps"></a>Étapes suivantes

Vous pouvez effectuer une mise à niveau de plus pour passer de Windows Server 2012 R2 à Windows Server 2019. Pour obtenir des instructions détaillées, consultez [Mettre à niveau Windows Server 2012 R2 vers Windows Server 2019](upgrade-2012r2-to-2019.md).

## <a name="related-articles"></a>Articles connexes

- Pour plus de détails et d’informations sur Windows Server 2012 R2, consultez [Windows Server 2012 R2 et Windows Server 2012](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh801901(v=ws.11)).
