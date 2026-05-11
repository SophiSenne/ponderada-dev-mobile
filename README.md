# Lançador de Dados — Ponderada 1

Aplicação Android para lançamento de dados de RPG (D6, D10, D20 e D100).

## Descrição da Atividade

A atividade consiste em identificar e corrigir um erro lógico em uma aplicação Android já existente, além de expandir suas funcionalidades para suportar múltiplos tipos de dados.

## Análise do Problema

Ao abrir o projeto no Android Studio e executar a aplicação, foi possível observar o seguinte comportamento incorreto no lançamento do D6:

- O dado D6 estava configurado com `Random.nextInt(6)`, o que gera números aleatórios no intervalo **[0, 5]**, ou seja, o valor **6 nunca era gerado** e o 0, que não deveria, é gerado.
- A função `Random.nextInt(from, until)` do Kotlin é **exclusiva no limite superior**: `until` não é incluído no resultado.
- O limite superior foi ajustado de `6` para `7`, garantindo que todos os valores de 1 a 6 possam ser sorteados, e foi adicionado 0 valor 1 no início do *range*:

```kotlin
"D6" -> Random.nextInt(1, 7) // Gera valores de 1 a 6
```

## Adição dos dados D10, D20 e D100

O bloco `when` responsável pelo sorteio foi expandido com os novos tipos de dado:

```kotlin
valorSorteado = when (dadoSelecionado) {
    "D6"   -> Random.nextInt(1, 7)    // 1 a 6
    "D10"  -> Random.nextInt(1, 11)   // 1 a 10
    "D20"  -> Random.nextInt(1, 21)   // 1 a 20
    "D100" -> Random.nextInt(1, 101)  // 1 a 100
    else   -> 0
}
```

### 3. Atualização da interface (RadioButtons)

A lista de dados disponíveis foi atualizada para incluir todas as opções:

```kotlin
val dados = listOf("D6", "D10", "D20", "D100")
```

Cada opção é exibida como um `RadioButton`, permitindo ao usuário escolher o tipo de dado antes de lançar.

## [IR ALÉM] Exibição de Imagens por Face do Dado

Foi implementada a exibição de imagens correspondentes ao resultado sorteado:

- **D6**: cada face (1 a 6) exibe uma imagem diferente do dado (`inverted_dice_1` a `inverted_dice_6`).
- **D10**: exibe a imagem `dice_10`.
- **D20**: exibe a imagem `dice_20`.
- **D100**: exibe o ícone padrão do app como fallback.

A lógica foi centralizada na função `obterImagemDado`:

```kotlin
fun obterImagemDado(tipo: String, valor: Int): Int {
    return when (tipo) {
        "D6" -> when (valor) {
            1 -> R.drawable.inverted_dice_1
            2 -> R.drawable.inverted_dice_2
            3 -> R.drawable.inverted_dice_3
            4 -> R.drawable.inverted_dice_4
            5 -> R.drawable.inverted_dice_5
            else -> R.drawable.inverted_dice_6
        }
        "D10" -> R.drawable.dice_10
        "D20"  -> R.drawable.dice_20
        else   -> R.drawable.ic_launcher_foreground
    }
}
```

As imagens foram adicionadas à pasta `res/drawable/` do projeto.

## 🗂️ Estrutura do Projeto

```
app/src/main/
├── java/carvalho/zanini/ponderada1/
│   └── MainActivity.kt          # Tela principal e lógica do lançador
├── res/
│   ├── drawable/                # Imagens das faces dos dados
│   ├── values/
│   │   ├── strings.xml
│   │   ├── colors.xml
│   │   └── themes.xml
│   └── xml/
│       ├── backup_rules.xml
│       └── data_extraction_rules.xml
```

## Como Executar

1. Clone o repositório:
   ```bash
   git clone https://github.com/Murilo-ZC/ponderada-base-m10-01
   ```
2. Abra o projeto no **Android Studio**.
3. Aguarde a sincronização do Gradle.
4. Execute o app em um emulador ou dispositivo físico com Android 7.0+ (API 24).

## 🛠️ Tecnologias Utilizadas

- **Kotlin**
- **Jetpack Compose**
- **Material3**
- **Android SDK 36**