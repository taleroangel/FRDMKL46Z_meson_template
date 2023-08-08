project(
    'frdmkl46z_template',
    ['c', 'cpp'],
    version: '0.1',
    default_options: [
        'c_std=gnu18',
        'optimization=0',
        'warning_level=3',
        'debug=true',
    ],
)

# Define libraries
libraries = ['-lm', '-lc', '-lgcc', '-lnosys']

# Define macros
defines = [
    '-DDEBUG',
    '-DCPU_MKL46Z256VLL4',
    '-DFRDM_KL46Z',
    '-DFREEDOM',
    '-DPRINTF_FLOAT_ENABLE',
    '-DSCANF_FLOAT_ENABLE',
    '-DPRINTF_ADVANCED_ENABLE',
    '-DSCANF_ADVANCED_ENABLE',
]

# Build arguments
add_project_arguments(
    defines,
    '-mcpu=cortex-m0plus',
    '-fno-common',
    '-ffunction-sections',
    '-fdata-sections',
    '-ffreestanding',
    '-fno-builtin',
    '-mthumb',
    '-mapcs',
    '-mfloat-abi=soft',
    '-MMD',
    '-MP',
    language: ['c', 'cpp'],
)

# Linker arguments
add_project_link_arguments(
    libraries,
    '-mcpu=cortex-m0plus',
    '--specs=nano.specs',
    '--specs=nosys.specs',
    '-fno-common',
    '-ffunction-sections',
    '-fdata-sections',
    '-ffreestanding',
    '-fno-builtin',
    '-mthumb',
    '-mapcs',
    '-Xlinker',
    '--gc-sections',
    '-Xlinker',
    '-static',
    '-Xlinker',
    '-z',
    '-Xlinker',
    'muldefs',
    '-mfloat-abi=soft',
    language: ['c', 'cpp'],
)

# Variable tracking
project_src = []
project_inc = []

# Source subdirectories
subdir('lib')
subdir('src')

# Compile SDK
frdmkl46z_sdk = declare_dependency(
    compile_args: ['-w', '-std=gnu99'],
    sources: lib_frdmkl46_src,
    include_directories: project_inc,
)

# Execute elf file
elf = executable(
    meson.project_name(),
    project_src,
    name_suffix: 'elf',
    include_directories: [project_inc],
    dependencies: [frdmkl46z_sdk],
)

# Find arm toolchain executable
objcopy = find_program('arm-none-eabi-objcopy')

# Custom target '.elf' to '.bin'
custom_target(
    '@0@.bin'.format(meson.project_name()),
    output: '@0@.bin'.format(meson.project_name()),
    build_by_default: true,
    command: [
        objcopy,
        [
            '-O',
            'binary',
            elf.full_path(),
            '@0@.bin'.format(meson.project_name()),
        ],
    ],
    depends: elf,
)
