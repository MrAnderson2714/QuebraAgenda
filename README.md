# Dashboard Novos Domicílios Claro

Dashboard executivo e operacional para acompanhamento de **Quebra**, **Antiquebra** e **Conversão**, desenvolvido em HTML responsivo e interativo para publicação no GitHub Pages.

## Acesso

O painel é publicado pelo arquivo `index.html` do repositório. Após habilitar o GitHub Pages, utilize a URL gerada para o projeto.

## Bases de dados

O dashboard lê os arquivos abaixo na mesma pasta do HTML:

```text
index.html
quebra.json ou quebra.json.gz
nd.json ou nd.json.gz
ibge.json ou ibge.json.gz
metas.json
README.md
```

- `quebra.json`: dados de Quebra e Antiquebra;
- `nd.json`: dados de Conversão e Linha de Negócio;
- `ibge.json`: enriquecimento geográfico;
- `metas.json`: metas dos indicadores.

O carregamento utiliza primeiro a versão `.json.gz` e, caso não esteja disponível, tenta o arquivo `.json`.

## Abas do dashboard

### Quebra

Apresenta rotas, quebras, percentual de quebra, ADE Checklist, recuperação necessária, evolução temporal, Pareto de motivos, análise por hierarquia e heatmap por código de reagendamento.

Principais fórmulas:

```text
% Quebra = SUM(QB) / SUM(RT)
ADE Checklist = SUM(D1) / SUM(RT)
Recuperação = MAX(0, CEILING(SUM(QB) - Meta × SUM(RT)))
```

### Antiquebra

Acompanha registros, itens tratados, revertidos, aderência elegível e volume não tratado.

```text
% Tratadas = SUM(AT_TRAT) / SUM(AQ_REG)
% Revertidas = SUM(AQ_REV) / SUM(AQ_REG)
Aderência = SUM(AQ_REG elegível) / SUM(AQ_ADE)
```

### Conversão

Apresenta os indicadores e volumes de Venda Bruta, Venda Líquida, Desistência, Instalações e Instalações em até três dias.

```text
Conversão Bruta = SUM(INS) / SUM(VB)
Conversão Líquida = (SUM(INS) - SUM(DES)) / SUM(VB)
Desistência = SUM(DES) / SUM(VB)
Instalação 3D = SUM(INS_3D) / SUM(INS)
```

## Filtros

Os filtros geográficos e comerciais afetam todas as abas compatíveis. O painel inclui filtros de competência, intervalo de datas, regional, UF, cluster, subcluster, município, canal, grupo, subgrupo, equipe e vendedor.

O filtro **Linha Negócio** utiliza o campo `NM_LINHA_NEGOCIO` do `nd.json` e atua somente na aba Conversão. A configuração inicial é:

- BSOD: desmarcado;
- CLARO EMPRESAS: marcado;
- HOTEL E HOSPITAL: marcado;
- RESIDENCIAL: marcado.

## Recursos

- agrupamento por dia, semana ou mês;
- KPIs com metas e comparativos;
- tooltips detalhados;
- rótulos de percentuais e volumes nos gráficos;
- Pareto de códigos de reagendamento;
- heatmap por hierarquia e motivo;
- busca e ordenação nas tabelas;
- exportação CSV, PNG e HTML;
- modo claro e escuro;
- detalhamento por clique nas linhas das tabelas;
- indicadores e metas carregados pelo `metas.json`.

## Publicação no GitHub Pages

1. Mantenha todos os arquivos na mesma pasta do repositório.
2. Use `index.html` como página principal.
3. Ative o GitHub Pages nas configurações do repositório.
4. Após atualizar os arquivos, aguarde a publicação e recarregue a página com `Ctrl + F5`.

Arquivos maiores que 100 MB podem exigir compactação `.gz` ou Git LFS. A opção recomendada para as bases maiores é manter `quebra.json.gz` e `nd.json.gz` no repositório.

## Execução local

Execute um servidor HTTP na pasta do projeto:

```bash
python -m http.server 8000
```

Acesse:

```text
http://localhost:8000
```

A abertura direta pelo protocolo `file://` pode bloquear a leitura dos arquivos JSON.

## Tecnologias

- HTML5 e CSS responsivo;
- JavaScript;
- Chart.js e chartjs-plugin-datalabels;
- Pako para arquivos GZIP;
- html2canvas;
- SheetJS.

## Segurança

As bases podem conter informações operacionais e identificadores internos. Utilize repositório privado ou GitHub Enterprise, controle as permissões e revise os dados antes de qualquer publicação externa.
