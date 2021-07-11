# NextJS + HTTPS | Servidor Https local

## Proteja seu servidor de desenvolvimento local para fornecer acesso a recursos restritos, como APIs de dispositivo.

Para utilizar alguns recursos dos navegadores modernos, você precisa operar em uma conexão https segura.

## Passos

* Gerar um certificado para localhost
* Adicionar o certificado as chaves
* Atualizar o arquivo **server.js** para usar o certificado e exigir https
* Atualizar scripts **packege.json**

### Gerando um certificado para localhost
Crie um certificado com OpenSSL usando este comando.

```cmd

openssl req -x509 -out localhost.crt -keyout localhost.key \ -days 365 \ -newkey rsa:2048 -nodes -sha256 \
  -subj '/CN=localhost' -extensions EXT -config <( \
   printf "[dn]\nCN=localhost\n[req]\ndistinguished_name = 
	 dn\n[EXT]\nsubjectAltName=DNS:localhost\nkeyUsage=digitalSignature\nextendedKeyUsage=serverAuth")	

```

O comando acima vai gerar o certificado e a chave:.

* __req__ comando para criar certificados
* __-x509__ indica que queremos um certificado autoassinado
* __-out__ nome e localização para o arquivo de certificado
* __-keyout__ nome e localização para o arquivo de chave
* __-days__ dias antes que o certificado expire
* __-newkey__ tipo e força da criptografia
* __-nodes__ não criptografar a chave
* __-subj__ campo de assunto, não precisamos navegar pelos prompts de perguntas
* __-extensions__ extensão do certificado
* __-config__ especificar configuração alternativa

### Adicione o certificado ao chaveiro
Mova o certificado e a chave para o local onde deseja armazená-los.
No meu caso deixo dentro de uma pasta chamada **certificates** em meu projeto corrente.
Certifique-se de adicionar essa pasta ao seu **.gitignore**.

**Clique duas vezes em seu certificado para adiciona-lo ao seu chaveiro**.
![](https://miro.medium.com/max/700/1*eNCvApvyh3NulxRwKYoGSA.png)

**Na janela de chaves, selecion "Certificados" e clique duas vezes no certificado que criou**.
![](https://miro.medium.com/max/700/1*AWe0KVFSoGbsmwkmKmJ6RA.png)


**Agora configure como confiável seu novo certificado**.
![](https://miro.medium.com/max/700/1*2X70fHxS8qP9e9eMrJ6UTg.png)


### Configuração do arquivo **server.js**
Caso ainda não tenha o arquivo criado, crie um com o nome de **server.js**.
Este arquivo vai instruir o NextJs sobre como implementar o servidor de desenvolvimento para rodar sobre o protocolo https.

**Coloque o código abaixo em seu arquivo _server.js_**.
```javascript

const { createServer } = require("https");
const { parse } = require("url");
const next = require("next");
const fs = require("fs");
const dev = process.env.NODE_ENV !== "production";
const app = next({ dev });
const handle = app.getRequestHandler();
const httpsOptions = {
  key: fs.readFileSync("./certificates/localhost.key"),
  cert: fs.readFileSync("./certificates/localhost.crt"),
};
app.prepare().then(() => {
  createServer(httpsOptions, (req, res) => {
    const parsedUrl = parse(req.url, true);
    handle(req, res, parsedUrl);
  }).listen(3000, (err) => {
    if (err) throw err;
    console.log("> Server started on https://localhost:3000");
  });
});

```

### Iniciando o servidor https
Agora edite seu arquivo **package.json** incluindo o comando abaixo.

```json

"scripts": {
  "start": "node server.js"
},

```

### Execute o servidor
Você pode chamar o servidor https com o comando **npm run start**.


> Este documento foi inspirado no tutorial de [**Greg Farrow**](https://medium.com/responsetap-engineering/nextjs-https-for-a-local-dev-server-98bb441eabd7)













#### ****************************************
**Se este repositório te ajuda ou ajudou de alguma forma, considere nos fazer uma doação**.
[![](https://www.paypalobjects.com/pt_BR/BR/i/btn/btn_donateCC_LG.gif)](https://www.paypal.com/donate?hosted_button_id=T4ZWGJ7HPHK5A)

[![](https://drive.google.com/file/d/1S087uwrl8ctW0Q22S5xifXjCtcX5Jpmo/view?usp=sharing)](https://www.paypal.com/donate?hosted_button_id=T4ZWGJ7HPHK5A)

















	
	






  