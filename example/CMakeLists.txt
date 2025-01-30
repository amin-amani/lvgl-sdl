cmake_minimum_required(VERSION 3.14)

project(lvgl_example LANGUAGES C CXX)

# تنظیم مسیر SDL2 (توجه کن که این مسیر را بر اساس محل دقیق نصب SDL2 اصلاح کن)


set(SDL2MAIN_LIBRARY "C:/Qt/SDL2/x86_64-w64-mingw32/lib/libSDL2main.a")
link_directories("C:/Qt/SDL2/x86_64-w64-mingw32/lib")
set(SDL2_INCLUDE_DIR "C:/Qt/SDL2/x86_64-w64-mingw32/include/")



# اضافه کردن مسیر هدرهای SDL2
include_directories(${SDL2_INCLUDE_DIR})
include_directories(${SDL2_INCLUDE_DIR}/SDL2)  # این خط اضافه شده تا SDL.h بدون مشکل پیدا شود

# تنظیم مسیرهای پروژه
set(PROJECT_DIR "${CMAKE_SOURCE_DIR}")
set(LVGL_DIR "${PROJECT_DIR}/../lvgl")
set(LVDRV_DIR "${PROJECT_DIR}/../lvgl_drv")
set(LV_CONF_PATH ${PROJECT_DIR}/../lv_conf.h CACHE STRING "" FORCE)

# تنظیمات کامپایل
add_definitions(-DLV_LVGL_H_INCLUDE_SIMPLE)
add_definitions(-DLV_CONF_PATH=${LV_CONF_PATH})

# اضافه کردن کتابخانه‌های موردنیاز
add_library(lvglsdl ${LVDRV_DIR}/lv_sdl_disp.c ${LVDRV_DIR}/lv_port_indev.c)
target_include_directories(lvglsdl PUBLIC ${SDL2_INCLUDE_DIR})

# پیدا کردن فایل‌های منبع پروژه
file(GLOB LVGL_DRIVER_FILES
    "${PROJECT_DIR}/main.c"
    "${PROJECT_DIR}/example.c"
    "${PROJECT_DIR}/assets/*.c"
)

# تنظیم گزینه‌های کامپایل
add_compile_options(-Wall -O2)
add_executable(lvgl_example ${LVGL_DRIVER_FILES})

# اضافه کردن مسیرهای `include`
include_directories(${LVGL_DIR})
include_directories(${LVDRV_DIR})
include_directories(${PROJECT_DIR})

# اضافه کردن تنظیمات `lvgl`
include(${LVGL_DIR}/CMakeLists.txt)

# تنظیم مسیرهای کتابخانه‌ها
target_include_directories(lvgl_example PUBLIC ${SDL2_INCLUDE_DIR})
target_compile_options(lvgl_example PUBLIC ${SDL2_CFLAGS_OTHER})
target_link_libraries(lvgl_example ${SDL2_LIBRARY} ${SDL2MAIN_LIBRARY} lvglsdl lvgl::lvgl  -lmingw32 -lSDL2main -lSDL2)
