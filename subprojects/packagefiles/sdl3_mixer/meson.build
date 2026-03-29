project(
  'SDL3_mixer',
  'c',
  version: '3.2.0',
  license: 'Zlib',
  meson_version: '>=1.1.0',
  default_options: ['c_std=c99'],
)

cc = meson.get_compiler('c')
sdl3_dep = dependency(
  'sdl3',
  required: true,
)

# --- Version defines ---

varr = meson.project_version().split('.')

sdl3_mixer_defines = [
  '-DBUILD_SDL',
  '-DSDL_BUILD_MAJOR_VERSION=@0@'.format(varr[0]),
  '-DSDL_BUILD_MINOR_VERSION=@0@'.format(varr[1]),
  '-DSDL_BUILD_MICRO_VERSION=@0@'.format(varr[2]),
]

sdl3_mixer_deps = [sdl3_dep]

# --- Core sources (always compiled) ---

sdl3_mixer_sources = files(
  'src/SDL_mixer.c',
  'src/SDL_mixer_metadata_tags.c',
  'src/SDL_mixer_spatialization.c',
)

# All decoder .c files are always compiled; the DECODER_* defines control
# which codecs are actually active (inactive decoders compile to no-ops).

sdl3_mixer_sources += files(
  'src/decoder_aiff.c',
  'src/decoder_au.c',
  'src/decoder_drflac.c',
  'src/decoder_drmp3.c',
  'src/decoder_flac.c',
  'src/decoder_fluidsynth.c',
  'src/decoder_gme.c',
  'src/decoder_mpg123.c',
  'src/decoder_opus.c',
  'src/decoder_raw.c',
  'src/decoder_sinewave.c',
  'src/decoder_stb_vorbis.c',
  'src/decoder_timidity.c',
  'src/decoder_voc.c',
  'src/decoder_vorbis.c',
  'src/decoder_wav.c',
  'src/decoder_wavpack.c',
  'src/decoder_xmp.c',
)

# --- Built-in decoders (no external dependencies) ---

if get_option('wave')
  sdl3_mixer_defines += '-DDECODER_WAV'
endif

if get_option('aiff')
  sdl3_mixer_defines += '-DDECODER_AIFF'
endif

if get_option('voc')
  sdl3_mixer_defines += '-DDECODER_VOC'
endif

if get_option('au')
  sdl3_mixer_defines += '-DDECODER_AU'
endif

if get_option('mp3_drmp3')
  sdl3_mixer_defines += '-DDECODER_MP3_DRMP3'
endif

if get_option('flac_drflac')
  sdl3_mixer_defines += '-DDECODER_FLAC_DRFLAC'
endif

if get_option('vorbis_stb')
  sdl3_mixer_defines += '-DDECODER_OGGVORBIS_STB'
endif

if get_option('midi_timidity')
  sdl3_mixer_defines += '-DDECODER_MIDI_TIMIDITY'
  sdl3_mixer_sources += files(
    'src/timidity/common.c',
    'src/timidity/instrum.c',
    'src/timidity/mix.c',
    'src/timidity/output.c',
    'src/timidity/playmidi.c',
    'src/timidity/readmidi.c',
    'src/timidity/resample.c',
    'src/timidity/tables.c',
    'src/timidity/timidity.c',
  )
endif

# --- External decoders (require system libraries) ---

if get_option('flac_libflac')
  flac_dep = dependency(
    'flac',
    required: true,
  )
  sdl3_mixer_deps += flac_dep
  sdl3_mixer_defines += '-DDECODER_FLAC_LIBFLAC'
endif

if get_option('vorbis_vorbisfile')
  vorbisfile_dep = dependency(
    'vorbisfile',
    required: true,
  )
  sdl3_mixer_deps += vorbisfile_dep
  sdl3_mixer_defines += '-DDECODER_OGGVORBIS_VORBISFILE'
endif

if get_option('opus')
  opusfile_dep = dependency(
    'opusfile',
    required: true,
  )
  sdl3_mixer_deps += opusfile_dep
  sdl3_mixer_defines += '-DDECODER_OPUS'
endif

if get_option('mp3_mpg123')
  mpg123_dep = dependency(
    'libmpg123',
    required: true,
  )
  sdl3_mixer_deps += mpg123_dep
  sdl3_mixer_defines += '-DDECODER_MP3_MPG123'
endif

if get_option('midi_fluidsynth')
  fluidsynth_dep = dependency(
    'fluidsynth',
    version: '>=2.2.0',
    required: true,
  )
  sdl3_mixer_deps += fluidsynth_dep
  sdl3_mixer_defines += '-DDECODER_MIDI_FLUIDSYNTH'
endif

if get_option('mod_xmp')
  xmp_dep = dependency(
    'libxmp',
    required: false,
  )
  if not xmp_dep.found()
    xmp_dep = dependency(
      'libxmp-lite',
      required: true,
    )
  endif
  sdl3_mixer_deps += xmp_dep
  sdl3_mixer_defines += '-DDECODER_MOD_XMP'
endif

if get_option('wavpack')
  wavpack_dep = dependency(
    'wavpack',
    required: true,
  )
  sdl3_mixer_deps += wavpack_dep
  sdl3_mixer_defines += '-DDECODER_WAVPACK'
endif

if get_option('gme')
  add_languages(
    'cpp',
    native: false,
  )
  gme_dep = dependency(
    'libgme',
    required: true,
  )
  sdl3_mixer_deps += gme_dep
  sdl3_mixer_defines += '-DDECODER_GME'
endif

# --- Build library ---

sdl3_mixer_inc = include_directories('include', 'src')

sdl3_mixer_lib = library(
  'SDL3_mixer',
  sdl3_mixer_sources,
  c_args: sdl3_mixer_defines,
  include_directories: sdl3_mixer_inc,
  dependencies: sdl3_mixer_deps,
  gnu_symbol_visibility: 'hidden',
  install: true,
)

sdl3_mixer_dep = declare_dependency(
  link_with: sdl3_mixer_lib,
  include_directories: include_directories('include'),
  dependencies: sdl3_mixer_deps,
)

meson.override_dependency('sdl3_mixer', sdl3_mixer_dep)
