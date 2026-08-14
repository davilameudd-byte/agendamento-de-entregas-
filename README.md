# Torre de Controle de Entregas — Almoxarifado

Painel único em HTML/JS (sem backend) para gestão de críticos, sobreestoque e agendamento de
entregas do almoxarifado, com dados sincronizados pela própria API do GitHub e eventos reais
no Google Agenda.

## Como publicar (GitHub Pages)

1. Crie um repositório novo no GitHub (pode ser privado).
2. Suba o arquivo `index.html` deste pacote para a raiz do repositório (pela interface web:
   "Add file → Upload files").
3. Vá em **Settings → Pages**, em "Source" escolha a branch (ex: `main`) e pasta `/ (root)`,
   salve. Em alguns minutos o GitHub mostra a URL pública (algo como
   `https://SEU_USUARIO.github.io/SEU_REPOSITORIO/`).

## Como ativar a sincronização entre a equipe (GitHub)

Sem isso, cada pessoa vê os dados só no próprio navegador.

1. No GitHub, vá em **Settings (da sua conta) → Developer settings → Fine-grained tokens →
   Generate new token**.
2. Dê um nome, defina expiração, e em "Repository access" escolha **apenas este repositório**.
3. Em "Permissions", dê acesso de **leitura e escrita em "Contents"** (nada além disso).
4. Copie o token gerado.
5. Abra o painel publicado → aba **Configurações** → seção "Sincronização com GitHub" →
   preencha usuário/organização, nome do repositório, branch (`main`) e cole o token → Salvar.
6. Repita esse passo em cada navegador/computador da equipe que for usar o painel, sempre
   apontando para o mesmo repositório.

O token fica salvo apenas no `localStorage` do navegador de cada pessoa — nunca é enviado a
lugar nenhum além da API do próprio GitHub.

## Como ativar a criação de eventos no Google Agenda

1. Acesse o [Google Cloud Console](https://console.cloud.google.com/), crie um projeto (ou
   use um existente).
2. Em **APIs e Serviços → Biblioteca**, ative a **Google Calendar API**.
3. Em **APIs e Serviços → Tela de consentimento OAuth**, configure como "Externo" (ou
   "Interno" se for Google Workspace) e adicione os e-mails da equipe como usuários de teste,
   se necessário.
4. Em **APIs e Serviços → Credenciais → Criar credenciais → ID do cliente OAuth**, tipo
   "Aplicativo da Web".
5. Em **Origens JavaScript autorizadas**, adicione a URL exata do GitHub Pages (ex:
   `https://SEU_USUARIO.github.io`).
6. Copie o **Client ID** gerado.
7. No painel → aba **Configurações** → seção "Conexão com Google Agenda" → cole o Client ID →
   Salvar → clique em **Conectar Google Agenda** e autorize com a conta Google desejada.

A conexão com o Google expira depois de um tempo (padrão ~1h) — é só clicar em
"Conectar Google Agenda" de novo quando precisar confirmar um agendamento.

## Estrutura de dados no repositório

Depois de configurada a sincronização, o painel cria/atualiza estes arquivos automaticamente:

```
data/agendamentos.json        # todos os agendamentos de entrega e revisões de sobreestoque
data/dados-consolidados.json  # dados de estoque/trânsito/consumo importados manualmente
data/config.json              # participantes padrão, local e duração dos eventos
```

Cada atualização gera um commit no repositório — dá pra ver o histórico normalmente pelo GitHub.

## Atualização mensal dos dados

Na aba **Atualizar Dados**, cole as três exportações do sistema (mesmo formato de sempre) e
clique em processar — os números de trânsito, estoque e consumo médio são recalculados e
publicados para toda a equipe.
