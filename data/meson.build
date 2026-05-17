if udev_hwdb_dir != ''
    # This file has to be updated using
    # ninja -C <builddir> libfprint/sync-udev-hwdb
    # Note that the unsupported device list needs to be manually synced from
    # the wiki. See comment in libfprint/fprint-list-uev-hwdb.c

    install_data('autosuspend.hwdb',
        rename: '60-autosuspend-@0@.hwdb'.format(versioned_libname),
        install_dir: udev_hwdb_dir,
    )
endif

# SELinux policy module for fprintd + OpenCV (libsigfm)
# Grants fprintd the minimum permissions to read /proc/sys/vm/nr_hugepages,
# which OpenCV accesses at startup for hugepage-backed memory optimisation.
checkmodule = find_program('checkmodule', required: false)
semodule_package = find_program('semodule_package', required: false)
semodule = find_program('semodule', required: false)

if checkmodule.found() and semodule_package.found() and semodule.found()
    selinux_te = files('selinux/fprintd-libfprint.te')

    selinux_mod = custom_target('fprintd-libfprint-mod',
        input: selinux_te,
        output: 'fprintd-libfprint.mod',
        command: [checkmodule, '-M', '-m', '-o', '@OUTPUT@', '@INPUT@'],
    )

    selinux_pp = custom_target('fprintd-libfprint-pp',
        input: selinux_mod,
        output: 'fprintd-libfprint.pp',
        command: [semodule_package, '-o', '@OUTPUT@', '-m', '@INPUT@'],
        install: true,
        install_dir: datadir / 'libfprint-2' / 'selinux',
    )

    meson.add_install_script('sh', '-c',
        'semodule -X 300 -i "@0@" || true'.format(
            datadir / 'libfprint-2' / 'selinux' / 'fprintd-libfprint.pp'
        )
    )
else
    message('SELinux tools not found; skipping SELinux policy module installation.')
endif
