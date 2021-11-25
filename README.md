# Serialization

Сериализация является процессом преобразования данных, привыкших приложением к формату, который может быть передан по сети или сохранен в базе данных или файле. В свою очередь десериализация является противоположным процессом чтения данных с внешнего источника и преобразования его в объект периода выполнения. Вместе они - основная часть большинства приложений, которые обмениваются данными с третьими лицами.

Некоторые форматы сериализации данных, такие как JSON и буферы протокола особенно распространены. Будучи нейтральными в отношении языка и независимым от платформы, они включают обмен данными между системами, записанными на любом современном языке.

В Kotlin инструменты сериализации данных доступны в отдельном компоненте, kotlinx.serialization. Это состоит из двух основных частей: плагин Gradle –org.jetbrains.kotlin.plugin.serialization и библиотеки времени выполнения.

## Библиотеки

kotlinx.serialization обеспечивает наборы библиотек для всех поддерживаемых платформ – JVM, JavaScript, Собственного компонента – и для различных форматов сериализации – JSON, CBOR, буферы протокола и другие. Можно найти полный список поддерживаемых форматов сериализации ниже.

Все библиотеки сериализации Kotlin принадлежат org.jetbrains.kotlinx: группе. Их имена запускаются с kotlinx-serialization

kotlinx.serialization библиотеки используют свою собственную структуру управления версиями, которая не соответствует Kotlin.

## Форматы

kotlinx.serialization включает библиотеки для различных форматов сериализации:

* JSON: kotlinx-serialization-core
* Буферы протокола: kotlinx-serialization-protobuf
* CBOR: kotlinx-serialization-cbor
* Свойства: kotlinx-serialization-properties
* HOCON: kotlinx-serialization-hocon 

Обратите внимание что все библиотеки кроме сериализации JSON (kotlinx-serialization-core) находятся в экспериментальном состоянии, что означает, что их API может быть изменен без уведомления.

## Пример: сериализация JSON

Давайте смотреть на то, как сериализировать объекты Kotlin в JSON.

Перед запуском необходимо будет настроить сценарий сборки так, чтобы можно было использовать инструменты сериализации Kotlin в проекте:

1) Примените сериализацию Kotlin плагин Gradle org.jetbrains.kotlin.plugin.serialization (или kotlin(“plugin.serialization”) в Gradle DSL Kotlin).

   ``` 
   plugins {
     id 'org.jetbrains.kotlin.jvm' version '1.4.10'
     id 'org.jetbrains.kotlin.plugin.serialization' version '1.4.10'  }
   ```
2) Добавьте зависимость библиотеки сериализации JSON: org.jetbrains.kotlinx:kotlinx-serialization-core:1.0.1
   ```
    dependencies {
     implementation 'org.jetbrains.kotlinx:kotlinx-serialization-core:1.0.1'
     } 
   ```
   Теперь вы готовы использовать API сериализации в своем коде.
   
Во-первых, сделайте класс сериализуемым путем аннотирования его @Serializable.
```
@Serializable
data class Data(val a: Int, val str: String = "str")

```

Можно теперь сериализировать экземпляр этого класса путем вызова Json.encodeToString().
```
Json.encodeToString(Data(42))

```

В результате Вы получаете строку, содержащую состояние этого объекта в формате JSON: {"a": 42, "b": "str"}

Можно также сериализировать объектные наборы, такие как списки, в единственном вызове.
```
val dataList = listOf(Data(42), Data(12, "test"))
val jsonList = Json.encodeToString(dataList)

```
Для десериализации объекта от JSON используйте decodeFromString() функция:
```
val obj = Json.decodeFromString<Data>("""{"a":42}""")

```
