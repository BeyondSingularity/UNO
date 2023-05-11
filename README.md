# UNO

Это текстовая игра UNO, написанная на языке Python. Игра поддерживает до четырех игроков и ведется с помощью терминала. Игрок может либо присоединиться к игре, которую ведет другой игрок, либо вести свою собственную игру.

## Preview

![Screenshot_11](https://user-images.githubusercontent.com/57673975/230796329-be8bc648-a748-4783-8f21-f9952d20b828.png)

## Установка
Для запуска игры вам понадобится Python 3, установленный на вашей машине. После установки скачайте или клонируйте репозиторий на вашу машину и перейдите в корневой каталог. В проекте используются только встроенные в Python 3 библиотеки, поэтому больше ничего вам устанавливать не понадобится.

```
git clone git@github.com:BeyondSingularity/UNO.git
```

После клонирования репозитория игроки могут настроить параметры игры, изменив файл 'config.ini' в корневой папке. 

Файл 'config.ini' позволяет игрокам установить свой ник, IP-адрес сервера, порт и правила игры перед запуском игры.

## Игра
Чтобы начать игру, хост должен запустить скрипт 'run_game.py' в корневой папке.
```
python3 run_game.py
```
После запуска хост будет получать сообщения с ником, IP и портом подключающихся игроков. Когда все ожидаемые игроки присоединятся к игре, ведущий может начать игру, набрав 'start'.

Ход каждого игрока содержит информацию об игре, такую как верхняя карта в стопке сброса, чей сейчас ход и направление игры. Игроку предоставляется информация о его собственных картах, и он может выбрать сыграть карту, взять карту из стопки сброса или сказать "UNO".

В некоторых ситуациях игроку может потребоваться выбрать вариант, например, выбрать цвет дикой карты или поменять руки с другим игроком.

## Структура проекта

Игра ведётся по стандартным правилам UNO.

Есть игровое поле, на котором находятся:
 
1) Колода, из которой игроки тянут карты. 
2) Игровая колода, на которую игроки кладут свои карты (тем самым делая ход).
3) Игроки со своими картами (каждый игрок видит только свои карты, карты из первой колоды берутся вслепую). 

Таким образом, на игровом поле имеются объекты трёх типов:

- Игрок (4 игрока, у каждого своя колода)
- Колода 1
- Колода 2

## Карты, используемые в игре

В игре всего 4 цвета карт: красный, желтый, зелёный и синий

Различные типы карт:
- Цветные карты от "0" до "9"
- Цветные карты "+2 карты следующему игроку"
- Цветные карты "пропуск хода следующему игроку"
- Цветные карты "ревёрс порядка хода"
- Дикая (черная) карта "поменять цвет колоды"
- Дикая (черная) карта "+4 карты следующему игроку"

## Детали игры

Задача игрока - полностью избавиться от своих карт, тем самым победив в игре.

Игра происходит по стандартным правилам UNO (на первой итерации взаимодействие игроков с игрой происходит через консоль):

1) Чтобы походить определённой картой, нужно написать её название, к примеру зелёная семёрка пишется как "green_7".
2) Игрок может вытягивать карты из колоды№1 с помощью команды "draw", однако если в руке игрока уже есть карта, которую можно сыграть, игрок обязан выкинуть её на стол (то есть он не сможет взять ещё одну карту из колоды).
3) После выбрасывания чёрной карты, игрок обязан написать цвет, в который он хочет "покрасить" колоду. Если была сыграна карта "+4", следующий игрок должен написать accept/challenge, чтобы принять 4 карты/проверить, что предыдущий игрок сыграл по правилам (google: uno challenge rule). В случае проваленного challenge, игрок получает ещё 2 карты к и так причитающимся 4-ём, иначе: игрок, сыгравший "+4" не по правилам, берёт 4 карты вместо "жертвы своих манипуляций".
4) Если после хода, у вас осталась одна карта (не распространяется на обмен карт после "семёрки"), то вы должны сказать "uno" в консоль. В противном случае, если кто-то во время хода ***следующего*** игрока заметит, что у вас одна карта, а "uno" сказано не было, и напишет в чат "uno", то вы получите две штрафные карты.
5) Карты "+2" и "+4" дают следующему игроку соответствующее количесвто карт и заставляют его пропустить ход.
6) Карты типа "reverse" изменяют порядок хода на противоположный.
7) Карта "skip" заставляет следующего игрока пропустить свой ход.

Также есть дополнительные правила, которые Хост может включить/отключить в конфиге config.ini:
1. 7-0: карта 7 позволяет кому-то выбрать, с кем поменяться руками. Карта 0 заставляет ВСЕХ поменяться руками друг с другом, в каком бы направлении ни шла игра (по часовой стрелке или против часовой стрелки!).
2. Jump-In: Позволяет игроку сыграть свою карту в свой ход, если эта карта ИДЕНТИЧНА той, что лежит в стопке сброса.
3. Принудительная игра: Игрок не может тянуть карту, если у него уже есть хотя бы одна, которую он может сыграть.
4. Блеф запрещен: Запрещает бросать вызов "дикому" розыгрышу 4 карт.
5. Тяни пока не найдёшь (Draw To Match): Заставляет игрока разыгрывать карты до тех пор, пока он не вытянет карту, совпадающую по цвету, числу или символу.
6. Stacking: Если правило включено, карта +2 не сразу пропускает ход следующему игроку, а дает ему возможность сыграть еще одну карту +2 - в этом случае следующий уже наберёт 4 карты из колоды, следующий 6 и т.д. Если игрок не может сыграть карту +2, то он берет 2 карты из колоды. **ВАЖНО**: 'Stacking' работает только для карт +2, но не для карт +4.

**БЛЕФ ЗАПРЕЩЕН и STACKING не могут быть включены одновременно.**

https://www.unorules.com/
