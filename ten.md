# 10 - Configurando MongoDB

**Objetivo**: Configurar MongoDB para ser usado no armazenamento de dados não relacionais, visando melhor performance da aplicação.

### Criando uma instância no docker com MongoDB

Basta rodar o seguinte comando para baixar e rodar uma imagem com MongoDB já configurado:

```bash
docker run --name mongobarber -p 27017:27017 -d -t mongo
```

Acessando o endereço `http://localhost:27017` em um navegador, será exibido uma mensagem de erro indicando que o servidor do MongoDB está funcionando.

### Acessando o banco pela aplicação

Para acessar um banco MongoDB de um aplicação node, será necessario usar uma biblioteca chamada `mongoose`.

```bash
yarn add mongoose
```

Para realizar um conexão, chame a função `connect` do `mongoose`.

```javascript
import mongoose from 'mongoose';

// the uri that defines where to connect
// the gobarber path is the name of the database
// it will be created automatically in the first connection
const connectionString = 'mongodb://localhost:27017/gobarber';

// configuration options for mongoose
const useNewUrlParser = true;
const useFindAndModify = true;

mongoose.connnect(connectionString, { useNewUrlParser, useFindAndModify });
```
