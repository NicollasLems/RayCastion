# RAYCASTION

Nesse projeto abordamos o "Physics.Raycast" e seus funcionamentos a partir da emulação de treino de mira (AimLab)

- O que é o Physics.RayCast?

O RayCast é um elemento de física dentro do motor gráfico da unity responsável por criar um raio com colisão, esse raio em princípio é invisível e não possui função alguma além de ser lançado, no entanto, de acordo a como for programado, o raio pode colidir objetos e destrui-los,  criar novos objetos ou simplesmente enviar uma mensagem de resposta de colisão. Também é importante mencionar de que é possível "desenhar" o raio para que ele seja visível para o programador ou usuário do programa ao qual foi inserido 

- Como abordamos?

Pegando o serviço de treino de mira conhecido como AimLab, fizermos uma simulação um pouco mais básica de como é o seu funcionamento, adicionamos um prefab de movimentação, um de alvos a serem acertados pelo jogador. Com isso em mente configuramos a área de spawn destes alvos para que após eles serem destruídos pelo raio lançado pelo jogador, eles reapareçam em cordenadas diferentes de acordo com o limite imposto para que os alvos fiquem sempre visíveis para o player.

## DRIVE DO VIDEO

https://drive.google.com/file/d/1IyXH9gnzdSso4eKswWMx2XKaY6peBvUf/view?usp=drive_link

## DRIVE DO JOGO

https://drive.google.com/file/d/1q9BRfHDpC1jtFVdEZmCSTSUUzFSGL3xw/view?usp=drive_link


# Explicação Do Jogo

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
