# 15 - Configurando Nodemailer

**Objetivo**: Configurar nodemailer para realizar envio de emails.

### Instalando nodemailer

Instale a dependência usando yarn.

```bash
yarn add nodemailer
```

### Criando um arquivo de configuração

Existem algumas informações necessárias para enviar um email. Vamos guarda-las em um arquivo de configuração em `src/config/mail.js`. Essas informações virão do serviço que você usa para enviar emails. Durante o curso foram recomendados:

- Amazon SES
- Mailgun
- Sparkpost

Durante o desenvolvimento serviços como [mailtrap.io](https://mailtrap.io) devem ser usados.

```javascript
export default {
  host: 'smt.mailtrap.io',
  port: 2525,
  secure: false,
  auth: {
    user: 'your-user',
    pass: 'your-pass',
  },
  default: {
    from: 'GoBarber team <noreply@gobarber.com>',
  },
};
```
### Criando um wrapper para o nodemailer

Ao invés de configurar uma instância do nodemailer toda vez que precisamos usa-lo, vamos criar um wrapper que irá fazer isso apenas uma vez, além de expor métodos utilitários para o envio de email.

```javascript
import nodemailer from 'nodemailer';
import mailConfig from '../config/mail';

class Mail {
  constructor() {
    const { host, port, secure, auth } = mailConfig;

    this.transporter = nodemailer.createTransport({
      host,
      port,
      secure,
      // there are some strategies to send email that does not make use of auth
      auth: auth.user ? auth : null,
    });
  }

  sendMail(message) {
    return this.transporter.sendMail({
      ...mailConfig.default,
      ...message,
    });
  }
}

export default new Mail();

```

Para usa-lo faça o import e chame o método `sendMail`.

```javascript

import Mail from './Mail';

Mail.sendMail({
  to: 'Name <email@email.com>',
  subject: 'Subject',
  text: 'Text message'
});

```