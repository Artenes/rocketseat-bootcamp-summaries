# 14 - Cancelamento de agendamento

**Objetivo**: Cancelar um agendamento. Um usuário pode realizar o cancelamento apenas com duas horas ou mais de antecedência.

### Verificando antecedência

Verificar antecedência significa criar uma hora limite. Se a hora atual passar desse limite, o usuário não pode cancelar o agendamento.

```javascript

import { subHours, isAfter, parseISO } from 'date-fns';

const appointment = parseISO('2019-01-02T12:00:00-04:00'); // 12:00:00
const limit = subHours(appointment, 2); // 10:00:00
const now = new Date(); // 11:00:00

if (isAfter(now, limit)) {
    // now passes the limit, so user can't cancel appointment
}

// else, the appointment can be removed

```
