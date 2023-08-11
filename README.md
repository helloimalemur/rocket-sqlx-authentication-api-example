
## Tables needed;
```sql
    CREATE TABLE `users` (`id` int(11) NOT NULL AUTO_INCREMENT,
    `username` varchar(255) NOT NULL,
    `password` varchar(255) NOT NULL,
    `email` varchar(255) NOT NULL,
    `first_name` varchar(255) NOT NULL,
    `last_name` varchar(255) NOT NULL,
    PRIMARY KEY (`id`));
```

```sql
    CREATE TABLE `web_sessions` (`id` int(11) NOT NULL AUTO_INCREMENT,
    `user_name` varchar(255) NOT NULL,
    `session_id` varchar(255) NOT NULL,
    `date_created` varchar(255) NOT NULL,
    PRIMARY KEY (`id`));
```


[//]: # (```sql)

[//]: # (    CREATE TABLE `login` &#40;`id` int&#40;11&#41; NOT NULL AUTO_INCREMENT,)

[//]: # (    `username` varchar&#40;255&#41; NOT NULL,)

[//]: # (    `password` varchar&#40;255&#41; NOT NULL,)

[//]: # (    `ipaddress` varchar&#40;255&#41; NOT NULL,)

[//]: # (    PRIMARY KEY &#40;`id`&#41;&#41; ENGINE=InnoDB AUTO_INCREMENT=7 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_general_ci;)

[//]: # (```)



## functions;
```shell
# create user;
curl -XPOST -H 'Content-Type:application/json' -H 'x-api-key:yourapikey' http://127.0.0.1:8030/api/adduser -d '{"username": "foxx","password": "doxx","email": "test","first_name": "test","last_name": "test"}'
```

```shell
# login;
curl -XPOST -H 'Content-Type:application/json' http://127.0.0.1:8030/api/login -d '{"username": "foxx","password": "doxx","ipaddress": "0.0.0.0"}'
```
```shell
# verify user / get username by session;
curl -XGET -H 'x-api-key:yourapikey' http://127.0.0.1:8030/api/<session_id>
```
```shell
# verify session;
curl -XGET http://127.0.0.1:8030/api/verify/sessionid
```
```shell
# logout;
curl -XGET http://127.0.0.1:8030/api/logout/<session_id>
```

## functions for later;
    delete user
    modify user (change password)

### Resources
    https://www.baeldung.com/cs/tokens-vs-sessions
    https://api.rocket.rs/v0.4/rocket/http/enum.Cookies.html
    https://api.rocket.rs/v0.4/rocket/request/trait.FromRequest.html
    https://rocket.rs/v0.5-rc/guide/requests/#custom-guards
    https://api.rocket.rs/v0.5-rc/rocket/request/trait.FromRequest.html
    https://stackoverflow.com/questions/69377336/how-to-get-state-in-fromrequest-implementation-with-rocket
    https://stackoverflow.com/questions/73868771/rust-rocket-with-sqlx-test-database-endpoints

## Session based auth
    We can use this to implement authentication by storing the logged in user into the session cookie. If a user is present with the session cookie, then this is the user we are authenticated as. If there isn’t a user present, then we aren’t currently authenticated. We can store additional data into the session as needed, such as the user’s set of permissions or anything else that is potentially useful.
    Typically session IDs are transmitted by cookies, or by injecting them into the URL.

## database user
    CREATE USER 'dev'@'%' IDENTIFIED WITH sha256_password BY 'password';
    CREATE USER 'dev'@'%' IDENTIFIED BY 'password';
    GRANT ALL PRIVILEGES ON testing.* TO 'dev'@'%';
    FLUSH PRIVILEGES;


## database
    DATABASE_URL="mysql://dev:password@localhost:3306/testing"
