* Open Unreal > New Project > C++ > Basic Code and enable starter content
* File New C++ Class > Actor and name it "FloatingActor"
* In FloatingActor.h at the end of the Tick Funciton Definition
	** float RunningTime;
* In FloatingActor.cpp at the end of Tick Function
	**  FVector NewLocation = GetActorLocation();
		float DeltaHeight = (FMath::Sin(RunningTime + DeltaTime) - FMath.Sin(RunningTime));
		NewLocation.Z += DeltaHeight * 20.0f;
		RunningTime += DeltaTime;
		SetActorLocation(NewLocation);
* Save and Build Project
* In the Editor, drag in our new actor then add a sphere component to it and reposition the floating sphere
* Click Play

-----

* Create Blueprint Class from the existing GameMode
* In Project Settings, change the default GameMode to our new BP_GameMode

---------

* File > New C++ Class > Character and name it "FPSCharacter"
* Now, open FPSCharacter.h and FPSCharacter.cpp
* In FPSCharacter.cpp, after Super::BeginPlayer(), enter the following:
	**  if(GEngine) {
			GEngine->AddOnScreenDebugMessage(-1, 0.5f, FColor::Red, TEXT("Using FPSCharacter"));
		}
* Save and Build and verify our new character is in the editor
* Create a new Blueprint class for our character
* In our project settings, just under our gamemode, set the default pawn to BP_FPSCharacter
* Click Play and verify our text pops up

-----------

* Map our WASD Keys, and mouse input in the input section of project settings

----------

* In FPSCharacter.h, under virtual void SetupPlayInputComponent, add the following lines:
	**  UFUNCTION()
		void MoveForward(float Value);
		
		UFUNCTION()
		void MoveRight(float Value);
* In FPSCharacter.cpp, under Super::SetupPlayerInputComponent, add the following lines:
	**  PlayerInputComponent->BindAxis("MoveForward", this, &AFPSCharacter::MoveForward);
		PlayerInputComponent->BindAxis("MoveRight", this, &AFPSCharacter::MoveRight);
* Now we are going to define what these functions do. So, at the end of our .cpp file, we are going to add two new functions:
	**  void AFPSCharacter::MoveForward(float Value) {
			FVector Direction = FRotationMatrix(Controller->GetControlRotation()).GetScaledAxis(EAxis::X);
			AddMovementInput(Direction, Value);
		}
		
		void AFPSCharacter::MoveRight(float Value) {
			FVector Direction = FRotationMatrix(Controller->GetControlRotation()).GetScaledAxis(EAxis::Y);
			AddMovementInput(Direction, Value);
		}
* Save and Build then test it out in the editor

------------

* In FPSCharacter.cpp, at the bottom of our SetPlayerInputComponent function add:
	**  PlayerInputComponent->BindAxis("LookVertical", this, &AFPSCharacter::AddControllerPitchInput);
		PlayerInputComponent->BindAxis("LookHorizontal", this, &AFPSCharacter::AddControllerYawInput);
* Save and Build then test in the editor

-------

* In FPSCharacter.h, under the other two UFUNCITONS we previously created, we are going to add two more.
	**  UFUNCTION()
		void StartJump();
		
		UFUNCTION()
		void StopJump();
* In FPSCharacter.cpp, at the end of the file we are going to add two new functions below the ones we just defined:
	**  void AFPSCharacter::StartJump() {
			bPressedJump = true;
		}
		
		void AFPSCharacter::StopJump() {
			bPressedJump = false;
		}
* Finally, we are going to add two more lines in our SetupPlayerInputComponent method to apply funcitonality:
	**  PlayerInputComponent->BindAction("Jump", IE_Pressed, this, &AFPSCharacter::StartJump);
		PlayerInputComponent->BindAction("Jump", IE_Released, this, &AFPSCharacter::StopJump);
* Now we can Save and Build and we should have a fully functioning model