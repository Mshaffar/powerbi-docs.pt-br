---
title: Tipos de dados no Power BI Desktop
description: Tipos de dados no Power BI Desktop
author: davidiseminger
ms.reviewer: ''
ms.service: powerbi
ms.subservice: powerbi-desktop
ms.topic: reference
ms.date: 05/06/2020
ms.author: davidi
LocalizationGroup: Connect to data
ms.openlocfilehash: 06033a65acd090f0297a53bfbf0f0a004b68649a
ms.sourcegitcommit: a72567f26c1653c25f7730fab6210cd011343707
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83561560"
---
# <a name="data-types-in-power-bi-desktop"></a>Tipos de dados no Power BI Desktop
Este artigo descreve os tipos de dados com suporte no Power BI Desktop e DAX (Data Analysis Expressions). 

Quando você carrega dados no Power BI Desktop, ele tenta converter o tipo de dados da coluna de origem em um tipo de dados que dá melhor suporte a armazenamento, cálculos e visualização de dados mais eficientes. Por exemplo, se uma coluna de valores que você importa do Excel não tem nenhum valor fracionário, o Power BI Desktop converterá toda a coluna de dados para um tipo de dados de Número Inteiro, que é mais adequado para armazenar inteiros.

Esse conceito é importante porque algumas funções do DAX têm requisitos de tipo de dados especiais. Embora em muitos casos o DAX converta implicitamente um determinado tipo de dados para você, há alguns casos em que isso não ocorrerá.  Por exemplo, se uma função DAX requer um tipo de dados de Data e o tipo de dados para a coluna é Texto, a função DAX não funcionará corretamente.  Portanto, é importante e útil obter o tipo de dados correto para uma coluna. Conversões implícitas são descritas posteriormente neste artigo.

## <a name="determine-and-specify-a-columns-data-type"></a>Determinar e especificar o tipo de dados da coluna
Na área de trabalho do Power BI, você pode determinar e especificar o tipo de dados de uma coluna no Editor de Consultas, ou na Exibição de Dados ou de Relatório:

**Tipos de dados no Editor de consultas**

![](media/desktop-data-types/pbiddatatypesinqueryeditort.png)

**Tipos de dados na Exibição de dados ou de relatório**

![](media/desktop-data-types/pbiddatatypesindatareportview.png)

A lista suspensa Tipo de Dados no Editor de Consultas tem dois tipos de dados que não estão presentes atualmente nos Modos de Exibição de Dados ou de Relatório: **Data/Hora/Fuso horário** e **Duração**. Quando uma coluna com esses tipos de dados for carregada no modelo e exibida no modo de exibição Dados ou Relatório, uma coluna com o tipo de dados Data/Hora/Fuso Horário será convertida em Data/Hora e uma coluna com o tipo de dados Duração será convertida em um Número Decimal.

Atualmente, não há suporte para o tipo de dados **Binary** fora do Editor de Consultas. No Editor de Consultas, você poderá usá-lo ao carregar arquivos binários se o converter em outros tipos de dados antes de carregá-lo no modelo do Power BI. Ele está localizado nos menus Exibição de Dados e Exibição de Relatório por motivos herdados, mas se você tentar carregar colunas binárias no modelo do Power BI, poderá receber erros.  

### <a name="number-types"></a>Tipos de número
O Power BI Desktop dá suporte a três tipos de número:

**Número Decimal** – representa um número de ponto flutuante (oito bytes) de 64 bits. É o tipo de número mais comum e corresponde aos números como você normalmente os imagina.  Embora seja projetado para lidar com números com valores fracionários, ele também lida com números inteiros.  O tipo de Número Decimal pode lidar com valores negativos de -1,79E +308 a -2,23E -308, 0, e valores positivos de 2,23E -308 a 1,79E + 308. Por exemplo, números como 34, 34,01 e 34,000367063 são números decimais válidos. A maior precisão que pode ser representada em um tipo de Número Decimal tem 15 dígitos. O separador decimal pode ocorrer em qualquer lugar no número. O tipo de Número Decimal corresponde a como o Excel armazena seus números.

