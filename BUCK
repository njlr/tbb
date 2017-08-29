version_string = """#define __TBB_VERSION_STRINGS(N) \\\\
#N\\": BUILD_HOST		Box (i386)\\" ENDL \\\\
#N\\": BUILD_OS		Mac OS X version 10.12.6\\" ENDL \\\\
#N\\": BUILD_KERNEL	Darwin Kernel Version 16.7.0: Thu Jun 15 17:36:27 PDT 2017; root:xnu-3789.70.16~2/RELEASE_X86_64\\" ENDL \\\\
#N\\": BUILD_CLANG	clang version 4.0.1 (tags/RELEASE_401/final)\\" ENDL \\\\
#N\\": BUILD_XCODE	Xcode 8.3.3\\" ENDL \\\\
#N\\": BUILD_TARGET	intel64 on cc4.0.1_os10.12.6\\" ENDL \\\\
#N\\": BUILD_COMMAND	buck build //:tbb \\" ENDL \\\\

#define __TBB_DATETIME \\"Tue 29 Aug 2017 13:47:02 UTC\\"
"""

genrule(
  name = 'version-string-file',
  out = 'version_string.ver',
  cmd = 'echo "' + version_string + '" > $OUT',
)

prebuilt_cxx_library(
  name = 'version-string',
  header_only = True,
  header_namespace = '',
  exported_headers = {
    'version_string.ver': ':version-string-file',
  },
)

macos_flags = [
  '-DUSE_PTHREAD=1',
  '-stdlib=libc++',
  '-D__TBB_INITIALIZER_LISTS_PRESENT=1',
  '-m64',
  '-mrtm',
  '-fPIC',
]

linux_flags = [
  '-DUSE_PTHREAD=1',
]

windows_flags = [
  '-DUSE_WINTHREAD=1',
]

cxx_library(
  name = 'tbb',
  header_namespace = '',
  exported_headers = subdir_glob([
    ('include', '**/condition_variable'),
    ('include', '**/thread'),
    ('include', '**/tuple'),
    ('include', '**/*.h'),
  ]),
  headers = subdir_glob([
    ('src', '**/*.h'),
    ('src', 'tbb/dynamic_link.cpp'),
    ('src/rml/include', '**/*.h'),
  ]),
  srcs = glob([
    'src/**/*.cpp',
  ], excludes = glob([
    'src/rml/test/**/*.cpp',
    'src/old/**/test*.cpp',
    'src/test/**/*.cpp',
  ])),
  compiler_flags = [
    '-D__TBB_BUILD=1',
    '-D_TBB_CPP0X=1',

    '-DTBB_USE_THREADING_TOOLS=0',
    '-D__TBB_RML_STATIC=1',
    '-D__TBB_NO_IMPLICIT_LINKAGE=1',

    '-fno-rtti',
    '-fno-exceptions',
  ],
  platform_compiler_flags = [
    ('default', macos_flags),
    ('^macos.*', macos_flags),
    ('^linux.*', linux_flags),
    ('^windows.*', windows_flags),
  ],
  deps = [
    ':version-string',
  ],
  visibility = [
    'PUBLIC',
  ],
)
