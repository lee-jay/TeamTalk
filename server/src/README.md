#编译说明
###编译环境
	TeamTalk编译需要依赖一些最新的c++标准, 建议使用CentOS 7.0, 如果使用的是CentOS 6.x,需要将g++版本升至支持c++11特性,升级脚本可以使用自动安装脚本目录下的gcc_setup

###第三方库
	TeamTalk使用了许多第三方库，包括protobuf,hiredis,mariadb(mysql),log4cxx等等,在第一次编译TeamTalk之前,建议先执行目录下的
		protobuf: make_protobuf.sh 
		hiredis: make_hiredis.sh
		mariadb: make_mariadb.sh
		log4cxx: make_log4cxx.sh
	这些脚本执行完后会自动将头文件和库文件拷贝至指定的目录
	
###编译TeamTalk服务器
	当以上步骤都完成后，可以使用"./build.sh version 1"编译整个TeamTalk工程,一旦编译完成，会在上级目录生成im_server_x.tar.gz包，该压缩包包含的内容有:
	sync_lib_for_zip.sh: 将lib目录下的依赖库copy至各个服务器的目录下，启动服务前需要先执行一次该脚本
	lib: 主要包含各个服务器依赖的第三方库
	restart.sh: 启动脚本，启动方式为./restart.sh msg_server
	login_server:
	msg_server:
	route_server:			
	db_proxy_server:
	file_server:
	push_server:
	msfs:

QA:
1. configure: error: C++ preprocessor "/lib/cpp" fails sanity check
according to some articles on web:
http://forum.ubuntu.org.cn/viewtopic.php?f=85&t=102970
出现该情况是由于c++编译器的相关package没有安装，以超级用户登陆，在终端上执行：
#yum install glibc-headers
#yum install gcc-c++

2. Existing lock /var/run/yum.pid: another copy is running as pid
解决办法:
yum只能支持一个例程运行，所以如果有一个例程已经在运行，其他的必须等待该进程退出释放lock。出现这种情况时，可以用以下命令来恢复：
rm -f /var/run/yum.pid

3. 有make_protobuf.sh，但无法执行
因为权限不对，没有可执行权限（绿色）
解决办法:
chmod 0777 make_protobuf.sh

4. Error: download MariaDB-10.0.17-centos6-x86_64-devel.rpm failed
HTTP request sent, awaiting response... 404 Not Found
2017-11-02 06:20:46 ERROR 404: Not Found.
Error: download MariaDB-10.0.17-centos6-x86_64-devel.rpm failed

解决办法:
修改make_mariadb.sh文件，将MariaDB的包名中的版本号和url中的版本号修改为http://sfo1.mirrors.digitalocean.com/mariadb上对应大版本中的最新feature号即可。
比如，将10.0.17修改为10.0.33


