# Пошаговая настройка AWX проекта

## Предварительные требования
- AWX версии 17.0.0+
- Git репозиторий с правами на чтение
- SSH доступ к целевым серверам

## Шаг 1: Клонирование репозитория в AWX

```bash
# В AWX создайте новый Project
# Source Control Type: Git
# Source Control URL: https://github.com/your-org/ansible-linux-updates.git
# Опции:
   ✓ Update Revision on Launch
   ✓ Update Role Dependencies
```

## Шаг 2: Настройка Credentials

Создайте Machine Credential:
- Name: "Linux Sudo User"
- Username: your_admin_user
- Password: your_password
- Privilege Escalation Method: sudo

## Шаг 3: Настройка Inventory

Создайте инвентарь и добавьте хосты:
```ini
web01 ansible_host=192.168.1.10
web02 ansible_host=192.168.1.11
```

## Шаг 4: Создание Job Template

- Name: "Security Updates Production"
- Project: Ваш проект
- Playbook: `playbooks/update-critical.yml`
- Inventory: Ваш инвентарь
- Credentials: Linux Sudo User
- Options:
  ✓ Privilege Escalation

## Шаг 5: Первый запуск

1. Включите Check Mode
2. Запустите на staging
3. Проверьте вывод
4. Запустите без Check Mode на staging
5. Перенесите на production
