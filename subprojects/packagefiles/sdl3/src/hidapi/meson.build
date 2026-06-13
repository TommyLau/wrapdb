if host_machine.system() == 'windows'
  subdir('windows')
elif host_machine.subsystem() == 'macos'
  #  subdir('mac')
elif host_machine.subsystem() == 'ios'
  # iOS has no raw-HID access; like macOS, compile only SDL_hidapi.c (no
  # platform backend) so the Linux hidraw backend is not pulled in.
elif host_machine.system() == 'android'
  # core/android's JNI_OnLoad unconditionally registers the HIDDeviceManager
  # JNI table defined in the C++ Android hidapi backend.
  subdir('android')
else
  subdir('linux')
endif

sources += files('SDL_hidapi.c')