**Número Decimal Fixo** – tem um local para o separador decimal fixo. O separador decimal tem sempre quatro dígitos à direita e permite 19 dígitos de significância.  O maior valor que ele pode representar é 922.337.203.685.477,5807 (positivo ou negativo).  O tipo de Número Decimal Fixo é útil em casos em que o arredondamento pode introduzir erros.  Quando você trabalha com muitos números que têm valores fracionários pequenos, às vezes, eles podem se acumular e forçar um número a apresentar uma pequena diferença.  Como os valores após os quatro dígitos à direita do separador decimal são truncados, o tipo Decimal Fixo pode ajudá-lo a evitar esses tipos de erros.   Se você está familiarizado com o SQL Server, esse tipo de dados correspondente ao Decimal (19,4) do SQL Server, ou ao tipo de Dados de Moeda no Power Pivot. 

**Número Inteiro** – representa um valor inteiro (oito bytes) de 64 bits. Como é um número inteiro, ele não tem nenhum dígito à direita da casa decimal. Ele permite 19 dígitos; números inteiros positivos ou negativos entre -9.223.372.036.854.775.807 (-2^63+1) e 9.223.372.036.854.775.806 (2^63-2). Pode representar a maior precisão possível dos diversos tipos de dados numéricos.  Assim como com o tipo Decimal Fixo, o tipo de Número Inteiro pode ser útil em casos nos quais você precisa controlar o arredondamento. 

> [!NOTE]
>  O modelo de dados do Power BI Desktop dá suporte a valores inteiros de 64 bits, mas o maior número que os visuais conseguem expressar com segurança é 9.007.199.254.740.991 (2^53-1), devido a limitações do JavaScript. Se você trabalha com números maiores que esse em seu modelo de dados, reduza o tamanho por meio de cálculos antes de adicioná-los a um visual 
> 
>

### <a name="datetime-types"></a>Tipos de data/hora
O Power BI Desktop dá suporte a cinco tipos de dados de data/hora na Visualização da Consulta.  Tanto Data/Hora/Fuso horário quanto a Duração são convertidos durante o carregamento para o modelo. O modelo de dados do Power BI Desktop só dá suporte a data/hora, mas elas podem ser formatadas como datas ou horas de maneira independente. 

**Data/Hora** – representa um valor de data e um valor temporal.  Nos bastidores, o valor de Data/Hora é armazenado como um Tipo de Número Decimal.  Então, na verdade, é possível converter entre os dois.   A parte de hora da data é armazenada como uma fração de múltiplos inteiros de 1/300 segundos (3,33 ms).  Há suporte para datas entre os anos de 1900 e 9999.

**Data** – representa apenas uma Data (sem parte referente à hora).  Quando convertido para o modelo, uma Data é o mesmo que um valor de Data/Hora com zero como o valor fracionário.

**Hora** – representa apenas a Hora (nenhuma parte referente à Data).  Quando convertido para o modelo, um valor de Hora é igual a um valor de Data/Hora sem dígitos à esquerda da casa decimal.

**Data/Hora/Fuso Horário**: representa uma data/hora UTC com um deslocamento de fuso horário.  Ele é convertido em Data/Hora quando é carregado no modelo. O modelo do Power BI não ajusta o fuso horário com base na localização ou na localidade de um usuário etc. Se um valor igual a 09:00 for carregado no modelo nos EUA, ele será exibido como 09:00 sempre que o relatório for aberto ou exibido. 

**Duração** – representa um intervalo de tempo. Ele é convertido em um Tipo de Número Decimal quando é carregado no modelo.  Como um tipo de Número Decimal, ele pode ser adicionado ou subtraído de um campo de Data/Hora com resultados corretos.  Como um tipo de Número Decimal, você pode usá-lo facilmente em visualizações que mostram a magnitude.

### <a name="text-type"></a>Tipo de texto
**Texto** - uma cadeia de caracteres de dados de caractere Unicode. Ela pode conter cadeias de caracteres, datas ou números representados no formato de texto. O comprimento máximo da cadeia de caracteres é 268.435.456 caracteres Unicode (caracteres de 256 megabytes) ou 536.870.912 bytes.

### <a name="truefalse-type"></a>Tipo verdadeiro/falso
**Verdadeiro/Falso** – um valor Booliano de Verdadeiro ou Falso.

### <a name="blanksnulls-type"></a>Tipo em branco/nulos
**Em branco** - é um tipo de dados em DAX que representa e substitui nulos SQL. Você também pode gerar um elemento em branco usando a função [BLANK](/dax/blank-function-dax) e testar elementos em branco usando a função lógica [ISBLANK](/dax/isblank-function-dax).

### <a name="binary-data-type"></a>Tipo de dados Binary

