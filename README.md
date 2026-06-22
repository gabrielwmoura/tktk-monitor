# IPM TikTok Monitor

Dashboard estático com duas abas — **Vídeos publicados** e **Comentários** — para monitorar a presença dos alunos da mentoria IPM no TikTok. Os dados vêm de duas planilhas Google Sheets públicas (uma para vídeos, uma para comentários). Sem backend, sem build — basta abrir o link.

---

## 1. Criar e configurar as planilhas Google Sheets

Você vai precisar de **duas planilhas** (ou duas abas dentro do mesmo arquivo, publicadas separadamente — veja o passo 2).

### 1a. Planilha de Vídeos

Configure as colunas **exatamente nesta ordem** na linha 1:

| A | B | C | D | E | F | G | H | I |
|---|---|---|---|---|---|---|---|---|
| semana | aluno | link_aluno | link | data_postagem | resumo | sentimento | alcance | responsavel |

> **Novo:** a coluna `link_aluno` é o link do perfil do aluno na plataforma administrativa da mentoria (`https://administrativo.mentoriaipm.com.br/.../painel`). Quando alguém da equipe **não encontrar** o aluno na plataforma, escreva literalmente `Não encontrado` na célula em vez de deixar em branco — isso deixa claro que já foi procurado e não achado (e não que ainda falta checar). Células vazias também são tratadas como "não encontrado" pelo dashboard.

#### Valores aceitos por coluna

- **link_aluno**: URL do perfil na plataforma, ou `Não encontrado`
- **sentimento**: `Falando bem` / `Falando bem com ressalvas` / `Falando mal` / `Neutro`
- **alcance**: `Alto` / `Médio` / `Baixo`
- **semana**: formato `22/03 a 28/03/2026` (opcional — se a coluna não existir, o dashboard calcula a semana automaticamente a partir de `data_postagem`)
- **data_postagem**: formato `DD/MM/YYYY`

#### Dados iniciais — cole a partir da linha 2

| semana | aluno | link_aluno | link | data_postagem | resumo | sentimento | alcance | responsavel |
|---|---|---|---|---|---|---|---|---|
| 02/02 a 09/02/2026 | Patrícia Camila | Não encontrado | https://vt.tiktok.com/ZSHjajuBG/ | 09/02/2026 | Principal benefício: organização dos estudos. Pontos negativos: custo alto, conteúdos em texto, instabilidades na plataforma. | Falando bem com ressalvas | Alto | Julia Cavalari |
| 02/02 a 09/02/2026 | Med Marioli | https://administrativo.mentoriaipm.com.br/administrativo/dashboard/cmkbkaemc057cs625gqbh94yu/painel | https://vt.tiktok.com/ZSHjmebHA/ | 28/01/2026 | IPM foi uma das melhores coisas que fez. Destaca organização, individualização, papel do anjo e mentor. | Falando bem | Médio | Julia Cavalari |
| 02/02 a 09/02/2026 | Anna Paula Freitas | Não encontrado | https://vt.tiktok.com/ZSHjHxELA/ | 01/03/2026 | Vai continuar no IPM. Fala sobre início da mentoria, simulado inicial e estrutura de anjo e mentor. | Falando bem | Médio | Julia Cavalari |

### 1b. Planilha de Comentários

Configure as colunas **exatamente nesta ordem** na linha 1:

| A | B | C | D | E |
|---|---|---|---|---|
| aluno | link_aluno | link_post | data_postagem | resumo |

- **link_aluno**: mesma regra acima — URL do perfil ou `Não encontrado`
- **link_post**: link do post/vídeo do TikTok onde o aluno comentou
- **data_postagem**: formato `DD/MM/YYYY`
- **resumo**: resumo do que o aluno comentou

#### Dados iniciais — cole a partir da linha 2

