# 16 - Configurando templates de e-mail

**Objetivo**: Configurar handlebars (template engine) para gerar e-mails.

### Instalando handlebars

Instale a dependência usando yarn. Não instalamos o handlebars diretamente, mas outras bibliotecas que ensinam o express e nodemailer a lidar com handlebars.

```bash
yarn add express-handlebars nodemailer-express-handlebars
```

### Configurando nodemailer

Precisamos dizer ao nodemailer como usar handlebars.

```javascript
import { resolve } from 'path';
import nodemailer from 'nodemailer';
import exphbs from 'express-handlebars';
import nodemailerhbs from 'nodemailer-express-handlebars';
  
const transporter = nodemailer.createTransport({ ... });

const viewPath = resolve(__dirname, '..', 'app', 'views', 'emails');

// tells nodemailer to use handlebars
transporter.use(
  'compile',
  nodemailerhbs({
    viewEngine: exphbs.create({
      layoutsDir: resolve(viewPath, 'layouts'),
      partialsDir: resolve(viewPath, 'partials'),
      defaultLayout: 'default',
      extname: '.hbs',
    }),
    viewPath,
    extName: '.hbs',
  })
);
```

Com isso será necessário criar arquivos com a extensão .hbs seguindo a sintaxe do handlebars em `/app/views/emails`.

### Enviando e-mails

Ao enviar o e-mail diga ao nodemailer que um template deve ser usado para gerar o corpo do e-mail e passe os dados necessário para gerar esse template.

```javascript
import nodemailer from 'nodemailer';

const transporter = nodemailer.createTransport({ ... });

transporter.sendMail({
  template: 'your-template-name',
  context: {  } // in this object put any data used to render the template
});
```