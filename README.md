## Комп'ютерні системи імітаційного моделювання

## СПм-23-5, Лобач Яків Вадимович
### Лабораторна робота №2. Редагування імітаційних моделей у середовищі NetLogo

### [Варіант 7: Wolf Sheep Predation](https://www.netlogoweb.org/launch#http://www.netlogoweb.org/assets/modelslib/Sample%20Models/Biology/Wolf%20Sheep%20Predation.nlogo)

---

### Зміни у вихідній логіці моделі, відповідно до завдання:

#### Логіка поведінки овець:
- Якщо вівця бачить вовка в радіусі 5 клітинок, вона намагається втекти від нього.
- Внесено зміну в процедуру руху овець, де функцію `move` було змінено на `sheep-move`.

```netlogo
to sheep-move
  let nearest-wolf one-of wolves in-radius 5 ; знаходить найближчого вовка в радіусі видимості
  ifelse nearest-wolf != nobody [
    face nearest-wolf
    rt 180 ; повертається в протилежний бік
    fd 1   ; рухається вперед
  ]
  [
    flock
  ]
end
```
#### Логіка поведінки вовків:
- Якщо вовк бачить іншого вовка в радіусі 5 клітинок, він змінює напрямок і віддаляється.
```netlogo
 to wolf-run-from-wolf
  let nearest-wolf one-of other wolves in-radius 5 ; знаходить найближчого вовка
  ifelse nearest-wolf != nobody [
    face nearest-wolf
    rt 180
    fd 1
  ]
  [
    wolf-move
  ]
end
```
- Якщо два вовки опиняються на одній клітинці, один із них зникає.
```netlogo
to eat-wolf
  let wolf-in-same-chunk one-of other wolves in-radius 1
  if wolf-in-same-chunk != nobody [
    ask wolf-in-same-chunk [ die ]
    set energy energy + wolf-gain-from-food / 2
  ]
end
```
- Вовки женуться за найближчою вівцею в радіусі видимості 10 клітинок.
```netlogo
to wolf-move
  let nearest-sheep one-of sheep in-radius 10
  ifelse nearest-sheep != nobody [
    face nearest-sheep
    fd 1
  ]
  [
    move
  ]
end
```
#### Власні покращення моделі:
- Якщо овечок поруч немає, вівці рухаються випадково або ж збираються в отару, якщо поблизу є інші вівці.
```netlogo
to flock
  let neighbor other sheep in-radius 1
  ifelse any? neighbor [
    let average-heading mean [heading] of neighbor
    turn-towards average-heading 
  ]
  [
    move
  ]
end

to turn-towards [ new-heading ]
  let current-heading heading
  let turn-angle subtract-headings new-heading current-heading
  rt turn-angle
  fd 1
end
```
#### Скріншот моделі під час симуляції:
![image](https://github.com/Avareco/Ksim/assets/31128616/7a7f4f89-ecb3-4930-9180-7e4431094c10)
## Обчислювальні експерименти 
### 1. Дослідження залежності популяції вовків від насиченості від однієї вівці
Досліджено залежність змін у чисельності популяції вовків від показника "насиченість від вівці", тобто кількості енергії, яку отримує один вовк за поїдання однієї вівці. В процесі змінюється значення параметра **wolf-gain-from-food**, тоді як решта параметрів залишаються за замовчуванням:

- **initial-number-sheep**: 100
- **initial-number-wolves**: 50
- **sheep-gain-from-food**: 4
- **grass-regrowth-time**: 30
- **wolf-reproduce**: 5%
- **sheep-reproduce**: 4%

<table>
<thead>
<tr><th>Насиченість від вівці</th><th>Середня кількість вовків</th></tr>
</thead>
<tbody>
<tr><td>20</td><td>47</td></tr>
<tr><td>25</td><td>67</td></tr>
<tr><td>30</td><td>123</td></tr>
<tr><td>35</td><td>128</td></tr>
<tr><td>40</td><td>112</td></tr>
<tr><td>45</td><td>90</td></tr>
<tr><td>50</td><td>33</td></tr>
</tbody>
</table>
![image](https://github.com/user-attachments/assets/7f215fb2-43d0-46ba-a31a-77d85d051bac)
Таблиця демонструє, що для стабільності екосистеми оптимальний показник "насиченість від вівці" для вовка варіюється від 30 до 45. При нижчих значеннях популяція вовків швидко скорочується через голод, а при вищих — надмірне розмноження вовків призводить до швидкого знищення овець і подальшого голодування хижаків.
