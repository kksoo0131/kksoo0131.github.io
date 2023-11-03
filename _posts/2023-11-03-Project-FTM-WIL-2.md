---
layout: post
title:  "팀 프로젝트 FTM 2주차 회고노트"
date:   2023-11-03
excerpt: "Project FTM WIL"
tag:
- WIL
- 내일배움캠프
- FosterTheMonster
comments: true
---

![nbcbanner](/assets/img/TILbanner.png)

1. ResourceManager

2. DataManager

3. CardRarity

4. CardEffect

5. CardSystem Refactoring

6. Enhancement Deck
<br/>
<br/>


## ResourceManager 

`사용한 기술 스택 : 싱글톤 패턴, 지연로드 패턴, 제네릭, Dictionary`

Resources.Load()를 사용하는 부분에서 어떤식으로 사용할지에 대해서 여러가지 고민을 했다.

프로젝트를 목표대로 완성하게 된다면 몬스터의 종류와 카드의 종류가 100개 이상이 될텐데 기존에 해온 미니 프로잭트들 처럼

모든 리소스를 Load 시켜두고 사용하면 메모리를 너무 많이 사용하게 된다라는 팀원들의 의견이 있었다.

<br/>

그래서 이 부분을 개선하기 위해서 자료를 찾아보다가 `지연 로딩 lazy loading`의 방식을 사용하기로 결정했다.

필요한 상황에 리소스를 로드하고 씬 전환이 발생하면 모든 리소스를 Resources.UnloadUnusedAssets()해서 메모리에서 비워주기로 했다.

<br/>

리소스 매니저 에서는 같은 리소스를 중복해서 로드하는 것을 막기 위해서 로드한 리소스들을 Dictionary로 관리하고

리소스 매니저 에게 어떤 리소스를 요청했을때 해당 리소스가 Dictionary에 존재하면 그냥 return하고 없다면 새로 Resources.Load하여

Dictionary에 추가한 후 return 해주도록 만들어 주었다.

<br/>

또 리소스를 호출하기 위해서는 해당 리소스의 정보가 필요하기 떄문에 리소스를 사용하는 측에 List<string>을 만들어서 운용했는데

리소스를 사용할 매니저들이 생성될 때 모든 리소스를 한번 불러와서 리소스를 직접 저장하지는 않고 리소스에 접근하기 위한

이름들만 저장한 뒤 실제 사용할때 List<string>에 있는 정보를 통해서 리소스를 지연 로드 방식으로 사용하도록 했다.

```cs
using System.Collections.Generic;
using UnityEngine;

public class ResourceManager
{
    private static ResourceManager _instance;
    public static ResourceManager Instance { get { return _instance ?? (_instance = new ResourceManager()); } }

    Dictionary<System.Type, Dictionary<string, Object>> dic = new Dictionary<System.Type, Dictionary<string, Object>>();

    public T LoadPrefab<T> (string path, string name) where T : Object
    {
        System.Type type = typeof(T);
        if (!dic.ContainsKey(type))
        {
            dic.Add(type, new Dictionary<string, Object>());
        }

        if (!dic[type].ContainsKey(name))
        {
            dic[type].Add(name, Resources.Load<T>(path));
        }

        return (T)dic[type][name];
    }

    public List<T> LoadPrefabAll<T>(string path) where T : Object
    {
        List<T> returnList = new List<T>();
        var resources = Resources.LoadAll(path);

        foreach (T resource in resources)
        {
            returnList.Add(resource);
        }
        return returnList;
    }

    public void UnLoadAllResource()
    {
        foreach(Dictionary<string, Object> innerDic in dic.Values) 
        {
            innerDic.Clear();
        }
        Resources.UnloadUnusedAssets();
    }


    public void UnLoadResource<T>(string name)
    {
        dic[typeof(T)].Remove(name);
        Resources.UnloadUnusedAssets();
    }


}

```


<br/>
<br/>

## DataManager

`사용한 기술 스택 : 싱글톤 패턴, List`
씬을 이동할 때 유지 되어야하는 Data들을 DataManager를 통해서 관리하기 위해 만들었다. 추후 저장관련 기능도 DataManager에서 처리할 예정이다.

저장이 필요한 데이터들은 DataManager의 데이터를 참조해서 사용하는 것으로 DataManager의 데이터들을 유지한다.

<br/>

주의 사항은 데이터들은 저장하지만 게임 오브젝트를 저장해주지는 않기 때문에 게임 오브젝트와 연결을해서 사용하는 데이터들의 경우에는

DataManager를 참조해서 게임 오브젝트를 새로 생성한 뒤 기존의 데이터로 초기화 해주는 부분이 필요하다.

즉, DataManager, GameObject, Data를 담은 Script가 동기화 되어야 한다.

```cs
using System;
using System.Collections.Generic;

[Serializable]
public class DataManager
{
   
    private static DataManager _instance;
    public static DataManager Instance { get { return _instance ?? (_instance = new DataManager()); } }

    public int Date { get; set; }
    public int CageCount { get; set; }
    public int DrawCardCount { get; set; }

    private Deck _deck;
    public Deck Deck { get { return _deck ?? (_deck = new Deck()); } } 
    public List<Cage> Cages { get; private set; }

    public List<MonsterData> CatchSceneMonsters { get; private set; }
    public List<MonsterData> CatchedMonsters { get; private set; }
    public List<MonsterData> EscapeMonsters { get; private set; }

    public HashSet<int> caughtMonsterIds;

    public void CatchMosnter(MonsterSO monster)
    {
        caughtMonsterIds.Add(monster.monsterID);
    }

    public bool HasCaughtMonster(int monsterID)
    {
        return caughtMonsterIds.Contains(monsterID);
    }


    DataManager()
    {
        Date = 0;
        CageCount = 0;
        DrawCardCount = 5;
        Cages = new List<Cage>();
        CatchSceneMonsters = new List<MonsterData>();
        CatchedMonsters = new List<MonsterData>();
        EscapeMonsters = new List<MonsterData>();
        caughtMonsterIds = new HashSet<int>();
    }
```

