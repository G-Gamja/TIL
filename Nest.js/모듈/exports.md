서비스를 export하지 않으면 import한 모듈에서 그 원하는 서비스를 사용할 수 없다.
-> 서비스를 export하지 않으면, 해당 서비스는 해당 모듈 내에서만 사용할 수 있습니다.

Nest가 프로바이더를 찾는 순서는 다음과 같습니다.

1. 현재 모듈의 프로바이더
2. import한 모듈의 exports
3. 전역 모듈의 exports

이 순서대로 프로바이더를 찾아서 사용합니다.

3

You don't need to add JwtService to the providers. Nest will look through the current module's providers, then the imported module's exports, and then the global module's exports to find providers that are not immediately in the level it's resolving the providers in. This means that for AuthService it'll look up to the JwtModule's exports and find the JwtService already created, and use that instance. Then in the AppModule you don't need to mention the AuthService or JwtService, just import the AuthModule that exports the AuthService and JwtModule (doing some module re-exporting) and you should be good to go
