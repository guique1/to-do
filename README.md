# TaskFlow — To-Do List

> Projeto desenvolvido como entrega do **Desafio: IA no Desenvolvimento de Software**  
> Prof. Msc. Alexander Gobbato

![HTML](https://img.shields.io/badge/HTML5-E34F26?style=flat&logo=html5&logoColor=white)
![CSS](https://img.shields.io/badge/CSS3-1572B6?style=flat&logo=css3&logoColor=white)
![JS](https://img.shields.io/badge/JavaScript-F7DF1E?style=flat&logo=javascript&logoColor=black)

---

## 🖥️ Demo

- **Repositório:** `https://github.com/<seu-usuario>/taskflow`
- **Deploy (Vercel):** `https://taskflow-<seu-usuario>.vercel.app`

---

## 📋 Funcionalidades

| Requisito | Status |
|---|---|
| Campo de entrada para descrição | ✅ |
| Seletor de prioridade (Alta / Média / Baixa) | ✅ |
| Lista de exibição dinâmica | ✅ |
| Ação de conclusão (checkbox) | ✅ |
| Estilização condicional por prioridade | ✅ |
| Filtros por prioridade / concluídas | ✅ |
| Persistência via localStorage | ✅ |
| Prevenção de XSS | ✅ |
| Acessibilidade (aria-labels, aria-live) | ✅ |

---

## 🧠 Engenharia de Prompt — Processo Documentado

Abaixo estão os prompts estruturados utilizados em cada etapa, seguindo o modelo **Persona → Contexto → Tarefa → Formato**.

---

### Etapa 1 — Planejamento

**Prompt usado:**
```
Persona: Arquiteto de Software front-end sênior.

Contexto: Estou construindo um To-Do List em HTML, CSS e JavaScript puro (sem frameworks),
como exercício acadêmico que avalia minha capacidade de entender e justificar cada linha de código.
O app precisa: campo de texto, seletor de prioridade (Alta/Média/Baixa), lista dinâmica,
marcar como concluído e estilização condicional por prioridade.

Tarefa: Antes de qualquer código, me ajude a planejar a estrutura de dados do estado da
aplicação e quais funções principais precisarei. Não gere código ainda.

Formato: Lista organizada de decisões de design com justificativa breve para cada uma.
```

**O que aprendi:** Planejar o estado antes de escrever código evita retrabalho. Definir
que cada tarefa seria um objeto `{ id, text, priority, done, createdAt }` antes de começar
tornou todas as funções seguintes previsíveis.

---

### Etapa 2 — Estrutura HTML (Boilerplate)

**Prompt usado:**
```
Persona: Especialista em HTML semântico e acessibilidade web (WCAG 2.1).

Contexto: To-Do List com 3 seções: painel de entrada (input + select + botão),
barra de filtros e lista de tarefas.

Tarefa: Gere apenas o esqueleto HTML com as tags semânticas corretas, atributos
aria e comentários explicando cada seção. Sem nenhum CSS ou JavaScript.

Formato: Apenas o HTML comentado. Explique em 2 linhas por que cada tag semântica
foi escolhida.
```

**O que aprendi:** `<section aria-label>`, `role="toolbar"` e `aria-live="polite"`
não são enfeite — tornam o app usável por leitores de tela. O `aria-live` na lista
anuncia mudanças automaticamente para tecnologias assistivas.

---

### Etapa 3a — Implementação CSS

**Prompt usado:**
```
Persona: Designer de interfaces com foco em design systems e dark mode.

Contexto: App To-Do de tema escuro (#0e0f11 de fundo). Preciso de um sistema de
cores que use CSS Custom Properties para as 3 prioridades e estados interativos.
A estética deve ser "terminal/dev tool" — tipografia monospace, bordas sutis.

Tarefa: Crie as CSS Custom Properties (tokens) para cores, raios e transições.
Depois estilize o card de tarefa com faixa lateral colorida por prioridade e
fundo levemente tingido.

Formato: CSS puro com comentários explicando cada decisão de cor. Sem JavaScript.
```

**O que aprendi:** CSS Custom Properties (variáveis) são a base de qualquer design
system sustentável. Mudar a cor de uma prioridade em um lugar (`--high: #ff4d4d`)
reflete em todos os elementos que a usam. `color-mix()` foi uma descoberta —
permite misturar cores nativamente no CSS sem JavaScript.

---

### Etapa 3b — Implementação JavaScript

**Prompt usado:**
```
Persona: Engenheiro JavaScript focado em código legível e seguro.

Contexto: To-Do List sem frameworks. Estado em array na memória + localStorage.
Funções necessárias: addTask, toggleDone, deleteTask, render, save.

Tarefa: Implemente as 5 funções com foco em:
1. Segurança: prevenir XSS ao exibir texto do usuário no DOM
2. UX: animação de saída ao deletar item
3. Legibilidade: comentário de 1 linha acima de cada bloco lógico

Formato: JavaScript puro com comentários. Explique o risco de XSS e como
escapeHTML() o mitiga.
```

**O que aprendi:** Injetar `innerHTML` diretamente com input do usuário é uma
vulnerabilidade XSS real. A função `escapeHTML()` substitui `<`, `>`, `&` e `"`
por entidades HTML, impedindo que texto malicioso como `<script>alert(1)</script>`
seja executado.

---

### Etapa 4 — Revisão e Melhorias

**Prompt usado:**
```
Persona: Revisor de código sênior especializado em acessibilidade e boas práticas.

Contexto: Código de To-Do List finalizado (colei o HTML+CSS+JS completo aqui).

Tarefa: Analise o código e aponte:
- Problemas de acessibilidade
- Possíveis bugs ou edge cases não tratados
- Oportunidades de melhoria de organização

Formato: Lista priorizada por impacto (Alto / Médio / Baixo).
```

**Melhorias aplicadas após revisão:**
- Adicionado `aria-live="polite"` na lista e no contador de pendentes
- Adicionado `escapeHTML()` para prevenir XSS
- Input limitado a 140 caracteres (`maxlength`)
- Foco devolvido ao input após adicionar tarefa (fluxo contínuo)
- Feedback visual (borda vermelha) para campo vazio

---

## 🏗️ Estrutura do Projeto

```
taskflow/
├── index.html      # App completo (HTML + CSS + JS em arquivo único)
└── README.md       # Esta documentação
```

---

## 💡 Reflexão Crítica

### O que a IA fez bem
- Sugeriu a separação entre **estado** (array `tasks`) e **visão** (função `render`)
  antes de eu pedir — padrão MVC em miniatura.
- Apontou o risco de XSS que eu não havia considerado.
- Produziu CSS com Custom Properties desde o início, o que tornou a manutenção trivial.

### Onde precisei pensar por conta própria
- **Animação de saída do item deletado:** a IA gerou um `tasks.filter()` direto,
  sem animação. Tive que solicitar explicitamente o padrão "anima → aguarda → remove".
- **Filtro "Todas" mostrando só pendentes:** a IA interpretou "todas" como
  literalmente todas (incluindo concluídas). Refinei o prompt para deixar claro
  que "Todas" = pendentes, e "Concluídas" é filtro separado.
- **Cor dinâmica do `<select>`:** a IA não sugeriu isso espontaneamente — foi uma
  decisão estética minha, depois confirmada como boa prática pela IA na revisão.

### Lição principal
> A IA é uma ferramenta de amplificação, não de substituição. Ela acelerou a
> implementação em ~60%, mas cada decisão de arquitetura, UX e segurança passou
> pelo meu julgamento. Saber **qual pergunta fazer** foi mais importante do que
> saber **escrever o código**.

---

## 🚀 Como rodar localmente

```bash
# Sem dependências! Basta abrir o arquivo no navegador:
open index.html

# Ou com live server (VS Code):
# Clique com o botão direito → "Open with Live Server"
```

---

## 📦 Deploy na Vercel

1. Suba o código para um repositório GitHub
2. Acesse [vercel.com](https://vercel.com) → **New Project**
3. Importe o repositório
4. Clique em **Deploy** (sem configuração extra necessária)