O tipo de dados Binary pode ser usado para representar outros dados com um formato binário. No Editor de Consultas, você poderá usá-lo ao carregar arquivos binários se você o converter em outros tipos de dados antes de carregá-lo no modelo do Power BI. Não há suporte para colunas binárias no modelo de dados do Power BI. Ele está localizado nos menus Exibição de Dados e Exibição de Relatório por motivos herdados, mas se você tentar carregar colunas binárias no modelo do Power BI, poderá receber erros.




> [!NOTE]
>  Se uma coluna binária estiver na saída das etapas de uma consulta, a tentativa de atualizar os dados por meio de um gateway poderá causar erros. Recomendamos que você remova explicitamente todas as colunas binárias como a última etapa nas consultas.    
> 

### <a name="table-data-type"></a>Tipo de dados de tabela
O DAX usa um tipo de dados de tabela em muitas funções, como agregações e cálculos de inteligência de dados temporais. Algumas funções exigem uma referência a uma tabela; outras funções retornam uma tabela que pode ser usada como entrada para outras funções. Em algumas funções que exigem uma tabela como entrada, você pode especificar uma expressão que é avaliada como uma tabela; para algumas funções, é necessária uma referência a uma tabela base. Para obter informações sobre os requisitos de funções específicas, consulte [Referência de função DAX](/dax/dax-function-reference).

## <a name="implicit-and-explicit-data-type-conversion-in-dax-formulas"></a>Conversão implícita e explícita de tipo de dados em fórmulas DAX
Cada função DAX tem requisitos específicos quanto aos tipos de dados que são usados como entradas e saídas. Por exemplo, algumas funções exigem inteiros para alguns argumentos e datas para outros; outras funções exigem texto ou tabelas.

Se os dados na coluna que você especifica como um argumento são incompatíveis com o tipo de dados exigido pela função, em muitos casos o DAX retornará um erro. No entanto, sempre que possível, o DAX tentará converter implicitamente os dados para o tipo de dados necessário. Por exemplo:

* Você pode digitar uma data como uma cadeia de caracteres e o DAX analisará a cadeia de caracteres e tentará transmiti-la como um dos formatos de data e hora do Windows.
* Você pode adicionar TRUE + 1 e obter o resultado 2, pois TRUE é implicitamente convertido para o número 1 e a operação 1+1 é executada.
* Se você adicionar valores em duas colunas e um valor for representado como texto ("12") e o outro como um número (12), o DAX converte implicitamente a cadeia de caracteres em um número e, em seguida, faz a adição para chegar a um resultado numérico. A expressão a seguir retorna 44: = "22" + 22.
* Se você tentar concatenar dois números, o Excel vai apresentá-los como cadeias de caracteres e, em seguida, concatenar. A expressão a seguir retorna "1234": = 12 & 34.

### <a name="table-of-implicit-data-conversions"></a>Tabela de conversões implícitas de dados
O tipo de conversão executada é determinado pelo operador, que transmite os valores que ele requer antes de executar a operação solicitada. Essas tabelas listam os operadores e indicam a conversão que é realizada em cada tipo de dados na coluna quando ele é emparelhado com o tipo de dados na linha que intersecciona essa coluna.

> [!NOTE]
>  Tipos de dados de texto não são incluídos nessas tabelas. Quando um número é representado em um formato de texto, em alguns casos o Power BI tentará determinar o tipo de número e representá-lo como um número.
> 
> 

**Adição (+)**

| Operador(+) | INTEGER | CURRENCY | REAL | Date/time |
| --- | --- | --- | --- | --- |
| INTEGER |INTEGER |CURRENCY |REAL |Date/time |
| CURRENCY |CURRENCY |CURRENCY |REAL |Date/time |
| REAL |REAL |REAL |REAL |Date/time |
| Date/time |Date/time |Date/time |Date/time |Date/time |

Por exemplo, se um número real é usado em uma operação de adição em combinação com dados de moedas, os dois valores são convertidos em REAL e o resultado é retornado como REAL.

**Subtração (-)**

Na tabela a seguir, o cabeçalho da linha é o minuendo (lado esquerdo) e o cabeçalho da coluna é o subtraendo (lado direito).

| Operador(-) | INTEGER | CURRENCY | REAL | Date/time |
| --- | --- | --- | --- | --- |
| INTEGER |INTEGER |CURRENCY |REAL |REAL |
| CURRENCY |CURRENCY |CURRENCY |REAL |REAL |
| REAL |REAL |REAL |REAL |REAL |
| Date/time |Date/time |Date/time |Date/time |Date/time |

