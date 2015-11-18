see http://wjchen0.appspot.com/?p=6001

PATH=$PATH:/opt/android-ndk/toolchains/arm-linux-androideabi-4.4.3/prebuilt/linux-x86/bin
for C,
-fpic -ffunction-sections -funwind-tables -fstack-protector -DARM\_ARCH\_5 -DARM\_ARCH\_5T -DARM\_ARCH\_5E -DARM\_ARCH\_5TE -Wno-psabi -march=armv5te -mtune=xscale -msoft-float -mthumb -Os -fomit-frame-pointer -fno-strict-aliasing -finline-limit=64 -DANDROID -Wa,--noexecstack -O2 -DNDEBUG -g -fno-short-enums -I/opt/android-ndk/platforms/android-8/arch-arm/usr/include -Wl,-rpath-link=/opt/android-ndk/platforms/android-8/arch-arm/usr/lib,-dynamic-linker=/system/bin/linker -L/opt/android-ndk/platforms/android-8/arch-arm/usr/lib -nostdlib -lc /opt/android-ndk/platforms/android-8/arch-arm/usr/lib/crtbegin\_dynamic.o /opt/android-ndk/platforms/android-8/arch-arm/usr/lib/crtend\_android.o
for C++,差不多不过include文件夹、link文件多些，链接的静态库多些。
-fpic -ffunction-sections -funwind-tables -fstack-protector -DARM\_ARCH\_5 -DARM\_ARCH\_5T -DARM\_ARCH\_5E -DARM\_ARCH\_5TE -Wno-psabi -march=armv5te -mtune=xscale -msoft-float -mthumb -Os -fomit-frame-pointer -fno-strict-aliasing -finline-limit=64 -DANDROID -Wa,--noexecstack -O2 -DNDEBUG -g -fno-short-enums -I/opt/android-ndk/sources/cxx-stl/system/include -I/opt/android-ndk/platforms/android-8/arch-arm/usr/include -Wl,-rpath-link=/opt/android-ndk/platforms/android-8/arch-arm/usr/lib,-dynamic-linker=/system/bin/linker -L/opt/android-ndk/platforms/android-8/arch-arm/usr/lib -nostdlib -lc /opt/android-ndk/platforms/android-8/arch-arm/usr/lib/crtbegin\_dynamic.o /opt/android-ndk/platforms/android-8/arch-arm/usr/lib/crtend\_android.o /opt/android-ndk/sources/cxx-stl/gnu-libstdc++/libs/armeabi/libstdc++.a /opt/android-ndk/toolchains/arm-linux-androideabi-4.4.3/prebuilt/linux-x86/lib/gcc/arm-linux-androideabi/4.4.3/libgcc.a
编译命令行程序是需要crtbegin\_dynamic.o crtend\_android.o，编译生成.a文件不需要crtbegin\_dynamic.o crtend\_android.o，编译.so文件不需要crtbegin\_dynamic.o crtend\_android.o而需要链接
> /opt/android-ndk/toolchains/arm-linux-androideabi-4.4.3/prebuilt/linux-x86/lib/gcc/arm-linux-androideabi/4.4.3/crtbeginS.o 和
/opt/android-ndk/toolchains/arm-linux-androideabi-4.4.3/prebuilt/linux-x86/lib/gcc/arm-linux-androideabi/4.4.3/crtendS.o 两个文件。
有了这些参数就不用写android.mk了。

最重要的是bionic和glibc有些地方不一样。比如在编译p7zip时候我发现bionic没有getpass,setmnsent,getmnset；glic中time.h中的timegm在bionic中对应的是time64.h中的timegm64.
编译参数参考了ndk-build -p > filename

和 http://warpedtimes.wordpress.com/category/technology/android-technology/
和 http://www.bekatul.info/content/native-c-application-android