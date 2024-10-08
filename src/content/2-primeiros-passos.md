# Primeiros passos

Para criar um projeto é simples, basta executar:

```bash
nest new project-name
```

O Nest CLI irá realizar uma pergunta básica de configuração relacionado ao gerenciador de pacotes que irá ser usado por padrão, aqui eu escolhi o `npm`, mas fique a vontade para escolher o seu. Após a escolha o CLI irá fazer o trabalho de configurar e gerar todos os arquivos necessários para o funcionamento do projeto.

![configure_nest](../images/configure_nest.png)

No repositório que foi criado com o processo, são gerados arquivos de configuração relacionadas à algumas ferramentas vinculadas a aplicação principalmente o TypeScript, e o diretório `src` composto por arquivos essenciais para o funcionamento básico da aplicação.

```txt
src
  |-- app.controller.spec.ts
  |-- app.controller.ts
  |-- app.module.ts
  |-- app.service.ts
  |-- main.ts
```

| Arquivo                  | Sobre                                                                                                     |
| ------------------------ | --------------------------------------------------------------------------------------------------------- |
| `app.controller.spec.ts` | Módulo de teste do controller                                                                             |
| `app.controller.ts`      | Controller básico com uma rota                                                                            |
| `app.module.ts`          | Módulo raiz da aplicação                                                                                  |
| `app.service.ts`         | Service básico com um único método                                                                        |
| `main.ts`                | Arquivo de entrada da aplicação que usa a função principal NestFactory para criar uma instância do Nest.  |

Sendo o arquivo de entrada, o `main.ts` inclui uma função que inicializa a aplicação.

```typescript
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  await app.listen(300);
}
bootstrap();
```

Para criar uma instância do aplicativo Nest, a classe principal `NestFactory` que expõe métodos estáticos e permitem a criação da instância do aplicativo. O método `create()` retorna um objeto que preenche a interface `INestApplication`. Este objeto fornece um conjunto de métodos que serão descritos nos próximos capítulos. No exemplo acima, simplesmente o `HTTP` é iniciado e permite que o aplicativo aguarde solicitações de entrada.

Para dar um start no projeto criado basta executar:

```bash
cd project-name
npm run start
```

ou

```bash
cd project-name
yarn start
```

ou

```bash
cd project-name
pnpm start
```

O sistema irá executar uma transpilação do TypeScript e mapear os endpoints criados imprimindo na tela os ativos:

![start_nest](../images/start_nest.png)

Agora basta acessar em seu navegador ou em algum cliente REST API de sua preferência o [http://localhost:3000/](http://localhost:3000/), você deverá ver a mensagem `Hello World!` na tela.

Para executar a aplicação em modo de observação (watch mode) recarregando a mesma após qualquer alteração no código, basta executar `npm start:dev` ou `yarn start:dev` ou `pnpm start:dev` no terminal, esse modo auxilia o desenvolvedor e aumenta a produtividade.

---
[<< Anterior](./1-introducao.md) [Próximo >>](./3-controllers.md)
