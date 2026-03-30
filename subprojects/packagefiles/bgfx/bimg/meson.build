# bimg — image libraries for bgfx
# Three libraries matching cmake: bimg.cmake, bimg_decode.cmake, bimg_encode.cmake

# =============================================================================
# Bundled 3rdparty codecs
# =============================================================================

subdir('3rdparty/astc-encoder')
subdir('3rdparty/libsquish')
subdir('3rdparty/nvtt')
subdir('3rdparty/pvrtc')
subdir('3rdparty/etc1')
subdir('3rdparty/etc2')
subdir('3rdparty/edtaa3')
subdir('3rdparty/iqa')

# =============================================================================
# bimg — base image library (cmake: bimg.cmake)
# =============================================================================

bimg_inc = include_directories('include')

bimg_lib = static_library(
  'bimg',
  files('src/image.cpp', 'src/image_gnf.cpp'),
  astc_encoder_sources,
  include_directories: [bimg_inc, astc_encoder_inc],
  dependencies: bx_dep,
)

bimg_dep = declare_dependency(
  include_directories: bimg_inc,
  link_with: bimg_lib,
)

# =============================================================================
# bimg_decode — image decoding library (cmake: bimg_decode.cmake)
# image_decode.cpp #includes lodepng, miniz, tinyexr inline.
# =============================================================================

bimg_decode_lib = static_library(
  'bimg_decode',
  files('src/image_decode.cpp'),
  include_directories: [
    bimg_inc,
    include_directories('3rdparty'),
    include_directories('3rdparty/tinyexr/deps'),
  ],
  dependencies: bx_dep,
)

bimg_decode_dep = declare_dependency(
  include_directories: bimg_inc,
  link_with: bimg_decode_lib,
)

# =============================================================================
# bimg_encode — image encoding library + 3rdparty codecs (cmake: bimg_encode.cmake)
# =============================================================================

bimg_encode_warn_args = []
cpp = meson.get_compiler('cpp')
foreach flag : [
  '-Wno-implicit-fallthrough',
  '-Wno-shadow',
  '-Wno-shift-negative-value',
  '-Wno-undef',
  '-Wno-class-memaccess',
  '-Wno-deprecated-copy',
]
  if cpp.has_argument(flag)
    bimg_encode_warn_args += flag
  endif
endforeach

# nvtt debug.h only checks NV_CPU_X86_64/NV_CPU_AARCH64 for 64-bit pointer
# casts, but ppc64/riscv64/loongarch64 also have 64-bit pointers.
if cpp.sizeof('void*') == 8 and host_machine.cpu_family() not in [
  'x86_64',
  'aarch64',
]
  bimg_encode_warn_args += '-DNV_CPU_X86_64=1'
endif

bimg_encode_lib = static_library(
  'bimg_encode',
  files('src/image_cubemap_filter.cpp', 'src/image_encode.cpp'),
  libsquish_sources,
  nvtt_sources,
  pvrtc_sources,
  etc1_sources,
  etc2_sources,
  edtaa3_sources,
  iqa_sources,
  include_directories: [
    bimg_inc,
    include_directories('3rdparty'),
    include_directories('3rdparty/nvtt'),
    include_directories('3rdparty/iqa/include'),
    include_directories('3rdparty/astc-encoder/include'),
    include_directories('3rdparty/tinyexr/deps'),
  ],
  dependencies: bx_dep,
  cpp_args: bimg_encode_warn_args,
)

bimg_encode_dep = declare_dependency(
  include_directories: bimg_inc,
  link_with: bimg_encode_lib,
)
