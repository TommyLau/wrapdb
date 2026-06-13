if host_machine.system() == 'windows'
  subdir('windows')
elif host_machine.system() == 'darwin'
  subdir('unix')
elif host_machine.system() == 'android'
  subdir('unix')
  subdir('android')
else
  subdir('unix')
  subdir('linux')
endif

sources += files('SDL_core_unsupported.c')
