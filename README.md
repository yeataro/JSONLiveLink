# JSONLiveLink
LiveLink Source for receiving JSON over sockets.

This is an example of how a LiveLink plugin can be implemented inside UE4. The plugin is not intended to be used in production.

See https://docs.unrealengine.com/en-US/Engine/Animation/LiveLinkPlugin/index.html for more information about UE4 LiveLink.

Modified to work with Transform, Camera and Light. The first object in the array defines the type of transform, camera, light or character. The second object defines the attributes for the subject and the animation. Information is transfered via UDP sockets. Rotations are quaternions (x,y,z,w). The scale values for camera and light animation are optional. Cone angles, color and intensity for lights are optional.
## Transform Subject and Animation JSON
```
{
    "conecube": [{
        "Type": "TransformSubject"
    }]
}
```
```
{
    "conecube": [{
        "Type": "TransformAnimation"
    }, {
        "Location": [465.48657, 43.207475, 478.44958305],
        "Rotation": [0.0198907, -0.011143, 0.0526314, 0.998353325],
        "Scale": [1, 1, 1]
    }]
}
```
---
## Camera Subject and Animation JSON
Values in order are field of view, aspect ratio and focal length. Special name "EditorActiveCamera" can be used to the sync views between the DCC app and Unreal.
```
{
    "EditorActiveCamera": [{
        "Type": "CameraSubject"
    }, {
        "FieldOfView": "true",
        "AspectRatio": "true",
        "FocalLength": "true",
        "ProjectionMode": "false"
    }]
}
```
```
{
    "EditorActiveCamera": [{
        "Type": "CameraAnimation"
    }, {
        "Location": [-802.9778, -172.124338, 643.369150],
        "Rotation": [-0.0235417670, 0.200008, 0.0148016, 0.9794137833],
        "Scale": [1, 1, 1],
        "Values": [88.00094273819883, 1.5, 0.5]
    }]
}
```
---
## Light Subject and Animation JSON
Color order is R,G,B. Angle is inner then outer angles
```
{
    "mylightspot": [{
        "Type": "LightSubject"
    }, {
        "Intensity": "true",
        "LightColor": "true",
        "InnerConeAngle": "true",
        "OuterConeAngle": "true"
    }]
}
```
```
{
    "mylightspot": [{
        "Type": "LightAnimation"
    }, {
        "Location": [-757.5631141, -168.424999, 590.9420490],
        "Rotation": [0.764406, -0.005041, -0.644523, -0.016625],
        "Scale": [1, 1, 1],
        "Intensity": [100],
        "LightColor": [255, 83, 101],
        "Angle": [26.1905, 34.9206]
    }]
}
```
---
## Character Subject and Animation JSON (partial sample)
Bone name then the bone parent index. The root index is -1. Animation has 1 corresponding entry for each bone.
```
{
    "mycharacter2": [{
        "Type": "CharacterSubject"
    }, {
        "Name": "root",
        "Parent": "-1"
    }, {
        "Name": "pelvis",
        "Parent": "0"
    }, {
        "Name": "spine_01",
        "Parent": "1"
    }, {
        "Name": "spine_02",
        "Parent": "2"
    }, {
```
```
{
    "mycharacter2": [{
        "Type": "CharacterAnimation"
    }, {
        "Location": [0, 0, 0],
        "Rotation": [0, 0, -8.344650268554687e-7, 1],
        "Scale": [1, 1, 1]
    }, {
        "Location": [1.94479618, -8.02260, 87.59125471115112],
        "Rotation": [-0.096259604, -0.7220132, -0.012154278, 0.685045914,
        "Scale": [1, 1, 1]
    }, {
        "Location": [10.808876905, 0.851702425003, 9.536899679e-7],
        "Rotation": [0.051463736, -0.0115846067, 0.57723715, 0.99114267],
        "Scale": [1, 1, 1]
    }, {

```
---
## Python2 Test Subject
```
import socket
 
TCP_IP = '127.0.0.1'
TCP_PORT = 54321
BUFFER_SIZE = 1024
MESSAGE = '{"mything":[{"Type": "CharacterSubject"},{"Name":"root","Parent":-1}]}'
 
s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
s.connect((TCP_IP, TCP_PORT))
s.send(MESSAGE)
s.close()
```
---
## Python2 Test Animation Frame
```
import socket
 
TCP_IP = '127.0.0.1'
TCP_PORT = 54321
BUFFER_SIZE = 1024
MESSAGE = '{"mything":[{"Type": "CharacterAnimation"},{"Location":[1,2,200],"Rotation":[0,0,0,1],"Scale":[1,1,1]}]}'
 
s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
s.connect((TCP_IP, TCP_PORT))
s.send(MESSAGE)
s.close()
```