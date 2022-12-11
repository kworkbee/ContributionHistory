[![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fgithub.com%2Fkworkbee%2FContributionHistory&count_bg=%2379C83D&title_bg=%23555555&icon=&icon_color=%23E7E7E7&title=hits&edge_flat=false)](https://hits.seeyoufarm.com)

# ContributionHistory

- ![In Review](https://img.shields.io/badge/-in%20review-red) [spring-cloud/spring-cloud-netflix](https://github.com/spring-cloud/spring-cloud-netflix) [#4148](https://github.com/spring-cloud/spring-cloud-netflix/pull/4148)
  - Eureka Client가 Peer-configured Server에 Heartbeat를 보낼 때 서버가 Increased Network Latency로 인해 응답을 하지 못하는 경우 내부적으로 `RestTemplate`의 Default Timeout이 지정되지 않아 infinitely waiting 하는 상황이 발생함
  - 이로 인해서 `RetryableEurekaHttpClient`가 해당 서버 Peer를 `quarantineSet`에 포함하지 못하기 때문에 `HeartBeatThread`가 매 interval마다 재실행되어도 동일하게 무응답 서버 Peer로 request를 보냄
  - `RestTemplateEurekaHttpClient`의 `RestTemplate` Default Timeout을 설정하여 `RetryableEurekaHttpClient`에서 `TransportException`이 발생할 수 있도록 Fast-fail 시켜야함
  - property 주입이 가능하도록 코드 변경
