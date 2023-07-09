# binancetest

## Prerequisites(for MacOS)
Install `clang` (just install XCode with default compiler)
Install `cmake` (any version >= 3.14 should work)
Install `ninja` (I have installed version 1.11.0): https://ninja-build.org

## Instructions to build
1. `cd binancetest`
2. `cmake -E make_directory build/Ninja/clang`
3. `cd build/Ninja/clang`
4. `cmake -GNinja ../../..`
5. `cmake --build . --config Debug` (if you get error: `/Users/user_name/projects/deps_content/Ninja/binance-cxx-api-src/ThirdParty/libwebsockets/lib/plat/unix/private-lib-plat-unix.h:176:9: error: 'MSG_NOSIGNAL' macro redefined [-Werror,-Wmacro-redefined]
#define MSG_NOSIGNAL SO_NOSIGPIPE` you can open file`/Users/user_name/projects/deps_content/Ninja/binance-cxx-api-src/ThirdParty/libwebsockets/lib/plat/unix/private-lib-plat-unix.h` and add this code snippet:
`#ifdef MSG_NOSIGNAL
#undef MSG_NOSIGNAL
#endif`
between lines 175(`#ifdef __APPLE__`) and 176(`#define MSG_NOSIGNAL SO_NOSIGPIPE`) and run command `cmake --build . --config Debug` again).


## Useful only if you care about nice formatted code after making a commit
Install `pre-commit` package. For instructions see: [pre-commit installation](https://pre-commit.com/#install)

In order to be able to make commits with proper formatting and other checks you should run this commands after clonning the repo:
  1. `cd binancetest`
  2. `pre-commit install`
  3. `pre-commit install --hook-type prepare-commit-msg`
