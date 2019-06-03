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
### Manipulação de URL

A requisição URL pode ser atualizada dinamicamente usando blocos de reposição e métodos de parâmetros. O bloco de reposição é uma string alfanumérica definida por { e }. O parâmetro correspondente deve ser anotado com ```@Path``` usando a mesma string.

```kotlin
@GET("group/{id}/users")
Call<List<User>> groupList(@Path("id") int groupId);
```

Parâmetros Query podem ser adicionados:

```kotlin
@GET("group/{id}/users")
Call<List<User>> groupList(@Path("id") int groupId, @Query("sort") String sort);
```

Para combinações complexas de parâmetros Query, se usa ```Map```:

```kotlin
@GET("group/{id}/users")
Call<List<User>> groupList(@Path("id") int groupId, @QueryMap Map<String, String> options);
```

### Requisição de corpo

Um objeto especifico pode ser usado para uma requisição do corpo HTTP com a anotação @Body:

```kotlin
@POST("users/new")
Call<User> createUser(@Body User user);
```

### Formulário codificado e Multipart

Os métodos também podem ser declarados para enviar dados codificados e multipartes.

Dados codificados por formulário são enviados quando ```@FormUrlEncoded``` está presente no método. Cada par de valores-chave é anotado com ```@Field``` contendo o nome e o objeto fornecendo o valor.

```kotlin
@FormUrlEncoded
@POST("user/edit")
Call<User> updateUser(@Field("first_name") String first, @Field("last_name") String last);
```

Solicitações multipartes são usadas quando ```@Multipart``` está presente no método. Partes são declaradas usando a anotação ```@Part```:

```kotlin
@Multipart
@PUT("user/photo")
Call<User> updateUser(@Part("photo") RequestBody photo, @Part("description") RequestBody description);
```

Peças de várias partes usam um dos conversores do Retrofit ou podem implementar o ```RequestBody``` para lidar com sua própria serialização.

### Manipulação de Header

Você pode definir cabeçalhos estáticos para um método usando a anotação ```@Headers```:

```kotlin
@Headers("Cache-Control: max-age=640000")
@GET("widget/list")
Call<List<Widget>> widgetList();
```

```kotlin
@Headers({
    "Accept: application/vnd.github.v3.full+json",
    "User-Agent: Retrofit-Sample-App"
})
@GET("users/{username}")
Call<User> getUser(@Path("username") String username);
```

..<CONTINUA>..
