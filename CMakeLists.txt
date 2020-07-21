cmake_minimum_required(VERSION 2.8)

include(CryptoNoteWallet.cmake)

project(${CN_PROJECT_NAME})

execute_process(COMMAND git log -1 --pretty=format:%h
WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
OUTPUT_VARIABLE GIT_REVISION)

set(CRYPTONOTE_LIB crazycoin)

include_directories(${CMAKE_BINARY_DIR}
    src
    crazycoin/external
    crazycoin/include
    crazycoin/src)

find_package(Qt5Gui REQUIRED)
find_package(Qt5Widgets REQUIRED)

set(Boost_USE_STATIC_LIBS ON)
if(WIN32)
  set(Boost_USE_STATIC_RUNTIME OFF)
else(WIN32)
  set(Boost_USE_STATIC_RUNTIME ON)
endif(WIN32)

find_package(Boost 1.55 REQUIRED COMPONENTS date_time filesystem program_options regex serialization system thread chrono)
if ((${Boost_MAJOR_VERSION} EQUAL 1) AND (${Boost_MINOR_VERSION} EQUAL 54))
	message(SEND_ERROR "Boost version 1.54 is unsupported, more details are available here http://goo.gl/RrCFmA")
endif ()

include_directories(${Boost_INCLUDE_DIRS})
link_directories(${Boost_LIBRARY_DIRS})

set(VERSION "")
configure_file("crazycoin/src/version.h.in" "version.h")
configure_file("src/CryptoNoteWalletConfig.h.in" "CryptoNoteWalletConfig.h")

