#
# This file is open source software, licensed to you under the terms
# of the Apache License, Version 2.0 (the "License").  See the NOTICE file
# distributed with this work for additional information regarding copyright
# ownership.  You may not use this file except in compliance with the License.
#
# You may obtain a copy of the License at
#
#   http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing,
# software distributed under the License is distributed on an
# "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
# KIND, either express or implied.  See the License for the
# specific language governing permissions and limitations
# under the License.
#

project('seastar', 'cpp')

defines = []

compiler = meson.get_compiler('cpp')

if compiler.version().version_compare('<4.9')
  error('Seastar requires g++ >= 4.9.  Install g++-4.9 or later (use --compiler option).')
endif

defines += 'HAVE_ASAN_FIBER_SUPPORT' # asan support added in gcc 4.8

if compiler.compiles('#include <gnutls/gnutls.h>') == false
  error('Seastar requires gnutls.  Install gnutls-devel/libgnutls-dev')
endif

hwloc_compile = compiler.compiles('#include <hwloc.h>\n#include <numa.h>')
if get_option('enable-hwloc') and not hwloc_compile
    error('Error: required packages hwloc-devel/numactl-devel not installed.')
endif

if compiler.has_function('LZ4_compress_default', args: ['-llz4'])
  defines += 'HAVE_LZ4_COMPRESS_DEFAULT'
endif

gnutls = dependency('gnutls', version: '>= 2.8')
boost = dependency('boost', version: '>= 1.55')

seastar = static_library('seastar', [])
