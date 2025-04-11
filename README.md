# RAYCASTION
>[!Important]
> Dupla: Daniel Carvalho Da Silva & Nicollas Lemes.
> Turma: Terceiro Jogos.
>

Nesse projeto abordamos o "Physics.Raycast" e seus funcionamentos a partir da emulação de treino de mira (AimLab)

- O que é o Physics.RayCast?

O RayCast é um elemento de física dentro do motor gráfico da unity responsável por criar um raio com colisão, esse raio em princípio é invisível e não possui função alguma além de ser lançado, no entanto, de acordo a como for programado, o raio pode colidir objetos e destrui-los,  criar novos objetos ou simplesmente enviar uma mensagem de resposta de colisão. Também é importante mencionar de que é possível "desenhar" o raio para que ele seja visível para o programador ou usuário do programa ao qual foi inserido 

- Como abordamos?

Pegando o serviço de treino de mira conhecido como AimLab, fizermos uma simulação um pouco mais básica de como é o seu funcionamento, adicionamos um prefab de movimentação, um de alvos a serem acertados pelo jogador. Com isso em mente configuramos a área de spawn destes alvos para que após eles serem destruídos pelo raio lançado pelo jogador, eles reapareçam em cordenadas diferentes de acordo com o limite imposto para que os alvos fiquem sempre visíveis para o player.

- O que cada um fez?

Fomos equilibrados, Nicollas se manteve atento, auxiliando na programação e também na utilização de ideias para a nossa simulação. Daniel foi quem fez a parte dos scripts, Nicollas pegou os assets e auxiliou na criação dos scripts. 
Ambos trabalharam, sendo o GitHub organizado por ambos enquanto em sala, o que explica a quantidade de commits, enquanto outros foram arrumados em casa.

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

RayCastion:

Utilizamos de uma programação simples que cria o raio (ray) após o clique do mouse. Com isso, no início para identificar caso acertamos, criamos a classe "Mandar", essa identificando se o raio acertou e guardando o seu dado, sendo essas identificações:

- Ponto de acerto.
- Distância.
- olisão com tag.
- Colisão com GameObject.

Indo adiante, criamos uma classe "SpawnNextSphere" junto de um simples script para a criação do nosso objeto (Sphere). Adicionamos uma classe publica de GameObject, utilizando os vetores 3D e também o comando "Random.Range, criamos uma área na qual o objeto pode spawnar. logo após instâncias sua posição e usamos o Quaternion para arrumar sua rotação.

Para que houvesse destruição do Objeto, apenas fizemos uma série de "if" que identifica sua tag, botão do mouse e puxa a classe "SpawNextSphere" para destruir o GameObject e criar outro em seguida.




