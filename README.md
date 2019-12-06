# Estrutura mínima de um projeto gradle para uma aplicação Java

Esse repositório contém um esqueleto mínimo para criar uma aplicação Java usando o [gradle](https://docs.gradle.org/current/userguide/userguide.html) por meio de linha de comando. O projeto aqui criado poderá ser aberto em qualquer IDE que tenha suporte a projetos gradle, como [IntelliJ](https://www.jetbrains.com/idea/), [Visual Studio Code](https://code.visualstudio.com/docs/languages/java), [Netbeans](https://netbeans.org/) e [Ecplise](http://www.eclipse.org/).

Segundo a documentação do gradle, a maneira recomendada para executar uma construção gradle é por meio do *script* [gradle wrapper](https://docs.gradle.org/current/userguide/gradle_wrapper.html#gradle_wrapper), que já está contido nesse repositório.

## Baixando o gradle

Caso não deseje instalar o gradle em seu sistema operacional, por exemplo, por meio de gerenciador pacotes como o [apt](https://pt.wikipedia.org/wiki/Advanced_Packaging_Tool) do Linux ou mesmo [SDKMAN](https://sdkman.io) (disponível para Linux e macOS), é possível baixar e instalar o gradle por meio do script gradle wrapper, que está contido nesse repositório. Para instalar o gradle versão 6.0.1, execute (no Linux ou macOS, no windows use o `gradlew.bat`):

```bash
./gradlew wrapper --gradle-version=6.0.1 --distribution-type=bin
```

## Criando um projeto de uma aplicação Java

O gradle possui um grande conjunto de [plugins](https://plugins.gradle.org/), aqui será feito uso do plugin para criar aplicação Java. Para criar uma estrutura básica de uma [aplicação Java](https://docs.gradle.org/current/userguide/build_init_plugin.html), com o nome "PrimeiroExemplo", com um pacote Java `engtelecom.poo` e fazendo uso do framework de teste unidade JUnit4, execute:

```bash
./gradlew init --type java-application --test-framework junit --dsl groovy --project-name "PrimeiroExemplo" --package "engtelecom.poo"
```

### Estrutura de arquivos depois de ter criado uma aplicação Java

Abaixo tem uma listagem de como ficará o diretório depois de ter executado o comando para criar uma aplicação Java. 

```
.
|-- build.gradle
|-- gradle
|   `-- wrapper
|       |-- gradle-wrapper.jar
|       `-- gradle-wrapper.properties
|-- gradlew
|-- gradlew.bat
|-- settings.gradle
`-- src
    |-- main
    |   |-- java
    |   |   `-- engtelecom
    |   |       `-- poo
    |   |           `-- App.java
    |   `-- resources
    `-- test
        |-- java
        |   `-- engtelecom
        |       `-- poo
        |           `-- AppTest.java
        `-- resources
```

Toda configuração do projeto fica dentro arquivo `build.gradle`. Ali poderá adicionar dependências de bibliotecas, alterar o nome da classe que contém o método `main`, etc.  

Note que o gradle criou uma classe exemplo `engtelecom.poo.App.java`, bem como uma classe de teste exemplo `engtelecom.poo.AppTest.java`. Você poderá excluir ou alterar esses arquivos sem problema algum. Contudo, se alterar o nome do arquivo `engtelecom.poo.App.java`, lembre de atualizar o arquivo `build.gradle`, pois há uma referência para essa classe lá.

## Compilando a aplicação

As tarefas que o gradle possui poderão ser listadas com o comando `./gradlew tasks`. Para compilar a aplicação execute a tarefa `build`:
```bash
./gradlew build
```
Será criado um subdiretório `build` onde ficará armazenado o arquivo `.jar` com a aplicação empacotada. Para executar essa aplicação, faça:

```bash
java -cp build/libs/PrimeiroExemplo.jar engtelecom.poo.App
```

## Executando os testes de unidade

Execute a tarefa `./gradlew test` e um relatório em HTML será colocado no subdiretório `build/reports/tests/test`. Se algum teste não passar, então o gradle lançará um aviso após a execução da tarefa. Pore exemplo:
```bash
> Task :test FAILED

engtelecom.poo.AppTest > testAppHasAGreeting FAILED
    org.junit.ComparisonFailure at AppTest.java:12

1 test completed, 1 failed
```

