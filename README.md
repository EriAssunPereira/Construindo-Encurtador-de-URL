# Construindo-Encurtador-de-URL

Para construir um encurtador de URL com NodeJS, TypeScript e MongoDB, você precisará seguir alguns passos essenciais. Vou descrever o processo em módulos para facilitar o entendimento e a implementação.

### Módulo 1: Configuração Inicial
- **Instalação do NodeJS**: Certifique-se de ter o NodeJS instalado em sua máquina.
- **Criação do projeto**: Inicie um novo projeto NodeJS com `npm init` e instale as dependências necessárias como `express`, `mongoose`, `shortid`, e `typescript`.

### Módulo 2: Configuração do TypeScript
- **tsconfig.json**: Crie um arquivo `tsconfig.json` para configurar o TypeScript com as opções desejadas para o projeto.
- **Compilação**: Configure scripts no `package.json` para compilar o TypeScript para JavaScript.

### Módulo 3: Construção da API
- **Express**: Utilize o Express para criar as rotas da API.
- **Rotas**: Defina rotas para criar URLs encurtadas e para redirecionar as URLs curtas para as URLs originais.

### Módulo 4: Integração com MongoDB
- **Mongoose**: Use Mongoose para conectar ao MongoDB e definir o esquema para as URLs.
- **Modelo de Dados**: Crie um modelo para salvar as URLs com um identificador único gerado pelo `shortid`.

### Módulo 5: Lógica de Encurtamento
- **Encurtamento**: Implemente a lógica para encurtar a URL, que inclui salvar a URL original e o identificador único no banco de dados.
- **Redirecionamento**: Crie uma rota que recebe o identificador único, busca a URL original no banco de dados e redireciona o usuário.

### Módulo 6: Validação e Segurança
- **Validação**: Adicione validação para garantir que as URLs são válidas antes de encurtá-las.
- **Segurança**: Implemente medidas de segurança, como limitar o número de requisições para evitar abusos.

### Exemplo Prático:
```javascript
const express = require('express');
const mongoose = require('mongoose');
const shortId = require('shortid');

const app = express();
app.use(express.json());

// Conectar ao MongoDB
mongoose.connect('sua_string_de_conexao_mongodb', {
  useNewUrlParser: true,
  useUnifiedTopology: true
});

// Modelo de dados
const UrlSchema = new mongoose.Schema({
  originalUrl: String,
  shortUrl: String,
  shortId: {
    type: String,
    default: shortId.generate
  }
});

const Url = mongoose.model('Url', UrlSchema);

// Rota para encurtar URL
app.post('/shorten', async (req, res) => {
  const { originalUrl } = req.body;
  const url = await Url.create({ originalUrl });
  res.send({ originalUrl, shortUrl: `http://seuhost/${url.shortId}` });
});

// Rota para redirecionar
app.get('/:shortId', async (req, res) => {
  const { shortId } = req.params;
  const url = await Url.findOne({ shortId });
  if (url) {
    res.redirect(url.originalUrl);
  } else {
    res.status(404).send('URL não encontrada');
  }
});

const PORT = 5000;
app.listen(PORT, () => console.log(`Servidor rodando na porta ${PORT}`));
```

Este é um exemplo simplificado de como você pode começar a construir seu encurtador de URL. Lembre-se de substituir `'sua_string_de_conexao_mongodb'` pela string de conexão do seu banco de dados e `'http://seuhost/'` pelo endereço onde sua API estará hospedada.

Aqui está uma ótima dica de como dar um ponto de partida para o seu projeto.
