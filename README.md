# Spring-Swagger
Swagger 사용하기

## Swagger란 ##
- API 스펙 명세 및 관리를 목적으로 하며, API에 대해 테스트해 볼 수 있는 UI를 제공해주는 개발도구
- 공식 사이트: https://swagger.io/
- 공식 사이트: https://swagger.io/tools/swagger-ui/

## Springfox ##
- 공식 사이트: https://springfox.github.io/springfox/docs/current/
- Spring에서 Swagger를 쉽게 사용하기 위한 기능을 제공한다. 
- Spring에서 API 자동 문서화를 수행
- maven repository springfox-boot-starter는 Jul 14, 2020 이후로 업데이트가 되지 않고 있다.
- [공식 사이트](https://springfox.github.io/springfox/docs/current/)에 접속해서 제일 아래에 "Last updated 2020-06-23 13:45:06" 라고 되어있다.
- springfox 공식 github에서도 현재(22년12월21일) 기준 2년 전 마지막으로 업데이트가 되지 않고 있다. https://github.com/springfox/springfox
- 그로인하여 에러들이 존재한다.

### 1. SpringBoot 2.6.x 에서 springfox 3.0 사용 ###
- spring.mvc.pathmatch.matching-strategy 값이 ant_apth_matcher에서 path_pattern_parser로 변경되면서 오류가 발생
 ````java
 // Error Log
org.springframework.context.ApplicationContextException: 
Failed to start bean 'documentationPluginsBootstrapper'; nested exception is java.lang.NullPointerException
  ````
  
**해결방법**
1. application.properties 설정 추가
  ````propeties
  spring.mvc.pathmatch.matching-strategy=ant_path_matcher
  ````

2. 혹은 Spring Boot 버전을 2.5.x로 낮추고 사용하는 방법

### 2. Actuator와 spring fox PathPatternParser 이슈 ###
- Actuator는 PathPatternParser를 사용하기 때문에 properties 설정으로는 해결되지 않는다.
- https://dev-minji.tistory.com/m/144

**해결방법**
````java
// swaggerConfig.java 안에 bean 추가
@Bean
public WebMvcEndpointHandlerMapping webEndpointServletHandlerMapping(WebEndpointsSupplier webEndpointsSupplier
		, ServletEndpointsSupplier servletEndpointsSupplier
		, ControllerEndpointsSupplier controllerEndpointsSupplier
		, EndpointMediaTypes endpointMediaTypes
		, CorsEndpointProperties corsProperties
		, WebEndpointProperties webEndpointProperties
		, Environment environment) {
	        List<ExposableEndpoint<?>> allEndpoints = new ArrayList<>();
	        Collection<ExposableWebEndpoint> webEndpoints = webEndpointsSupplier.getEndpoints();
	        allEndpoints.addAll(webEndpoints);
	        allEndpoints.addAll(servletEndpointsSupplier.getEndpoints());
	        allEndpoints.addAll(controllerEndpointsSupplier.getEndpoints());
	        String basePath = webEndpointProperties.getBasePath();
	        EndpointMapping endpointMapping = new EndpointMapping(basePath);
	        boolean shouldRegisterLinksMapping = this.shouldRegisterLinksMapping(
            		webEndpointProperties, environment, basePath);
	        return new WebMvcEndpointHandlerMapping(
            		endpointMapping, webEndpoints, endpointMediaTypes, 
                    corsProperties.toCorsConfiguration(), 
                    new EndpointLinksResolver(allEndpoints, basePath), 
                    shouldRegisterLinksMapping, null);
}

private boolean shouldRegisterLinksMapping(WebEndpointProperties webEndpointProperties
		, Environment environment
		, String basePath) {
	return webEndpointProperties.getDiscovery().isEnabled() 
			&& (StringUtils.hasText(basePath) 
			|| ManagementPortType.get(environment).equals(ManagementPortType.DIFFERENT));
}

````

## SpringDoc ###
- 공식 사이트: https://springdoc.org/#Introduction
- 현재(22년12월21일) 기준 Maven Repository에서 Dec 16, 2022 에 업데이트가 되었다. 계속 업데이트 중이다.

## OpenAPI? Open API? ##

### 1. Open API ###
- 단어 그대로 개방된 API 이다.
- 예를 들어, 기상청의 단기예보 조회API, 우체국의 우편번호 API 등이 있다. 
- Public API라고도 부른다.

### 2. OpenAPI ###
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


![swagger](https://user-images.githubusercontent.com/24876345/208798428-82d1df31-9590-400d-b710-5300853a288d.png)

OpenAPI Specification을 json또는 yaml로 기술한 문서를 swagger-ui를 통해 띄우게되면 브라우저에서 편리하게 API문서를 볼 수 있다.


## Swagger 도구 ##
- swagger도 여러 도구가 있다.
1. Swagger Editor : 브라우저 기반의 편집기, OpenAPI Spec을 쉽게 작성할 수 있게 도와줌
2. Swagger UI : OpenAPI spec문서를 브라우저에서 확인할 수 있게 해줌, API Test도 가능
3. Swagger Codegen : OpenAPI spec에 맞게 Server나 Client의 stub code생성

### OpenAPI Tool ###
- OpenAPI  Specification을 Implement하기위한 Tool들은 Swagger말고도 종류가 많다.
- https://github.com/OAI/OpenAPI-Specification/blob/main/IMPLEMENTATIONS.md


### OpenAPI 2.0 vs OpenAPI 3.0 ###
- 2015년 Swagger Specification을 OpenAPI Initiative에 기부하면서 OpenAPI Specification(OAS)로 명칭이 바뀌었고, 그 이후 첫번째 major release가 ***OAS3.0***입니다.
