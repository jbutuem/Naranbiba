# Playbook do Tour — Fábrica Narandiba · Lesaffre

Playbook de treinamento interno para guias que conduzem tours com clientes e parceiros na nova fábrica da Lesaffre em Narandiba (inauguração: **18 de junho de 2026**).

Site estático, single-file HTML. Sem build, sem dependências.

---

## 🔐 Acesso

O playbook é protegido por senha (treinamento interno).

**Senha:** `TGTLES2026`

Uma vez autenticado no dispositivo, a sessão fica válida por **12 horas** — o guia não precisa redigitar a cada acesso dentro desse período.

> **Importante sobre a segurança:** como este é um site estático, a proteção por senha serve como "tranca simples" para evitar que terceiros acessem o material por link direto. **Não é criptografia real** — a senha está armazenada como hash SHA-256 no próprio HTML. Para um material de treinamento interno (não sigiloso), é adequado. Para requisitos de segurança corporativa mais rigorosos, considere usar o **Vercel Password Protection** (plano Pro) ou autenticação via SSO da Lesaffre.

### Como trocar a senha

1. Gerar o hash SHA-256 da nova senha. No terminal:
   ```bash
   echo -n "NOVA_SENHA" | shasum -a 256
   ```
   Ou em Python:
   ```python
   import hashlib
   hashlib.sha256("NOVA_SENHA".encode()).hexdigest()
   ```
2. No `index.html`, buscar por `PASS_HASH` e substituir pelo novo hash.
3. Commit + push. Vercel redeploya automaticamente.

---

## O que tem aqui

### Modo leitura / estudo (para antes do tour)

- **Visão geral** do tour (6 estações) + 4 pilares da "nova era" (proximidade, qualidade, serviço, inovação)
- **Briefing pré-tour** — quem é o grupo, papel do guia, fio condutor, safety first
- **Mapa** das 6 estações com duração estimada
- **Ficha detalhada de cada estação** — script do especialista, pontos-chave para o guia reforçar, lista de "diga / evite", quiz interativo
- **FAQ** — "e se o cliente perguntar" — 10 perguntas recorrentes com resposta padrão
- **Checklist** do guia (antes / dia / durante / depois) com persistência via `localStorage`

### Modo cola (para usar durante o tour no celular)

Botão laranja flutuante **"modo cola"** abre painel em tela cheia, otimizado para consulta rápida no celular enquanto o guia está conduzindo:

- Abas deslizáveis: **Início · Estação 3 · 4 · 5 · 6 · FAQ**
- Frase-âncora em destaque no topo de cada estação (o que o guia deve falar quase literalmente)
- Resumo técnico do especialista em versão compacta
- Grid "diga / evite" simplificado
- FAQ com 10 respostas em cards curtos
- Navegação por abas ou botões anterior/próxima
- ESC ou X fecha e volta para a página principal

---

## Design

Alinhado ao **Brand Guidelines Lesaffre 2025** — azul ultramarino `#0A3B93` como cor dominante, paleta secundária (taupe brown, passion orange, ruby red, tunic blue, grass green), tipografia Lato + Nothing You Could Do para acentos em script, andorinha como elemento gráfico de fundo. Responsivo, modo impressão otimizado, acessível.

---

## Deploy no Vercel (via GitHub)

### Opção 1 — Deploy direto (mais rápido)

1. Criar um repositório no GitHub (ex.: `lesaffre-playbook-narandiba`)
2. Subir os arquivos:
   ```bash
   git init
   git add .
   git commit -m "Initial playbook"
   git branch -M main
   git remote add origin https://github.com/<seu-usuario>/lesaffre-playbook-narandiba.git
   git push -u origin main
   ```
3. Entrar em [vercel.com/new](https://vercel.com/new), importar o repositório.
4. Na tela de configuração: **não é preciso mudar nada**. Vercel detecta site estático e serve `index.html`.
5. Clicar em **Deploy**. Em ~30 segundos está no ar.

### Opção 2 — Vercel CLI

```bash
npm i -g vercel
cd lesaffre-playbook-narandiba
vercel
```

Aceita os defaults. Pronto.

---

## Estrutura

```
.
├── index.html      # playbook completo (HTML + CSS + JS inline)
├── vercel.json     # headers básicos (opcional)
├── .gitignore      # o que não vai para o Git
└── README.md       # este arquivo
```

---

## Customização rápida

**Paleta de cores**, **tipografia** e **largura máxima**: primeiras linhas do `<style>` no `index.html`, na seção `:root`.

**Conteúdo textual** (textos das estações, FAQ, checklist): HTML direto, sem template engine. Basta editar e dar `git push`.

**Adicionar/remover estação no modo cola:** as abas estão dentro de `<div class="fm-tabs">` e cada painel em `<div class="fm-panel">`. A lista `order` no JS controla a navegação anterior/próxima.

---

## Notas

- Todo o conteúdo textual vem de: briefing de inauguração, PPT "Tour Visita Planta Narandiba" (alinhamento de 01/04/2026) e transcrições da reunião com o diretor da fábrica.
- As cores, tipografia e tratamento da andorinha seguem o Lesaffre Brand Guidelines 2025.
- Este é um material interno de treinamento. Se for subir com URL pública no Vercel, considere adicionar `<meta name="robots" content="noindex">` no `<head>` para não aparecer em buscadores.
- Senha e sessão ficam armazenadas em `localStorage` do navegador — cada dispositivo autentica uma vez.
