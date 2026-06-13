if host_machine.system() == 'windows'
  subdir('windows')
elif host_machine.system() == 'darwin'
  if host_machine.subsystem() == 'ios'
    subdir('ios')
  else
    subdir('macos')
  endif
elif host_machine.system() == 'android'
  subdir('android')
else
  subdir('unix')
endif

sources += files('SDL_url.c')
