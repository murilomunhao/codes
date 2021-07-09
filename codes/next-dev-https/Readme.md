# NextJS + HTTPS | For a Local Dev Server
## Secure your local dev server to give you access to restricted features, such as device APIs.

To utilise some features of modern browsers, you need to be operating over a secure https connection. By default, the NextJS development server runs on http.
Here’s how:
Generate a certificate for localhost
Add the certificate to keychain
Update server.js to use the certificate and require https
Update package.json scripts

Generate a Certificate for localhost
You can create a certificate using OpenSSL with this command:
openssl req -x509 -out localhost.crt -keyout localhost.key \
  -days 365 \
  -newkey rsa:2048 -nodes -sha256 \
  -subj '/CN=localhost' -extensions EXT -config <( \
   printf "[dn]\nCN=localhost\n[req]\ndistinguished_name = dn\n[EXT]\nsubjectAltName=DNS:localhost\nkeyUsage=digitalSignature\nextendedKeyUsage=serverAuth")
This command will generate the certificate and the key:
req the command for creating certificates
-x509 is used to indicate we want a self-signed certificate
-out name and location for the certificate file
-keyout name and location for the certificate file
-days how many days before this certificate expires
-newkey the type and strength of encryption
-nodes don't encrypt the key
-subj provides the subject field (so we don't need to navigate question prompts)
-extensions use the provided certificate extension (in this case to help with config)
-config specify alternative configuration
Add your Cert to Keychain
Move the certificate and key to the location you want to store them. For me, that’s a directory in my project helpfully called certificates. I also make sure to add that directory name to my .gitignore .
Double-click on your certificate to add it to your keychain:
dialogue box to add the cert to keychain
Click Add
In the keychain window, select “Certificates” and then double click on your new certificate:
keychain window showing the new certificate
You can now set it to be trusted:
dialogue showing “always trust” settings
Development Server Config
Go to your NextJS project and locate your server configuration file: server.js (if you don't have one create one).
This file instructs NextJS on how to deploy our development server — and in our case, it tells it to use https instead of HTTP. Drop this code into it:
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
Start Scripts
The last thing to do is to instruct the project to use the server configuration when it starts up. Do that by amending the package.json . Change the scripts entry so that the "start" script looks like this:
"scripts": {
  "start": "node server.js"
},
Run!
You can now start up your server using npm run start .
screengrab of a secured localhost connection
https://localhost:30
