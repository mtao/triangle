project('triangle', 'c',
  version : '0.1',
  default_options : ['warning_level=3'])


# add the math library portably
# https://mesonbuild.com/howtox.html#add-math-library-lm-portably
cc = meson.get_compiler('c')
m_dep = cc.find_library('m', required : false)

# To get the exact arithmetic to work right on an Intel processor, use the
#   -DCPU86 switch with Microsoft C, or the -DLINUX switch with gcc running
#   on Linux.  The floating-point arithmetic might not be robust otherwise.
#   Please see http://www.cs.cmu.edu/~quake/robust.pc.html for details.
if target_machine.system() == 'linux'
  add_project_arguments('-DLINUX', language: 'c')
elif target_machine.system() == 'windows'
  add_project_arguments('-DCPU86', language: 'c')
endif

args = ['-DTRILIBRARY', '-DANSI_DECLARATORS']

arg_pairs = [['single','-DSINGLE']
            ,['cdt_only','-DCDT_ONLY']
            ,['reduced','-DREDUCED']
            ,['self_check','-DSELF_CHECK']]

foreach pair: arg_pairs
  opt = pair[0]
  flag = pair[1]
  if get_option(opt)
    add_project_arguments(flag,language: 'c')
  endif
endforeach

#if get_option('single')
#  args+='-DSINGLE'
#endif
#if get_option('reduced')
#  args+='-DREDUCED'
#endif
#if get_option('cdt_only')
#  args+='-DCDT_ONLY'
#endif
if get_option('no_timer') or target_machine.system() != 'linux'
  args+='-DNO_TIMER'
endif
#if get_option('self_check')
#  args+='-DSELF_CHECK'
#endif




triangle = library('triangle', 'triangle.c', install: true, c_args: ['-DTRILIBRARY'], dependencies:m_dep)

executable('triangle_bin', 'triangle.c', install: false, dependencies:m_dep, build_by_default: false)

install_headers('triangle.h')

executable('tricall',
           'tricall.c', install: false, link_with: triangle,  c_args: '-DVOID=int', build_by_default: false)
           
if target_machine.system() == 'linux'
  executable('showme',
    'showme.c',
    install : false, link_with: triangle, dependencies: dependency('x11'), build_by_default: false)
endif

pkgc = import('pkgconfig')
pkgc.generate(name: 'triangle',
    version: meson.project_version(),
    description: 'A Two-Dimensional Quality Mesh Generator and Delaunay Triangulator.'
)
