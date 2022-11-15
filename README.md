# Learning Journal

## 04/10/2022

Started programming module. Created all the templates; Journal, Task Log and Tutorial.


## 11/10/2022

(tutorial 1)
First issue I came across was due to an autofill in visual studio without me noticing. As you can see in the images below, I inteded to write "Animation", and it changed it to the title of the script, which is "LightFlickerAnimation"

#The error



#The fix



Then trying to test it out, it was not working, the mistake was putting the script on both the main object and on the light effect. After deleting the script component on the light source.


#The error
![image](https://user-images.githubusercontent.com/91538305/197642562-29310895-3a60-4d09-bfe7-4c3f87ed54e2.png)



#The fix


After fixing the previous issue, I've noticed another missed detail. In the script for the animation, that is now attached to the main object only, had no Game Object selected for "Lamp Lights" which was a quick drag-and-drop fix.

#The error
![image](https://user-images.githubusercontent.com/91538305/197642536-57bd2154-3631-4215-bbc9-626f0cf1b1dd.png)


#The fix
![image]()

## 18/10/2022




## 25/10/2022

## 01/11/2022

## 08/11/2022



## 15/11/2022




ENEMY 




using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Enemy : MonoBehaviour
{
    public EnemyManager enemyManager;;
    private float enemyHealth = 2f;

    // Start is called before the first frame update
    void Start()
    {
        
    }

    // Update is called once per frame
    void Update()
    {
        if (enemyHealth <= 0)
        {
            enemyManager.RemoveEnemy(this);
            Destroy(gameObject);
        }
    }
    public void TakeDamage(float damage)
    {
        enemyHealth -= damage;
    }

}




ENEMYMANAGER




using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class EnemyManager : MonoBehaviour
{
    public List<Enemy> enemiesInTrigger = mew List<Enemy>();

    public void AddEnemy(Enemy enemy)
    {
        enemiesInTrigger.Add(enemy);
    }
 public void RemoveEnemy(Enemy enemy)
    {
        enemiesInTrigger.Remove(enemy);
    }


}




GUN





using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Gun : MonoBehaviour
{

//sett the range of the weapon
    public float range = 20f;
    public float verticalRange = 20f;
    public float fireRate;
    public float bigDamage = 2f;
    public float smallDamage = 1f;

    public float nextTimeToFire;
    private BoxCollider gunTrigger;

    public LayerMask raycastLayerMask;
    public EnemyManager enemyManager;

    void Start()
    {
        gunTrigger = GetComponent<BoxCollider>();
        gunTrigger.size = new Vector3(1, verticalRange, range);
        gunTrigger.center = new Vector3(0,0,range * 0.5f);

    }


    void Update()
    {
        if(Input.GetMouseButtonDown(0)&& Time.time > nextTimeToFire)
        {
            Fire();
        }
    }


    void Fire()
{
        //damage enemies
        foreach (var enemy in enemyManager.enemiesInTrigger)
        {
            //get direction to enemy
            var dir = enemy.transform.position - transform.position;

            RaycastHit hit;
            if (Physics.Raycast(transform.position, dir, out hit, range * 1.5f, raycastLayerMask))
            {
                if (hit.transform == enemy.transform)
                {
                    //range check
                    float dist = Vector3.Distance(enemy.transform.position, transform.position);
                    if (dist > range * 0.5f)
                    {
                        //damage enemy small
                        enemy.TakeDamage(smallDamage);
                    }
                    else
                    {
                        //damage enemy big
                        enemy.TakeDamage(bigDamage);
                    }

                    //Debug.DrawRay(transform.position, dir, Color.green);
                    //Debug.Break();
                }
            }

        }


    //reset timer
    nextTimeToFire = Time.time + fireRate;
}

    private void OnTriggerEnter(Collider other) 
{
     //add potential enemy to shoot
     Enemy enemy = other.transform.GetComponent<Enemy>();

     if (enemy)
     {
         //add here
         enemyManager.AddEnemy(enemy);
    }
 }
    private void OnTriggerExit(Collider other) 
{
        //remove enemy to shoot
    Enemy enemy = other.transform.GetComponent<Enemy>();
    if (enemy)
    {
        //remove here
        enemyManager.RemoveEnemy(enemy);
    }
}
}






































































