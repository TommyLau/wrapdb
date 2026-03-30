# geometryc - bgfx geometry compiler

geometryc_sources = files(
  'geometryc.cpp',
)

geometryc = executable(
  'geometryc',
  geometryc_sources,
  meshoptimizer_src,
  include_directories: bgfx_3rdparty_inc,
  dependencies: [bgfx_vertexlayout_dep],
  cpp_args: '-D_CRT_SECURE_NO_WARNINGS',
)
