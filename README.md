# -Programming-Learning-Journal-
## 28/09/21
# Day 1 of programming, introduction.
Started the lesson talking about programming as a whole and what's expected of us this semester in terms of course work. Also introduced to Github, and how we're going to be writing on our journey, and essentially updating our journal using Github. Being able to understand how it works as well as how to work repositories and sort out work as we go along. Learning as well that it's a collaberative space, but while I know what it is I still have a lot to learn regarding what and how to work with Github more in due time.
Did some excersises through Games that Paul set us, and essentially tried one that tested our memories and collaberative work with another person to mimic how us as users input data into a computer through a programming language. 

We were also told that we'll be working on learning tutorials and then updating our journal accordingly with how, we followed certain steps and whatnot, as if we're teaching someone else so that it could be in our minds. Starting next week. 

We also learned a bit about the scientific method which is in the name, A hypothesis that essentially we try to prove right but instead prove wrong, by trying various possibilities to find something wrong, now it's possible to get infinite correct answers so the lesson here, is that we take what we can and run with it. If it works in a solid manner it works and if it doesn't then it doesn't. In terms of code, learnt some basic things such as what {} do, and what && mean as well. 

So overall a general good beginning and introduction to programming in our second year and I'm looking forward to what's next. 

## 5/10/21
# Day 2 week 3 
# Learning to do third person movement, and camera control in a 3D space 
-Setup a 3D space within Unity, then generate a terrain for some space. 
-Change the terrain X and Z axis to -500 to center the Terrain. 
-Create a player character.
-Go to the heirachy, create a 3D object, in a cylinder for our player
-Reset its Transform position so that it's centered. 
-To create an indication of what direction we're facing, we're going to make a cube and flatten it down so that it shows us what way is forward.
-Now we have a character, remove the box collider on the Cube so that there's no collision issues. 
-Make a parent controller and place it inside the cylinder in the Heirach, reset its position so that it's centered. 
-Then drag out the Empty Game Object and place the cylinder in the Game Object and rename the class anything, so that it's easier to distinguish what we're working with in the heirachy. 
-To have some good looking camera movement we'll be using Unity's Cinemachine plugin to help with advanced camera movement to follow our character. 
-If you can't see it then go to Window, and Package Manager and search Cinemachine, now install it. 
-Select Cinemachine and create a Freelook camera. 
-Rename it to ThirdPersonCam for convienience sake. One thing to note is that our Main Camera and Third Person Camera are completely different objects in the heirachy. 
-Select the Third person Camera object and where it says "Follow" and "Look at" Drag and drop the player object there, now it should follow the player wherever they end up moving along our terrain. 
-Problem encountered was that the camera ended up inside the player, creating a 1st person view, the reason for this was because the scale of the player was too big, meaning the values would have collided with it. So make sure the scale should be 1 on the X and 1 on the Y.
-We have mouse movement thanks to Cinemachine but because the camera is very zoomed in, we need to change that by going into Orbits in the Third Person Camera section. 
-You can see three Orbits or sections on the player that tell the camera what to target and where, we have three rings to indicate this, one at the top, middle and bottom of the object the camera is focusing on. 
-Change the values of the orbits to something that has a wider range allowing for a better look at the player.
-In this sense we want the player to move with infulence of the camera. 
-Go to Axis Control to fix the camera's Y axis so that the camera doesn't fall through the floor, double check the bottomrig's height. 
-By going into Axis control you can change the keys to have the camera be controlled by something else such as WASD or IJKL for example. 
-If you want to mess around with the sensitivity then you can change the speed on the X axis. 
-In the game view play around with the gridded lines to get a feel of what kind of camera angle you may want. 

# Movement 
Now it's time for Movement.
-First select the player then add a new component and search for Character controller. What this does is it helps us determine what it is we want to move.
-Now Add component again and add a script, name it something like ThirdPersonMovment so we know what it's for, this Script we'll be writing on our own. 
-Double click and be sure to open up Visual Studio as that's where the code will be written. 
-Remove the start function and first things first we need a reference to our character controller component that we added before.



