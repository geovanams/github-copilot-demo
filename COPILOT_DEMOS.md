<!-- Generate a focumentation with a list of sample prompt to demo github copilot capacities -->

# Github Copliot demo

## Pré-Requisitos:

- Conta GitHub
- IDE Visual Studio Code
- Licença GitHub Copilot

## VSCode Shortcuts

Depois de começar a digitar um prompt e o copilot gerar propostas, você poderá usar os seguintes atalhos para interagir com o Copilot:
- `tab` para aceitar inteiramente a sugestão atual (`mais comum`)
- `ctrl + seta para a direita` para aceitar palavra por palavra a sugestão (`para uso parcial`)
- `alt + ^` para passar para a próxima sugestão
- `shift + tab` para voltar à sugestão anterior
- `ctrl+enter` para exibir o painel do copiloto

Se você não consegue se lembrar, basta passar o ponteiro sobre uma sugestão para que ela apareça.<br>

# Demos

## Natural Language Translations

**Automate text translation**

- Abra o arquivo `album-viewer/lang/translations.json`
```json
[
    {
        "language": "en",
        "values": {
            "main-title": "Welcome to the world of the future",
            "main-subtitle": "The future is now with copilot",
            "main-button": "Get started"
        }
    }
]
```

- Comece a adicionar um novo bloco adicionando um "," após o último "}" e pressione enter
<br>

## Code Generation

**Generate code from prompt**

- Abra o arquivo `album-viewer/utils/validators.ts`e inicie com o prompt:
```ts
// validate date from text input and convert it to a date object
```

- O Copilot também pode ajudá-lo a escrever `padrões RegExp`. Tente :

```ts
// function that validates the format of a IPV6 address string
```

<br>

**Descubra novas ferramentas e bibliotecas no trabalho com o Copilot**

- No mesmo arquivo `album-viewer/utils/validators.ts` adicione o seguinte prompt:
```ts
// validate phone number from text input and extract the country code
```
Para este provavelmente lhe dará uma proposta de chamar alguns métodos não definidos aqui e que precisavam ser definidos. Você pode explorar as alternativas usando o atalho `ctrl+enter` para exibir mais sugestões.

**Complex algoritms generation**

- No arquivo `albums-api/Controllers/AlbumController.cs` tente completar o método `GetByID` substituindo o retorno atual:

```cs
// GET api/<AlbumController>/5
[HttpGet("{id}")]
public IActionResult Get(int id)
{
    //here
}
```

- Você pode explorar outros prompts:
```cs
// function that search album by name, artist or genre

// function that sort albums by name, artist or genre
```

## Tests

O Copilot pode ajudar a gerar todos os tipos de testes escritos com código. Inclui testes de `unidade, testes de integração, testes ponta a ponta e testes de carga` com scripts jmeters, por exemplo.

- Abra o arquivo `albums-viewer/tests/validators.test.ts`

Para ter uma boa sugestão de teste, você deve fornecer algumas informações básicas ao Copilot, como test framework que deseja usar. Insira no arquivo:

```ts
import { describe }
```

Quando você começar a digitar a função `describe`, o copilot verá que você está no arquivo de teste no TS e sugerirá que você importe as funções `describe` e `it` do Mochai, que é um famoso framework de teste para JS/TS.
Aceite a sugestão e ela irá automaticamente sugerir também a função `expect` do Chai: aceite também.

```ts
import {describe, it} from 'mocha';
import {expect} from 'chai';
```

Você tem sua estrutura de teste implementada! Agora basta importar as funções que deseja testar iniciando uma nova linha pela palavra-chave `import` o copilot verá que está em um arquivo de teste, para testar alguns `validators` por causa do nome e irá sugerir algo assim:

```ts
import {validateAlbumId} from '../src/validators';
```

Dependendo dos arquivos abertos disponíveis e o contexto, possa ser que ele gere o nome e caminho da função incorretamente. Portanto sempre é importante validar as sugestões!
<br>

- Aceite a sugestão e mude o caminho
  
- Adicione um comentário com a primeira função que deseja testar e deixe a mágica acontecer:

```ts
import {describe, it} from 'mocha';
import {expect} from 'chai';

import {validateDate, validateIPV6} from '../utils/validators';

// test the validataDate function
```
Boom!
```ts	
describe('validateDate', () => {
    it('should return a date object when given a valid date string', () => {
        const date = '01/01/2019';
        const expectedDate = new Date(2019, 0, 1);
        expect(validateDate(date)).to.deep.equal(expectedDate);
    });

    it('should throw an error when given an invalid date string', () => {
        const date = '01/01/2019';
        expect(() => validateDate(date)).to.throw();
    });
});
```

*Você pode adicionar outro bloco `it` para adicionar mais casos de teste e também adicionar os testes para as outras funções

## CI pipelines

*O Copilot irá ajudá-lo a escrever seus arquivos de definição de pipeline para gerar o código para as diferentes steps e tasks.. Aqui estão alguns exemplos do que ele pode fazer:*
- *gerar um arquivo de definição de pipeline `do zero`*
- *acelerar a gravação de um arquivo de definição de pipeline `gerando o código` para as diferentes `steps, tasks e partes do script`*
- *ajude a `descobrir tarefas e extensões do marketplace` que atendam às suas necessidades*

### Step 1: gerando do Zero

- No arquivo `.github/workflow/pipeline.yml` insira o seguinte prompt:

```yml
# Github Action pipeline that runs on push to main branch
# Docker build and push the album-api image to ACR
```

