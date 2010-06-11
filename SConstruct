# vim: ft=python expandtab
import os
import re
from site_init import *

opts = Variables()
opts.Add(PathVariable('PREFIX', 'Installation prefix', os.path.expanduser('~/FOSS'), PathVariable.PathIsDirCreate))
opts.Add(BoolVariable('DEBUG', 'Build with Debugging information', 0))
opts.Add(PathVariable('PERL', 'Path to the executable perl', r'C:\Perl\bin\perl.exe', PathVariable.PathIsFile))
opts.Add(BoolVariable('WITH_OSMSVCRT', 'Link with the os supplied msvcrt.dll instead of the one supplied by the compiler (msvcr90.dll, for instance)', 0))

env = Environment(variables = opts,
                  ENV=os.environ, tools = ['default', GBuilder])

Initialize(env)
env.Append(CPPPATH=['#'])
env.Append(CFLAGS=env['DEBUG_CFLAGS'])
env.Append(CPPDEFINES=env['DEBUG_CPPDEFINES'])

ATK_MAJOR_VERSION=1
ATK_MINOR_VERSION=27
ATK_MICRO_VERSION=90
env['ATK_VERSION'] = '%d.%d.%d' % (ATK_MAJOR_VERSION, ATK_MINOR_VERSION, ATK_MICRO_VERSION)
env['PACKAGE_NAME'] = "atk"
env['PACKAGE_VERSION'] = env['ATK_VERSION']
env['PACKAGE_DEPENDS'] = 'glib'
env['DOT_IN_SUBS'] = {'@ATK_MAJOR_VERSION@': str(ATK_MAJOR_VERSION),
                      '@ATK_MINOR_VERSION@': str(ATK_MINOR_VERSION),
                      '@ATK_MICRO_VERSION@': str(ATK_MICRO_VERSION),
                      '@VERSION@': env['ATK_VERSION'],
                      '@prefix@': env['PREFIX'],
                      '@exec_prefix@': '${prefix}/bin',
                      '@libdir@': '${prefix}/lib',
                      '@includedir@': '${prefix}/include',
                      '@GLIB_PACKAGES@': 'glib-2.0 gobject-2.0',
                      '@ATK_API_VERSION@': '1.0'
                      }
env.DotIn('config.h', 'config.h.in')
env.DotIn('atk.pc', 'atk.pc.in')
InstallDev('$PREFIX/lib/pkgconfig', ['atk.pc'], env)
env['DOT_IN_SUBS']['@PCS@'] = generate_file_element('atk.pc', r'lib/pkgconfig', env)

SConscript(['atk/SConscript'], exports = 'env')

DumpInstalledFiles(env)
