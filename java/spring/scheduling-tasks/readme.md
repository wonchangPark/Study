Scheduled 어노테이션은 특정 메소드가 실행될 때를 정의 한다.   
cron : cron표현식을 지원한다. "초 분 시 일 월 주 (년)"으로 표현한다. cron표현식에 쓸 수 있는 것들(특수문자 활용 포함)이 많은데 해당 내용이 핵심이 아니므로 다른 블로그에서 확인해보기를 바란다.   
fixedDelay : milliseconds 단위로, 이전 작업이 끝난 시점으로 부터 고정된 시간을 설정한다. ex) fixedDelay = 5000   
fixedDelayString : fixedDelay와 같은데 property의 value만 문자열로 넣는 것이다. ex) fixedDelay = "5000"   
fixedRate : milliseconds 단위로, 이전 작업이 수행되기 시작한 시점으로 부터 고정된 시간을 설정한다. ex) fixedRate = 3000  
fixedRateString : fixedDelay와 같은데 property의 value만 문자열로 넣는 것이다. ex) fixedRate = "3000"   
initialDelay : 스케줄러에서 메서드가 등록되자마자 수행하는 것이 아닌 초기 지연시간을 설정하는 것이다.   
initialDelayString : 위와 마찬가지로 문자열로 값을 표현하겠다는 의미다.   
zone : cron표현식을 사용했을 때 사용할 time zone으로 따로 설정하지 않으면 기본적으로 서버의 time zone이다.   
여기에서는 fixedRate을 사용하고 있으므로 이전 작업이 수행하기 시작한 시점으로부터 5000 milisecond 후에 이 메소드를 발생한다.   

@EnableScheduling 어노테이션은 background task executor가 생기게 한다. 이게 없으면 스케줄링은 할 수 없다.   

@Component 어노테이션은 Bean Configuration 파일에 Bean을 따로 등록하지 않아도 사용할 수 있다.   
