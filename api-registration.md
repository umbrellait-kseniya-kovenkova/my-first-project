# API регистрации

[Главная](README.md) | [Требования](requirements.md) | [Нефункциональные требования](non-functional-requirements.md)

OpenAPI-файл для Swagger: [openapi.yaml](openapi.yaml)
Docker Compose для Swagger: [docker-compose.swagger.yml](docker-compose.swagger.yml)

## Создание аккаунта

- Метод: `POST`
- URL: `/api/v1/auth/register`
- Content-Type: `application/json`

Тело запроса:

```json
{
  "firstName": "Иван",
  "lastName": "Иванов",
  "email": "ivan@example.com",
  "password": "StrongPass123!",
  "confirmPassword": "StrongPass123!",
  "acceptTerms": true
}
```

Правила валидации:

- `firstName`: обязательное, от 2 до 50 символов.
- `lastName`: обязательное, от 2 до 50 символов.
- `email`: обязательное, корректный email-формат, уникальное в системе.
- `password`: обязательное, минимум 8 символов, минимум 1 заглавная буква, 1 строчная буква, 1 цифра и 1 спецсимвол.
- `confirmPassword`: должно совпадать с `password`.
- `acceptTerms`: должно быть `true`.

Успешный ответ:

- Код: `201 Created`

```json
{
  "userId": "3f5c4d7c-12ab-4cc4-9c2a-8ed54bdbf401",
  "email": "ivan@example.com",
  "status": "pending_verification",
  "message": "Письмо с подтверждением отправлено"
}
```

Ответы с ошибками:

- `400 Bad Request` - ошибки валидации.
- `409 Conflict` - пользователь с таким email уже существует.
- `429 Too Many Requests` - слишком много попыток регистрации.
- `500 Internal Server Error` - внутренняя ошибка сервера.

Пример ответа `400 Bad Request`:

```json
{
  "error": "validation_error",
  "message": "Некорректные данные",
  "details": [
    {
      "field": "password",
      "issue": "Пароль должен содержать минимум 8 символов"
    }
  ]
}
```

## Подтверждение email

- Метод: `GET`
- URL: `/api/v1/auth/verify-email?token={token}`

Успешный ответ:

- Код: `200 OK`

```json
{
  "status": "verified",
  "message": "Email успешно подтвержден"
}
```

Ошибки:

- `400 Bad Request` - токен отсутствует или неверного формата.
- `410 Gone` - токен истек.
- `404 Not Found` - токен не найден.
- `500 Internal Server Error` - внутренняя ошибка сервера.

## Пример Bearer авторизации

В Swagger добавлен пример защищенного endpoint:

- Метод: `GET`
- URL: `/api/v1/users/me`
- Ошибки: `401 Unauthorized` и `500 Internal Server Error`

Как протестировать Bearer token в Swagger UI:

1. Откройте Swagger UI.
2. Нажмите кнопку `Authorize`.
3. Вставьте токен в формате: `Bearer <ваш_токен>`.
4. Подтвердите и вызовите `GET /api/v1/users/me`.

## Как открыть в Swagger UI

1. Убедитесь, что установлен Docker.
2. Откройте терминал в корне проекта.
3. Выполните команду:

```bash
docker run -p 8080:8080 -e SWAGGER_JSON=/docs/openapi.yaml -v "$PWD/openapi.yaml:/docs/openapi.yaml" swaggerapi/swagger-ui
```

4. Откройте в браузере: `http://localhost:8080`

## Запуск Swagger одной командой через Docker Compose

1. Откройте терминал в корне проекта.
2. Выполните:

```bash
docker compose -f docker-compose.swagger.yml up -d
```

3. Откройте: `http://localhost:8080`
