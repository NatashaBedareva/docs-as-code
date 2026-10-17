# Диаграммы

## Диаграмма последовательности
```mermaid
sequenceDiagram
    actor User as Покупатель
    participant UI as Веб-приложение
    participant API as Сервис анализа (API)
    participant DB as База данных
    participant Model as ML-модель

    User->>UI: Вводит текст отзыва и нажимает «Анализировать»
    UI->>API: текст отзыва
    API->>API: Проверка, что текст не пустой

    alt Текст пустой
        API-->>UI: Ошибка 400: требуется текст отзыва
        UI-->>User: Показать сообщение «Введите текст отзыва»
    else Текст корректен
        API->>Model: Передать текст на анализ
        Model-->>API: Вернуть тональность (positive / neutral / negative)
        API->>DB: Сохранить отзыв и результат анализа
        DB-->>API: Подтвердить сохранение
        API-->>UI: Результат анализа (тональность)
        UI-->>User: Показать результат анализа
    end

    alt Модель не может обработать текст
        Model-->>API: Ошибка обработки
        API-->>UI: Сообщение «Анализ не удался, повторите позже»
        UI-->>User: Показать сообщение об ошибке
    end
```

## Диаграмма классов

```mermaid
classDiagram

    class User {
        +int id
        +string name
        +string role
    }

    class Review {
        +int id
        +string text
        +datetime createdAt
    }

    class AnalysisResult {
        +string sentiment
        +float confidence
    }

    class Product {
        +int id
        +string title
    }

    User "1" --> "*" Review : оставляет
    Product "1" --> "*" Review : относится к
    Review "1" --> "1" AnalysisResult : имеет
```