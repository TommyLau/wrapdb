if host_machine.system() == 'windows'
  subdir('windows')
elif host_machine.system() == 'android'
  cdata.set('SDL_PROCESS_DUMMY', 1)
  subdir('dummy')
else
  subdir('posix')
endif

sources += files('SDL_process.c')
