# РАЗРАБОТКА ИГРОВЫХ СЕРВИСОВ
Отчет по лабораторной работе #5 выполнил(а):
- Кыров Валентин Сергеевич
- 1922524
Отметка о выполнении заданий (заполняется студентом):

| Задание | Выполнение | Баллы |
| ------ | ---------- | ------ |
| Задание 1 | *          | 60 |
| Задание 2 | *          | 20 |
| Задание 3 | #          | 20 |

знак "*" - задание выполнено; знак "#" - задание не выполнено;

Работу проверили:
- к.т.н., доцент Денисов Д.В.
- к.э.н., доцент Панов М.А.
- ст. преп., Фадеев В.О.


## Цель работы
Cоздание интерактивного приложения с рейтинговой системой пользователя и интеграция игровых сервисов в готовое приложение.

## Задание 1
### Используя видео-материалы практических работ 1-5 повторить реализацию приведенного ниже функционала:
* Интеграции авторизации с помощью Яндекс SDK
* Сохранение данных пользователя на платформе Яндекс Игры
* Сбор данных об игроке и вывод их в интерфейсе
* Интеграция таблицы лидеров
* Интеграция системы достижений в проект

По сути, я повторил действия, указанные в видео. Были проблемы с системой достижений, данные оттуда я просто вынес на стартовый экран.  
 Если подробнее: для того, чтобы заносить данные в массив, объект списка достижений на Canvas должен был быть активен. В противном случае вызывается NullPointerException на функции CheckSDK,  из-за чего ломается всё взаимодействие с SDK.
Ниже скрипт авторизации `YSAuth.cs` с примыкающей информацией вроде рекорда в игре:
```csharp
using UnityEngine.UI;
using UnityEngine;
using YG;

public class YGAuth : MonoBehaviour
{
    private void OnEnable() => YandexGame.GetDataEvent += CheckSDK;
    private void OnDisable() => YandexGame.GetDataEvent -= CheckSDK;
    private Text bestScore;
    private string[] achievements;

    void Start()
    {
        if (YandexGame.SDKEnabled == true)
        {
            CheckSDK();    
        }
    }
    public void CheckSDK()
    {     
        if (YandexGame.auth == true)
        {
            Debug.Log("Authorized");
        }
        else
        {
            Debug.Log("Authorization Error");
            YandexGame.AuthDialog();
        } 
        GameObject ScoreB = GameObject.Find("BestScore");
        bestScore = ScoreB.GetComponent<Text>();
        bestScore.text = "Рекорд: " + YandexGame.savesData.bestScore.ToString();
        if ((YandexGame.savesData.achievements)[0]==null &!GameObject.Find("AchievementsList"))
        {
        }
        else
        {
            foreach (string value in YandexGame.savesData.achievements)
            {
                GameObject.Find("AchievementsList").GetComponent<Text>().text = GameObject.Find("AchievementsList").GetComponent<Text>().text + value + "\n";
            }
        }
    }
}
```

Все переменные, используемые в качестве трансферных, лежат в файле `SavesYG.cs`
```csharp
namespace YG
{
    [System.Serializable]
    public class SavesYG
    {
        public bool isFirstSession = true;
        public string language = "ru";
        public bool feedbackDone;
        public bool promptDone;

        // Ваши сохранения
       public int score;
       public int bestScore;
      public string[] achievements;
    }
}
```
Операции в геймплее осуществляются в рамках скрипта `HighCount.cs`
```csharp
...
      if (high>10000) //отметка высоты, на которой для персонажа заканчивается игра
       {
      Time.timeScale = 0f;
            FinishPanel.SetActive(true); // включение интерфейса окончания игры
            FinishScore.text = Score.text;
            string[] achievelist = {"smth"};
            achievelist[0] = "Достижение 1";
            SaveData(int.Parse(FinishScore.text), YandexGame.savesData.bestScore, achievelist); //сохранение результата и ачивки
            YandexGame.NewLeaderboardScores ("TopPlayers", int.Parse(FinishScore.text)); //добавление результата в лидерборд
           
       }
    }
    public void SaveData(int currentScore, int aBestScore)
    {
        YandexGame.savesData.score = currentScore;
        if (currentScore > aBestScore)
        {
            YandexGame.savesData.bestScore = currentScore;
        };
        YandexGame.savesData.achievements = currentAchieve;
        YandexGame.SaveProgress(); //сохранение изменений
}
```
![](Overview)

## Задание 2
### Описать не менее трех дополнительных функций Яндекс SDK, которые могут быть интегрированы в игру. 

1. Оценка игры. Можно настроить непосредственно внутри игры, а также редиректом на страницу игры на сервисе, и оценить уже там
2. Сброс прогресса. Если игрок желает сбросить ачивки/рекорды/покупки, это также можно сделать методами Яндекс SDK
3. Монетизация. Можно монетизировать игру различными способами, такими как рекламные баннеры, видеореклама, а также продажа внутриигровой валюты.


## Выводы
На этой практической я познакомился с основными функциями Яндекс SDK и начал налаживать их интеграцию в свой игровой проект.

