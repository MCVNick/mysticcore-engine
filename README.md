# Mystic Core Engine

A modern C++20 game engine with ECS architecture, OpenGL rendering, and hot-reload capabilities.

## Features

- **ECS Architecture**: Built on EnTT for high-performance entity-component systems
- **Modern Rendering**: OpenGL-based renderer with PBR materials, post-processing (bloom, fog, tone mapping)
- **Hot-Reload**: Live asset and data reloading during development
- **Ability System**: Flexible ability framework with mana, cooldowns, range checking, and line-of-sight
- **Debug Tools**: ImGui integration with entity inspector, performance profiling (Tracy)
- **Cross-Platform**: Windows and Linux support via CMake

## Using in Your Game

### As a Git Submodule

```bash
# In your game repository
git submodule add https://github.com/YOUR_USERNAME/mysticcore-engine.git
git submodule update --init --recursive
```

Then in your game's CMakeLists.txt:

```cmake
add_subdirectory(mysticcore-engine)
target_link_libraries(YourGame PRIVATE MysticCoreEngine)
```

### Prerequisites

- **CMake** 3.21+
- **C++20 compiler** (MSVC 2022, GCC 11+, Clang 13+)
- **vcpkg** for dependencies
- **Git**

## Building the Engine Standalone

```bash
# Clone with vcpkg
git clone https://github.com/YOUR_USERNAME/mysticcore-engine.git
cd mysticcore-engine
git clone https://github.com/microsoft/vcpkg.git
./vcpkg/bootstrap-vcpkg.sh  # or .bat on Windows

# Configure and build
cmake -B build -S . -DCMAKE_BUILD_TYPE=Debug \
  -DCMAKE_TOOLCHAIN_FILE=./vcpkg/scripts/buildsystems/vcpkg.cmake
cmake --build build -j

# Run example (if built)
./build/bin/MysticCoreExample
```

## Project Structure

```
mysticcore-engine/
├── include/
│   └── mysticcore/
│       ├── core/       # App, time, logger, input
│       ├── gfx/        # Renderer, shaders, materials
│       ├── ecs/        # Components, systems, registry
│       ├── io/         # File system, asset loading
│       ├── physics/    # Raycasting, collision
│       └── tools/      # ImGui debug panels
├── src/               # Implementation files
├── examples/          # Example applications
└── CMakeLists.txt
```

## Documentation

See the [docs](docs/) folder for:
- Architecture overview
- API reference
- Component/system guides
- Ability system tutorial

## License

MIT License - feel free to use in commercial projects!

## Contributing

Contributions welcome! Please read [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.
