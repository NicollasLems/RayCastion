# RayCastion

## DRIVE DO VIDEO

https://drive.google.com/file/d/1IyXH9gnzdSso4eKswWMx2XKaY6peBvUf/view?usp=drive_link

## DRIVE DO JOGO

https://drive.google.com/file/d/1q9BRfHDpC1jtFVdEZmCSTSUUzFSGL3xw/view?usp=drive_link



![image](https://github.com/user-attachments/assets/7c3c5c13-9383-4766-9ed7-b8fc1f831c03)




```csharp

using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class RayCastion : MonoBehaviour
{
    Ray ray;
    RaycastHit hitData;
    Vector3 point;
    Color color;
    public GameObject spherePrefab;

    // Start is called once before the first execution of Update after the MonoBehaviour is created
    void Start()
    {
        
    }

    // Update is called once per frame
    void Update()
    {
        
        if (UnityEngine.Input.GetKey(KeyCode.Mouse0))
        {
            ray = Camera.main.ScreenPointToRay(Input.mousePosition);
            /*ray = new Ray(transform.position, transform.forward);*/
            color = Color.cyan;
            Mandar(ray, color, 1);

            if (hitData.collider != null) 
            {
                if (hitData.collider.tag == "bloquinho")
                {
                    if (Input.GetMouseButtonDown(0))
                    {
                        SpawnNextSphere();
                    }
                }
            }
        }

    }

    private void Mandar(Ray ray, Color color, int tipo)
    {
        if(Physics.Raycast(ray, out hitData))
        {
            Vector3 hitPosition = hitData.point;
            float hitDistance = hitData.distance;
            string tag = hitData.collider.tag;
            GameObject hitObject = hitData.transform.gameObject;

            if(tag == "bloquinho")
            {
                Debug.Log("Acertouuu");
            }
            else
            {
                Debug.Log("Errouuu!!");
            }
        }
    }
    public void SpawnNextSphere()
    {
        Vector3 randomSpawnPosition = new Vector3(Random.Range(-10, 11), Random.Range(1, 4), Random.Range(1, 1));
        Instantiate(spherePrefab, randomSpawnPosition, Quaternion.identity);
   
        Destroy(hitData.transform.gameObject);
      

    }
      
}


```
