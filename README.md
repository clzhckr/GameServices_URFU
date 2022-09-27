# РАЗРАБОТКА ИГРОВЫХ СЕРВИСОВ
Отчет по лабораторной работе #1 выполнил(а):
- Кыров Валентин Сергеевич
- 1922524
Отметка о выполнении заданий (заполняется студентом):

| Задание | Выполнение | Баллы |
| ------ | ------ | ------ |
| Задание 1 | # | 60 |
| Задание 2 | # | 20 |
| Задание 3 | # | 20 |

знак "*" - задание выполнено; знак "#" - задание не выполнено;

Работу проверили:
- к.т.н., доцент Денисов Д.В.
- к.э.н., доцент Панов М.А.
- ст. преп., Фадеев В.О.


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
Ознакомиться с основными функциями Unity и взаимодействием с объектами внутри редактора.

## Задание 1
### В разделе «ход работы» пошагово выполнить каждый пункт с описанием и примера реализации задач по теме видео «Практическая работа 1-4».
Ход работы:
1. Был создан новый проект 3D-Core под названием GameServices, в дальнейшем вся работа будет проходить в этом проекте
2. В дальнейшем в качестве редактора кода был указан VSCode. [pic 1](https://drive.google.com/file/d/14WnskIyDgGCBZCJIN7sxEGTqSo85K3uU/view?usp=sharing)
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
### 

## Задание 3
### 

```



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
