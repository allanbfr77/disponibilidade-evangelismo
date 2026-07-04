# Disponibilidade — Grupo de Evangelismo

Formulário web para coleta de disponibilidades do **Ministério de Evangelismo & Integração** (Igreja Nova Vida Botafogo). Os voluntários informam em quais dias do mês podem servir; os dados são gravados automaticamente em uma planilha Google Sheets.

---

## Funcionalidades

- Interface responsiva e otimizada para celular
- Seleção de nome em lista pré-definida (cada pessoa só pode enviar uma vez)
- Calendário interativo com destaque nos dias de evangelismo
- Disponibilidade por turno: quartas (18h–19h30), sábados e domingos (17h–18h30)
- Opção de marcar indisponibilidade total no mês
- Janela de envio automática: liberada do dia **22** ao **28**; encerrada a partir do dia **29**
- Sincronização em tempo real dos nomes já utilizados (polling a cada 60 s)
- Backend com fallback JSONP para maior compatibilidade

---

## Arquitetura

```
┌─────────────────┐       HTTPS        ┌──────────────────────┐
│   index.html    │  ◄──────────────►  │  Google Apps Script  │
│  (GitHub Pages) │                    │      (Code.gs)       │
└─────────────────┘                    └──────────┬───────────┘
                                                  │
                                                  ▼
                                       ┌──────────────────────┐
                                       │   Google Sheets        │
                                       │  • Disponibilidades    │
                                       │  • Escala              │
                                       └──────────────────────┘
```

| Camada | Tecnologia |
|--------|------------|
| Frontend | HTML, CSS e JavaScript (página única) |
| Hospedagem | GitHub Pages + domínio customizado (`CNAME`) |
| Backend | Google Apps Script (Web App) |
| Banco de dados | Google Sheets |

---

## Estrutura do repositório

```
disponibilidade-evangelismo/
├── index.html   # Interface do formulário
├── Code.gs      # Backend (implantar no Google Apps Script)
├── CNAME        # Domínio customizado para GitHub Pages
└── README.md
```

---

## Configuração inicial

### 1. Criar a planilha Google Sheets

1. Acesse [sheets.google.com](https://sheets.google.com) e crie uma planilha em branco.
2. Copie o **ID da planilha** — trecho da URL entre `/d/` e `/edit`:

   ```
   https://docs.google.com/spreadsheets/d/ESTE_E_O_ID/edit
   ```

### 2. Configurar o Google Apps Script

1. Acesse [script.google.com](https://script.google.com) e clique em **Novo projeto**.
2. Apague o conteúdo padrão e cole todo o conteúdo de `Code.gs`.
3. Na variável `SHEET_ID`, substitua pelo ID copiado no passo anterior.
4. Salve o projeto (Ctrl+S).

### 3. Implantar como Web App

1. Clique em **Implantar → Nova implantação**.
2. Tipo: **Aplicativo da Web**.
3. Configurações:
   - **Executar como:** Eu mesmo
   - **Quem tem acesso:** Qualquer pessoa
4. Clique em **Implantar** e copie a URL gerada (termina em `/exec`).

### 4. Conectar o frontend ao backend

No `index.html`, localize e atualize a variável `SCRIPT_URL`:

```javascript
var SCRIPT_URL = 'https://script.google.com/macros/s/SUA_URL_AQUI/exec';
```

### 5. Publicar no GitHub Pages

1. Envie os arquivos para o repositório na branch `main`.
2. Em **Settings → Pages**, selecione branch `main` e pasta `/ (root)`.
3. (Opcional) Para domínio próprio, o arquivo `CNAME` já aponta para `evangelismo.invbotafogo.com.br` — configure o DNS conforme a [documentação do GitHub](https://docs.github.com/pt/pages/configuring-a-custom-domain-for-your-github-pages-site).

---

## API do Apps Script

| Ação (`?action=`) | Método | Descrição |
|-------------------|--------|-----------|
| `getUsedNames` | GET | Lista nomes que já enviaram disponibilidade |
| `getAll` | GET | Retorna todas as disponibilidades registradas |
| `getSchedule` | GET | Retorna a escala montada |
| `setSchedule` | GET | Define a escala (`sched` em JSON) |
| `addPerson` | GET | Adiciona pessoa à escala (`date`, `name`) |
| `removePerson` | GET | Remove pessoa da escala (`date`, `name`) |
| `clearSchedule` | GET | Limpa toda a escala |
| *(padrão)* | GET/POST | Registra disponibilidades (`data` em JSON) |

### Formato de envio de disponibilidade

```json
[
  ["NOME", "08/jul", "Disponivel"],
  ["NOME", "04/jul", "Indisponivel"]
]
```

A planilha **Disponibilidades** é criada automaticamente com as colunas de data e um carimbo de horário na coluna **Recebido em**.

---

## Atualizações futuras

Para publicar alterações no site:

```bash
git add .
git commit -m "descrição da mudança"
git push
```

O GitHub Pages atualiza automaticamente em cerca de 1 minuto.

> **Importante:** ao alterar `Code.gs`, é necessário criar uma **nova implantação** no Apps Script (ou atualizar a implantação existente) para que as mudanças entrem em vigor.

---

## Licença

Uso interno do Ministério de Evangelismo & Integração — Igreja Nova Vida Botafogo.
