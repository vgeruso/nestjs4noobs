# Controllers

Os controllers são classes responsáveis por receber solicitações e retornar respostas ao cliente atrevés do protocolo `HTTP`.

![controller_scheme](../images/Controllers_1.png)
> Imagem tirada da documentação oficial

O mecanismo de roteamento gerencía qual controller recebe quais solicitações.

Para criar um controller básico utilizamos classes e decorators. os Decorators realizam a função de asociação das classes à metadados necessários que permitem que o Nest crie um mapeamento que vincule o as solicitações aos seus devidos controllers.

## Roteamento

No exemplo a seguir usamos o decorator `@Controller()` ele é necessário para definir o controller básico do módulo, nele especificaremos um prefixo de caminho de rota opcional no caso `user`. O prefixo usado no decorator permite o agurpamento facilitado de um conjunto de rotas minimizando condigos repetitivos. Por exemplo ao indicarmos o prefixo estamos indicando que toda a rota do controller usará o prefixo `/user` isso fará que todas as rotas do módulo User ao serem chamadas tenha o padrão `/user/[rota]` evitando que repitamos esse prefixo todas as vezes que criarmos uma nova rota.

```typescript
import { Controller, Get } from '@nestjs/common';

@Controller('users');
export class UserController {
  @Get()
  index(): string {
    return 'Essa ação retorna todos os usuários';
  }
}
```

O decorator `@Get()` indica o metodo HTTP que corresponde aquela chamada especifica, indica ao Nest a criação do endpoint no qual corresponde ao metodo HTTP e ao caminho da rota. A rota para um manipulador é determinado pela concatenação do do prefixo indicado no decorator `@Controller()` e qual quer caminho especificado no decorator do metodo seja ele `GET`, `POST`, `PUT`, `PATH` ou `DELETE`.

No exemplo como foi declarado um prefixo `user` e não foi adicionado nenhuma informação no caminho do metodo, o Nest mapeará como `GET /user` para cessar o metodo index declarado. Conforme mencionado, caso seja adicionado ao metodo algum informação do caminho por exemplo `@Get('name')` será produzido um mapeamento de rota como `GET /user/name`.

No exemplo quando a solicitação GET é feita para o edpoint, o Nest roteia a solicitação para nosso método definido como `index()`. O nome do método aqui é completamente arbitrário, obviamente é necessário declarar um método ao gerar uma rota, mas o Nest não atribui nenhum significado a o nome escolhido.

Este método retorna um status `200` e a resposta associada, que nesse caso é apena uma string `Essa ação retorna todos os usuários`. Por padrão o Nest trata todo os retornos verificando o tipo, quando as funções retornam como resposta um objeto ou um array JavaScript esse retorno será serializado automaticamente para JSON, mas quando é retornado um tipo primitivo - por exemplo `string`, `number`, `boolean` - o Nest enviará apenas o valor sem tentar serealizá-lo simplificando o tratamento da resposta, para nós basta retornar o valor, o nest cuida do resto. Além disso ele também retorna o código de status que sempre é padronizado para `200` exeto em metodos `POST` que usam `201`.

Este é o comportamento padrão no nest, mas o framework abre a possibilidade de manipular os métodos e retornos através de bibliotecas especificas configuradas como por exemplo o express que pode ser injetado usando o decorator `@Res()` na assinatura do método (por exemplo, `findAll(@Res() response)`). Com esse recurso obtemos a capacidade de usar métodos de manipulção de resposta nativos expostos. Por exemplo como express pode ser construídas respostas usando o código `response.status(200).send()`.

## Object Request

Os manipuladores geralmente precisam acessar os detalhes do request do cliente. O Nest fornece acesso ao objeto de solicitação da plataforma subjscente (Express por padrão). Podendo acessar o objeto de solicitação instruindo o nest a injetá-lo adicionado o decorator `@Req()` à assinatura do manipulador.

```typescript
import {  Controller, Get, Req } from '@nestjs/common';
import {  Request } from 'express';

@Controller('users')
export class UsersController {
  @Get()
  findAll(@Req() request: Request): string {
    return 'Esta ação retorna todos os usuários';
  }
}
```

O object request representa a solicitação HTTP e tem como propriedades para string de consulta da solicitação, parâmetros, cabeçalhos e corpo. Na maioria dos casos, não é necessário pegar essas propriedades manualmente. Podemos usar decorators dedicados, como `@Body()` ou `@Query()`, que estão disponíveis prontos para uso. Abaixo está uma lista dos decorators fornecidos e os objetivos especificos da plataforma simples que eles representam.

| Decorator                | representação                      |
| ------------------------ | ---------------------------------- |
| `@Request()`, `@Req()`   | `req`                              |
| `@Response()`, `@Res()*` | `res`                              |
| `@Next()`                | `next`                             |
| `@Session()`             | `req.session`                      |
| `@Param(key?: string)`   | `req.params` / `req.params[key]`   |
| `@Body(key?: string)`    | `req.body` / `req.body[key]`       |
| `@Query(key?: string)`   | `req.query` / `req.query[key]`     |
| `@Headers(key?: string)` | `req.headers` / `req.headers[key]` |
| `@Ip()`                  | `req.ip`                           |
| `@HostParam()`           | `req.hosts`                        |

