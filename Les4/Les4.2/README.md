# Les4.2 - Week 4 - (CODE) Peggle Game, Waardes (score) versturen naar de UI
## Beschrijving
Ik heb mijn score en de waarde van mijn combo multiplier naar de UI gestuurd.
## Wat heb ik gemaakt.
De kern van mijn project is de succesvolle implementatie van het Observer-patroon in Unity: Game-logica (ComboSystem, CountBalls) verstuurt Action Events die de UI-elementen (UIScoreBoard, UIBallCounter) updaten, en tevens de spelmechaniek (Shoot) besturen door een schietlimiet af te dwingen.
## Wat heb ik geleerd.
Ik heb geleerd hoe ik Action Events (Observer-patroon) effectief kan inzetten om logica en UI los te koppelen, wat resulteert in een schone en uitbreidbare architectuur.
## Demo
![Les4.2](Les4.2.gif)
## Code ( UIBallCounter)
``` Csharp
using UnityEngine;
using TMPro; 

public class UIBallCounter : MonoBehaviour
{
    
    public TMP_Text ballCountField;

    private void Start()
    {
        
        CountBalls.onBallCountChanged += UpdateBallCountUI;
    }

    private void OnDisable()
    {
        CountBalls.onBallCountChanged -= UpdateBallCountUI;
    }

    private void UpdateBallCountUI(int ballsRemaining)
    {
      
        ballCountField.text = $"Ballen: {ballsRemaining} / 5";
    }
}

```
## Code (CountBalls)
``` Scharp
using System;
using UnityEngine;

public class CountBalls : MonoBehaviour
{
    public static event Action onBallLost;
    public static event Action onBallsDepleted;

    public static event Action<int> onBallCountChanged;

    [SerializeField] private int ballsLeft = 5;
    
    private void Start()
    {
        Shoot.onShootBall += CountOnShot;

        onBallCountChanged?.Invoke(ballsLeft);
    }

    private void OnDisable()
    {
        Shoot.onShootBall -= CountOnShot;
    }
    private void CountOnShot()
    {
        if(ballsLeft > 0)
        {
            ballsLeft--;

            onBallCountChanged?.Invoke(ballsLeft);
        }
        else
        {
            onBallsDepleted?.Invoke();
        }
    }
    private void OnTriggerExit2D(Collider2D collision)
    {
        if (collision.gameObject.CompareTag("Ball"))
        {
            onBallLost?.Invoke();
            Destroy(collision.gameObject);
        }
    }

}

```
## Code (UIScoreBoard)
``` Scharp
using TMPro;
using UnityEngine;

public class UIScoreBoard : MonoBehaviour
{
    public TMP_Text scoreField;
    public TMP_Text multiplierField;
    
    private void Start()
    {
        ComboSystem.OnScoreChange += UpdateUI;
    }
    private void OnDisable()
    {
        ComboSystem.OnScoreChange -= UpdateUI;
    }
    private void UpdateUI(int score, int multiplier)
    {
        scoreField.text = "Score: " + score;
        multiplierField.text = "Multiplier: " + multiplier + "X";
    }
}

```
## Code (UIBallCounter )
``` Scharp
using UnityEngine;
using TMPro; 

public class UIBallCounter : MonoBehaviour
{
    
    public TMP_Text ballCountField;

    private void Start()
    {
        
        CountBalls.onBallCountChanged += UpdateBallCountUI;
    }

    private void OnDisable()
    {
        CountBalls.onBallCountChanged -= UpdateBallCountUI;
    }

    private void UpdateBallCountUI(int ballsRemaining)
    {
      
        ballCountField.text = $"Ballen: {ballsRemaining} / 5";
    }
}

```
