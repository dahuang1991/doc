# 客户端
## CLI独立测试
```
$conf = new \EasySwoole\Rpc\Config();
$rpc = new \EasySwoole\Rpc\Rpc($conf);
$conf->setServiceName('serviceName');
//开启通讯密钥
//$conf->setAuthKey('123456');

//虚拟一个服务节点
$serviceNode = new \EasySwoole\Rpc\ServiceNode();
$serviceNode->setServiceName('serviceName');
$serviceNode->setServiceIp('127.0.0.1');
$serviceNode->setServicePort(9601);
$serviceNode->setNodeId('asadas');
//设置为永不过期
$serviceNode->setNodeExpire(0);
$rpc->nodeManager()->refreshServiceNode($serviceNode);

go(function ()use($rpc){
    $client = $rpc->client();
    $client->selectService('serviceName')->callAction('a1')->setArg(
        [
            'callTime'=>time()
        ]
    )->onSuccess(function (\EasySwoole\Rpc\Task $task,\EasySwoole\Rpc\Response $response,?\EasySwoole\Rpc\ServiceNode $serviceNode){
        var_dump('success'.$response->getMessage());
    })->onFail(function (\EasySwoole\Rpc\Task $task,\EasySwoole\Rpc\Response $response,?\EasySwoole\Rpc\ServiceNode $serviceNode){
        var_dump('fail'.$response->getStatus());
    })->setTimeout(1.5);

    $client->selectService('serviceName')->callAction('a2')->onSuccess(function (){
        var_dump('succ');
    });
    $client->call(1.5);
});

```
