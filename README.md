# Go Concurrency Example

[![Go Version](https://img.shields.io/badge/Go-1.24.3-blue)](https://golang.org)
[![CI Status](https://github.com/VladislavKV-MSK/action/actions/workflows/go.yml/badge.svg)](https://github.com/VladislavKV-MSK/action/actions)

Пример работы с горутинами, демонстрирующий типичную проблему переменных в циклах (специально оставлено для учебных целей).

## 🔍 Особенности кода

### Важная учебная заметка
В коде специально сохранена классическая проблема работы с горутинами в циклах:
```go
for i := 0; i < 5; i++ {
    go func() {
        fmt.Println(i) // Все горутины используют одно значение i!
        wg.Done()
    }()
}
```

Почему это проблема?

Горутины захватывают переменную i по ссылке, что может приводить к:

- Выводу одинаковых значений
- Непредсказуемому поведению в версиях Go до 1.22

Как исправить для старых версий ниже 1.22:

```go
for i := 0; i < 5; i++ {
    go func(val int) {
        fmt.Println(val)
        wg.Done()
    }(i) // Явная передача значения
}
```

Начиная с Go 1.22+ поведение изменено - каждая итерация создает новую переменную, что делает код более безопасным.

## 🛠 Функционал

Демонстрация работы с:
- Горутинами
- WaitGroup
- Race condition (в тестах)

Функция MaxInt(a, b int) с тестами

## 🚀 Запуск

```bash
go run main.go   # Запуск демо
go test -v       # Запуск тестов
go test -race -v # Проверка на гонки
```

## 🔄 CI/CD

Автоматически проверяет:
- Сборку (go build)
- Тесты (go test)
- Race condition (go test -race)
- Качество кода (golangci-lint)

Конфигурации:
  - go.yml - основной workflow
  - go2.yaml - для ветки new_workflow