public CharacterController controller;
    public float speed = 6f;
    
    Line one is our reference to our character controller 
    And Line two is a reference to our character's movement speed, essentially how fast we want it to move which we could always change later in the script. 
   
-Here our main goal is to get movement along the Horizontal and Vertical axis, so we'd write:
     float horizontal = Input.GetAxisRaw("Horizontal");
        float vertical = Input.GetAxisRaw("Vertical");
        These two lines tell us that we want to have our character move in the Horizontal and Vertical axis, of the scene. 
- The horizontal axis goes between -1 and 1 which will happen if we were to press the A or the Left Arrow Key but it'd be +1 or 1 if we press the D or right arrow. 
- Same principle applies here with vertical with W and S or Up and Down. 
-Next we're going to define our direction and store it in Vectors
Vector3 direction = new Vector3(horizontal, 0f, vertical);
Vector3 helps us store the direction we then tell the script to focus on not wanting to move our character on the  Y axis. Otherwise it'll float. 
if we do .normalised it means that we don't run the risk of our character going at a crazy speed if we want diagonal movement. 


-Next we need to write an if statement. 
 if (direction.magnitude>=0.1f)
        {
            controller.Move(direction * speed * Time.deltaTime);
        }
        
-Line one helps to define that if we're moving then this is how we're moving 
Line two is defining our speed so that we have framerate independent meaning it doesn't rely on the framerate. movement, and that our movement doesn't mess up in anyway. 
-Once done save the script and then drag and drop the character controller into the empty slot.
-Set the speed to whatever you want and you should be able to start moving.
-Movement now works! 
-However one problem that we have encountered is that the player is hovering. 
-In order to fix it, we had to apply some gravity code to it and make sure there's no movement on the Y axis. 

-Next we're going to  point the player in the direction their moving in, using Atan2 
float targetAngle = Mathf.Atan2(direction.x, direction.y);
Atan 2 is the mathematical function that returns any values ranging from the Xaxis that starts at  0, and stops at X,y. Giving the angle from the X axis to the vector and increases clockwise as we move along so it calculates it accordingly.

 transform.rotation = Quaternion.Euler(0f, targetAngle, 0f);
 This line of code allows the player to rotate in the direction that the camera is facing accordingly. 
 Now we're going to use this line of code to make the turn time much more smooth in terms of its animation, to do this we declare a new public variable.  
 public float turnSmoothTime = 0.1f;  
 This makes it look more cleaner and making it so that rotation doesn't snap. 
 float angle = Mathf.SmoothDampAngle(transform.eulerAngles.y, targetAngle, ref turnSmoothTime); 
 
 
 
 
 Vector3 moveDirection = Quaternion.Euler(0f, targetAngle, 0f) * Vector3.forward; 
 This line of code takes our movement into account.
 Finally add an extension, CinemachineCollider and that essentially makes camera angles more clear. 
        
  ## 12/10/21
# Day 3 week 4 
# Doing Movement for a platform fighter. 

So my initial idea for my final year project was to make a 2D platform fighter but in order to do so I'd need to know how to have decent movement such as run and jumps so in this tutorial I'll be talking about how I'll be doing that. 


# The Stage

First I need to have a simple stage, one that's just a regular rectangle will do. Go to the Heirachy, and make a Square object and place it into your scene then go ahead and scale it to however you'd want it to look, for this one I'm gonna be doing simple stage no platforms for simplicity sake.  

Next we're going to make sure that we have a solid surface because if we run the game it's more than likely that our character will fall through the map without being able to run on something. 





# Setting up our Character 

For this test, just make a sprite same thing with a square as we did before, next add a box collider (2D) as we're doing it in a 2D space. Next, add a rigidbody 2D component to our character too, so we can jump around later on. 

# Movement

