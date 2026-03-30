# example-common library (imgui, debugdraw, entry, font, nanovg, ps)
# cmake: example-common target from examples.cmake

example_common_src = files(
  'bgfx_utils.cpp',
  'camera.cpp',
  'cube_atlas.cpp',
  'debugdraw/debugdraw.cpp',
  'entry/cmd.cpp',
  'entry/dialog.cpp',
  'entry/entry.cpp',
  'entry/entry_android.cpp',
  'entry/entry_glfw.cpp',
  'entry/entry_html5.cpp',
  'entry/entry_noop.cpp',
  'entry/entry_sdl.cpp',
  'entry/entry_windows.cpp',
  'entry/entry_x11.cpp',
  'entry/input.cpp',
  'example-glue.cpp',
  'font/font_manager.cpp',
  'font/text_buffer_manager.cpp',
  'font/text_metrics.cpp',
  'font/utf8.cpp',
  'imgui/imgui.cpp',
  'nanovg/nanovg.cpp',
  'nanovg/nanovg_bgfx.cpp',
  'ps/particle_system.cpp',
)

if host_machine.system() == 'darwin'
  example_common_src += files(
    'entry/dialog_darwin.mm',
    'entry/entry_osx.mm',
  )
endif

example_common_public_inc = include_directories(
  '.',
  '../../3rdparty',
)

example_common_platform_deps = []
if host_machine.system() == 'linux'
  example_common_platform_deps += dependency('x11')
endif

example_common_lib = static_library(
  'example-common',
  dear_imgui_src + meshoptimizer_src + example_common_src,
  include_directories: [
    example_common_public_inc,
    bgfx_public_inc,
    bgfx_private_inc,
  ],
  dependencies: [bgfx_dep, bx_dep, bimg_dep, bimg_decode_dep] + example_common_platform_deps,
  cpp_args: ['-D__STDC_FORMAT_MACROS', '-D_CRT_SECURE_NO_WARNINGS'],
)

imgui_dep = declare_dependency(
  include_directories: [example_common_public_inc, bgfx_public_inc],
  link_with: example_common_lib,
  dependencies: [bgfx_dep, bx_dep, bimg_dep, bimg_decode_dep],
)
