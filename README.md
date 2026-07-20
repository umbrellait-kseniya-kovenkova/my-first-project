markdown
# Мой первый проект на GitHub!
Это мой личный проект для изучения Git и VS Code.
Здесь надо будет описать проект.

## Страницы требований

- [Требования](requirements.md)
- [Нефункциональные требования](non-functional-requirements.md)
- [API регистрации](api-registration.md)

## Как запустить Swagger

- OpenAPI-спецификация: [openapi.yaml](openapi.yaml)
- Docker Compose: [docker-compose.swagger.yml](docker-compose.swagger.yml)

Запуск одной командой:

```bash
docker compose -f docker-compose.swagger.yml up -d
```

После запуска откройте в браузере: http://localhost:8080

Подробные шаги и описание авторизации доступны на странице:
[API регистрации](api-registration.md)