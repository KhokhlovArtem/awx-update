# Troubleshooting

## Проблема: роль не скачивается в AWX

Проверьте:
1. Файл `requirements.yml` находится в корне репозитория
2. Опция `Update Role Dependencies` включена в настройках проекта
3. У AWX есть доступ к GitHub (проверьте network)

## Проблема: ошибка "role not found"

Убедитесь, что:
- requirements.yml корректный YAML
- Версия роли существует (main/master или конкретный тег)
- Репозиторий публичный или добавлен SSH ключ

## Проблема: сервер не перезагружается

- Проверьте файл-триггер `/var/run/reboot-required.ansible`
- Убедитесь, что `auto_reboot` включен
- Проверьте timeouts в reboot-servers.yml
