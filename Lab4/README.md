# РАЗРАБОТКА ИГРОВЫХ СЕРВИСОВ
Отчет по лабораторной работе #4 выполнил(а):
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
Подготовить разрабатываемое интерактивное приложение к сборке и публикации.

## Задание 1
### Используя видео-материалы практических работ 1-5 повторить реализацию приведенного ниже функционала:
+ Создание анимации объектов на сцене
+ Создание стартовой сцены и переключение между ними
+ Доработка меню и функционала с остановкой игры
+ Добавление звукового сопровождения в игре
+ Добавление персонажа и сборка сцены для публикации на web-ресурсе»


Ход работы:  
1. В качестве анимируемого элемента я решил использовать препятствия, которые появляются на пути персонажа. Механизм их появления описан в предыдущей лабораторной работе в скрипте `Spawner.cs`. Так, после появления объекты двигались вверх, но оставались статичным по горизонтальной оси. Я решил исправить это и добавить движение по горизонтали. 
```csharp
using UnityEngine;

public class Motion : MonoBehaviour
{
    public float speedy = 3.0f;
    public float speedx = 10.0f; // стартовая скорость перемещения в пространстве
    public Vector2 diry;
    public Vector2 dirx; //отдельные вектора, чтобы корректно срабатывал коллайдер

    private void FixedUpdate()
    {      
      transform.Translate(speedy * diry * Time.fixedDeltaTime, Space.World);

      transform.Translate(speedx * dirx * Time.fixedDeltaTime, Space.World); //перемещение по направлению
    }
 
 private void OnCollisionEnter2D (Collision2D other){
 GameObject collide = other.gameObject;
 if (collide.name == "RightBorder" )
 {
    speedx = Random.Range(-6,-10);
 }      
  GameObject collide2 = other.gameObject;
 if (collide2.name == "LeftBorder" )
 {
      speedx = Random.Range(6,10); 
    // при столкновении с границей устанавливается рандомная скорость в обратном направлении чтобы избежать "гармошки из препятствий"  
 }    
}
}
```
Благодаря этому игра становится гораздо интереснее, так как определить паттерн поведения препятствий теперь практически невозможно.  
2. В игре ещё до лабораторной я сделал стартовую сцену - меню с переходом в меню опций (настраивается методом SetActive для другой панели Canvas'a) ![](https://github.com/clzhckr/GameServices_URFU/blob/main/Lab4/Media/MenuOverview.png)  
3. Также, было сделано меню паузы внутри игры (там тоже активируется другая панель по нажатию кнопки паузы кнопкой мыши, а также параметр Time.timeScale становится равным 0, а при выходе из паузы равным 1) ![](https://github.com/clzhckr/GameServices_URFU/blob/main/Lab4/Media/PauseOverview.png)  
4. В качестве фоновой музыки я использовал [бесплатный ассет](https://assetstore.unity.com/packages/audio/music/electronic/electronic-music-songpack-214055) из AssetStore. На камеру в каждой сцене я добавил Audio Source и по одному треку в качестве фоновой музыки.
![](https://github.com/clzhckr/GameServices_URFU/blob/main/Lab4/Media/InGameMusic.png)  
5. Также в игре у меня уже был персонаж, у него проигрывается анимация падения, когда он меняет свою координату по Y. Баг (фича?) - из-за специфики игры персонаж проигрывает эту анимацию постоянно..
###  Bonus
В игре была жёсткая привязка камеры к положению персонажа, из-за чего создавалось ощущение, что он не перемещается в пространстве. Я решил это исправить, заменив самописный скрипт контроллера камеры на скрипт из Unity CineMachine - "продвинутой камеры" от разработчиков движка. Теперь камера плавно следит за движением персонажа и в принципе более отзывчива. ![](https://github.com/clzhckr/GameServices_URFU/blob/main/Lab4/Media/NewCamera.gif)


## Задание 2
### Привести описание того, как происходит сборка проекта проекта под другие платформы. Какие могут быть особенности? 
Сборка для ПК/UWP наиболее простая: достаточно выбрать платформу и собрать проект. Также, под эту платформу наиболее удобно проставлять уровни качества графики (да и в принципе не ограничивать себя по этой части). По умолчанию целевой платформой является Windows, для Mac и Linux нужно докачивать модули.  
Сборка для мобильных устройств чуть сложнее, т.к. требует посредника: среду разработки (Android Studio и XCode, догружается через Unity Hub), в которых происходит докомпиляция проекта. В старых версиях движка проверить можно было только на устройстве/в эмуляторе IDE, в новых версиях появилась симуляция геймплея на мобильном устройстве прямо в Unity. Также, для мобильных устройств необходимо поменять схему управления на управление для тачскрина.  
В случае с Android также могут быть проблемы, связанные с размером экрана устройства, т.к из-за большой фрагментации устройств какие-то элементы интерфейса могут не помещаться.
Для сборки под консоли Microsoft и Sony необходимо иметь доступ к платформам разработчиков ID@Xbox и PlayStation Partners соответственно.
## Задание 3
### Добавить в меню Option возможность изменения громкости (от 0 до 100%) фоновой музыки в игре.
Для данной функции я написал простой скрипт:
```csharp
using UnityEngine;

public class VolumeChanger : MonoBehaviour
{
    private AudioSource source;
    private float volumelevel = 1f;
    void Start()
    {
        source = GetComponent<AudioSource>();
    }
    void Update()
    {
        source.volume = volumelevel;
    }
    public void SetVolume (float volume)
    {   
            volumelevel = volume;
    }
}

```
За аудио в сцене отвечает объект MusicBox. К нему добавлен AudioSource и написанный скрипт.
Фунцкия SetVolume вызывается при изменении положения слайдера, добавленного на Canvas.
Данный функционал реализован как в меню, так и в самой игре в меню паузы.
![](https://github.com/clzhckr/GameServices_URFU/blob/main/Lab4/Media/VolumeChanger.png)




## Выводы

В ходе работы я ознакомился с новыми методами работы с камерой, с тем, как работает аудио в движке, посмотрел возможности постпроцессинга (до введения новой камеры я хотел использовать эффект motion blur, но он так и не заработал), а также изучил различные варианты реализации движения объектов в пространстве в Unity.