<br/>
<br/>


## CardRarity

카드의 레어도를 추가하고 카드의 데이터를 레어도 별로 나누어서 저장하게 변경했다.

또, 카드를 생성할 때 레어도 별로 다른 확률로 생성되도록 기능을 추가했다.

이때 Random이 아니라 RNGCryptoServiceProvider를 사용하여 패턴이 없는 난수를 생성하였다.

```cs

public class CardManager
{

    public void LoadToName()
    {
        for (int i = 0; i < (int)CardRarity.EndPoint; i++)
        {
            CardsDic.Add((CardRarity)i, new List<string>());
            LoadWithRarity((CardRarity)i);
        }
    }

    public void LoadWithRarity(CardRarity rarity)
    {
        var cards = ResourceManager.Instance.LoadPrefabAll<CardSO>($"{PATH}{rarity}");

        foreach(var so in cards) 
        {
            CardsDic[rarity].Add(so.name);
        }
    }

    public CardSO SelectCard()
    {
        CardRarity rarity = SelectRarity();
        int index = Math.Abs(RandomInt() % CardsDic[rarity].Count);
        string cardName = CardsDic[rarity][index];

        CardSO so = ResourceManager.Instance.LoadPrefab<CardSO>($"{PATH}{rarity}/{cardName}", cardName);
        return so;
    }

    public CardRarity SelectRarity()
    {
        CardRarity rarity;

        int date = DataManager.Instance.Date;
        int randomValue = Math.Abs(RandomInt() % 100);

        // 나중에 날짜별로 확률을 정해줄 필요가있음

        if (randomValue < 70 - date)
        {
            rarity = CardRarity.Normal;
        }
        else if (randomValue < 93 - date)
        {
            rarity = CardRarity.Rare;
        }
        else if (randomValue < 99 - date)
        {
            rarity = CardRarity.Epic;
        }
        else
        {
            rarity = CardRarity.Legend;
        }

        return rarity;
    }
}


```
<br/>
<br/>

## CardEffect

`사용한 기술 스택 : ScirptableObject, abstract`

카드의 효과부분을 CardSO에서 원하는 효과를 선택해서 장착할 수 있는 확장성을 위해

ScriptableObject를 사용해서 만들었고, 인스펙터 창을 통해 ScriptableObject에 할당할 수 있게 되었다.

<br/>

카드의 경우 대상이 Cage인 경우와 Monster인 경우로 나뉘게 되었는데 추상 클래스인 CardEffectSO에서 두 케이스에 대해 모두 추상 메서드로 선언 했다.

```cs
public abstract class CardEffect : ScriptableObject
{
    public abstract void ApplyEffectOnCage(Cage cage, int value);


    public abstract void ApplyEffectOnMonster(MonsterData monster, int value);
}

[CreateAssetMenu(fileName = "CardEffect", menuName = "DataSO/CardEffect/Temperature", order = 3)]
public class TemperatureAdjustment : CardEffect
{
    public override void ApplyEffectOnCage(Cage cage, int value)
    {
        cage.temperature += value;
    }

    public override void ApplyEffectOnMonster(MonsterData monster, int value)
    {

    }
}

[CreateAssetMenu(fileName = "CardEffect", menuName = "DataSO/CardEffect/Hunger", order = 2)]
public class HungerAdjustment : CardEffect
{
    public override void ApplyEffectOnCage(Cage cage, int value)
    {

    }

    public override void ApplyEffectOnMonster(MonsterData monster, int value)
    {
        // monster가 해당 특성을 좋아하는지 확인하고 좋아한다면 가중치를 받아와서 value에 곱해준다.
        monster.CurHunger -= value;
    }
}
```

그리고 실제 구현할 카드의 클래스에서는 필요하지 않은 메서드는 비워두어서 아무 기능을 하지 않도록 만들어 주었고

카드의 효과를 실제로 적용하는 부분을 추가했다.

```cs
public class CageManager
{
    public void UpdateCage()
    {
        foreach(Cage c in cages)
        {
            c.UseCard();
            c.UpdateMonster();
        }
    }
}

public class Cage
{
    public void UseCard()
    {
        int index = cards.Count;
        while (--index >= 0)
        {
            cards[index].UseCardOnCage(this);
            cards[index].UseCardOnMonster(Monster);
            RemoveCard();
        }
        cageInfoTxt.text = CageInfoText();
    }
}
```



<br/>
<br/>

## CardSystem Refactoring

카드 시스템 == 카드와 관련된 클래스들을 약간 리팩토링 했다.

카드의 움직임과 관련된 부분을 최대한 실제 게임 오브젝트에 컴포넌트로 붙혀줄 Monobehaviour를 상속받은 클래스로 이관 하였고

UI와 상호작용 하는 부분들에서 임시로 작성한 코드들을 수정했다.

<br/>
<br/>

## Enhancement Deck

덱에 여러가지 기능을 추가했다.

- 패를 버리는 기능
- 버린 패를 저장하는 기능
- 덱을 모두 사용한 경우 버려진 카드들을 셔플에서 다시 덱에 추가하는 기능
- 덱과 버려진카드를 확인할 수 있는 UI를 생성하는 기능

<br/>
<br/>

![nbcthumbnail](/assets/img/thumbnail-image.png)