# Real-Time Ray Tracing Engine

A high-performance CPU-based ray tracing engine written in pure C with real-time rendering capabilities, interactive object manipulation, and physically-based materials.

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![C](https://img.shields.io/badge/C-11-green.svg)
![SDL2](https://img.shields.io/badge/SDL2-2.0-orange.svg)

## Features

### Rendering
- Real-time ray tracing at 30-60 FPS
- BVH acceleration structure for fast intersection testing
- Multi-threaded rendering with OpenMP
- Double-buffered framebuffer
- Configurable resolution (640x480 to 1920x1080)

### Geometry
- Sphere primitives with quadratic intersection
- Box/AABB primitives with slab method intersection
- Extensible object interface for custom primitives

### Materials
- Lambertian BRDF for diffuse surfaces
- Specular reflection with controllable reflectivity
- Transparency with Fresnel-based refraction
- Snell's law for accurate light bending
- Opacity control (transparent, translucent, opaque)

### Lighting
- Point lights with color, intensity, and range
- Inverse square law attenuation
- Hard shadow rays
- Blinn-Phong specular highlights
- Support for multiple light sources

### Interactive Features
- Click and drag to move objects
- Scroll wheel for Z-depth adjustment
- Right-click context menu
- Real-time property editing
- Add/remove objects and lights
- Scene save/load (JSON format)

## Building

### Prerequisites
- CMake 3.16+
- C11 compiler (GCC, Clang, or MSVC)
- SDL2 development libraries

### Windows (MSVC)
```powershell
mkdir build
cd build
cmake .. -DCMAKE_BUILD_TYPE=Release
cmake --build . --config Release
```

### Linux/macOS
```bash
mkdir build && cd build
cmake .. -DCMAKE_BUILD_TYPE=Release
make -j$(nproc)
```

## Controls

| Key | Action |
|-----|--------|
| 1 | Add sphere |
| 2 | Add box |
| 3 | Add light |
| Delete | Remove selected object |
| Tab | Toggle UI |
| F | Toggle FPS display |
| F11 | Toggle fullscreen |
| Ctrl+S | Save scene |
| Esc | Deselect / Close menu |

| Mouse | Action |
|-------|--------|
| Left Click | Select object |
| Left Drag | Move object (XY plane) |
| Right Click | Context menu |
| Scroll | Adjust object depth (Z) |

## Architecture

```
src/
├── main.c              # Application entry point
├── math/
│   ├── vector.h/c      # Vector operations
├── core/
│   ├── ray.h/c         # Ray structure
├── geometry/
│   ├── object.h/c      # Generic object interface
│   ├── sphere.h/c      # Sphere primitive
│   ├── box.h/c         # Box primitive
│   └── bvh.h/c         # BVH acceleration
├── material/
│   ├── material.h/c    # Material system
├── light/
│   ├── light.h/c       # Point lights
├── scene/
│   ├── camera.h/c      # Camera and ray generation
│   └── scene.h/c       # Scene management
├── render/
│   ├── framebuffer.h/c # Double-buffered pixels
│   └── renderer.h/c    # Ray tracing core
└── ui/
    ├── window.h/c      # SDL2 window
    ├── input.h/c       # Input handling
    └── ui.h/c          # UI overlay
```

## Performance

| Scene | Objects | Target FPS |
|-------|---------|------------|
| Simple | ≤5 | 60 FPS |
| Medium | ≤10 | 30 FPS |
| Complex | >10 | 15+ FPS |

## License

MIT License - see [LICENSE](LICENSE) for details.
# Ray-Tracer
