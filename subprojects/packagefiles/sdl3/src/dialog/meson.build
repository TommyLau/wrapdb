if host_machine.system() == 'windows'
  subdir('windows')
elif host_machine.system() == 'darwin'
  if host_machine.subsystem() == 'ios'
    subdir('dummy')
  else
    subdir('cocoa')
  endif
elif host_machine.system() == 'android'
  subdir('android')
else
  subdir('unix')
endif

sources += files('SDL_dialog.c', 'SDL_dialog_utils.c')
