1、maven常用参数和命令 主要介绍maven常用参数和命令以及简单故障排除

1.1 mvn常用参数 

mvn -e 显示详细错误 

mvn -U 强制更新 snapshot类型的插件或依赖库（否则maven一天只会更新一次snapshot依赖）

mvn -o 运行offline模式，不联网更新依赖 

mvn -N仅在当前项目模块执行命令，关闭reactor 

mvn -pl module_name在指定模块上执行命令 

mvn -ff 在递归执行命令过程中，一旦发生错误就直接退出 

mvn -Dxxx=yyy指定java全局属性 

mvn -Pxxx引用profile xxx

1.2 首先是2.4 Build Lifecycle中介绍的命令

 mvn test-compile 编译测试代码

 mvn test 运行程序中的单元测试

mvn compile 编译项目 

mvn package 打包，此时target目录下会出现maven-quickstart-1.0-SNAPSHOT.jar文件，即为打包后文件

 mvn install 打包并安装到本地仓库，此时本机仓库会新增maven-quickstart-1.0-SNAPSHOT.jar文件。 每个phase都可以作为goal，也可以联合，如之前介绍的mvn clean install

1.3 maven 日用三板斧

 mvn archetype:generate 创建maven项目 

mvn package 打包，上面已经介绍过了 

mvn package -Prelease打包，并生成部署用的包，比如deploy/*.tgz 

mvn install 打包并安装到本地库 

mvn eclipse:eclipse 生成eclipse项目文件 

mvn eclipse:clean 清除eclipse项目文件

 mvn site 生成项目相关信息的网站

1.4 maven插件常用参数

mvn -Dwtpversion=2.0 指定maven版本 

mvn -Dmaven.test.skip=true 如果命令包含了test phase，则忽略单元测试

 mvn -DuserProp=filePath 指定用户自定义配置文件位置 

mvn -DdownloadSources=true -Declipse.addVersionToProjectName=true eclipse:eclipse 生成eclipse项目文件，尝试从仓库下载源代码，并且生成的项目包含模块版本（注意如果使用公用POM，上述的开关缺省已打开）

1.5 maven简单故障排除

mvn -Dsurefire.useFile=false如果执行单元测试出错，用该命令可以在console输出失败的单元测试及相关信息 set MAVEN_OPTS=-Xmx512m -XX:MaxPermSize=256m 调大jvm内存和持久代，maven/jvm out of memory error 

mvn -X maven log level设定为debug在运行 

mvn debug 运行jpda允许remote debug 

mvn –help 这个就不说了。。