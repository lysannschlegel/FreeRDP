# WinPR: Windows Portable Runtime
# libwinpr-sspi cmake build script
#
# Copyright 2011 O.S. Systems Software Ltda.
# Copyright 2011 Otavio Salvador <otavio@ossystems.com.br>
# Copyright 2011 Marc-Andre Moreau <marcandre.moreau@gmail.com>
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

set(WINPR_SSPI_NTLM_SRCS
	NTLM/ntlm_av_pairs.c
	NTLM/ntlm_av_pairs.h
	NTLM/ntlm_compute.c
	NTLM/ntlm_compute.h
	NTLM/ntlm_message.c
	NTLM/ntlm_message.h
	NTLM/ntlm.c
	NTLM/ntlm.h)

set(WINPR_SSPI_NEGOTIATE_SRCS
	Negotiate/negotiate.c
	Negotiate/negotiate.h)

set(WINPR_SSPI_SCHANNEL_SRCS
	Schannel/schannel.c
	Schannel/schannel.h)

set(WINPR_SSPI_CREDSSP_SRCS
	CredSSP/credssp.c)

set(WINPR_SSPI_SRCS
	${WINPR_SSPI_CREDSSP_SRCS}
	sspi.c
	sspi.h)

if(NOT WITH_NATIVE_SSPI)
	set(WINPR_SSPI_SRCS
		${WINPR_SSPI_NTLM_SRCS}
		${WINPR_SSPI_KERBEROS_SRCS}
		${WINPR_SSPI_NEGOTIATE_SRCS}
		${WINPR_SSPI_SCHANNEL_SRCS}
		${WINPR_SSPI_SRCS})
endif()

add_library(winpr-sspi ${WINPR_SSPI_SRCS})

include_directories(${ZLIB_INCLUDE_DIRS})
include_directories(${OPENSSL_INCLUDE_DIR})

set_target_properties(winpr-sspi PROPERTIES VERSION ${FREERDP_VERSION_FULL} SOVERSION ${FREERDP_VERSION} PREFIX "lib")

if (NOT WIN32)
	target_link_libraries(winpr-sspi winpr-crt)
	target_link_libraries(winpr-sspi winpr-sysinfo)
endif()

target_link_libraries(winpr-sspi winpr-utils)
target_link_libraries(winpr-sspi ${ZLIB_LIBRARIES})
target_link_libraries(winpr-sspi ${OPENSSL_LIBRARIES})

install(TARGETS winpr-sspi DESTINATION ${CMAKE_INSTALL_LIBDIR})