Now let's move on to the meat of the project which is the movement, set up a brand new character script let's name it CharacterController.
Open up the script and we have a basic script now that we can work with. In the Update part of the script we'll start by writing something for the X axis. 
We'll start by writing this: 
 
    public float MovementSpeed = 5f;
    private float Movement;
    private Rigidbody2D rb;
    private void Awake()
    {
        rb = GetComponent<Rigidbody2D>();
    }
    void Start()
    {
        
    }

  
    void Update()
    {
        Movement = Input.GetAxis("Horizontal");
       
    }

    private void FixedUpdate()
    {
        rb.velocity = new Vector2(Movement* MovementSpeed, rb.velocity.y); 
    }
}
  
  
  Now to test this I then added the script to my test character and hit play. As it seems the character moves fairly slow, now in order to make the player move at a faster pace I'd need to mess around with the "Movement Speed" component in the inspector attached to my square. Make the movement speed about 8, and move around and the player now moves at a faster rate!
  
  # Jumping
  Now we're focusing on jumps, here's the script for it, firstly we've made variables that work off of if the player's feet is touching the ground if it is then it allows the player to jump, if not then they can't be allowed to jump.
  
  Here's the script: 
  using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Charactercontroller : MonoBehaviour
{
    public float MovementSpeed = 10f;
    private float Movement;
    private Rigidbody2D rb;
    public float JumpForce = 1;
    private bool isGrounded;
    public float CheckRadius;
    public Transform feetPos;
    public LayerMask whatIsGround;

    private void Awake()
    {
        rb = GetComponent<Rigidbody2D>();
    }
    void Start()
    {
     
    }

  
    void Update()
    {
        Movement = Input.GetAxis("Horizontal");
        isGrounded = Physics2D.OverlapCircle(feetPos.position, CheckRadius, whatIsGround);
        if (isGrounded==true && Input.GetKeyDown(KeyCode.Space))
        {
            rb.velocity = Vector2.up * JumpForce;
        }
       
    }

    private void FixedUpdate()
    {
          Movement = Input.GetAxisRaw("Horizontal");
        rb.velocity = new Vector2(Movement * MovementSpeed, rb.velocity.y);

    }
}

  
  To test this go back to unity and make an empty game object called FeetPos which helps determine where we want the feet position of our character.
  Once done, then place the FeetPos on the player object underneath it making it a parent object. Select our player and head to the inspector, now choose feetPos, and work find the object and drag and drop to the appropriate area. Make the radius of the position around 0.3 so that it's not so big or small. Turn up the jump force to 10 and Gravity scale so that the character doesn't function like a balloon. 
  
  
  

Make a new layer called Ground, and add an empty game object to it this makes it so that the program can detect any ground allowing us to jump accordingly once we press the correct key. 
I've run the test and it works, small and easy jumps if I wanted to change how high I can make the jump, I'd need to make sure that the Jump force is increased to make the jump higher. 20 makes the jump much more realistic, and makes the jump feel more natural and smoother.
    
## 19/10/21
# Day 4 Falling off the stage, and respawn.
   
In the idea of a platform fighter, the goal is to knock your opponents off the screen if the game is being played with lives then it's normal for the player to respawn back on to the stage after falling off, that's what I'll be working on today. 


First things first, is to make an invisible platform that exists at the bottom of the level and acts as a trigger that when touched, will destroy our player object and make it respawn, but to make it outside the Camera's initial view. In order to make it so that it's not in plain view I changed the opacity of the rectangle. Check off the "Is Trigger" box in the Inspector. Make sure that the player is tagged as "Player" like in the previous project. 

Next I needed to set a respawn point for the player so that we know where they'll respawn when they come back from falling. 
Now it's time to make a script for this to work, make a new script and I chose to name it BlastZone. 

Next I ended up writing this code:
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class BlastZones : MonoBehaviour
{

    [SerializeField] private Transform player;

    [SerializeField] private Transform BlastZone;
    // Start is called before the first frame update
    private void OnTriggerEnter2D(Collider other)
    {
        player.transform.position = BlastZone.transform.position;  
    }
}

The code is basically telling Unity to respawn our player object whenever it hits the collider in this case being the blast zone. Next I had to assign the blast zone and player variables after attaching the script to the Blastzone object (our trigger). However one I play the project all seems to be working except there's no respawn happening because Unity encountered the error, OntriggerEnter2D. The solution was to change the code to OntriggerEnter but one problem is our player object falls through the floor. To solve this I reused one of my other square assets as a floor and it works the only problem is now the respawn doesn't work. 

