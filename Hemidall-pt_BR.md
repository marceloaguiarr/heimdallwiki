# Executar Heimdall localmente

Este tutorial pretende mostrar como criar executar o Heimdall localmente para facilitar o desenvolvimento.

Após a leitura desse tutorial o desenvolvedor deve ser capaz de:

* Baixar o Heimdall para a sua máquina
* Executar o Heimdall localmente
* Criar um Environment
* Criar uma Api
* Criar um Resource
* Criar uma Operation
* Criar um Inteceptor

### Baixar o Heimdall para a sua máquina

Crie uma pasta na sua máquina que vai conter o Heimdall e utilize o comando

```bash
$ git clone --depth=1 https://github.com/getheimdall/heimdall.git heimdall
$ cd heimdall
```

### Como executar o Heimdall localmente

Para executar o Heimdall faça:

Inicie o Config
```bash
$ cd /heimdall-config
$ mvn spring-boot:run
```

Inicie o Gateway (precisa do **config** executando)
```bash
$ cd /heimdall-gateway
$ mvn spring-boot:run
```

Inicie a Api (precisa do **config** executando)
```bash
$ cd /heimdall-api
$ mvn spring-boot:run
```

Inicie o front-end (precisa da **Api** executando)
```bash
$ cd /heimdall-frontend
$ npm install
$ npm run start
```

Abra um browser e navege para `http://localhost:3000/` e você deverá ver a tela de login do Heimdall. Utilize admin/admin como usuário/senha para fazer login.
 
### Criar um Environment

Selecione Environment na barra lateral para criar o seu Environment.

Na parte inferior da página clique no botão para criar um novo Environment e preencha os seguintes valores nos campos mostrados:

* Name: Environment de Teste
* Description: Simples environment para teste
* Inbound URL: `http://127.0.0.1:8080`
* Outbound URL: `http://127.0.0.1:8080`

Existe uma validação para o inbound do Environment. Todo inbound url deve seguir um dos seguintes padrões:

* `http[s]://host.domain[:port]`
* `www.host.domain[:port]`

Por isso não é possível criar um Environment com o inbound URL `http://localhost:8080`. No entanto isso não impacta os testes abaixo. As chamadas podem ser feitas tanto para `http://localhost:8080` quando para `http://127.0.0.1:8080`.

Com isso crie seu Environment.

### Criar uma Api

Selecione Api na barra lateral para criar o sua Api.

Na parte inferior da página clique no botão para criar um novo Environment e preencha os seguintes valores nos campos mostrados:

* API Name: Api de Teste
* API version: 1.0.0 (ou qualquer versão que você queira)
* Description: Simples Api para teste
* Base path: /basepath (ou qualquer basepath que você queira)
* Selecione o Environment que você criou no passo anterior

Com isso crie sua Api.

### Criar um Resource

Após criar a Api você vai ser redirecionado para tela de listagem de Api's. Selecione a sua Api.

Na aba Resources escolha a opção de adicionar novo Resource e preencha os campos:

* Name: Resource de Teste
* Description: Simples Resource de teste

Crie seu Resource.

### Criar uma Operation

Ainda na aba de Resource, clique no seu Resource recém criado em seguida na opção de adicionar uma Operation e preencha os campos com os seguintes valores:

* Method: GET
* Path: /teste
* Description: Operation de teste

Crie o Operation.

### Criar um Inteceptor
 
 Na tela de Interceptor selecione o Resource e o Operation que você criou.
 
 Para adicionar um Interceptor, clique e arraste o tipo para a área de `Request`. O tipo que queremos é o `Mock`.
 
* Name: Interceptor de Teste
* Description: 
* Life Cycle: OPERATION
* Content: <Não modifique>

## Testando 

Para testar seu trabalho utilize uma ferramenta REST como Postman ou no seu browser aponte para `http://localhost:8080/basepath/teste` e você deverá ver o resultado:

```json
{
  "name":"Mock Example"
}
```