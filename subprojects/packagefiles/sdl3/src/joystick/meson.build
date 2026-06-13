if host_machine.system() == 'windows'
  cdata.set('SDL_JOYSTICK_DINPUT', 1)
  subdir('hidapi')
  subdir('windows')
  subdir('gdk')
elif host_machine.system() == 'darwin'
  cdata.set('SDL_JOYSTICK_MFI', 1)
  subdir('apple')
  if host_machine.subsystem() != 'ios'
    cdata.set('SDL_JOYSTICK_IOKIT', 1)
    subdir('darwin')
  endif
elif host_machine.system() == 'android'
  cdata.set('SDL_JOYSTICK_ANDROID', 1)
  subdir('android')
else
  cdata.set('SDL_JOYSTICK_LINUX', 1)
  subdir('linux')
endif

subdir('virtual')
cdata.set('SDL_JOYSTICK_VIRTUAL', 1)

sources += files(
  'SDL_gamepad.c',
  'SDL_joystick.c',
  'SDL_steam_virtual_gamepad.c',
  'controller_type.c',
)
