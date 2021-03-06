
Complete Tank model file
============================

.. code-block:: YAML

 format: ChoreonoidBody
 formatVersion: 1.0
 angleUnit: degree
 name: Tank
 
 links:
   -
     name: CHASSIS
     translation: [ 0, 0, 0.1 ]
     jointType: free
     centerOfMass: [ 0, 0, 0 ]
     mass: 8.0
     inertia: [
       0.1, 0,   0,
       0,   0.1, 0,
       0,   0,   0.5 ]
     elements:
       Shape:
         geometry:
           type: Box
           size: [ 0.45, 0.3, 0.1 ]
         appearance: &BodyAppearance
           material:
             diffuseColor: [ 0, 0.6, 0 ]
             specularColor: [ 0.2, 0.8, 0.2 ]
             shinines: 0.6
   -
     name: CANNON_Y
     parent: CHASSIS
     translation: [ -0.04, 0, 0.08 ]
     jointType: revolute
     jointAxis: Z
     jointRange: unlimited
     jointId: 0
     centerOfMass: [ 0, 0, 0.025 ]
     mass: 4.0
     inertia: [
       0.1, 0,   0,
       0,   0.1, 0,
       0,   0,   0.1 ]
     elements:
       Shape:
         geometry:
           type: Box
           size: [ 0.2, 0.2, 0.08 ]
         appearance: *BodyAppearance
   -
     name: CANNON_P
     parent: CANNON_Y
     translation: [ 0, 0, 0.04 ]
     jointType: revolute
     jointAxis: Y
     jointRange: [ -45, 10 ]
     jointId: 1
     elements:
       - 
         # Turnet
         type: RigidBody
         centerOfMass: [ 0, 0, 0 ]
         mass: 3.0
         inertia: [
           0.1, 0,   0,
           0,   0.1, 0,
           0,   0,   0.1 ]
         elements:
           Shape:
             geometry:
               type: Cylinder
               height: 0.1
               radius: 0.11
             appearance: *BodyAppearance
       - 
         # Barrel
         type: Transform
         translation: [ 0.2, 0, 0 ]
         rotation: [ 0, 0, 1, 90 ]
         elements:
           RigidBody:
             centerOfMass: [ 0, 0, 0 ]
             mass: 1.0
             inertia: [
               0.01, 0,   0,
               0,    0.1, 0,
               0,    0,   0.1 ]
             elements:
               Shape:
                 geometry:
                   type: Cylinder
                   height: 0.2
                   radius: 0.02
                 appearance: *BodyAppearance
       -
         type: SpotLight
         name: MainLight
         translation: [ 0.08, 0, 0.1 ]
         direction: [ 1, 0, 0 ]
         beamWidth: 36
         cutOffAngle: 40
         cutOffExponent: 6
         attenuation: [ 1, 0, 0.01 ]
         elements:
           Shape:
             rotation: [ 0, 0, 1, 90 ]
             translation: [ -0.02, 0, 0 ]
             geometry:
               type: Cone
               height: 0.04
               radius: 0.025
             appearance:
               material:
                 diffuseColor: [ 1.0, 1.0, 0.4 ]
                 ambientIntensity: 0.3
                 emissiveColor: [ 0.8, 0.8, 0.3 ]
       - 
         type: Transform
         translation: [ 0.1, 0, 0.05 ]
         rotation: [ [ 1, 0, 0, 90 ], [ 0, 1, 0, -90 ] ]
         elements:
           -
             type: Camera
             name: Camera
             format: COLOR_DEPTH
             fieldOfView: 65
             width: 320
             height: 240
             frameRate: 30
           -
             type: RangeSensor
             name: RangeSensor
             scanAngle: 90
             scanStep:  0.5
             scanRate:  10
             maxDistance: 10
           -
             type: Shape
             geometry:
               type: Box
               size: [ 0.04, 0.015, 0.01 ]
             appearance:
               material:
                 diffuseColor: [ 0.2, 0.2, 0.8 ]
                 specularColor: [ 0.6, 0.6, 1.0 ]
                 shininess: 0.6
   -
     name: CRAWLER_TRACK_L
     parent: CHASSIS
     translation: [ 0, 0.2, 0 ]
     jointType: pseudoContinuousTrack
     jointAxis: Y
     centerOfMass: [ 0, 0, 0 ]
     mass: 1.0
     inertia: [
       0.02, 0,    0,
       0,    0.02, 0,
       0,    0,    0.02 ]
     elements:
       Shape: &CRAWLER 
         geometry:
           type: Extrusion
           crossSection: [
             -0.22, -0.1,
              0.22, -0.1,
              0.34,  0.06,
             -0.34,  0.06,
             -0.22, -0.1
             ]
           spine: [ 0, -0.05, 0, 0, 0.05, 0 ]
         appearance:
           material:
             diffuseColor: [ 0.2, 0.2, 0.2 ]
   -
     name: CRAWLER_TRACK_R
     parent: CHASSIS
     translation: [ 0, -0.2, 0 ]
     jointType: pseudoContinuousTrack
     jointAxis: Y
     centerOfMass: [ 0, 0, 0 ]
     mass: 1.0
     inertia: [
       0.02, 0,    0,
       0,    0.02, 0,
       0,    0,    0.02 ]
     elements:
       Shape: *CRAWLER 
