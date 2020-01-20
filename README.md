# Monorepo com Yarn Workspaces

Criar as pastas do projeto

    mkdir NomeProjeto
    cd NomeProjero
    yarn init -y
    mkdir packages
    

Altere o package.json da raiz

```
{
  "private": true,
  "workspaces": [
    "packages/*"
  ],
  "scripts": {
    "start:server": "yarn workspace server start",
    "start:web": "concurrently --kill-others \"yarn start:server\" \"yarn workspace web start\"",
    "start:mobile": "concurrently --kill-others \"yarn start:server\" \"yarn workspace mobile start\"",
    "start": "concurrently --kill-others \"yarn start:server\" \"yarn workspace web start\" \"yarn workspace mobile start\""
  },
  "devDependencies": {
    "concurrently": "^5.0.2"
  }
}
```


> Projetos

### Server (NodeJs - Express)

    cd packages 
    mkdir server && cd server
    yarn init -y
    yarn add express cors
    yarn add sucrase nodemon -D
    mkdir src && cd src
    touch index.js
    

Modelo do index.js

    const express = require("express");
    const cors = require("cors");
    
    const app = express();
    
    app.use(cors());
    app.use(express.json());
    
    app.get("/", (req, res) => {
      return res.send("Ola mundo!");
    });
    
    app.listen(3333);

Acrescentar ao packages/server/package.json

    "private": false,
    "scripts": {
        "start": "nodemon src/index.js"
      },

### Web (ReactJs)

    cd packages
    create-react-app web

Exclua a pasta .git

### Mobile (Expo)

    cd packages
    expo init mobile
    yarn add expo-yarn-workspaces -D

Crie o arquivo packages/mobile/metro.config.js

    const { createMetroConfiguration } = require('expo-yarn-workspaces');
    
    module.exports = createMetroConfiguration(__dirname);

No arquivo packages/mobile/package.json

    "name": "mobile",
    "version": "0.1.0",
    "main": "__generated__/AppEntry.js",
    "packagerOpts": {
        "config": "metro.config.js"
      },
    "scripts": {
        "start": "expo start",
        "android": "expo start --android",
        "ios": "expo start --ios",
        "web": "expo start --web",
        "eject": "expo eject",
        "postinstall": "expo-yarn-workspaces postinstall"
      },
