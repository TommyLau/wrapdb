project(
  'ucl',
  'c',
  version: '1.03',
  license: 'GPL-2.0-or-later',
  meson_version: '>=1.1.0',
)

cc = meson.get_compiler('c')

ucl_sources = files(
  'src/alloc.c',
  'src/n2b_99.c',
  'src/n2b_d.c',
  'src/n2b_ds.c',
  'src/n2b_to.c',
  'src/n2d_99.c',
  'src/n2d_d.c',
  'src/n2d_ds.c',
  'src/n2d_to.c',
  'src/n2e_99.c',
  'src/n2e_d.c',
  'src/n2e_ds.c',
  'src/n2e_to.c',
  'src/ucl_crc.c',
  'src/ucl_init.c',
  'src/ucl_ptr.c',
  'src/ucl_str.c',
  'src/ucl_util.c',
)

ucl_inc = include_directories('include')
ucl_c_args = ['-DUCL_STRICT']

if host_machine.cpu_family() == 'aarch64'
  ucl_c_args += ['-DACC_ARCH_UNKNOWN=1', '-DACC_INFO_ARCH="aarch64"']
endif

if cc.get_id() == 'msvc' or cc.get_id() == 'clang-cl'
  ucl_c_args += ['-DACC_CONFIG_AUTO_NO_FUNCTIONS']
  ucl_c_args += cc.get_supported_arguments(
    '/wd4100',
    '/wd4127',
    '/wd4244',
    '/wd4267',
    '/wd4996',
  )
else
  ucl_c_args += cc.get_supported_arguments(
    '-Wno-sign-conversion',
    '-Wno-conversion',
    '-Wno-unused-parameter',
    '-Wno-cast-align',
    '-Wno-implicit-int-conversion',
    '-Wno-shorten-64-to-32',
    '-Wno-tautological-constant-out-of-range-compare',
    '-Wno-unused-local-typedef',
    '-Wno-type-limits',
    '-Wno-unused-local-typedefs',
    '-Wno-maybe-uninitialized',
  )
  if cc.get_id() == 'gcc'
    ucl_c_args += ['-DACC_COMPILE_TIME_ASSERT_HEADER(e)=']
  endif
endif

ucl_lib = library(
  'ucl',
  ucl_sources,
  include_directories: ucl_inc,
  c_args: ucl_c_args,
)

ucl_dep = declare_dependency(
  include_directories: ucl_inc,
  link_with: ucl_lib,
)

meson.override_dependency('ucl', ucl_dep)
