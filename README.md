# c_and_clion
C언어 및 CLion 정리

## CMake macOS arm-linux cross compile
### crosstool-ng
* 설명 : Toolchain(크로스 컴파일 개발 환경) 생성기
* 설치 : `brew install crosstool-ng`

### 

### 참고 사이트
* 공식사이트 : [링크](https://crosstool-ng.github.io/)

* CMakeLists.txt 
```text
...
## ARM Linux Cross Compile Options
set(TOOLCHAIN /Volumes/crosstool/x-tools/arm-unknown-linux-gnueabi)
set(CMAKE_C_COMPILER    ${TOOLCHAIN}/bin/arm-unknown-linux-gnueabi-gcc)
set(CMAKE_LINKER        ${TOOLCHAIN}/bin/arm-unknown-linux-gnueabi-ld)
set(CMAKE_NM            ${TOOLCHAIN}/bin/arm-unknown-linux-gnueabi-nm)
set(CMAKE_OBJCOPY       ${TOOLCHAIN}/bin/arm-unknown-linux-gnueabi-objcopy)
set(CMAKE_OBJDUMP       ${TOOLCHAIN}/bin/arm-unknown-linux-gnueabi-objdump)
set(CMAKE_RANLIB        ${TOOLCHAIN}/bin/arm-unknown-linux-gnueabi-ranlib)
include_directories(${TOOLCHAIN}/arm-unknown-linux-gnueabi/sysroot/usr/include)

## warning: cannot find entry symbol arch_paths_first; 에러 대처
set(HAVE_FLAG_SEARCH_PATHS_FIRST 0)
set(CMAKE_C_LINK_FLAGS "")
set(CMAKE_CXX_LINK_FLAGS "")
...
```
### 참고 사이트
* (Avoid cmake to add the flags -search_paths_first and -headerpad_max_install_names in MacOS)[https://stackoverflow.com/questions/54482519/avoid-cmake-to-add-the-flags-search-paths-first-and-headerpad-max-install-name]
* [CMake: 크로스 컴파일을 하자](https://codecooking.tistory.com/81)


### log4c
* 공식 페이지 - http://log4c.sourceforge.net/?
* macOS 사용시에 로컬이 아닌 docker에 접속하여 설치
### log4c 설치
```
# apt-get install -y liblog4c3
# apt-get install -y liblog4c3-32bit
# apt-get install -y liblog4c3-dev
``` 
### 로그 환경설정 xml 파일 : log4crc
* log4crc 를 cmake-build-debug-XXX 아래 .out(실행파일)과 함께 위치
* log4crc 주의사항
    - `<category name="dev" priority="debug" appender="stdout" />`
    - `<category name="dev" priority="debug" appender="myrollingfileappender" />`
    - 위와 같이 같은 이름의 카테고리를 2개 선언할 경우 마지막에 선언된 1개의 카테고리(여기서는 appender가 myrollingfileappender)만 로깅에 적용된다.
    
### armv7용 라이브러리 생성 과정
* log4c 라이브러리 설치 압축파일 다운로드 : [링크](https://sourceforge.net/projects/log4c/files/log4c/1.2.4/log4c-1.2.4.tar.gz/download?use_mirror=jaist)

## pthread
* 기초 : [링크](https://bitsoul.tistory.com/156?category=683199)

## fork()
* 기초 : [링크](https://thdev.net/176)
