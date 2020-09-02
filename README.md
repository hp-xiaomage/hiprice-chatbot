你好！
很冒昧用这样的方式来和你沟通，如有打扰请忽略我的提交哈。我是光年实验室（gnlab.com）的HR，在招Golang开发工程师，我们是一个技术型团队，技术氛围非常好。全职和兼职都可以，不过最好是全职，工作地点杭州。
我们公司是做流量增长的，Golang负责开发SAAS平台的应用，我们做的很多应用是全新的，工作非常有挑战也很有意思，是国内很多大厂的顾问。
如果有兴趣的话加我微信：13515810775  ，也可以访问 https://gnlab.com/，联系客服转发给HR。
[中文文档](README_CN.md)

# hiprice-chatbot

Chatbot for HiPrice.

## HiPrice
HiPrice is a wechat bot for personal account.

It receives product links and crawls regularly, you will be notified when its price goes up/down.

_I have already deployed such a bot, if you want to use this price notification service or have a look, add wechat friend __hiprice001__._

![](assets/welcome1.png)



![](assets/welcome2.png)



Currently it supports the following websites:

- taobao.com/tmall.com
- jd.com
- suning.com
- amazon.cn
- vip.com
- jumei.com
- mogujie.com
- kaola.com

## Docker

```
// build
docker image build -f Dockerfile -t hiprice-chatbot .

// run
docker container run -d --name hiprice-chatbot -p 6200:6200 --link mariadb:mariadb --link beanstalk:beanstalk hiprice-chatbot

// if you do not want to build yourself, a default image is ready in use
docker container run -d --name hiprice-chatbot -p 6200:6200 --link mariadb:mariadb --link beanstalk:beanstalk wf2030/hiprice-chatbot:0.1.0
```

### MariaDB

Image: `wf2030/mariadb:10.3`, or you can build it yourself in mariadb directory.

Run: `docker container run -d --name mariadb -p 3306:3306 -e MYSQL_ROOT_PASSWORD=root wf2030/mariadb:10.3`

### Beanstalk

Image: `wf2030/beanstalk:1.11`, or you can build it yourself in beanstalk directory.

Run: `docker container run -d --name beanstalk -p 11300:11300 wf2030/beanstalk:1.11`

### Tips

Currently the whole project has 4 sub projects(now is refactoring):

- [hiprice-chatbot](https://github.com/kwf2030/hiprice-chatbot)
  Chat bot for HiPrice. Also contains admin console for bot login. Requires MySQL/MariaDB and Beanstalk.
- [hiprice-dispatcher](https://github.com/kwf2030/hiprice-dispatcher)
  Dispatcher for HiPrice. Collects product links and dispatches to runners. Requires MySQL/MariaDB and Beanstalk.
- [hiprice-runner](https://github.com/kwf2030/hiprice-runner)
  Crawler for HiPrice. Deploy as many runners as you can, they are "distributed". Requires Beanstalk and Chrome/Chromium.
- [hiprice-web](https://github.com/kwf2030/hiprice-web)
  Web for HiPrice. Manage your own watched products.

You may need the [docker-compose](docker-compose.yaml) that lauches all services in one step, the docker-compose does not contain hiprice-runner, compiles and runs it manually.

While all services get up, go to http://localhost:6200/admin to get your wechat bot login, and then congratulations, your bot is working! Send it "help" to see how to play.

__Warning: wechat bot uses remark as persistent scheme, it will remark all your contacts with sequence number while you login, that means all your remarks before will be lost and can not restore, use it in caution(you can use my bot service hiprice001 for testing purpose, or  you can apply a new account).__
