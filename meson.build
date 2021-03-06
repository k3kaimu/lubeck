project('lubeck', 'd', 'c', version : '1.0.0', license: 'BSL-1.0')

cblas_d_dep = dependency('cblas-d', fallback : ['cblas', 'cblas_dep'])
lapack_d_dep = dependency('lapack-d', fallback : ['lapack', 'lapack_dep'])
mir_algorithm_dep = dependency('mir-algorithm', fallback : ['mir-algorithm', 'mir_algorithm_dep'])
mir_blas_dep = dependency('mir-blas', fallback : ['mir-blas', 'mir_blas_dep'])
mir_lapack_dep = dependency('mir-lapack', fallback : ['mir-lapack', 'mir_lapack_dep'])
mir_random_dep = dependency('mir-random', fallback : ['mir-random', 'mir_random_dep'], default_options : ['extended=true'] )

required_deps = [
    cblas_d_dep,
    lapack_d_dep,
    mir_algorithm_dep,
    mir_blas_dep,
    mir_lapack_dep,
    mir_random_dep,
    ]

if target_machine.system() == 'linux'
    mir_linux_kernel_dep = dependency('mir-linux-kernel', fallback : ['mir-linux-kernel', 'mir_linux_kernel_dep'])
    required_deps = [required_deps, mir_linux_kernel_dep]
endif

lubeck_dir = include_directories('source/')

lubeck_src = [
    'source/lubeck.d',
]

lubeck_lib = library(meson.project_name(),
    lubeck_src,
    include_directories: lubeck_dir,
    install: true,
    version: meson.project_version(),
    dependencies: required_deps,
)

lubeck_dep = declare_dependency(
    link_with: [lubeck_lib],
    include_directories: lubeck_dir,
    dependencies: required_deps,
)

install_subdir('source/',
    strip_directory : true,
    install_dir: 'include/d/' + meson.project_name(),
)

import('pkgconfig').generate(lubeck_lib,
    description: 'Optimisation Framework',
    subdirs: 'd/' + meson.project_name(),
)

if get_option('with_test')

    lubeck_test_exe = executable(meson.project_name() + '-test',
        lubeck_src,
        include_directories: lubeck_dir,
        d_unittest: true,
        link_args: '-main',
        dependencies: required_deps,
    )

    test(meson.project_name() + '-test', lubeck_test_exe)

endif
