# 3.2 - Abrindo navegador

Após o download do driver da sua escolha, podemos criar um script simples para abrir o navegador.

Então vamo lá!

Aqui vou mostrar com chrome mas vale pra todos.

Primeiramente vamos setar o driver do Selenium com o driver baixado, pra isso vamos usar a função do Java `System.setProperty();`

Depois disso, basta chamar o WebDriver e intanciar o chrome driver do Selenium

```Java
public static void main(String[] args) {

        System.setProperty("webdriver.chrome.driver", "C://chromedriver.exe");

        WebDriver driver = new ChromeDriver();
    }
```

### Manipulando a dimenssão do browser

Após o script rodar, você vai perceber que a janela do browser não está maximizada, para maximizar vamos fazer assim.

```Java
public static void main(String[] args) {

        System.setProperty("webdriver.chrome.driver", "C://chromedriver.exe");

        WebDriver driver = new ChromeDriver();

        driver.manage().window().maximize();
    }
```

Ir para: [3.4 Scripts simples](4-Scripts-simples.md)
