geth -datadir ./ init XXXXX.json

geth -datadir ./  (-port value) （-ipcdisable） (-nodiscover) console 2 >> geth.log

 

personal.newAccount()创建账户

​	admin.nodeInfo.enode 查看节点信息 n1

​	手动添加节点  admin.addPeer( n1 )

​	admin.Peers 查看当前节点信息

​	节点同步 选择一个节点尝试区块

​	eth.bolckNumber 查看同步区块数量

​	admin.removePeer( n1 ) 移除节点

personal.newAccount()

miner.start()

miner.stop()





