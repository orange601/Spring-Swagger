# Spring-Swagger
Swagger 사용하기

## Official - 사이트 ##
- https://swagger.io/
- https://swagger.io/tools/swagger-ui/

## OpenAPI? Open API? ##

### Open API ###
- 단어 그대로 개방된 API 이다.
- 예를 들어, 기상청의 단기예보 조회API, 우체국의 우편번호 API 등이 있다. 
- Public API라고도 부른다.

### OpenAPI ###
- penAPI또는 OpenAPI Specification(OAS)라고 부르는데, 이는 RESTful API를 기 정의된 규칙에 맞게 API spec을 json이나 yaml로 표현하는 방식을 의미
- 직접 소스코드나 문서를 보지 않고 서비스를 이해할 수 있다는 장점이 있다.
- 예전에는 Swagger 2.0와 같은 이름으로 불렸다가 현재는 3.0버전으로 올라오면서 OpenApi 3.0 Specification으로 부른다.

출처: https://gruuuuu.github.io/programming/openapi/


## OpenAPI vs Swagger ##
1. 2010년 Tam Wordnik가 **Swagger** 개발 시작   
2. 처음에는 모든걸 포함하는 방법론이 아니라, Wordnik(회사) 자체 API용 UI로 개발    
3. 2015년 초 SmartBear(회사)에서 Swagger 인수   
4. 2015년 말 SmartBear는 Linux Foundation의 후원으로 OpenAPI Initiative에 Swagger를 기부하면서 OpenAPI Specification으로 이름이 변경되었다.
5. 하지만 현재도 Swagger는 사용되는 용어이다.
  - OpenAPI: 이전에 Swagger Specification으로 알려진 Specification 자체 (RESTful API 디자인에 대한 정의(Specification))
  - swagger: OpenAPI를 Implement하기 위한 도구 (SmartBear사의 tool)
  - 즉 Swagger는 API들이 갖고 있는 specification을 정의할 수 있는 툴들 중 하나
