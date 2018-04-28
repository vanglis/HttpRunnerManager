HttpRunnerManager
=================

Design Philosophy
-----------------

基于HttpRunner的接口自动化测试平台: `Requests`_, `unittest`_ and `Django`_.

Key Features
------------

- 项目管理模块：新增项目，列表展示及相关操作
- 模块管理：为项目新增模块，子模块用module1>module2方式
- 用例管理：分为添加config与test子功能，config定义全部变量和request等相关信息
- 业务组织方式：在include输入框中以 config>case1>case2...或者case1>case2
- 报告管理：单个运行会前端显示，批量的运行后可以选在打开
- 运行方式：可单个test，单个module，单个project，也可选择多个批量运行
- 脚本编写：可在线编写，也支持用例批量导入(yaml格式或json格式)

Deployment Setp
---------------
1. 安装mysql数据库服务端(推荐5.7+),并设置为utf-8编码，创建相应HttpRunner数据库，设置好相应用户名、密码，启动mysql

2. 修改:HttpRunnerManager/HttpRunnerManager/settings.py里DATABASES字典相关配置：NAME(默认HttpRunner)
   USER(用户名，建议root用户，需要有增删改查权限！)、PASSWORD(对应登录用户名密码)、HOST(数据库所在服务器ip地址)
   PORT(数据库服务监听端口，默认3306)
3. 安装rabbitmq消息中间件，service rabbitmq-server start启动服务，访问：http://host:15672/#/ host即为你部署rabbitmq的服务器ip地址
   username：guest、Password：guest, 成功登陆即可

4. 修改:HttpRunnerManager/HttpRunnerManager/settings.py里BROKER_URL = 'amqp://guest:guest@127.0.0.1:5672//'将127.0.0.1替换为步骤3的host

5. 命令行窗口执行pip install -r requirements.txt 安装工程所依赖的库文件

6. 命令行窗口切换到HttpRunnerManager目录，执行python manage.py makemigrations ApiManager 生成数据库迁移脚本

7. 执行python manage.py migrate 对应HttpRunner数据库生成相应表结构

8. 执行python manage.py createsuperuser 根据提示输入用户名，邮箱，密码

9. 执行python manage.py runserver

10. shell或dos窗口切换到HttpRunnerManager目录执行：python manage.py celery -A HttpRunnerManager worker --loglevel=info 启动worker

11. shell或dos窗口切换到HttpRunnerManager目录执行：python manage.py celery beat --loglevel=info 开启定时任务配置

12. CLI窗口执行：celery flower 访问：http://localhost:5555/dashboard 即可查看任务列表和状态

13. 浏览器输入：http://127.0.0.1:8000/api/register/  注册用户，开始享用

14. 浏览器输入http://127.0.0.1:8000/admin/  输入步骤6设置的用户名、密码，登录后台运维管理系统
