# Shri homework: CI server

## Вопросы:
1. правильное использование БЭМ-сущностей:
    1. какие части макета являются одним и тем же блоком?
    1. какие стили относятся к блокам, а какие к элементам и модификаторам?
    1. где нужно использовать каскады и почему?
1. консистентность:
    1. какие видите базовые и семантические константы?
    1. какие видите закономерности в интерфейсе?
1. адаптивность
    1. где видите вариативность данных и как это обрабатываете?
    1. какие видите особенности, связанные с размером экрана?
    1. что еще повлияло на вашу вёрстку?

По каждому пункту кратко напишите принципы/правила, которыми вы руководствуетесь. Укажите места макета, которые их иллюстрируют. Примените эти правила в своей вёрстке.

---

## Ответы:
1.1. Шапка, футер, кнопка, блок карточки с тенью, иконки, инпуты, заголовок в шапке страницы, текстовый блок.

1.2. Стили отвечающие за внешний вид в общем относятся к блокам. Те стили, которые отвечают за позицию и внешнюю геометрию объекта интерфейса, относятся к элементам. А стили которые опциональные, зависящие от статуса сущности, относятся к модификаторам.

1.3. Например, когда значение модификатора на блоке меняет позицию его элемента(или вообще его наличие).

2.1. Базовые: палитра цветов, таблица размеров шрифта, таблица размеров отступов. Семантические: цвет задизэйбленной кнопки, цвет футера страницы, цвет кнопки при наведении и тд.

2.2. На каждой странице есть шапка, футер и область с контентом. Все внешние отступы на странице чётные.

3.1. На странице настроек: в будущем могут добавиться ещё новые поля/инпуты, нужно это учесть, чтобы форма могла быть любой высоты, и кнопки "Save" / "Cancel" отображались корректно. Длина названия репозитория может быть любой, поэтому если название не влазит в одну строку, должен быть перенос по буквам на следующую строку(тк в названии репозитория не бывает пробелов, надо использовать `overflow-wrap: break-word`). Также на странице `build details` показывается блок с логом билда, для генерации этого блока я использовал утилиту [aha - Ansi HTML Adapter](https://github.com/theZiz/aha) написанную на C, и вывод сборки webpack'ом, на мобильном экране видно что в этом блоке добавляется горизонтальный скролл, чтобы можно было посмотреть нормально вывод, а вертикальный не появляется. `overflow-x: auto; overflow-y: hidden;`. Также поле даты/времени сборки не учитывается год что не очень хорошо, так как если этот сервер сборки будет работать больше 1 года, то будет всё тяжелее и тяжелее понять в какую именно дату была сборка(поэтому на сам текстовый блок с датой, я добавил атрибут `title` с полной датой с годом, чтобы она показывалась при ховере).

3.2. На странице настроек инпуты имеют ширину 100% с указанием максимальной ширины в 474px, чтобы на узком экране они полностью вмещались на страницу без горизонтального скролла. На странице `build history` кол-во сборок может быть разное, и на сколько я понимаю, в зависимости от высоты viewport экрана (в десктопной версии) должно показываться разное стартовое кол-во строк сборок, чтобы было видно как можно больше информации, и также сразу же была видна кнопка "Show more". (+ если учесть сколько строк показывается в мобильной версии, возможно минимальное кол-во строк с билдами = 5, а если высота viewport'a позволяет показать больше, то показывается больше). Название коммита я обрезаю троеточием, если оно не влазит в отведённую строку для него.

3.3. На странице настроек инпут `GitHub repository` обязателен к заполнению, но я не нашёл блока с подсказкой об ошибке, и также не нашёл как выглядит инпут с ошибкой, но догадываюсь что обводка инпута при ошибке становится красной `--red-500`. Также инпут с вводом кол-ва минут, разрешает вводить только цифры и ничего больше, поэтому для улучшения юзабилити пользователей на мобильных устройствах(чтобы сразу открывалась клавиатура только с цифрами, и была проверка на ввод чисел строго больше 0, так как синхронизировать каждые 0 минут я считаю слишком большой нагрузкой на сервер) я использовал `<input type="text" inputmode="numeric" pattern="^[0]*[1-9][\d]*$">`. Также возможно стоит сделать чтобы инпут `Main branch` был не инпутом, а селектом, из уже доступных веток в введённом репозитории выше(как было сделано на странице автотестов вступительного задания ШРИ). Текстовые блоки с указание хэша коммита, имени пользователя, стоит сделать ссылками, чтобы при клике по ним можно было легко перейти на страницу коммита и почитать изменения и информацию о авторе, соответственно(но если [делать это без js](https://css-tricks.com/nested-links/) выглядит довольно костыльно, и поэтому это реализовывать я не стал, но возможно добавлю когда к вёрстке будем добавлять js). Константы для ширины экрана, я сделал sass-переменными, так как css-переменные нельзя использовать в @media.

---
## Значение брейкпоинтов
С версии десктопа на планшеты выбрал равным 768px, так как это наиболее популярное значение покрывающее большинство различных устройств. Второй брейкпоинт равен 576px, когда футер становится высотой в 2 строки, и вёрстка полностью становится как в макете для мобайл. Минимальная ширина страницы 270px. И ширина страницы когда страница `build history` начинает выглядеть как в мобайл 320px.