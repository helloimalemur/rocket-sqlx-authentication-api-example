
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

## Session bases auth
    Another alternative is to make use of the session infrastructure available from many containers, e.g. Tomcat. Sessions act as a means to store simple pieces of data against a session ID, while the webapp container manages the storage of these and relates them to the session ID.
    We can use this to implement authentication by storing the logged in user into the session. If a user is present in the session, then this is the user we are authenticated as. If there isn’t a user present, then we aren’t currently authenticated. We can store additional data into the session as needed, such as the user’s set of permissions or anything else that is potentially useful:
# ![img.png](assets/img.png
    Typically these IDs are transmitted by cookies, or by injecting them into the URL. This is much more convenient since the container does a lot of the work for us. However, this is only the case when the application works within these bounds. For example, the injection of session IDs into URLs doesn’t work well in combination with API clients that are generating the URLs themselves.
    The container will also handle the session’s entire lifecycle, including expiring it when it’s no longer needed. Often this is tied not to a specific point in time, but rather to a period of inactivity. This means that the user remains authenticated only for as long as they are actively using the system, and once they finish, the session will expire and they will no longer be authenticated.
    There are complications though. Using sessions depends on access to the storage, requiring all calls from the same client to reach a single server or else configuring session replication between servers. Additionally, we need some way to handle logging out. Common ways to achieve this are to expire the entire session or to clear out the user details.



## database user
    CREATE USER 'dev'@'%' IDENTIFIED WITH sha256_password BY 'password';
    CREATE USER 'dev'@'%' IDENTIFIED BY 'password';
    GRANT ALL PRIVILEGES ON testing.* TO 'dev'@'%';
    FLUSH PRIVILEGES;


## database
    DATABASE_URL="mysql://dev:password@localhost:3306/testing"
