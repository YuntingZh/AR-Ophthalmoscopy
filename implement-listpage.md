# Implement ListPage

create a scroll view for the buttons

Attach this ListController on Viewport

```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;


public class ListController : MonoBehaviour
{

	//public Sprite[] ConditionPic;
	public GameObject ContentPanel;
	public GameObject ListItemPrefab;

	ArrayList listOfConditions;
	public datafile dataFile;
	void Start()
	{
		
		// use foreach loop, fetch the data and instantiate items 
		foreach(worddata words in dataFile.alldatas)
        {
			//listOfConditions.Add(words);
			GameObject newList = Instantiate(ListItemPrefab, ContentPanel.transform) as GameObject;
			newList.GetComponent<ListItemController>().updateMyData(words.Name,words.ID,words.Des);
		}

	}
}

```

Use those to create a List for all the eye conditions

```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

[CreateAssetMenu (fileName ="List",menuName ="createConditionList",order=1)]

public class datafile : ScriptableObject
{
    public List<worddata> alldatas;   //alldatas for linked list
}

```

```
using System;
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
[Serializable]

public class worddata
{
    public int ID;
    public string Name;
    public string Des;
    public Sprite Pics;
    public Sprite eyeTexture;
    //public string[] seachwords;
}

```

Create a Panel called "Popup" attach this popupEnable.cs to it

```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class popupEnable : MonoBehaviour
{
    public GameObject popupPanel;
    //[SerializeField] GameObject QuestionPopup;
    public GameObject QuestionPopup;
    public GameObject reportBTN;
    public GameObject nextBTN;
    public GameObject EyePositionJoystick;
    public GameObject HumanPositionSlider;
    // This script should be attach to Popup and the popupPanel is the child under Popup called"PopuoPanel"
    public void openPopup()
    {
       
            popupPanel.SetActive(true);
       
    }
    public void closePopup()
    {

        popupPanel.SetActive(false);

    }
    public void OpenOrClose()
    {
        //QuestionPopup = GameObject.Find("QuestionPanel");
     

        if (QuestionPopup.activeSelf == true)
        {
            QuestionPopup.SetActive(false);
            Debug.Log("found the obj" + QuestionPopup);
        }

       else
        {
            QuestionPopup.SetActive(true);

        }
    }
    public void ShowReport()
    {
        reportBTN.SetActive(true);
        nextBTN.SetActive(false);
        Debug.Log("call showreport" );

    }
    public void EyePositionOpenOrClose()
    {
        if (EyePositionJoystick.activeSelf == true)
        {
            EyePositionJoystick.SetActive(false);
        } else
        {
            EyePositionJoystick.SetActive(true);
        }
    }
    public void HumanPositionOpenOrClose()
    {
        if (HumanPositionSlider.activeSelf == true)
        {
            HumanPositionSlider.SetActive(false);
        }
        else
        {
            HumanPositionSlider.SetActive(true);
        }
    }
}

```

make a SceneChanger to switch between scenes and also pass ID&#x20;

```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;
using UnityEngine.SceneManagement;
using System;


public class SceneChanger : MonoBehaviour
{
    public datafile file;
    public static int textureID;

    //public static int RandomID;
    public void ChangeScene()
    {
        SceneManager.LoadScene("simpleAR");
    }
    public void LearnARScene()
    {
        SceneManager.LoadScene("LearnAR");

    }
    public void jumpback()
    {

        SceneManager.LoadScene("UI");

    }
  
}
```

add "grid layout group" on content

under content, create a button for instantiation, and attach the ListItemController to it

