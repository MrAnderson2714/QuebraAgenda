# Dashboard de Quebra de Agenda

Dashboard executivo e operacional para acompanhamento de **Quebra de Agenda** e **Aderência ao Checklist**, desenvolvido em HTML único, responsivo e interativo.

O painel consolida informações geográficas, comerciais e operacionais para identificar os principais ofensores, causas de baixa, equipes, canais e logins de vendedores que demandam atuação.

---

## Visão geral

O dashboard relaciona três conjuntos de dados do arquivo `IND_QUEBRA.xlsx`:

- **CIDADES**: cadastro de cidades e dimensões geográficas por código IBGE;
- **CONTRATOS**: canal, equipe e login do vendedor por `NR_SOLICITACAO + COD_MUNICIPIO`;
- **QUEBRA**: agenda, quebra, checklist, tipo de OS e causa de baixa.

O relacionamento principal entre as bases comerciais e operacionais utiliza:

```text
NR_SOLICITACAO + IBGE
```

O arquivo HTML possui uma base incorporada para contingência e, quando publicado no GitHub Pages, tenta carregar automaticamente o arquivo `IND_QUEBRA.xlsx` localizado na mesma pasta do dashboard.

---

## Indicadores principais

### IND Quebra

Percentual de agendas com quebra:

```text
IND Quebra = SUM(QUEBRA) / SUM(TOTAL)
```

**Meta:** menor ou igual a **20%**.

Faixas visuais:

- `0% a 20%`: verde;
- `acima de 20% a 25%`: amarelo;
- `acima de 25%`: vermelho.

### ADE Checklist

Percentual de agendas aderentes ao checklist de confirmação:

```text
ADE Checklist = SUM(N) / SUM(TOTAL)
```

Onde:

- `N = 1`: contato/checklist aderente;
- `N = 0`: contato/checklist não aderente.

**Meta:** maior ou igual a **80%**.

O checklist representa o contato com o cliente para confirmar o agendamento e reduzir a probabilidade de quebra.

### Recuperação

Quantidade estimada de agendas que precisam deixar de quebrar para o indicador retornar à meta:

```text
Recuperação = MAX(0, CEILING(SUM(QUEBRA) - 20% × SUM(TOTAL)))
```

---

## Estrutura do dashboard

### Cards executivos

- **Atualização**: última data de agenda disponível e origem da base;
- **IND Quebra**: valor atual, desvio da meta, comparação com D-1 e D-7 e recuperação necessária;
- **ADE Checklist**: aderência atual, distância da meta, comparação com D-1 e D-7 e confirmações adicionais necessárias;
- **Total**: volume de agendas no recorte;
- **Total Quebra**: volume de quebras no recorte.

### Indicador Diário: Quebra x ADE Checklist

Gráfico temporal com barras de:

- IND Quebra;
- ADE Checklist.

Recursos:

- valores percentuais nas barras;
- metas de 20% e 80%;
- agrupamento por **Dia**, **Semana** ou **Mês**;
- semana considerada de **domingo a sábado**;
- comparação com o período anterior;
- tooltip com volume, indicadores e principais canais.

### Pareto de Causa Raiz

Ranking dos principais `COD_BAIXA` e `DSC_BAIXA` por volume de quebra.

O tooltip apresenta:

- código e descrição;
- total de quebras;
- participação da causa;
- distribuição por Grupo de Canais.

### Analítico de Indicador por Hierarquia

Tabela dinâmica com seletor de hierarquia.

Hierarquias disponíveis:

- UF;
- Cluster;
- Cidade;
- Segmento do Município;
- Equipe de Vendas;
- Canal de Venda Subgrupo;
- Canal de Venda Grupo.

Métricas exibidas:

- Total;
- Quebras;
- Recuperação;
- IND Quebra;
- ADE Checklist.

O tooltip de cada item apresenta os indicadores consolidados e a evolução completa dos últimos 20 dias disponíveis no recorte:

```text
DIA        | QUEBRA  | ADERÊNCIA
10/10/2026 |   22,2% |     25,9%
11/10/2026 |   30,0% |     32,4%
```

