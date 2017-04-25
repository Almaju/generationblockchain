# Génération Blockchain
Ce projet utlise Ethereum pour le protocole, un container Docker pour le cluster privé, NodeJS et Truffle pour la dApp.
Il s'agit d'un MVP créé pour prouver la possiblité d'utiliser la technologie de la blockchain afin de proposer un système de vote en AG pour les entreprises.
Tous les tests ont été effectués via des machines Ubuntu sur des instances AWS.

## Mode d'emploi
### Créer le cluster privé Ethereum
Clonez ce repository : https://github.com/Capgemini-AIE/ethereum-docker et installez Docker ainsi que Docker Compose. 
```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
sudo apt-get update
sudo apt-get install docker-ce docker-compose
```

Puis executez cette commande dans le dossier ethereum-docker.
```
sudo docker-compose up -d
```

Pour augmenter ou réduire le nombre de noeuds :
```
sudo docker-compose scale eth=3
```

Pour se connecter à la console geth (pour intéragir directement avec la blockchain) :
```
sudo docker exec -it ethereumdocker_eth_X /geth attach ipc://root/.ethereum/devchain/geth.ipc
```
Dans la console geth, on peut notammenter lancer le minage avec `miner.start();` ou le stopper avec `miner.stop();` (voir la doc de geth).
Attention, dans `ethereumdocker_eth_X` il faut remplacer X par le numéro du noeud que vous souhaitez configurer.

Pour accéder à l'ethereum netstat : http://localhost:3000

### Installer la dApp
Clonez ce repository puis ouvrez une console dans le dossier :
```
curl -sL https://deb.nodesource.com/setup_7.x | sudo -E bash -
sudo apt-get install -y nodejs build-essential
npm install
npm install -g truffle
```
Attention, il faut parfois installer truffle avec sudo selon où sont les fichiers node.

### Configuration
Maintenant, il faut déverouiller le compte courant pour migrer le certificat :
```
truffle console
> web3.personal.unlockAccount(web3.personal.listAccounts[0],"",15000);
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

### Utilisation
Installez MetaMask pour interagir avec les wallets : https://chrome.google.com/webstore/detail/metamask/nkbihfbeogaeaoehlefnkodbefgpgknn?authuser=2
Il faut configurer MetaMask pour pointer sur l'IP sur cluster.

### Personnalisation
Model :
/contracts/Voting.sol

Controller :
/app/javascripts/app.js

View :
/app/index.html

### Sources
Docker : https://capgemini.github.io/blockchain/ethereum-docker-compose/
dApp : https://medium.com/@mvmurthy/full-stack-hello-world-voting-ethereum-dapp-tutorial-part-2-30b3d335aa1f
