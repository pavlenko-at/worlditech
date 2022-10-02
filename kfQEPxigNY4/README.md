# Конфиг файлы для видео [CRON.D больше НЕ НУЖЕН. Как пользоваться SYSTEMD TIMER?](https://youtu.be/kfQEPxigNY4)

См. также посты и обсуждения в [Telegram-канале](https://t.me/worlditech):

- [Я хочу в systemd unit не писать путь до bash скрипта, а просто написать его имя myscript.sh . Что нужно сделать чтобы гарантировано отработало?](https://t.me/worlditech/1279).
- [Конфиг Unit & Timer](https://t.me/worlditech/1283).

## Полезные команды из видео

### Unit

```bash
# Разовый запуск.
sudo systemctl start SwordInquisition.service

# Проверка статуса службы и последние логи.
sudo systemctl status SwordInquisition.service

# Просмотреть логи службы за сегодня.
journalctl -S today -u SwordInquisition
```

### Timers

```bash
# Проверка значений OnCalendar timer values.
systemd-analyze calendar --iterations=5 "09:5/5" 

# Активация и запуск таймера.
sudo systemctl enable SwordInquisition.timer
systemctl daemon-reload
sudo systemctl start SwordInquisition.timer
```

## Примеры для OnCalendar

| Spec                | Описание                               |
| ------------------- |:--------------------------------------:|
| `*:*:00`            | каждую минуту                          |
| `*:15:00`           | в 15 минут каждого часа                |
| `*-*-1,5,7 *:00:00` | каждый час 1, 5 и 7 числа              |
| `Mon *-*~1`         | если последний день месяца понедельник |
| `Mon *-*~7/1`       | последний понедельник месяца           |

### Алиасы

| Spec           | Описание      |
| -------------- |:-------------:|
| `minutely`     | ежеминутно    |
| `hourly`       | каждый час    |
| `daily`        | каждый день   |
| `monthly`      | каждый месяц  |
| `weekly`       | еженедельно   |
| `yearly`       | ежегодно      |
| `quarterly`    | ежеквартально |
| `semiannually` | раз в полгода |

### Проверка

Для проверки можно использовать

```bash
systemd-analyze calendar --iterations=5 "SPEC"
```

например:

```bash
$ systemd-analyze calendar --iterations=5 "Mon *-*~7/1"
  Original form: Mon *-*~7/1
Normalized form: Mon *-*~07/1 00:00:00
    Next elapse: Mon 2022-10-31 00:00:00 MSK
       (in UTC): Sun 2022-10-30 21:00:00 UTC
       From now: 4 weeks 1 day left
       Iter. #2: Mon 2022-11-28 00:00:00 MSK
       (in UTC): Sun 2022-11-27 21:00:00 UTC
       From now: 1 month 26 days left
       Iter. #3: Mon 2022-12-26 00:00:00 MSK
       (in UTC): Sun 2022-12-25 21:00:00 UTC
       From now: 2 months 24 days left
       Iter. #4: Mon 2023-01-30 00:00:00 MSK
       (in UTC): Sun 2023-01-29 21:00:00 UTC
       From now: 3 months 29 days left
       Iter. #5: Mon 2023-02-27 00:00:00 MSK
       (in UTC): Sun 2023-02-26 21:00:00 UTC
       From now: 4 months 26 days left

```

### Попробуй сам

- `*-*-* 00:15:30`
- `Mon *-*-* 00:00:00`
- `Wed 2020-*-*`
- `Mon..Fri 2021-*-*`
- `2022-6,7,8-1,15 01:15:00`
- `Mon *-05~03`
- `Mon..Fri *-08~04`
- `*-05~03/2`
- `*-05-03/2`
