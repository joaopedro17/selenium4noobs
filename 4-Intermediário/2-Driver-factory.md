# 4.2 - Driver factory

Driver Factory é um padrão de projeto amplamente utilizado em automação de testes para gerenciar a criação e configuração de drivers de automação, como o WebDriver do Selenium. Ele encapsula a lógica de inicialização do driver e fornece uma maneira centralizada e padronizada de criar instâncias do driver conforme necessário.

## Benefícios da Driver Factory

1. **Manutenção centralizada:** Toda a lógica de configuração do driver fica localizada em um único lugar. Isso facilita mudanças, como atualizar a versão do WebDriver ou modificar a configuração de um navegador.

2. **Reutilização de código:** Evita duplicação de código relacionado à configuração do driver em diferentes testes ou classes de teste.

3. **Flexibilidade:** Permite criar drivers para diferentes navegadores e configurações de maneira dinâmica. Por exemplo, a escolha do navegador pode ser baseada em parâmetros fornecidos por linha de comando, arquivos de configuração ou variáveis de ambiente.

4. **Facilidade na paralelização:** Em testes paralelos, a Driver Factory pode garantir que cada thread receba uma instância isolada do WebDriver.

5. **Melhoria na escalabilidade:** Uma boa implementação de Driver Factory facilita a adaptação do framework para novos requisitos, como suporte a mais navegadores, execução em nuvem (por exemplo, Selenium Grid) ou integração com ferramentas como BrowserStack e Sauce Labs.

6. **Redução de erros:** Centralizar a criação e configuração do driver minimiza a possibilidade de erros em diferentes partes do código, como esquecer de configurar uma propriedade necessária.

## Como funciona a Driver Factory?

A ideia principal de uma Driver Factory é criar uma classe ou conjunto de classes que são responsáveis por:

- **Configurar o WebDriver:** Definir propriedades, como o tipo de navegador (Chrome, Firefox, etc.), resolução da tela, e comportamentos específicos (por exemplo, se o navegador deve ser executado em modo "headless").

- **Gerenciar instâncias do WebDriver:** Criar, reutilizar e destruir instâncias do driver.

- **Manter a consistência:** Assegurar que o driver seja configurado de forma padronizada em toda a suíte de testes.

>Geralmente, a implementação de uma Driver Factory utiliza o padrão de design Singleton ou Factory Method.

## Exemplo simples de implementação

```Java
public class DriverFactory {

    private static ThreadLocal<WebDriver> driver = new ThreadLocal<>();

    public static WebDriver getDriver() {
        if (driver.get() == null) {
            driver.set(createDriver());
        }
        return driver.get();
    }

    private static WebDriver createDriver() {
        String browser = System.getProperty("browser", "chrome");
        switch (browser.toLowerCase()) {
            case "firefox":
                return new FirefoxDriver();
            case "edge":
                return new EdgeDriver();
            case "chrome":
            default:
                return new ChromeDriver();
        }
    }

    public static void quitDriver() {
        if (driver.get() != null) {
            driver.get().quit();
            driver.remove();
        }
    }
}

```

Após a implementação da classe `DriverFactory` basta user da seguinte forma:

```Java
WebDriver driver = DriverFactory.getDriver();
driver.get("https://example.com");
// Realizar as ações do teste
DriverFactory.quitDriver();
```

Ir para: [4.3 Espera implícita e explícita](3-Espera-implicita-e-explicita.md)
