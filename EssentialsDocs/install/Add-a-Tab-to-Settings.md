---
title: Ajouter un onglet aux paramètres
description: Décrit comment utiliser Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: aac6b7f3-9020-46c3-a83f-b81542300385
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 68c469487c5fff1e690d6eddda6c28fb64b8f015
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80817483"
---
# <a name="add-a-tab-to-settings"></a>Ajouter un onglet aux paramètres

>S’applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Vous pouvez ajouter un onglet aux Paramètres dans le Tableau de bord en créant et en installant un assembly de code utilisé par le Gestionnaire des paramètres dans le système d'exploitation.  
  
## <a name="add-a-tab-to-settings"></a>Ajouter un onglet aux Paramètres  
 Vous ajoutez un onglet aux paramètres en exécutant les tâches suivantes :  
  
-   [Ajout d’une implémentation de l’interface ISettingsData à l’assembly](Add-a-Tab-to-Settings.md#BKMK_ISettingsData).  
  
-   [Sign the assembly with an Authenticode signature](Add-a-Tab-to-Settings.md#BKMK_SignAssembly).  
  
-   [Install the assembly on the reference computer](Add-a-Tab-to-Settings.md#BKMK_InstallAssembly).  
  
###  <a name="add-an-implementation-of-the-isettingsdata-interface-to-the-assembly"></a><a name="BKMK_ISettingsData"></a>Ajouter une implémentation de l’interface ISettingsData à l’assembly  
 L’interface ISettingsData fait partie de l’espace de noms Microsoft.WindowsServerSolutions.Settings de l’assembly AdminCommon.dll, lequel figure dans \Program Files\Windows Server\Bin.  
  
##### <a name="to-add-the-isettingsdata-code-to-the-assembly"></a>Pour ajouter le code ISettingsData à l’assembly  
  
1.  Ouvrez Visual Studio 2010 en tant qu’administrateur en cliquant avec le bouton droit sur le programme dans le menu **Démarrer**, puis en sélectionnant **Exécuter en tant qu’administrateur**.  
  
2.  Cliquez sur **Fichier**, sur **Nouveau**, puis sur **Projet**.  
  
3.  Dans la boîte de dialogue **Nouveau projet**, cliquez sur **Visual C#** puis sur **Bibliothèque de classes**, donnez le nom **DashboardSettingsPage** à la solution, puis cliquez sur **OK**.  
  
    > [!IMPORTANT]
    >  L’assembly installé sur le serveur doit être nommé DashboardSettingsPage.dll et le fichier dll doit être copié sur %ProgramFiles%\Windows Server\Bin\OEM.  
  
4.  Créez le contrôle que vous souhaitez utiliser dans l’onglet. Dans cet exemple, le contrôle Settings est appelé MySettingsControl.  
  
5.  Renommez le fichier Class1.cs. Appelez-le, par exemple, MySettingTab.cs.  
  
6.  Ajoutez une référence au fichier AdminCommon.dll.  
  
7.  Ajoutez l’instruction d’utilisation suivante :  
  
    ```c#  
    using Microsoft.WindowsServerSolutions.Settings;  
    ```  
  
8.  Changez l’espace de noms et l’en-tête de classe pour qu’ils correspondent à l’exemple suivant :  
  
    ```  
  
    namespace DashboardSettingsPage  
    {  
        public class MySettingTab : ISettingsData  
        {  
        }  
    }  
  
    ```  
  
9. Instanciez une instance du contrôle que vous avez créé pour l’onglet. Par exemple :  
  
    ```c#  
    private MySettingsControl tab;  
    ```  
  
10. Ajoutez le constructeur de la classe. L’exemple de code suivant illustre le constructeur :  
  
    ```  
  
    public MySettingTab()  
    {  
       tab = new MySettingsControl();  
    }  
    ```  
  
11. Ajoutez la méthode Commit qui a pour effet de soumettre les modifications du paramètre. L’exemple de code suivant illustre la méthode Commit :  
  
    ```  
  
    void ISettingsData.Commit(bool dismissed)  
    {  
       // Implement the code that is required to submit your setting changes  
    }  
    ```  
  
12. Ajoutez la méthode TabControl, qui identifie le contrôle pour l’onglet. L’exemple de code suivant montre la méthode TabControl :  
  
    ```  
  
    System.Windows.Forms.Control ISettingsData.TabControl  
    {  
       get { return tab; }  
    }  
    ```  
  
13. Ajoutez la méthode, qui fournit un identificateur unique pour l’onglet. L’exemple de code suivant montre la méthode :  
  
    ```  
  
    private Guid id = Guid.NewGuid();  
  
    Guid ISettingsData.TabId  
    {  
       get { return id; }  
    }  
    ```  
  
14. Ajoutez la méthode TabOrder, qui retourne l’ordre de l’onglet. L’exemple de code suivant illustre la méthode TabOrder :  
  
    ```  
  
    int ISettingsData.TabOrder  
    {  
       get { return 0; }  
    }  
    ```  
  
    > [!NOTE]
    >  L'ordre des onglets est déterminé par l'utilisation de chiffres commençant par 0. Les onglets des paramètres Microsoft sont affichés en premier et sont suivis de vos propres onglets en fonction du classement que vous avez défini. Si vous disposez de trois onglets de paramètres, par exemple, attribuez-leur les valeurs 0, 1 et 2 en fonction de l’ordre d’affichage voulu.  
  
15. Ajoutez la méthode TabTitle, qui fournit le titre de l’onglet. L’exemple de code suivant illustre la méthode TabTitle :  
  
    ```  
  
    string ISettingsData.TabTitle  
    {  
      get { return "My Settings Tab"; }  
    }  
    ```  
  
    > [!NOTE]
    >  Le texte du titre peut également provenir d’un fichier de ressources lorsqu’il est nécessaire de localiser son contenu dans une autre langue.  
  
16. Enregistrez et générez la solution.  
  
###  <a name="sign-the-assembly-with-an-authenticode-signature"></a><a name="BKMK_SignAssembly"></a>Signer l’assembly avec une signature Authenticode  
 Vous devez signer l'assembly avec une signature Authenticode pour pouvoir l'utiliser dans le système d'exploitation. Pour plus d’informations sur la signature de l’assembly, consultez [Signature et vérification d’un code avec Authenticode](https://msdn.microsoft.com/library/ms537364\(VS.85\).aspx#SignCode).  
  
###  <a name="install-the-assembly-on-the-reference-computer"></a><a name="BKMK_InstallAssembly"></a>Installer l’assembly sur l’ordinateur de référence  
 Après avoir généré la solution, placez une copie du fichier DashboardSettingsPage.dll dans le dossier suivant sur l’ordinateur de référence :  
  
 **%Programfiles%\Windows dossier server\bin\oem**  
  
## <a name="see-also"></a>Voir aussi  
 [Création et personnalisation de l’Image](Creating-and-Customizing-the-Image.md)   
 [Personnalisations supplémentaires](Additional-Customizations.md)   
 [Préparation de l’image pour le déploiement](Preparing-the-Image-for-Deployment.md)   
 [Test de l’expérience utilisateur](Testing-the-Customer-Experience.md)