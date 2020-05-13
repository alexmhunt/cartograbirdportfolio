# Alex Hunt - IMGD4000 Development Portfolio
## Computer Science / IMGD Major, Cartograbird Gameplay & Story Programmer

# Project Description
Cartograbird is an open world exploration game in which the player is tasked to find the scattered parts of a crash landed explorer. The game has flight mechanics for the main character Carmen to soar around the world in search of these parts. The player can look at their map to orient themselves and document their exploration. Cartograbird was developed using Unreal Engine 4.24.3.

# My Contribution
The focus of my work on Cartograbird was to program the game's movement mechanics (including flight), as well as program the game's story. I implemented character and camera movement, as well as a dialogue and quest progression system. To achieve these objectives, I used both C++ and Unreal Engine 4's Blueprints scripting language.

As a team, we determined what movement functionality I would need to implement:
- A player character that walks around using WASD.
- When the F key is pressed, the player character begins to fly, moving forward at a near constant speed.
- A third-person camera that follows the player around.
- Mouse controls for the third-person camera, which controls the player during flight.

...And what story we would need to guide the player into exploring our world.
- A single NPC with dialogue
- Six plane parts that the player can pick up
- A simple quest system to keep track of the player's progress in helping this NPC.

The following graph shows the architecture of Cartograbird, as well as the division of work among each of the developers. Though we made minor changes to help each other out during development, this graph is generally accurate to our workload.
![Graph](https://github.com/alexmhunt/cartograbirdportfolio/blob/master/graph.png?raw=true)

# And we have liftoff!
When I first laid my hands on this project, I was admittedly intimidated. I thought that I would have to program a whole flight physics system from scratch. Such is possible in Unreal, but as I worked on defining the player character's movement behavior, I found that Unreal had an existing movement API: the [CharacterMovementComponent](https://docs.unrealengine.com/en-US/API/Runtime/Engine/GameFramework/UCharacterMovementComponent/index.html). 

For a seven-week scope project, this API was a godsend. Instead of learning detailed physics on the fly, I was able to focus more on the feel of the character movement controls and on the camera dynamics. Below is all the code I needed to get the player character off the ground:

```c++
// Toggles flight mode on/off when ToggleFlight key pressed
void ACarmenCharacter::ToggleFlight(){
	if(!isFlying) // #define isFlying (Movement->MovementMode == MOVE_Flying)
		OnFlightModeStart();
	else
		OnFlightModeStop();
}

// Handles entering flight mode
void ACarmenCharacter::OnFlightModeStart(){
	// bPressedJump = true if player toggled flight mode while jumping.
	// If it stayed true, Carmen would fly up indefinitely.
	bPressedJump = false; 
	
	UE_LOG(LogTemp, Warning, TEXT("FLIGHT MODE START"));
	// Set player movement mode to Flying.
	Movement->SetMovementMode(MOVE_Flying);
	
}

```

# Mr. Wiki, I don't feel so good...
My initial intent was to implement all of my functionality in C++. However, in an unfortunate turn of events, the Unreal C++ Wiki was deleted at the start of the project, leaving sparse online resources for C++ at best. Though I did not abandon C++ entirely, I ended up coding camera movement functionality and some flight movement using Blueprints.
Looking back, I believe it was beneficial that I got experience in both scripting languages. They both have their uses: C++ for its performance and greater level of control, and Blueprints for its speedy compile time, visual nature, and user interface functionality.

# Movement and Camera
I implemented the player character's ground movement entirely in C++. In the below code, when the player wants to move forward or backward, I find out which direction is "forward" for the player and tell the player character's CharacterMovementComponent to move in that direction by however much the input binding specifies (positive values will move forward, negative values will move backward).
```c++
// Handles forward and backward movement.
// If amount is positive, player moves forward, or backward if negative.
void ACarmenCharacter::MoveForward(float amount){
	if(notFlying){ // WASD controls disabled during flight.
	// Find out which way is "forward" and record that the player wants to move that way.
	FVector Direction = FRotationMatrix(Controller->GetControlRotation()).GetScaledAxis(EAxis::X);
	AddMovementInput(Direction, amount); // Input component takes care of the rest!
	}
}
```
Unlike ground movement, flight is not controlled by WASD. In flight, the player character automatically moves forward at a set speed, which the player can either increase or decrease by holding down the Space or Shift keys.

# Story and Dialogue

# Git: Friend or Foe?
This was my first experience in Unreal, and so my first experience with version control in Unreal. My group chose to use Git through Gitlab, due to its open-source nature and our previous experience with Git. Initially, we had little trouble, but when we happened to edit the same Blueprints files, we felt the wrath of Git merge conflicts. Git most likely isn't used to dealing with .uasset files and visual code like this, so sometimes, whenever I merged another developer's branch into my work, my Blueprints would get corrupted and I would risk losing my work. In fact, I did have to redo certain aspects of my Blueprints code a few times.

Even after all this trouble, I still like Git, and will still use in the future. For an Unreal Engine 4 project with multiple developers, though, I'm curious to see if any alternatives such as Perforce handle Blueprint merging better.

# Conclusion
This game provided me with an invaluable development experience. I've learned a lot about using Unreal Engine 4, and feel more confident using it for future personal projects, coded in both Blueprints and C++. Additionally, I've gained a better sense for scoping games and dividing work in a fair manner amongst developers.
