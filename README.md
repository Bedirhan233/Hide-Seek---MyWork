# Hide-Seek---MyWork


## About Game
Hide and Seek is a simple mobile game where the player 1 hides and player 2 search for him. Player 1 can put blocks and bomb in the map to stop the player 2 but he also have to be aware of water pool that makes him leave footsteps behind him. Player 2 have to find the player 1 and kill him with his weapon. He can destroy the blocks but needs to avoid bombs or he will die and lose. 

![GIF 2024-04-20 18-10-15](https://github.com/Bedirhan233/Hide-Seek---MyWork/assets/114574131/c69f72c3-c221-4c9d-922e-0978d79036f0) ![GIF 2024-04-20 19-27-12](https://github.com/Bedirhan233/Hide-Seek---MyWork/assets/114574131/c573d1ea-9bb0-4c05-be8c-e4c76ea6defc)



## FOV
I started to watch some videos and tried to make a field of view logic but I was not happy with the result so I made my own fov system. Its a basic system where I send a raycast to target and if the raycast can hit the target the target will be visible, if it block something else in the way the target will be invisible. 

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


