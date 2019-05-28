---
title: Ajoutez les balises de métadonnées requises pour votre article concernant Windows Server
description: Une liste des informations vous devez ajouter sous forme de balises de métadonnées vers le haut de vos articles relatifs à Windows Server. Les balises requises sont susceptibles de changer, selon les besoins de votre création de rapports et team.
author: eross-msft
ms.author: lizross
ms.date: 05/06/2019
ms.openlocfilehash: f7c514def1353d44386b1bc53c8cabffe1e31fda
ms.sourcegitcommit: 7e54a1bcd31cd2c6b18fd1f21b03f5cfb6165bf3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65461644"
---
# <a name="add-the-required-metadata-tags-to-your-windows-server-related-article"></a>Ajoutez les balises de métadonnées requises pour votre article concernant Windows Server

En haut de chaque article, il existe des métadonnées spécifiques qui doivent être incluses pour le suivi et à des fins de moteurs de recherche. Les balises requises sont susceptibles de changer, selon les exigences de création de rapports. Toutefois, vous devez être averti si vous avez besoin d’ajouter/supprimer tous les champs.

Il doit ressembler à ceci, y compris les trois traits d’union (-) en haut et bas :

```markdown

---
title: The title of the article should go here. This is used in SEO and search results.

description: A description for the article should go here. This is used in search results, to provide users with information about whether the article has the information they’re looking for.

ms.prod: Use this specific text, windows-server-threshold

ms.reviewer: The Microsoft alias for the primary PM for the feature/functionality

author: Your GitHub alias

ms.author: Your Microsoft alias

manager: Your manager’s Microsoft alias

ms.topic: Type of article, including article, landing-page, get-started-article, or reference

ms.date: Date of change (MM/DD/YYYY)

---

```

## <a name="example"></a>Exemple

```markdown

---
title: What is Windows Admin Center?
description: Learn about the Windows Admin Center, a locally-deployed, browser-based management tool set that lets you manage your Windows Servers with no Azure or cloud dependency.
ms.prod: windows-server-threshold
ms.reviewer: alainch
author: danielle-github
ms.author: danielle
manager: alainch
ms.topic: article
ms.date: 07/06/2019
---

```