Задание
Добавьте к классам Text, Circle и Polyline SVG-библиотеки поддержку следующих методов:
SetFillColor(Color color) задаёт значение свойства fill — цвет заливки. По умолчанию свойство не выводится.
SetStrokeColor(Color color) задаёт значение свойства stroke — цвет контура. По умолчанию свойство не выводится.
SetStrokeWidth(double width) задаёт значение свойства stroke-width — толщину линии. По умолчанию свойство не выводится.
SetStrokeLineCap(StrokeLineCap line_cap) задаёт значение свойства stroke-linecap — тип формы конца линии. По умолчанию свойство не выводится.
SetStrokeLineJoin(StrokeLineJoin line_join) задаёт значение свойства stroke-linejoin — тип формы соединения линий. По умолчанию свойство не выводится.
Эти методы должны поддерживать method chaining, то есть возвращать ссылку на тип объекта, у которого были вызваны.
Тип Color объявите на основе типа std::string. Также объявите константу NoneColor, имеющую значение none:

namespace svg {
...
using Color = std::string;

// Объявив в заголовочном файле константу со спецификатором inline,
// мы сделаем так, что она будет одной на все единицы трансляции,
// которые подключают этот заголовок.
// В противном случае каждая единица трансляции будет использовать свою копию этой константы
inline const Color NoneColor{"none"};
...
}

Типы StrokeLineCap и StrokeLineJoin объявляются с использованием enum:
namespace svg {
...
enum class StrokeLineCap {
    BUTT,
    ROUND,
    SQUARE,
};

enum class StrokeLineJoin {
    ARCS,
    BEVEL,
    MITER,
    MITER_CLIP,
    ROUND,
};
...
} // namespace svg

Перегрузите для StrokeLineCap и StrokeLineJoin операцию << для вывода в std::ostream как показано ниже.

StrokeLineCap	Формат вывода
BUTT	butt
ROUND	round
SQUARE	square
StrokeLineJoin	Формат вывода
ARCS	arcs
BEVEL	bevel
MITER	miter
MITER_CLIP	miter-clip
ROUND	round

Класс shapes::Snowman должен использовать следующие цвета:
Заливка: rgb(240,240,240)
Контур: black
Класс shapes::Star должен использовать следующие цвета:
Заливка: red
Контур: black
Класс shapes::Triangle оставьте без изменений.

Пример использования:
int main() {
    using namespace svg;
    using namespace shapes;
    using namespace std;

    vector<unique_ptr<svg::Drawable>> picture;
    picture.emplace_back(make_unique<Triangle>(Point{100, 20}, Point{120, 50}, Point{80, 40}));
    picture.emplace_back(make_unique<Star>(Point{50.0, 20.0}, 10.0, 4.0, 5));
    picture.emplace_back(make_unique<Snowman>(Point{30, 20}, 10.0));

    svg::Document doc;
    DrawPicture(picture, doc);

    const Text base_text =  //
        Text()
            .SetFontFamily("Verdana"s)
            .SetFontSize(12)
            .SetPosition({10, 100})
            .SetData("Happy New Year!"s);
    doc.Add(Text{base_text}
                .SetStrokeColor("yellow"s)
                .SetFillColor("yellow"s)
                .SetStrokeLineJoin(StrokeLineJoin::ROUND)
                .SetStrokeLineCap(StrokeLineCap::ROUND)
                .SetStrokeWidth(3));
    doc.Add(Text{base_text}.SetFillColor("red"s));

    doc.Render(cout);
}
Ожидаемый результат (порядок перечисления атрибутов может отличаться):
<?xml version="1.0" encoding="UTF-8" ?>
<svg xmlns="http://www.w3.org/2000/svg" version="1.1">
  <polyline points="100,20 120,50 80,40 100,20" />
  <polyline points="50,10 52.3511,16.7639 59.5106,16.9098 53.8042,21.2361 55.8779,28.0902 50,24 44.1221,28.0902 46.1958,21.2361 40.4894,16.9098 47.6489,16.7639 50,10" fill="red" stroke="black"/>
  <circle cx="30" cy="70" r="20" fill="rgb(240,240,240)" stroke="black"/>
  <circle cx="30" cy="40" r="15" fill="rgb(240,240,240)" stroke="black"/>
  <circle cx="30" cy="20" r="10" fill="rgb(240,240,240)" stroke="black"/>
  <text fill="yellow" stroke="yellow" stroke-width="3" stroke-linecap="round" stroke-linejoin="round" x="10" y="100" dx="0" dy="0" font-size="12" font-family="Verdana">Happy New Year!</text>
  <text fill="red" x="10" y="100" dx="0" dy="0" font-size="12" font-family="Verdana">Happy New Year!</text>
</svg>

Описание формата выходных данных
Требования к содержимому, выводимому методом svg::Document::Render, аналогичны требованиям из предыдущего урока.
Ограничения
Ограничения допускают любое адекватное линейное решение: за одну секунду нужно обрабатывать порядка 10 000 вызовов конструкторов объектов и методов выставления свойств, а также один вызов метода svg::Document::Render.
Гарантируется, что при тестировании тренажёр будет передавать валидные параметры в методы классов и их конструкторы. Поэтому усложнять код проверками не нужно.
Что отправлять на проверку
Код библиотеки должен располагаться в файлах svg.h и svg.cpp. Файл main.cpp должен содержать классы Triangle, Snowman и Star в пространстве имён shapes и необходимые для их работы функции. Функция main будет заменена на версию из тренажёра.
Как будет тестироваться ваш код
Тренажёр проверит работу классов shapes::Triangle, shapes::Snowman и shapes::Star, а также классов объектов библиотеки svg.
