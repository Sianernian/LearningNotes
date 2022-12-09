```
生产组织 和 证书
../bin/cryptogen generate --config=./crypto-config.yaml

创建创世区块	
../bin/configtxgen -profile TwoOrgsOrdererGenesis -outputBlock ./channel-artifacts/genesis.block

创建通道
 export CHANNEL_NAME=mychannel
 ../bin/configtxgen -profile TwoOrgsChannel -outputCreateChannelTx ./channel-artifacts/channel.tx -channelID $CHANNEL_NAME
 
 锚节点更新
 ../bin/configtxgen -profile TwoOrgsChannel -outputAnchorPeersUpdate ./channel-artifacts/Org1MSPanchors.tx -channelID $CHANNEL_NAME -asOrg  Orgs1MSP
 
  ../bin/configtxgen -profile TwoOrgsCh -outputAnchorPeersUpdate ./channel-artifacts/Org2MSPanchors.tx -channelID $CHANNEL_NAME -asOrg  Orgs2MSP

启动网络  docker-compose -f XXXXX.yaml up -d 
进入容器  docker exec -it cli bash

创建通道   echo $CHANNEL_NAME     
          export CHANNEL_NAME=mychannel
peer channel create -o orderer.example.com:7050 -c $CHANNEL_NAME -f ./channel-artifacts/channel.tx --tls --cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem 

节点加入通道
 peer channel join -b mychannel.block
 
 更新锚节点
  peer channel update -o orderer.example.com:7050 -c $CHANNEL_NAME   -f ./channel-artifacts/Org1MSPanchors.tx --tls --cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem 
  
  查看加入当节点的通道
  peer channel list
```

```
安装链码  
channel——name
   peer chaincode install -n xxxcc -v 1.0 -p github.com/chaincode/chaincode_hello
```

```shell
实例化链码instantiate 

peer chaincode instantiate -o orderer.example.com:7050 --tls --cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem  -C $CHANNEL_NAME -n hellocc -v 1.0  -c '{"Args":["init","Hello","World"]}' -P "OR('Org1MSP.peer','Org2MSP.peer')"
```

```shell
查询链码
 peer chaincode query  -n hellocc -c '{"Args":["query","Hello"]}' -C $CHANNEL_NAME
```

```shell
peer chaincode invoke -o orderer.example.com:7050 --tls --cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem  -C $CHANNEL_NAME -n mycc -c '{"Args":["invoke","a","b","10"]}'
```

```
 只有链码的原来示例化的peer可以执行升级操作，即通道的管理者，目前org3不是，而org1和org2是管理者
 
 链码更新只能在安装了新版本链码的节点上有效，老的节点仍然只能用旧的链码，同样，参与背书的节点，也需要将链码升级，才能让提交Invoke的操作背书通过。
 
 被邀请节点 不是 通道管理员身份？
 
 
 https://hyperledger-fabric.readthedocs.io/zh_CN/latest/config_update.html

// 通道配置文件
peer channel fetch config config_block.pb -o xuorder1008 -c xuchannel1008 --tls --cafile /var/lib/nfs/agent/fabric/certs/ordererOrganizations/xuorder1008/orderers/order.xuorder1008/tls/ca.crt
```

