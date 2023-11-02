# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]
Отчет по лабораторной работе #3 выполнил(а):
- Тимохов Кирилл Александрович
- РИ220910
Отметка о выполнении заданий (заполняется студентом):

| Задание | Выполнение | Баллы |
| ------ | ------ | ------ |
| Задание 1 | * | 60 |
| Задание 2 | * | 20 |
| Задание 3 | * | 20 |

знак "*" - задание выполнено; знак "#" - задание не выполнено;

Работу проверили:
- к.т.н., доцент Денисов Д.В.
- к.э.н., доцент Панов М.А.
- ст. преп., Фадеев В.О.

[![N|Solid](https://cldup.com/dTxpPi9lDf.thumb.png)](https://nodesource.com/products/nsolid)

[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)

Структура отчета

- Данные о работе: название работы, фио, группа, выполненные задания.
- Цель работы.
- Задание 1.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Задание 2.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Задание 3.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Выводы.
- ✨Magic ✨

## Цель работы
Ознакомиться с основными операторами зыка Python на примере реализации линейной регрессии.

## Задание 1
### Предложите вариант изменения найденных переменных для 10 уровней в игре. Визуализируйте изменение уровня сложности в таблице.
Посидев над прототипом игры, я сделал следующие выводы: на движение дракона влияют скорость, диапазон значений в котором они движется и вероятность, что он поменяет направления, а на сбрасывание яиц влияет время, которое проходит между сбрасыванием яиц. Следовательно, меняя эти перемнные, мы можем изменить уровень сложности игры.
Например:
Увеличить скорость дракона
Увеличить диапазон значений в пределах, которых передвигается дракон
Увеличить вероятнсть изменения направления дракона
Уменьшить время между сбрасыванием яиц

Путём проб и ошибок, я подобрал значения переменных для самого сложного уровня и уже отталкиваясь от него нашел значения для предшествующих ему уровней
Ссылка на google таблицу: https://docs.google.com/spreadsheets/d/1CNWxN388RsqgzYLJ6IshVzevfc4bHsYxXpPQe3js6-4/edit#gid=0
![image](https://github.com/ManualCode/DragonPicker-in-GameDev-lab3/assets/120582775/f547ff81-7626-45c6-9a7e-3c2e764e2eac)





## Задание 2
### Создайте 10 сцен на Unity с изменяющимся уровнем сложности.
Ход работы:
- Скачаем проект DrakonPicker и добавим в Unity
- Я скопировал сцену и поменял переменные.

## Задание 3
### Заполнить google-таблицу данными из Python. В Python данные также должны быть визуализированы.
Ход работы:
- Откроем Anaconda Jupyter Notebook
- Создадим новую таблицу и напишем код для подключения к Google таблице, заполним её и визуализируем полученные данные

```py
import gspread
import numpy as np
import time
import matplotlib.pyplot as plt

gc = gspread.service_account(filename='unitydatascience2-403913-2b0ff59b1198.json')
sh = gc.open("WorkShop3")
time.sleep(1)
speed = 4
time_between_egg_drops = 2
left_right_distance = 10
chance_direction = 0.01
l, l_speed, l_time, l_distance, l_chance = [],[],[],[],[]
n = [i for i in range(1,12)]
for i in range(11):
    tempRand = np.random.randint(-100, 100, 4)   
    sh.sheet1.update(('A' + str(i+2)), i)
    sh.sheet1.update(('B' + str(i+2)), speed)
    sh.sheet1.update(('C' + str(i+2)), left_right_distance*2)
    sh.sheet1.update(('D' + str(i+2)), chance_direction)
    sh.sheet1.update(('E' + str(i+2)), time_between_egg_drops)
    print([speed,left_right_distance,chance_direction,time_between_egg_drops])
    l.append([speed,left_right_distance,chance_direction,time_between_egg_drops])
    l_speed.append(speed)
    l_distance.append(left_right_distance*2)
    l_chance.append(chance_direction)
    l_time.append(time_between_egg_drops)
    speed += 2 + (tempRand[0]/1000)
    left_right_distance += 1.5 + (tempRand[1]/1000)
    chance_direction += 0.003 + abs(tempRand[2]/50000)
    time_between_egg_drops -= 0.15 + (tempRand[3]/2000)
    
plt.title('Скорость дракона', fontsize=20)
plt.plot(n,l_speed)
plt.xlabel("Уровень")
plt.xticks(n)
plt.show()
plt.title('Диапазон движения дракона', fontsize=20)
plt.plot(n,l_distance)
plt.xlabel("Уровень")
plt.xticks(n)
plt.show()
plt.title('Вероятность поменять направление', fontsize=20)
plt.plot(n,l_chance)
plt.xlabel("Уровень")
plt.xticks(n)
plt.show()
plt.title('Время между сбрасыванием яиц', fontsize=20)
plt.plot(n,l_time)
plt.xlabel("Уровень")
plt.xticks(n)
plt.show()
bw = 0.2
index = np.arange(1,12)
plt.title('Динамика изменений уровней сложности', fontsize=20)
plt.bar(index, l_time, bw, color='b')
plt.bar(index+bw, l_speed, bw,color='g')
plt.bar(index+2*bw, l_distance, bw, color='r')
plt.bar(index+3*bw, l_chance, bw, color='yellow')
plt.xticks(index+1.5*bw, np.arange(1,12))
plt.xlabel("Уровень")
plt.show()
```
![image](https://github.com/ManualCode/DragonPicker-in-GameDev-lab3/assets/120582775/1461b710-ce6e-493b-bf49-9c23b6845d67)
![image](https://github.com/ManualCode/DragonPicker-in-GameDev-lab3/assets/120582775/b62305e2-cbbd-4409-8ea9-e7c5e59193e4)
![image](https://github.com/ManualCode/DragonPicker-in-GameDev-lab3/assets/120582775/14afa059-673f-4220-9b96-dd929ce98516)
![image](https://github.com/ManualCode/DragonPicker-in-GameDev-lab3/assets/120582775/4858fe80-58e7-4a32-b34e-5a07419baebd)
![image](https://github.com/ManualCode/DragonPicker-in-GameDev-lab3/assets/120582775/032d815b-b306-4b8b-8eb1-1bd4217d9f7f)

## Выводы

Абзац умных слов о том, что было сделано и что было узнано.

| Plugin | README |
| ------ | ------ |
| Dropbox | [plugins/dropbox/README.md][PlDb] |
| GitHub | [plugins/github/README.md][PlGh] |
| Google Drive | [plugins/googledrive/README.md][PlGd] |
| OneDrive | [plugins/onedrive/README.md][PlOd] |
| Medium | [plugins/medium/README.md][PlMe] |
| Google Analytics | [plugins/googleanalytics/README.md][PlGa] |

## Powered by

**BigDigital Team: Denisov | Fadeev | Panov**
