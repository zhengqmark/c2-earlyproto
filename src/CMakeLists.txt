# Copyright (c) 2021 Triad National Security, LLC, as operator of Los Alamos
# National Laboratory with the U.S. Department of Energy/National Nuclear
# Security Administration. All Rights Reserved.
#
# Permission is hereby granted, free of charge, to any person obtaining a copy
# of this software and associated documentation files (the "Software"), to deal
# with the Software without restriction, including without limitation the
# rights to use, copy, modify, merge, publish, distribute, sublicense, and/or
# sell copies of the Software, and to permit persons to whom the Software is
# furnished to do so, subject to the following conditions:
#
# 1. Redistributions of source code must retain the above copyright notice,
#    this list of conditions and the following disclaimer.
# 2. Redistributions in binary form must reproduce the above copyright notice,
#    this list of conditions and the following disclaimer in the documentation
#    and/or other materials provided with the distribution.
# 3. Neither the name of TRIAD, Los Alamos National Laboratory, LANL, the
#    U.S. Government, nor the names of its contributors may be used to endorse
#    or promote products derived from this software without specific prior
#    written permission.
#
# THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS "AS IS" AND ANY EXPRESS OR
# IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF
# MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO
# EVENT SHALL THE COPYRIGHT HOLDERS OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT,
# INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES
# (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES;
# LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND
# ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT
# (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS
# SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
set(c2-srcs c2_slice.cc c2_status.cc c2_coding.cc c2_env.cc c2_io.cc
        c2_io_posix.cc c2_index_builder.cc port_posix.cc)
set(c2-test-srcs testharness.cc testutil.cc)
set(c2-tests c2_random_test.cc c2_index_builder_test.cc)

add_library(c2 ${c2-srcs} ${c2-test-srcs})
target_link_libraries(c2 fastbit)

add_executable(c2_index_bench c2_index_bench.cc)
target_link_libraries(c2_index_bench c2)
if(NOT BUILD_SHARED_LIBS)
    target_link_libraries(c2_index_bench "-static")
endif()

foreach(lcv ${c2-tests})
    # use basename as test name
    get_filename_component (id ${lcv} NAME_WE)
    add_executable(${id} EXCLUDE_FROM_ALL ${lcv})
    target_link_libraries(${id} c2)
    add_test(${id} ${id})
    add_dependencies(c2-build-tests ${id})
endforeach()
