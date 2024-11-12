# Heist Master
Software Engineering - Product Șutdaun


## Analysis
+ ### Product Vision
  To immerse players in the ultimate heist experience, combining strategic planning and high-stakes action, where every difficulty is a thrilling and dynamic adventure.
+ ### Requirements, User Personas, Features
  These are included in the GDD(Game Design Document) file from the [Documentation](./Documentation) folder.

## Planning
+ ### Backlog
  For our backlog, we used Jira. To access it, click [here](https://raducordunianu02.atlassian.net/jira/software/projects/SCRUM/boards/1) and you have to log in and have permission to our project.

## Design
+ ### Diagrams
  - UML Diagram
      ```mermaid
          classDiagram
              class Interactable {
                  <<abstract>>
                  - useEvents: bool
                  - promptMessage: string
                  *+ Interact()*: void
                  + BaseInteract(): void
              }
      
              class KeyPad {
                  - objectToInteractWith: GameObject
                  + Interact(): void
              }
      
              class EventOnlyInteractable { }
      
              class InteractableEditor {
                  - sampleObject: EventOnlyInteractable
                  + OnInspectorGUI(): void
              }
      
              class InteractionEvent { 
                  - OnInteract: UnityEvent 
              }
              
              Interactable <|-- EventOnlyInteractable : extends
              EventOnlyInteractable --* InteractableEditor
              Interactable <|-- KeyPad : extends
      
              class Map {
                  - npcSpawner : NPCSpawner
              }
      
              class NPCSpawner {
                  - listOfNPCs : List~NPC~
                  - npcCounter : int
                  + Spawn() : void
              }
      
              class NPC {
                  - stateMacine : StateMachine
                  - agent : NavMeshAgent
                  - currentState : string
                  - path : PathAI
                  + Agent : (NavMeshAgent: Get;)
              }
      
              class PathAI {
                  - waypoints : List~Transform~
                  - alwaysDrawPath : bool
                  - drawAsLoop : bool
                  - drawNumbers : bool
                  - debugColour : Color
                  + OnDrawGizmos() : void
                  + DrawPath() : void
                  + OnDrawGizmosSelected() : void
              }
      
              class StateMachine {
                  - activeState : BaseState
                  + Initialize() : void
                  + Update() : void
                  + ChangeState(BaseState newState) : void
              }
      
              class BaseState {
                  <<abstract>>
                  - enemy : enemy
                  - stateMachine : StateMachine
                  *+ Enter()* : void
                  *+ Perform()* : void
                  *+ Exit()* : void
              }
      
              class PatrolState {
                  - waypointIndex : int
                  - waitTimer : float
                  + Enter() : void
                  + Perform() : void
                  + Exit() : void
                  + PartolCycle() : void
              }
      
              class SearchState {
                  - searchTimer : float
                  - movingTimer : float
                  + Enter() : void
                  + Perform() : void
                  + Exit() : void
              }
      
              class AttackState {
                  - movingTimer : float
                  - losingPlayerTimer : float
                  - shotTimer : float
                  + Enter() : void
                  + Perform() : void
                  + Exit() : void
                  + Shoot() : void
                  + Start() : void
                  + Update() : void
              }
      
              class NPCNonEnemy {
                  - followPattern : FollowPattern
              }
      
              class FollowPattern {
                  + Start() : void
                  + Update() : void
              }
      
              class NPCEnemy {
                  - enemy : Enemy
              }
      
              class Enemy {
                  - agent : NavMeshAgent
                  - debugSphere : GameObject
                  - player : GameObject
                  - Player : (GameObject: Get;)
                  - lastKnownPos : Vector3
                  - LaskKnownPos : (Vector3: Get; Set;)
                  - distanceForSight : float
                  - fieldOfVieew : float
                  - eyeHeight : float
                  - gunBarrel : Transform
                  - fireRate: float
                  + Start() : void
                  + SeePlayer() : bool
                  + Update() : void
              }
      
              Map --* NPCSpawner
              NPCSpawner --* NPC
              StateMachine --* BaseState
              BaseState <|-- AttackState : extends
              BaseState <|-- SearchState : extends
              BaseState <|-- PatrolState : extends
              NPC <|-- NPCEnemy : extends
              NPCEnemy --* Enemy
              NPC <|-- NPCNonEnemy : extends
              NPCNonEnemy --* FollowPattern
              NPC --* StateMachine
              NPC --* PathAI
      
      
              class GameManager {
                  - instance : GameManager
                  - isStealth : bool
                  - player : Player
                  - guiManager : GUIManager
                  - sceneManager : SceneManager
                  - musicManager : MusicManager
                  - audioManager : AudioManager
                  - saveProgressManager : SaveProgressManager
                  + Awake() : void
              }
          
              class AudioManager {
                  - instance : AudioManager
                  - audioSourcePrefab : GameObject
                  - timeToSwitch : float
                  - audioSourceCount : int
                  - audioSources : List~AudioSource~
                  - volume : float
                  + Awake() : void
                  + Start() : void
                  + Init() : void
                  + Play(AudioClip audioClip) : void
                  + GetFreeAudioSource() : AudioSource
              }
          
              class MusicManager {
                  - instance : MusicManager
                  - audioSource : AudioSource
                  - timeToSwitch : float
                  - playOnStart : AudioClip
                  - volume : float
                  - switchTo : AudioClip
                  + Awake() : void
                  + Start() : void
                  + Play(AudioClip musicToPlay, bool interrupt) : void
                  + SmoothSwitchMusic() : IEnumerator
              }
          
              class GameData {
                  - health : float
                  - playerPosition : float[]
                  - isStealth : bool
                  - currentScene : string
                  + GameData(GameData data) : Constructor
              }
          
              class SaveProgressManager {
                  - savedGames : List~GameData~
                  + SaveGame(GameData dataToSave) : void
                  + LoadGame() : GameData
              }
      
              class ScreenTint {
                  - untitledColor : Color
                  - tintedColor : Color
                  - image : Image
                  - speed : float
                  + Awake() : void
                  + Tint() : void
                  + Untint() : void
                  + TintScreen() : IEnumerator
                  + UntintScreen() : IEnumerator
              }
              
              class SceneManager {
                  - instance : SceneManager
                  - screenTint : ScreenTint
                  - cameraConfiner : CameraConfiner
                  - currentScene : string
                  - unload : AsyncOperation
                  - load : AsyncOperation
                  + Awake() : void
                  + Start() : void
                  + InitSwitchScene(string sceneTo, Vector3 targetPosition) : void
                  + Transition(string sceneTo, Vector3 targetPosition) : IEnumerator
                  + SwitchScene(string sceneTo, Vector3 targetPosition) : void
              }
          
              class GUIManager {
                  - instance : GUIManager
                  - isVisible : bool
                  + SaveGame() : void
                  + LoadGame() : void
                  + ChangeSensitivity(float input) : void
                  + ChangeAudioVolume(float input) : void
                  + ChangeMusicVolume(float input) : void
              }
      
              GUIManager --* GameManager
              AudioManager --* GameManager
              MusicManager --* GameManager
              ScreenTint --* SceneManager
              SceneManager --* GameManager
              GameData --* SaveProgressManager
              SaveProgressManager --* GameManager
          
              class Player {
                  - CharacterController : Unity Component
                  - inputManager : InputManager
                  - playerMotor : PlayerMotor
                  - playerLook : PlayerLook
                  - playerHealth : PlayerHealth
                  - audioController : AudioController
              }
          
              class PlayerLook {
                  - cam: Camera
                  - xRotation: float
                  - xSensitivity : float
                  - ySensitivity : float
                  + ProcessLook(Vector2 input) : void
              }
          
              class InputManager {
                  - playerInput : PlayerInput
                  - onFoot : PlayerInput.OnFootActions
                  - motor : PlayerMotor
                  - look : PlayerLook
                  + Awake() : void
                  + FixedUpdate() : void
                  + LateUpdate() : void
                  + OnEnable() : void
                  + OnDisable() : void
              }
          
              class PlayerMotor {
                  - controller : CharacterController
                  - playerVelocity : Vector3
                  - speed : float
                  - isGrounded : bool
                  - jumpHeight : float
                  - gravity : float
                  - isCrouching : bool
                  - lerpCrouch : bool
                  - crouchTimer : float
                  - isSprinting : bool
                  + Start() : void
                  + Update() : void
                  + ProcessMove(Vector2 input) : void
                  + Jump() : void
                  + Crouch() : void
                  + Sprint() : void
              }
          
              class PlayerHealth {
                  - health : float
                  - lerpTimer : float
                  - maxHealth : float
                  - chipSpeed : float
                  - frontHealthBar : Image
                  - backHealthBar : Image
                  - overlay : Image
                  - duration : float
                  - fadeSpeed : float
                  - durationTimer : float
                  + Start() : void
                  + Update() : void
                  + UpdateHealthUI() : void
                  + TakeDamage(float damage) : void
                  + RestoreHealth(float healAmount) : void
              }
      
              class PlayerUI {
                  - promptText : TextMeshProUGUI
                  + Start() : void
                  + UpdateText(string promptMessage) : void
              }
      
              class PlayerInteract {
                  - cam : Camera
                  - interactDistance : float
                  - mask : LayerMask
                  - playerUI : PlayerUI
                  - inputManager : InputManager
                  - soundsMade : List~AudioClip~
                  - audioController : AudioController
                  + Start() : void
                  + Update() : void
              }
          
              class AudioController {
                  + Play(AudioClip sound) : void
              }
      
              GameManager *-- Player
              Player *-- PlayerLook
              Player *-- InputManager
              Player *-- PlayerMotor
              Player *-- AudioController
              Player *-- PlayerHealth
              Player *-- PlayerInteract
              Player *-- PlayerUI
      ```


## Source Code
The code for project can be found [here](https://github.com/TaviF24/Game).