add_definitions(-DGIT_REVISION=\"${GIT_REVISION}\")

set(CMAKE_AUTOMOC ON)

set(CRYPTONOTE_SOURCES
  crazycoin/external/miniupnpc/connecthostport.c
  crazycoin/external/miniupnpc/igd_desc_parse.c
  crazycoin/external/miniupnpc/minisoap.c
  crazycoin/external/miniupnpc/miniupnpc.c
  crazycoin/external/miniupnpc/miniwget.c
  crazycoin/external/miniupnpc/minixml.c
  crazycoin/external/miniupnpc/portlistingparse.c
  crazycoin/external/miniupnpc/receivedata.c
  crazycoin/external/miniupnpc/upnpcommands.c
  crazycoin/external/miniupnpc/upnpreplyparse.c
  crazycoin/src/Common/Base58.cpp
  crazycoin/src/Common/CommandLine.cpp
  crazycoin/src/Common/Util.cpp
  crazycoin/src/Common/StringTools.cpp
  crazycoin/src/Common/JsonValue.cpp
  crazycoin/src/Common/ConsoleTools.cpp
  crazycoin/src/Common/MemoryInputStream.cpp
  crazycoin/src/Common/PathTools.cpp
  crazycoin/src/Common/StdInputStream.cpp
  crazycoin/src/Common/StdOutputStream.cpp
  crazycoin/src/Common/StreamTools.cpp
  crazycoin/src/Common/StringOutputStream.cpp
  crazycoin/src/Common/StringView.cpp
  crazycoin/src/Common/VectorOutputStream.cpp
  crazycoin/src/crypto/blake256.c
  crazycoin/src/crypto/chacha8.c
  crazycoin/src/crypto/crypto-ops-data.c
  crazycoin/src/crypto/crypto-ops.c
  crazycoin/src/crypto/crypto.cpp
  crazycoin/src/crypto/groestl.c
  crazycoin/src/crypto/hash-extra-blake.c
  crazycoin/src/crypto/hash-extra-groestl.c
  crazycoin/src/crypto/hash-extra-jh.c
  crazycoin/src/crypto/hash-extra-skein.c
  crazycoin/src/crypto/hash.c
  crazycoin/src/crypto/jh.c
  crazycoin/src/crypto/keccak.c
  crazycoin/src/crypto/oaes_lib.c
  crazycoin/src/crypto/random.c
  crazycoin/src/crypto/skein.c
  crazycoin/src/crypto/slow-hash.c
  crazycoin/src/crypto/slow-hash.cpp
  crazycoin/src/crypto/tree-hash.c
  crazycoin/src/CryptoNoteCore/BlockchainIndices.cpp
  crazycoin/src/CryptoNoteCore/BlockchainMessages.cpp
  crazycoin/src/CryptoNoteCore/BlockIndex.cpp
  crazycoin/src/CryptoNoteCore/CoreConfig.cpp
  crazycoin/src/CryptoNoteCore/CryptoNoteBasic.cpp
  crazycoin/src/CryptoNoteCore/CryptoNoteTools.cpp
  crazycoin/src/CryptoNoteCore/Currency.cpp
  crazycoin/src/CryptoNoteCore/MinerConfig.cpp
  crazycoin/src/CryptoNoteCore/Transaction.cpp
  crazycoin/src/CryptoNoteCore/Account.cpp
  crazycoin/src/CryptoNoteCore/Blockchain.cpp
  crazycoin/src/CryptoNoteCore/Checkpoints.cpp
  crazycoin/src/CryptoNoteCore/CryptoNoteBasicImpl.cpp
  crazycoin/src/CryptoNoteCore/Core.cpp
  crazycoin/src/CryptoNoteCore/CryptoNoteFormatUtils.cpp
  crazycoin/src/CryptoNoteCore/CryptoNoteSerialization.cpp
  crazycoin/src/CryptoNoteCore/Difficulty.cpp
  crazycoin/src/CryptoNoteCore/IBlock.cpp
  crazycoin/src/CryptoNoteCore/Miner.cpp
  crazycoin/src/CryptoNoteCore/TransactionExtra.cpp
  crazycoin/src/CryptoNoteCore/TransactionPool.cpp
  crazycoin/src/CryptoNoteCore/TransactionPrefixImpl.cpp
  crazycoin/src/CryptoNoteCore/TransactionUtils.cpp
  crazycoin/src/InProcessNode/InProcessNode.cpp
  crazycoin/src/InProcessNode/InProcessNodeErrors.cpp
  crazycoin/src/NodeRpcProxy/NodeErrors.cpp
  crazycoin/src/NodeRpcProxy/NodeRpcProxy.cpp
  crazycoin/src/P2p/NetNodeConfig.cpp
  crazycoin/src/Serialization/BinaryInputStreamSerializer.cpp
  crazycoin/src/Serialization/BinaryOutputStreamSerializer.cpp
  crazycoin/src/Serialization/JsonInputValueSerializer.cpp
  crazycoin/src/Serialization/JsonOutputStreamSerializer.cpp
  crazycoin/src/Serialization/KVBinaryInputStreamSerializer.cpp
  crazycoin/src/Serialization/KVBinaryOutputStreamSerializer.cpp
  crazycoin/src/Serialization/SerializationOverloads.cpp
  crazycoin/src/Transfers/BlockchainSynchronizer.cpp
  crazycoin/src/Transfers/SynchronizationState.cpp
  crazycoin/src/Transfers/TransfersConsumer.cpp
  crazycoin/src/Transfers/TransfersContainer.cpp
  crazycoin/src/Transfers/TransfersSubscription.cpp
  crazycoin/src/Transfers/TransfersSynchronizer.cpp
  crazycoin/src/Wallet/LegacyKeysImporter.cpp
  crazycoin/src/Wallet/WalletAsyncContextCounter.cpp
  crazycoin/src/Wallet/WalletErrors.cpp
  crazycoin/src/WalletLegacy/KeysStorage.cpp
  crazycoin/src/WalletLegacy/WalletLegacy.cpp
  crazycoin/src/WalletLegacy/WalletHelper.cpp
  crazycoin/src/WalletLegacy/WalletLegacySerializer.cpp
  crazycoin/src/WalletLegacy/WalletLegacySerialization.cpp
  crazycoin/src/WalletLegacy/WalletTransactionSender.cpp
  crazycoin/src/WalletLegacy/WalletUnconfirmedTransactions.cpp
  crazycoin/src/WalletLegacy/WalletUserTransactionsCache.cpp
  crazycoin/src/System/ContextGroup.cpp
  crazycoin/src/System/Event.cpp
  crazycoin/src/System/EventLock.cpp
  crazycoin/src/System/InterruptedException.cpp
  crazycoin/src/System/Ipv4Address.cpp
  crazycoin/src/System/TcpStream.cpp
  crazycoin/src/HTTP/HttpRequest.cpp
  crazycoin/src/HTTP/HttpParser.cpp
  crazycoin/src/HTTP/HttpParserErrorCodes.cpp
  crazycoin/src/HTTP/HttpResponse.cpp
  crazycoin/src/Rpc/HttpClient.cpp
  crazycoin/src/Rpc/JsonRpc.cpp
  crazycoin/src/P2p/NetNode.cpp
  crazycoin/src/P2p/LevinProtocol.cpp
  crazycoin/src/P2p/NetNodeConfig.cpp
  crazycoin/src/P2p/PeerListManager.cpp
  crazycoin/src/CryptoNoteProtocol/CryptoNoteProtocolHandler.cpp
  crazycoin/src/Logging/ILogger.cpp
  crazycoin/src/Logging/LoggerGroup.cpp
  crazycoin/src/Logging/CommonLogger.cpp
  crazycoin/src/Logging/LoggerManager.cpp
  crazycoin/src/Logging/FileLogger.cpp
  crazycoin/src/Logging/StreamLogger.cpp
  crazycoin/src/Logging/ConsoleLogger.cpp
  crazycoin/src/Logging/LoggerMessage.cpp
  crazycoin/src/Logging/LoggerRef.cpp
  crazycoin/src/BlockchainExplorer/BlockchainExplorer.cpp
  crazycoin/src/BlockchainExplorer/BlockchainExplorerDataBuilder.cpp
  crazycoin/src/BlockchainExplorer/BlockchainExplorerErrors.cpp
)

file(GLOB_RECURSE SOURCES src/*.cpp)
file(GLOB_RECURSE HEADERS src/*.h)
file(GLOB_RECURSE FORMS src/gui/ui/*.ui)

set(QRC src/resources.qrc)

qt5_wrap_ui(UIS ${FORMS})
qt5_add_resources(RCC ${QRC})


if (WIN32)
  if (NOT MSVC)
	message(FATAL_ERROR "Only MSVC is supported on this platform")
  endif ()
  add_definitions(/D_CRT_SECURE_NO_WARNINGS /D_WIN32_WINNT=0x0600 /DSTATICLIB)
  include_directories(crazycoin/src/platform/msc)

  set(PLATFORM_DIR Windows)
  set(BUILD_PLATFORM WIN32)
  set(BUILD_RESOURCES src/crazycoinwallet.rc)
  set(QTMAIN Qt5::WinMain)

elseif (UNIX)
  set(CRYPTONOTE_SOURCES ${CRYPTONOTE_SOURCES} crazycoin/external/miniupnpc/minissdpc.c)
  if (APPLE)
    enable_language(ASM)
    file(GLOB_RECURSE OBJC_SOURCES src/*.mm)
    set(SOURCES ${SOURCES} ${OBJC_SOURCES})
    set(PLATFORM_DIR OSX)
    set(MACOSX_BUNDLE_INFO_STRING "Cryptonote GUI wallet")
    set(MACOSX_BUNDLE_LONG_VERSION_STRING "${VERSION_VERSION}.${VERSION_MINOR}.${VERSION_PATCH}")
    set(MACOSX_BUNDLE_BUNDLE_NAME CryptonoteWallet)
    set(MACOSX_BUNDLE_SHORT_VERSION_STRING "${VERSION_VERSION}.${VERSION_MINOR}.${VERSION_PATCH}")
    set(MACOSX_BUNDLE_BUNDLE_VERSION "$${VERSION_VERSION}.${VERSION_MINOR}.${VERSION_PATCH}")

    find_package(Qt5PrintSupport REQUIRED)

    include_directories(/usr/include/malloc)

    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -maes -std=c++11 -stdlib=libc++")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -std=c11 -maes -D_DARWIN_C_SOURCE")
    set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -framework Cocoa -framework OpenGL -framework CoreFoundation -framework Carbon -framework IOKit -L/usr/lib")

    set(MACOSX_BUNDLE_ICON_FILE crazycoin.icns)
    set(APPLICATION_ICON src/images/crazycoin.icns)
    set_source_files_properties(${APPLICATION_ICON} PROPERTIES MACOSX_PACKAGE_LOCATION "Resources")

    set(BUILD_PLATFORM MACOSX_BUNDLE)
    set(BUILD_RESOURCES ${APPLICATION_ICON})
	
    GET_TARGET_PROPERTY(QT_LIB_DIR "${Qt5Widgets_LIBRARIES}" LOCATION)
    GET_FILENAME_COMPONENT(QT_LIB_DIR "${QT_LIB_DIR}" PATH)
  else(APPLE)
    set(PLATFORM_DIR Linux)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -maes -std=c++11")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -maes -std=c11")
  endif (APPLE)
endif ()

include_directories(crazycoin/src/Platform/${PLATFORM_DIR})
file(GLOB PLATFORM_SOURCES crazycoin/src/Platform/${PLATFORM_DIR}/System/*)
set(CRYPTONOTE_SOURCES ${CRYPTONOTE_SOURCES} ${PLATFORM_SOURCES})

add_library(${CRYPTONOTE_LIB} STATIC ${CRYPTONOTE_SOURCES})
set_target_properties(${CRYPTONOTE_LIB} PROPERTIES COMPILE_DEFINITIONS _GNU_SOURCE)
target_link_libraries(${CRYPTONOTE_LIB} ${Boost_LIBRARIES})

add_executable(${PROJECT_NAME} ${BUILD_PLATFORM} ${BUILD_RESOURCES} ${SOURCES} ${HEADERS} ${UIS} ${RCC})
set_target_properties(${PROJECT_NAME} PROPERTIES COMPILE_DEFINITIONS _GNU_SOURCE)
target_link_libraries(${PROJECT_NAME} ${Boost_LIBRARIES} ${QTMAIN} ${CRYPTONOTE_LIB})

if (APPLE)
  qt5_use_modules(${PROJECT_NAME} PrintSupport)
elseif (UNIX)
  target_link_libraries(${PROJECT_NAME} -lpthread)
elseif (WIN32)
  target_link_libraries(${PROJECT_NAME} Imm32 Iphlpapi Winmm)
endif (APPLE)

qt5_use_modules(${PROJECT_NAME} Widgets Gui)
