# Rest Retrofit API com Kotlin

## Introdução

Retrofit torna sua API HTTP em uma interface Java:

```kotlin
public interface GitHubService {
  @GET("users/{user}/repos")
  Call<List<Repo>> listRepos(@Path("user") String user);
}
```

A classe ```Retrofit``` gera uma implementação para a interface do ```GithubService```:

```kotlin
Retrofit retrofit = new Retrofit.Builder()
    .baseUrl("https://api.github.com/")
    .build();
    
GitHubService service = retrofit.create(GitHubService.class);
```

A ```call``` pode fazer uma chamada HTTP síncrona ou assíncrona para o servidor web remoto:

```kotlin
Call<List<Repo>> repos = service.listRepos("octocat");
```

Use anotações para descrever requisições http:

* Substituição de paramentos URL e suporte a parâmetros de consulta
* Conversão de objeto para requisição de body(JSON, Protocolo Buffer)
* Requisição do body em varias partes e envio de arquivos

## Declaração de API


Anotações nos métodos de interface e seus parâmetros indicam como uma solicitação será tratada.

### Metodos Request

Todo método deve ter uma anotação HTTP que forneça o método de solicitação e a URL relativa. Existem cinco anotações integradas: ```GET```,```POST```,```PUT```,```DELETE``` e ```HEAD```. O URL relativo do recurso é especificado na anotação.

```kotlin
@GET("users/list")
```

Você pode passar parâmetros query da URL:

```kotlin
@GET("users/list?sort=desc")
```

..<CONTINUA>..
