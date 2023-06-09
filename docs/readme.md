# Vanessa Usher - библиотека быстрого построения сборочных циклов для 1С:Предприятие 8 в Jenkins

Цель продукта - сократить время построения сборочных линий для проектов 1C.

Конвейер — это группа событий или заданий, которые последовательно связаны друг с другом. Проще говоря, 
конвейер представляет собой комбинацию скриптов/плагинов, которые поддерживают интеграцию и реализацию конвейеров 
непрерывной доставки с использованием Jenkins.

## Типовые конвейеры

В состав библиотеки входят такие `типовые` конвейеры как:

* Выгрузка истории хранилища 1С в git-репозиторий (`repoSyncPipeline`)
* Проверочный контур проекта 1С (`buildPipeline`)
* Синхронизация релиза 1С с сайта `releases.1c.ru` и git-репозитория (`syncVendorConfPipeline`)

## Библиотечные шаги

* Выгрузить историю хранилища 1С (или нескольких хранилищ 1С) в git-репозиторий (`gitsync`)
* Трансформировать исходный код из формата EDT в XML (`edtTransform`)
* Подготовить информационную базу 1С (`prepareInfobase`)
* Выполнить внешнуюю обработку 1с (или несколько) с передачей параметров запуска (`runExternal`)
* Провести проверку применимости расширения (или нескольких) (`checkExtensions`)
* Провести синтаксическую проверку конфигурации и расширений 1С (`syntaxCheck`)
* Провести статический анализ проекта 1С в SonarQube (`sonarAnalyze`)
* Провести дымовое тестирование, используя ADD (`smokeTesting`)
* Провести модульное тестирование, используя xUnit (`unitTesting`)
* Провести поведенческое тестирование (`bddTesting`)
* Собрать и опубликовать в артефактах файл поставки 1С (`distributionBuild`)
* Опубликовать в Jenkins накопленные allure и xunit отчеты (`publishReports`)
* Синхронизировать релиз 1С с сайта `releases.1c.ru` в git-репозиторий (`yard`)

## Примеры использования

* Выгрузка истории хранилища 1С в git-репозиторий
* Запуск проверочного контура для проекта 1С

### Выгрузка истории хранилища 1с в git-репозиторий

Выполняется пакетная синхронизация, по одному или нескольким проектам (хранилищам 1с).

#### Jenkins-файл для запуска pipeline gitsync

```json
@Library('usher2') _

repoSyncPipeline('tools/pipeline/gitsync.json', 'main')
```

* `@Library` - указываем подключенную в Jenkins библиотеку
* [`repoSyncPipeline`](../docs/pipeline/repoSyncPipeline.md)
  * `'tools/pipeline/gitsync.json'` - путь к конфигурационному файлу gitsync для пакетной синхронизации.
  * `'main'` - Jenkins-агент (имя или метка ноды) на котором будет выполняться чтение конфигурационного файла. (опционально)

### Запуск проверочного контура для проекта 1с

```json
@Library('usher2') _

buildPipeline('tools/pipeline/ci.json', 'main')
```
* `@Library` - указываем подключенную в Jenkins библиотеку
* [`buildPipeline`](../docs/pipeline/buildPipeline.md)
  * `'tools/pipeline/ci.json'` - путь к конфигурационному файлу для запуска проверочного контура для проекта 1с.
  * `'main'` - Jenkins-агент (имя или метка ноды) на котором будет выполняться чтение конфигурационного файла. (опционально)

