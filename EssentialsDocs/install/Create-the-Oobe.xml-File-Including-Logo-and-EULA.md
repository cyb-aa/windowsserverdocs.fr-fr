---
title: "Créer le fichier Oobe.xml, y compris le Logo et le CLUF"
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8a7b3cc1-21bb-4344-8110-f5d5959b370d
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: f8f99a2051e114b3c890f1cdac23aebf58689980
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="create-the-oobexml-file-including-logo-and-eula"></a>Créer le fichier Oobe.xml, y compris le Logo et le CLUF

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials

Vous pouvez ajouter votre propre contrat de licence utilisateur final (CLUF) pour la Configuration initiale en utilisant le fichier Oobe.xml. Oobe.xml est un fichier utilisé pour fournir le texte et des images pour la Configuration initiale, accueil de Windows et les autres pages présentées à l’utilisateur final. Vous pouvez ajouter plusieurs fichiers Oobe.xml pour personnaliser le contenu selon les sélections de langue et du pays ou région de l’utilisateur final. Pour plus d’informations, voir la [Kit de déploiement pour Windows8 et évaluation Windows](https://go.microsoft.com/fwlink/?LinkId=248694) documentation.  
  
 Le CLUF de votre société est affiché en plus de EULA de Microsoft. Le sera EULA Microsoft le CLUF première affiché lors de l’expérience utilisateur final de la configuration initiale, puis votre volonté CLUF s’afficher. Votre CLUF permettre être placée n’importe où sur le serveur et que vous spécifiez l’emplacement dans le fichier Oobe.xml.  
  
#### <a name="to-add-your-company-eula-and-logo"></a>Pour ajouter votre société CLUF et le logo  
  
1.  Ouvrez le fichier Oobe.xml dans un éditeur de texte, tel que le bloc-notes.  
  
2.  Dans le < logopath\ >< / logopath\ > balises, entrez le chemin d’accès absolu au fichier de votre logo. Ce fichier doit contenir un fichier (.png) graphique réseau portable 32bits de 240 x 100pixels.  
  
3.  Dans le < eulafilename\ >< / eulafilename\ > balises, entrez le chemin d’accès absolu au fichier du CLUF. Le fichier CLUF doit être un fichier de (.rtf) format RTF.  
  
4.  Dans le < nom\ >< / nom\ > balises, entrez le nom de votre société.  
  
     L’exemple suivant montre les balises dans un fichier Oobe.xml:  
  
    ```  
  
    <FirstExperience>  
       <oobe>  
          <oem>  
             <name>Fabrikam</name>  
             <logopath>c:\fabrikam\fabrikam.png</logopath>  
             <eulafilename>c:\fabrikam\fabrikam_eula.rtf</eulafilename>  
          </oem>  
       </oobe>  
    </FirstExperience>  
  
    ```  
  
5.  Enregistrez le fichier.  
  
6.  Placez le fichier Oobe.xml dans un des emplacements suivants:  
  
    |Emplacement Oobe.xml|Condition pour déterminer l’emplacement|  
    |-----------------------|----------------------------------------|  
    |%windir%\system32\oobe\info\|Le serveur est prévu pour fonctionner dans un pays/une région unique et un système de langue unique.|  
    |%windir%\system32\oobe\info\default\\ < langue >|Le serveur est prévu pour fonctionner dans un pays/une région unique et système multilingue.|  
    |%windir%\system32\oobe\info\\ < pays/région > \ et %windir%\system32\oobe\info\\ < pays/région > \\ < langue > \|Le serveur est prévu pour fonctionner à plusieurs pays/région et nécessitera chaque pays/région, chacun avec une seule langue. Où < pays/région > est la version en notation décimale de l’identificateur d’emplacement géographique (GeoID) du pays ou région où le serveur est déployé et < langue > est la version en notation décimale de l’identificateur de paramètres régionaux (LCID).|  
  
 Si vous avez un logo d’entreprise alternatif avec un texte blanc, il peut afficher une meilleure pendant la configuration en raison de l’arrière-plan bleu.  Vous pouvez éventuellement spécifier ce logo en définissant une clé de Registre et une valeur.  
  
#### <a name="to-specify-a-company-logo-by-setting-the-oem-registry-key"></a>Pour spécifier un logo d’entreprise en définissant la clé de Registre OEM  
  
1.  Sur le serveur, déplacez votre souris vers le coin supérieur droit de l’écran, puis cliquez sur **recherche**.  
  
2.  Dans la zone de recherche, tapez **regedit**, puis cliquez sur l’application Regedit.  
  
3.  Dans le volet de navigation, accédez à **HKEY_LOCAL_MACHINE**, développez **logiciel**, développez **Microsoft**, développez **Windows Server**. Si la clé OEM n’existe pas, créez la clé comme suit:  
  
    1.  Avec le bouton droit **Windows Server**, cliquez sur **New**, puis cliquez sur **clé**.  
  
    2.  Pour le nom de la clé, tapez **OEM**.  
  
4.  (Facultatif) Si vous créez une entrée pour un logo, vous pouvez créer des clés différentes pour différencier les versions de langue du logo. Par exemple, si vous avez les versions anglaise et allemandes d’un logo, vous pouvez créer un en-us de clé et une clé fr-fr. Étant donné que tous les fichiers des logos sont stockés dans le même dossier, vous devez fournir les instances du fichier image du logo avec un nom unique pour chaque langue. Par exemple, vous pouvez créer un fichier appelé LogoWithWhiteText_en.png et LogoWithWhiteText_de.png.  
  
5.  Un clic droit **OEM** ou avec le bouton droit de la clé de langue approprié, cliquez sur **New**, puis cliquez sur **valeur de chaîne**.  
  
6.  Caractères logowithwhitetext la chaîne, puis appuyez sur ENTRÉE.  
  
7.  Avec le bouton droit de la nouvelle chaîne, puis cliquez sur **modifier**.  
  
8.  Entrez le chemin d’accès qui contient l’image du logo, puis cliquez sur OK.  
  
## <a name="see-also"></a>Voir aussi  
 [Prise en main du kit ADK WindowsServerEssentials](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [Création et personnalisation de l’Image](Creating-and-Customizing-the-Image.md)   
 [Personnalisations supplémentaires](Additional-Customizations.md)   
 [Préparation de l’Image pour le déploiement](Preparing-the-Image-for-Deployment.md)   
 [Test de l’expérience client](Testing-the-Customer-Experience.md)