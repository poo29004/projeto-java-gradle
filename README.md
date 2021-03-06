# Estrutura mínima de um projeto gradle para uma aplicação Java

Esse repositório contém um esqueleto mínimo para criar uma aplicação Java usando o [gradle](https://docs.gradle.org/current/userguide/userguide.html) por meio de linha de comando. O projeto aqui criado poderá ser aberto em qualquer IDE que tenha suporte a projetos gradle, como [IntelliJ](https://www.jetbrains.com/idea/), [Visual Studio Code](https://code.visualstudio.com/docs/languages/java), [Netbeans](https://netbeans.org/) e [Ecplise](http://www.eclipse.org/).

Segundo a documentação do gradle, a maneira recomendada para executar uma construção gradle é por meio do *script* [gradle wrapper](https://docs.gradle.org/current/userguide/gradle_wrapper.html#gradle_wrapper), que já está contido nesse repositório.

## Baixando o gradle

Caso não deseje instalar o gradle em seu sistema operacional, por exemplo, por meio de gerenciador pacotes como o [apt](https://pt.wikipedia.org/wiki/Advanced_Packaging_Tool) do Linux ou mesmo [SDKMAN](https://sdkman.io) (disponível para Linux e macOS), é possível baixar e instalar o gradle por meio do script gradle wrapper, que está contido nesse repositório. 
Para instalar o gradle versão 6.3, no Linux ou macOS, execute: 

```bash
./gradlew
```
> No Windows use o script `gradlew.bat`

## Criando um projeto de uma aplicação Java

O gradle possui um grande conjunto de [plugins](https://plugins.gradle.org/), aqui será feito uso do plugin para criar aplicação Java. Para criar uma estrutura básica de uma [aplicação Java](https://docs.gradle.org/current/userguide/build_init_plugin.html), com o nome "PrimeiroExemplo", com um pacote Java `engtelecom.poo` e fazendo uso do framework de teste unidade JUnit4, execute:

```bash
./gradlew init
```
e responda:
```
Select type of project to generate:
  1: basic
  2: application
  3: library
  4: Gradle plugin
Enter selection (default: basic) [1..4] 2

Select implementation language:
  1: C++
  2: Groovy
  3: Java
  4: Kotlin
  5: Swift
Enter selection (default: Java) [1..5] 3

Select build script DSL:
  1: Groovy
  2: Kotlin
Enter selection (default: Groovy) [1..2] 1

Select test framework:
  1: JUnit 4
  2: TestNG
  3: Spock
  4: JUnit Jupiter
Enter selection (default: JUnit 4) [1..4] 1

Project name (default: projeto-java-gradle): PrimeiroExemplo

Source package (default: PrimeiroExemplo): engtelecom.poo
```

Ou execute a linha abaixo, caso não queira fazer uso do menu interativo apresentado acima.

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
Será criado um subdiretório `build` onde ficará armazenado o arquivo `.jar` com todas as classes da aplicação empacotadas. 

## Executando a aplicação

### Usando gradle
Para executar essa aplicação usando o gradle, faça:
```bash
./gradlew run
```
### Usando o java

Da forma que o arquivo `.jar` foi gerado, para executar a aplicação é necessário informar o nome da classe que tem o método `main`.

```bash
java -cp build/libs/PrimeiroExemplo.jar engtelecom.poo.App
```

É possível criar um arquivo `.jar` executável de forma que não seja necessário informar o nome da classe que se deseja executar. Para tal, edite o arquivo `build.gradle` e adicione as seguintes linhas logo abaixo do bloco já existente chamado `dependencies`:
```groovy
jar {
    manifest {
        // Classe principal da aplicação
        attributes "Main-Class": "engtelecom.poo.App"
    }
}
```
Por fim, compile o projeto com o comando `./gradlew build` e execute a aplicação com a linha abaixo:
```bash
java -jar build/libs/PrimeiroExemplo.jar
```

Veja mais sobre arquivo manifesto do JAR na [documentação oficial da Oracle](https://docs.oracle.com/javase/tutorial/deployment/jar/manifestindex.html).

## Executando os testes de unidade

Execute a tarefa `./gradlew test` e um relatório em HTML será colocado no subdiretório `build/reports/tests/test`. Se algum teste não passar, então o gradle lançará um aviso após a execução da tarefa. Por exemplo:
```bash
> Task :test FAILED

engtelecom.poo.AppTest > testAppHasAGreeting FAILED
    org.junit.ComparisonFailure at AppTest.java:12

1 test completed, 1 failed
```