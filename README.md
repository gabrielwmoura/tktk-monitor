# IPM TikTok Monitor

Dashboard estático para monitorar vídeos de alunos no TikTok falando sobre a mentoria IPM. Os dados vêm de uma planilha Google Sheets pública. Sem backend, sem build — basta abrir o link.

---

## 1. Criar e configurar a planilha Google Sheets

Crie uma nova planilha em [sheets.google.com](https://sheets.google.com) e configure as colunas **exatamente nesta ordem** na linha 1:

| A | B | C | D | E | F | G | H |
|---|---|---|---|---|---|---|---|
| semana | aluno | link | data_postagem | resumo | sentimento | alcance | responsavel |

### Valores aceitos por coluna

- **sentimento**: `Falando bem` / `Falando bem com ressalvas` / `Falando mal`
- **alcance**: `Alto` / `Médio` / `Baixo`
- **semana**: formato `22/03 a 28/03/2026`
- **data_postagem**: formato `DD/MM/YYYY`

### Dados iniciais — cole a partir da linha 2

| semana | aluno | link | data_postagem | resumo | sentimento | alcance | responsavel |
|---|---|---|---|---|---|---|---|
| 02/02 a 09/02/2026 | Patrícia Camila | https://vt.tiktok.com/ZSHjajuBG/ | 09/02/2026 | Principal benefício: organização dos estudos. Pontos negativos: custo alto, conteúdos em texto, instabilidades na plataforma. | Falando bem com ressalvas | Alto | Julia Cavalari |
| 02/02 a 09/02/2026 | Med Marioli | https://vt.tiktok.com/ZSHjmebHA/ | 28/01/2026 | IPM foi uma das melhores coisas que fez. Destaca organização, individualização, papel do anjo e mentor. | Falando bem | Médio | Julia Cavalari |
| 02/02 a 09/02/2026 | Anna Paula Freitas | https://vt.tiktok.com/ZSHjHxELA/ | 01/03/2026 | Vai continuar no IPM. Fala sobre início da mentoria, simulado inicial e estrutura de anjo e mentor. | Falando bem | Médio | Julia Cavalari |
| 02/02 a 09/02/2026 | José Gabriel Corado | https://vt.tiktok.com/ZSHj9BGbV/ | 02/02/2026 | IPM foi divisor de águas. Mesmo sem passar, teve progressão enorme. Organização da rotina foi essencial. | Falando bem | Alto | Julia Cavalari |
| 22/03 a 28/03/2026 | Laura Meirelles | https://vt.tiktok.com/ZSuTG48MN/ | 25/03/2026 | O que mais faz diferença: organização e revisões espaçadas. Quer dermatologia. | Falando bem | Médio | Julia Cavalari |
| 22/03 a 28/03/2026 | Catharina Cunha | https://vt.tiktok.com/ZSuTtLbMC/ | 10/03/2026 | Sempre foi contra mentorias, mas percebeu que faltava estratégia. Agora tem metodologia ativa e flashcards. | Falando bem com ressalvas | Alto | Julia Cavalari |
| 22/03 a 28/03/2026 | Mari Estuda | https://vt.tiktok.com/ZSuTnapH8/ | 13/03/2026 | Teve experiência ruim com outra mentoria. Pesquisou muito, fechou IPM e considera ótimo investimento. | Falando bem com ressalvas | Baixo | Julia Cavalari |

---

## 2. Publicar a planilha como CSV

1. No Google Sheets, clique em **Arquivo → Compartilhar → Publicar na web**
2. Em "Link", selecione a aba com os dados (geralmente "Página1")
3. No formato, selecione **Valores separados por vírgula (.csv)**
4. Clique em **Publicar** e confirme
5. **Copie a URL** gerada — ela tem o formato:
   ```
   https://docs.google.com/spreadsheets/d/SEU_ID/pub?gid=0&single=true&output=csv
   ```

---

## 3. Configurar o dashboard

Abra `index.html` em qualquer editor de texto e localize esta linha no topo do `<script>`:

```js
const SHEET_URL = "COLE_AQUI_A_URL_DO_CSV_DO_GOOGLE_SHEETS";
```

Substitua pelo link copiado no passo anterior:

```js
const SHEET_URL = "https://docs.google.com/spreadsheets/d/SEU_ID/pub?gid=0&single=true&output=csv";
```

Salve o arquivo.

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

## 5. Como Julia adiciona novos vídeos

Basta abrir a planilha Google Sheets e **adicionar uma nova linha** com os dados do vídeo, seguindo o formato das colunas. O dashboard lê a planilha a cada vez que a página é aberta — nenhuma outra ação é necessária.

**Dica:** para uma nova semana, use o mesmo padrão de formato na coluna `semana`, por exemplo: `07/04 a 13/04/2026`.

---

## 6. Como o Pedro acessa

O link do GitHub Pages funciona em qualquer navegador, no celular e no computador. Basta acessar:

```
https://SEU_USUARIO.github.io/ipm-tiktok-monitor/
```

O dashboard carrega automaticamente os dados mais recentes da planilha a cada acesso.