Para compatibilidade com tipagens em plataformas HTTP subjscentes (por exemplo, Express e Fastify), o nest fornece os decorators`@Res()` e `@Response()`. `@Res()` é simplesmente um alias para `@Response()`. Ambos expõem diretamente a interface do objeto da plataforma nativa subjacente `response`. Ao usá-los, deve-se importar as tipágens para a biblioteca subjscente (por exemplo, `@types/express`) para aproveitar om máximo. Observe que quando injetado o `@Res()` em `@Response()` um manipulador de método, o nest é colocado no modo específico da biblioteca para esse manipulador e se torna responsável por gerenciar a resposta. Ao fazer isso, deve ser emitido algum tipo de respostafazendo uma chamada no object `response` (por exemplo, `res.json(...)` ou `res.send(...)`), ou o servidor HTTP travará.

## Recursos

Anteriormente, definimos um endpoint para buscar o recurso users (rota `GET`). Normalmente, tembém queremos fornecer um endpoint que crie novos registros. Para isso, vamos criar o manipulador `POST`:

```typescript
import { Controller, Get, Post } from '@nestjs/common';

@Controller('users')
export class UsersController {
  @Post()
  create(): string {
    return 'Esta ação adiciona um novo usuário'
  }

  @Get()
  findAll(@Req() request: Request): string {
    return 'Esta ação retorna todos os usuários';
  }
}
```

É simples assim. O nest fornece decorators para todos os métodos HTTP padrão `@Get()`, `@Post()`, `@Put()`, `@Delete()`, `@Patch()`, `@Options()` e `@Head()`. Além disso, `@All()` define um endpoint que manipula todos eles.

## Curingas de rota

Rotas baseadas em padrões também são suportadas. Por exemplo, o asterisco é usado como curinga e coreponderá a qualquer combinação de caracteres.

```typescript
@Get('ab*cd')
findAll() {
  return 'Esta rota usa um curinga';
}
```

O `ab*cd` caminho da rota corresponderá a `abcd`, `ab_cd`, `abecd`, e assim por diante. Os caracteres `?`, `+`, `*` e `()` podem ser usados em um caminho de rota e são subconjuntos de suas contrapartes de expressão regular. O hífen (`-`) e o ponto (`.`) são interpretados literalmente por caminhos baseados em string.

> Observação
> Um curinga no meio da rota só é suportado pelo express.

## Cabeçalhos

Para especificar um cabeçalho de reposta personalizado, pode ser usado o decorator `@Header()` ou um object response específico da biblioteca (e chamar `res.header()` deretamente).

```typescript
@Post()
@Header('Cache-Control', 'none')
create() {
  return 'Esta ação adiciona um novo usuário';
}
```

## Redirecionamento

Para redirecionar uma resposta para uma URL específica, você pode usar o decorator `@Redirect()` ou um object response específico da biblioteca (e chamar `res.redirect()` diretamente).

`@Redirect()` recebe dois argumentos, `url` e `statusCode`, ambos são opcionais. o valor padrão de `statusCode` é `302` (`Found`) se omitido.

```typescript
@Get()
@Redirect('https://nestjs.com', 301)
```

Os valores retornados substituirão quaisquer argumentos passados ao decorator `@Redirect()`. Por exemplo:

```typescript
@Get('docs')
@Redirect('https://docs.nestjs.com', 302)
getDocs(@Query('version') version) {
  if(version && version === '5') {
    return { url: 'https://docs.nestjs.com/v5/' }
  }
}
```

## Parametros de rota

Rotas com caminhos estáticos não funcionarão quando você precisar aceitar dados dinamicos como parte da solicitação (por exemplo, `GET /users/1` para obter usuário com o id `1`). Para definir rotas com parametros, pode-se adicionar tokens de parametro de rota no caminho da rota para capturar o valor dinamico naquela posição na URL da solicitação. O token de parametro de rota no `@Get()` exemplo de decorator abaixo demonstra esse uso. Prametros de rota declarados dessa forma podem ser acessados usando o decorator `@Param()`, que deve ser adicionado à assinatura do método.

```typescript
@Get(':id')
findOne(@Param() params: any): string {
  console.log(prams.id);
  return `Esta ação retorna um #${params.id} usuário`;
}
```

O decorator `@Param()` é usado para capturar os parametros para o método (`params` no exemplo acima), e retorna os parametros de rota disponíveis como propriedades desse parametro de método. Como foi visto no codigo acima, pode se acessar o parametro `id` referenciando `params.id`. Você também prde passar um token de parametro especifico para o decorator e então referenciar o mesmo diretamente pelo nome no corpo do método.

```typescript
@Get(':id')
findOne(@Param('id') id: string): string {
  return `Esta ação retorna um #${id} usuário`;
}
```

> DICA
> Importar `Param` do pacote `@nestjs/common`

## Roteamento de subdomínio

O decorator `@Controller` pode ter a opção `host` e exigir que o host HTTP das solicitações recebidas correspondam a algum valor expecifico.

```typescript
@Controller({ host: 'admin.example.com' })
export class AdminController {
  @Get()
  index(): string {
    return 'Admin page';
  }
}
```

> AVISO IMPORTANTE
> Como o Fastify não oferece suporte para roteadores aninhados, o Express deve ser usadi para usar roteamento do subdomínio.

Semelhante a uma rota `path`, a opção `hosts` pode usar tokens para capturar o valor dinâmico naquela posição no nome do host. O token de parametro do host no decorator `@Controller()` no exemplo abaixo demonstra esse uso. Os parametros do host declarados dessa forma podem ser acessados usando o decorator `@HostParam()`, que deve ser adicionado à assinatura do método.

```typescript
@Controller({ host: ':account.example.com' })
export class AccountController {
  @Get()
  getInfo(@HostParam('account') account: string) {
    return account;
  }
}
```

---
[<< Anterior](./2-primeiros-passos.md) [Próximo >>](./3-controllers.md.md)
