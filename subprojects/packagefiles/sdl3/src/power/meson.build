if host_machine.system() == 'windows'
  subdir('windows')
elif host_machine.system() == 'darwin'
  if host_machine.subsystem() == 'ios'
    cdata.set('SDL_POWER_UIKIT', 1)
    subdir('uikit')
  else
    subdir('macos')
  endif
elif host_machine.system() == 'android'
  cdata.set('SDL_POWER_ANDROID', 1)
  subdir('android')
else
  subdir('linux')
endif

sources += files('SDL_power.c')
