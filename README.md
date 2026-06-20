# Escala de Julho 2026 — Grupo de Evangelismo

Site estático hospedado no **GitHub Pages**, com dados gravados no **Google Sheets** via Apps Script novo (separado do projeto original).

---

## Estrutura do projeto

```
escala-evangelismo/
├── index.html   ← site (GitHub Pages)
├── Code.gs      ← backend (Google Apps Script novo)
└── README.md
```

---

## Passo 1 — Criar a nova planilha

1. Acesse [sheets.google.com](https://sheets.google.com) e crie uma planilha em branco.
2. Dê o nome que quiser (ex: "Escala Julho 2026 - Web").
3. Copie o **ID da planilha** — é a parte longa da URL entre `/d/` e `/edit`:
   ```
   https://docs.google.com/spreadsheets/d/  ESTE_E_O_ID  /edit
   ```

---

## Passo 2 — Criar o novo Apps Script

1. Acesse [script.google.com](https://script.google.com) e clique em **"Novo projeto"**.
2. Apague o conteúdo padrão e cole todo o conteúdo do arquivo `Code.gs`.
3. Na linha `var SHEET_ID = '...'`, substitua pelo ID copiado no Passo 1.
4. Salve o projeto (Ctrl+S).

---

## Passo 3 — Implantar o Apps Script como Web App

1. Clique em **Implantar → Nova implantação**.
2. Tipo: **Aplicativo da Web**.
3. Configurações:
   - Executar como: **Eu mesmo**
   - Quem tem acesso: **Qualquer pessoa**
4. Clique em **Implantar** e **copie a URL gerada**.

---

## Passo 4 — Colar a URL no index.html

Abra o `index.html` no VSCode e localize esta linha:

```javascript
var SCRIPT_URL = 'COLE_AQUI_A_URL_DO_NOVO_APPS_SCRIPT';
```

Substitua o texto entre aspas pela URL copiada no Passo 3.

---

## Passo 5 — Publicar no GitHub Pages

### Criar o repositório e enviar os arquivos:

```bash
git init
git add .
git commit -m "primeiro commit"
git branch -M main
git remote add origin https://github.com/SEU_USUARIO/escala-evangelismo.git
git push -u origin main
```

### Ativar o GitHub Pages:

1. No repositório → **Settings** → **Pages**.
2. Branch: `main`, pasta: `/ (root)`.
3. Clique em **Save**.
4. Aguarde ~1 minuto. O site estará em:
   `https://SEU_USUARIO.github.io/escala-evangelismo/`

### Atualizações futuras:

```bash
git add .
git commit -m "descrição da mudança"
git push
```

---

## Observação importante

O projeto original no Apps Script **não foi alterado** e continua funcionando normalmente.
Este é um projeto totalmente novo e independente.
