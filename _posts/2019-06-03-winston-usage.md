---
title: "winston으로 log rotating하기"
classes: wide
categories:
  - nodejs
date: 2019-06-03 19:00:00 -0600
---

### nodejs에 winston 적용

테스트 및 디버깅 뿐만아니라, 통계를 낼 때 필요한 것이 로그이다.  

console.log로 찍는 것도 물론 프로토타입을 개발할 때는 쉽고 간편하다.
그러나, 서비스 단계로 넘어가거나 유지보수를 위해서는 날짜별로 로그를 수집해야하는 것은 물론이고,  

로그가 일정기간 쌓이면 지난 로그들은 자동적으로 삭제해주는 기능이 필요할 수도 있다.  

이럴 때 유용한 노드 모듈은 winston이다.  
다음 코드는 winston을 기본적으로 사용할 수 있도록 설정해주는 코드이다.

```javascript
const winston       = require('winston')
const winstonDaily  = require('winston-daily-rotate-file')
const { createLogger, format, transports } = require('winston')
const { combine, timestamp, label, prettyPrint } = format

const myFormat  = winston.format.printf(info =>
{
    return `${info.timestamp} - ${info.level}: ${info.message}`
})

const logger  = winston.createLogger
({
    format: combine( timestamp({format:'YYYY-MM-DD HH:mm:ss:SSS'}), myFormat),
    
    transports:[
    
            new(winstonDaily)({
            
                filename: 'system_record_%DATE%.log',
                datePattern: 'YYYY-MM-DD',
                maxSize: '50m',
                maxFiles: '7d',
                level:'info'
            }),
            new(winstonDaily)({
            
                filename: 'system_error_record_%DATE%.log',
                datePattern: 'YYYY-MM-DD',
                maxSize: '50m',
                maxFiles: '7d',
                level:'error'      
            }),
    ]
})

```

maxSize는 50mb가 넘어가면 새롭게 로그파일을 생성해서 이어서 작성한다는 의미이다.  
```
developer    51M   5월 31일  22:47   system_record_2019-05-31.log
developer   9.1M   5월 31일  23:59   system_record_2019-05-31.log.1
```

### Logging levels

winstons의 logging level은 [RFC5424](https://tools.ietf.org/html/rfc5424)에 명시된 심각도 수준에 따라 설정되어있다.  

![로깅수준](https://user-images.githubusercontent.com/10937193/58778254-d010c400-860c-11e9-9b3a-1c129bd7df47.JPG)

위와 같이 그대로 사용하려면  
다음과 같이 `levels:winston.config.syslog.levels`코드를 한줄 추가해주면 된다.  

```javascript
const logger  = winston.createLogger
({
    format: combine( timestamp({format:'YYYY-MM-DD HH:mm:ss:SSS'}), myFormat),
    levels: winston.config.syslog.levels,
    transports:[
    
            new(winstonDaily)({
            
                filename: 'system_record_%DATE%.log',
                datePattern: 'YYYY-MM-DD',
                maxSize: '50m',
                maxFiles: '7d',
                level:'info'
            }),
            new(winstonDaily)({
            
                filename: 'system_error_record_%DATE%.log',
                datePattern: 'YYYY-MM-DD',
                maxSize: '50m',
                maxFiles: '7d',
                level:'error'      
            }),
    ]
})
```
각 level은 어떤 정보까지 출력할 것인지 결정하는 것이다.  
하위 level은 상위 level을 모두 포함하여 출력한다.  

예를 들면 error level로 명시해두면 emerg, alert, crit, error까지 포함해서 log가 기록된다.  

logger.log('emerg', "LOG CONTENTS~~~") 와 같이 log 바로 앞에 level을 입력하고 사용하면된다.  

만약, RFC5424에 정의된 대로 사용하지 않고,
npm logging levels를 사용하려면 위에 언급한 levels코드를 삭제하고 아래 사전을 참고하면 된다.  

```
{
    error   : 0,
    warn    : 1,
    info    : 2,
    verbose : 3,
    debug   : 4,
    silly   : 5
}
```

`logger.log('silly', SILLY CONTENTS~~~)` 와 같이 사용하면 된다.


마지막으로 `maxFiles:7d`는 최대 7일의 로그를 저장하고 있겠다는 의미이다.  
로그를 기록한 이후로 현재시각으로부터 일주일 전의 로그는 자동으로 삭제하는 것이다.  

본 포스팅의 내용으로도 충분한 로깅 시스템을 구현할 수 있으며 추가적인 정보를 원하면 winston github을 방문해보는 것을 권한다.  
