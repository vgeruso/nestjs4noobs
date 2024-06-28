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

@Controller('user');
export class UserController {
  @Get()
  index(): string {
    return 'Essa ação retorna todos os usuários';
  }
}
```

O decorator `@Get()` indica o metodo HTTP que corresponde aquela chamda especifica, indica ao Nest a criação do endpoint no qual corresponde ao metodo HTTP e ao caminho da rota. A rota para um manipulador é determinado pela concatenação do do prefixo indicado no decorator `@Controller()` e qual quer caminho especificado no decorator do metodo seja ele GET, POST, PUT, PATH ou DELETE.

No exemplo como foi declarado um prefixo `user` e não foi adicionado nenhuma informação no caminho do metodo, o Nest mapeará como `GET /user` para cessar o metodo index declarado. Conforme mencioando, caso seja adicionado ao metodo algum informação do caminho por exemplo `@Get('name')` será produzido um mapeamento de rota como `GET /user/name`.

No exemplo quando a solicitação GET é feita para o edpoint, o Nest roteia a solicitação para nosso método definido como `index()`. O nome do método aqui é completamente arbitrário, obviamente é necessário declarar um método ao gerar uma rota, mas o Nest não atribui nenhum significado a o nome escolhido.

Este método retorna um status 200 e a resposta asociada, que nesse caso é apena uma string `Essa ação retorna todos os usuários`. Por padrão o Nest trata todo os retornos verificando o tipo, quando as funções retornam como resposta um objeto ou um array JavaScript esse retorno será serializado automaticamente para JSON, mas quando é retornado um tipo primitivo - por exemplo `string`, `number`, `boolean` - o Nest enviará apenas o valor sem tentar serealizá-lo simplificando o tratamento da resposta, para nós basta retornar o valor o Nest cuida do resto. Além disso ele também retorna o código de status que sempre é padronizado para `200` exeto em metodos `POST` que usam `201`.

---
[<< Anterior](./2-primeiros-passos.md) [Próximo >>](./3-controllers.md.md)
