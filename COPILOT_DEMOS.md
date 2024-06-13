<!-- Generate a focumentation with a list of sample prompt to demo github copilot capacities -->

# Github Copilot Demos

## Pré-Requisitos:

- Conta GitHub
- IDE com suporte GitHub Copilot (VS Code, Visual Studio, JetBrains, Vim/Neovim) **Para esse laboratório estaremos usando VS Code**
- Clonar repositório:
  
   ```git
   git clone https://github.com/geovanams/github-copilot-demo.git
   cd github-copilot-demo
   code .
   ```
- [Licença GitHub Copilot](https://github.com/settings/copilot)
- [Extensao GitHub Copilot e GitHub Copilot Chat](https://marketplace.visualstudio.com/items?itemName=GitHub.copilot)
  
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

- Comece a adicionar um novo bloco adicionando um "," após o último "}" e pressione enter.

Nesse primeiro momento possa ser que ele não traga de imediato uma sugestão, pois é necessário passar mais contexto. Então comece a inserir um novo objeto com o valor da  propriedade "language" como "pt" e após a vírgula pressione Enter. Exemplo:

  ```json
  {
    "language":"pt",
  }
  ```
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

Você pode explorar as alternativas usando o atalho `ctrl+enter` para exibir mais sugestões.

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
# tag the image with the github run id and push to ACR
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

- Na aba Soure Control do VS Code adicione algum arquivo para stage e selecione o ícone de estrelas para gerar mensagens de commit.


## Code Documentation 

O Copilot pode entender um prompt em linguagem natural e gerar código e, como é apenas uma linguagem, também pode "entender o código e explicá-lo em linguagem natural" para ajudá-lo a documentar seu código.

### simple documentation comment

Para isso, basta colocar o ponteiro sobre uma classe, um método ou qualquer linha de código e começar a digitar o manipulador de comentários do idioma selecionado para acionar o copiloto. Em linguagens como Java, C# ou TS por exemplo, basta digitar `// `.

Aqui está um exemplo no arquivo `albums-viewer/routes/index.js`. Insira uma linha e comece a digitar na linha 13 dentro do `try block`

```js
router.get("/", async function (req, res, next) {
  try {
    // Invoke the album-api via Dapr
    const url = `http://127.0.0.1:${DaprHttpPort}/v1.0/invoke/${AlbumService}/method/albums`;

```

### standardized documentation comment (JavaDoc, JsDoc, etc...)

Para este, para acionar a geração de comentários da documentação, você precisa respeitar o formato específico dos comentários:
-  `/**` (Para JS/TS) no arquivo `albums-viewer/utils/validators.js` por exemplo
- `///` Para C# no arquivo `AlbumController.cs` no método httpGet{id} por exemplo

```cs
/// <summary>
/// function that returns a single album by id
/// </summary>
/// <param name="id"></param>
/// <returns></returns>
[HttpGet("{id}")]
public IActionResult Get(int id)
```

### Markdown e html documentation

Ele pode gerar código markdown e html e acelerar a gravação de seus arquivos readme.md como este, por exemplo.

Você pode testar usando o arquivo demo.md na raiz do projeto e começando a digitar o seguinte prompt:
```md
# Github Copilot documentation
This documentation is generated with Github Copilot to show what the tool can do.

##
```

# Demo - GitHub Copilot Chat

GitHub Copilot é uma IA generativa e, portanto, perfeita para gerar código, mas possui poderosos recursos de análise de seu código que podem ser usados ​​em diversos casos para melhorar a qualidade do código, como: encontrar problemas de segurança, más práticas em seu código e gerar uma correção, refatorar e adicionar comentários ao código legado, gerar testes, etc...

## Iniciar com Copilot Chat

Instale a extensão GitHub Copilot Chat em sua IDE.

Depois que o Copilot Chat estiver configurado, você poderá começar a usá-lo:

- Acessando a visualização de bate-papo na barra de ferramentas esquerda do seu IDE (ícone de bate-papo)
- Pressionando Ctrl + Shift + i atalho para uma pergunta rápida embutida no chat

O primeiro é uma versão fixa, muito útil para manter o chat aberto e tirar dúvidas ao Copilot. O segundo é uma maneira rápida de fazer uma pergunta, obter uma resposta e iniciar comandos.

## Chat View

A visualização de chat oferece uma experiência de chat completa, integrada como qualquer outra visualização de ferramenta em seu IDE. Assim que a visualização estiver aberta, você poderá começar a conversar com o Copilot como seu treinador pessoal de código. Ele mantém o histórico da conversa e você pode fazer perguntas relacionadas às respostas anteriores. Ele também fornece sugestões para perguntas ao longo do caminho. Você pode:

- Faça perguntas gerais sobre codificação em qualquer idioma ou práticas recomendadas
- Peça para gerar ou corrigir o código relacionado ao arquivo atual e injetar o código diretamente no arquivo

- Comece criando um script node:

```
Node script calculator
```
- No código gerado, passe o mouse no topo e clique em add file. Será criado um arquivo com o código gerado. Salve o arquivo.

## Code Explanation and documentation

- Com o arquivo aberto, vá ao chat e peça para ele uma explicação do código, você pode usar prompts como:

```
Can you explain me what this code does?
```
```
Can you generate a function that returns a random number between 1 and 10?
```
- Você também pode selecionar o código inteiro ou apenas um trecho, clicar com botão direito, clique em copilot e então explain. Será gerado uma explicação que aparecerá no chat.

- Com o chat, você também pode pedir para ele gerar documentação:

```
Generate documentation comments for this code
```

## Slash Commands

Para ajudar ainda mais o Copilot a fornecer respostas mais relevantes, você pode escolher um tópico para suas perguntas por meio de “comandos de barra”.

Você pode preceder suas entradas de bate-papo com um nome de tópico específico para ajudar o Copilot a fornecer uma resposta mais relevante. Ao começar a digitar /, você verá a lista de tópicos possíveis:

- `/explain` Explica passo a passo como funciona o código selecionado.
- `/fix` Propõe uma correção para os bugs no código selecionado.
- `/help` Imprime ajuda geral sobre GitHub Copilot.
- `/tests` gera testes unitários para o código selecionado.
- `/vscode` Perguntas sobre comandos e configurações do VS Code.
- `/clear` Limpa a sessão.

Com um arquivo aberto ou código selecionado, teste alguns desses slash.

## Usar Agents

Os agentes são como especialistas que podem ajudá-lo em tarefas específicas. Você pode mencioná-los no chat usando o símbolo @. Como:

- `@workspace`: este agente tem conhecimento sobre o código em seu espaço de trabalho e pode ajudá-lo a navegar nele, encontrando arquivos ou classes relevantes. O agente @workspace usa um meta prompt para determinar quais informações coletar do workspace para ajudar a responder sua pergunta.

- `@vscode`: Este agente conhece comandos e recursos do próprio editor do VS Code e pode ajudá-lo a usá-los.

Abra o GitHub Copilot e faça uma pergunta sobre o projeto usando @Workspace:

```
What is about this project ?
```
Copilot chat também pode te ajudar a criar um projeto completo:

```
@workspace /new create a new asp.net core 6.0 project, with three views Index, Users and products.
```
Ele vai criar uma estrutura de um projeto completo. Você pode clicar em **Create Workspace** e ele criará um workspace completo contendo o código gerado.

## Segurança de código

Copilot pode ajudar a encontrar problemas de segurança em seu código e sugerir correções. Também pode ajudar a encontrar bad practices e corrigi-lás.

Abra o arquivo album-api/Controllers/UnsecuredController.cs e insira o seguinte prompt no chat:
```
Can you check this code for security issues?
```
O Copilot vai retornar os problemas de segurança identificados e sugestões de correção. Mas você também pode pedir especificamente para ele sugerir as correrções:
```
Can you propose a fix?
```

## Refatorando Código

Copilot Chat pode ajudar a refatorar seu código. Pode ajudar com:
`rename variables, extract methods, extract classes, etc...`

- Com o arquivo album-api/Controllers/UnsecuredController.cs aberto, insira no chat:
```
extract methods
```
```
create Async version of each methods when it makes sense
```

## Code Translation
Copilot pode entender e gerar linguagem natural e linguagem de código, combinando você pode usar para `traduzir pedaços de código de una linguagem para outra`.

- Abra o arquivo Validators.ts e solicite ao chat para traduzir para linguagem c:

```
translate to C
```
- Copilot também pode ajudar a traduzir códigos de linguagem legadas, porém como ele usa os repositórios públicos do GitHub, linguagens como Java, Javascript e .NET possuem maiores quantidades de código disponível, então a acertividade de retorno para essas linguagens são maiores.

- Com o arquivo legacy/albums.cbl aberto, vamos traduzir para Python,  digite no chat:
```
translate to python
```

Prontinho! Aqui você viu um pouco de como o GitHub Copilot e GitHub Copilot Chat podem ajudar a impulsionar a produtividade no fluxo de desenvolvimento. Continue a explorar essas funcionalidades para aplicar em seus cenários de desenvolvimento diário!

# Desafio

Executar um mini jogo de pedra, papel e tesoura: (Linguagem de sua escolha)
 - Mostrar quem venceu cada rodadada
 - A cada rodada mostrar o score

**Opcional:** Executar em um container Docker.