The issue wasn't in the code it was the fact that I used the incorrect game object, for the respawn point


# Part 2 Moving platforms 

This one's fairly easy, some stages will have moving platforms and I wanted to essentially go over how to do them, this was because I wanted to make stages similar to the ones from Super Smash Bros. 

The idea behind this methodology is to animate top platform along the stage, First things First, I made an animations folder, then went to Window >Animation>Animation and chose the timeline then begun animating, the idea here is to have it loop an animation that sees the stage move side to side at an appropriate pace. My first issue was with the inital Keyframes themseleves and the fact the platform is moving too fast. So to fix this I'd space them out every 30 frames then decided it was too fast and spaced it out more to 80 frames each and it worked. 
I noticed that there were some problems with the animation after it ended as the platform would just teleport back, so I had to loop its position in order to make the animation much more smoother. This is to make sure that the positioning works. 

## 02/11/21

# Day 5 working on Melee attacks and knockback
For this part, I wanted to make a swordfighter do attacks first so I have an easy visual indicator that the attack system for the game actually works. To make this hapepn, I imported a sword sprite and attached it to my main fighter, so when I move around the sword also moves around in the scene with the main character. Now that movement is working with no issues whatsoever, it's time to make a player attack script. 

To make it so attacks can't be spammed instantly I'm going to be adding a small timer for cooldown between attacks so far I have: 

using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Playerattack : MonoBehaviour
{
    private float Cooldown;
    public float StartCooldown;

    private void Update()
    {
        if (Cooldown<=0)
            //then you'd be allowed to attack because the cooldown is at 0 
        {
            StartCooldown = Cooldown;
        }
    }
}

What I'm doing here is I'm telling the program when the cooldown is active and when it's not hence if the cooldown for an attack is at 0. I later realised that I'd have to apply this script to a lot more attacks one for each direction one for up, down, left and right. But in order to get this right I'm primarily focusing on neutral attack or the standard button press attack, or a neutral air. 


First things first I made an enemy script that helps ensure that when attaked the enemy takes damage to make it so that both players can deal damage I've attached the script to both players respectively, so that both can be attacked. 

using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Enemy : MonoBehaviour
{
    public int health;
    public float speed;

    public void TakeDamage(int damage)
    {
        health -= damage;
        Debug.Log("damage TAKEN");
    }
}
The script just ensures that damage is taken when our character makes an attack. 

Here's the attack script 

using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Playerattack : MonoBehaviour
{
    private float Cooldown;
    public float StartCooldown;
    public LayerMask whatisEnemies;
    public Transform attackPos;
    public float attackRange;
    public int damage; 
    private void Update()
    {
        if (Cooldown<=0)
            //then you'd be allowed to attack because the cooldown is at 0 
        {
            StartCooldown = Cooldown;
        }
        if (Input.GetKey(KeyCode.P))
        {
            Collider2D[] enemiestoDamage = Physics2D.OverlapCircleAll(attackPos.position,attackRange,whatisEnemies);
            for (int i = 0; i < enemiestoDamage.Length; i++)
            {
                enemiestoDamage[i].GetComponent<Enemy>().TakeDamage(damage); 
            }
        }
        else
        {
            Cooldown -= Time.deltaTime; 
        }

        void OnDrawGizmosSelected()
        {
            Gizmos.color = Color.red;
            Gizmos.DrawWireSphere(attackPos.position, attackRange);

        }
    }
}

   
    A problem I ran into was that the Hitbox wouldn't show, so I had to change the "void" on the OnDrawGizmos line, to private void, so that it'd be declared and would show up in my unity view. Messing with the attack range allowed me to mess around with the size of the hitbox and setting the cooldown to 0.3 allows for a small window for attack cooldown to occur within unity. 
   After adding the Enemy tag to the second sprite it should now recognise the attack. 
    
Seeing as there's no animation to it, it's hard to see if something takes damage, hence the debug, that tels us at the bottom of the screen if something takes damage or not, and it does! The cooldown elements were inspired by framedata principles from other fighting games. 