### P&R: Análise Crítica

A análise automática considera:

- situação das metas;
- tendência temporal;
- relação entre checklist e quebra;
- principais causas raiz;
- concentração de quebras;
- hierarquias e áreas prioritárias;
- volume necessário para recuperação.

A versão nominal também analisa os **Top 5 logins de vendedores por volume de quebra**, informando:

- `CD_LOGIN_VENDEDOR`;
- equipe de vendas predominante;
- canal de venda;
- Total;
- Quebras;
- Recuperação;
- participação nas quebras;
- IND Quebra;
- ADE Checklist;
- três principais `DSC_BAIXA`.

Registros sem login identificado são apresentados como alerta de qualidade cadastral.

---

## Filtros

O dashboard oferece filtros encadeados por:

- Grupo;
- UF;
- Cluster;
- Cidade;
- Ano/Mês;
- Grupo OS;
- Tipo OS;
- Novo Domicílio;
- Grupo de Canais;
- Canal Grupo;
- intervalo de datas.

O intervalo de datas é recalculado conforme o **Ano/Mês** e as datas disponíveis na base. Todos os componentes do painel respeitam o período e os filtros selecionados.

### Agrupamento de canais

| Grupo | Canais incluídos |
|---|---|
| AA | Agente Autorizado |
| PAP | Porta Porta Exclusivo e Porta Porta Indireto |
| Lojas | Lojas Próprias |
| Remotos | Ativo, Canais Eletrônicos, DAC e Receptivo |
| Varejo | Varejo |
| PME | Canais Exclusivos Empresas |
| Outros | Demais canais |

---

## Exportações

### Exportar PNG

Gera uma imagem do dashboard com os filtros ativos.

Quando um Grupo de Canais estiver selecionado, o grupo será incluído no nome:

```text
Dashboard_Quebra_Agenda_v3_AA.png
Dashboard_Quebra_Agenda_v3_PAP.png
Dashboard_Quebra_Agenda_v3_PME.png
```

Sem seleção específica:

```text
Dashboard_Quebra_Agenda_v3.png
```

### Exportar XLSX

A tabela hierárquica pode ser exportada para Excel, respeitando:

- hierarquia selecionada;
- filtros ativos;
- intervalo de datas;
- pesquisa aplicada;
- ordenação atual.

Exemplos:

```text
Analitico_Indicador_Cidade.xlsx
Analitico_Indicador_Cidade_PAP.xlsx
Analitico_Indicador_Equipe_de_Vendas_PME.xlsx
```

### Baixar HTML

Gera uma nova cópia do dashboard com a base mais recente incorporada no próprio HTML.

---

## Estrutura recomendada do repositório

```text
/
├── index.html
├── IND_QUEBRA.xlsx
└── README.md
```

Renomeie o dashboard para `index.html` para publicação direta pelo GitHub Pages.

O caminho de leitura configurado no HTML é:

```javascript
const REPO_EXCEL_PATH = "./IND_QUEBRA.xlsx";
```

Portanto, `index.html` e `IND_QUEBRA.xlsx` devem permanecer na mesma pasta.

---

## Publicação no GitHub Pages

1. Crie ou acesse o repositório no GitHub.
2. Adicione os arquivos:
   - `index.html`;
   - `IND_QUEBRA.xlsx`;
   - `README.md`.
3. Abra **Settings > Pages**.
4. Em **Build and deployment**, selecione **Deploy from a branch**.
5. Escolha a branch, normalmente `main`, e a pasta `/root`.
6. Salve a configuração.
7. Acesse a URL publicada pelo GitHub Pages.

Ao substituir o Excel no repositório:

1. mantenha o nome `IND_QUEBRA.xlsx`;
2. confirme o commit da nova versão;
3. abra ou atualize o dashboard com `F5`;
4. se necessário, use **Atualizar do repositório**.

O carregamento remoto possui timeout configurado de **30 segundos**. Se a leitura falhar, o painel mantém a última base disponível na sessão ou utiliza a base incorporada.

---

## Estrutura mínima esperada do Excel

### Guia CIDADES

