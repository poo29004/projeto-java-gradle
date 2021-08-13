# Estrutura mínima de um projeto gradle de uma aplicação Java

Esse repositório contém um esqueleto mínimo de uma aplicação Java que foi gerado com [gradle](https://docs.gradle.org/current/userguide/userguide.html) por meio de linha de comando. O projeto aqui criado poderá ser aberto em qualquer IDE que tenha suporte a projetos gradle, como [IntelliJ](https://www.jetbrains.com/idea/), [Visual Studio Code](https://code.visualstudio.com/docs/languages/java), [Netbeans](https://netbeans.org/) ou [Ecplise](http://www.eclipse.org/). Se desejar, veja a [documentação oficial do gradle](https://docs.gradle.org/current/samples/sample_building_java_applications.html) sobre como criar uma aplicação Java.

## Obtenha e instale o gradle

Na distribuições Linux baseadas no Debian é possível instalar o gradle por meio de gerenciador pacotes como o [apt](https://pt.wikipedia.org/wiki/Advanced_Packaging_Tool), porém geralmente ali encontra-se uma versão não tão atual. Assim, a forma mais recomendada para instalar no Linux seria por meio [SDKMAN](https://sdkman.io) ou do [Snapcraft](https://snapcraft.io). No macOS é possível instalar o gradle por meio do [SDKMAN](https://sdkman.io) ou do [Homebrew](https://brew.sh/index_pt-br). Para instalar no Windows, veja a [documentação oficial do gradle](https://gradle.org/install/).


## Criando um projeto de uma aplicação Java

Neste repositório foi criada uma estrutura básica de uma [aplicação Java](https://docs.gradle.org/current/userguide/build_init_plugin.html), com o nome projeto sendo "primeiro", com o nome do pacote Java sendo `engtelecom.poo` e fazendo uso do framework de teste unidade JUnit Jupiter. O projeto disponível neste repositório foi criado com o gradle 7.1.1 executando a linha abaixo:
```bash
gradle init
```
e foram escolhidas as seguintes opções:
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
  5: Scala
  6: Swift
Enter selection (default: Java) [1..6] 3

Split functionality across multiple subprojects?:
  1: no - only one application project
  2: yes - application and library projects
Enter selection (default: no - only one application project) [1..2] 1

Select build script DSL:
  1: Groovy
  2: Kotlin
Enter selection (default: Groovy) [1..2] 1

Select test framework:
  1: JUnit 4
  2: TestNG
  3: Spock
  4: JUnit Jupiter
Enter selection (default: JUnit Jupiter) [1..4] 

Project name (default: projeto-java-gradle): primeiro      
Source package (default: primeiro): engtelecom.poo
```

Seria possível criar um projeto gradle sem o menu interativo apresentado acima. Abaixo seria a linha necessária para criar tal projeto:

```bash
gradle init --type java-application --test-framework junit-jupiter --dsl groovy --project-name "primeiro" --package "engtelecom.poo"
```

### Estrutura de diretórios e arquivos que foram criados

Abaixo tem uma listagem de como ficará o diretório depois de ter executado o comando para criar uma aplicação Java. 

```
.
├── app
│   ├── build.gradle                       <--- Arquivo com configurações do projeto  
│   └── src
│       ├── main
│       │   ├── java
│       │   │   └── engtelecom
│       │   │       └── poo
│       │   │           └── App.java       <--- Classe com método main
│       │   └── resources
│       └── test
│           ├── java
│           │   └── engtelecom
│           │       └── poo
│           │           └── AppTest.java   <--- Classe com os testes de unidade
│           └── resources
├── gradle
│   └── wrapper
│       ├── gradle-wrapper.jar
│       └── gradle-wrapper.properties
├── gradlew
├── gradlew.bat
└── settings.gradle
```

Toda configuração do projeto fica dentro arquivo [`app/build.gradle`](app/build.gradle). Ali é onde você poderá adicionar dependências de bibliotecas, alterar o nome da classe que contém o método `main`, etc.  

Note que o gradle criou uma classe exemplo [`engtelecom.poo.App.java`](app/src/main/java/engtelecom/poo/App.java), bem como uma classe de teste exemplo [`engtelecom.poo.AppTest.java`](app/src/test/java/engtelecom/poo/AppTest.java). Você poderá excluir ou alterar esses arquivos sem problema algum. Contudo, se alterar o nome do arquivo `engtelecom.poo.App.java`, lembre-se de atualizar o arquivo [`app/build.gradle`](app/build.gradle), pois há lá uma referência para essa classe.

## Compilando a aplicação

> Segundo a documentação do gradle, a maneira recomendada para executar uma construção gradle é por meio do *script* [gradle wrapper](https://docs.gradle.org/current/userguide/gradle_wrapper.html#gradle_wrapper), que já está contido nesse repositório. Os exemplos aqui usam o script `gradlew` que é específico para Linux ou macOS. Se estiver usando Windows, então substitua o `./gradlew` por `gradlew.bat`.


As tarefas que o gradle possui poderão ser listadas com o comando `./gradlew tasks`. Para compilar a aplicação execute a tarefa `build`:
```bash
./gradlew build
```
Será criado um subdiretório `app/build` com os seguintes subdiretórios

- **distributions**
  - Sua aplicação compilada, bem com as bibliotecas que você incluiu na sessão de dependências do arquivo [`app/build.gradle`](app/build.gradle), estarão empacotadas em arquivo `.tar` e em um arquivo `.zip`. Use esses arquivos para distribuir sua aplicação para outras pessoas. Para executar sua aplicação será necessário:
    - Descompactar. Ex: `unzip app.zip`
    - Executar o script presente no diretório `app/bin`. Ex: `./app/bin/app` (no Linux ou macOS) ou `app\bin\app.bat` no Windows
- **libs** 
  - Terá um arquivo `.jar` com sua aplicação compilada, porém este arquivo JAR não contém as bibliotecas que você incluiu na sessão de dependências do arquivo [`app/build.gradle`](app/build.gradle). 

## Executando a aplicação

Para executar essa aplicação usando o gradle, faça:
```bash
./gradlew run
```

Também é possível executar uma aplicação Java com o gradle e passar argumentos de linha de comando. Abaixo um exemplo passando dois argumentos, no caso as palavras `POO` e `IFSC`:
```bash
./gradlew run --args="POO IFSC"
```

### Executando a aplicação sem usar o gradle

Da forma que o arquivo `.jar` foi gerado, para executar a aplicação é necessário informar o nome da classe que tem o método `main`.

```bash
java -cp build/libs/app.jar engtelecom.poo.App
```

É possível criar um arquivo `.jar` de forma que não seja necessário informar o nome da classe que se deseja executar. Para tal, edite o arquivo [`app/build.gradle`](app/build.gradle) e adicione as seguintes linhas logo abaixo do bloco já existente chamado `dependencies`:
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
java -jar build/libs/app.jar
```

Veja mais sobre arquivo manifesto do JAR na [documentação oficial da Oracle](https://docs.oracle.com/javase/tutorial/deployment/jar/manifestindex.html).

## Redirecionamento de entrada

Uma aplicação Java poderia processar um arquivo texto que é enviado para a aplicação por meio do redirecionamento da entrada. Por exemplo, o trecho Java abaixo fará a leitura de todas as linhas do arquivo texto que fora encaminhado e irá imprimir tais linhas:

```java
import java.util.Scanner;

public class Teste{
    public static void main(String[] args){
        Scanner entrada = new Scanner(System.in);

        while(entrada.hasNext()){
            System.out.println(entrada.nextLine());
        }
        entrada.close();
    }
}
```
Supondo que queira enviar o conteúdo do arquivo `aulas.txt` para essa classe `Teste`, então a linha de comando para executar o código seria:

```bash
java Teste < aulas.txt
```

### Redirecionamento da entrada quando a aplicação é executada pelo gradle

No arquivo [app/build.gradle](app/build.gradle) foram adicionadas as seguintes linhas para definir o fluxo de entrada padrão quando for executada a tarefa `run`.

```groovy
// Definindo o fluxo de entrada padrão quando o projeto for executado
run{
 standardInput = System.in
}
```

Supondo que queira enviar o conteúdo do arquivo `aulas.txt` para essa classe `Teste`, então a linha de comando para executar o código seria:

```bash
./gradlew run < aulas.txt
```

## Executando os testes de unidade

Execute a tarefa `./gradlew test` e um relatório em HTML será colocado no subdiretório `app/build/reports/tests/test`. Se algum teste não passar, então o gradle lançará um aviso após a execução da tarefa. Por exemplo:
```bash
> Task :test FAILED

engtelecom.poo.AppTest > testAppHasAGreeting FAILED
    org.junit.ComparisonFailure at AppTest.java:12

1 test completed, 1 failed
```

## Para atualizar a versão do gradle

Veja documentação oficial que está disponível em https://docs.gradle.org/current/userguide/upgrading_version_7.html

```bash
gradle wrapper --gradle-version 7.1.1
```