Por exemplo, se uma data é usada em uma operação de subtração com qualquer outro tipo de dados, ambos os valores são convertidos em datas e o valor retornado também é uma data.

> [!NOTE]
>    Modelos de dados também são compatíveis com o operador unário, - (negativo), mas esse operador não altera o tipo de dados do operando.
> 
> 

**Multiplicação (*)**

| Operador(*) | INTEGER | CURRENCY | REAL | Date/time |
| --- | --- | --- | --- | --- |
| INTEGER |INTEGER |CURRENCY |REAL |INTEGER |
| CURRENCY |CURRENCY |REAL |CURRENCY |CURRENCY |
| REAL |REAL |CURRENCY |REAL |REAL |

Por exemplo, se um inteiro for combinado com um número real em uma operação de multiplicação, os dois números são convertidos em números reais, e o valor retornado também é REAL.

**Divisão (/)**

Na tabela a seguir, o cabeçalho da linha é o numerador e o cabeçalho da coluna é o denominador.

| Operador(/) (Linha/Coluna) | INTEGER | CURRENCY | REAL | Date/time |
| --- | --- | --- | --- | --- |
| INTEGER |REAL |CURRENCY |REAL |REAL |
| CURRENCY |CURRENCY |REAL |CURRENCY |REAL |
| REAL |REAL |REAL |REAL |REAL |
| Date/time |REAL |REAL |REAL |REAL |

Por exemplo, se um inteiro for combinado com um valor de moeda em uma operação de divisão, os dois valores são convertidos em números reais e o resultado também é um número real.

### <a name="comparison-operators"></a>Operadores de comparação
Em expressões de comparação, valores Boolianos são considerados maiores que valores de cadeia de caracteres, enquanto valores de cadeia de caracteres são considerados maiores que valores numéricos ou de data/hora; números e valores de data/hora são considerados como tendo a mesma classificação. Nenhuma conversão implícita é executada para valores de cadeia de caracteres ou Boolianos; BLANK ou um valor em branco é convertido em 0/""/false, dependendo do tipo de dados do outro valor comparado.

As seguintes expressões DAX ilustram esse comportamento:

=IF(FALSE()\>"true","Expression is true", "Expression is false"), retorna "Expression is true"

=IF("12"\>12,"Expression is true", "Expression is false"), retorna "Expression is true"

=IF("12"=12,"Expression is true", "Expression is false"), retorna "Expression is false"

As conversões são executadas implicitamente para tipos numéricos ou de data/hora, conforme descrito na tabela a seguir:

| Operador de Comparação | INTEGER | CURRENCY | REAL | Date/time |
| --- | --- | --- | --- | --- |
| INTEGER |INTEGER |CURRENCY |REAL |REAL |
| CURRENCY |CURRENCY |CURRENCY |REAL |REAL |
| REAL |REAL |REAL |REAL |REAL |
| Date/time |REAL |REAL |REAL |Date/time |

### <a name="handling-blanks-empty-strings-and-zero-values"></a>Tratamento de elementos em branco, cadeias de caracteres vazias e valores zero
No DAX, um valor nulo ou em branco, uma célula vazia ou um valor ausente são representados pelo mesmo tipo novo de valor, um BLANK. Você também pode gerar elementos em branco usando a função BLANK, ou testar elementos em branco usando a função ISBLANK.

O modo como os elementos em branco são tratados em operações como adição ou concatenação depende da função individual. A tabela a seguir resume as diferenças entre as fórmulas DAX e do Microsoft Excel, da maneira que os elementos em branco são tratados.

| Expression | DAX | Excel |
| --- | --- | --- |
| BLANK + BLANK |BLANK |0(zero) |
| BLANK + 5 |5 |5 |
| BLANK * 5 |BLANK |0(zero) |
| 5/BLANK |Infinity |Erro |
| 0/BLANK |NaN |Error |
| BLANK/BLANK |BLANK |Erro |
| FALSE OR BLANK |FALSO |FALSO |
| FALSE AND BLANK |FALSO |FALSE |
| TRUE OR BLANK |VERDADEIRO |TRUE |
| TRUE AND BLANK |FALSO |VERDADEIRO |
| BLANK OR BLANK |BLANK |Erro |
| BLANK AND BLANK |BLANK |Erro |