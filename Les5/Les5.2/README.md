# Les5.2 - Week 5 -  (CODE) Particles , Sounds & Shakes
## Beschrijving 
Ik heb kennis gemaakt met particle effecten in Unity en hoe moet ik geluiden toevoegen.
## Wat heb ik gemaakt
Ik heb een Particle System toegevoegd als component in Unity.
## Demo 
![Les5.2](5.2.gif)
## Code (Bumper hit)
``` Csharp
using System;
using UnityEngine;

public class BumperHit : MonoBehaviour
{
    [SerializeField] private int bumperValue = 50;

    private ParticleSystem ps;

    public static event Action<Transform, int> onBumperHit;
    private void Start()
    {
        ps = GetComponent<ParticleSystem>();

        ps?.Stop();
    }
    private void OnCollisionEnter2D(Collision2D collision)
    {
        if (collision.gameObject.CompareTag("Ball"))
        {

            onBumperHit?.Invoke(gameObject.transform, bumperValue);

            ps?.Stop();

            ps?.Play();
        }
    }
}

```
## Code 2 (Combo System)
``` Csharp
using System;
using System.Collections.Generic;
using UnityEngine;

public class ComboSystem : MonoBehaviour
{
    public static event Action<int, int> OnScoreChange;

    private List<string> bumperTags = new List<string>();
    private int scoreMultiplier = 1;
    private void Start()
    {
        BumperHit.onBumperHit += CheckForCombo;
    }
    private void OnDisable()
    {
        BumperHit.onBumperHit -= CheckForCombo;
    }
    
    private void CheckForCombo(Transform bumperTransform, int bumperValue)
    {
        string tag = bumperTransform.tag;
        bumperTags.Add(tag);
        if (bumperTags.Count > 1)
        {
            if (bumperTags[bumperTags.Count - 2] == bumperTags[bumperTags.Count - 1])
            {
                scoreMultiplier++;
            }
            else
            {
                scoreMultiplier = 1;
                bumperTags.Clear();
                
            }
        }
        ScoreManager.Instance.AddScore(bumperValue * scoreMultiplier);
        
        OnScoreChange?.Invoke(ScoreManager.Instance.GetScore(), scoreMultiplier);

    }
}
```