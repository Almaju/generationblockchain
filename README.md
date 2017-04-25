# Génération Blockchain
Ce projet utlise Ethereum pour le protocole, un container Docker pour le cluster privé, NodeJS et Truffle pour le client.
Tous les tests ont été effectués via des machines Ubuntu sur des instances AWS.

## Mode d'emploi
### Créer le cluster privé Ethereum
Clonez ce repository : https://github.com/Capgemini-AIE/ethereum-docker et installez Docker ainsi que Docker Compose. Puis executez cette commande dans le dossier ethereum-docker.
```
docker-compose up -d
```

Pour augmenter ou réduire le nombre de noeuds :
```
docker-compose scale eth=3
```

Pour se connecter à la console geth (pour intéragir directement avec la blockchain) :
```
docker exec -it ethereumdocker_eth_1 /geth attach ipc://root/.ethereum/devchain/geth.ipc
```

Pour accéder à l'ethereum netstat : http://localhost:3000

### Installer le client
Clonez ce repository puis ouvrez une console dans le dossier :
```
curl -sL https://deb.nodesource.com/setup_7.... | sudo -E bash -
sudo apt-get install -y nodejs
npm install
npm install -g truffle
```
Attention, il faut parfois installer truffle avec sudo selon où sont les fichiers node.

### Configuration
Maintenant, il faut déverouiller le compte courant pour migrer le certificat :
```
truffle console
> web3.personal.unlockAccount(web3.personal.listAccounts[0],"password",15000);
```

On quitte la console et on migre les certificats :
```
truffle migrate
```

On lance ensuite la web app via la commande :
```
npm run dev
```

On peut y accéder via http://localhost:8080
