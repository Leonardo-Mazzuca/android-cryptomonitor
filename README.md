📱 android-cryptomonitor

🏠 Tela inicial do app

 ![image](https://github.com/user-attachments/assets/8ddeca91-0536-497f-beba-fbd145c99737)

🔄 Tela do app após clicar no botão "Atualizar"

 ![image](https://github.com/user-attachments/assets/341a3f60-549d-4f34-bf13-6466898c8717)

🧐 Sobre o que é o app?

O android-cryptomonitor é um aplicativo Android que permite atualizar a cotação do Bitcoin em tempo real! 🚀
Ao clicar no botão "Atualizar", o app traz a cotação mais recente do Bitcoin diretamente na tela, além de atualizar a data e hora da última consulta. Assim, o usuário sempre sabe quando a cotação foi atualizada. 📈⏰


⚙️ Como o app funciona?

O app utiliza a API do Mercado Bitcoin para buscar a cotação atual da criptomoeda. 🌐

As chamadas HTTP são feitas usando a biblioteca Retrofit 📡.

As respostas JSON são convertidas em objetos Kotlin usando o Gson, uma poderosa biblioteca do Google para trabalhar com dados no formato JSON. 📦

🛠️ Modelo de resposta (TickerResponse)

A classe TickerResponse é responsável por modelar o payload da resposta da API.
Se a API for atualizada futuramente, é só adicionar novos campos nessa classe, facilitando a manutenção. 🔧

```kotlin

class TickerResponse(
        val ticker: Ticker
)

class Ticker(
        val high: String,
        val low: String,
        val vol: String,
        val last: String,
        val buy: String,
        val sell: String,
        val date: Long
)

```

🌎 Serviço de comunicação com a API

O serviço MercadoBitcoinService define a requisição que consulta a cotação atual do Bitcoin:

```kotlin

import retrofit2.Response
import retrofit2.http.GET
import leonardomazzuca.com.github.cryptomonitor.model.TickerResponse

interface MercadoBitcoinService {

    // Fazendo o uso do endpoint em cima da url base disponibilizada no arquivo MercadoBitcoinServiceFactory.kt
    @GET("api/BTC/ticker/")
    suspend fun getTicker(): Response<TickerResponse>
}

```

🔗 Observação: Se você acessar o endereço https://www.mercadobitcoin.net/api/BTC/ticker diretamente no navegador (ex: Firefox 🌐), verá o conteúdo da resposta JSON.

⚡ Atualizando a tela com dados em tempo real
Dentro da MainActivity, o app usa Coroutines (CoroutineScope) para fazer a chamada assíncrona e atualizar a interface assim que o botão "Atualizar" for clicado.

Além disso, o app formata:

💲 O valor da moeda com NumberFormat, usando o padrão monetário brasileiro:

```kotlin

 // formatando número
 val numberFormat = NumberFormat.getCurrencyInstance(Locale("pt", "BR"))
 lblValue.text = numberFormat.format(lastValue)

```

🗓️ A data e hora da atualização:

```kotlin

//formatando data
val date = tickerResponse?.ticker?.date?.let { Date(it * 1000L) }
val sdf = SimpleDateFormat("dd/MM/yyyy HH:mm:ss", Locale.getDefault())

```

Com esses recursos, o Crypto Monitor entrega uma experiência fluída e intuitiva para o usuário! ✨

🚀 Melhorias futuras
🧾 Salvar histórico: armazenar as atualizações em uma tabela para criar um histórico de preços.

💱 Consultar outras moedas: permitir que o usuário busque a cotação de outras criptomoedas além do Bitcoin.

🔄 Conversão entre moedas: exemplo: converter valor de Solana (SOL) para Bitcoin (BTC).


# 🛠️ Tecnologias e APIs Utilizadas

| Categoria                  | Tecnologia / API                                | Descrição                                                                 |
|:----------------------------|:------------------------------------------------|:-------------------------------------------------------------------------|
| 📱 Framework                | **Android SDK (CompileSdk 35)**                 | Framework principal para desenvolvimento Android nativo.               |
| 🌐 HTTP Client              | **Retrofit 2.9.0**                              | Biblioteca para fazer chamadas de API HTTP de forma simples e segura.   |
| 🔄 Conversor JSON           | **Gson Converter (Retrofit)**                   | Conversor para tratar respostas JSON automaticamente.                  |
| ⚡ Assincronismo             | **Kotlin Coroutines**                           | Permite realizar chamadas assíncronas sem travar a interface do app.    |
| 🛡️ Testes                   | **JUnit**, **Espresso**                         | Frameworks para testes unitários e de interface.                        |
| 🧪 Instrumentação           | **AndroidJUnitRunner**                         | Runner para execução de testes de instrumentação no Android.            |
| 🧰 Gerenciamento de Dependências | **Gradle com Version Catalogs**              | Organização moderna de dependências via `libs.versions.toml`.           |
| 🎨 UI Design (Material)     | **Material 3**                                  | Implementação dos componentes visuais seguindo o Material Design 3.    |
| 🧩 Compatibilidade          | **AndroidX AppCompat**                          | Garante compatibilidade de recursos com versões mais antigas do Android.|
| 🌍 Localização/Internacionalização | **NumberFormat / SimpleDateFormat**        | Formatação de valores monetários e datas/hora conforme o idioma local.  |
| 🔗 API Externa              | **Mercado Bitcoin API**                         | API pública para consultar a cotação do Bitcoin em tempo real.          |








