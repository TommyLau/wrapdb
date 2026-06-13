if host_machine.system() == 'windows'
  subdir('windows')
elif host_machine.system() == 'darwin'
  if host_machine.subsystem() == 'ios'
    cdata.set('SDL_TRAY_DUMMY', 1)
    subdir('dummy')
  else
    subdir('cocoa')
  endif
elif host_machine.system() == 'android'
  cdata.set('SDL_TRAY_DUMMY', 1)
  subdir('dummy')
else
  subdir('unix')
endif

sources += files('SDL_tray_utils.c')
