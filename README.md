## Комп'ютерні системи імітаційного моделювання
## СПм-23-3, **Ірха Дмитро Максимович**
### Лабораторна робота №**3**. Використання засобів обчислювального інтелекту для оптимізації імітаційних моделей

### Варіант 6, модель у середовищі NetLogo:
[Rabbits Grass Weeds](http://www.netlogoweb.org/launch#http://www.netlogoweb.org/assets/modelslib/Sample%20Models/Biology/Rabbits%20Grass%20Weeds.nlogo)

### Опис моделі:
Ця модель демонструє взаємодію трьох основних компонентів екосистеми: трави, кроликів та бур'яну. Трава виступає головним джерелом їжі для кроликів, тоді як бур'ян конкурує з травою за ресурси, але не є основною їжею для кроликів. Взаємодії між цими трьома компонентами дозволяють вивчати такі екологічні процеси, як динаміка популяцій, співіснування різних видів та конкуренція за ресурси.

### Ключові параметри моделі:
- number-of-rabbits — стартова кількість кроликів.
- birth-threshold — мінімальний рівень енергії для розмноження.
- grass-grow-rate — швидкість росту трави за одиницю часу.
- grass-energy — кількість енергії, яку отримує кролик, з'ївши траву.
- weeds-grow-rate — швидкість росту бур’яну за одиницю часу.
- weeds-energy — кількість енергії, яку кролик отримує від бур’яну.

### Вихідні дані симуляції:
- Кількість трави.
- Кількість бур’яну.
- Поточна популяція кроликів.

### Налаштування BehaviorSearch:
**Модифікації моделі**<br>
До базової моделі були внесені наступні зміни:

- Додано ймовірність отруєння кроликів бур’яном. Отруєні кролики не можуть їсти, рухатися та розмножуватися протягом трьох ітерацій і позначаються окремим кольором.
- Кролики тепер поділяються на самців і самок.
- Нові кролики можуть народжуватися тільки за умов ситості, здоров’я та присутності поруч кролика протилежної статі.
- Розмноження відбувається з ймовірністю 50%.
- Додано параметр "вік" для кроликів, який визначає тривалість їхнього життя.
- Народжувати потомство можуть лише самки (1-2 дитинчати за випадковим вибором).

**Діапазони параметрів моделі**<br>
*Параметри моделі були автоматично завантажені через BehaviorSearch:*:
<pre>
["grass-grow-rate" [0 1 20]]
["weeds-grow-rate" [0 1 20]]
["grass-energy" [0 0.5 10]]
["weed-energy" [0 0.5 10]]
["number" [0 150 500]]
["birth-threshold" [5 1 20]]
</pre>
*Значення нижнього порогу енергії для розмноження (birth-threshold) було збільшено до 5, щоб наблизити симуляцію до реальності.*

**Оцінка фітнес-функції**<br>
Для оцінки ефективності роботи моделі використовується середня кількість кроликів у середовищі протягом 500 тактів. Міра фітнес-функції визначається як:

<pre> count rabbits </pre>
Для зменшення впливу випадковості симуляція повторюється 10 разів, а результат обчислюється як середнє значення.

Середня кількість кроликів у середовищі симуляції повинна враховуватися за весь період моделювання, який триває 500 тактів (оскільки це значення змінюється в кожному такті). Підрахунок починається з нульового такту (параметр Step limit). Для цього необхідно задати параметр "Measure if" зі значенням true.

У деяких випадках, через початковий хаос у моделі, доцільно не враховувати перші такти симуляції. Для цього можна задати специфічні умови в параметрі **Measure if**.

Параметри **Setup** і **Go** відповідають за процедури ініціалізації та запуску моделі відповідно. У процесі симуляції BehaviorSearch автоматично викликає ці процедури.

У даній лабораторній роботі параметр зупинки за умовою **Stop if** не використовується.

**Налаштування цільової функції** (Search Objective)<br>

Мета налаштування параметрів імітаційної моделі — максимізація середньої популяції кроликів у середовищі існування. Це означає, що потрібно знайти такі параметри моделі, які забезпечують найбільше середнє значення популяції кроликів.

Ця мета задається через параметр **Goal** із вибором значення Maximize Fitness. Важливо враховувати не значення популяції в конкретний момент часу, а її середнє значення за весь період симуляції, який триває 500 тактів. Для цього у параметрі **Collected measure** обирається значення MEAN_ACROSS_STEPS.

Щоб мінімізувати вплив випадковості в логіці моделі, кожна симуляція виконується 10 разів, а результат обчислюється як середнє арифметичне.

**Налаштування алгоритму пошуку** (Search Algorithm)<br>
На цьому етапі була визначена модель, налаштовані її параметри та вибрана міра ефективності, яка лежить в основі функції пристосованості. Це дозволяє оцінювати "якість" кожного з варіантів рішень, що перевіряються за допомогою BehaviorSearch.

У дослідженні будуть використані два алгоритми: Випадковий пошук (RandomSearch) та Простий генетичний алгоритм (StandardGA). Для цих алгоритмів потрібно задати наступні параметри:

- Evaluation limit — кількість ітерацій пошуку; для StandardGA це буде кількість поколінь.
- Search Space Encoding Representation — спосіб кодування варіантів рішень.
Оскільки загальноприйнятого "найкращого" способу кодування не існує, необхідно підібрати відповідний для конкретної моделі.

Параметр Use fitness caching впливає виключно на продуктивність.

### Результати використання BehaviorSearch:

Було проведено експерименти з усіма доступними налаштуваннями параметра "**Search Space Encoding Representation**".
Нижче наведено таблицю результатів Fitness:

<table>
<thead>
<tr><th></th><th>GrayBinary</th><th>StandardBinary</th><th>MixedType</th><th>RealHypercube</th></tr>
</thead>
<tbody>
<tr><td>RandomSearch</td><td>484.9</td><td>484.9</td><td>478.9</td><td>456.2</td></tr>
<tr><td>StandardGA</td><td>496.2</td><td>579.6</td><td>603.9</td><td>591.7</td></tr>
</tbody>
</table>
