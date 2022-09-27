# РАЗРАБОТКА ИГРОВЫХ СЕРВИСОВ
Отчет по лабораторной работе #1 выполнил(а):
- Кыров Валентин Сергеевич
- 1922524
Отметка о выполнении заданий (заполняется студентом):

| Задание | Выполнение | Баллы |
| ------ | ---------- | ------ |
| Задание 1 | *          | 60 |
| Задание 2 | *          | 20 |
| Задание 3 | *          | 20 |

знак "*" - задание выполнено; знак "#" - задание не выполнено;

Работу проверили:
- к.т.н., доцент Денисов Д.В.
- к.э.н., доцент Панов М.А.
- ст. преп., Фадеев В.О.


## Цель работы
Ознакомиться с основными функциями Unity и взаимодействием с объектами внутри редактора.

## Задание 1
### В разделе «ход работы» пошагово выполнить каждый пункт с описанием и примера реализации задач по теме видео «Практическая работа 1-4».
Ход работы:
1. Был создан новый проект 3D-Core под названием GameServices, в дальнейшем вся работа будет проходить в этом проекте. [pic 1](https://drive.google.com/file/d/14WnskIyDgGCBZCJIN7sxEGTqSo85K3uU/view?usp=sharing)
2. В дальнейшем в качестве редактора кода был указан VSCode. 
3. С помощью меню GameObject -> 3D Object был создан объект Plane
4. С помощью меню GameObject -> 3D Object был создан объект Cube
5. С помощью меню GameObject -> 3D Object был создан объект Sphere [pic 2 пункты 3-5](https://drive.google.com/file/d/1NId8BSxX67R8x984uoGO-HU-BS2soP7d/view?usp=sharing)
6. В объекте Sphere коллайдер включён по умолчанию, его границы соответствуют границам объекта
7. В Inspector компонента коллайдера отмечен галочкой параметр Is Trigger [pic 3](https://drive.google.com/file/d/1suiYb3clYUGOX7MYqwOU7KanWS_pxrt0/view?usp=sharing)
8. В контекстном меню вкладки Assets по пути Create -> Material был создан материал Red_Color. Перетащив материал на объект Cube последний окрасился в красный цвет. [pic 4](https://drive.google.com/file/d/1kaSXNrKRx7z5n_vF6zVE-6vTm8PY7oOQ/view?usp=sharing)
9. С помощью кнопки Add Component в объект Cube был добавлен компонент Rigidbody, делающий из игрового объекта физическое тело. Пока в компоненте не включен параметр Use Gravity, объект будет являться статичным, т.е не будет проваливаться под плоскость. [pic 5](https://drive.google.com/file/d/1IW48L7jIRSWVtdibaAAunu0FJ2NZLF2t/view?usp=sharing)
10. С помощью кнопки Add Component -> New Script в объект Sphere был добавлен скрипт CheckCollider со следующей функцией:
```csharp
   private void OnTriggerEnter(Collider other) {
        Debug.Log("Произошло столкновение с" + other.gameObject.name);
    }
```
Функция проверяет, было ли пересечение объекта с другим, и указывает это в консоли с названием объекта, с которым было пересечение. [pic 6](https://drive.google.com/file/d/1TkrGM93zCbgRvGa_Z8ukv-87ouX3iYld/view?usp=sharing)
11. Для этой задачи скрипт был модифицирован:
```csharp
private void OnTriggerEnter(Collider other) {
        Debug.Log("Произошло столкновение с " + other.gameObject.name);
        other.GetComponent<Renderer>().material.SetColor("_Color", Color.green);
    }
    
    private void OnTriggerExit(Collider other) {
        Debug.Log("Завершено столкновение с " + other.gameObject.name);
        other.GetComponent<Renderer>().material.SetColor("_Color", Color.red);
    }
```
При столкновении скрипт получает объект материала и устанавливает соответствующее значение цвета. [pic 7](https://drive.google.com/file/d/1Wdn4IqLU_I2BkrAbmgNWSMiT5msZvsiy/view?usp=sharing) [pic 8](https://drive.google.com/file/d/1HL-6SQRZULvYDV8-oy6Xt9fS0Jap31Dn/view?usp=sharing)



## Задание 2
### Продемонстрировать 
а) что произойдёт с координатами объекта, если он перестанет быть дочерним
б) три примера работы компонента Rigidbody.

Ход работы (задание А):
1. Создадим ещё два объекта куба, назовём их CubeParent и CubeChildren соответственно. Родительский объект будет синего цвета, координаты для него по умолчанию выставлены в 1;1;1 [pic 9](https://drive.google.com/file/d/1IFFdAgwFwLdOCQArwBFEQMdqMYRJ26SU/view?usp=sharing)
2. Стоит обратить внимание, что т.к. объект CubeChildren является дочерним, то и все его координаты выстраиваются **относительно родительского**, поэтому, пока он находится в подчинении, его координаты отличаются от базовых. Так, по Оси Y координата равна 0, поскольку, несмотря на то, что объект оторван от плоскости, относительно родительского объекта он остаётся на той же высоте. [pic 10](https://drive.google.com/file/d/1iCyBcFhsmtDb9ELh7VB6vw2ytgtYX-nN/view?usp=sharing) Также, стоит отметить, что вместе с перемещением базового объекта перемещаются и все дочерние, а дочерние могут перемещаться независимо, но с сохранением относительности координат.
3. Если убрать объект из подчинения, то его система координат станет такой же, как и у объекта CubeParent, т.е пропадёт относительность от объекта. [pic 11](https://drive.google.com/file/d/1xp9j5XmX4yu2Kv-HnfWfmypNavdOv9pY/view?usp=sharing)

Ход работы (задание Б):
1. В качестве демонстрации примеров я воспроизвёл ситуацию из сопровождающего видео, где сфера падает на куб, в ней можно выделить три т.н. примера работы:

а) Падение физического тела за счёт ускорения свободного падения

б) Коллизия (столкновение) с другим физическим объектом, которая, в свою очередь, может служить триггером в игровой логике (тот же пример со сменой цветов)

в) Применение силы к объекту (сфера толкает куб)
Демонстрация приложена в [гиф](https://drive.google.com/file/d/19JY25GIaC8a2pgPHPn3y3Tk0Oro-D6VL/view?usp=sharing)

Функция определения коллизии: 
```csharp
private void OnCollisionEnter(Collision other) {
        if (other.gameObject.name == "Cube"){
        other.gameObject.GetComponent<Renderer>().material.SetColor("_Color", Color.blue);
        }
    }

    private void OnCollisionExit(Collision other) {
        if (other.gameObject.name == "Cube"){
          other.gameObject.GetComponent<Renderer>().material.SetColor("_Color", Color.grey);
        }
    }
```

## Задание 3
### Сгенерировать n кубов по запросу пользователя

Ход работы:
1. В начале я сделал объект, который будет генерироваться - куб. Так как он будет появляться уже после запуска сцены, я его сделал в виде префаба - своеобразного шаблона объекта с заранее заданными параметрами. Сделал я его через меню Create -> Prefab. Так же, как в одном из предыдущих заданий, с помощью перетаскивания из списка объектов я сделал пустой префаб кубом синего цвета, после чего я оставил на сцене только плоскость, на которой будут размещаться кубы
2. Затем я добавил объект InputField (GameObject -> UI -> Legacy -> Input Field), в который пользователь потом будет вписывать число кубов, которое он хочет.
3. Затем я создал пустой GameObject и назвал его GenerationManager. Ему я добавил одноимённый скрипт. В нём прописаны объект, который будет сгенерирован и поле для ввода, откуда нудно брать информацию. Их нужно будет впоследствии указать в инспекторе объекта. [pic 12](https://drive.google.com/file/d/1O06kaccTzXpwGv1MVX8c756Sg-yeVmjL/view?usp=sharing)
```csharp
using UnityEngine;
using UnityEngine.UI;

public class GenerationManager : MonoBehaviour
{
    public GameObject objectToGenerate;
    public InputField inputField;


   public void Generate()
    {
    string text = inputField.text;
    int value = int.Parse(text);

     for (var i = 0; i < value; i++)
        {
            Instantiate(objectToGenerate, new Vector3(i * 2.0f, 0, 0), Quaternion.identity);
        }
    }
}

```
Сама функция Generate работает следующим образом:
  1. Парсит значение строки в число;
  2. Совершает цикл, в котором на каждой итерации создаёт объект, указанный в инспекторе, с помощью метода Instantiate с шагом в 2 по оси X, чтобы созданные объекты не наслаивались друг на друга.

Функция срабатывает по триггеру OnEndEdit() в инспекторе текстового поля, т.е. при смене фокуса с текстового поля на любой другой элемент.
Всё это выглядит следующим образом: [gif](https://drive.google.com/file/d/1sf2aX91yz3bGZX1kcDmlFg0ESdOOraJz/view?usp=sharing)





## Выводы

В ходе работы я вспомнил основы Unity и кодовую базу связки Unity и C#. Попробовал создавать объекты в редакторе и коде, посмотрел основные методы взаимодействия физических тел, изучил принципы эффективной работы в редакторе.

