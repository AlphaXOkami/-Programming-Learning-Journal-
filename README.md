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
        
        
    
   