Campos utilizados:

```text
CD_IBGE
NM_GRUPO
UF
NM_CLUSTER
NM_CIDADE
NM_SEGMENTO_MUNICIPIO
```

### Guia CONTRATOS

Campos utilizados:

```text
DT
NR_SOLICITACAO
NM_EQUIPE_VENDA
NM_CANAL_VENDA_SUBGRUPO
NM_CANAL_VENDA_GRUPO
CD_LOGIN_VENDEDOR
COD_MUNICIPIO
QT
```

### Guia QUEBRA

Campos utilizados:

```text
NR_SOLICITACAO
ID_TIPO_OS
TIPO_OS
CD_IBGE
DT_AGENDA
NR_CONTRATO
NM_NOVO_DOMICILIO
COD_BAIXA
DSC_BAIXA
GRUPO_OS
QUEBRA
TOTAL
N
```

A remoção ou alteração dos nomes dessas colunas pode impedir a atualização correta do dashboard.

---

## Atualização da base

### Pelo repositório

Substitua `IND_QUEBRA.xlsx` por uma nova versão com o mesmo nome e estrutura. Depois atualize a página ou use o botão de atualização do repositório.

### Pelo navegador

Quando disponível na interface, **Substituir Excel** permite selecionar uma planilha local sem recarregar a página.

Após validar a nova base, utilize **Baixar HTML** para gerar uma cópia com o XLSX bruto incorporado em Base64.

---

## Tecnologias

- HTML5;
- CSS responsivo;
- JavaScript;
- SheetJS/XLSX para leitura e exportação de Excel;
- html2canvas para exportação PNG;
- SVG para gráficos;
- armazenamento local do navegador para preferências da interface.

---

## Execução local

O arquivo pode ser aberto diretamente no navegador. Contudo, navegadores podem restringir a leitura automática de arquivos locais por políticas de segurança.

Para simular o comportamento publicado, execute um servidor local na pasta do projeto. Exemplo com Python:

```bash
python -m http.server 8000
```

Depois acesse:

```text
http://localhost:8000
```

---

## Limitações conhecidas

- A atualização automática depende da disponibilidade de `IND_QUEBRA.xlsx` na mesma origem do HTML.
- A planilha deve manter as guias e os campos esperados.
- Registros sem correspondência na chave `NR_SOLICITACAO + IBGE` são classificados como **Não identificado**.
- O relacionamento nominal de vendedores depende do preenchimento de `CD_LOGIN_VENDEDOR` em CONTRATOS.
- As bibliotecas externas precisam estar acessíveis para determinadas funções, como exportação PNG ou substituição de Excel.
- O dashboard apresenta associação operacional entre checklist e quebra, não uma comprovação estatística de causalidade.

---

## Boas práticas de manutenção

- Preserve o nome `IND_QUEBRA.xlsx`.
- Evite renomear guias ou colunas.
- Valide datas em `DT_AGENDA` e `DT`.
- Garanta que `QUEBRA`, `TOTAL` e `N` sejam numéricos.
- Monitore chaves sem correspondência entre QUEBRA e CONTRATOS.
- Monitore registros sem `CD_LOGIN_VENDEDOR`.
- Faça commit conjunto do HTML, Excel e README quando houver mudança estrutural.
- Não publique dados sensíveis em repositórios públicos.

---

## Segurança e privacidade

A base pode conter informações operacionais e identificadores internos. Recomenda-se utilizar:

- repositório privado;
- GitHub Enterprise, quando aplicável;
- controle de acesso adequado;
- revisão de dados antes da publicação;
- anonimização de logins quando o painel for compartilhado externamente.

---

## Versão

**Dashboard de Quebra de Agenda v3**

Principais recursos desta versão:

- Quebra x ADE Checklist;
- comparativos D-1 e D-7;
- filtro global de datas;
- visão Dia/Semana/Mês;
- Pareto de causa raiz;
- analítico por hierarquia;
- exportação XLSX e PNG;
- análise nominal de logins, equipes e causas de baixa;
- carregamento de Excel pelo repositório;
- base de contingência incorporada ao HTML.
