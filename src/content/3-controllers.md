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

---
[<< Anterior](./2-primeiros-passos.md) [Próximo >>](./3-controllers.md.md)
