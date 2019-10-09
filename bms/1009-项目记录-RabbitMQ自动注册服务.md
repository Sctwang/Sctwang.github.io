## 1009-项目记录-RabbitMQ 自动注册微服务

> 本片接 [1008-项目记录-PERMISSION_MISMATCH] 开始，该文章中有说明 RabbitMQ 会自动注册服务，本文中将探究原理。



## RabbitMQ 发送消息

要先启动 oauth-server 服务



## Rabbit 客户端位置

Oauth-server

io.choerodon.oauth.domain.service.permissionServer

 

~~~java
	@Override
    public Boolean insertList(List<PermissionDO> permissionDOS) {
        //已经存在的执行更新操作，未插入的执行插入
        permissionDOS.forEach(a -> {
            //根据code查询
            PermissionDO permissionDO = permissionMapper.selectByCode(a);
            //不为空执行更新操作
            if (permissionDO != null) {
                //设置id，objectVersionNumber
                a.setId(permissionDO.getId());
                a.setObjectVersionNumber(permissionDO.getObjectVersionNumber());
                if (permissionMapper.updateByPrimaryKeySelective(a) != 1) {
                    LOGGER.error("iam_permission更新失败" + a.toString());
                }
                ;
            } else {
                if (permissionMapper.insert(a) != 1) {
                    LOGGER.error("iam_permission插入失败" + a.toString());
                }
            }
        });
        return true;
    }
~~~







## Rabbit 服务端位置

Choerodon-starter-oauth-resource

io.choerodon.resource.handler.RequestMappingHandler



