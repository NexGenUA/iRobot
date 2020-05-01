# iRobot

Код выглядит следующим образом и будет выполняться каждый шаг (каждую секунду):
```javascript
function program(robot) {
    // enter yor code here
}
```
Робот может обнаруживать стены и другие препятствия с помощью сканера. Для использования сканера необходимо выполнить следующий код:
```javascript
function program(robot) {
    var scanner = robot.getScanner();
    if (scanner.at("RIGHT") != "WALL") {
        robot.go("RIGHT");
    };
}
```
Доска может содержать следующие объекты:

- ```START``` - Начальная точка. Место возрождения
- ```EXIT``` - Конечная точка. Принеси деньги сюда.
- ```WALL``` - Граница доски. Перепрыгнуть это невозможно
- ```HOLE``` - Дыра в полу. Робот умрет, если он наступит на него.
- ```BOX``` - Барьер, который можно перемещать. Робот может перепрыгнуть через него.
- ```GOLD``` - Вы должны собрать его и довести до финиша.
- ```MY_ROBOT``` - Это твой робот. Помогите ему собрать золото и довести его до финиша.
- ```ZOMBIE``` - Это зомби.
- ```OTHER_ROBOT``` - Робот других игроков.
- ```LASER_MACHINE``` - Лазерный станок производит лазерные лучи, которые могут убить роботов, если они его пересекают.
- ```LASER_MACHINE_READY``` - В этом состоянии лазер сообщает вам, что при следующем тике он испустит смертельный луч.
- ```LASER_LEFT``` - Лазерный луч движется влево.
- ```LASER_RIGHT``` - Лазерный луч движется вправо.
- ```LASER_UP``` - Лазерный луч движется вверх.
- ```LASER_DOWN``` - Лазерный луч движется вниз.

Робот может ходить, перепрыгивать через дыры и лазеры и тянуть коробки со следующим кодом:
```javascript
robot.go("LEFT");
robot.jump("RIGHT");
robot.pull("UP");
```
Вы можете использовать этот метод для обнаружения элементов по всему миру:
```javascript
var scanner = robot.getScanner();
var point = new Point(4, 8);
if (scanner.at(point) == "HOLE") {
    // some statement here
}
```
Координаты {x: 0, y: 0} в левом нижнем углу доски.

Для отображения данных в консоли используйте следующий код:

```javascript
var someVariable = "someData";
robot.log(someVariable);
```

Используйте метод getMe() для получения положения робота.
```javascript
robot.log(scanner.getMe());
```
# ОСНОВНЫЕ МЕТОДЫ РОБОТА

**function log(message)** - Выводит сообщение в консоль
```javascript
var message = 'some message';
robot.log(message);
```
**function invert(direction)** - Возвращает перевернутое направление. Инвертирует UP в DOWN, RIGHT в LEFT и т.д.
```javascript
assert robot.invert("UP") == "DOWN";
assert robot.invert("LEFT") == "RIGHT";
```
**function cameFrom()** - Получает направление робота откуда он пришел
```javascript
robot.go("UP");
// next tick
assert robot.cameFrom() == "DOWN";
```
**function previousDirection()** - Получает направление движение робота
```javascript
robot.go("UP");
// next tick
assert robot.previousDirection() == "UP";
```
**function go(direction)** - Идите в указанном направлении. Для каждого направления есть 4 соответствующие функции: goLeft, goRight, goUp, goDown.
```javascript
robot.go("LEFT");
// robot will go up
// the same about jump and pull
// robot.jump("RIGHT");
// robot.pull("UP");
```
**function jump(direction)** - Переходит в указанном направлении. Перепрыгивает через барьер в указанном направлении. Требуется 2 тика. После этого робот движется на 2 клетки по направлению. Невозможно перепрыгнуть через несколько барьеров в ряду. Для каждого направления есть 4 соответствующие функции: jumpLeft, jumpRight, jumpUp, jumpDown.
```javascript
robot.jump("RIGHT");
```
**function pull(direction)** - Тянет / толкает коробку в направлении. Коробку можно перемещать только вперед или назад, «боковое смещение» не допускается. Для каждого направления есть 4 соответствующие функции: pullLeft, pullRight, pullUp, pullDown.
```javascript
robot.pull("UP");
```
**function fire(direction)** - Стреляй в заданном направлении. Для каждого направления есть 4 соответствующие функции: fireLeft, fireRight, fireUp, fireDown.
```javascript
robot.fire("DOWN");
```
**function getMemory()** - Объект помогает хранить / передавать данные ключа / значения между ходами / тиками, содержит следующие методы:
    
- **function has(key)** - Проверьте, существуют ли данные / значение с указанным ключом
- **function save(key, value)** - Сохраняет значение с указанным ключом
- **function remove(key)** - Удаляет данные / значение по ключу
- **function load(key)** - Получает данные / значение по ключу
- **function clean()** - Удаляет все данные
```javascript
var memory = robot.getMemory();
assert memory.has("key") == false;
memory.save("key", "value");
memory.save("key2", "value2");
assert memory.load("key") == "value";
assert memory.has("key") == true;
memory.remove("key");
assert memory.has("key") == false;
assert memory.has("key2") == true;
memory.clean();
assert memory.has("key2") == false;
```
# ОСНОВНЫЕ МЕТОДЫ СКАНЕРА

