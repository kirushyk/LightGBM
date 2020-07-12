project('LightGBM', 'cpp',
    version: '2.3.2',
    meson_version: '>= 0.47.0',
    default_options: [
        'cpp_std=c++11'
    ]
)

openmp = dependency('OpenMP', language: 'cpp', required: get_option('openmp'))

inc = include_directories('include')

subdir('src')