```csharp

### Movimento & Crosshair

**public class FirstPersonController : MonoBehaviour
{
    private Rigidbody rb;

    #region Camera Movement Variables

    public Camera playerCamera;

    public float fov = 60f;
    public bool invertCamera = false;
    public bool cameraCanMove = true;
    public float mouseSensitivity = 2f;
    public float maxLookAngle = 50f;

    // Crosshair
    public bool lockCursor = true;
    public bool crosshair = true;
    public Sprite crosshairImage;
    public Color crosshairColor = Color.white;

    // Internal Variables
    private float yaw = 0.0f;
    private float pitch = 0.0f;
    private Image crosshairObject;

    #region Camera Zoom Variables

    public bool enableZoom = true;
    public bool holdToZoom = false;
    public KeyCode zoomKey = KeyCode.Mouse1;
    public float zoomFOV = 30f;
    public float zoomStepTime = 5f;

    // Internal Variables
    private bool isZoomed = false;

    #endregion
    #endregion

    #region Movement Variables

    public bool playerCanMove = true;
    public float walkSpeed = 5f;
    public float maxVelocityChange = 10f;

    // Internal Variables
    private bool isWalking = false;

    #region Sprint

    public bool enableSprint = true;
    public bool unlimitedSprint = false;
    public KeyCode sprintKey = KeyCode.LeftShift;
    public float sprintSpeed = 7f;
    public float sprintDuration = 5f;
    public float sprintCooldown = .5f;
    public float sprintFOV = 80f;
    public float sprintFOVStepTime = 10f;

    // Sprint Bar
    public bool useSprintBar = true;
    public bool hideBarWhenFull = true;
    public Image sprintBarBG;
    public Image sprintBar;
    public float sprintBarWidthPercent = .3f;
    public float sprintBarHeightPercent = .015f;

    // Internal Variables
    private CanvasGroup sprintBarCG;
    private bool isSprinting = false;
    private float sprintRemaining;
    private float sprintBarWidth;
    private float sprintBarHeight;
    private bool isSprintCooldown = false;
    private float sprintCooldownReset;

    #endregion

    #region Jump

    public bool enableJump = true;
    public KeyCode jumpKey = KeyCode.Space;
    public float jumpPower = 5f;

    // Internal Variables
    private bool isGrounded = false;

    #endregion

    #region Crouch

    public bool enableCrouch = true;
    public bool holdToCrouch = true;
    public KeyCode crouchKey = KeyCode.LeftControl;
    public float crouchHeight = .75f;
    public float speedReduction = .5f;

    // Internal Variables
    private bool isCrouched = false;
    private Vector3 originalScale;

    #endregion
    #endregion

    #region Head Bob

    public bool enableHeadBob = true;
    public Transform joint;
    public float bobSpeed = 10f;
    public Vector3 bobAmount = new Vector3(.15f, .05f, 0f);

    // Internal Variables
    private Vector3 jointOriginalPos;
    private float timer = 0;

    #endregion

    private void Awake()
    {
        rb = GetComponent<Rigidbody>();

        crosshairObject = GetComponentInChildren<Image>();

        // Set internal variables
        playerCamera.fieldOfView = fov;
        originalScale = transform.localScale;
        jointOriginalPos = joint.localPosition;

        if (!unlimitedSprint)
        {
            sprintRemaining = sprintDuration;
            sprintCooldownReset = sprintCooldown;
        }
    }

    void Start()
    {
        if(lockCursor)
        {
            Cursor.lockState = CursorLockMode.Locked;
        }

        if(crosshair)
        {
            crosshairObject.sprite = crosshairImage;
            crosshairObject.color = crosshairColor;
        }
        else
        {
            crosshairObject.gameObject.SetActive(false);
        }

        #region Sprint Bar

        sprintBarCG = GetComponentInChildren<CanvasGroup>();

        if(useSprintBar)
        {
            sprintBarBG.gameObject.SetActive(true);
            sprintBar.gameObject.SetActive(true);

            float screenWidth = Screen.width;
            float screenHeight = Screen.height;

            sprintBarWidth = screenWidth * sprintBarWidthPercent;
            sprintBarHeight = screenHeight * sprintBarHeightPercent;

            sprintBarBG.rectTransform.sizeDelta = new Vector3(sprintBarWidth, sprintBarHeight, 0f);
            sprintBar.rectTransform.sizeDelta = new Vector3(sprintBarWidth - 2, sprintBarHeight - 2, 0f);

            if(hideBarWhenFull)
            {
                sprintBarCG.alpha = 0;
            }
        }
        else
        {
            sprintBarBG.gameObject.SetActive(false);
            sprintBar.gameObject.SetActive(false);
        }

        #endregion
    }

    float camRotation;

    private void Update()
    {
        #region Camera

        // Control camera movement
        if(cameraCanMove)
        {
            yaw = transform.localEulerAngles.y + Input.GetAxis("Mouse X") * mouseSensitivity;

            if (!invertCamera)
            {
                pitch -= mouseSensitivity * Input.GetAxis("Mouse Y");
            }
            else
            {
                // Inverted Y
                pitch += mouseSensitivity * Input.GetAxis("Mouse Y");
            }

            // Clamp pitch between lookAngle
            pitch = Mathf.Clamp(pitch, -maxLookAngle, maxLookAngle);

            transform.localEulerAngles = new Vector3(0, yaw, 0);
            playerCamera.transform.localEulerAngles = new Vector3(pitch, 0, 0);
        }

        #region Camera Zoom

        if (enableZoom)
        {
            // Changes isZoomed when key is pressed
            // Behavior for toogle zoom
            if(Input.GetKeyDown(zoomKey) && !holdToZoom && !isSprinting)
            {
                if (!isZoomed)
                {
                    isZoomed = true;
                }
                else
                {
                    isZoomed = false;
                }
            }

            // Changes isZoomed when key is pressed
            // Behavior for hold to zoom
            if(holdToZoom && !isSprinting)
            {
                if(Input.GetKeyDown(zoomKey))
                {
                    isZoomed = true;
                }
                else if(Input.GetKeyUp(zoomKey))
                {
                    isZoomed = false;
                }
            }

            // Lerps camera.fieldOfView to allow for a smooth transistion
            if(isZoomed)
            {
                playerCamera.fieldOfView = Mathf.Lerp(playerCamera.fieldOfView, zoomFOV, zoomStepTime * Time.deltaTime);
            }
            else if(!isZoomed && !isSprinting)
            {
                playerCamera.fieldOfView = Mathf.Lerp(playerCamera.fieldOfView, fov, zoomStepTime * Time.deltaTime);
            }
        }

        #endregion
        #endregion

        #region Sprint

        if(enableSprint)
        {
            if(isSprinting)
            {
                isZoomed = false;
                playerCamera.fieldOfView = Mathf.Lerp(playerCamera.fieldOfView, sprintFOV, sprintFOVStepTime * Time.deltaTime);

                // Drain sprint remaining while sprinting
                if(!unlimitedSprint)
                {
                    sprintRemaining -= 1 * Time.deltaTime;
                    if (sprintRemaining <= 0)
                    {
                        isSprinting = false;
                        isSprintCooldown = true;
                    }
                }
            }
            else
            {
                // Regain sprint while not sprinting
                sprintRemaining = Mathf.Clamp(sprintRemaining += 1 * Time.deltaTime, 0, sprintDuration);
            }

            // Handles sprint cooldown 
            // When sprint remaining == 0 stops sprint ability until hitting cooldown
            if(isSprintCooldown)
            {
                sprintCooldown -= 1 * Time.deltaTime;
                if (sprintCooldown <= 0)
                {
                    isSprintCooldown = false;
                }
            }
            else
            {
                sprintCooldown = sprintCooldownReset;
            }

            // Handles sprintBar 
            if(useSprintBar && !unlimitedSprint)
            {
                float sprintRemainingPercent = sprintRemaining / sprintDuration;
                sprintBar.transform.localScale = new Vector3(sprintRemainingPercent, 1f, 1f);
            }
        }

        #endregion

        #region Jump

        // Gets input and calls jump method
        if(enableJump && Input.GetKeyDown(jumpKey) && isGrounded)
        {
            Jump();
        }

        #endregion

        #region Crouch

        if (enableCrouch)
        {
            if(Input.GetKeyDown(crouchKey) && !holdToCrouch)
            {
                Crouch();
            }
            
            if(Input.GetKeyDown(crouchKey) && holdToCrouch)
            {
                isCrouched = false;
                Crouch();
            }
            else if(Input.GetKeyUp(crouchKey) && holdToCrouch)
            {
                isCrouched = true;
                Crouch();
            }
        }

        #endregion

        CheckGround();

        if(enableHeadBob)
        {
            HeadBob();
        }
    }

    void FixedUpdate()
    {
        #region Movement

        if (playerCanMove)
        {
            // Calculate how fast we should be moving
            Vector3 targetVelocity = new Vector3(Input.GetAxis("Horizontal"), 0, Input.GetAxis("Vertical"));

            // Checks if player is walking and isGrounded
            // Will allow head bob
            if (targetVelocity.x != 0 || targetVelocity.z != 0 && isGrounded)
            {
                isWalking = true;
            }
            else
            {
                isWalking = false;
            }

            // All movement calculations shile sprint is active
            if (enableSprint && Input.GetKey(sprintKey) && sprintRemaining > 0f && !isSprintCooldown)
            {
                targetVelocity = transform.TransformDirection(targetVelocity) * sprintSpeed;

                // Apply a force that attempts to reach our target velocity
                Vector3 velocity = rb.linearVelocity;
                Vector3 velocityChange = (targetVelocity - velocity);
                velocityChange.x = Mathf.Clamp(velocityChange.x, -maxVelocityChange, maxVelocityChange);
                velocityChange.z = Mathf.Clamp(velocityChange.z, -maxVelocityChange, maxVelocityChange);
                velocityChange.y = 0;

                // Player is only moving when valocity change != 0
                // Makes sure fov change only happens during movement
                if (velocityChange.x != 0 || velocityChange.z != 0)
                {
                    isSprinting = true;

                    if (isCrouched)
                    {
                        Crouch();
                    }

                    if (hideBarWhenFull && !unlimitedSprint)
                    {
                        sprintBarCG.alpha += 5 * Time.deltaTime;
                    }
                }

                rb.AddForce(velocityChange, ForceMode.VelocityChange);
            }
            // All movement calculations while walking
            else
            {
                isSprinting = false;

                if (hideBarWhenFull && sprintRemaining == sprintDuration)
                {
                    sprintBarCG.alpha -= 3 * Time.deltaTime;
                }

                targetVelocity = transform.TransformDirection(targetVelocity) * walkSpeed;

                // Apply a force that attempts to reach our target velocity
                Vector3 velocity = rb.linearVelocity;
                Vector3 velocityChange = (targetVelocity - velocity);
                velocityChange.x = Mathf.Clamp(velocityChange.x, -maxVelocityChange, maxVelocityChange);
                velocityChange.z = Mathf.Clamp(velocityChange.z, -maxVelocityChange, maxVelocityChange);
                velocityChange.y = 0;

                rb.AddForce(velocityChange, ForceMode.VelocityChange);
            }
        }

        #endregion
    }

    // Sets isGrounded based on a raycast sent straigth down from the player object
    private void CheckGround()
    {
        Vector3 origin = new Vector3(transform.position.x, transform.position.y - (transform.localScale.y * .5f), transform.position.z);
        Vector3 direction = transform.TransformDirection(Vector3.down);
        float distance = .75f;

        if (Physics.Raycast(origin, direction, out RaycastHit hit, distance))
        {
            Debug.DrawRay(origin, direction * distance, Color.red);
            isGrounded = true;
        }
        else
        {
            isGrounded = false;
        }
    }

    private void Jump()
    {
        // Adds force to the player rigidbody to jump
        if (isGrounded)
        {
            rb.AddForce(0f, jumpPower, 0f, ForceMode.Impulse);
            isGrounded = false;
        }

        // When crouched and using toggle system, will uncrouch for a jump
        if(isCrouched && !holdToCrouch)
        {
            Crouch();
        }
    }

    private void Crouch()
    {
        // Stands player up to full height
        // Brings walkSpeed back up to original speed
        if(isCrouched)
        {
            transform.localScale = new Vector3(originalScale.x, originalScale.y, originalScale.z);
            walkSpeed /= speedReduction;

            isCrouched = false;
        }
        // Crouches player down to set height
        // Reduces walkSpeed
        else
        {
            transform.localScale = new Vector3(originalScale.x, crouchHeight, originalScale.z);
            walkSpeed *= speedReduction;

            isCrouched = true;
        }
    }

    private void HeadBob()
    {
        if(isWalking)
        {
            // Calculates HeadBob speed during sprint
            if(isSprinting)
            {
                timer += Time.deltaTime * (bobSpeed + sprintSpeed);
            }
            // Calculates HeadBob speed during crouched movement
            else if (isCrouched)
            {
                timer += Time.deltaTime * (bobSpeed * speedReduction);
            }
            // Calculates HeadBob speed during walking
            else
            {
                timer += Time.deltaTime * bobSpeed;
            }
            // Applies HeadBob movement
            joint.localPosition = new Vector3(jointOriginalPos.x + Mathf.Sin(timer) * bobAmount.x, jointOriginalPos.y + Mathf.Sin(timer) * bobAmount.y, jointOriginalPos.z + Mathf.Sin(timer) * bobAmount.z);
        }
        else
        {
            // Resets when play stops moving
            timer = 0;
            joint.localPosition = new Vector3(Mathf.Lerp(joint.localPosition.x, jointOriginalPos.x, Time.deltaTime * bobSpeed), Mathf.Lerp(joint.localPosition.y, jointOriginalPos.y, Time.deltaTime * bobSpeed), Mathf.Lerp(joint.localPosition.z, jointOriginalPos.z, Time.deltaTime * bobSpeed));
        }
    }
}

```


