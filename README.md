# Carvis

<img src=/IndependentProject/Report/CarPhotos/carFrontSide.jpg width="360">        <img src=/IndependentProject/Report/CarPhotos/carSide.jpg width="360">

ELE 302 (Building Real Systems) is the junior level capstone lab class in the Electrical Engineering curriculum. For the independent project portion of this class, we created Carvis the autonomous guard robot. Carvis continuously patrols a route marked out with black tape on the floor. If it hears a large sound like a clap or footstep, the robot stops and attempts to interrogate the source of the sound by playing sound effects and shining a bright light. If the intruder displays the proper identification, the robot returns to patrolling the track. Otherwise, it sounds a loud alarm with its lights and speakers. The primary technical challenge of the project was in the techniques for sound localization via time difference of arrival in order to accurately determine the direction of origin of the sound, and thus turn its LED and camera in the proper direction.

As this project was completed as part of the ELE 302 course, we are required to keep source code and some details of the drive system private. Feel free to contact the authors [Sunny He](https://github.com/AG6GR) and [Vincent Po](https://github.com/vbpo) for more details.

## Sound Localization
<img src=/IndependentProject/Report/SoundDiagram.png width="480">

Our primary goal is to use sound to localize the position of a loud audio pulse, such as the sound of a clap. The approach we used is the technique of time difference of arrival, where the direction to the source of the sound is determined based on the difference in time the sound wave takes to reach two or more microphones located a known distance apart. With two microphones, the bearing to a sound source can be calculated fairly easily using trigonometry, assuming the distance to the sound source is large relative to the distance between the two microphones.
From the above model, the bearing θ can be calculated as:

θ = sin−1(∆T/d)

The principle disadvantage of this approach is that there is a ambiguity as to which quadrant the true bearing lies in. Mathematically, the locus of possible sound source locations for a given time difference traces out a hyperbola with the two microphones as the foci. By detecting which microphone is triggered first, we can determine if the source is to the left or right of the car’s centerline, equivalent to one branch of the hyperbola. However the ambiguity as to whether the sound came from in front of the car or behind the car remains. We chose to resolve this ambiguity by checking both the forward and backward bearings when visually identifying the sound source. To acquire complete triangulation of the sound source (as well as determine its distance) we could have used three microhpones, but the time saved would not justify the substantial added complexity.

## Sound Detection

<img src=/IndependentProject/Report/CarPhotos/micTop.jpg width="360">
<img src=/IndependentProject/Report/CarPhotos/Schematics/MicBoard.png width="480">

Two identical circuits were constructed on protoboard to acquire and amplify sounds before sending the signals to the main PSoC for processing. The microphones used were small electret microphones (Pro Signal ABM-707-RC). The microphone output was amplified with a basic inverting op-amp circuit based around the LM324. The signal is then compared to a threshold with a LM311 comparator to give a simple digital output. Originally we intended to include extra filtering and reserved space on the microphone board to implement active filters. Had we needed the actual shape of the waveform rather than just the arrival time, additional filtering may have been appropriate. However, testing showed that the inherent band-limited response of the microphone provided enough noise suppression for our purposes.

## Turret Construction
<img src=/IndependentProject/Report/CarPhotos/turretFront.jpg width="240">        <img src=/IndependentProject/Report/CarPhotos/turretSide.jpg width="240">        <img src=/IndependentProject/Report/CarPhotos/turretBack.jpg width="240">

To take advantage of the sound localization data, we constructed a rotating turret which points in the direction of the sound. A 3D printed bracket attaches a Pixy camera and a custom LED driver circuit to the shaft of a stepper motor. An Arduino Uno controls the stepper motor, activates a MP3 player board to plays sound effects, and turns on the high power RGB LED.
