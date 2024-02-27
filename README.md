
<a name="H0rPr"></a>
## server
![image.png](https://cdn.nlark.com/yuque/0/2024/png/28909512/1709019749972-fad3d722-5a93-4919-87c1-a5a98529d53d.png#averageHue=%232a2724&clientId=ub8f55431-2add-4&from=paste&height=154&id=u465fb145&originHeight=154&originWidth=595&originalType=binary&ratio=1&rotation=0&showTitle=false&size=22959&status=done&style=none&taskId=u3a6111fa-8470-4a28-81b9-019640c52c6&title=&width=595)
```shell
#import image
docker load -i git-webhook.tar.gz
docker load -i mysql.tar.gz
#start
docker-compose up 
```

```yaml
version: '3'
services:
  
  mysql:
    image: mysql:latest
    ports:
      - "3306:3306"
    environment:
      MYSQL_ROOT_PASSWORD: 123456
    volumes:
      - ./data:/var/lib/mysql
      - ./webhook_db.sql:/docker-entrypoint-initdb.d/webhook_db.sql
      
  git-webhook:
    image: git-webhook:v03
    ports:
      - "8181:8181"
    links:
      - mysql
    depends_on:
      - mysql
    restart: always
    volumes:
      - ./server/conf:/project/server/conf
      - ./server/logs:/project/server/logs
    working_dir: /project/server/
    command: sh -c "sleep 3; ./go-git-webhook"
    
```

<a name="ivP5z"></a>
## client
![image.png](https://cdn.nlark.com/yuque/0/2024/png/28909512/1709019776040-3c6cdaa0-034d-4ac0-965b-ac092f333507.png#averageHue=%232b2724&clientId=ub8f55431-2add-4&from=paste&height=121&id=u33fded85&originHeight=121&originWidth=634&originalType=binary&ratio=1&rotation=0&showTitle=false&size=17580&status=done&style=none&taskId=u6ddd625e-eb4a-42b7-b509-8865d4d47de&title=&width=634)
```yaml
#import image
docker load -i git-webhook-client.tar.gz
#start
docker-compose up
```
```yaml
version: '3'
services:
  git-webhook-client:
    image: git-webhook-client:v02
    ports:
      - 8081:8081
    restart: always
    volumes:
      - ./client/conf:/project/client/conf
    working_dir: /project/client/
    command: ./go-git-webhook-client
```
