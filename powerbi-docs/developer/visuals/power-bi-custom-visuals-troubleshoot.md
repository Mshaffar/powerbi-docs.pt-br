---
title: Solução de problemas sobre como desenvolver visuais do Power BI
description: Este artigo aborda alguns problemas comuns que podem ser encontrados ao desenvolver ou criar um visual personalizado do Power BI.
author: KesemSharabi
ms.author: kesharab
ms.reviewer: ''
ms.service: powerbi
ms.topic: conceptual
ms.subservice: powerbi-custom-visuals
ms.date: 11/06/2018
ms.openlocfilehash: e066ea5128709a0e16873bba5f025e96938cac54
ms.sourcegitcommit: 7aa0136f93f88516f97ddd8031ccac5d07863b92
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/05/2020
ms.locfileid: "80113636"
---
# <a name="troubleshoot-power-bi-visuals"></a>Solucionar problemas de visuais do Power BI

## <a name="debug"></a>Depurar

**Comando Pbiviz não encontrado (ou erros semelhantes)**

Se você executar `pbiviz` na linha de comando do seu terminal, a tela de ajuda será exibida. Caso contrário, ele não está instalado corretamente. Verifique se no mínimo a versão 4.0 do NodeJS está instalada.

**Não é possível localizar o visual de depuração na guia Visualizações**

O visual de depuração é semelhante a um ícone de aviso dentro da guia **Visualizações**.

![Seleção de visuais](media/power-bi-custom-visuals-troubleshoot/powerbi-developer-visual-selection.png)

Se ele não estiver visível, verifique se você o habilitou nas configurações do Power BI.

> [!NOTE]
> No momento, o visual de depuração somente está disponível no serviço do Power BI, mas não no Power BI Desktop nem no aplicativo móvel. O visual empacotado ainda funcionará em todos os lugares.

**Não é possível contatar o servidor de elemento visual**

Execute o servidor de visual com o comando `pbiviz start` na linha de comando do terminal na raiz do projeto de visual. Se o servidor não estiver em execução, provavelmente os certificados SSL não foram instalados corretamente.

Fique à vontade para contatar a equipe de suporte de visuais do Power BI (pbicvsupport@microsoft.com) caso tenha perguntas, comentários ou problemas.

## <a name="next-steps"></a>Próximas etapas

Saiba mais em [Perguntas frequentes sobre os visuais do Power BI](power-bi-custom-visuals-faq.md#organizational-power-bi-visuals).