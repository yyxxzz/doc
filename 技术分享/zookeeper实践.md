#zookeeper 实践
	Zookeeper,一种分布式应用的协作服务,是Google的Chubby一个开源的实现,是Hadoop的分布式协调服务,它包含一个简单的原语集,应用于
	分布式应用的协作服务,使得分布式应用可以基于这些接口实现诸如同步、配置维护和分集群或者命名的服务。

	zookeeper是一个由多个service组成的集群,一个leader,多个follower,每个server保存一份数据部分,全局数据一致,分布式读写,更新
	请求转发由leader实施.更新请求顺序进行,来自同一个client的更新请求按其发送顺序依次执行,数据更新原子性,一次数据更新要么成功,
	要么失败,全局唯一数据试图,client无论连接到哪个server,数据试图是一致的.

##为什么要用zookeeper
	大部分分布式应用需要一个主控、协调器或控制器来管理物理分布的子进程（如资源、任务分配等）,目前,大部分应用需要开发私有的协调程序,
	缺乏一个通用的机制.协调程序的反复编写浪费,且难以形成通用、伸缩性好的协调器,ZooKeeper：提供通用的分布式锁服务,用以协调分布式应用

##zookeeper工作原理
	zookeeper的核心是原子广播,这个机制保证了各个server之间的同步,实现这个机制的协议叫做Zab协议.Zab协议有两种模式,他们分别是恢复
	模式和广播模式.
	
	当服务启动或者在领导者崩溃后,Zab就进入了恢复模式,当领导着被选举出来,且大多数server都完成了和leader的状态同步后,恢复模式就结
	束了.状态同步保证了leader和server具有相同的系统状态.
	一旦leader已经和多数的follower进行了状态同步后,他就可以开始广播消息了,即进入广播状态.这时候当一个server加入zookeeper服务
	中,它会在恢复模式下启动,发下leader,并和leader进行状态同步,待到同步结束,它也参与广播消息.
	
*注意*
	
	广播模式需要保证proposal被按顺序处理,因此zk采用了递增的事务id号(zxid)来保证.所有的提议(proposal)都在被提出的时候加上zxid.
	实现中zxid是一个64为的数字,它高32位是epoch用来标识leader关系是否改变,每次一个leader被选出来,它都会有一个新的epoch.低32位是
	个递增计数.

##leader选举
	每个Server启动以后都询问其它的Server它要投票给谁,对于其他server的询问,server每次根据自己的状态都回复自己推荐的leader的id和
	上一次处理事务的zxid（系统启动时每个server都会推荐自己）,收到所有Server回复以后,就计算出zxid最大的哪个Server,并将这Server
	相关信息设置成下一次要投票的Server.计算这过程中获得票数最多的的sever为获胜者,如果获胜者的票数超过半数,则改server被选leader
	.否则,继续这个过程,直到leader被选举出来.leader就会开始等待server连接,Follower连接leader,将最大的zxid发送给leader,Leader
	根据follower的zxid确定同步点,完成同步后通知follower 已经成为uptodate状态,Follower收到uptodate消息后,又可以重新接client
	的请求进行服务了.

##zookeeper的数据模型
	每个节点在zookeeper中叫做znode,并且其有一个唯一的路径标识节点Znode可以包含数据和子节点,但是EPHEMERAL类型的节点不能有子节点
	Znode中的数据可以有多个版本,比如某一个路径下存有多个数据版本,那么查询这个路径下的数据就需要带上版本客户端应用可以在节点上设置
	监视器,节点不支持部分读写,而是一次性完整读写Zoopkeeper 提供了一套很好的分布式集群管理的机制,就是它这种基于层次型的目录树的数
	据结构,并对树中的节点进行有效管理,从而可以设计出多种多样的分布式的数据管理模型.

##zookeeper的节点
	znode有两种类型,短暂的（ephemeral）和持久的（persistent）

	znode的类型在创建时确定并且之后不能再修改,短暂znode的客户端会话结时,zookeeper会将该短暂znode删除,短暂znode不可以有子节点,
	持久znode不依赖于客户端会话,只有当客户端明确要删除该持久znode时才会被删除.

	znode有四种形式的目录节点PERSISTENT、PERSISTENT_SEQUENTIAL、EPHEMERAL、EPHEMERAL_SEQUENTIAL.

	znode 可以被监控,包括这个目录节点中存储的数据的修改,子节点目录的变化等,一旦变化可以通知设置监控的客户端,这个功能是zookeeper
	对于应用最重要的特性,通过这个特性可以实现的功能包括配置的集中管理,集群管理,分布式锁等等.

##zookeeper的角色
	领导者（leader）,负责进行投票的发起和决议,更新系统状态.

	学习者（learner）,包括跟随者（follower）和观察者（observer）.follower用于接受客户端请求并想客户端返回结果,在选主过程中参与
	投票,observer可以接受客户端连接,将写请求转发给leader,但observer不参加投票过程,只同步leader的状态,observer的目的是为了扩展
	系统,提高读取速度.

	客户端（client）,请求发起方.

	watcher
	watcher 在 zooKeeper 是一个核心功能,Watcher 可以监控目录节点的数据变化以及子目录的变化,一旦这些状态发生变化,服务器就会通知
	所有设置在这个目录节点上的 Watcher,从而每个客户端都很快知道它所关注的目录节点的状态发生变化,而做出相应的反应可以设置观察的操
	作：exists,getChildren,getData可以触发观察的操作：create,delete,setDataznode以某种方式发生变化时,“观察”（watch）机制可
	以让客户端得到通知.可以针对zooKeeper服务的“操作”来设置观察,该服务的其他 操作可以触发观察.比如,客户端可以对某个客户端调exists
	操作,同时在它上面设置一个观察,如果此时这个znode不存在,则exists返回 false,如果一段时间之后,这个znode被其他客户端创建,则这个
	观察会被触发,之前的那个客户端就会得到通知.

	