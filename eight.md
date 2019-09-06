# 8 - Aplicando paginação

**Objetivo**: Implementar paginação em uma rota que lista dados.

Ao realizar a query no banco, basta informar dois parâmetros para a função `findAll`:

- limit
- offset

```javascript
import Appointment from './models/Appointment';

// a pagina atual, que pode vir como um query parameter
const page = 1; //const { page = 1 } = req.query;

// define o numero maximo de items por pagina
const limit = 20;

// define quantos registros serao pulados
// se page = 1
// (1 - 1) * 20 = 0 * 20 = 0
// logo nenhum registro sera pulado
// se page = 2
// (2 - 1) * 20 = 1 * 20 = 20
// logo 20 registros serao pulados
const offset = (page - 1) * 20;

// realiza a query
const appointments = await Appointment.findAll({

	limit,
    offset

})
```
