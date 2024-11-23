# 4.1 - Page object

Nessa seção vamos entrar em tópicos mais avançados, por conta disso isso é recomendável que os conhecimentos em Java e orientação a objetos já estejam bem consolidados

## Estruturando o código 

Para qualquer projeto moderno hoje em dia precisamos além de garantir que tudo esteja rodando como planejado,  também temos que levar em consideração a manutenibilidade e para isso usamos o Page Object Model (POM) 

## Page Object Model (POM)

O Page Object é um padrão de design amplamente utilizado em automação de testes, especialmente em testes de interface gráfica (GUI). Ele organiza o código de automação separando a lógica de interação com a interface do usuário (UI) dos próprios testes. No Page Object, cada página ou funcionalidade específica de uma aplicação web é representada como uma classe. Esses objetos encapsulam a estrutura da página e seus elementos, bem como as ações possíveis sobre eles.

## Benefícios do Page Object Model

-  Manutenção Facilitada:
    >Se houver alterações no layout ou estrutura da aplicação, você só precisa atualizar o código na classe correspondente, sem necessidade de modificar todos os testes.

-  Reutilização de Código:
	>Elementos e métodos de uma página podem ser reutilizados em diferentes cenários de teste, reduzindo duplicação de código.

-  Legibilidade e Organização:
	>O código fica mais legível, pois os testes se concentram na lógica de validação, enquanto as classes de página lidam com a interação com a interface.

-  Redução de Dependência:
	>A separação entre testes e lógica de interação garante que as mudanças na interface afetem o mínimo possível os testes.

-  Modularidade:
	>Cada página tem sua própria classe, permitindo modularizar e organizar melhor o projeto de automação.

- Escalabilidade:
	>Projetos grandes podem ser escalados com facilidade, pois o padrão facilita a adição de novas páginas ou funcionalidades.

- Facilidade de Leitura por Não-Técnicos:
	>Como os testes são mais claros e descritivos, é mais fácil para stakeholders não-técnicos entenderem o que está sendo testado.

## Como funciona o Page Object Model?

-  Classe como Representação da Página:
    >Cada página ou componente da aplicação é representado por uma classe.
    Por exemplo, uma página de login seria uma classe LoginPage.

-  Elementos como Propriedades:
    >Os elementos da interface (como botões, campos de texto, links, etc.) são definidos como propriedades ou atributos dessa classe.

-  Métodos como Ações:
	>As ações que podem ser realizadas na página (como clicar em um botão ou preencher um campo de texto) são implementadas como métodos da classe.

-  Separação de Lógica:
	>A lógica de automação (como asserções ou validações) é mantida separada no código de teste, enquanto a lógica de interação com a interface é encapsulada na classe de página.

## Exemplos em código

### Classe da Página

A classe `LoginPage` representa a página de login, encapsulando os elementos e as ações possíveis:

```Java
import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;

public class LoginPage {
    private WebDriver driver;

    // Locators
    private By usernameField = By.id("username");
    private By passwordField = By.id("password");
    private By loginButton = By.id("loginBtn");

    // Constructor
    public LoginPage(WebDriver driver) {
        this.driver = driver;
    }

    // Actions
    public void enterUsername(String username) {
        driver.findElement(usernameField).sendKeys(username);
    }

    public void enterPassword(String password) {
        driver.findElement(passwordField).sendKeys(password);
    }

    public void clickLogin() {
        driver.findElement(loginButton).click();
    }
}

```

### Classe de Teste

A classe de teste utiliza a página encapsulada para executar as ações e realizar validações:

```Java
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;

public class LoginPageTest {

    private WebDriver driver;
    private LoginPage loginPage;

    public void setUp() {
        // Configuração do WebDriver (ajuste o caminho do driver para sua configuração local)
        System.setProperty("webdriver.chrome.driver", "caminho/do/chromedriver");
        driver = new ChromeDriver();
        driver.get("https://exemplo.com/login");
        
        // Inicialização da classe LoginPage
        loginPage = new LoginPage(driver);
    }

    public void testLoginSuccessfully() {
        // Ações no POM
        loginPage.enterUsername("test_user");
        loginPage.enterPassword("password123");
        loginPage.clickLogin();

        // Validação
    }

    public void tearDown() {
        // Fecha o navegador
        if (driver != null) {
            driver.quit();
        }
    }
}

```

Ir para: [4.2 Driver factory](2-Driver-factory.md)
