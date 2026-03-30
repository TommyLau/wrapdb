# bgfx library
# cmake: bgfx.cmake + shared.cmake

# =============================================================================
# Shared include directories (defined here so subdirs avoid ../ references)
# =============================================================================

bgfx_3rdparty_inc = include_directories('3rdparty')

bgfx_dawn_inc = include_directories(
  '3rdparty/dawn',
  '3rdparty/dawn/include',
  '3rdparty/dawn/src',
  '3rdparty/dawn/src/tint',
)

bgfx_spirv_tools_inc = include_directories(
  '3rdparty/spirv-tools',
  '3rdparty/spirv-tools/include',
  '3rdparty/spirv-tools/include/generated',
  '3rdparty/spirv-tools/source',
)

bgfx_spirv_headers_inc = include_directories('3rdparty/spirv-headers/include')

# =============================================================================
# bgfx library
# =============================================================================

# renderer_mtl/vk/webgpu excluded — added per-platform (ObjC++ on macOS, C++ elsewhere)
bgfx_src = files(
  'src/bgfx.cpp',
  'src/debug_renderdoc.cpp',
  'src/dxgi.cpp',
  'src/glcontext_egl.cpp',
  'src/glcontext_html5.cpp',
  'src/glcontext_wgl.cpp',
  'src/nvapi.cpp',
  'src/renderer_agc.cpp',
  'src/renderer_d3d11.cpp',
  'src/renderer_d3d12.cpp',
  'src/renderer_gl.cpp',
  'src/renderer_gnm.cpp',
  'src/renderer_noop.cpp',
  'src/renderer_nvn.cpp',
  'src/shader.cpp',
  'src/shader_dxbc.cpp',
  'src/shader_spirv.cpp',
  'src/topology.cpp',
  'src/vertexlayout.cpp',
)

bgfx_public_inc = include_directories('include')
bgfx_private_inc = include_directories(
  '3rdparty',
  '3rdparty/khronos',
)

bgfx_compile_args = [
  '-D__STDC_LIMIT_MACROS',
  '-D__STDC_FORMAT_MACROS',
  '-D__STDC_CONSTANT_MACROS',
] + bgfx_config_args

bgfx_private_args = []

# Vulkan renderer uses NULL comparisons with non-dispatchable handles (uint64_t
# on 32-bit), which is a compile error on clang/gcc but not MSVC.
if meson.get_compiler('cpp').sizeof('void*') == 4 and host_machine.system() == 'linux'
  bgfx_compile_args += '-DBGFX_CONFIG_RENDERER_VULKAN=0'
endif
bgfx_platform_deps = []
bgfx_link_extra = []
bgfx_dx_inc = []

if host_machine.system() == 'darwin'
  bgfx_platform_deps += dependency(
    'appleframeworks',
    modules: ['Cocoa', 'Metal', 'QuartzCore', 'IOKit', 'CoreFoundation'],
  )
  # renderer_mtl/vk/webgpu include ObjC headers on macOS — compile as ObjC++
  bgfx_src_inc = include_directories('src')
  bgfx_objcpp_files = []
  foreach f : ['renderer_mtl', 'renderer_vk', 'renderer_webgpu']
    bgfx_objcpp_files += configure_file(
      input: 'src/' + f + '.cpp',
      output: f + '.mm',
      copy: true,
    )
  endforeach
  bgfx_objcpp_lib = static_library(
    'bgfx_objcpp',
    bgfx_objcpp_files,
    include_directories: [bgfx_public_inc, bgfx_private_inc, bgfx_src_inc],
    dependencies: [bx_dep, bimg_dep],
    objcpp_args: bgfx_compile_args,
  )
  bgfx_link_extra += bgfx_objcpp_lib

