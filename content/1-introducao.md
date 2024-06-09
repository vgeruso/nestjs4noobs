# Introdução

O Nest de acordo com a documentação oficial é um framework para contrução de aplicações Node.js do lado do servidor eficientes e escalonáveis. Usando JavaScript progressivo, é construido e possui suporte total a TypeScript, dando a liberdade também dos desenvolvedores usarem o JavaScript puro, combinando elementos de tres tipos de programação, POO (Programação Orientada a Obijetos), PF (Programação funcional) e PFR (Programação funcional reativa).

Por debaixo dos panos, o nosso famework umas como padrão o Express, mas pode ser facilmente configurado para usaro Fastfy. Sendo inspirado no framework forntend Angular o nest fornece uma arquitetura de aplicativos pronta para uso, permitindo que os desenvolvedores criem sistemas altamente testaveis, escalaveis e de fácil manutenção.

## Instalação

pré requisitos para o uso e instalação do Nest:

1. Conhecimento basico em JavaScript e Node.js
2. Node.js 20 ou maior
3. Npm 10 ou maior

Existem diversas formas para iniciar um projeto Nest, mas o mais basico e utilizado, por ser mais prático, é usando o Nest CLI, para isso temos que instala-lo de forma global em nossa maquina com o seguinte comando:

```bash
npm i -g @nestjs/cli
```

Para criar um projeto é simples basta executar:

```bash
nest new project-name
```

O Nest CLI irá realizar uma pergunta basica de configuração relacionado ao gerenciador de pacotes que irá ser usado do por padrão aqui eu escolhi o npm mas fique a vontado para escolher o seu. Após a escolha o CLI irá fazer o trabalho de configurar e gerar todos os arquivos necessários para o funcionamento do projeto.

![configure_nest](/images/configure_nest.png)

Para dar um start no projeto criado basta executar:

```bash
cd project-name
npm run start:dev
```

O sistema irá executar uma transpilação do TypeScript e excutar os endpoits criados imprimindo na tela os endpoints ativos:

![start_nest](/images/start_nest.png)

Agora basta acessar em seu navegador o [http://localhost:3000/](http://localhost:3000/) e verá que o projeto básico do NestJS está rodando em seu local.

---
[Próximo >>](/content/2-primeiros-passos.md)
