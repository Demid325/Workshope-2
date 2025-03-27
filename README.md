# Анализ данных в разработке игр [in GameDev]
Отчет по лабораторной работе #2 выполнил(а):
- Белоусов Демид Алексеевич
- НМТ231702

| Задание | Выполнение | Баллы |
| ------ | ------ | ------ |
| Задание 1 | * | 60 |
| Задание 2 | * | 20 |
| Задание 3 | # | 20 |

знак "*" - задание выполнено; знак "#" - задание не выполнено;

Работу проверили:
- к.т.н., доцент Денисов Д.В.
- к.э.н., доцент Панов М.А.
- ст. преп., Фадеев В.О.

[![N|Solid](https://cldup.com/dTxpPi9lDf.thumb.png)](https://nodesource.com/products/nsolid)

[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)

## Цель работы
Научиться передавать в Unity данные из Google Sheets с помощью Python.

## Задание 1
### Выберите одну из игровых переменных в игре СПАСТИ РТФ: Выживание (HP, SP, игровая валюта, здоровье и т.д.), 
опишите её роль в игре, условия изменения / появления и диапазон допустимых значений. Постройте схему экономической модели в игре и укажите место выбранного ресурса в ней.

Для работы я выбрал переменную HP игрока (Health Points) — числовую характеристику, определяющую активность. Она напрямую влияет на геймплей: если HP достигает нуля, игра заканчивается. Кроме того, эта переменная меняет поведение игрока, вынуждая его использовать при игре тактику.
По умолчанию HP равен 30, но при получении урона значение снижается. Стартовый диапазон значений — от 0 до 30. Восстановить или увеличить максимальный уровень можно через прокачку навыков. 
Схема экономической модели в игре 
![Схема](https://github.com/user-attachments/assets/dfff5785-afe9-41db-8c2a-754e92b884b9)

В данной схеме потеря здоровья будет являться трубой. Получая урон, мы можем через преобразователь напрямую обменять деньги или косвенно очки опыта на HP или же получить из крана, находясь в особой зоне или завершив волну.

## Задание 2
### С помощью скрипта на языке Python заполните google-таблицу данными, описывающими выбранную игровую переменную в игре “СПАСТИ РТФ:Выживание”. Средствами google-sheets визуализируйте данные в google-таблице (постройте график / диаграмму и пр.) для наглядного представления выбранной игровой величины. Опишите характер изменения этой величины, опишите недостатки в реализации этой величины (например, в игре может произойти условие наступления эксплойта) и предложите до 3-х вариантов модификации условий работы с переменной, чтобы сделать игровой опыт лучше.
```py

 import gspread
import random
import numpy as np
gc = gspread.service_account(filename='workshop2-454914-ad782f9aaa44.json')
sh = gc.open("Workshop2")
worksheet = sh.sheet1
worksheet.clear()
headers = ['Волна', 'Текущее HP', 'Макс. HP', 'Полученный урон', 'Восстановление']
worksheet.update('A1:E1', [headers])
max_hp = 30
current_hp = max_hp
damage_per_wave = random.choice([10, 20, 30])
for wave in range(1, 11):
    current_hp = max(0, current_hp - damage_per_wave)
    if wave % 3 == 0:  # Каждые 3 волны увеличиваем максимальное HP
        max_hp_increase = random.randint(5, 15)
        max_hp += max_hp_increase
        current_hp = max_hp
        heal = max_hp_increase
    else:
        heal = random.randint(5, 15)
        current_hp = min(current_hp + heal, max_hp) 
    row = [wave, current_hp, max_hp, damage_per_wave, heal]
    worksheet.update(f'A{wave+1}:E{wave+1}', [row])
```

![Текущее HP, Макс  HP, Полученный урон и Восстановление(1)](https://github.com/user-attachments/assets/55530485-7db1-4a9c-a2f8-d5b5261448a0)

Выбранная переменная HP демонстрирует устойчивый рост по мере прогресса в игре, сопровождаемый незначительными колебаниями в результате получения урона.

Чрезмерное увеличение максимального HP снижает напряженность игрового процесса, минимизирует последствия ошибок игрока.
Для решения этой проблемы можно: 
- введение увеличения урона противников за каждые 10 единиц максимального HP игрока
- создание системы негативных эффектов получаемых игроком 
- постепенное уменьшение эффективности улучшений максимума HP.

## Задание 3
### Настройте на сцене Unity воспроизведение звуковых файлов, описывающих динамику изменения выбранной переменной. Например, если выбрано здоровье главного персонажа вы можете выводить сообщения, связанные с его состоянием.

-

## Выводы
В ходе работы мы освоили перенос и заполнение таблиц с помощью языка программирования Python, а также разобрали принципы работы игровой экономики на примере переменной HP (Health Points), изучив ее роль и взаимодействие с игровыми системами.

## Powered by

**BigDigital Team: Denisov | Fadeev | Panov**
