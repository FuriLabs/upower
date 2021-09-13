subdir('man')

ifaces_refs = []

dbus_dir = join_paths(meson.source_root(), 'dbus')
spec_to_docbook = files('spec-to-docbook.xsl')

foreach iface: upowerd_dbus_interfaces
  iface = iface[1]
  iface_ref = iface + '.ref.xml'

  ifaces_refs += custom_target(
    iface_ref,
    input: files(dbus_dir / iface + '.xml'),
    output: iface_ref,
    command: [xsltproc, '--output', '@OUTPUT@', spec_to_docbook, '@INPUT@'],
    # XXX: This appears to be needed so that "ninja -C _build install" works
    build_by_default: get_option('gtk-doc'),
  )
endforeach

cdata = configuration_data()
cdata.set('VERSION', meson.project_version())
version_xml = configure_file(
    output: 'version.xml',
    input: 'version.xml.in',
    configuration: cdata)

gnome.gtkdoc('UPower',
    main_xml: 'upower-docs.xml',
    src_dir: meson.source_root() / 'libupower-glib',
    dependencies: [ libupower_glib_dep ],
    scan_args: ['--rebuild-types', '--rebuild-sections'],
    content_files: [
        version_xml,
        ifaces_refs,
        'man/UPower.xml',
        'man/upowerd.xml',
        'man/upower.xml',
        '../COPYING',
    ],
    ignore_headers: [
        'config.h',
    ],
    install: get_option('gtk-doc')
)