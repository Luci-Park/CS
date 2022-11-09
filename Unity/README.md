# Unity


## Coroutine and Multi-Processing, Multithreading and Coroutine

## Rendering Pipelines
3D 데이터를 2D 이미지로 구성하는 과정.
Application -> Geometry -> Rasterizer
1. Application: 오브젝트 선별, 배칭처리 등등
2. Geometry: world-view projection transform -> clipping -> vertex shader
3. Rasterizer: 폴리곤을 픽셀로 매칭시키는 것.

### High Definition Render Pipeline(HDRP)
고품질 비주얼 구현에 적합

### Universal Render Pipeline(URP)
모바일 디바이스에 적합. 가볍고 고품질임.