Usando um script do próprio Unity, obtivemos o movimento e também uma mira para o jogador saber onde está acertando.

O Script é separado em uma série de Classes, sendo elas:

- Camera: que utiliza do “FOV” (Field of view), Crosshair (Mira), e diminuição de FOV quando o personagem está correndo, para dar sensação de movimento acelerado.

- Movement: Todos os fatores de movimentação, com movimentos básicos dos eixos, junto de Pulo, agachamento, corrida e barra de cansaço.

Primeiramente o script pega variáveis para o fov, scale e posição das juntas, e detecta caso o Sprint esteja cheio.
Iniciando, usamos um comando de lockCursor para o mouse ser preso na tela, adicionando a nossa crosshair com um gameObject de sprite. Configuramos a barra de cansaço, colocando variáveis para o seu tamanho na tela e seu funcionamento conforme o player corre. Aproveitamos para partir em direção à configuração da câmera, criamos uma variável que permite um Zoom caso o player aperte o botão direito do mouse ou que ele esteja correndo. Utilizando Mathf.Lerp para interpolar as variáveis, caso o player esteja correndo e o isZoomed = false, a câmera vai se aproximar sem a necessidade do botão direito, que volta caso a barra de corrida acabe ou o player decida parar de correr. Melhorando as possibilidades de movimento, criamos a possibilidade do pulo e do agachamento, criamos mais uma variável que verifica se o player está pisando em algo e também o quanto de velocidade ele tem, impedindo que a velocidade aumente caso aperte os botões para andar verticalmente. Para a possibilidade da corrida, iremos usar o método Mathf.Clamp que adiciona uma força necessária até que o player esteja correndo em sua velocidade máxima, além de verificar a sua velocidade enquanto está agachado, correndo ou parado, isso, utilizando o rb.AddForce para identificar a força usada para essa mudança de velocidade.
A classe CheckGround utiliza um raycast para baixo, este identificando o objeto que está abaixo do player e envia uma mensagem dizendo se o player está ou não pisando no chão, que ativa o script para o player ficar 0.75f acima desse objeto, dando a impressão de estar pisando. 
As classes Jump e Crounch são para o pulo do personagem e sua agachada, sendo usados somente enquanto o player pula e usam o rigidybody do player para aplicar força no pula, adicionando o termo AddForce, o que também possibilita identificar as mudanças na velocidade do player em ambas possibilidades. 
Obs: Caso o player esteja agachado, se o player pular, ele primeiro se levanta e depois pula, isso com o método if(isCrounched && !holdToCrouch).

Finalizamos com a classe HeadBob, que calcula a mudança da câmera em seu movimento, permitindo criar uma sensação de balançar na cabeça, para isso usando as juntas, que permitem o movimento da câmera se tornar mais suave.


