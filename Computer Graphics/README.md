# Computer Graphics

## Geometry

## Rotation
오일러(Euler)은 축들을 순서대로 독립적으로 계산하기 때문에 겹쳐지는 짐벌락 현상이 발생함.

__짐벌락(Gimbal Lock)__
두 회전 축이 같은 평면으로 겹치는 현상. 축이 한번 겹치면 이후 회전에 영향을 줄 수 있다.

### Quaternion(쿼터니언)
* 각 축을 한꺼번에 계산하기에 짐벌락 문제가 생기지 않음
* (x, y, z, w)로 이루어짐
* 180보다 큰 값은 표현할 수 없음.
* float 4개만으로 연산을 하기 때문에 4*4의 회전 행렬보다 연산 수가 적어 속도가 빠르다.
* eular보다 행렬로 변하는 것이 빠르다.

_Interpolation_
두 점 사이의 어떤 점의 위치를 알아내기 위한 것.

1. LERP(Linear intERPolation) : 두 점을 지나는 직선 위에서의 점

2. SLERP(Spherical Linear intERPolation) : 두 점을 지나는 원 위에서의 점


## Coordinates on the Rendering Pipleline
### Model(Local Space)
Object Pivot이 원점인 좌표계.

### World Space
Model Matrix로 변환을 줘서 world 좌표계로 변환.

### View Space
카메라의 시점으로 오브젝트를 보는 공간
#### Camera Transformation Matrix
The transformation that places the camera in world space.
씬에 배치된 카메라 위치를 표현하는 행렬.

M = TR (=> rotate, than translate)
- M : Camera Transformation Matrix
- R : Orientation of Camera
- T : Translation of Camera

#### View Matrix
The matrix that transform world-space vertices to view-space.
The inverse of Camera Transformation Matrix.

##### Look At Camera
카메라 위치 + world의 up vector, target으로 view matrix 제작
1. target -> 카메라 위치를 중심으로 한 좌표계 제작
    1. zAxis: normal(eye - target) => forward
    2. xAxis: normal(cross(up, zAxis)) => right
    3. yAxis: cross(zAxis, xAxis) => up (여기선up이 yaxis다)
2. 해당 좌표계의 orientation matrix(R)의 inverse 제작 => R^-1 = R^T
``` cpp
    mat4 orientation = {
        vec4(xaxis.x, yaxis.x, zaxis.x, 0),
        vec4(xaxis.y, yaxis.y, zaxis.y, 0),
        vec4(xaxis.z, yaxis.z, zaxis.z, 0),
        vec4(   0,      0,      0,      1),
    };
```
3. 해당 좌표계의 translation matrix(T)의 inverse 제작 => T^-1 = -T
``` cpp
    mat4 translation = {
        vec4(   1,      0,      0,   0 ),
        vec4(   0,      1,      0,   0 ), 
        vec4(   0,      0,      1,   0 ),
        vec4(-eye.x, -eye.y, -eye.z, 1 )
    };
```
4. V = (TR)^-1 = R^-1 * T^-1
``` cpp
    return (orientation * translation);
```

__Optimized__
eye와 axis의 dot product는 orientation 과 translation을 바로 곱하는 것과 동일하다. Translation의 inverse를 위해 음수를 곱해줘야 한다.
``` cpp
mat4 LookAtRH( vec3 eye, vec3 target, vec3 up )
{
    vec3 zaxis = normal(eye - target);    // The "forward" vector.
    vec3 xaxis = normal(cross(up, zaxis));// The "right" vector.
    vec3 yaxis = cross(zaxis, xaxis);     // The "up" vector.
 
    // Create a 4x4 view matrix from the right, up, forward and eye position vectors
    mat4 viewMatrix = {
        vec4(      xaxis.x,            yaxis.x,            zaxis.x,       0 ),
        vec4(      xaxis.y,            yaxis.y,            zaxis.y,       0 ),
        vec4(      xaxis.z,            yaxis.z,            zaxis.z,       0 ),
        vec4(-dot( xaxis, eye ), -dot( yaxis, eye ), -dot( zaxis, eye ),  1 )
    };
    
    return viewMatrix;
}
```
##### FPS Camera
pitch(위 아래로 회전)와 yaw(양 옆으로 회전) world 좌표를 이용해서 계산.
목표: a camera matrix that first rotates pitch angle about the x axis, than rotates yaw angle about the y axis, than translates position.
V = (T(RyRx)) ^-1

``` cpp
//Pitch range [-PI/2, PI]
// yaw range [0, 2PI]
mat4 FPSViewRH(vec3 eye, float pitch, float yaw){
    float cosPitch = cos(pitch);
    float sinPitch = sin(pitch);
    float cosYaw = cos(yaw);
    float sinYaw = sin(yaw);

    vec3 xaxis = {cosYaw, 0 , -sinYaw};
    vec3 yaxis = {sinYaw * sinPtch, cosPitch, cosYaw * sinPitch};
    vec3 zaxis = {sinYaw * cosPitch, -sinPitch, cosPitch * cosYaw};
    // Create a 4x4 view matrix from the right, up, forward and eye position vectors
    mat4 viewMatrix = {
        vec4(      xaxis.x,            yaxis.x,            zaxis.x,       0 ),
        vec4(      xaxis.y,            yaxis.y,            zaxis.y,       0 ),
        vec4(      xaxis.z,            yaxis.z,            zaxis.z,       0 ),
        vec4(-dot( xaxis, eye ), -dot( yaxis, eye ), -dot( zaxis, eye ),  1 )
    };
    return viewMatrix;
}
```

##### Arcball Orbit Camera
Orbit around an object that is placed in the scene.
오일러 각도를 사용할 때 x축으로 90 이상 움직이면 화면이 뒤집히고 양옆 회전이 반대가 된다.
Quaternion으로 이를 방지 할 수 있다.

V = (T1RT0) ^ -1
- V = view matrix
- T0 = translation that moves the camera a certain distance away from the object so the object can fit in the view.
- R = the rotation around the object
- T1 = the translation that moves the pivot point of the camera to the center of the object being observed. If object is at the origin, identity matrix 

```cpp
mat4 Arcball( vec3 t0, quat r, vec3 t1 = vec3(0) )
{
    mat4 T0 = translate( t0 ); // Translation away from object.
    mat4 R  = toMat4( r );     // Rotate around object.
    mat4 T1 = translate( t1 ); // Translate to center of object.
 
    mat4 viewMatrix = inverse( T1 * R * T0 );
 
    return viewMatrix;
}

mat4 ArcballOptimized( vec3 t0, quat r, vec3 t1 )
{
    mat4 T0 = translate( -t0 );       // Translation away from object.
    mat4 R  = toMat4( inverse( r ) ); // Rotate around object.
    mat4 T1 = translate( -t1 );       // Translate to center of object.
 
    mat4 viewMatrix = T0 * R * T1;
 
    return viewMatrix;
}
```

### Clip Space
보여지지 않는 오브젝트를 걸러내고 카메라의 뷰에 따라서 Perspective View, 혹은 Orthogonal View를 적용한 상태로 만드는 변환 공간.
Projection Matrix적용으로 이루어짐.

v' = MVP * v
- v = local vertex
- P = projection matrix
- V = view matrix
- M = world(model) matrix


### Screen Space
Clip Space의 좌표들을 화면에 보이는 공간(2D)으로 이동시키는 변환.
