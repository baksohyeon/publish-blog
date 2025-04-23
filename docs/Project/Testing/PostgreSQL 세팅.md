---
category: Project/Testing
share: true
---
mySQL 말고 한번 이것도 써보자~   
  
# Aurora MySQL Vs Aurora PostgreSQL  
[Aurora MySQL vs Aurora PostgreSQL | 우아한형제들 기술블로그](https://techblog.woowahan.com/6550/)  
CRUD 처리는 mySQL 이 빠른데 복잡한 쿼리 처리 및 대규모 데이터 처리 시에는 PostgreSQL가 빠르다.  
  
  
# PostgreSQL 설치하기  
[Set up postgres + database on MacOS (M1) · GitHub](https://gist.github.com/phortuin/2fe698b6c741fd84357cec84219c6667)  
  
Based on [this blogpost](https://www.codementor.io/@engineerapart/getting-started-with-postgresql-on-mac-osx-are8jcopb).  
  
Install with Homebrew:  
  
```bash  
$ brew install postgresql  
```  
  
Starting Services:  
- 깔고나서 이 문구 보면 됨.  
```bash  
To restart postgresql@14 after an upgrade:  
  brew services restart postgresql@14  
Or, if you don't want/need a background service you can just run:  
  /opt/homebrew/opt/postgresql@14/bin/postgres -D /opt/homebrew/var/postgresql@14  
```  
  
  
Run server:  
```bash  
$ brew services start postgresql@14 # restart  
  
$ pg_ctl -D /opt/homebrew/var/postgresql@14 start  
```  
  
  
![Pasted image 20221219174945.png](../../attachments/Pasted%20image%2020221219174945.png)  
  
  
  
Start psql and open database `postgres`, which is the database postgres uses itself to store roles, permissions, and structure:  
  
- 실행  
```bash  
$ psql postgres  
```  
  
Create role for application, give login and `CREATEDB` permissions:  
  
```postgres  
postgres-# CREATE ROLE myuser WITH LOGIN;  
postgres-# ALTER ROLE myuser CREATEDB;  
```  
  
Note that the user has no password. Listing users `\du` should look like this:  
  
```postgres  
postgres-# \du  
  
                                    List of roles  
  Role name  |                         Attributes                         | Member of   
-------------+------------------------------------------------------------+-----------  
 <root user> | Superuser, Create role, Create DB, Replication, Bypass RLS | {}  
 myuser      | Create DB                                                  | {}  
```  
  
Quit psql, because we will login with the new role (=user) to create a database:  
  
```postgres  
postgres-# \q  
```  
  
On shell, open psql with `postgres` database with user `myuser`:  
  
```bash  
$ psql postgres -U myuser  
```  
  
Note that the postgres prompt looks different, because we’re not logged in as a root user anymore. We’ll create a database and grant all privileges to our user:  
  
```postgres  
postgres=> CREATE DATABASE mydatabase;  
postgres⇨ GRANT ALL PRIVILEGES ON DATABASE mydatabase TO myuser;  
```  
  
List databases to verify:  
  
```postgres  
postgres-> \list  
```  
  
![Pasted image 20221219175555.png](../../attachments/Pasted%20image%2020221219175555.png)  
  
If you want to connect to a database and list all tables:  
  
```postgres  
postgres-> \c mydatabase  
mydatabase-> \dt  
```  
  
...should print `Did not find any relations.` for an empty database. To quit the postgres CLI:  
  
```  
mydatabase-> \q  
```  
  
Finally, in a `.env` file for Node.js software development, your database connection string should look like this:  
  
```  
PG_CONNECTION_STRING=postgres://myuser@localhost/mydatabase  
```  
  
  
# Datagrip and Postgres Connection  
[PostgreSQL | DataGrip Documentation](https://www.jetbrains.com/help/datagrip/postgresql.html)  
  
## SQL 네이밍 컨벤션  
[How I Write SQL, Part 1: Naming Conventions](https://launchbylunch.com/posts/2014/Feb/16/sql-naming-conventions/)  
[10 Database Naming Conventions Best Practices - CLIMB](https://climbtheladder.com/10-database-naming-conventions-best-practices/)  
복수로 적으면 안됨~   
  
  
---  
  
## Update 컬럼 쿼리 mySQL 이랑 다름  
## create/update/delete 타임스탬프  
  
[PostgreSQL: Update the Timestamp column with the use of TRIGGER](https://www.dbrnd.com/2016/03/postgresql-update-the-timestamp-column-with-the-use-of-trigger/)  
  
[types - Difference between timestamps with/without time zone in PostgreSQL - Stack Overflow](https://stackoverflow.com/questions/5876218/difference-between-timestamps-with-without-time-zone-in-postgresql)  
  
[PostgreSQL에서 CREATE/UPDATE시의 자동입력 구현](https://idenrai.tistory.com/m/223)  
- Table 작성  
```sql  
create table "user"."user"  
(  
    id         serial,  
    user_name  varchar(255) not null,  
    email      varchar(255) not null,  
    password   varchar(255) not null,  
    created_at timestamp default CURRENT_TIMESTAMP,  
    updated_at timestamp default now()  
);  
  
alter table "user"."user"  
    owner to dorito;  
  
create unique index user_email_uindex  
    on "user"."user" (email);  
  
create trigger trg_update_tbl_user  
    before update  
    on "user"."user"  
    for each row  
execute procedure ???();  
  
  
```  
  
- Create a Trigger function:  
```sql  
CREATE OR REPLACE FUNCTION trigger_set_timestamp()  
RETURNS TRIGGER AS $$  
BEGIN  
  NEW.updated_at = NOW();  
  RETURN NEW;  
END;  
$$ LANGUAGE plpgsql;  
```  
  
In this block of code, we defined our function with a RETURNS TRIGGER. This opens up a special variable for us to use:  
  
`NEW` is a RECORD object. It contains the data that's being inserted or updated. As you can see in the example function, PostgreSQL allows us to read from and write to any field in the `NEW` object before it gets saved to disk.  
  
more information on Postgres trigger variables [here](https://www.postgresql.org/docs/current/plpgsql-trigger.html).  
  
- Create an UPDATE TRIGGER:  
Trigger는 각 테이블에 Function을 연결시키는 역할을 한다.  
무조건 테이블 수만큼 만들어야 한다.  
```sql  
CREATE TRIGGER trg_update_tbl_user BEFORE UPDATE  
ON "user"."user" FOR EACH ROW EXECUTE PROCEDURE  
trg_fn_tbl_user_updated_at();  
```  
  
```sql  
CREATE TRIGGER set_timestamp  
BEFORE UPDATE ON todos  
FOR EACH ROW  
EXECUTE PROCEDURE trigger_set_timestamp();  
```  
  
트리거 아니래서 더 찾았는데 [How to Create Automatic PostgreSQL Timestamps](https://x-team.com/blog/automatic-timestamps-with-postgresql/) → 이거 보고 하면 됨 이거 나중에 정리하기!!!!!   
맞음   
### 트리거가 뭔데  
[Understanding PostgreSQL Triggers: A Comprehensive 101 Guide - Learn | Hevo](https://hevodata.com/learn/postgresql-triggers/)  
  
  
---  
  
[database design - How can I integrate social login to my existing user schema/login system? - Stack Overflow](https://stackoverflow.com/questions/24748208/how-can-i-integrate-social-login-to-my-existing-user-schema-login-system)  
  
  
  
  
# Postgres 비밀번호 설정  
[Postgresql 정리 :: 보안망집](https://99876654.tistory.com/6#:~:text=%E2%97%8F%20%EA%B3%84%EC%A0%95%20%EB%B9%84%EB%B0%80%EB%B2%88%ED%98%B8%20%EC%97%AC%EB%B6%80%20%ED%99%95%EC%9D%B8,%EC%97%AC%EB%B6%80%EB%A5%BC%20%EC%95%8C%20%EC%88%98%20%EC%9E%88%EB%8B%A4.)  
  
  
postgres port number: `5432`  
mysql port number: `3306`  
  
  
# TypeOrm 연결  
[typescript - NestJS Using ConfigService with TypeOrmModule - Stack Overflow](https://stackoverflow.com/questions/52570212/nestjs-using-configservice-with-typeormmodule)  
[Nest.js + TypeORM + PostgreSQL 시작하기 | by readbetweenthelines | Medium](https://medium.com/@feedbotstar/nest-js-typeorm-postgresql-%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0-153c3a55aba1)  
[NestJS DB config that plays well with migrations | Ronen Agranat](https://www.ronenagranat.com/2021-03-01/nestjs-typeorm-environment-config-migration.html)  
[Setting up Nestjs with PostgreSQL | by Dorcas Olajide | Dev Genius](https://blog.devgenius.io/setting-up-nestjs-with-postgresql-ac2cce9045fe)  
[Configure TypeORM by Injecting NestJS Config](https://jaketrent.com/post/configure-typeorm-inject-nestjs-config/)  
[Nest.js에 ConfigModule 설정](https://kscodebase.tistory.com/553?category=1282990)  
[Fetching Title#nc84](https://cocook.tistory.com/m/186)  
  
```shell  
$ yarn add @nestjs/typeorm typeorm pg   
```  
  
이번엔 configService 기능 써서 연결하고 싶음  
  
  
  
  
- [여기 문서 참고](https://stackoverflow.com/questions/52570212/nestjs-using-configservice-with-typeormmodule) 답변 갖고옴  
See [Database | NestJS - A progressive Node.js framework](https://docs.nestjs.com/techniques/database#async-configuration)Async Configuration chapter  
  
  
  
[Database | NestJS - A progressive Node.js framework](https://docs.nestjs.com/techniques/database#async-configuration)  
  
  
  
# ConfigService 사용하여 환경변수 설정하기. (NestJs TypeOrm 연결: PostgreSql)  
  
  
dev, test, production 레벨마다 환경변수 다르게 가져올 때 유용하게 쓸 수 있다고 한다.  
dot env 를 좀더 nest js에 특화되게끔 만든 config 기능을 한번 써보고 싶어서 찾아보았다.  
  
Nestjs에는 ConfigModule이 있다. ConfigModule을 사용해서 미리 .env를 삽입해두면 그 하위 모듈에서는 모두 process.env를 통해서 값에 접근할 수 있게 된다.   
  
  
[출처](https://jaketrent.com/post/configure-typeorm-inject-nestjs-config/)  
import { ConfigService } from '@nestjs/config' is the class that you need to inject and is the type of the useFactory argument. The docs were unclear on where this was imported from or possibly custom-implemented.  
The typing of number for configService.get() and a number (5432) as fallback are important to get right as type for the port property, otherwise the TypeScript compiler is not happy with the useFactory implementation.  
There are so many other examples on the web of people having to create their own ConfigService implementations. The above solution avoids that (even though we ended up going to a custom implementation later ourselves in order to share config with migrations).  
  
  
```ts:app.module.ts  
import { ConfigService } from './config.service'  
import { Module } from '@nestjs/common'  
import { TypeOrmModule } from '@nestjs/typeorm'  
  
@Module({  
  imports: [  
    TypeOrmModule.forRootAsync({  
      imports: [ConfigModule],  
      useFactory: (config: ConfigService) => config.get('database'),  
      inject: [ConfigService],  
    }),  
  ],  
})  
export class AppModule {}  
```  
  
[Database | NestJS - A progressive Node.js framework](https://docs.nestjs.com/techniques/database#async-configuration)  
[NestJS and TypeORM database configuration - DEV Community 👩‍💻👨‍💻](https://dev.to/raphael_haecker/nestjs-and-typeorm-database-configuration-15ob)  
  
  
- config 폴더  
> [!tldr] src/config  
  
  
- google.config.ts  
```ts  
import { registerAs } from '@nestjs/config';  
  
export const googleConfiguration = registerAs('googleOauth', () => ({  
  clientId: process.env.CLIENT_ID,  
  clientSecret: process.env.CLIENT_SECRET,  
  callbackURL: process.env.CALLBACK_URL,  
}));  
```  
  
  
- database.config.ts  
```ts  
import { registerAs } from '@nestjs/config';  
  
export const databaseConfig = registerAs('database', () => ({  
  host: process.env.TYPEORM_HOST,  
  port: parseInt(process.env.TYPEORM_PORT),  
  username: process.env.TYPEORM_USERNAME,  
  password: process.env.TYPEORM_PASSWORD,  
  database: process.env.TYPEORM_DATABASE,  
}));  
  
```  
  
  
- typeorm.config.ts  
```ts  
import { Inject, Injectable } from '@nestjs/common';  
import { ConfigType } from '@nestjs/config';  
import { TypeOrmModuleOptions, TypeOrmOptionsFactory } from '@nestjs/typeorm';  
import { databaseConfig } from './database.config';  
  
@Injectable()  
export class TypeOrmConfigService implements TypeOrmOptionsFactory {  
  constructor(  
    @Inject(databaseConfig.KEY)  
    private dbConfig: ConfigType<typeof databaseConfig>,  
  ) {}  
  
  createTypeOrmOptions(): TypeOrmModuleOptions {  
    return {  
      type: 'postgres',  
      host: this.dbConfig.host,  
      port: this.dbConfig.port || 5432,  
      username: this.dbConfig.username,  
      password: this.dbConfig.password,  
      database: this.dbConfig.database,  
      synchronize: false,  
      migrationsRun: false,  
      entities: ['dist/**/*.entity{.ts,.js}'],  
    };  
  }  
}  
  
```  
  
  
> [!tldr] root   
  
- app.module.ts  
```ts  
import { Module } from '@nestjs/common';  
import { ConfigModule } from '@nestjs/config';  
import { TypeOrmModule } from '@nestjs/typeorm';  
import { AuthModule } from './auth/auth.module';  
import { databaseConfig } from './config/database.config';  
import { googleConfiguration } from './config/google.config';  
import { TypeOrmConfigService } from './config/typeorm.config';  
  
@Module({  
  imports: [  
    ConfigModule.forRoot({  
      load: [databaseConfig],  
      isGlobal: true,  
    }),  
    TypeOrmModule.forRootAsync({  
      useClass: TypeOrmConfigService,  
    }),  
  ],  
  controllers: [],  
  providers: [],  
})  
export class AppModule {}  
  
```  
  
  
[Nest.js에 ConfigModule 설정](https://kscodebase.tistory.com/553?category=1282990)  
모듈은 정적으로 만들어진다. 그리고 dotenv와 달리 Nest에서의 configModule은 정적이다. 그래서 안 된다. 무슨 말이냐면, controller나 service처럼, Module들로 인해 모든 application이 만들어진 다음에야 configModule을 사용할 수 있다는 것이다. module의 생성 시점에는 configModule을 사용할 수 없다.  
  
+ 더 찾아봄  
[Nest <정적 모듈 바인딩 vs 동적 모듈 바인딩>](https://velog.io/@from_numpy/Nest-정적-모듈-바인딩-vs-동적-모듈-바인딩)  
  
정적 라이브러리는 실행할 때 라이브러리의 내용을 모두 메모리에 로드하는 반면에   
동적 라이브러리는 메모리에 이미 존재하는 경우 로드되는 시간과 공간을 아낄 수 있습니다. 하지만 매번 라이브러리의 주소에 접근해야 하기 때문에 오버헤드가 존재해 수행 시간은 정적 라이브러리보다 느립니다.  
  
  
  
---  
  
# Entity 중복 코드 줄이기  
[https://typeorm.io/embedded-entities](https://typeorm.io/embedded-entities)  
  
타임 스탬프   
  
# User Id: Uuid 타입으로 수정  
  
[The Basics Of PostgreSQL UUID Data Type](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-uuid/)