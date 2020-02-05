# cmake-starter
CMake 관련 정리

## CMake Install(Deploy)
설치 매크로(make install) 정의 : 리눅스에서 설치란 빌드 완료된 실행 바이너리와 라이브러리 및 기타 리소스 파일들을 시스템의
적절한 위치로 복사하는 작업

### CMAKE_INSTALL_PREFIX
make install 시에 설치 디렉토리 설정
```cmake
SET(CMAKE_INSTALL_PREFIX /Users/ju-jinyoo/Documents/dev/project-deploy)
```

### INSTALL(TARGETS)
TARGETS 뒤에 복사할 바이너리 파일 지정, DESTINATION 뒤에 복사한 바이너리 파일이 위치할 경로 지정
* 템플릿 : INSTALL(TARGETS <b>바이너리파일</b> DESTINATION <b>설치경로</b>)
```cmake 
INSTALL(TARGETS ${APPLICATION_NAME} DESTINATION ${CMAKE_INSTALL_PREFIX})
```

### INSTALL(DIRECTORY)
DIRECTORY 뒤에 복사할 디렉터리 지정, DESTINATION 뒤에 복사한 디렉터리가 위치할 경로 지정, FILE_PERMISSIONS 뒤에 
디렉터리 내부 파일들의 권한 설정
* 탬플릿 : INSTALL(DIRECTORY <b>디렉터리</b> DESTINATION <b>설치경로</b> FILE_PERMISSIONS <b>내부파일권한</b>)
```cmake
INSTALL(DIRECTORY bash config
        DESTINATION ${CMAKE_INSTALL_PREFIX}
        FILE_PERMISSIONS OWNER_EXECUTE OWNER_WRITE OWNER_READ GROUP_READ GROUP_EXECUTE WORLD_READ WORLD_EXECUTE)
```

### INSTALL(PROGRAMS | FILES)
PROGRAMS 뒤에 복사할 파일들 지정, DESTINATION 뒤에 복사한 파일들이 위치할 경로 지정
* 탬플릿 : INSTALL(PROGRAMS | FILES <b>파일</b> DESTINATION <b>설치경로</b>)
```cmake
INSTALL(PROGRAMS
        start_process.sh
        stop_process.sh
        DESTINATION ${CMAKE_INSTALL_PREFIX})
``` 

PROGRAMS 와 FILES의 차이 : Permission
* FILES : 오너 쓰기, 모두 읽기 권한만 설정됨, 실행 권한은 모두 빠짐 - `OWNER_READ`, `GROUP_READ`, `WORLD_READ`
* PROGRAMS : FILES의 권한 및 실행권한 모두 설정됨 - `OWNER_EXECUTE`, `GROUP_EXECUTE`, `WORLD_EXECUTE`

참조사이트 : [[CMake 튜토리얼] 2. CMakeLists.txt 주요 명령과 변수 정리](https://www.tuwlab.com/ece/27260)

## 매크로
### 예약변수 모두 보기 
```cmake
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

## ARM-Linux Cross Compile : macOS
### 다운로드 및 압축풀기
* 여기서는 tar.gz 파일을 `~/Downloads`에 다운로드 받은 것으로 가정
```bash
$ cd ~/Downloads
$ wget https://github.com/thinkski/osx-arm-linux-toolchains/releases/download/8.3.0/arm-unknown-linux-gnueabi.tar.xz
$ tar -xvf arm-unknown-linux-gnueabi.tar.xz
```
`/usr/local` 디렉터리 아래로 이동 - $ `mv ~/Downloads/arm-unknown-linux-gnueabi /usr/local`

환경변수 등록  
* bash 사용시 - `$ vi ~/.bashrc`
* zsh 사용시 - `$ vi ~/.zshrc`
* 파일의 마지막에 `export PATH=/usr/local/arm-unknown-linux-gnueabi/bin:$PATH` 추가

### CMake 설정
CMakeLists.txt에 추가
```cmake
## ARM-Linux Cross Compile Options
set(CMAKE_C_COMPILER    ${ARM_LINUX_TOOLCHAIN_DIR}/bin/arm-unknown-linux-gnueabi-gcc)

## arm-linux Cross Compile Options for macOS(Ubuntu dosen't need below options)
set(ARM_LINUX_TOOLCHAIN_DIR /usr/local/arm-unknown-linux-gnueabi)
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
```

## ARM-Linux Cross Compile : Ubuntu
### 설치
```bash
dpkg --add-architecture i386
apt-get update; apt-get install -y g++ libc6:i386 libstdc++6:i386
apt-get install -y emdebian-archive-keyring libc6-armel-cros libc6-device-armel-cross
apt-get install -y binutils-arm-linux-gnueabi gcc-arm-linux-gnueabi g++-arm-linux-gnueabi 
apt-get install -y u-boot-tools libncurses5-device
```

### CMake 설정
CMakeLists.txt에 추가
```cmake
set(CMAKE_C_COMPILER arm-linux-gnueabi-gcc)
```

### 참고 사이트
* [Avoid cmake to add the flags -search_paths_first and -headerpad_max_install_names in MacOS](https://stackoverflow.com/questions/54482519/avoid-cmake-to-add-the-flags-search-paths-first-and-headerpad-max-install-name)
* [CMake: 크로스 컴파일을 하자](https://codecooking.tistory.com/81)
* crosstool-ng 공식사이트 : [링크](https://crosstool-ng.github.io/)
