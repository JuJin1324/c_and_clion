# c_and_clion
C언어 및 CLion 정리

## CMake 예약변수 모두 보기 매크로
```text
## CMake의 예약변수들에 대한 내용을 모두 출력하도록 하는 매크로
### 매크로 정의
macro(print_all_variables)
    message(STATUS "print_all_variables------------------------------------------{")
    get_cmake_property(_variableNames VARIABLES)
    foreach (_variableName ${_variableNames})
        message(STATUS "${_variableName}=${${_variableName}}")
    endforeach()
    message(STATUS "print_all_variables------------------------------------------}")
endmacro()

### 매크로 사용
print_all_variables()
```

## Unity(TDD Library for C)
* [공식사이트](http://www.throwtheswitch.org/)
* [Unity Git](https://github.com/ThrowTheSwitch/Unity)

### 사용법
* git 다운로드 : `$ git clone https://github.com/ThrowTheSwitch/Unity.git`
* Unity-master 아래 src 디렉터리에서 `unity_internals.h`, `unity.h`, `unity.c` 파일을 unity를 사용하려하는 프로젝트로 붙여넣기 후 CMake에 추가하여 사용

## CMake macOS arm-linux cross compile
### arm-linux Toolchain  
* [GitHub 페이지](https://github.com/thinkski/osx-arm-linux-toolchains)   
Download 항목에서 [arm-unknown-linux-gnueabi](https://github.com/thinkski/osx-arm-linux-toolchains/releases/download/8.3.0/arm-unknown-linux-gnueabi.tar.xz) 클릭해서 다운로드

* 다운받은 디렉터리로 이동 후 압축풀기
    - 여기서는 tar.gz 파일을 `~/Downloads`에 다운로드 받은 것으로 가정
```bash
$ cd ~/Downloads
$ tar -xvf arm-unknown-linux-gnueabi.tar.xz
```
* 폴더 이동
    - `~/Downloads` 디렉터리 아래에 두기 뭐해서 옮김 
    - `/usr/local` 디렉터리 아래로 이동
```bash
$ mv ~/Downloads/arm-unknown-linux-gnueabi /usr/local
```

### 환경변수 등록
* bash 사용시 - `$ vi ~/.bashrc`
* zsh 사용시 - `$ vi ~/.zshrc`
* 파일의 마지막에 `export PATH=/usr/local/arm-unknown-linux-gnueabi/bin:$PATH` 추가

### CMakeLists.txt에 추가
```text
...
## arm-linux Cross Compile Options for macOS(Ubuntu dosen't need below options)
set(ARM_LINUX_TOOLCHAIN_DIR /usr/local/arm-unknown-linux-gnueabi)
set(CMAKE_C_COMPILER    ${ARM_LINUX_TOOLCHAIN_DIR}/bin/arm-unknown-linux-gnueabi-gcc)
set(CMAKE_LINKER        ${ARM_LINUX_TOOLCHAIN_DIR}/bin/arm-unknown-linux-gnueabi-ld)
set(CMAKE_NM            ${ARM_LINUX_TOOLCHAIN_DIR}/bin/arm-unknown-linux-gnueabi-nm)
set(CMAKE_OBJCOPY       ${ARM_LINUX_TOOLCHAIN_DIR}/bin/arm-unknown-linux-gnueabi-objcopy)
set(CMAKE_OBJDUMP       ${ARM_LINUX_TOOLCHAIN_DIR}/bin/arm-unknown-linux-gnueabi-objdump)
set(CMAKE_RANLIB        ${ARM_LINUX_TOOLCHAIN_DIR}/bin/arm-unknown-linux-gnueabi-ranlib)
include_directories(${ARM_LINUX_TOOLCHAIN_DIR}/arm-unknown-linux-gnueabi/sysroot/usr/include)

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
