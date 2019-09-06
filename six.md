# 6 - Validação de agendamento

**Objetivo**: Validar se a data passada na requisição:

- não é uma data no passado
- não está sendo usada por outro agendamento

Para se trabalhar com datas em node usa-se a biblioteca `date-fns`.

```shell
yarn add date-fns@next
```
`@next` significa que a versão mais atual será baixada (seja ela estável ou não).

### Validando se uma data está no passado
Será usado três funções do `data-fns`:

- isBefore
- parseISO
- startOfHour

`parseISO` é usada para converter uma data em `string` para um objeto `Date` do javascript. Isso é necessário pois as funções do `date-fns` recebem como argumento um objeto `Date` ao invés de uma `string`.

```javascript
import { parseISO } from 'date-fns';

const date = '2019-01-01T10:30:00-04:00';
const parsedDate = parseISO(date);
```

`isBefore` é usada para comparar se uma data vem antes de uma outra data.

```javascript
import { parseISO, isBefore } from 'date-fns';

const yesterday = parseISO('2019-01-02T13:00:00-04:00');
const today = new Date();
const hasPassed = isBefore(yesterday, today); //true
```

`startOfHour` é usada para obter a hora inicial de uma data. Se tivermos a hora `12:45:12`, a função ira retornar `12:00:00`. Esta verificação é específica do `gobarber` pois ele define que todo agendamento acontece em horas cheias.

```javascript
import { parseISO, startOfHour } from 'date-fns';

const date = parseISO('2019-01-02T13:45:12-04:00');
const hour = startOfHour(date); //2019-01-02T13:00:00-04:00
```

### Validando se existe outro agendamento na mesma data
Como todo agendamento no `gobarber` acontece em horas cheias, basta pegarmos a hora correspondente da data enviada na requisição e fazer uma query no banco.

```javascript
import { parseISO, startOfHour } from 'date-fns';
import Appointment from '../models/Appointment';

const hour = startOfHour(parseISO('2019-01-02T13:45:12-04:00'));
const isDateUsed = await Appointment.findOne({ where: { 

	provider_id: 3,
    canceled_at: null,
    date: hour

} });
```
