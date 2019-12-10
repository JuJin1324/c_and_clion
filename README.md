# c_and_clion
C언어 및 CLion 정리

## Unity(TDD Library for C)
* [Unity Git](https://github.com/ThrowTheSwitch/Unity)
* [공식사이트](http://www.throwtheswitch.org/)

### 사용법
* git 다운로드 : `$ git clone https://github.com/ThrowTheSwitch/Unity.git`
* Unity-master 아래 src 디렉터리에서 `unity_internals.h`, `unity.h`, `unity.c` 파일을 unity를 사용하려하는 프로젝트로 붙여넣기 후 CMake에 추가하여 사용

## CMake macOS arm-linux cross compile
### arm-linux Toolchain  
* 다운로드 [링크](https://github.com/thinkski/osx-arm-linux-toolchains)   
Download 항목에서 [arm-unknown-linux-gnueabi](https://github.com/thinkski/osx-arm-linux-toolchains/releases/download/8.3.0/arm-unknown-linux-gnueabi.tar.xz) 클릭해서 다운로드

* 다운받은 디렉터리로 이동 후 압축풀기
    - 여기서는 `/Users/jujin/Downloads`로 가정
```bash
$ cd ~/Downloads
$ tar -xvf arm-unknown-linux-gnueabi.tar.xz
```
* 폴더 이동
    - `/Users/jujin/Downloads` 디렉터리 아래에 두기 뭐해서 옮김 
    - `/Users/jujin/Documents/dev/toolchain` 디렉터리 밑에 두기로 가정
    - `Documents` 디렉터리 아래 `dev` 및 `toolchain` 디렉터리 모두 직접 만듬
```bash
$ mv /Users/jujin/Downloads/arm-unknown-linux-gnueabi.tar.xz /Users/jujin/Documents/dev/toolchain/
```

### CMakeLists.txt에 추가
```text
...
## ARM Linux Cross Compile Options
set(TOOLCHAIN /Users/jujin/Documents/dev/toolchain/arm-unknown-linux-gnueabi)
set(CMAKE_C_COMPILER    ${TOOLCHAIN}/bin/arm-unknown-linux-gnueabi-gcc)
set(CMAKE_LINKER        ${TOOLCHAIN}/bin/arm-unknown-linux-gnueabi-ld)
set(CMAKE_NM            ${TOOLCHAIN}/bin/arm-unknown-linux-gnueabi-nm)
set(CMAKE_OBJCOPY       ${TOOLCHAIN}/bin/arm-unknown-linux-gnueabi-objcopy)
set(CMAKE_OBJDUMP       ${TOOLCHAIN}/bin/arm-unknown-linux-gnueabi-objdump)
set(CMAKE_RANLIB        ${TOOLCHAIN}/bin/arm-unknown-linux-gnueabi-ranlib)
include_directories(${TOOLCHAIN}/arm-unknown-linux-gnueabi/sysroot/usr/include)

## macOS - warning: cannot find entry symbol arch_paths_first; 에러 대처
set(HAVE_FLAG_SEARCH_PATHS_FIRST 0)
set(CMAKE_C_LINK_FLAGS "")
set(CMAKE_CXX_LINK_FLAGS "")
...
```
### 참고 사이트
* [Avoid cmake to add the flags -search_paths_first and -headerpad_max_install_names in MacOS](https://stackoverflow.com/questions/54482519/avoid-cmake-to-add-the-flags-search-paths-first-and-headerpad-max-install-name)
* [CMake: 크로스 컴파일을 하자](https://codecooking.tistory.com/81)
* crosstool-ng 공식사이트 : [링크](https://crosstool-ng.github.io/)
* [thinkski/osx-arm-linux-toolchains](https://github.com/thinkski/osx-arm-linux-toolchains)

## log4c
* 공식 페이지 - http://log4c.sourceforge.net/?
* [log4c_example - jujin](https://github.com/JuJin1324/log4c_example)

## pthread
* 기초 : [링크](https://bitsoul.tistory.com/156?category=683199)

## fork()
* 기초 : [링크](https://thdev.net/176)
