# Dashboard de Quebra de Agenda

Dashboard executivo e operacional para acompanhamento de **Quebra de Agenda** e **Aderência ao Checklist**, desenvolvido em HTML único, responsivo e interativo.

## Acesso ao dashboard

[**Abrir Dashboard de Quebra de Agenda**](https://mranderson2714.github.io/QuebraAgenda/)

> Link direto: https://mranderson2714.github.io/QuebraAgenda/

---

## Visão geral

O painel consolida informações geográficas, comerciais e operacionais para identificar os principais ofensores, causas de baixa, equipes, canais e logins de vendedores que demandam atuação.

O dashboard relaciona três conjuntos de dados do arquivo `IND_QUEBRA.xlsx`:

- **CIDADES**: cadastro geográfico por código IBGE;
- **CONTRATOS**: canal, equipe e login do vendedor;
- **QUEBRA**: agenda, quebra, checklist, tipo de OS e causa de baixa.

O relacionamento principal entre as bases comerciais e operacionais utiliza:

```text
NR_SOLICITACAO + IBGE
```

Quando publicado no GitHub Pages, o dashboard procura automaticamente o arquivo `IND_QUEBRA.xlsx` na mesma pasta do HTML.

---

## Indicadores

### IND Quebra

```text
IND Quebra = SUM(QUEBRA) / SUM(TOTAL)
```

**Meta:** menor ou igual a **20%**.

Faixas:

- `0% a 20%`: verde;
- `acima de 20% a 25%`: amarelo;
- `acima de 25%`: vermelho.

### ADE Checklist

```text
ADE Checklist = SUM(N) / SUM(TOTAL)
```

- `N = 1`: aderente ao checklist;
- `N = 0`: não aderente ao checklist.

**Meta:** maior ou igual a **80%**.

### Recuperação

```text
Recuperação = MAX(0, CEILING(SUM(QUEBRA) - 20% × SUM(TOTAL)))
```

Representa a quantidade estimada de agendas que precisam deixar de quebrar para o indicador retornar à meta.

---

## Recursos do dashboard

### Cards executivos

- Atualização da base;
- IND Quebra;
- ADE Checklist;
- Total de agendas;
- Total de quebras;
- comparativos D-1 e D-7;
- volumes necessários para recuperação das metas.

### Gráfico Quebra x ADE Checklist

- visão por Dia, Semana ou Mês;
- semana de domingo a sábado;
- metas de 20% e 80%;
- comparação com o período anterior;
- detalhamento dos principais canais em tooltip.

### Pareto de Causa Raiz

- ranking por `COD_BAIXA` e `DSC_BAIXA`;
- quantidade de quebras;
- participação da causa;
- distribuição por Grupo de Canais.

### Analítico por Hierarquia

Hierarquias disponíveis:

- UF;
- Cluster;
- Cidade;
- Segmento do Município;
- Equipe de Vendas;
- Canal de Venda Subgrupo;
- Canal de Venda Grupo.

Métricas:

- Total;
- Quebras;
- Recuperação;
- IND Quebra;
- ADE Checklist.

O tooltip apresenta os indicadores e os últimos 20 dias disponíveis para cada item.

### P&R: Análise Crítica

A análise automática considera:

- situação e tendência dos indicadores;
- relação entre checklist e quebra;
- principais causas raiz;
- áreas prioritárias;
- volume necessário para recuperação;
- Top 5 logins de vendedores por volume de quebra;
- equipe e canal predominantes;
- principais `DSC_BAIXA` por login;
- alerta de registros sem login identificado.

---

## Filtros

- Grupo;
- UF;
- Cluster;
- Cidade;
- Ano/Mês;
- intervalo de datas;
- Grupo OS;
- Tipo OS;
- Novo Domicílio;
- Grupo de Canais;
- Canal Grupo.

Todos os KPIs, gráficos, tabelas, análises e exportações respeitam os filtros ativos.

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

### PNG

Exemplos:

```text
Dashboard_Quebra_Agenda_v3.png
Dashboard_Quebra_Agenda_v3_AA.png
Dashboard_Quebra_Agenda_v3_PAP.png
Dashboard_Quebra_Agenda_v3_PME.png
```

### XLSX

A exportação do Analítico por Hierarquia respeita período, filtros, pesquisa e ordenação.

Exemplos:

```text
Analitico_Indicador_Cidade.xlsx
Analitico_Indicador_Cidade_PAP.xlsx
Analitico_Indicador_Equipe_de_Vendas_PME.xlsx
```

### HTML

O botão **Baixar HTML** gera uma nova cópia do painel com a base mais recente incorporada.

---

## Estrutura do repositório

```text
/
├── index.html
├── IND_QUEBRA.xlsx
└── README.md
```

O caminho de leitura configurado no dashboard é:

```javascript
const REPO_EXCEL_PATH = "./IND_QUEBRA.xlsx";
```

O HTML e o Excel devem permanecer na mesma pasta.

---

## Atualização da base

1. Substitua `IND_QUEBRA.xlsx` no repositório.
2. Preserve o nome, as guias e os cabeçalhos esperados.
3. Confirme o commit da nova versão.
4. Atualize a página com `F5` ou use **Atualizar do repositório**.

Se a atualização remota falhar, o painel mantém a última base disponível na sessão ou utiliza a base incorporada ao HTML.

---

## Estrutura mínima do Excel

### CIDADES

```text
CD_IBGE
NM_GRUPO
UF
NM_CLUSTER
NM_CIDADE
NM_SEGMENTO_MUNICIPIO
```

### CONTRATOS

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

### QUEBRA

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

---

## Tecnologias

- HTML5;
- CSS responsivo;
- JavaScript;
- SheetJS/XLSX;
- html2canvas;
- SVG;
- armazenamento local do navegador.

---

## Execução local

```bash
python -m http.server 8000
```

Acesse:

```text
http://localhost:8000
```

---

## Segurança e privacidade

A base pode conter informações operacionais e identificadores internos. Recomenda-se:

- utilizar repositório privado ou GitHub Enterprise;
- controlar permissões de acesso;
- revisar os dados antes da publicação;
- anonimizar logins em compartilhamentos externos;
- evitar a publicação de dados sensíveis em repositórios públicos.

---

## Versão

**Dashboard de Quebra de Agenda v3**

Acesso: [https://mranderson2714.github.io/QuebraAgenda/](https://mranderson2714.github.io/QuebraAgenda/)
