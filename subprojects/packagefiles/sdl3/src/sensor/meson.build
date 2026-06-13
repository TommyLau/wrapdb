if host_machine.system() == 'windows'
  subdir('windows')
elif host_machine.system() == 'android'
  cdata.set('SDL_SENSOR_ANDROID', 1)
  subdir('android')
elif host_machine.subsystem() == 'ios'
  cdata.set('SDL_SENSOR_COREMOTION', 1)
  subdir('coremotion')
else
  subdir('dummy')
endif

sources += files('SDL_sensor.c')
