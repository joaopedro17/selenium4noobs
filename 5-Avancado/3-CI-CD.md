# 5.3 - Integração com CI/CD

Integrar testes Selenium a um pipeline de **CI/CD** (Integração Contínua / Entrega Contínua) permite executar a suíte de testes automaticamente a cada push, pull request ou deploy — garantindo que regressões sejam detectadas cedo.

## Por que rodar Selenium no CI?

- Detecta quebras antes de chegar à produção
- Feedback rápido para o time de desenvolvimento
- Elimina a necessidade de rodar testes manualmente antes de cada entrega

## GitHub Actions

O **GitHub Actions** é a opção mais acessível para projetos hospedados no GitHub. Crie o arquivo `.github/workflows/testes.yml`:

```yaml
name: Testes Selenium

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  testes:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout do repositório
        uses: actions/checkout@v4

      - name: Configurar Java 11
        uses: actions/setup-java@v4
        with:
          java-version: '11'
          distribution: 'temurin'

      - name: Instalar Google Chrome
        uses: browser-actions/setup-chrome@v1

      - name: Executar testes com Maven
        run: mvn test
```

> O Selenium Manager (Selenium 4.6+) detecta automaticamente o Chrome instalado e baixa o ChromeDriver correspondente — não é necessária nenhuma configuração extra de driver no CI.

## Headless no CI

Ambientes de CI não têm interface gráfica. Por isso, os testes precisam rodar em modo **headless** (sem janela visível). Veja como configurar isso no próximo capítulo. Por enquanto, uma forma simples é passar opções ao Chrome:

```java
ChromeOptions options = new ChromeOptions();
options.addArguments("--headless=new");
options.addArguments("--no-sandbox");
options.addArguments("--disable-dev-shm-usage");

WebDriver driver = new ChromeDriver(options);
```

> A flag `--no-sandbox` e `--disable-dev-shm-usage` são necessárias em ambientes Linux sem usuário root (como containers Docker e a maioria dos CI runners).

## Jenkins

Se o seu projeto usa Jenkins, adicione um `Jenkinsfile` na raiz:

```groovy
pipeline {
    agent any

    tools {
        jdk 'JDK-11'
        maven 'Maven-3'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Testes') {
            steps {
                sh 'mvn test'
            }
        }
    }

    post {
        always {
            junit '**/target/surefire-reports/*.xml'
        }
    }
}
```

## Publicando relatórios de teste

O Maven Surefire gera relatórios XML em `target/surefire-reports/`. No GitHub Actions, você pode publicar um resumo com:

```yaml
      - name: Publicar resultado dos testes
        uses: dorny/test-reporter@v1
        if: always()
        with:
          name: Resultados JUnit
          path: target/surefire-reports/*.xml
          reporter: java-junit
```

Ir para: [5.4 Execução Headless](4-Headless.md)
