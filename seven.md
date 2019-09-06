# 7 - Listando agendamentos do usuário

**Objetivo**: Listar os agendamentos criados pelo usuário logado.

### Construindo a query
Ao efetuar a query contra o modelo `Appointment`, deve-se retornar os seguintes dados:

- `id` e `date` do `appointment`
- `id` e `name` do `provider`
- `url` do avatar do `provider`

```javascript
import Appointment from './models/Appointment';
import User from './models/User';
import File from './models/File';

// obtem apenas os appointments nao cancelados de um usuario
const where = { user_id: 4, canceled_at: null };

// ordena por data em ordem crescente
const order = ['date'];

// pega somente o id e date do appointment
const attributes = ['id', 'date'];

// obtem apenas o path e url do avatar do provider
// como url eh um campo virtual que depende do path
// deve-se carregar o path para que o url nao venha null
const providerAvatar = {

	model: File,
    as: 'avatar',
    attributes: ['path', 'url'],

};

// define o relacionamento a ser pego junto com o appointment
// incluindo consigo o avatar do provider
const provider = {

	model: User,
    as: 'provider',
    include: [ providerAvatar ]

};

// realiza a query com os atributos acima
const appointments = await Appointment.findAll({
	
    where,
    order,
    attributes,
    include: [ provider ]

});
```
