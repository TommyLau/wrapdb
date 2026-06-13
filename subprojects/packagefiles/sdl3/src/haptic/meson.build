if host_machine.system() == 'windows'
  cdata.set('SDL_HAPTIC_DINPUT', 1)
  subdir('windows')
elif host_machine.system() == 'darwin'
  if host_machine.subsystem() == 'ios'
    cdata.set('SDL_HAPTIC_DUMMY', 1)
    subdir('dummy')
  else
    cdata.set('SDL_HAPTIC_IOKIT', 1)
    subdir('darwin')
  endif
elif host_machine.system() == 'android'
  cdata.set('SDL_HAPTIC_ANDROID', 1)
  subdir('android')
else
  cdata.set('SDL_HAPTIC_LINUX', 1)
  subdir('linux')
endif

sources += files('SDL_haptic.c')
