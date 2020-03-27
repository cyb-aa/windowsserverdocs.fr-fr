---
title: Créer le fichier OOBE.xml, y compris le logo et le CLUF
description: Décrit comment utiliser Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8a7b3cc1-21bb-4344-8110-f5d5959b370d
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 58d98aa84b8851e3226ebc76c86cffd574400c42
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80312058"
---
# <a name="create-the-oobexml-file-including-logo-and-eula"></a>Créer le fichier OOBE.xml, y compris le logo et le CLUF

>S’applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Vous avez la possibilité d'ajouter votre propre Contrat de Licence Utilisateur Final (CLUF) à la configuration initiale grâce au fichier Oobe.xml. Le fichier Oobe.xml contient le texte et les images destinés à la configuration initiale, à l'écran d'accueil de Windows et à d'autres pages présentées à l'utilisateur final. Vous pouvez ajouter plusieurs fichiers Oobe.xml afin de personnaliser le contenu en fonction de la langue et du pays ou de la région sélectionnés par l'utilisateur final. Pour plus d’informations, consultez la documentation [Kit de déploiement et d’évaluation Windows (ADK) pour Windows 8](https://go.microsoft.com/fwlink/?LinkId=248694).  
  
 Le CLUF de votre société est affiché à la suite du CLUF Microsoft. En effet, le CLUF de Microsoft est le premier contrat de licence présenté à l'utilisateur au cours de la configuration initiale. Vous êtes libre de placer votre CLUF à l'endroit de votre choix sur le serveur. Il suffira simplement de mentionner son emplacement dans le fichier Oobe.xml.  
  
#### <a name="to-add-your-company-eula-and-logo"></a>Pour ajouter le CLUF et le logo de votre société  
  
1. Ouvrez le fichier Oobe.xml dans un éditeur de texte, tel que le Bloc-notes.  
  
2. Dans les balises < logopath\></logopath\>, entrez le chemin d’accès absolu à votre fichier de logo. Choisissez un logo au format .png (Portable Network Graphics) 32 bits de 240 x 100 pixels.  
  
3. Dans les balises < eulafilename\></eulafilename\>, entrez le chemin d’accès absolu au fichier EULA. Optez pour un fichier au format .rtf.  
  
4. Dans le nom de <\>étiquettes </name\>, entrez le nom de votre société.  
  
    L'exemple suivant illustre les balises d'un fichier Oobe.xml :  
  
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
  
5. Enregistrez le fichier.  
  
6. Placez le fichier Oobe.xml à l'un des emplacements suivants :  
  
   |Emplacement Oobe.XML|Critère de détermination de l'emplacement|  
   |-----------------------|----------------------------------------|  
   |%WINDIR%\System32\Oobe\Info\|le serveur est livré dans un seul pays/région et dans un système de langue unique.|  
   |%windir%\system32\oobe\info\default\\< Language\>|Le serveur est prévu pour fonctionner dans une seule région/pays et sur un système multilingue.|  
   |%WINDIR%\System32\Oobe\Info\\< pays/région > \ et%WINDIR%\System32\Oobe\Info\\< pays/région >\\<\>\|le serveur est livré dans plus d’un pays/région, et les paramètres nécessitent des personnalisations par pays/région, chacun avec une seule langue. Où < pays/région > est la version décimale de l’identificateur d’emplacement géographique (GeoID) du pays ou de la région où le serveur est déployé, et < langue\> est la version décimale de l’identificateur de paramètres régionaux (LCID).|  
  
   Si vous disposez d'un logo d'entreprise alternatif avec un texte blanc, la qualité d'affichage sera meilleure pendant la configuration car l'arrière-plan est bleu.  Vous pouvez spécifier ce logo en option en définissant une clé de registre et une valeur.  
  
#### <a name="to-specify-a-company-logo-by-setting-the-oem-registry-key"></a>Pour spécifier un logo d'entreprise en définissant la clé de registre OEM  
  
1.  Sur le serveur, déplacez votre souris sur le coin supérieur droit de l’écran, puis cliquez sur **Rechercher**.  
  
2.  Dans la zone de recherche, tapez **regedit**, puis cliquez sur l'application Regedit.  
  
3.  Dans le volet de navigation, naviguez jusqu'aux entrées **HKEY_LOCAL_MACHINE**, **SOFTWARE**, **Microsoft** et **Windows Server**. Si la clé OEM n'existe pas, vous pouvez la créer de la manière suivante :  
  
    1.  Faites un clic droit sur **Windows Server**, cliquez sur **Nouveau**, puis cliquez sur **Clé**.  
  
    2.  Tapez **OEM** comme nom de clé.  
  
4.  (Facultatif) Si vous définissez une nouvelle entrée pour un logo, vous pouvez créer plusieurs clés afin de distinguer les versions du logo prévues pour chaque langue. Si vous disposez, par exemple, d'une version anglaise et française du logo, il suffit de créer une clé en-us et une clé fr-fr. Comme tous les fichiers des logos sont stockés dans le même dossier, les différentes instances du fichier d'image spécifiques à chaque langue doivent porter un nom unique. Dans notre exemple, vous pouvez créer un fichier appelé LogoWithWhiteText_en.png et LogoWithWhiteText_de.png.  
  
5.  Cliquez avec le bouton droit sur **OEM** ou sur la clé de langue appropriée, cliquez sur **Nouveau**, puis sur **Valeur de chaîne**.  
  
6.  Tapez la chaîne de caractères LogoWithWhiteText, puis appuyez sur ENTRÉE.  
  
7.  Cliquez avec le bouton droit sur la nouvelle chaîne, puis cliquez sur **Modifier**.  
  
8.  Saisissez le chemin qui contient l'image du logo, puis cliquez sur OK.  
  
## <a name="see-also"></a>Voir aussi  
 [Prise en main avec Windows Server Essentials ADK](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [Création et personnalisation de l’Image](Creating-and-Customizing-the-Image.md)   
 [Personnalisations supplémentaires](Additional-Customizations.md)   
 [Préparation de l’image pour le déploiement](Preparing-the-Image-for-Deployment.md)   
 [Test de l’expérience utilisateur](Testing-the-Customer-Experience.md)