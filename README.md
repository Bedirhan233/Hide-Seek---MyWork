# Hide-Seek---MyWork

![menuPNG](https://github.com/Bedirhan233/Hide-Seek---MyWork/assets/114574131/9ad23036-2024-45e8-aafd-b56209dfb5c5)
![HSMenu3](https://github.com/Bedirhan233/Hide-Seek---MyWork/assets/114574131/53235927-d982-451f-aa95-0f1093b4148e)


## About Game
Hide and Seek is a simple mobile game where Player 1 hides and Player 2 searches for him. Player 1 can place blocks and bombs on the map to stop Player 2, but he also has to watch out for water pools that leave footsteps behind. Player 2 must find Player 1 and kill him with his axe. He can destroy blocks but needs to avoid bombs, as getting too close to them will cause him to lose the game.

![GIF 2024-04-20 18-10-15](https://github.com/Bedirhan233/Hide-Seek---MyWork/assets/114574131/c69f72c3-c221-4c9d-922e-0978d79036f0) ![GIF 2024-04-20 19-27-12](https://github.com/Bedirhan233/Hide-Seek---MyWork/assets/114574131/c573d1ea-9bb0-4c05-be8c-e4c76ea6defc)



## FOV
I started by watching some videos and tried to create a field of view logic, but I wasn't satisfied with the result. So, I developed my own FOV system. It's a basic system where I send a raycast to the target, and if the raycast hits the target, it becomes visible. If something else hits the ray, the target becomes invisible.

<details>
  <summary>Click to expand</summary>
  
```csharp
private void CheckTargetObjectRaycast(GameObject target)
{
    directionTarget = target.transform.position - eyes.transform.position;
    directionTarget.Normalize();
    distanceToTarget = Vector2.Distance(eyes.transform.position, target.transform.position);

    if (sendRaycast)
    {
        RaycastHit2D hitObject = Physics2D.Raycast(eyes.transform.position, directionTarget, distanceToTarget, blockadingToSeeObject);

        if (hitObject.collider != null)
        {
            if (hitObject.collider != null)
            {
                for (int i = 0; i < objectsThatBlocksTag.Length; i++)
                {
                    if (hitObject.collider.CompareTag(objectsThatBlocksTag[i]))
                    {
                        hitBlock = true;
                        canSeePlayer = false;
                        target.GetComponent<HideObjectFOV>().canSeeTheObject = false;
                    }
                }
            }
            else
            {
                hitBlock = false;
            }

        }
        else
        {
            target.GetComponent<HideObjectFOV>().canSeeTheObject = true;
        }
    }
    else
    {
        target.GetComponent<HideObjectFOV>().canSeeTheObject = false;
    }
}


```
</details>


![GIF 2024-04-20 18-03-06](https://github.com/Bedirhan233/Hide-Seek---MyWork/assets/114574131/07e56703-2ef3-4c20-ba0d-19618d96af11)
## Load Data

The saving and loading positions and blocks was the most fun part of the project! To load data from firebase I used objectPool system. At the start of every match where its first players turn I instantiate all bombs and blocks into a object pool and set them to true or false after first players behviour. After I save and load I just instantiate the same amount that have been saved in database. 
<details>
  <summary>Click to expand</summary>
  
```csharp
    private void LoadGameDataActions()
    {
        BoyActions.Instance.BoyIsFreezed();

        boy.position = new Vector3(gameInfo.hidePos.x, gameInfo.hidePos.y);

        if(totalLoaded > 0) { return; }
        //load blocks
        for (int i = 0; i < gameInfo.blocks.Count; i++)
        {
            GameObject blockTmp = Instantiate(gamePlayManager.block, gamePlayManager.objectPool.transform);
            blockPool.Add(blockTmp);
            blockPool[i].transform.position = gameInfo.blocks[i];
            blockPool[i].SetActive(true);
        }

        //load bombs
        for (int i = 0; i < gameInfo.bombs.Count; i++)
        {
            GameObject bombTmp = Instantiate(gamePlayManager.bomb, gamePlayManager.objectPool.transform);
            bombPool.Add(bombTmp);
            bombPool[i].transform.position = gameInfo.bombs[i];
            bombPool[i].SetActive(true);
        }


        for (int i = 0; i < gameInfo.footStepPos.Count; i++)
        {
            GameObject footStep = Instantiate(gamePlayManager.footSteps, gamePlayManager.footStepPool.transform);
            footStepList.Add(footStep);
            footStepList[i].transform.position = gameInfo.footStepPos[i];

            Debug.Log("Loop chck");
        }
        for (int i = 0; i < gameInfo.footStepRot.Count; i++)
        {
            footStepList[i].transform.rotation = gameInfo.footStepRot[i];

        }
        totalLoaded++;


    }


