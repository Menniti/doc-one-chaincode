Pre requisitos
Docker, Node, Npm

## Setup de ambiente

``` 
npm install -g composer-cli
npm install -g generator-hyperledger-composer
npm install -g composer-rest-server
npm install -g yo

```

## Limpar o containers do Docker
(tome cuidado se o docker estiver usando em outros projetos)

``` 
docker kill $(docker ps -q)
docker rm $(docker ps -aq)
docker rmi $(docker images dev-* -q)

```

## Remova todas as credentials que o composer tem
```
rm -rf ~/.composer-connection-profiles
rm -rf ~/.composer-credentials
rm -fr ~/.composer
rm -rf ~/.composer-connection-profiles/hlfv1
rm -f ~/.composer-credentials/*
```

## set variables de ambiente para Fabric v1.0
```
export FABRIC_VERSION=hlfv1
export FABRIC_START_TIMEOUT=30
```


## Dando Start ao Hyperledger Fabric (Esse e o que a business network connecta e uma serie de docker containers)

```
mkdir ~/fabric-tools && cd ~/fabric-tools
curl -O https://raw.githubusercontent.com/hyperledger/composer-tools/master/packages/fabric-dev-servers/fabric-dev-servers.tar.gz
tar xzf fabric-dev-servers.tar.gz
```

Na pasta fabric-tools

```
./downloadFabric.sh
./stopFabric.sh
./teardownFabric.sh
./startFabric.sh
./createPeerAdminCard.sh

```

## Setup comandos do projeto

## Instale as dependencias do chaincode

```
~/mvp-docone-network
npm install
```

## Crie uma pasta chamada dist

```
mkdir dist

```

## Agora crie um arquivo um chaincode binario (.bna) para ser instalado em nossa rede.

```
composer archive create --sourceType dir --sourceName . --archiveFile ./dist/docone-network.bna 
```

## Deploy a network no peer

```
composer network deploy --card PeerAdmin@hlfv1 --networkAdmin adminname --networkAdminEnrollSecret adminPassword --archiveFile ./dist/docone-network.bna  --file docone-network-admin-card.card

```

## Importando o networkadmin.card para nossa lista de cards

```
composer card import --file docone-network-admin-card.card
```

## Verifique se o card foi importado corretamente.

```
composer card list
```

## Testando nossa network.

```

composer network ping -c adminname@mvp-docone-network
    

   The connection to the network was successfully tested: perishable-network
	version: 0.16.0
	participant: org.hyperledger.composer.system.NetworkAdmin#admin

```
