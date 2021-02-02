1. Вызов рендера. Первым делом получаем массив задач из хранилища.
2. Проверка localStorage: если tasks есть -- отдаём их. 
Если нет -- устанавливаем дефолт и тогда отдаём json, преобразованный в массив.
3. Находим элемент списка (с id root, например) и вставляем туда
строку с хтмл разметкой (innerHTML), которую формируем исходя из
элементиков нашего массива. Формируем, опять же, с помощью функции.
4. После вставки html в страницу, вызываем функцию, которая повесит обработчики событий 
на все элементы, на которые нужно (кнопки, инпуты и тд), предварительно удалив их, если они уже есть.

В каждой функции (ну, или почти, будет видно потом), которая будет вызвана при событиях, будет в конце вызываться рендер, поскольку мы в этих функциях меняем localStorage каким-то образом. Соответственно список задач надо будет изменить - перерисовать. А этим занимается рендер, который получает каждый раз свежий localStorage.

ОСНОВНЫЕ ФУНКЦИИ

- Добавление задачи
- Удаление задачи
- Удаление всех задач
- Редактирование задачи
- Смена статуса задачи
Ещё есть вспомогательные функции
- Для работы с localStorage
- Для сортировки (активные вверху, зачёркнутые внизу)



✦ ✦ ✦ДОБАВЛЕНИЕ ЗАДАЧИ 
По клику на кнопку "Добавить" и по enter. 
- enter
   Событие - keydown, при нажатии на любую клавишу будем проверять code, если равен
   13 идём дальше. А дальше получаем значение инпута (после того как проверим 
   на пустоту и проч) и передаём функции, которая добавит новый объект 
   (см. Примечания п. 2) в localStorage и вызовет перерендеринг.
- click "Добавить"
   Находим ближайший элемент инпута, получаем его значение и дальше как в примере выше
   вызываем функцию.

✦ ✦ ✦УДАЛЕНИЕ ЗАДАЧИ
При клике на кнопку "Удалить", мы должны получить data-id родителя 
(li class="task-list__task task"), обратиться к localStorage, передав туда значение data-id, выдрать оттуда json объект, преобразовать его в массив, найти нужную таску по id, 
удалить её и вставить этот массив вместо того, что сейчас в localStorage.
Перерендеринг. 

✦ ✦ ✦УДАЛЕНИЕ ВСЕХ ЗАДАЧ
По клику на кнопку "очистить"(или как мы её назовём), вызываем функцию,
которая удалит все значения в localStorage и вызовет перерендеринг.

✦ ✦ ✦РЕДАКТИРОВАНИЕ ЗАДАЧИ
- p.task__desc - onclick
- input.task__input - onchange, keydown
По клику на p.task__desc вызываем функцию, которая поменяет видимость
кликнутого (есть ли такое слово??) элемента и инпута ниже. Инпут станет видимым
и focused, а значение у него будет как текст p. Onchange сработает при потере фокуса (когда тыкнем на любое место на странице, кроме инпута) и вызовем функцию, которая снова изменит видимость p и инпута на противоположные (с помощью удаления/добавления класса).

Если keydown - то получаем value и data-id у родителя, если onclick - кроме data-id у родителя, получаем value ближайшего соседнего инпута.
Передаём их функции, которая обратится к localStorage, достанет json объект с задачами, преобразует его в массив. Найдёт по id нашу задачу, вставит значение value вместо того,
что там сейчас. Перерендеринг.

✦ ✦ ✦СМЕНА СТАТУСА ЗАДАЧИ
Чекбокс будет кастомный. При клике на кастомный чекбокс, мы получаем data-id у родителя,
вызываем функцию, в которую передадим этот id, она обратится к localStorage, достанет json объект с задачами, преобразует его в массив. Найдёт по id нашу задачу и поменяет у неё значение done на противоположное.

- ВИД ЭЛЕМЕНТОВ (примерно)
<ul class="task-list"> -----> taskList.js
   <li class="task-list__task task" data-id="1"> -----> taskBlock.js 
      <input class="task__checkbox" type="checkbox" {task.done ? 'checked' : ''}/> -----> taskCheckbox.js
      <p class="task__desc">${task.desc}</p> -----> taskDesc.js
         или
      <input class="task__input" value="Simple task"/> -----> taskInput.js
      <button>Удалить</button> -----> taskDeleteButton.js
   </li>
</ul>

Примечания:
1. В localStorage будет json объект, который мы будем преобразовывать в массив с задачами
2. Массив с задачами будет вида:
[
   {
      id: 1,
      taskDesc: "Simple task",
      done: false
   },
   {
      id: 2,
      taskDesc: "Difficult task",
      done: true
   }
]