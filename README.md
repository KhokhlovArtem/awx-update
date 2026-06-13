# Ansible Linux Updates Management for AWX

## Описание
Этот репозиторий содержит плейбуки для управления обновлениями безопасности Linux серверов через AWX.

## Структура
```
├── playbooks/          # Основные плейбуки
│   ├── update-critical.yml   # Обновление безопасности
│   └── reboot-servers.yml    # Перезагрузка серверов
├── inventories/        # Инвентари для разных окружений
├── requirements.yml    # Внешние зависимости
└── .gitignore
```

## Использование в AWX

### Настройка проекта
1. Создать Project в AWX
2. Указать URL этого репозитория
3. Включить опции:
   - `Update Revision on Launch`
   - `Update Role Dependencies`

### Шаблоны заданий (Job Templates)

#### 1. Security Updates Only
- Playbook: `playbooks/update-critical.yml`
- Variables:
  ```yaml
  target_hosts: production
  update_level: security
  auto_reboot: false
  ```

#### 2. Проверка (Dry Run)
- Используйте Check Mode в AWX
- Variables:
  ```yaml
  target_hosts: staging
  update_level: security
  ansible_check_mode: true
  ```

#### 3. Перезагрузка
- Playbook: `playbooks/reboot-servers.yml`
- Запускайте ПОСЛЕ успешного обновления

## Переменные для переопределения

| Variable | Default | Description |
|----------|---------|-------------|
| `update_level` | `security` | `security` или `all` |
| `auto_reboot` | `false` | Авто-перезагрузка (отключите в AWX) |
| `update_serial` | `25%` | Процент серверов для одновременного обновления |
| `target_hosts` | `all` | Целевая группа хостов |

## Безопасность
- Всегда сначала тестируйте на staging
- Используйте Check Mode перед prod
- Делайте backup перед обновлением
- Обновляйте серверы порциями

## Troubleshooting
Если роль не скачивается в AWX, проверьте:
1. Файл `requirements.yml` в корне репозитория
2. Опция `Update Role Dependencies` включена
3. У AWX есть доступ к GitHub