| aluno | link_aluno | link_post | data_postagem | resumo |
|---|---|---|---|---|
| Gabrielle raupp | https://administrativo.mentoriaipm.com.br/administrativo/dashboard/cmkbagjfz0069s622wsprolma/painel | https://vt.tiktok.com/ZS97QAvjR/ | 06/05/2026 | Aluna relata experiência negativa inicial com o anjo, refere melhora importante após a troca. |
| Maria Clara | Não encontrado | https://vt.tiktok.com/ZS97QAvjR/ | 06/05/2026 | Aluna relata experiência negativa inicial com o anjo, refere melhora importante após a troca. |

---

## 2. Publicar as planilhas como CSV

Repita esse processo **para cada uma das duas planilhas** (vídeos e comentários):

1. No Google Sheets, clique em **Arquivo → Compartilhar → Publicar na web**
2. Em "Link", selecione a aba com os dados correspondente
3. No formato, selecione **Valores separados por vírgula (.csv)**
4. Clique em **Publicar** e confirme
5. **Copie a URL** gerada — ela tem o formato:
   ```
   https://docs.google.com/spreadsheets/d/SEU_ID/pub?gid=0&single=true&output=csv
   ```

Se vídeos e comentários estiverem em abas diferentes do **mesmo arquivo**, o `gid` na URL muda de uma aba pra outra — confira que copiou o link da aba certa em cada publicação.

---

## 3. Configurar o dashboard

Abra `index.html` em qualquer editor de texto e localize estas duas linhas no topo do `<script>`:

```js
const SHEET_URL_VIDEOS = "COLE_AQUI_A_URL_DO_CSV_DE_VIDEOS";
const SHEET_URL_COMMENTS = "COLE_AQUI_A_URL_DO_CSV_DE_COMENTARIOS";
```

Substitua pelos links copiados no passo anterior:

```js
const SHEET_URL_VIDEOS   = "https://docs.google.com/spreadsheets/d/SEU_ID/pub?gid=0&single=true&output=csv";
const SHEET_URL_COMMENTS = "https://docs.google.com/spreadsheets/d/SEU_ID/pub?gid=123456&single=true&output=csv";
```

Salve o arquivo.

> Se a planilha de comentários ainda não estiver pronta, o dashboard funciona normalmente na aba "Vídeos publicados" e mostra um aviso só dentro da aba "Comentários" — não trava a ferramenta inteira.

---

## 4. Deploy no GitHub Pages

1. Crie um repositório **público** no GitHub (ex: `ipm-tiktok-monitor`)
2. Faça upload dos arquivos `index.html` e `README.md` (ou use `git push`)
3. Nas configurações do repositório, vá em **Settings → Pages**
4. Em "Source", selecione **Deploy from a branch → main → / (root)**
5. Clique em **Save**
6. Aguarde ~1 minuto — o GitHub disponibilizará o link:
   ```
   https://SEU_USUARIO.github.io/ipm-tiktok-monitor/
   ```

---

## 5. Como Julia adiciona novos dados

**Vídeo novo:** abra a planilha de vídeos e adicione uma linha com os dados, incluindo o `link_aluno` (ou `Não encontrado`, se não localizar o aluno na plataforma).

**Comentário novo:** abra a planilha de comentários e adicione uma linha do mesmo jeito.

O dashboard lê as duas planilhas a cada vez que a página é aberta — nenhuma outra ação é necessária.

**Dica:** para uma nova semana, use o mesmo padrão de formato na coluna `semana` (quando existir), por exemplo: `07/04 a 13/04/2026`.

---

## 6. Como o Pedro acessa

O link do GitHub Pages funciona em qualquer navegador, no celular e no computador. Basta acessar:

```
https://SEU_USUARIO.github.io/ipm-tiktok-monitor/
```

O dashboard tem duas abas no topo — **Vídeos publicados** e **Comentários** — e carrega automaticamente os dados mais recentes das duas planilhas a cada acesso. Cada aluno mostra um selo indicando se o link do perfil dele na plataforma foi localizado; o painel "Visão Geral" de cada aba também traz um ranking dos alunos mais ativos.
