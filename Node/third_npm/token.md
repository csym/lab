# token验证机制

## 第三方库

- [jsonwebtoken](https://www.npmjs.com/package/jsonwebtoken)
- [passport-jwt](https://www.npmjs.com/package/passport-jwt)
- [passport](https://www.npmjs.com/package/passport)

### jsonwebtoken

> 使用方式
```
const jwt = require('jsonwebtoken');

const jwtOptions = {
  secretOrKey: 'uwebserver',
};

const payload = { id: 1 };
const token = jwt.sign(payload, jwtOptions.secretOrKey, { expiresIn: '1 days' });

console.log(`token: ${token}`);

```

### passport-jwt
> 使用方式

```
import { ExtractJwt, Strategy } from 'passport-jwt';

const jwtOptions = {
  passReqToCallback: true,
  jwtFromRequest: ExtractJwt.fromAuthHeaderWithScheme('jwt'),
  secretOrKey: 'uwebserver',
};

const strategy = new Strategy(jwtOptions, ((req, jwtPayload, next) => {
  // console.log('payload received', jwtPayload);
  log.debug('[auth start]');
  userInfoDao.checkUser(jwtPayload.id, req.authority)
    .then((user) => {
      if (user) {
        log.debug('[auth end]');
        next(null, user);
      } else {
        next(null, false);
      }
    });
}));


```

### passport
> 使用方式

```
import passport from 'passport';
passport.use(strategy);

const auth = authority => (req, res, next) => {
  if (typeof authority === 'object') { // restful
    req.authority = authority[req.body.method];
  } else if (authority === 'usmc') { // rabbitmq
    if (cmdMap[req.body.cmd]) {
      req.authority = cmdMap[req.body.cmd];
    } else {
      log.error('[auth] - 该请求 cmd 字段没有对应权限, 请添加! cmd: ', req.body.cmd);
    }
  } else { // authority or undefined
    req.authority = authority;
  }
  passport.authenticate('jwt', { session: false })(req, res, next);
};

```

## 参考资料

- [使用Json Web Token设计Passport系统](https://www.cnblogs.com/binyue/p/4812798.html)
- [express 使用password](https://jonathanmh.com/express-passport-json-web-token-jwt-authentication-beginners/)
