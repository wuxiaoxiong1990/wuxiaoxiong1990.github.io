|名称|是否必须|默认值|说明|例子|
|-|-|-|-|-|
|appl.host|是|无|当前应用发布的IP地址或机器名|appl.host=192.168.10.134|
|appl.port|是|无|当前应用发布的远程服务的端口 不能去ecas项目config配置文件中appl.port重复|appl.port=13884|
|ecas.uri|是|无|远程调用ECAS的地址，它可以配置集群。格式ecas项目config配置文件中appl.host:appl.port，如果有多个用","分隔|ecas.uri=192.168.10.134:13883|
|passConfig|否|classpath*:conf/ecas/client/pass.config|默认的文件在ecas-5.0.jar里，如果有需要修改的，需要把这个jar包里的这个文件删除，然后配置你自己的文件路径|passConfig=classpath*:conf/cim/pass.config|
|constantConfig|否|classpath*:conf/properties/config.properties|虽然这个值不是必须且有默认值，但必须在对应的目录下有这个文件，并且把ECAS相关配置参数写到这个文件里，当然你可以把配置文件写到自己的文件中，然后该值指向该文件|constantConfig=classpath*:conf/properties/test.properties|
|appCode|是|无|当前应用的应用编码|appCode=cim|
|userAdapterImplClass|是|无|用户session设置的适配实现类，ECAS5中默认为``com.erayt.ecas5.adapter.impl.DefaultEcasUserAdapter``，ECAS4乔接的为``com.erayt.ecas.adapter.impl.OldEcasUserAdapter``，如果要有特殊的用户实现的请继承``com.erayt.ecas5.adapter.impl.AbstractUserAdapter``类|userAdapterImplClass=com.erayt.ecas5.adapter.impl.DefaultEcasUserAdapter|
|ecasLoginUrl|是|无|ecas访问根路径|ecasLoginUrl=http://192.168.30.51:8090/ecas|
|applUrl|是|无|当前应用问的协议+地址+端口|applUrl=http://www.xfunds.com:82|