**function at(direction)** - Возвращает значение элемента в указанном направлении. Для каждого направления есть 4 соответствующие функции: atLeft, atRight, atUp, atDown.
```javascript
assert scanner.at("UP") == "HOLE";
assert scanner.atLeft() == "WALL";
assert scanner.atRight() == "BOX";
assert scanner.atUp() == "HOLE";
assert scanner.atDown() == "GOLD";
```
**function at(point)** - Возвращает элементы в ячейке.
```javascript
assert scanner.at(new Point(6, 2)) == "HOLE,OTHER_ROBOT";
assert scanner.at(1, 2) == "START,MY_ROBOT";
```
**function atNearRobot(dx, dy)** - Возвращает элемент в ячейке с координатами.
```javascript
var scanner = robot.getScanner();
var xOffset = 1;
var yOffset = -2;
var element = "HOLE";
assert scanner.atNearRobot(xOffset, yOffset) == element;
```
**function getMe()** - Возвращает позицию вашего робота ({x: 0, y: 0} в левом нижнем углу).
```javascript
var point = scanner.getMe();
assert point.getX() == 7;
assert point.getY() == 5;
```
**function getScannerOffset()** - Возвращает положение вашего сканера внутри карты - расстояние между левым нижним углом сканера и левым нижним углом карты с координатой {x: 0, y: 0}.
```javascript
var point = scanner.getScannerOffset();
assert point.getX() == 7;
assert point.getY() == 5;
```
**function getAt(x, y)** - Возвращает элемент в указанной позиции.
```javascript
scanner.getAt(12, 7) == ["START", "HERO"];
```
**function findAll(elementType)** - Возвращает все позиции указанного элемента. А как насчет тумана войны?
```javascript
var points = scanner.findAll("HOLE");
assert points == [new Point(5, 12), new Point(3, 7)];
assert points[0].getX() == 5;
assert points[0].getY() == 12;
```
**function isAt(x, y, elementType)** - Проверяет, содержит ли позиция элемент с указанным типом.
```javascript
assert scanner.isAt(12, 7, "HOLE") == false;
```
**function isAnyOfAt(x, y, elementTypes)** - Проверяет, содержит ли позиция какие-либо элементы с указанными типами.
```javascript
assert scanner.isAnyOfAt(12, 7, ["HOLE", "START", "HERO"]) == false;
```
**function isNear(x, y, elementTypes)** - Проверяет, содержат ли ячейки вокруг позиции {x, y} какие-либо элементы с указанными типами
```javascript
assert scanner.isNear(12, 7, ["HOLE", "START", "HERO"]) == true;
assert scanner.isNear(12, 7, "HERO") == true;
// near means: atLeft || atRight || atUp || atDown
```
**function isBarrierAt(x, y)** - Возможно ли пройти (не перепрыгнуть) через ячейку с {x, y} координатами.
```javascript
assert scanner.isBarrierAt(12, 7) == false;
```
**function countNear(x, y, elementType)** - Возвращает количество элементов с типом, указанным вокруг точки {x, y}.
```javascript
assert scanner.countNear(12, 7, "HOLE") == 0;
assert scanner.countNear(12, 7, "BOX") == 3;
```
**function isMyRobotAlive()** - Проверяет, жив ли ваш робот
```javascript
assert scanner.isMyRobotAlive() = true;
```
**function getElements()** - Возвращает список типов элементов.
```javascript
assert scanner.getElements() == ['NONE', 'WALL', 'MY_ROBOT', 'OTHER_ROBOT',
'LASER_MACHINE', 'LASER_MACHINE_READY',
'LASER_LEFT', 'LASER_RIGHT', 'LASER_UP', 'LASER_DOWN',
'START', 'EXIT', 'GOLD', 'HOLE', 'BOX']; 
```
**function getWholeBoard()** - Возвращает все элементы на доске. Это трехмерный массив [x] [y] [elements].
```javascript
assert scanner.getWholeBoard() ==
[[['WALL'],['WALL'],['WALL'],['WALL']],
[['WALL'],['START'],['HOLE','OTHER_ROBOT'],['WALL']],
[['WALL'],['MY_ROBOT'],['GOLD'],['WALL']],
[['WALL'],['WALL'],['WALL'],['WALL']]];
```
**function getOtherRobots()** - Возвращает список координат для всех видимых вражеских роботов. Для каждого типа элементов существуют соответствующие функции: getLaserMachines, getLasers, getWalls, getBoxes, getGold, getStart, getExit, getHoles
```javascript
var points = scanner.getOtherRobots();
assert points == [new Point(5, 12), new Point(3, 7)];
assert points[0].getX() == 5;
assert points[0].getY() == 12;
```
**function getShortestWay(to)** - Вернуть кратчайший путь (список координат) от местоположения вашего робота до указанных координат.
```javascript
var destination = new Point(2, 4);
var nextStep = scanner.getShortestWay(destination);
assert nextStep[0].getX() = 3;
assert nextStep[0].getY() = 4;
```
**function getShortestWay(from, to)** - Вернуть кратчайший путь (список координат) между двумя координатами.
```javascript
var from = new Point(2, 4);
var to = new Point(2, 4);
var nextStep = scanner.getShortestWay(from, to);
assert nextStep[0].getX() = 3;
assert nextStep[0].getY() = 4;
```
