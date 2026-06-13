# AWX Linux Updates

Самодостаточные плейбуки для обновления безопасности Linux серверов через AWX — без внешних зависимостей.

## Структура

```
awx-linux-updates/
├── playbooks/
│   ├── update-critical.yml   # Критические обновления (RHEL/Debian/SUSE)
│   └── reboot-servers.yml    # Безопасная перезагрузка
└── inventories/
    ├── production/hosts.ini
    └── staging/hosts.ini
```

## Использование в AWX

### Настройка проекта
1. Создать **Project** в AWX, указать URL репозитория
2. **ОТКЛЮЧИТЬ** опцию `Update Role Dependencies` (нет внешних ролей)
3. **ВКЛЮЧИТЬ** `Update Revision on Launch`

### Job Templates

| Шаблон | Playbook | Описание |
|--------|----------|---------|
| Linux Security Updates | `playbooks/update-critical.yml` | Critical Security обновления |
| Reboot After Updates | `playbooks/reboot-servers.yml` | Перезагрузка (после обновления) |

### Переменные

| Variable | Default | Описание |
|----------|---------|----------|
| `target_hosts` | `all` | Группа хостов |
| `update_serial` | `20%` | Процент серверов для параллельного обновления |
| `log_dir` | `/var/log/ansible-updates` | Директория для логов |

### Workflow (рекомендуется)
1. **Check Mode** — `Linux Security Updates` с Extra Variables: `ansible_check_mode: true`
2. **Apply** — `Linux Security Updates` с Extra Variables: `target_hosts: production`
3. **Reboot** — `Reboot After Updates` (только если были изменения)

## Поддержка ОС
- RHEL / CentOS / AlmaLinux / Rocky 7+
- Debian / Ubuntu 18.04+
- SUSE / openSUSE

## Преимущества
- Нет внешних зависимостей (не нужны GitHub, Galaxy, роли)
- Работает из коробки после клонирования
- Полный контроль над каждой командой
- Логирование всех действий
- Только critical/security обновления
