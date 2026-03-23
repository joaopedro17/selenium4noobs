# 5.1 - Selenium Grid

O **Selenium Grid** permite executar testes em múltiplas máquinas e navegadores ao mesmo tempo, de forma distribuída. É a solução oficial do Selenium para escalar a execução de testes além de uma única máquina.

## Por que usar o Selenium Grid?

- **Velocidade:** Executa vários testes em paralelo, reduzindo drasticamente o tempo total de execução.
- **Cobertura de browsers:** Testa em diferentes navegadores e sistemas operacionais simultaneamente.
- **Escalabilidade:** Adicione mais nós (nodes) conforme a suíte de testes cresce.
- **Integração com nuvem:** Pode ser combinado com serviços como BrowserStack e Sauce Labs.

## Arquitetura do Grid

O Selenium Grid 4 é composto por:

| Componente | Função |
|------------|--------|
| **Hub / Router** | Recebe as requisições de teste e as distribui para os nodes disponíveis |
| **Node** | Máquina que executa os testes em um ou mais navegadores |
| **Session Map** | Registra qual sessão está em qual node |
| **Distributor** | Decide qual node receberá cada nova sessão |

## Iniciando o Grid (modo standalone)

O modo **standalone** combina Hub e Node em um único processo — ideal para começar:

```bash
# Baixe o selenium-server.jar em https://github.com/SeleniumHQ/selenium/releases
java -jar selenium-server-<versao>.jar standalone
```

O Grid ficará disponível em `http://localhost:4444`.

## Conectando o teste ao Grid

Em vez de instanciar `new ChromeDriver()`, use `RemoteWebDriver` apontando para o Grid:

```java
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeOptions;
import org.openqa.selenium.remote.RemoteWebDriver;

import java.net.URL;

public class GridExample {
    public static void main(String[] args) throws Exception {
        ChromeOptions options = new ChromeOptions();

        // URL do Hub do Selenium Grid
        WebDriver driver = new RemoteWebDriver(
            new URL("http://localhost:4444"),
            options
        );

        driver.get("https://exemplo.com");
        System.out.println("Título: " + driver.getTitle());

        driver.quit();
    }
}
```

## Grid com múltiplos nodes (Hub + Node separados)

```bash
# Terminal 1: inicia o Hub
java -jar selenium-server-<versao>.jar hub

# Terminal 2: inicia um Node e registra no Hub
java -jar selenium-server-<versao>.jar node --hub http://localhost:4444
```

## Próximos passos

Com o Grid configurado, você está pronto para explorar testes paralelos — veja o próximo capítulo.

Ir para: [5.2 Testes Paralelos](2-Testes-paralelos.md)