elif host_machine.system() == 'linux'
  bgfx_platform_deps += dependency('x11')
  bgfx_platform_deps += dependency('gl')
  bgfx_platform_deps += dependency('threads')
  bgfx_platform_deps += meson.get_compiler('cpp').find_library('dl')
  if opt_with_wayland
    bgfx_private_args += '-DWL_EGL_PLATFORM=1'
    bgfx_platform_deps += dependency('wayland-egl')
  endif
  bgfx_src += files(
    'src/renderer_mtl.cpp',
    'src/renderer_vk.cpp',
    'src/renderer_webgpu.cpp',
  )
  bgfx_dx_inc = include_directories(
    '3rdparty/directx-headers/include',
    '3rdparty/directx-headers/include/directx',
    '3rdparty/directx-headers/include/wsl/stubs',
  )

elif host_machine.system() == 'windows'
  bgfx_private_args += '-D_CRT_SECURE_NO_WARNINGS'
  bgfx_src += files(
    'src/renderer_mtl.cpp',
    'src/renderer_vk.cpp',
    'src/renderer_webgpu.cpp',
  )
  bgfx_dx_inc = include_directories(
    '3rdparty/directx-headers/include',
    '3rdparty/directx-headers/include/directx',
  )
endif

bgfx_lib = static_library(
  'bgfx',
  bgfx_src,
  include_directories: [bgfx_public_inc, bgfx_private_inc] + bgfx_dx_inc,
  dependencies: [bx_dep, bimg_dep] + bgfx_platform_deps,
  cpp_args: bgfx_compile_args + bgfx_private_args,
  link_with: bgfx_link_extra,
)

bgfx_dep = declare_dependency(
  include_directories: bgfx_public_inc,
  link_with: bgfx_lib,
  compile_args: bgfx_compile_args,
  dependencies: bgfx_platform_deps,
)

# =============================================================================
# Shared interface dependencies (cmake: shared.cmake equivalents)
# =============================================================================

# bgfx-vertexlayout — used by shaderc, geometryc, and example-common
bgfx_vertexlayout_src = files('src/vertexlayout.cpp')
bgfx_vertexlayout_dep_list = [bx_dep]
if host_machine.system() == 'darwin'
  bgfx_vertexlayout_dep_list += dependency(
    'appleframeworks',
    modules: ['Cocoa'],
  )
endif
bgfx_vertexlayout_dep = declare_dependency(
  include_directories: bgfx_public_inc,
  sources: bgfx_vertexlayout_src,
  dependencies: bgfx_vertexlayout_dep_list,
)

# bgfx-shader — used by shaderc
bgfx_shader_src = files(
  'src/shader.cpp',
  'src/shader_dxbc.cpp',
  'src/shader_spirv.cpp',
)
bgfx_shader_dep = declare_dependency(
  include_directories: bgfx_public_inc,
  sources: bgfx_shader_src,
  dependencies: bx_dep,
)

# webgpu — header-only interface (cmake: 3rdparty/webgpu.cmake)
# Only available when webgpu renderer is enabled and the headers exist.
if get_option('bgfx_config_renderer_webgpu')
  webgpu_inc = include_directories('3rdparty/webgpu/include')
  webgpu_dep = declare_dependency(
    include_directories: webgpu_inc,
  )
else
  webgpu_dep = declare_dependency()
endif

# =============================================================================
# 3rdparty: meshoptimizer (used by geometryc AND example-common)
# =============================================================================

subdir('3rdparty/meshoptimizer')

# =============================================================================
# 3rdparty for shader tools (cmake: if(BGFX_BUILD_TOOLS_SHADER) in CMakeLists.txt)
# =============================================================================

if opt_build_tools_shader
  subdir('3rdparty/fcpp')
  subdir('3rdparty/spirv-cross')
  subdir('3rdparty/spirv-opt')
  subdir('3rdparty/glslang')
  subdir('3rdparty/glsl-optimizer')
  subdir('3rdparty/tint')
endif

# =============================================================================
# example-common (imgui, debugdraw, entry, font, nanovg, ps)
# cmake: example-common target from examples.cmake
# =============================================================================

if opt_build_example_common
  subdir('3rdparty/dear-imgui')
  subdir('examples/common')
else
  imgui_dep = disabler()
endif

# =============================================================================
# Examples (gated by bgfx_build_examples)
# =============================================================================

if opt_build_examples
  subdir('examples')
endif
