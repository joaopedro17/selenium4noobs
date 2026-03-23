# Guia de Contribuição

Obrigado pelo interesse em contribuir com o **selenium4noobs**! Este documento explica como participar do projeto de forma organizada.

## Tipos de contribuição bem-vindos

- Correção de erros (typos, links quebrados, código incorreto)
- Melhorias de conteúdo (explicações mais claras, exemplos melhores)
- Novos capítulos ou seções
- Tradução de conteúdo
- Revisão técnica de exemplos de código

## Como contribuir

### 1. Abra uma Issue primeiro (para mudanças grandes)

Para novas seções, mudanças estruturais ou qualquer coisa que leve mais de alguns minutos de revisão, abra uma [Issue](../../issues) descrevendo o que você quer fazer antes de começar. Isso evita trabalho duplicado e permite alinhar a proposta com os mantenedores.

Para correções pequenas (typo, link quebrado), pode ir direto para o Pull Request.

### 2. Faça um Fork e crie um branch

```bash
git clone https://github.com/seu-usuario/selenium4noobs.git
cd selenium4noobs
git checkout -b feature/minha-contribuicao
```

Use nomes de branch descritivos:
- `fix/typo-locators` — para correções
- `feature/modulo-avancado` — para novo conteúdo
- `update/webdriver-manager` — para atualizações de conteúdo existente

### 3. Faça as alterações

#### Padrões de conteúdo

- Todo conteúdo é escrito em **Português (Brasil)**
- Exemplos de código são em **Java**
- Use blocos de código com syntax highlighting: ` ```java `
- Mantenha a numeração e estrutura de pastas existente
- Cada arquivo deve ter um link "Ir para:" ao final apontando para o próximo

#### Padrões de código

- Use a API moderna do Selenium 4 (ex: `Duration.ofSeconds()` em vez de `TimeUnit`)
- Use o Selenium Manager (sem `System.setProperty` manual) quando possível
- Sempre inclua `driver.quit()` ao final dos exemplos
- Inclua os imports necessários em todos os exemplos — o leitor deve poder copiar e rodar

### 4. Commit e Push

```bash
git add nome-do-arquivo.md
git commit -m "fix: corrige descrição do locator By.id em 1-Locators.md"
git push origin feature/minha-contribuicao
```

**Formato do commit (recomendado):**

```
tipo: descrição curta do que foi feito

Tipos: fix, feat, docs, refactor
```

### 5. Abra um Pull Request

- Abra o PR com um título claro
- Descreva o que foi alterado e por quê
- Referencie a Issue relacionada, se houver (`Closes #123`)

## Estrutura do projeto

```
selenium4noobs/
├── 1-Introducao/       # Boas-vindas e comunicação
├── 2-Ambiente/         # Configuração de ambiente (Mac, Windows, Linux)
├── 3-Basico/           # Locators, WebDriver, scripts simples
├── 4-Intermediário/    # Page Object, Driver Factory, waits, JS injection, TestNG
├── 5-Avancado/         # Selenium Grid, paralelismo, CI/CD, headless
├── images/             # Imagens usadas nas páginas
├── pom.xml             # Projeto Maven de exemplo
└── CONTRIBUTING.md     # Este arquivo
```

## Dúvidas

Abra uma [Issue](../../issues) com a label `question` e teremos prazer em ajudar.