*O Copilot irá gerar o pipeline bloco por bloco. Pipelines Yaml, às vezes você precisará pular para uma nova linha para acionar a geração do próximo bloco com mais frequência do que com outro tipo de código.*
* Isso gera uma task com alguns erros provenientes de indentação incorreta ou falta de aspas no nome da task. Você pode corrigir isso facilmente com seu IDE e suas habilidades de desenvolvedor :)*

### Step 2: Adicionar tasks a partir do prompt

- Você provavelmente tem um github action workflow com pelo menos uma task de "login" no registro do contêiner e uma task de "docker build and deploy". Adicione um novo comentário após essas tasks para marcar(tag) a imagem do docker com o ID de execução do github e enviá-lo para o registro:

```yml
# tag the image with the github run id and push to docker hub
```

- Teste outro prompt:
```yml

# deploy the album-api image to the dev AKS cluster
```

### Step 3: Adicionando scripts a partir dos prompts

- O Copilot também é muito útil quando você precisa escrever um script personalizado como no exemplo a seguir:

```yml
# find and replace the %%VERSION%% by the github action run id in every appmanifest.yml file
```

## Infra As Code

O Copilot também pode ajudá-lo a escrever infraestrutura como código. Ele pode gerar código para `Terraform, ARM, Bicep, Pulumi, etc...` e também `arquivos de manifesto do Kubernetes`.

### Bicep
- Abra o arquivo `iac/bicep/main.bicep` in `iac/bicep` e comece a digitar prompts no final do arquivo para adicionar novos recursos:
  
```js
// Container Registry

// Azure Congitive Services Custom Vision resource
```

### Terraform
- Abra o arquivo `iac/app.tf`e comece a digitar prompts no final do arquivo para adicionar novos recursos:

```yml
# Container Registry

# Azure Congitive Services Custom Vision resource
```

## Gerar Comentários Git Commit

- Na aba Soure Control do VS Code selecione o ícone de estrelas para gerar mensagens de commit.


## Code Documentation 

Copilot can understand a natural language prompt and generate code and because it's just language to it, it can also `understand code and explain it in natural language` to help you document your code.

### simple documentation comment

To see that just put you pointer on top of a Class, a method or any line of code and start typing the comment handler for the selected language to trigger copilot. In language like Java, C# or TS for example, just type `// `and let the magic happen.

Here is an example in the `albums-viewer/routes/index.js` file. Insert a line and start typing on line 13 inside the `try block`

```js
router.get("/", async function (req, res, next) {
  try {
    // Invoke the album-api via Dapr
    const url = `http://127.0.0.1:${DaprHttpPort}/v1.0/invoke/${AlbumService}/method/albums`;

```

Continue to play with it and see what happens on other pieces of code.

### standardized documentation comment (JavaDoc, JsDoc, etc...)

For this one, to trigger the documentation comment generation, you need to respect the specific comment format:
-  `/**` (for JS/TS) in the `index.js` file for example
- `///` for C# in the `AlbumController.cs` of the AlbumApi file for example

```cs
/// <summary>
/// function that returns a single album by id
/// </summary>
/// <param name="id"></param>
/// <returns></returns>
[HttpGet("{id}")]
public IActionResult Get(int id)
```


## Code Translation

*Copilot can understand and generate natural languages and code language in both way so by combining everything you can use it to `translate code pieces from a language to another one`*

To translate a piece of code in a specific language, create a new file with the extension of the language you want to translate to (ie: `validator.c` in C language) and try to build the prompt like that:

```c
// translate the following code from typescript to java:
// <code here>
```

And go to a new line to trigger copilot. It will try generate the code in the new language.

```c
// translate the following code from typescript to c:
// function validateDate(date: string): Date {
//     const dateParts = date.split('/');
//     const day = parseInt(dateParts[0], 10);
//     const month = parseInt(dateParts[1], 10) - 1;
//     const year = parseInt(dateParts[2], 10);
//     return new Date(year, month, day);
// }

#include <stdio.h>
#include <stdlib.h>
#include <string.h>

struct Date {
    int day;
    int month;
    int year;
};

struct Date validateDate(char *date) {
    struct Date d;
    char *dateParts[3];
    char *token;
    int i = 0;

    token = strtok(date, "/");
    while (token != NULL) {
        dateParts[i++] = token;
        token = strtok(NULL, "/");
    }

    d.day = atoi(dateParts[0]);
    d.month = atoi(dateParts[1]) - 1;
    d.year = atoi(dateParts[2]);

    return d;
}
```

*It's not working perfectly every time depending on the size and complexity of the code and the language in input the the one in output but i made a lot of test and it's pretty impressive with common languages: `C, Java, C#, TS, Python, Go, VB.Net, F#, Kotlin, etc...`*

*`It's also able to work on more exotic language`: it worked on an language used by a customer that we didn't even found traces on internet. By the way it's not listed in the languages supported by Copilot but it worked. You can't rely only on it to migrate the whole source code without having the developper to understand what happens but it sure will help accelerate the process.*



## Refactoring

TBD





## Writing readme.md and documentation

Copilot is also very powerfull to help you write documentation. It can generate `markdown` and `html` code and accelerate the writing of your readme.md files like for this one for example.

You can show that by creating a new file `demo.md` in the root of the project and start typing the following prompt:

```md
# Github Copilot documentation
This documentation is generated with Github Copilot to show what the tool can do.

##
```

From there by starting a new line with a secondary level title it will start generating the content of the documentation and it will showcase how it will accelerate the documentation writing process.


# Extra: Copilot Labs

WIP

# Extra: Copilot X - Preview

WIP
