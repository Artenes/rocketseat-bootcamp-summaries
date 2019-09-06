# 9 - Listando agenda do prestador

**Objetivo**: Listar os agendamentos para um determinado dia de um prestador.

Para restringir os resultados por data, faz-se uso dos operadores `Op` do `sequelize`. Além diss é feito uso da biblioteca `date-fns` para criar um range de data para realizar a comparação.

### Criando o range de data

Ao receber uma data para fazer a query, é necessário pegar o início do dia e o fim do dia daquela data para montar uma query que retorne todos os agendamentos de um dado dia.

```javascript
import { startOfDay, endOfDay, parseISO } from 'date-fns';

const date = parseISO('2019-09-12T12:34:12-04:00');

// essas duas funcoes irao alterar a hora da data passada
// elas tambem pode alterar o dia conforme o fuso-horario
const start = startOfDay(date); //2019-09-12T00:00:00-04:00
const end = endOfDay(date); //2019-09-12T23:59:59-04:00

```

### Definindo o intervalo na query

Tendo as datas do intervalo definidos, agora a query deve ser montada.

```javascript
import Appointment from './models/Appointment';
import { Op } from 'sequelize'

// define que o campo date da tabela appointments precise estar entre
// o intervalo de data definido em start e end
const date = {

	[Op.between]: [start, end]

};

// cria a query e filtra os dados pelo date
const appointments = await Appointment.findAll({ date });
```
